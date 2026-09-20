# SIP Configuration Guide - WhatsApp Business Calling



**Warning:** When SIP is enabled, you **cannot use calling-related Graph API endpoints**. By default, **calling-related webhooks are not sent**, but you can enable webhook delivery for SIP calls to receive call lifecycle events.

## Overview

Session Initiation Protocol ([SIP](https://datatracker.ietf.org/doc/html/rfc3261)) is a signaling protocol used for initiating, maintaining, modifying, and terminating real-time communication sessions between two or more endpoints.

WhatsApp Business Calling API supports use of SIP as the signaling protocol instead of our Graph API endpoints.

### Before you get started

Before you get started with SIP call signaling, confirm the following:

* You meet overall [calling prerequisites](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling#step-1--prerequisites).
* Your app has messaging permissions for the business phone number you want to enable SIP for.
  * Test this by sending and receiving messages using Graph API messaging endpoints, then use the same app to configure your SIP server on the business phone number for calling.
  * Verify this by using the [health status API](https://developers.facebook.com/documentation/business-messaging/whatsapp/support/health-status) with `PHONE_NUMBER_ID`.
* Your app mode is &quot;Live&quot;, not &quot;Development&quot;.
* You have a standards-compliant third-party SIP server that supports [TLS](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/sip#security) transport and digest authentication.

See [Signaling and media possible configurations](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling#signaling-and-media-possible-configurations) for more info.

## Calling flows using SIP

Before you start, make sure you have [enabled and configured SIP on the business phone number](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/sip#configure-update-sip-settings-on-business-phone-number). Meta generates a unique SIP user password for each business phone number + app combination. You will need this information and can retrieve it by using the [get Call Settings endpoint](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/sip#get-phone-number-calling-settings--sip-).

### Security

* TLS transport is mandatory for SIP. Meta will present a valid server cert with subject name that covers our SIP domain wa.meta.vc.
Your SIP server should do the same as Meta ensures your cert is valid and subject name covers SIP domain you configured on the business phone number.
  * Meta does NOT support mutual TLS (aka mTLS). This means, when Meta takes the role of a TLS client, your TLS server should not request Client certificate. If you still request client cert, Meta will present a client certificate. However, the cert subject name would refer to a random dynamic host which will not pass certificate validation.
  * Meta adds `transport=TLS` to request URI as part of its SIP requests to your SIP server.
* For business initiated calls, SIP invite from your SIP server will be challenged using digest auth. See [business-initiated calls](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/sip#business-initiated-calls) for more details.
* For user-initiated calls, we strongly recommend that you challenge the SIP INVITE request from Meta with digest auth for added security. See [user-initiated calls](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/sip#user-initiated-calls) for more details.

### How to test if you have a valid TLS certificate

When a WhatsApp user calls a business, a common reason for your SIP server to **not** receive the SIP INVITE from Meta is the certificate validation error. You can use information here to confirm valid setup.

Run the command `openssl s_client -quiet -verify_hostname &#123;hostname&#125; -connect &#123;hostname&#125;:&#123;port&#125;` by properly substituting hostname and port with your values.

#### Example of valid server cert

```
$ openssl s_client -quiet -verify_hostname meta-voip.example.com -connect meta-voip.example.com:5061
Connecting to 64:ff9b::68f8:b0b8
depth=2 C=US, ST=New Jersey, L=Jersey City, O=The USERTRUST Network, CN=USERTrust RSA Certification Authority
verify return:1
depth=1 C=AT, O=ZeroSSL, CN=ZeroSSL RSA Domain Secure Site CA
verify return:1
depth=0 CN=example.com
verify return:1
```

#### Example of hostname:port not listening on TLS

```
openssl s_client -quiet -verify_hostname lb01.voice.usw2.pure.cloud -connect lb01.voice.usw2.pure.cloud:5060
Connecting to 34.211.206.63
009F0DFB01000000:error:0A000126:SSL routines::unexpected eof while reading:ssl/record/rec_layer_s3.c:693:
```

#### Example of invalid cert

```
$ openssl s_client -quiet -verify_hostname meta-inb.byoc.mypurecloud.com -connect meta-inb.byoc.mypurecloud.com:5061
Connecting to 64:ff9b::3652:f1c0
depth=0 jurisdictionC=US, jurisdictionST=California, businessCategory=Private Organization, serialNumber=1515861, C=US, ST=Indiana, L=Indianapolis, O=Genesys Cloud Services, Inc., CN=voice.mypurecloud.com
verify error:num=62:hostname mismatch
verify return:1
depth=2 C=US, O=DigiCert Inc, OU=www.digicert.com, CN=DigiCert High Assurance EV Root CA
verify return:1
depth=1 C=US, O=DigiCert Inc, OU=www.digicert.com, CN=DigiCert SHA2 Extended Validation Server CA
verify return:1
depth=0 jurisdictionC=US, jurisdictionST=California, businessCategory=Private Organization, serialNumber=1515861, C=US, ST=Indiana, L=Indianapolis, O=Genesys Cloud Services, Inc., CN=voice.mypurecloud.com
verify return:1
```

In this case, you can alter the certificate to match your hostname or [change your configured SIP server hostname](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/sip#configure-update-sip-settings-on-business-phone-number) to match your certificate.

### Business-initiated calls

#### Prerequisites

* You have the required call permission approval from the WhatsApp user
  * [Learn how to obtain user calling permissions](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/user-call-permissions)
* [Retrieve Meta generated SIP password](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/sip#include-sip-user-password) and configure it on your SIP server, so it can respond to digest authentication challenge from Meta SIP servers

#### Calling flow

1. Send an initial [SIP INVITE](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/sip#business-initiated-calls--with-webrtc-media-) to our servers. Our SIP domain is wa.meta.vc. To initiate a call to WhatsApp user with phone number 11234567890, the SIP request URI should be &#039;sip:+11234567890&#064;wa.meta.vc;transport=tls&#039;
  * This request fails with a 407 Proxy Authentication Required response.
1. Send a second [SIP INVITE](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/sip#business-initiated-calls--with-webrtc-media-) with proper Authorization header as per [RFC 3261](https://datatracker.ietf.org/doc/html/rfc3261#section-22).
  * The Authorization field&#039;s `username` attribute must match the `from` header&#039;s user name which is the business phone number
  * The password is generated by Meta and you can retrieve it using [get Call Settings endpoint](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/sip#get-phone-number-calling-settings--sip-)
  * The username portion of the from header must be the fully normalized business phone number
  * The domain name of the from header must match the SIP server you configured on the business phone number
  * The `SDP Offer` you include supports ICE, DTLS-SRTP, and OPUS (essentially WebRTC media)
1. Send the [SIP INVITE](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/sip#business-initiated-calls--with-webrtc-media-) to the WhatsApp user number you want to call.

### User-initiated calls

#### Prerequisites

* If you plan to use SIP Digest Auth, [retrieve Meta generated SIP password](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/sip#include-sip-user-password) and configure it on your SIP server, so it can respond to digest authentication challenge from Meta SIP servers

#### Calling flow

1. The WhatsApp user calls business phone number and is unaware of whether the business is using SIP or Graph API. In other words, the user experience is identical.
1. If the business phone number is SIP enabled, Meta sends a [SIP INVITE](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/sip#user-initiated-calls--with-webrtc-media-) to the SIP server [configured on the business phone number](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/sip#configure-update-sip-settings-on-business-phone-number).
1. You respond with [SIP digest auth challenge](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/sip#user-initiated-calls-with-digest-auth--with-sdes-media-) (recommended) or [SIP OK](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/sip#user-initiated-calls--with-webrtc-media-) and pass in an SDP answer.

**Warning:** If you are not receiving SIP INVITE from Meta, refer to [SIP specific FAQ](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/faq#session-initiation-protocol--sip--faq) to troubleshoot further

[View sample SIP requests](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/sip#sample-sip-requests)

[Learn more about Session Description Protocol (SDP)](https://www.rfc-editor.org/rfc/rfc8866.html)

[View example SDP structures](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/reference#sdp-overview-and-sample-sdp-structures)

### Custom SIP headers

The following custom SIP headers are common to both business and user initiated calls.

| Header name | Metadata | Description |
| --- | --- | --- |
| x-wa-meta-call-duration | Optional; String | Call duration in seconds. This is present on SIP BYE requests from Meta for termination of an established call. |
| x-wa-meta-wacid | Optional; String | WhatsApp call ID. This is present on SIP INVITE request from Meta for a user-initiated call and SIP BYE requests from Meta for termination of an established call. |
| x-wa-meta-user-id | Optional; String | The WhatsApp user&#039;s business-scoped user ID (BSUID). Present on SIP messages from Meta (INVITE, 200 OK, BYE) when the business has BSUIDs enabled. See [Business-scoped user IDs](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-scoped-user-ids#business-scoped-user-id). |
| x-wa-meta-parent-user-id | Optional; String | The WhatsApp user&#039;s parent BSUID, if parent BSUIDs are enabled for the business; otherwise omitted. See [Parent business-scoped user IDs](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-scoped-user-ids#parent-business-scoped-user-ids). |
| x-wa-meta-username | Optional; String | The WhatsApp user&#039;s username, if they have adopted one; otherwise omitted. |

The following custom SIP headers are specific to user-initiated calls.

| Header name | Metadata | Description |
| --- | --- | --- |
| x-wa-meta-cta-payload | Optional; String | Present when user-initiates a call from call button that has business specified payload. [Learn more](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/call-button-messages-deep-links#send-interactive-message-with-a-whatsapp-call-button) |
| x-wa-meta-deeplink-payload | Optional; String | Present when user-initiates a call from call deeplink that has business specified payload. [Learn more](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/call-button-messages-deep-links#send-payload-data-in-call-deeplink) |

## Configure or update SIP settings on business phone number

Use this endpoint to update call settings configuration for an individual business phone number.

### Request syntax

```html
POST /&lt;PHONE_NUMBER_ID&gt;/settings
```

### Endpoint parameters

| Placeholder | Description | Sample Value |
| --- | --- | --- |
| `&lt;PHONE_NUMBER_ID&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | **Required**&lt;br&gt;&lt;br&gt;The business phone number for which you are updating Calling API settings.&lt;br&gt;&lt;br&gt;[Learn more about formatting phone numbers in Cloud API](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-phone-numbers/phone-numbers) | `+12784358810` |

### Request body

```html
&#123;
  &quot;calling&quot;: &#123;
    ... // other calling api settings
    &quot;sip&quot;: &#123;
      &quot;status&quot;: &quot;ENABLED&quot;,
      &quot;webhook_delivery&quot;: &quot;ENABLED&quot;,
      &quot;servers&quot;: [
        &#123;
          &quot;hostname&quot;: SIP_SERVER_HOSTNAME
          &quot;port&quot;: SIP_SERVER_PORT,
          &quot;request_uri_user_params&quot;: &#123;
            &quot;KEY1&quot;: &quot;VALUE1&quot;, // for cases like trunk groups (tgrp)
            &quot;KEY2&quot;: &quot;VALUE2&quot;,
          &#125;
        &#125;
      ]
    &#125;
  &#125;,
  // Other non calling api feature configurations
&#125;
```

### Body parameters

| Parameter | Description |
| --- | --- |
| `status`&lt;br&gt;&lt;br&gt;_String_ | **Optional**&lt;br&gt;&lt;br&gt;Enable or disable SIP call signaling for the given business phone number.&lt;br&gt;&lt;br&gt;Default is `DISABLED`.&lt;br&gt;&lt;br&gt;When `status` is `ENABLED`, this phone number exclusively uses SIP for call signaling and will not work with Graph APIs.&lt;br&gt;&lt;br&gt;When `status` is set to `DISABLED`, the SIP `servers` values are not reset.&lt;br&gt;&lt;br&gt;If you enable SIP on the same phone number again, the previously configured `servers` values will take effect.&lt;br&gt;&lt;br&gt;You can configure both status and SIP servers in the same request. |
| `webhook_delivery`&lt;br&gt;&lt;br&gt;_String_ | **Optional**&lt;br&gt;&lt;br&gt;Enable or disable webhook delivery for SIP calls.&lt;br&gt;&lt;br&gt;Default is `DISABLED`.&lt;br&gt;&lt;br&gt;When set to `ENABLED`, SIP call lifecycle webhooks (`call_created` and `terminate`) will be sent to your configured webhook endpoint. This can allow your messaging subsystem to be aware of ongoing call events without your SIP infrastructure sharing that information directly.&lt;br&gt;&lt;br&gt;Only applicable when `status` is `ENABLED`.&lt;br&gt;&lt;br&gt;See [SIP Call Webhooks](#sip-call-webhooks) for details. |
| `servers`&lt;br&gt;&lt;br&gt;_String_ | **Optional**&lt;br&gt;&lt;br&gt;The SIP server routing configuration.&lt;br&gt;&lt;br&gt;Each phone number can have only one SIP server configured. The `servers` field is an array for forward compatibility.&lt;br&gt;&lt;br&gt;Meta previously allowed multiple apps each with their own SIP server but this setup will not work because Meta will terminate the call after receiving BYE from any of the SIP servers.&lt;br&gt;&lt;br&gt;In the GET payload, if you see multiple SIP servers, it means you&#039;ve used the POST API with different access tokens that belong to different apps.&lt;br&gt;&lt;br&gt;The associated app is extracted from the access token used to make the API call.&lt;br&gt;&lt;br&gt;To delete a previously configured SIP server, pass an empty array to this field. If you still see some servers remaining after you clear, those servers may belong to different apps, so you need to use the corresponding access tokens to clear them.&lt;br&gt;&lt;br&gt;Note that at least one SIP server of any app must exist when SIP status is ENABLED. To clear servers for all applications being used with a business phone number, the SIP status should be DISABLED.&lt;br&gt;&lt;br&gt;`hostname` — (_String_) **Required**&lt;br&gt;&lt;br&gt;The host name of the SIP server.&lt;br&gt;&lt;br&gt;Requests must use TLS.&lt;br&gt;&lt;br&gt;`port` — (_String_) **Required**&lt;br&gt;&lt;br&gt;The port within your SIP server that will accept requests.&lt;br&gt;&lt;br&gt;Requests must use TLS.&lt;br&gt;&lt;br&gt;Default port is 5061.&lt;br&gt;&lt;br&gt;`request_uri_user_params` — (_String_) **Optional**&lt;br&gt;&lt;br&gt;An optional field for passing any parameters you want included in the user portion of the request URI used in our SIP INVITE to your SIP server.&lt;br&gt;&lt;br&gt;Max key/value size is 128 characters.&lt;br&gt;&lt;br&gt;An example use case would be Trunk Groups ([RFC 4904](https://datatracker.ietf.org/doc/html/rfc4904)).&lt;br&gt;&lt;br&gt;* sip:+1234567890&#064;sip.example.com&lt;br&gt;* tgrp=wacall&lt;br&gt;* trunk-context=byoc.example.com&lt;br&gt;&lt;br&gt;This example has two user parameters for tgrp, and trunk-context.&lt;br&gt;&lt;br&gt;The effective SIP request URI line for this would be `sip:+1234567890;tgrp=wacall;trunk-context=byoc.example.com&#064;sip.example.com` |

### Success response

```html
&#123;
  &quot;success&quot;: true
&#125;
```

### Error response

[View Calling API Error Codes and Troubleshooting for more information](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/troubleshooting)

[View general Cloud API Error Codes here](https://developers.facebook.com/documentation/business-messaging/whatsapp/support/error-codes)

## Get phone number calling settings (SIP)

Use this endpoint to check the configuration of your Calling API feature settings, including SIP values.

This endpoint can return information for other Cloud API feature settings.

### Request syntax

```html
GET /&lt;PHONE_NUMBER_ID&gt;/settings
```

### Endpoint parameters

| Placeholder | Description | Sample Value |
| --- | --- | --- |
| `&lt;PHONE_NUMBER_ID&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | **Required**&lt;br&gt;&lt;br&gt;The business phone number for which you are retrieving Calling API settings.&lt;br&gt;&lt;br&gt;[Learn more about formatting phone numbers in Cloud API](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-phone-numbers/phone-numbers) | `+12784358810` |

#### App permission required

`whatsapp_business_management`: Advanced access is required to use the API for end business clients

### Response body

```html
&#123;
  &quot;calling&quot;: &#123;
    &quot;status&quot;: &quot;ENABLED&quot;,
    &quot;call_icon_visibility&quot;: &quot;DEFAULT&quot;,
    &quot;callback_permission_status&quot;: &quot;ENABLED&quot;,
    &quot;sip&quot;: &#123;
      &quot;status&quot;: &quot;ENABLED&quot;,
      &quot;servers&quot;: [
        &#123;
          &quot;app_id&quot;: &lt;APP_ID_THAT_CONFIGURED_THIS_SIP_SERVER&gt;,
          &quot;hostname&quot;: &quot;sip.example.com&quot;
        &#125;
      ]
    &#125;
  &#125;
&#125;
```

### Include SIP user password

By default, the response body does not include the Meta generated SIP password. To include the password in the response body, add the optional SIP credentials query parameter in the GET request:

```html
GET /&lt;PHONE_NUMBER_ID&gt;/settings?include_sip_credentials=true
```

Where the response will look like this:

```html
&#123;
  &quot;calling&quot;: &#123;
    &quot;status&quot;: &quot;ENABLED&quot;,
    &quot;call_icon_visibility&quot;: &quot;DEFAULT&quot;,
    &quot;callback_permission_status&quot;: &quot;ENABLED&quot;,
    &quot;sip&quot;: &#123;
      &quot;status&quot;: &quot;ENABLED&quot;,
      &quot;servers&quot;: [
        &#123;
          &quot;app_id&quot;: &lt;APP_ID_THAT_CONFIGURED_THIS_SIP_SERVER&gt;,
          &quot;hostname&quot;: &quot;sip.example.com&quot;,
          &quot;sip_user_password&quot;: &quot;&#123;SIP_USER_PASSWORD&#125;&quot;
        &#125;
      ]
    &#125;
  &#125;
&#125;
```

### Error response

[View Calling API Error Codes and Troubleshooting for more information](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/troubleshooting)

[View general Cloud API Error Codes here](https://developers.facebook.com/documentation/business-messaging/whatsapp/support/error-codes)

## Reset SIP password
To make Meta generate a new SIP password, you would need to disable SIP, delete SIP server and add your SIP server back.

* [Fetch your SIP configuration with password](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/sip#include-sip-user-password) to view your current password for your reference
* Disable and delete your SIP server

```curl
curl -X POST \
https://graph.facebook.com/&#123;VERSION&#125;/&#123;PHONE_NUMBER_ID&#125;/settings \
-H &#039;Authorization: Bearer &#123;TOKEN&#125;&#039; \
-H &#039;Content-Type: application/json&#039; \
-d &#039;
&#123;
  &quot;calling&quot;: &#123;
    &quot;status&quot;: &quot;DISABLED&quot;,
    &quot;sip&quot;: &#123;
      &quot;status&quot;: &quot;DISABLED&quot;,
      &quot;servers&quot;: []
    &#125;
  &#125;
&#125;&#039;
&#123;&quot;success&quot;:true&#125;
```

* Enable SIP and add your SIP server

```curl
curl -X POST \
https://graph.facebook.com/&#123;VERSION&#125;/&#123;PHONE_NUMBER_ID&#125;/settings \
-H &#039;Authorization: Bearer &#123;TOKEN&#125;&#039; \
-H &#039;Content-Type: application/json&#039; \
-d &#039;
&#123;
  &quot;calling&quot;: &#123;
    &quot;status&quot;: &quot;ENABLED&quot;,
    &quot;sip&quot;: &#123;
      &quot;status&quot;: &quot;ENABLED&quot;,
      &quot;servers&quot;: [&#123;&quot;hostname&quot;:&quot;sip.example.com&quot;&#125;],
    &#125;
  &#125;
&#125;&#039;
&#123;&quot;success&quot;:true&#125;
```

* [Fetch your SIP configuration with password](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/sip#include-sip-user-password) to note the new password

## SIP call webhooks

SIP calls now support webhooks — providing call lifecycle events to your webhook endpoint when you enable webhooks on a SIP-enabled number.

| Webhook | Description |
|---------|-------------|
| Call created | Sent when a SIP call is attempted |
| Call terminate | Sent when the call ends for any reason |

These webhooks apply to both business-initiated and user-initiated SIP calls.

### Prerequisites

To receive SIP call webhooks, you must:

1. [Subscribe](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/create-webhook-endpoint#configure-webhooks) to the **calls** webhook field
2. [Enable SIP on the business phone number](#configure-or-update-sip-settings-on-business-phone-number)
3. Enable webhook delivery for SIP calls by setting `webhook_delivery` to `ENABLED` in the [SIP settings](#configure-or-update-sip-settings-on-business-phone-number) (disabled by default)

### Call created webhook

A webhook notification is sent when a SIP call is attempted.

For the webhook payload structure and field descriptions, see the [Call created webhook](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/reference#call-created-webhook) section in the API and Webhook Reference.

**Note:** **Call ID mapping:** The `id` field in the webhook payload contains the WhatsApp Call ID (WACID), which is a unique identifier for the call (for example, `wacid.ABGGFjFVU2AfAgo6V-Hc5eCgK5Gh`). This WACID can be correlated with the `x-wa-meta-wacid` custom SIP header in SIP signaling messages to map webhook events to specific SIP call sessions.

**Note:** SIP webhooks do not include SDP (Session Description Protocol) information because the SIP server handles the media. These webhooks are informational only, to keep your messaging system informed about call lifecycle events.

### Call terminate webhook

Sent when the call ends for any reason. The terminate webhook for SIP calls has the same structure as the standard call terminate webhook. See the [call terminate webhook reference](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/reference#call-terminate-webhook).

## Sample SIP requests

**Note:** **Usernames and business-scoped user IDs:** In SIP signaling, a WhatsApp user may be identified by their business-scoped user ID (BSUID) or parent BSUID instead of a phone number, and Meta includes the user&#039;s BSUID, parent BSUID, and username in the `x-wa-meta-user-id`, `x-wa-meta-parent-user-id`, and `x-wa-meta-username` SIP headers. The phone number may be omitted. For details, see [Business-scoped user IDs](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-scoped-user-ids#business-scoped-user-id).

### Business-initiated calls (with WebRTC media)

#### Initial SIP INVITE request

```
INVITE sip:+12195550714&#064;wa.meta.vc;transport=tls SIP/2.0
Record-Route: &lt;sip:+159.65.244.171:5061;transport=tls;lr;ftag=Kc9QZg4496maQ;nat=yes&gt;
Via: SIP/2.0/TLS 159.65.244.171:5061;received=2803:6081:798c:93f8:5f9b:bfe8:300:0;branch=z9hG4bK0da2.36614b8977461b486ceabc004c723476.0;i=617261
Via: SIP/2.0/TLS 137.184.87.1:35181;rport=56533;received=137.184.87.1;branch=z9hG4bKQNa6meey5Dj2g
Max-Forwards: 69
From: &lt;sip:+17125550259&#064;meta-voip.example.com&gt;;tag=Kc9QZg4496maQ
To: &lt;sip:+12195550714&#064;wa.meta.vc&gt;
Call-ID: dc2c5b33-1b81-43ee-9213-afb56f4e56ba
CSeq: 96743476 INVITE
Contact: &lt;sip:mod_sofia&#064;137.184.87.1:35181;transport=tls;swrad=137.184.87.1~56533~3&gt;
User-Agent: SignalWire
Allow: INVITE, ACK, BYE, CANCEL, OPTIONS, MESSAGE, INFO, UPDATE, REGISTER, REFER, NOTIFY
Supported: timer, path, replaces
Allow-Events: talk, hold, conference, refer
Session-Expires: 600;refresher=uac
Min-SE: 90
Content-Type: application/sdp
Content-Disposition: session
Content-Length: 2427
X-Relay-Call-ID: dc2c5b33-1b81-43ee-9213-afb56f4e56ba
Remote-Party-ID: &lt;sip:+17125550259&#064;meta-voip.example.com&gt;;party=calling;screen=yes;privacy=off
Content-Type: application/sdp
Content-Length:  2427

&lt;&lt;SDP omitted for brevity&gt;&gt;
```

In this example, the called WhatsApp user is identified by their phone number (`+12195550714`) in the request-URI and `To:` header. If the call targets a user by their BSUID or parent BSUID, the BSUID or parent BSUID appears in the request-URI and `To:` header instead of the phone number. For details, see [Business-scoped user IDs](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-scoped-user-ids#business-scoped-user-id).

#### 407 response from Meta

```
SIP/2.0 407 Proxy Authentication Required
Via: SIP/2.0/TLS 159.65.244.171:5061;received=2803:6081:798c:93f8:5f9b:bfe8:300:0;branch=z9hG4bK0da2.36614b8977461b486ceabc004c723476.0;i=617261
Via: SIP/2.0/TLS 137.184.87.1:35181;rport=56533;received=137.184.87.1;branch=z9hG4bKQNa6meey5Dj2g
Record-Route: &lt;sip:+159.65.244.171:5061;transport=tls;lr;ftag=Kc9QZg4496maQ;nat=yes&gt;
Call-ID: dc2c5b33-1b81-43ee-9213-afb56f4e56ba
From: &lt;sip:+17125550259&#064;meta-voip.example.com&gt;;tag=Kc9QZg4496maQ
To: &lt;sip:+12195550714&#064;wa.meta.vc&gt;;tag=z9hG4bK0da2.36614b8977461b486ceabc004c723476.0
CSeq: 96743476 INVITE
Proxy-Authenticate: Digest realm=&quot;wa.meta.vc&quot;,nonce=&quot;419ac2415577f8e1&quot;,opaque=&quot;440badfc05072367&quot;,algorithm=MD5,qop=&quot;auth&quot;
```

#### Second SIP INVITE sent with authorization

```
INVITE sip:+12195550714&#064;wa.meta.vc;transport=tls SIP/2.0
        Record-Route: &lt;sip:+159.65.244.171:5061;transport=tls;lr;ftag=Kc9QZg4496maQ;nat=yes&gt;
        Via: SIP/2.0/TLS 159.65.244.171:5061;received=2803:6081:798c:93f8:5f9b:bfe8:300:0;branch=z9hG4bK1da2.ed8900012befced853927008d619d374.0;i=617261
        Via: SIP/2.0/TLS 137.184.87.1:35181;rport=56533;received=137.184.87.1;branch=z9hG4bKry3yp9y12p8mc
        Max-Forwards: 69
        From: &lt;sip:+17125550259&#064;meta-voip.example.com&gt;;tag=Kc9QZg4496maQ
        To: &lt;sip:+12195550714&#064;wa.meta.vc&gt;
        Call-ID: dc2c5b33-1b81-43ee-9213-afb56f4e56ba
        CSeq: 96743477 INVITE
        Contact: &lt;sip:mod_sofia&#064;137.184.87.1:35181;transport=tls;swrad=137.184.87.1~56533~3&gt;
        User-Agent: SignalWire
        Allow: INVITE, ACK, BYE, CANCEL, OPTIONS, MESSAGE, INFO, UPDATE, REGISTER, REFER, NOTIFY
        Supported: timer, path, replaces
        Allow-Events: talk, hold, conference, refer
        Proxy-Authorization: Digest username=&quot;17125550259&quot;, realm=&quot;wa.meta.vc&quot;, nonce=&quot;419ac2415577f8e1&quot;, uri=&quot;sip:+12195550714&#064;wa.meta.vc;transport=tls&quot;, response=&quot;blah&quot;, algorithm=MD5, cnonce=&quot;/mVZtYFCEj65YQJCrBEAAg&quot;, opaque=&quot;440badfc05072367&quot;, qop=auth, nc=00000001
        Session-Expires: 600;refresher=uac
        Min-SE: 90
        Content-Type: application/sdp
        Content-Disposition: session
        Content-Length: 2427
        X-Relay-Call-ID: dc2c5b33-1b81-43ee-9213-afb56f4e56ba
        Remote-Party-ID: &lt;sip:+17125550259&#064;meta-voip.example.com&gt;;party=calling;screen=yes;privacy=off
        Content-Type: application/sdp
        Content-Length:  2427
        &lt;&lt;SDP omitted for brevity&gt;&gt;
```
#### Example error response
```
SIP/2.0 403 SIP server wa.meta.vc from INVITE does not match any SIP server configured for phone number id &#123;ID&#125;
        Via: SIP/2.0/TLS [2803:6080:c954:b533:ecfb:5cec:300:0]:39459;rport=39459;received=2803:6080:c954:b533:ecfb:5cec:300:0;branch=z9hG4bKPjf9f3d0bddb3dbe0c9b1e3b486f39784a;alias
        Via: SIP/2.0/TLS 148.72.155.236:5061;rport=30498;received=2803:6080:d014:8e40:ddbb:4ed7:300:0;branch=z9hG4bKPjfd270ec8-7aaf-4a65-b290-4bef3b50b7b7;alias
        Record-Route: &lt;sip:onevc-sip-proxy-dev.fbinfra.net:8191;transport=tls;lr&gt;
        Record-Route: &lt;sip:wa.meta.vc;transport=tls;lr&gt;
        Call-ID: 91578781-44f1-4268-9a7f-d7efec1abf72
        From: &lt;sip:+17125550259&#064;wa.meta.vc&gt;;tag=3a63b370-a697-4a5a-82b4-e8105e23f176
        To: &lt;sip:+12195550714&#064;wa.meta.vc&gt;;tag=e0d30a05-657b-47ec-a668-e05ca79f9f05
        CSeq: 15659 INVITE
        Allow: INVITE, ACK, BYE, CANCEL, NOTIFY, OPTIONS
        X-FB-External-Domain: wa.meta.vc
        Warning: 399 wa.meta.vc &quot;SIP server wa.meta.vc from INVITE does not match any SIP server configured for phone number id &#123;ID&#125;&quot;
        Content-Length: 0
        Content-Length:  0
```
#### SIP BYE

```
BYE sip:+5559800000693&#064;wa.meta.vc;transport=tls;ob SIP/2.0
Via: SIP/2.0/TLS 137.184.4.155:5061;received=2803:6080:c074:cac:10ed:4b05:400:0;i=8d2dc2
Via: SIP/2.0/TLS 143.198.136.243:35181;rport=38087;received=143.198.136.243
Route: &lt;sip:wa.meta.vc;transport=tls;lr&gt;
Route: &lt;sip:onevc-sip-proxy.fbinfra.net:8191;transport=tls;lr&gt;
Max-Forwards: 69
From: &lt;sip:+12145551869&#064;meta-voip.example.com&gt;;tag=NcKQ6mtDKSDQB
To: &quot;5559800000693&quot; &lt;sip:+5559800000693&#064;wa.meta.vc&gt;;tag=92a01092-ee78-4870-865f-bc176203a6bd
Call-ID: outgoing:wacid.HBgPMjAwNzU2OTA0ODY5OTY1FRIAEhggMjQ4QzUwOUQ1REQ0NDUwNENEQzRFMTgwRTNGQjAwNjEcGAsxMjE0NTU1MTg2ORUCAAA
CSeq: 98734935 BYE
User-Agent: SignalWire
Allow: INVITE, ACK, BYE, CANCEL, OPTIONS, MESSAGE, INFO, UPDATE, REGISTER, REFER, NOTIFY
Supported: timer, path, replaces
Reason: Q.850;cause=16;text=&quot;NORMAL_CLEARING&quot;
Content-Length: 0
X-Relay-Call-ID: b72c0c65-e319-41b3-afb7-19ebcca05d38
Content-Length:  0
```
#### SIP INVITE (with SDES)

```
INVITE sip:+12195550714&#064;wa.meta.vc;transport=tls SIP/2.0
Record-Route: &lt;sip:54.172.60.1:5061;transport=tls;lr;r2=on&gt;
Record-Route: &lt;sip:54.172.60.1;lr;r2=on&gt;
CSeq: 2 INVITE
From: &quot;12145551869&quot; &lt;sip:+12145551869&#064;meta-voip.example.com&gt;;tag=28460006_c3356d0b_5cdada8c-cbf0-4369-b02d-cc97d3c36f2b
To: &lt;sip:+12195550714&#064;wa.meta.vc&gt;
Max-Forwards: 66
P-Asserted-Identity: &lt;sip:+12145551869&#064;meta-voip.example.com&gt;
Min-SE: 120
Call-ID: f304a1d2cafb8139c1f9ff93a7733586&#064;0.0.0.0
Contact: &quot;12145551869&quot; &lt;sip:+12145551869&#064;172.25.10.217:5060;transport=udp&gt;
Allow: INVITE, ACK, CANCEL, OPTIONS, BYE, REFER, NOTIFY
Via: SIP/2.0/TLS 54.172.60.1:5061;received=2803:6080:f934:8894:7eb5:24f9:300:0;branch=z9hG4bK1e5a.0da2ace9cc912d9e5f2595ca4acb9847.0
Via: SIP/2.0/UDP 172.25.10.217:5060;rport=5060;branch=z9hG4bK5cdada8c-cbf0-4369-b02d-cc97d3c36f2b_c3356d0b_54-457463274351249162
Supported: timer
User-Agent: Twilio Gateway
Proxy-Authorization: Digest username=&quot;12145551869&quot;, realm=&quot;wa.meta.vc&quot;, nonce=&quot;2a487cb01d4ed43b&quot;, uri=&quot;sip:+12195550714&#064;wa.meta.vc;transport=tls&quot;, response=&quot;3f58df7af575b948625aeffd51bf9060&quot;, algorithm=MD5, cnonce=&quot;b338deb7f0e004e66353e26d34ad62b7&quot;, opaque=&quot;725a06fb2cd89a32&quot;, qop=auth, nc=00000002
Content-Type: application/sdp
X-Twilio-CallSid: CA93eac6be615da5e6836c7059e9555348
Content-Length: 422
Content-Type: application/sdp
Content-Length:   422

v=0
o=root 1185414872 1185414872 IN IP4 172.18.155.180
s=Twilio Media Gateway
c=IN IP4 168.86.138.232
t=0 0
m=audio 19534 RTP/SAVP 107 0 8 101
a=crypto:**************************************************************************
a=rtpmap:0 PCMU/8000
a=rtpmap:107 opus/48000/2
a=fmtp:107 useinbandfec=1
a=rtpmap:8 PCMA/8000
a=rtpmap:101 telephone-event/8000
a=fmtp:101 0-16
a=ptime:20
a=maxptime:20
a=sendrecv
```

#### SIP OK (with SDES)

```
SIP/2.0 200 OK
Via: SIP/2.0/TLS 54.172.60.1:5061;received=2803:6080:f934:8894:7eb5:24f9:300:0;branch=z9hG4bK1e5a.0da2ace9cc912d9e5f2595ca4acb9847.0
Via: SIP/2.0/UDP 172.25.10.217:5060;rport=5060;branch=z9hG4bK5cdada8c-cbf0-4369-b02d-cc97d3c36f2b_c3356d0b_54-457463274351249162
Record-Route: &lt;sip:onevc-sip-proxy.fbinfra.net:8191;transport=tls;lr&gt;
Record-Route: &lt;sip:wa.meta.vc;transport=tls;lr&gt;
Record-Route: &lt;sip:54.172.60.1:5061;transport=tls;lr;r2=on&gt;
Record-Route: &lt;sip:54.172.60.1;lr;r2=on&gt;
Call-ID: f304a1d2cafb8139c1f9ff93a7733586&#064;0.0.0.0
From: &quot;12145551869&quot; &lt;sip:+12145551869&#064;meta-voip.example.com&gt;;tag=28460006_c3356d0b_5cdada8c-cbf0-4369-b02d-cc97d3c36f2b
To: &lt;sip:+12195550714&#064;wa.meta.vc&gt;;tag=0d185053-2615-46c7-8ff2-250bda494cf1
CSeq: 2 INVITE
Allow: INVITE, ACK, BYE, CANCEL, NOTIFY, OPTIONS
Supported: timer
X-FB-External-Domain: wa.meta.vc
Contact: &lt;sip:+12195550714&#064;wa.meta.vc;transport=tls;ob;X-FB-Sip-Smc-Tier=collaboration.sip_gateway.sip.prod&gt;;isfocus
Content-Type: application/sdp
Content-Length:   645

v=0
o=- 1746657286595 2 IN IP4 127.0.0.1
s=-
t=0 0
a=group:BUNDLE audio
a=msid-semantic: WMS 42da9643-cb50-4eca-95d3-ca41b3f1f4bb
m=audio 3480 RTP/SAVP 107 101
c=IN IP4 157.240.19.130
a=rtcp:9 IN IP4 0.0.0.0
a=mid:audio
a=sendrecv
a=msid:42da9643-cb50-4eca-95d3-ca41b3f1f4bb WhatsAppTrack1
a=rtcp-mux
a=crypto:**************************************************************************
a=rtpmap:107 opus/48000/2
a=fmtp:107 maxaveragebitrate=20000;maxplaybackrate=16000;minptime=20;sprop-maxcapturerate=16000;useinbandfec=1
a=rtpmap:101 telephone-event/8000
a=maxptime:20
a=ptime:20
a=ssrc:1238967757 cname:WhatsAppAudioStream1
```

Meta&#039;s 200 OK response also includes `x-wa-meta-user-id`, `x-wa-meta-parent-user-id`, and `x-wa-meta-username` headers identifying the WhatsApp user. When Meta sends a SIP BYE to terminate a call, it includes the same headers. See [Custom SIP headers](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/sip#custom-sip-headers) for details.

### User-initiated calls (with WebRTC media)

#### SIP INVITE
```
INVITE sip:+17015558857&#064;meta-voip.example.com;transport=tls SIP/2.0
Via: SIP/2.0/TLS [2803:6080:e888:51aa:d4a4:c5e0:300:0]:33819;rport=33819;received=2803:6080:e888:51aa:d4a4:c5e0:300:0;branch=z9hG4bKPjNvs.IZBnUa1W4l8oHPpk3SUMmcx3MMcE;alias
Max-Forwards: 70
From: &quot;12195550714&quot; &lt;sip:+12195550714&#064;wa.meta.vc&gt;;tag=bbf1ad6e-79bb-4d9c-8a2c-094168a10bea
To: &lt;sip:+17015558857&#064;meta-voip.example.com&gt;
Contact: &lt;sip:+12195550714&#064;wa.meta.vc;transport=tls;ob&gt;;isfocus
Call-ID: outgoing:wacid.HBgLMTIxOTU1NTA3MTQVAgASGCAzODg1NTE5NEU1NTBEMTc1RTFFQUY5NjNCQ0FCRkEzRhwYCzE3MDE1NTU4ODU3FQIAAA==
CSeq: 2824 INVITE
Route: &lt;sip:onevc-sip-proxy-dev.fbinfra.net:8191;transport=tls;lr&gt;
X-FB-External-Domain: wa.meta.vc
Allow: INVITE, ACK, BYE, CANCEL, NOTIFY, OPTIONS
User-Agent: Facebook SipGateway
Content-Type: application/sdp
Content-Length: 1028

v=0
o=- 1741113186367 2 IN IP4 127.0.0.1
s=-
t=0 0
a=group:BUNDLE audio
a=msid-semantic: WMS 632a909f-1060-4369-96a4-7bd03e291ee7
a=ice-lite
m=audio 3480 UDP/TLS/RTP/SAVPF 111 126
c=IN IP4 57.144.135.35
a=rtcp:9 IN IP4 0.0.0.0
a=candidate:1775469887 1 udp 2122260223 57.144.135.35 3480 typ host generation 0 network-cost 50
a=candidate:3355715111 1 udp 2122262783 2a03:2880:f343:131:face:b00c:0:699c 3480 typ host generation 0 network-cost 50
a=ice-ufrag:RmDDkfzkwbexPfbC
a=ice-pwd:*************************
a=fingerprint:********************************************************************************************************
a=setup:actpass
a=mid:audio
a=sendrecv
a=msid:632a909f-1060-4369-96a4-7bd03e291ee7 WhatsAppTrack1
a=rtcp-mux
a=rtpmap:111 opus/48000/2
a=rtcp-fb:111 transport-cc
a=fmtp:111 maxaveragebitrate=20000;maxplaybackrate=16000;minptime=20;sprop-maxcapturerate=16000;useinbandfec=1
a=rtpmap:126 telephone-event/8000
a=maxptime:20
a=ptime:20
a=ssrc:849255537 cname:WhatsAppAudioStream1
```

In this example, the calling WhatsApp user is identified by their phone number (`+12195550714`) in the `From:` and `Contact:` headers. If the user has a BSUID or parent BSUID, it may appear instead of the phone number.

Meta includes the following custom headers on this INVITE to identify the WhatsApp user:

* `x-wa-meta-user-id` — The user&#039;s BSUID.
* `x-wa-meta-parent-user-id` — The user&#039;s parent BSUID, if parent BSUIDs are enabled; otherwise omitted.
* `x-wa-meta-username` — The user&#039;s username, if they have adopted one; otherwise omitted.

#### SIP BYE
```
BYE sip:+5559800000693&#064;wa.meta.vc;transport=tls;ob SIP/2.0
Via: SIP/2.0/TLS 137.184.4.155:5061;received=2803:6080:c074:cac:10ed:4b05:400:0;i=8d2dc2
Via: SIP/2.0/TLS 143.198.136.243:35181;rport=38087;received=143.198.136.243
Route: &lt;sip:wa.meta.vc;transport=tls;lr&gt;
Route: &lt;sip:onevc-sip-proxy.fbinfra.net:8191;transport=tls;lr&gt;
Max-Forwards: 69
From: &lt;sip:+12145551869&#064;meta-voip.example.com&gt;;tag=NcKQ6mtDKSDQB
To: &quot;5559800000693&quot; &lt;sip:+5559800000693&#064;wa.meta.vc&gt;;tag=92a01092-ee78-4870-865f-bc176203a6bd
Call-ID: outgoing:wacid.HBgPMjAwNzU2OTA0ODY5OTY1FRIAEhggMjQ4QzUwOUQ1REQ0NDUwNENEQzRFMTgwRTNGQjAwNjEcGAsxMjE0NTU1MTg2ORUCAAA
CSeq: 98734935 BYE
User-Agent: SignalWire
Allow: INVITE, ACK, BYE, CANCEL, OPTIONS, MESSAGE, INFO, UPDATE, REGISTER, REFER, NOTIFY
Supported: timer, path, replaces
Reason: Q.850;cause=16;text=&quot;NORMAL_CLEARING&quot;
Content-Length: 0
X-Relay-Call-ID: b72c0c65-e319-41b3-afb7-19ebcca05d38
Content-Length:  0
```
#### SIP INVITE (with SDES)
```
INVITE sip:+12145551869&#064;meta-voip.example.com;transport=tls SIP/2.0
            Via: SIP/2.0/TLS [2803:6080:f948:9597::]:57363;rport;branch=z9hG4bKPj3a9f2ad89e4a3df61408aa84f7d9a63e;alias
            Record-Route: &lt;sip:wa.meta.vc;transport=tls;lr&gt;
            Record-Route: &lt;sip:onevc-sip-proxy.fbinfra.net:8191;transport=tls;lr&gt;
            Via: SIP/2.0/TLS [2803:6080:f948:9597:d33c:e00:400:0]:5061;branch=z9hG4bKPj3a9f2ad89e4a3df61408aa84f7d9a63e
            Via: SIP/2.0/TLS [2803:6080:f948:9597:1ac5:cdf8:300:0]:63057;rport=63057;received=2803:6080:f948:9597:1ac5:cdf8:300:0;branch=z9hG4bKPj-phic0sbns27DiP0OlrxRxgLtNg4mio7;alias
            Max-Forwards: 69
            From: &quot;12195550714&quot; &lt;sip:+12195550714&#064;wa.meta.vc&gt;;tag=8a0f7e65-6e9e-4801-bf92-85c3ef2485d9
            To: &lt;sip:+12145551869&#064;meta-voip.example.com&gt;
            Contact: &lt;sip:+12195550714&#064;wa.meta.vc;transport=tls;ob&gt;;isfocus
            Call-ID: outgoing:wacid.HBgLMTIxOTU1NTA3MTQVAgASGCA4QkY1MTJCQkNFNTgxMEVFRERFRTUzNTFERkE1MDU0MhwYCzEyMTQ1NTUxODY5FQIAAA
            CSeq: 31159 INVITE
            X-FB-External-Domain: wa.meta.vc
            Allow: INVITE, ACK, BYE, CANCEL, NOTIFY, OPTIONS
            User-Agent: Facebook SipGateway
            Content-Type: application/sdp
            Content-Length:   645

v=0
o=- 1746659966980 2 IN IP4 127.0.0.1
s=-
t=0 0
a=group:BUNDLE audio
a=msid-semantic: WMS 07092115-d151-427e-8722-26c70936b104
m=audio 3480 RTP/SAVP 111 126
c=IN IP4 157.240.19.130
a=rtcp:9 IN IP4 0.0.0.0
a=mid:audio
a=sendrecv
a=msid:07092115-d151-427e-8722-26c70936b104 WhatsAppTrack1
a=rtcp-mux
a=crypto:**************************************************************************
a=rtpmap:111 opus/48000/2
a=fmtp:111 maxaveragebitrate=20000;maxplaybackrate=16000;minptime=20;sprop-maxcapturerate=16000;useinbandfec=1
a=rtpmap:126 telephone-event/8000
a=maxptime:20
a=ptime:20
a=ssrc:1615009994 cname:WhatsAppAudioStream1
```

Meta&#039;s INVITE also includes `x-wa-meta-user-id`, `x-wa-meta-parent-user-id`, and `x-wa-meta-username` headers identifying the WhatsApp user. The user in the `From:` and `Contact:` headers may be identified by a BSUID or parent BSUID instead of a phone number.

#### SIP OK (with SDES)
```
SIP/2.0 200 OK
            CSeq: 31159 INVITE
            Call-ID: outgoing:wacid.HBgLMTIxOTU1NTA3MTQVAgASGCA4QkY1MTJCQkNFNTgxMEVFRERFRTUzNTFERkE1MDU0MhwYCzEyMTQ1NTUxODY5FQIAAA
            From: &quot;12195550714&quot; &lt;sip:+12195550714&#064;wa.meta.vc&gt;;tag=8a0f7e65-6e9e-4801-bf92-85c3ef2485d9
            To: &lt;sip:+12145551869&#064;meta-voip.example.com&gt;;tag=66596922_c3356d0b_fee164be-566a-4679-a80d-5bfdf1d0aa9e
            Via: SIP/2.0/TLS 157.240.229.209:5061;rport=51830;received=69.171.251.115;branch=z9hG4bKPj3a9f2ad89e4a3df61408aa84f7d9a63e;alias
            Via: SIP/2.0/TLS [2803:6080:f948:9597:d33c:e00:400:0]:5061;branch=z9hG4bKPj3a9f2ad89e4a3df61408aa84f7d9a63e
            Via: SIP/2.0/TLS [2803:6080:f948:9597:1ac5:cdf8:300:0]:63057;rport=63057;received=2803:6080:f948:9597:1ac5:cdf8:300:0;branch=z9hG4bKPj-phic0sbns27DiP0OlrxRxgLtNg4mio7;alias
            Record-Route: &lt;sip:54.172.60.1:5060;lr;r2=on;twnat=sip:69.171.251.115:51830&gt;
            Record-Route: &lt;sip:54.172.60.1:5061;transport=tls;lr;r2=on;twnat=sip:69.171.251.115:51830&gt;
            Record-Route: &lt;sip:wa.meta.vc;transport=tls;lr&gt;
            Record-Route: &lt;sip:onevc-sip-proxy.fbinfra.net:8191;transport=tls;lr&gt;
            Server: Twilio
            Contact: &lt;sip:+172.25.16.223:5060&gt;
            Allow: INVITE, ACK, CANCEL, OPTIONS, BYE, REFER, NOTIFY
            Content-Type: application/sdp
            X-Twilio-CallSid: CAb0d74508fe5fcdf6ec70ea3cf4e9b90b
            Content-Length: 446
            Content-Type: application/sdp
            Content-Length:   446

v=0
o=root 1353670385 1353670385 IN IP4 172.18.164.24
s=Twilio Media Gateway
c=IN IP4 168.86.138.176
t=0 0
m=audio 15822 RTP/SAVP 111 126
a=rtpmap:111 opus/48000/2
a=fmtp:111 maxplaybackrate=16000;sprop-maxcapturerate=16000;maxaveragebitrate=20000;useinbandfec=1
a=rtpmap:126 telephone-event/8000
a=fmtp:126 0-16
a=crypto:**************************************************************************
a=ptime:20
a=maxptime:20
a=sendrecv
```

### User-initiated calls with digest auth (with SDES media)
Meta SIP server supports digest auth for user-initiated calls. Your SIP server should respond with digest auth challenge,
and Meta will resend the SIP INVITE with challenge response. The username used for digest auth is the (normalized) business
phone number and the password is generated by Meta and retrievable using the [get Call settings endpoint](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/sip#include-sip-user-password).

#### First INVITE request from Meta
```
INVITE sip:+12145551869&#064;meta-voip.example.com;transport=tls SIP/2.0
Via: SIP/2.0/TLS [2803:6080:f948:9597::]:47237;rport;branch=z9hG4bKPj1e6c665db16b3ecacf32cadb4497fe77;alias
Record-Route: &lt;sip:wa.meta.vc;transport=tls;lr&gt;
Record-Route: &lt;sip:onevc-sip-proxy.fbinfra.net:8191;transport=tls;lr&gt;
Via: SIP/2.0/TLS [2803:6080:f948:9597:7253:922a:400:0]:5061;branch=z9hG4bKPj1e6c665db16b3ecacf32cadb4497fe77
Via: SIP/2.0/TLS [2803:6080:f8bc:9272:e488:9927:400:0]:58279;rport=58279;received=2803:6080:f8bc:9272:e488:9927:400:0;branch=z9hG4bKPjr33j97A1mx5J8HWHEy2zIgqZYCCIb4Fb;alias
Max-Forwards: 69
From: &quot;12195550714&quot; &lt;sip:+12195550714&#064;wa.meta.vc&gt;;tag=ece2da15-39e7-4983-ac65-e312f325d94a
To: &lt;sip:+12145551869&#064;meta-voip.example.com&gt;
Contact: &lt;sip:+12195550714&#064;wa.meta.vc;transport=tls;ob&gt;;isfocus
Call-ID: outgoing:wacid.HBgLMTIxOTU1NTA3MTQVAgASGCA2MUI2QUY0MDRCMTUyOTM4QkE5ODEwN0ZGQTAwODkxORwYCzEyMTQ1NTUxODY5FQIAFRoA
CSeq: 9989 INVITE
X-FB-External-Domain: wa.meta.vc
Allow: INVITE, ACK, BYE, CANCEL, NOTIFY, OPTIONS
User-Agent: Facebook SipGateway
Content-Type: application/sdp
Content-Length:   643

v=0
o=- 1750716867913 2 IN IP4 127.0.0.1
s=-
t=0 0
a=group:BUNDLE audio
a=msid-semantic: WMS 4e37b099-8aef-45d0-be4f-1cde2ca5a37d
m=audio 3480 RTP/SAVP 111 126
c=IN IP4 57.144.219.49
a=rtcp:9 IN IP4 0.0.0.0
a=mid:audio
a=sendrecv
a=msid:4e37b099-8aef-45d0-be4f-1cde2ca5a37d WhatsAppTrack1
a=rtcp-mux
a=crypto:**************************************************************************
a=rtpmap:111 opus/48000/2
a=fmtp:111 maxaveragebitrate=20000;maxplaybackrate=16000;minptime=20;sprop-maxcapturerate=16000;useinbandfec=1
a=rtpmap:126 telephone-event/8000
a=maxptime:20
a=ptime:20
a=ssrc:215879358 cname:WhatsAppAudioStream1
```

Meta&#039;s INVITE includes `x-wa-meta-user-id`, `x-wa-meta-parent-user-id`, and `x-wa-meta-username` headers identifying the WhatsApp user. The user in the `From:` and `Contact:` headers may be identified by a BSUID or parent BSUID instead of a phone number.

#### 407 Response from partner SIP server
```
SIP/2.0 407 Proxy Authentication required
CSeq: 9989 INVITE
Call-ID: outgoing:wacid.HBgLMTIxOTU1NTA3MTQVAgASGCA2MUI2QUY0MDRCMTUyOTM4QkE5ODEwN0ZGQTAwODkxORwYCzEyMTQ1NTUxODY5FQIAFRoA
From: &quot;12195550714&quot; &lt;sip:+12195550714&#064;wa.meta.vc&gt;;tag=ece2da15-39e7-4983-ac65-e312f325d94a
To: &lt;sip:+12145551869&#064;meta-voip.example.com&gt;;tag=45065608_c3356d0b_16001fd8-76d2-45f0-bb35-e0441d6dc4a2
Via: SIP/2.0/TLS 31.13.66.215:5061;rport=62080;received=69.171.251.112;branch=z9hG4bKPj1e6c665db16b3ecacf32cadb4497fe77;alias
Via: SIP/2.0/TLS [2803:6080:f948:9597:7253:922a:400:0]:5061;branch=z9hG4bKPj1e6c665db16b3ecacf32cadb4497fe77
Via: SIP/2.0/TLS [2803:6080:f8bc:9272:e488:9927:400:0]:58279;rport=58279;received=2803:6080:f8bc:9272:e488:9927:400:0;branch=z9hG4bKPjr33j97A1mx5J8HWHEy2zIgqZYCCIb4Fb;alias
Contact: &lt;sip:+172.25.58.54:5060&gt;
Proxy-Authenticate: Digest realm=&quot;sip.twilio.com&quot;,nonce=&quot;eyOam_8-l5FVugxsyxFRjnlxq9vy1TjQIMB3mBfJuAvB5gV4&quot;,opaque=&quot;4a6a068be2ca2032a57912b9a2a6adf7&quot;,qop=&quot;auth&quot;
Content-Length: 0
Content-Length:  0
```
#### Second INVITE with authorization from Meta
```
INVITE sip:+12145551869&#064;meta-voip.example.com;transport=tls SIP/2.0
Via: SIP/2.0/TLS 31.13.66.215:5061;rport;branch=z9hG4bKPj16be0694dc6763eb66de5ec5f262db03;alias
Record-Route: &lt;sip:wa.meta.vc;transport=tls;lr&gt;
Record-Route: &lt;sip:onevc-sip-proxy.fbinfra.net:8191;transport=tls;lr&gt;
Via: SIP/2.0/TLS [2803:6080:f948:9597:7253:922a:400:0]:5061;branch=z9hG4bKPj16be0694dc6763eb66de5ec5f262db03
Via: SIP/2.0/TLS [2803:6080:f8bc:9272:e488:9927:400:0]:58279;rport=58279;received=2803:6080:f8bc:9272:e488:9927:400:0;branch=z9hG4bKPjYp9LqI0D8zJ.wly5wyMyVaH9fUwIU921;alias
Max-Forwards: 69
From: &quot;12195550714&quot; &lt;sip:+12195550714&#064;wa.meta.vc&gt;;tag=ece2da15-39e7-4983-ac65-e312f325d94a
To: &lt;sip:+12145551869&#064;meta-voip.example.com&gt;
Contact: &lt;sip:+12195550714&#064;wa.meta.vc;transport=tls;ob&gt;;isfocus
Call-ID: outgoing:wacid.HBgLMTIxOTU1NTA3MTQVAgASGCA2MUI2QUY0MDRCMTUyOTM4QkE5ODEwN0ZGQTAwODkxORwYCzEyMTQ1NTUxODY5FQIAFRoA
CSeq: 9990 INVITE
X-FB-External-Domain: wa.meta.vc
Allow: INVITE, ACK, BYE, CANCEL, NOTIFY, OPTIONS
User-Agent: Facebook SipGateway
Proxy-Authorization: Digest username=&quot;12145551869&quot;, realm=&quot;sip.twilio.com&quot;, nonce=&quot;eyOam_8-l5FVugxsyxFRjnlxq9vy1TjQIMB3mBfJuAvB5gV4&quot;, uri=&quot;sip:+12145551869&#064;meta-voip.example.com&quot;, response=&quot;b28ed6b8bf1418e3c6eca05ef8c7a0b1&quot;, cnonce=&quot;TY2SszvYCKitUCBlVLpGiPKMQfmBbj&quot;, opaque=&quot;4a6a068be2ca2032a57912b9a2a6adf7&quot;, qop=auth, nc=00000001
Content-Type: application/sdp
Content-Length:   643

v=0
o=- 1750716867913 2 IN IP4 127.0.0.1
s=-
t=0 0
a=group:BUNDLE audio
a=msid-semantic: WMS 4e37b099-8aef-45d0-be4f-1cde2ca5a37d
m=audio 3480 RTP/SAVP 111 126
c=IN IP4 57.144.219.49
a=rtcp:9 IN IP4 0.0.0.0
a=mid:audio
a=sendrecv
a=msid:4e37b099-8aef-45d0-be4f-1cde2ca5a37d WhatsAppTrack1
a=rtcp-mux
a=crypto:**************************************************************************
a=rtpmap:111 opus/48000/2
a=fmtp:111 maxaveragebitrate=20000;maxplaybackrate=16000;minptime=20;sprop-maxcapturerate=16000;useinbandfec=1
a=rtpmap:126 telephone-event/8000
a=maxptime:20
a=ptime:20
a=ssrc:215879358 cname:WhatsAppAudioStream1
```
####  SIP OK from partner SIP server
```
SIP/2.0 200 OK
CSeq: 9990 INVITE
Call-ID: outgoing:wacid.HBgLMTIxOTU1NTA3MTQVAgASGCA2MUI2QUY0MDRCMTUyOTM4QkE5ODEwN0ZGQTAwODkxORwYCzEyMTQ1NTUxODY5FQIAFRoA
From: &quot;12195550714&quot; &lt;sip:+12195550714&#064;wa.meta.vc&gt;;tag=ece2da15-39e7-4983-ac65-e312f325d94a
To: &lt;sip:+12145551869&#064;meta-voip.example.com&gt;;tag=29360930_c3356d0b_4933dc58-f035-4597-b075-04b19e552329
Via: SIP/2.0/TLS 31.13.66.215:5061;rport=62080;received=69.171.251.112;branch=z9hG4bKPj16be0694dc6763eb66de5ec5f262db03;alias
Via: SIP/2.0/TLS [2803:6080:f948:9597:7253:922a:400:0]:5061;branch=z9hG4bKPj16be0694dc6763eb66de5ec5f262db03
Via: SIP/2.0/TLS [2803:6080:f8bc:9272:e488:9927:400:0]:58279;rport=58279;received=2803:6080:f8bc:9272:e488:9927:400:0;branch=z9hG4bKPjYp9LqI0D8zJ.wly5wyMyVaH9fUwIU921;alias
Record-Route: &lt;sip:54.172.60.0:5060;lr;r2=on;twnat=sip:69.171.251.112:62080&gt;
Record-Route: &lt;sip:54.172.60.0:5061;transport=tls;lr;r2=on;twnat=sip:69.171.251.112:62080&gt;
Record-Route: &lt;sip:wa.meta.vc;transport=tls;lr&gt;
Record-Route: &lt;sip:onevc-sip-proxy.fbinfra.net:8191;transport=tls;lr&gt;
Contact: &lt;sip:+172.25.43.84:5060&gt;
Allow: INVITE, ACK, CANCEL, OPTIONS, BYE, REFER, NOTIFY
Content-Type: application/sdp
X-Twilio-CallSid: CAd4d6e59a356c4d1b0ee85323b2d9dab5
Content-Length: 444
Content-Type: application/sdp
Content-Length:   444

v=0
o=root 477560318 477560318 IN IP4 172.18.156.61
s=Twilio Media Gateway
c=IN IP4 168.86.137.174
t=0 0
m=audio 12710 RTP/SAVP 111 126
a=rtpmap:111 opus/48000/2
a=fmtp:111 maxplaybackrate=16000;sprop-maxcapturerate=16000;maxaveragebitrate=20000;useinbandfec=1
a=rtpmap:126 telephone-event/8000
a=fmtp:126 0-16
a=crypto:**************************************************************************
a=ptime:20
a=maxptime:20
a=sendrecv
```

## Configure SDES for SRTP key exchange

The Secure Real-time Transport Protocol (SRTP) key exchange is a cryptographic protocol used to securely exchange encryption keys between two parties over an insecure communication channel.

You can configure SRTP key exchange to one of two options:

* DTLS (default) — Industry-standard encrypted key exchange. Recommended.
* SDES — Plain text key is included in the SDP which is sent over secure signaling protocol like SIP or Graph API. When SDES is used, there is no need for STUN, ICE, and DTLS which could help shorten the call setup time.

### Configure/update SRTP key exchange protocol

#### Request syntax

```html
POST /&lt;PHONE_NUMBER_ID&gt;/settings
```

#### Endpoint parameters

| Placeholder | Description | Sample Value |
| --- | --- | --- |
| `&lt;PHONE_NUMBER_ID&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | **Required**&lt;br&gt;&lt;br&gt;The business phone number for which you are updating Calling API settings.&lt;br&gt;&lt;br&gt;[Learn more about formatting phone numbers in Cloud API](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-phone-numbers/phone-numbers) | `+12784358810` |

#### Request body

```html
&#123;
  &quot;calling&quot;: &#123;
    &quot;status&quot;: &quot;ENABLED&quot;,
    &quot;call_icon_visibility&quot;: &quot;DEFAULT&quot;
  . . .
    &quot;srtp_key_exchange_protocol&quot;: &quot;DTLS (default) | SDES&quot;,
  . . .
  &#125;
. . .
&#125;
```

#### Body parameters

| Parameter | Description | Sample Value |
| --- | --- | --- |
| `srtp_key_exchange_protocol`&lt;br&gt;&lt;br&gt;_String_ | **Optional**&lt;br&gt;&lt;br&gt;Enable or disable use of SRTP key exchange protocol.&lt;br&gt;&lt;br&gt;Possible values are `SDES` and `DTLS`.&lt;br&gt;&lt;br&gt;Default is `DTLS`.&lt;br&gt;&lt;br&gt;Note: Meta still expects the business side to send the first SRTP packet for both user and business initiated calls. | `&quot;SDES&quot;` |

#### Success response

```html
&#123;
  &quot;success&quot;: true
&#125;
```

### Error response

[View Calling API Error Codes and Troubleshooting for more information](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/troubleshooting)

[View general Cloud API Error Codes here](https://developers.facebook.com/documentation/business-messaging/whatsapp/support/error-codes)

### Get SRTP key exchange protocol

#### Request syntax

```html
POST /&lt;PHONE_NUMBER_ID&gt;/settings
```

#### Endpoint parameters

| Placeholder | Description | Sample Value |
| --- | --- | --- |
| `&lt;PHONE_NUMBER_ID&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | **Required**&lt;br&gt;&lt;br&gt;The business phone number for which you are updating Calling API settings.&lt;br&gt;&lt;br&gt;[Learn more about formatting phone numbers in Cloud API](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-phone-numbers/phone-numbers) | `+12784358810` |

#### Response body

```html
&#123;
  &quot;calling&quot;: &#123;
    &quot;status&quot;: &quot;ENABLED&quot;,
    &quot;call_icon_visibility&quot;: &quot;DEFAULT&quot;
  . . .
    &quot;srtp_key_exchange_protocol&quot;: &quot;DTLS | SDES&quot;,
  . . .
  &#125;
. . .
&#125;
```

#### Response parameters

| Parameter | Description | Sample Value |
| --- | --- | --- |
| `srtp_key_exchange_protocol`&lt;br&gt;&lt;br&gt;_String_ | The type of SRTP key exchange protocol configured for the business phone number queried.&lt;br&gt;&lt;br&gt;Possible values are `SDES` and `DTLS`.&lt;br&gt;&lt;br&gt;Default is `DTLS`.&lt;br&gt;&lt;br&gt;**Note: If this field has not been explicitly set, it will not be returned.** | `&quot;SDES&quot;` |

#### Error response

[View Calling API Error Codes and Troubleshooting for more information](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/troubleshooting)

[View general Cloud API Error Codes here](https://developers.facebook.com/documentation/business-messaging/whatsapp/support/error-codes)

## IP addresses

The IP addresses used for SIP configuration are the same as those listed for the Webhooks in the [Cloud API Webhooks IP Addresses section](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/overview#ip-addresses).

This reference is solely to indicate the IP addresses to allow-list for SIP traffic. By default, when SIP is enabled, calling-related webhooks are not sent. You must explicitly enable [SIP call webhooks](#sip-call-webhooks) to receive call lifecycle events.

## Troubleshooting

For additional SIP-specific questions and answers, see the [SIP FAQ](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/faq#session-initiation-protocol--sip--faq), and for SIP-specific errors and solutions, see [SIP Errors](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/troubleshooting#sip-errors).
