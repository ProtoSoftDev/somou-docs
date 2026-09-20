# Integration Examples



This guide explains integration of common VoIP platforms with WhatsApp Business Calling API.

**Note:** This guide is for information purposes only with no support or warranties of any kind from Meta or any vendor. There are many ways to integrate and the guide explains just one way exclusively for illustrative purposes.

## Asterisk using SIP

### Overview

This guide explains how to set up [WhatsApp Business Calling API](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling) using SIP signaling with [Asterisk](https://www.asterisk.org/), an open-source PBX (Private Branch Exchange). You&#039;ll learn how to configure your Asterisk server, connect SIP phones, and handle both incoming and outgoing WhatsApp calls.

#### User-initiated calls

* The WhatsApp user dials the business number.
* The call is received by Asterisk and routed through an IVR, prompting the user to enter an extension, registered to the same Asterisk server.
* The call is then connected to the specified extension.

#### Business-initiated calls

* The business agent/user registers with Asterisk using SIP credentials (see &quot;[Configuring a VoIP Phone](#configuring-a-voip-phone)&quot; section).
* The business user dials the b2c-sip (business to consumer) extension, which is handled by an IVR. The IVR prompts for the WhatsApp number to call.
* The call is then connected to the WhatsApp user.

The WA to Asterisk leg uses SDES for media encryption key exchange and Opus for audio codec.

The Asterisk to SIP UA leg uses SDES for media encryption key exchange and Opus or G.711 for audio codec.

### Prerequisites

* Asterisk Deployment: Asterisk is deployed (for example, on a public cloud instance)
* Operating System: Any OS compatible with Asterisk. For example, CentOS 9
* Domain: Asterisk server is reachable via a public domain with valid certificate
* WhatsApp Business API: A WhatsApp Business phone number is registered and calling is enabled.
* SIP Support: [SIP is enabled](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/sip#configure-update-sip-settings-on-business-phone-number) on the WhatsApp Business Number
* SDES: [SDES is enabled](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/sip#configure-sdes-for-srtp-key-exchange-protocol) on the WhatsApp Business Number

### Building and installing Asterisk

Refer to the [Asterisk build and install guide](https://docs.asterisk.org/Getting-Started/Installing-Asterisk/Installing-Asterisk-From-Source/Building-and-Installing-Asterisk/).

This guide was tested using Asterisk version 22.5.2.

### Asterisk configuration

These configuration files are placed under /etc/asterisk/

#### extensions.conf

Replace the following placeholders with actual values

1. &#123;wa-business-phone-number&#125;: WhatsApp Business Phone Number
1. &#123;asterisk-sip-server-dns&#125;: DNS name of your Asterisk SIP server
1. incoming_welcome: incoming_welcome.wav (not provided) place this file under /var/lib/asterisk/sounds
1. outgoing_welcome: outgoing_welcome.wav (not provided) place this file under /var/lib/asterisk/sounds

```https
[c2b-sub-dial]
exten =&gt; s,1,NoOp()
  same =&gt; n,Read(Digits,incoming_welcome,0,,5, 500)
  same =&gt; n,Dial(PJSIP/$&#123;Digits&#125;)
  same =&gt; n,Hangup()

[whatsapp]
exten =&gt; _10XX,1,NoOp()
  same =&gt; n,Dial(PJSIP/$&#123;EXTEN&#125;)
  same =&gt; n,Hangup()

;Extension for B2C business call through Meta SIP gateway
exten =&gt; b2c-sip,1,NoOp()
  same =&gt; n,Read(Digits,outgoing_welcome,0,,5, 500)
  same =&gt; n,Dial(PJSIP/whatsapp/sip:$&#123;Digits&#125;&#064;wa.meta.vc)

;Extension to handle incoming invite requests from Meta SIP gateway to &lt;wa-business-phone-number&gt;&#064;&lt;asterisk-sip-server-dns&gt;
exten =&gt; _+&lt;wa-business-phone-number&gt;,1,Goto(c2b-sub-dial,s,1)
```

#### pjsip.conf

Replace the following placeholders with actual values

1. &#123;wa-business-phone-number&#125; : the business phone number
2. &#123;local-net&#125;: local network of the Asterisk server
3. &#123;external-media-address&#125;: Public IP of the Asterisk server media
4. &#123;external-signaling-address&#125;: Public IP of the Asterisk server signaling
5. &#123;sip-ua-password&#125;: Chosen SIP User Agent password
6. &#123;domain-name&#125;: domain name assigned to the Asterisk server

Certificate files should be placed under
/var/lib/asterisk/certs/fullchain.cer
/var/lib/asterisk/certs/cer.key

```https
[transport-tls]
type=transport
protocol=tls
bind=0.0.0.0:5061
cert_file=/var/lib/asterisk/certs/fullchain.cer
priv_key_file=/var/lib/asterisk/certs/cer.key
method=sslv23
allow_wildcard_certs=yes
external_media_address=&#123;external-media-address&#125;
;External address for SIP signalling
external_signaling_address=&#123;external-signaling-address&#125;
;Network to consider local used for NAT purposes
local_net=&#123;local-net&#125;

[sdes_endpointtemplate](!)
type=endpoint
context=whatsapp
disallow=all
allow=OPUS
direct_media=no
rtp_symmetric=yes
force_rport=yes
rewrite_contact=no
media_use_received_transport=yes
media_encryption=sdes

[authtemplate](!)
type=auth
auth_type=userpass
password=&#123;sip-ua-password&#125;

[aortemplate](!)
type=aor
max_contacts=1
remove_existing=yes

[aoridentitytemplate](!)
type=identify
match_header=X-FB-External-Domain: wa.meta.vc

;SDES users
[1000](sdes_endpointtemplate)
auth=1000_auth
aors=1000

[1000_auth](authtemplate)
username=1000

[1000](aortemplate)

[1000](aoridentitytemplate)
endpoint=1000

[1001](sdes_endpointtemplate)
auth=1001_auth
aors=1001

[1001_auth](authtemplate)
username=1001

[1001](aortemplate)

[1001](aoridentitytemplate)
endpoint=1001

[1002](sdes_endpointtemplate)
auth=1002_auth
aors=1002

[1002_auth](authtemplate)
username=1002

[1002](aortemplate)

[1002](aoridentitytemplate)
endpoint=1002

[1003](sdes_endpointtemplate)
auth=1003_auth
aors=1003

[1003_auth](authtemplate)
username=1003

[1003](aortemplate)

[1003](aoridentitytemplate)
endpoint=1003

[1004](sdes_endpointtemplate)
auth=1004_auth
aors=1004

[1004_auth](authtemplate)
username=1004

[1004](aortemplate)

[1004](aoridentitytemplate)
endpoint=1004

[1005](sdes_endpointtemplate)
auth=1005_auth
aors=1005

[1005_auth](authtemplate)
username=1005

[1005](aortemplate)

[1005](aoridentitytemplate)
endpoint=1005

;This endpoint maps to an IVR for C2B calls
[c2b-sip](sdes_endpointtemplate)

[c2b-sip](aortemplate)

[c2b-sip]
type=identify
endpoint=c2b-sip
match_header=X-FB-External-Domain: wa.meta.vc

;special endpoint for Meta SIP Gateway integration
;This endpoint maps to an IVR for B2C calls
[b2c-sip](sdes_endpointtemplate)

[b2c-sip](aortemplate)

[whatsapp](sdes_endpointtemplate)
type=endpoint
transport=transport-tls
disallow=all
allow=opus,ulaw,alaw
aors=whatsapp
from_user=&#123;wa-business-phone-number&#125;
from_domain=&#123;domain-name&#125;
outbound_auth=whatsapp

[whatsapp]
type=aor
contact=sip:wa.meta.vc

[whatsapp]
type=identify
endpoint=whatsapp

[whatsapp]
type=auth
auth_type=digest
password=&#123;meta-sip-user-password&#125;
username=&#123;wa-business-phone-number&#125;
realm=*
```

#### rtp.conf

```https
[general]
; Hostname or address for the STUN server used for determining the external
; IP address and port an RTP session can be reached at. The port number is
; optional. If omitted default value of 3478 will be used. This option is
; disabled by default. Name resolution occurs at load time, and if DNS is
; used, name resolution will occur repeatedly after the TTL expires.
;
; for example stundaddr=mystun.server.com:3478
;
stunaddr=stun.l.google.com:19302

rtpstart=10000
rtpend=60000
```

### Configuring a VoIP phone

Download and install a softphone client (for example, [Linphone](https://www.linphone.org/en/download)) for testing both business-initiated and user-initiated calls.

#### Account setup

1. Select an extension to register as a SIP UA (extensions 1001–1005).
2. Open Preferences.
3. Under &quot;SIP Accounts,&quot; click &quot;Add account.&quot;
4. Enter the following details:
   * SIP Address: for example, sip:1001&#064;&#123;asterisk-sip-server-dns&#125;
   * SIP Server Address: for example, sip:&#123;asterisk-sip-server-dns&#125;;transport=tls
   * Transport: TLS
   * Disable ICE
   * Enable AVPF
   * Disable &quot;Publish presence information&quot;
5. Confirm and save the account.
6. Enter the password when prompted (that is, &#123;sip-ua-password&#125;)
7. Once connected, return to Preferences and select the &quot;Audio&quot; tab. Enable all audio codecs.
8. In the &quot;Calls and Chat&quot; tab:
   * Select &quot;Encryption&quot;
   * Choose &quot;SRTP-SDES&quot;
   * Enable &quot;Encryption is mandatory&quot;
   * Confirm settings

### Final checklist

* Double-check all configuration files for correct numbers, passwords, and domain names.
* Make sure your firewall allows SIP (5061/TLS) and RTP (10000-20000) ports.
* For more details on SIP password setup, see the [WhatsApp Cloud API documentation](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/sip).

### Troubleshooting

#### Cannot register SIP UA

Confirm that the SIP URL is correct and the domain is pointing to the Asterisk server. Run `host &#123;domain-name&#125;` to verify that the IP address points to the Asterisk server.

#### Not receiving ACK from Meta OR Business audio stops around 30s OR Meta returns 404 response to BYE

For a user initiated call, Meta sends a `SIP INVITE` to your SIP server which then responds with `200 OK`. Meta acks your `200 OK` with an `ACK` but you never receive this ACK. So your SIP server keeps resending the `200 OK` and ultimately the SIP dialog is terminated due to ACK timeout (typically 32s).

The most likely cause for this problem is incorrect `Record-Route` headers in your `200 OK` to Meta. The `200 OK` response is supposed to not modify the `Record-Route` headers included in the original Meta&#039;s `INVITE`. Your SIP server can add new `Record-Route` headers but cannot modify those present in our `INVITE`.

The solution to this problem is to change `rewrite_contact=yes` to `rewrite_contact=no` on the WhatsApp endpoint configuration in pjsip.conf file. After this make sure your `200 OK` has following headers as the last 2 in the list of `Record-Route` headers

This problem is hard to detect or diagnose. Even with this bug, the call will get connected and media will flow in both directions but around 32s later, your SIP server will terminate the call which won&#039;t be propagated to WhatsApp client because your BYE request has incorrect `Route` headers. So WA user stops hearing business audio around 32s.

```https
Record-Route: &lt;sip:wa.meta.vc;transport=tls;lr&gt;
Record-Route: &lt;sip:onevc-sip-proxy.fbinfra.net:8191;transport=tls;lr&gt;
```

## FreeSWITCH using SIP

### Overview

This guide explains how to set up [WhatsApp Business Calling API](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling) using SIP signaling with [FreeSWITCH](https://signalwire.com/freeswitch), an open-source communication framework. You&#039;ll learn how to configure your FreeSWITCH server, connect SIP phones, and handle both user-initiated and business-initiated WhatsApp calls.

#### User-initiated calls

* The WhatsApp user dials the business number.
* The call is received by FreeSWITCH and routed through an IVR, which prompts the user to enter an agent&#039;s extension registered on the same FreeSWITCH server.
* Once the extension is entered, the call is connected to the specified recipient agent.

#### Business-initiated calls

* The business agent or user registers with FreeSWITCH using SIP credentials (see the [Configuring a VoIP Phone](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/integration-examples#configuring-a-voip-phone) section for details).
* The business user dials the b2c-sip (business-to-consumer) extension, which is managed by an IVR. The IVR then prompts for the WhatsApp number to call.
* After the number is entered, the call is connected to the WhatsApp user via SIP.

The WA to FreeSWITCH leg uses SDES for media encryption key exchange with Opus as the audio codec. FreeSWITCH - SIP UA leg uses SDES for media encryption key exchange with Opus or G.711 audio codecs.

### Prerequisites

* FreeSWITCH Deployment: FreeSWITCH is deployed (for example, on a public cloud instance)
* Operating System: Any OS compatible with FreeSWITCH. For example, CentOS 9
* Domain: FreeSWITCH server is reachable via a public domain with a valid certificate
* WhatsApp Business API: A WhatsApp Business phone number is registered and [calling is enabled](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/call-settings).
* SIP Support: [SIP is enabled](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/sip#configure-update-sip-settings-on-business-phone-number) on the WhatsApp Business Number
  * Note: FreeSWITCH is configured to listen on 5081 for TLS
* SDES: [SDES is enabled](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/sip#configure-sdes-for-srtp-key-exchange-protocol) on the WhatsApp Business Number

### Building and installing FreeSWITCH

Refer to the [FreeSWITCH installation guide](https://developer.signalwire.com/freeswitch/FreeSWITCH-Explained/Installation/).

This guide was tested using FreeSWITCH version 1.10.12. FreeSWITCH uses sofia (an open-source SIP user agent library). Sofia v1.13.17 was used for this guide.

#### FreeSWITCH configuration
These configuration files are placed under /usr/share/freeswitch/etc/freeswitch

**wa-biz-api-dialplan.xml**

Place the dial plan under /usr/share/freeswitch/etc/freeswitch/dialplan/default/wa-biz-api-dialplan.xml

```https
&lt;include&gt;
 &lt;extension name=&quot;c2b_calls_sip_ivr&quot;&gt;
   &lt;!--Dial plan is selected if the SIP request is coming from Meta--&gt;
   &lt;condition field=&quot;$&#123;sip_from_host&#125;&quot; expression=&quot;^wa.meta.vc$&quot;&gt;
     &lt;!--Verify the IP from where the request is coming, compare the IP with the Meta allowlisted IPs--&gt;
     &lt;action application=&quot;check_acl&quot; data=&quot;$&#123;network_addr&#125; whatsapp_allow normal_clearing&quot;/&gt;
     &lt;!--Enable encrypted media using SDES--&gt;
     &lt;action application=&quot;set&quot; data=&quot;rtp_secure_media=true&quot;/&gt;
     &lt;action application=&quot;answer&quot;/&gt;
     &lt;!--Add silence stream for  1 sec so that the media path is established between whatsapp and freeswitch to avoid audio clipping--&gt;
     &lt;action application=&quot;playback&quot; data=&quot;silence_stream://1000&quot;/&gt;
     &lt;action application=&quot;play_and_get_digits&quot; data=&quot;2 5 3 7000 # $$&#123;base_dir&#125;/sounds/incoming_welcome.wav  $$&#123;base_dir&#125;/sounds/incoming_invalid.wav extension \d+&quot;/&gt;
     &lt;!--While the call is being bridged, play a ringtone for the caller--&gt;
     &lt;action application=&quot;set&quot; data=&quot;ringback=%(2000, 4000, 440.0, 480.0)&quot;/&gt;
     &lt;!--Offer G711 and Opus for FreeSWITCH-SIP UA leg --&gt;
     &lt;action application=&quot;export&quot; data=&quot;nolocal:absolute_codec_string=PCMA,PCMU,OPUS&#064;48000h&#064;20i&quot;/&gt;
     &lt;action application=&quot;bridge&quot; data=&quot;user/$&#123;extension&#125;&quot;/&gt;
     &lt;action application=&quot;hangup&quot;/&gt;
   &lt;/condition&gt;
 &lt;/extension&gt;
 &lt;extension name=&quot;b2c_calls_ivr&quot;&gt;
   &lt;condition field=&quot;destination_number&quot; expression=&quot;^b2c-sip$&quot;&gt;
     &lt;!--Enable encrypted media using SDES--&gt;
     &lt;action application=&quot;set&quot; data=&quot;rtp_secure_media=true&quot;/&gt;
     &lt;action application=&quot;answer&quot;/&gt;
     &lt;action application=&quot;playback&quot; data=&quot;silence_stream://1000&quot;/&gt;
     &lt;action application=&quot;set&quot; data=&quot;caller_id_check=$&#123;caller_id_number&#125;&quot;/&gt;
     &lt;action application=&quot;play_and_get_digits&quot; data=&quot;2 12 3 20000 # $$&#123;base_dir&#125;/sounds/outgoing_welcome.wav $$&#123;base_dir&#125;/sounds/outgoing_invalid.wav whatsapp_number \d+&quot;/&gt;
     &lt;action application=&quot;log&quot; data=&quot;INFO [whatsapp_number] is $&#123;whatsapp_number&#125;&quot;/&gt;
     &lt;!--While the call is being bridged, play a ringtone for the caller--&gt;
     &lt;action application=&quot;set&quot; data=&quot;ringback=%(2000, 4000, 440.0, 480.0)&quot;/&gt;
     &lt;!--Offer only OPUS--&gt;
     &lt;action application=&quot;export&quot; data=&quot;nolocal:absolute_codec_string=OPUS&#064;48000h&#064;20i,OPUS&#064;8000h&#064;20i&quot;/&gt;
     &lt;!--Bridge the call by calling META SIP with the WA Number--&gt;
     &lt;action application=&quot;bridge&quot; data=&quot;sofia/gateway/whatsapp/+$&#123;whatsapp_number&#125;&quot;/&gt;
     &lt;action application=&quot;hangup&quot;/&gt;
   &lt;/condition&gt;
 &lt;/extension&gt;
&lt;/include&gt;
```

Audio files should be placed under /usr/share/freeswitch/sounds (not provided)

* `incoming_welcome.wav`
* `Incoming_invalid.wav`
* `outgoing_welcome.wav`
* `outgoing_invalid.wav`

**whatsapp.xml**

This file configures the WhatsApp gateway, copy the file to `/usr/share/freeswitch/etc/freeswitch/sip_profiles/external/whatsapp.xml`

```https
&lt;!--Gateway configuration for Meta SIP--&gt;
&lt;!--replace &#123;phone-number&#125;,&#123;meta-sip-password&#125; and &#123;domain-name&#125; before starting FreeSWITCH--&gt;
&lt;include&gt;
 &lt;gateway name=&quot;whatsapp&quot;&gt;
   &lt;param name=&quot;username&quot; value=&quot;&#123;phone-number&#125;&quot;/&gt;
   &lt;param name=&quot;password&quot; value=&quot;&#123;meta-sip-password&#125;&quot;/&gt;
   &lt;param name=&quot;register&quot; value=&quot;false&quot;/&gt;
   &lt;param name=&quot;realm&quot; value=&quot;wa.meta.vc&quot;/&gt;
   &lt;param name=&quot;from-user&quot; value=&quot;&#123;phone-number&#125;&quot;/&gt;
   &lt;param name=&quot;from-domain&quot; value=&quot;&#123;domain-name&#125;&quot;/&gt;
 &lt;/gateway&gt;
&lt;/include&gt;
```

Replace the following placeholders with actual values

1. &#123;phone-number&#125;: WhatsApp Business Phone Number
1. &#123;meta-sip-password&#125;: SIP password issued by Meta. For more details on SIP password setup, see the [WhatsApp Cloud API documentation](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/sip#include-sip-user-password).
1. &#123;domain-name&#125;: DNS name of your FreeSWITCH SIP server

**acl.conf.xml**

Open `/usr/share/freeswitch/etc/freeswitch/autoload_configs/acl.conf.xml`

Add the following list under `network-lists` element

```https
&lt;!--IP addresses from Meta that are allowed to send SIP requests via the gateway. Keep this up to date--&gt;
   &lt;list name=&quot;whatsapp_allow&quot; default=&quot;deny&quot;&gt;
     &lt;node type=&quot;allow&quot; cidr=&quot;31.13.24.0/21&quot;/&gt;
     &lt;node type=&quot;allow&quot; cidr=&quot;31.13.64.0/18&quot;/&gt;
     &lt;node type=&quot;allow&quot; cidr=&quot;45.64.40.0/22&quot;/&gt;
     &lt;node type=&quot;allow&quot; cidr=&quot;57.141.0.0/21&quot;/&gt;
     &lt;node type=&quot;allow&quot; cidr=&quot;57.141.8.0/22&quot;/&gt;
     &lt;node type=&quot;allow&quot; cidr=&quot;57.141.12.0/23&quot;/&gt;
     &lt;node type=&quot;allow&quot; cidr=&quot;57.144.0.0/14&quot;/&gt;
     &lt;node type=&quot;allow&quot; cidr=&quot;66.220.144.0/20&quot;/&gt;
     &lt;node type=&quot;allow&quot; cidr=&quot;69.63.176.0/20&quot;/&gt;
     &lt;node type=&quot;allow&quot; cidr=&quot;69.171.224.0/19&quot;/&gt;
     &lt;node type=&quot;allow&quot; cidr=&quot;74.119.76.0/22&quot;/&gt;
     &lt;node type=&quot;allow&quot; cidr=&quot;102.132.96.0/20&quot;/&gt;
     &lt;node type=&quot;allow&quot; cidr=&quot;103.4.96.0/22&quot;/&gt;
     &lt;node type=&quot;allow&quot; cidr=&quot;129.134.0.0/16&quot;/&gt;
     &lt;node type=&quot;allow&quot; cidr=&quot;147.75.208.0/20&quot;/&gt;
     &lt;node type=&quot;allow&quot; cidr=&quot;157.240.0.0/16&quot;/&gt;
     &lt;node type=&quot;allow&quot; cidr=&quot;163.70.128.0/17&quot;/&gt;
     &lt;node type=&quot;allow&quot; cidr=&quot;163.77.128.0/17&quot;/&gt;
     &lt;node type=&quot;allow&quot; cidr=&quot;173.252.64.0/18&quot;/&gt;
     &lt;node type=&quot;allow&quot; cidr=&quot;179.60.192.0/22&quot;/&gt;
     &lt;node type=&quot;allow&quot; cidr=&quot;185.60.216.0/22&quot;/&gt;
     &lt;node type=&quot;allow&quot; cidr=&quot;185.89.216.0/22&quot;/&gt;
     &lt;node type=&quot;allow&quot; cidr=&quot;204.15.20.0/22&quot;/&gt;
   &lt;/list&gt;
```

**vars.xml**

Modify /usr/share/freeswitch/etc/freeswitch/vars.xml

```https
Add line &lt;X-PRE-PROCESS cmd=&quot;set&quot; data=&quot;rtp_secure_media=mandatory&quot;/&gt; under &lt;include&gt;

Replace
  &lt;X-PRE-PROCESS cmd=&quot;set&quot; data=&quot;default_password=1234&quot;/&gt;
with (substitute &#123;sip_ua_password&#125; with your password)
  &lt;X-PRE-PROCESS cmd=&quot;set&quot; data=&quot;default_password=&#123;sip-ua-password&#125;&quot;/&gt;

Replace
  &lt;X-PRE-PROCESS cmd=&quot;set&quot; data=&quot;domain=$$&#123;local_ip_v4&#125;&quot;/&gt;
with (substitute &#123;domain-name&#125; with your FreeSWITCH SIP server DNS)
  &lt;X-PRE-PROCESS cmd=&quot;set&quot; data=&quot;domain=&#123;domain-name&#125;&quot;/&gt;

Replace
  &lt;X-PRE-PROCESS cmd=&quot;stun-set&quot; data=&quot;external_sip_ip=stun:stun.freeswitch.org&quot;/&gt;
with (substitute &#123;external-ip&#125; with your FreeSWITCH public ip)
  &lt;X-PRE-PROCESS cmd=&quot;set&quot; data=&quot;external_sip_ip=&#123;external-ip&#125;&quot;/&gt;

Replace
  &lt;X-PRE-PROCESS cmd=&quot;stun-set&quot; data=&quot;external_rtp_ip=stun:stun.freeswitch.org&quot;/&gt;
with (substitute &#123;external-ip&#125; with your FreeSWITCH public ip)
  &lt;X-PRE-PROCESS cmd=&quot;stun-set&quot; data=&quot;external_rtp_ip=&#123;external-ip&#125;&quot;/&gt;
```

**internal.xml**

Modify `/usr/share/freeswitch/etc/freeswitch/sip_profiles/internal.xml`
Look for:

```https
&lt;param name=&quot;sip-trace&quot; value=&quot;no&quot;/&gt;
```

Replace it with

```https
&lt;param name=&quot;sip-trace&quot; value=&quot;yes&quot;/&gt;
```

**external.xml**
Modify `/usr/share/freeswitch/etc/freeswitch/sip_profiles/external.xml`

```https
Replace
  &lt;param name=&quot;sip-trace&quot; value=&quot;no&quot;/&gt;
with
  &lt;param name=&quot;sip-trace&quot; value=&quot;yes&quot;/&gt;

Replace
  &lt;param name=&quot;tls&quot; value=&quot;$$&#123;external_ssl_enable&#125;&quot;/&gt;
with
  &lt;param name=&quot;tls&quot; value=&quot;true&quot;/&gt;

Replace
  &lt;!--&lt;param name=&quot;tls-cert-dir&quot; value=&quot;&quot;/&gt;--&gt;
with
  &lt;param name=&quot;tls-cert-dir&quot; value=&quot;/usr/share/freeswitch/etc/freeswitch/certs&quot;/&gt;
```

Make sure certificates are placed under /usr/share/freeswitch/etc/freeswitch/certs

### Final checklist

* Double-check all configuration files for correct numbers, passwords, and domain names.
* Make sure your firewall allows SIP (5081/TLS) and RTP (10000-20000) ports.
* For more details on SIP password setup, see the [WhatsApp Cloud API documentation](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/sip#include-sip-user-password).

### Troubleshooting

#### Cannot register SIP UA

Confirm that the SIP URL is correct and the domain is pointing to the FreeSWITCH server. Run `host &#123;domain-name&#125;` to verify that the IP address points to the FreeSWITCH server.

#### Trace SIP messages

Start CLI (`/usr/share/freeswitch/bin/fs_cli`) to view SIP messages

## FreeSWITCH using Graph API with Janus

### Overview

This guide explains how to set up [WhatsApp Business Calling API](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling) using [WhatsApp Cloud API signaling](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/business-initiated-calls) with [FreeSWITCH](https://signalwire.com/freeswitch), an open-source communication framework and [Janus](https://janus.conf.meetecho.com/), a general-purpose WebRTC server. You&#039;ll learn how to configure your FreeSWITCH server, connect SIP phones, and handle both incoming and outgoing WhatsApp calls.

#### User-initiated calls

* The WhatsApp user dials the business number.
* The call is received by Webhook server which forwards it to FreeSWITCH server via Janus SIP plugin.
* The call is received by FreeSWITCH and routed through an IVR, prompting the user to enter an extension, registered to the same FreeSWITCH server.
* The call is then connected to the specified extension.

#### Business-initiated calls

* The business agent/user registers with FreeSWITCH using SIP credentials (see &quot;[Configuring a VoIP Phone](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/integration-examples#configuring-a-voip-phone)&quot; section).
* The business user dials the b2c-sip (business to consumer) extension, which is handled by an IVR. The IVR prompts for the WhatsApp number to call.
* FreeSWITCH bridges the call to extension registered to Janus SIP plugin which translates it to an API request to Meta
* The call is then connected to the WhatsApp user.

The Janus server sits between WA and FreeSWITCH and converts media from WA (WebRTC compliant with DTLS key exchange) to FreeSWITCH negotiated media (SDES key exchange).

FreeSWITCH - SIP UA will be using SDES for media encryption key exchange and opus or G711 for audio codec.

### Prerequisites

* FreeSWITCH Deployment: FreeSWITCH is deployed (for example, on a public cloud instance)
* Janus Deployment: Can be deployed on the same machine as FreeSWITCH
* Operating System: Any OS compatible with FreeSWITCH. For example, CentOS 9
* Domain: FreeSWITCH server and Webhook server are reachable via a public domain with valid certificate
* WhatsApp Business API: A WhatsApp Business phone number is registered and [calling is enabled](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/call-settings).
* Webhooks: Configure Webhook callback URL pointing to domain name of the Webhook server

### Integration with Cloud API signaling

You will need to implement an integration module which sits between WA and Janus and translates Cloud API Signalling messages to Janus SIP plugin messages and vice versa.

You will need

1. A webhook server to receive calls webhook events from Meta
2. A Graph API module to send call messages to Meta
3. An implementation of [Janus SIP plugin](https://janus.conf.meetecho.com/docs/sip) to connect to Janus. The Janus plugin implementation will connect to FreeSWITCH using extension 1000 which is reserved for bridging

Business initiated calls

1. The module will receive a SIP INVITE via Janus SIP plugin on extension 1000. The SIP INVITE is converted to a [Graph API request](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/business-initiated-calls#initiate-call). The SDP received in the SIP INVITE is sent verbatim as the SDP offer to WA via the Graph API call
2. When the call is accepted by the WA user, an accepted webhook is received. On receiving the webhook, the Janus SIP Plugin accepts the SIP INVITE passing the answer SDP in the [connect webhook](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/business-initiated-calls#call-connect-webhook)

User Initiated calls

1. The webhook server receives an incoming call via a webhook message containing the offer SDP. On receiving the call invite, the Janus SIP plugin sends an invite to FreeSWITCH via extension 1000. The destination extension is **c2b-sip.**
2. When the Janus SIP plugin receives the SIP 200 OK, a Graph API accept call request is sent to Meta to accept the incoming call by passing the SDP received as part of SIP answer

### Building and installing Janus

Refer to [https://github.com/meetecho/janus-gateway](https://github.com/meetecho/janus-gateway)
This guide was tested using version 1.2.3.

### Janus configuration

**janus.jcfg**

Modify janus.jcfg which can be found at /usr/share/janus/etc/janus/janus.jcfg
Set `nat_1_1_mapping` to the public IP of the Janus Server

To start Janus

```https
/usr/share/janus/bin/janus  --debug-level=6 --libnice-debug=on -S stun.l.google.com:19302 --log-file=/var/log/janus.log --config=/usr/share/janus/etc/janus/janus.jcfg
```

### Building and installing FreeSWITCH

Refer to [https://developer.signalwire.com/freeswitch/FreeSWITCH-Explained/Installation/](https://developer.signalwire.com/freeswitch/FreeSWITCH-Explained/Installation/)

This guide was tested using FreeSWITCH version 1.10.12. FreeSWITCH uses sofia (an open-source SIP user agent library). Sofia v1.13.17 was used for this guide.

**FreeSWITCH Configuration**
These configuration files are placed under /usr/share/freeswitch/etc/freeswitch

**wa-biz-api-dialplan.xml**

Place the dial plan under /usr/share/freeswitch/etc/freeswitch/dialplan/default/wa-biz-api-dialplan.xml

```https
&lt;include&gt;
 &lt;extension name=&quot;c2b_calls_ivr&quot;&gt;
   &lt;condition field=&quot;destination_number&quot; expression=&quot;^c2b-sip$&quot;&gt;
     &lt;action application=&quot;set&quot; data=&quot;rtp_secure_media=true&quot;/&gt;
     &lt;action application=&quot;answer&quot;/&gt;
     &lt;!--Add silence stream for  1 sec so that the media path is established between whatsapp and freeswitch to avoid audio clipping. TODO: Investigate if silence can be removed--&gt;
     &lt;action application=&quot;playback&quot; data=&quot;silence_stream://1000&quot;/&gt;
     &lt;action application=&quot;play_and_get_digits&quot; data=&quot;2 5 3 7000 # $$&#123;base_dir&#125;/sounds/incoming_welcome.wav  $$&#123;base_dir&#125;/sounds/incoming_invalid.wav extension \d+&quot;/&gt;
     &lt;!--While the call is being bridged, play a ringtone for the caller--&gt;
     &lt;action application=&quot;set&quot; data=&quot;ringback=%(2000, 4000, 440.0, 480.0)&quot;/&gt;
     &lt;!--WA calls bridged via Janus through extension 1000 only support OPUS. However, the callee might be restricted to other codecs for example G722--&gt;
     &lt;!--Therefore , don&#039;t restrict to OPUS for C2B calls and offer more codecs to the caller. Transcoding between OPUS and the negotiated codec by the caller--&gt;
     &lt;!--will happen in freeswitch--&gt;
     &lt;action application=&quot;export&quot; data=&quot;nolocal:absolute_codec_string=PCMA,PCMU,OPUS&#064;48000h&#064;20i,G722&quot;/&gt;
     &lt;action application=&quot;bridge&quot; data=&quot;user/$&#123;extension&#125;&quot;/&gt;
     &lt;action application=&quot;hangup&quot;/&gt;
   &lt;/condition&gt;
 &lt;/extension&gt;

 &lt;extension name=&quot;b2c_calls_ivr&quot;&gt;
   &lt;condition field=&quot;destination_number&quot; expression=&quot;^b2c-sip$&quot;&gt;
     &lt;action application=&quot;set&quot; data=&quot;rtp_secure_media=true&quot;/&gt;
     &lt;action application=&quot;answer&quot;/&gt;
     &lt;action application=&quot;playback&quot; data=&quot;silence_stream://1000&quot;/&gt;
     &lt;action application=&quot;set&quot; data=&quot;caller_id_check=$&#123;caller_id_number&#125;&quot;/&gt;
     &lt;action application=&quot;log&quot; data=&quot;INFO [caller id ] is $&#123;caller_id_check&#125;&quot;/&gt;
     &lt;action application=&quot;play_and_get_digits&quot; data=&quot;2 12 3 20000 # $$&#123;base_dir&#125;/sounds/outgoing_welcome.wav $$&#123;base_dir&#125;/sounds/outgoing_invalid.wav whatsapp_number \d+&quot;/&gt;
     &lt;action application=&quot;log&quot; data=&quot;INFO [whatsapp_number] is $&#123;whatsapp_number&#125;&quot;/&gt;
     &lt;!--Add the whatsapp number entered by the user as a custom SIP header, Janus will use this WA user number in API request to Meta--&gt;
     &lt;action application=&quot;export&quot; data=&quot;sip_h_X-WhatsApp-Number=$&#123;whatsapp_number&quot;/&gt;
     &lt;!--While the call is being bridged, play a ringtone for the caller--&gt;
     &lt;action application=&quot;set&quot; data=&quot;ringback=%(2000, 4000, 440.0, 480.0)&quot;/&gt;
     &lt;!--WA calls bridged via Janus through extension 1000 only support OPUS. However, the caller might be restricted to other codecs for example G722--&gt;
     &lt;!--Therefore , don&#039;t restrict to OPUS for B2C calls and let caller select other codecs--&gt;
     &lt;!--However, force transcoding to OPUS by only offering OPUS to Janus--&gt;
     &lt;action application=&quot;export&quot; data=&quot;nolocal:absolute_codec_string=OPUS&#064;48000h&#064;20i,PCMU,PCMA&quot;/&gt;
     &lt;!--Bridge the call to extension 1000 to which capi-calling is registered via Janus to route calls to WhatsApp--&gt;
     &lt;action application=&quot;bridge&quot; data=&quot;user/1000&quot;/&gt;
     &lt;action application=&quot;hangup&quot;/&gt;
   &lt;/condition&gt;
 &lt;/extension&gt;
&lt;/include&gt;
```

Audio files should be placed under /usr/share/freeswitch/sounds (not provided)

* `incoming_welcome.wav`
* `Incoming_invalid.wav`
* `outgoing_welcome.wav`
* `outgoing_invalid.wav`

**internal.xml**

Modify `/usr/share/freeswitch/etc/freeswitch/sip_profiles/internal.xml`
Look for:

```https
&lt;param name=&quot;sip-trace&quot; value=&quot;no&quot;/&gt;
```

Replace it with

```https
&lt;param name=&quot;sip-trace&quot; value=&quot;yes&quot;/&gt;
```

### Configuring a VoIP phone

Refer to the [earlier section](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/integration-examples#configuring-a-voip-phone)

### Final checklist

* Double-check all configuration files for correct numbers, passwords, and domain names.
* Make sure your firewall allows SIP (5061/TLS) and RTP (10000-20000) ports.
* For more details on SIP password setup, see the [WhatsApp Cloud API documentation](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/sip).

### Troubleshooting

#### Cannot register SIP UA

Confirm that the SIP URL is correct and the domain is pointing to the FreeSWITCH server. Run `host &#123;domain-name&#125;` to verify that the IP address points to the FreeSWITCH server.

#### Trace SIP messages

Start CLI (`/usr/share/freeswitch/bin/fs_cli`) to view SIP messages

## Asterisk using Graph API with RtpEngine

### Overview

This guide explains how to set up [WhatsApp Business Calling API](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling) using [WhatsApp Cloud API signaling](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/business-initiated-calls) with [Asterisk](https://www.asterisk.org/), an open-source PBX (Private Branch Exchange) and [RtpEngine](https://github.com/sipwise/rtpengine), an open-source proxy used for relaying, manipulating, and controlling RTP streams. You&#039;ll learn how to configure your Asterisk server, connect SIP phones, and handle both incoming and outgoing WhatsApp calls.

#### User-initiated calls

* The WhatsApp user dials the business number.
* The call is received by the Webhook server which after bridging media using RtpEngine, forwards it to Asterisk using SIP.
* The call is received by Asterisk and routed through an IVR, prompting the user to enter an extension, registered to the same Asterisk server.
* The call is then connected to the specified extension.

#### Business-initiated calls

* The business agent/user registers with Asterisk using SIP credentials (see &quot;[Configuring a VoIP Phone](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/integration-examples#configuring-a-voip-phone)&quot; section).
* The business user dials the b2c-sip (business to consumer) extension, which is handled by an IVR. The IVR prompts for the WhatsApp number to call.
* Asterisk bridges the call to extension registered by the integration module (see &quot;Integration with Cloud API Signalling&quot;)
* On receiving the call, the integration module bridges the media using RtpEngine and then translates it to an API request to Meta
* The call is then connected to the WhatsApp user.

RtpEngine acts as a media proxy and sits between the media stream of WA (WebRTC compliant with DTLS key exchange) and Asterisk (SDES key exchange).

### Prerequisites

* Asterisk Deployment: Asterisk is deployed (for example, on a public cloud instance)
* RtpEngine Deployment: Can be deployed on the same machine as Asterisk
* Operating System: Any OS compatible with Asterisk and RtpEngine. For example, CentOS 9
* Domain: Asterisk server and Webhook server are reachable via a public domain with valid certificate
* WhatsApp Business API: A WhatsApp Business phone number is registered and [calling is enabled](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/call-settings).
* Webhooks: Configure Webhook callback URL pointing to domain name of the Webhook server

### Integration with Cloud API signaling

You will need to implement an integration module that acts as a bridge between WhatsApp and Asterisk. This module will:

* Translate Cloud API Signaling messages from WhatsApp to SIP for Asterisk, and vice versa
* Use SIP signaling for communication between the SIP UA inside the module and Asterisk
* Bridge the media between WhatsApp and Asterisk via RtpEngine

You will need following components, which are part of the integration module for the purpose of this setup

1. Webhook Server: Receives call webhook events from Meta (WhatsApp Cloud API)
2. Graph API client: Sends call-related requests to Meta using the Graph API
3. SIP User Agent (UA) such as PJSIP: Connects to Asterisk using extension 1000, which is reserved for bridging calls between WhatsApp and Asterisk.
4. RtpEngineClient: To control RtpEngine via [ng control protocol](https://rtpengine.readthedocs.io/en/latest/ng_control_protocol.html) for bridging media

Business initiated calls

* Business agent registered to the same Asterisk server dials b2c-sip extension to initiate a call to WhatsApp user
* The extension prompts the business agent to enter WA user&#039;s phone number
* Asterisk sends a SIP INVITE request to extension 1000 with a custom header containing the dialed WA user phone number
* The SIP UA inside the module would&#039;ve registered at extension 1000 and hence receives the SIP INVITE from Asterisk
* The SDP included in the SIP INVITE is sent to RtpEngine which returns a new SDP
* The new SDP is included in the [Graph API request](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/business-initiated-calls#initiate-call) to initiate a new call
* When the WhatsApp user accepts the call, an &quot;accepted&quot; webhook is received
* Upon receiving this webhook, the answer SDP received in the webhook is sent to RtpEngine which returns a new SDP
* The SIP UA accepts the original SIP INVITE (step 3), passing along the new SDP received from RtpEngine
* The call is now bridged between WA user, RtpEngine, and Asterisk

User Initiated calls

* The webhook server inside the module receives an [incoming call webhook](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/user-initiated-calls#call-connect-webhook) from Meta, which includes the offer SDP
* Upon receiving this call invite, the SDP included in the offer is sent to RtpEngine which returns a new SDP
* The SIP UA inside the module sends a SIP INVITE to Asterisk using extension 1000 passing the new SDP from RtpEngine in the SIP INVITE. The destination extension is c2b-sip.
* The extension prompts WA user to dial the extension of the business agent to connect to
* Asterisk dials the specified extension and waits for an answer
* After the agent answers the call, Asterisk sends SIP 200 OK to the SIP UA extension 1000 inside the module. The SDP in SIP 200 OK is sent to RtpEngine which returns a new SDP
* A Graph API request is sent to Meta to [accept the incoming call](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/user-initiated-calls#accept-call), with the new SDP received from RtpEngine

### Building and installing Asterisk

Refer to the [Asterisk build and install guide](https://docs.asterisk.org/Getting-Started/Installing-Asterisk/Installing-Asterisk-From-Source/Building-and-Installing-Asterisk/).

This guide was tested using Asterisk version 22.5.2.

### Building and installing RtpEngine

Refer to [https://github.com/sipwise/rtpengine](https://github.com/sipwise/rtpengine) to build and install RtpEngine
This guide was tested using RtpEngine version 13.3.1.4.

Refer to the [RtpEngine ng control protocol documentation](https://rtpengine.readthedocs.io/en/latest/ng_control_protocol.html) for details on ng control protocol

To start RtpEngine run

```https
/usr/bin/rtpengine --listen-ng=&#123;local-ip&#125;:22222 --interface=&#123;local-ip&#125;\!&#123;public-ip&#125; -f -E
```

Replace

1. &#123;local-ip&#125; with the local IP of the RtpEngine server
2. &#123;public-ip&#125; with the public IP of the RtpEngine server

**Asterisk Configuration**
These configuration files are placed under /etc/asterisk/

**extensions.conf**

Replace the following placeholders with actual values

1. incoming_welcome: incoming_welcome.wav (not provided) place this file under /var/lib/asterisk/sounds
2. outgoing_welcome: outgoing_welcome.wav (not provided) place this file under /var/lib/asterisk/sounds

```https
[handler]
;Set headers on callee channel
exten =&gt; addheader,1,Set(PJSIP_HEADER(add,X-WhatsApp-Number)=$&#123;DIGITS&#125;)
same =&gt; n,Return()

[default]
exten =&gt; _10XX,1,NoOp()
same =&gt; n,Dial(PJSIP/$&#123;EXTEN&#125;)
same =&gt; n,Hangup()

exten =&gt; b2c-sip,1,NoOp()
same =&gt; n,Read(Digits,outgoing_welcome,0,,5, 500)
same =&gt; n, Set(GLOBAL(DIGITS)=$&#123;Digits&#125;)
;Before starting a business initiated call, add customer WA header to store the WA user number captured from agent entered digits (DTMF)
same =&gt; n,Dial(PJSIP/1000,,b(handler^addheader^1))
same =&gt; n,Hangup()

exten =&gt; c2b-sip,1,NoOp()
same =&gt; n,Read(Digits,incoming_welcome,0,,5, 500)
same =&gt; n,Dial(PJSIP/$&#123;Digits&#125;)
same =&gt; n,Hangup()
```

**pjsip.conf**

Replace the following placeholders with actual values

1. &#123;external-media-address&#125;: Public IP of the Asterisk server for media
2. &#123;external-signaling-address&#125;: Public IP of the Asterisk server for signaling
3. &#123;local-net&#125;: local network of the Asterisk server
4. &#123;sip-ua-password&#125;: Chosen SIP User Agent password

Note:

Extension 1000 is used to bridge WA calls with Asterisk see section **Integration with Cloud API Signaling**

```https
[global]
type=global
debug=yes ; Enable/Disable SIP debug logging.  Valid options include yes|no

[transport-tcp]
type=transport
protocol=tcp
bind=0.0.0.0
;External IP address to use in RTP handling
external_media_address=&#123;external-media-address&#125;
;External address for SIP signalling
external_signaling_address=&#123;external-signaling-address&#125;
;Network to consider local used for NAT purposes
local_net=&#123;local-net&#125;

[endpointtemplate](!)
type=endpoint
context=default
disallow=all
allow=OPUS,g722,g729,ulaw
;No audio if direct_media is set to yes
direct_media=no
rtp_symmetric=yes
use_avpf=yes
media_encryption=sdes
media_use_received_transport=yes
rtcp_mux=yes

[authtemplate](!)
type=auth
auth_type=userpass
password=&#123;sip-ua-password&#125;

[aortemplate](!)
type=aor
max_contacts=1
remove_existing=yes

[1000](endpointtemplate)
disallow=all
;extension 1000 is used by RtpEngine to bridge whatsapp calls
;WhatsApp only support OPUS
allow=OPUS
auth=1000_auth
aors=1000

[1000_auth](authtemplate)
username=1000

[1000](aortemplate)

[1001](endpointtemplate)
auth=1001_auth
aors=1001

[1001_auth](authtemplate)
username=1001

[1001](aortemplate)

[1002](endpointtemplate)
auth=1002_auth
aors=1002

[1002_auth](authtemplate)
username=1002

[1002](aortemplate)

[1003](endpointtemplate)
auth=1003_auth
aors=1003

[1003_auth](authtemplate)
username=1003

[1003](aortemplate)

[1004](endpointtemplate)
auth=1004_auth
aors=1004

[1004_auth](authtemplate)
username=1004

[1004](aortemplate)

[1005](endpointtemplate)
auth=1005_auth
aors=1005

[1005_auth](authtemplate)
username=1005

[1005](aortemplate)
```

### Configuring a VoIP phone

Refer to the [earlier section](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/integration-examples#configuring-a-voip-phone)

### Final checklist

* Double-check all configuration files for correct numbers, passwords, and domain names.
* Make sure your firewall allows SIP (5060/TCP) and RTP (10000-20000) ports.
* For more details on SIP password setup, see the [WhatsApp Cloud API documentation](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/sip).

### Troubleshooting

#### Cannot register SIP UA

Confirm that the SIP URL is correct and the domain is pointing to the Asterisk server. Run `host &#123;domain-name&#125;` to verify that the IP address points to the Asterisk server.

## Asterisk with built-in WebRTC using Graph API

This approach is similar to [Asterisk using Graph API with RtpEngine
](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/integration-examples#asterisk-using-graph-api-with-rtpengine) except that it uses the built-in WebRTC support in Asterisk and hence does not require RtpEngine.

The RtpEngineClient component is hence not required in this approach.

In terms of configuration and setup, only difference is the configuration of extension 1000 which is given below.

```
...
; Rest of content omitted for brevity

[1000](endpointtemplate)
disallow=all
;extension 1000 is used by SIP UA of the integration module to bridge WhatsApp calls
;WhatsApp only support OPUS
allow=OPUS
auth=1000_auth
aors=1000
dtls_auto_generate_cert=yes
webrtc=yes
; Setting webrtc=yes is a shortcut for setting the following options:
; use_avpf=yes
; media_encryption=dtls
; dtls_verify=fingerprint
; dtls_setup=actpass
; ice_support=yes
; media_use_received_transport=yes
; rtcp_mux=yes

```
