# Troubleshoot WhatsApp Calling Errors - Reference Guide



## Standard error response

When you receive an error, in the majority of cases the error shape will look like this:

```https
&#123;
  &quot;error&quot;: &#123;
    &quot;message&quot;: &quot;&lt;Error Message&gt;&quot;,
    &quot;type&quot;: &quot;&lt;Exception Type&gt;&quot;,
    &quot;code&quot;: &lt;Exception Code&gt;,
    &quot;fbtrace_id&quot;: &quot;&lt;Trace ID&gt;&quot;
  &#125;
&#125;
```

Use the Calling API error codes list below to identify and resolve calling errors.

## Calling logs

The **Call Logs** tab in [WhatsApp Manager](https://business.facebook.com/latest/whatsapp_manager/phone_numbers) provides businesses and partners with a detailed, self-service view of call events to aid in call troubleshooting.

The tab displays a table of recent call logs for your business phone numbers. Each call has a log. Each log can have multiple events representing a Graph API request made by the business or a webhook sent by Meta. Each row represents a call, with different columns highlighted to provide information about each call log.

### How to view call logs

1. Navigate to [WhatsApp Manager &gt; Account tools &gt; Phone numbers](https://business.facebook.com/latest/whatsapp_manager/phone_numbers)
2. Select the desired phone number to view the call logs

### Call logs and events

| Field | Description |
| --- | --- |
| Timestamp | Timestamp of when the call occurred. |
| Call Direction | Outbound (Business-initiated) or Inbound (User-initiated). |
| Signaling | Signaling Protocol used to establish the call (SIP, GRAPH_API). |
| Call ID | WhatsApp call identifier.  &lt;br&gt; **Note: Please provide this ID when requesting support.** |
| Request ID | Identifier for the request that initiated the call. |
| Call Details | Additional information containing a log of events during the lifecycle of the call. |

## Calling error codes

| Code | Description | Possible Solutions | HTTP Status Code |
| --- | --- | --- | --- |
| `100`&lt;br&gt;&lt;br&gt;Invalid Parameter | The Graph API call you are making has an invalid parameter. | Exact error details will be listed in the `error_data` section of the response payload.&lt;br&gt;&lt;br&gt;If it&#039;s an SDP validation related error then the exact issue will be included in the details. | `400`&lt;br&gt;&lt;br&gt;Bad request |
| `613`&lt;br&gt;&lt;br&gt;Fetch Call Permission For Consumer Phone Number API Limit Hit | Limit reached for requests to the fetch call permission status API. | Try again later or reduce the frequency or amount of requests to the API the app is making. | `429`&lt;br&gt;&lt;br&gt;Too many requests |
| `131009` | Interactive Message type, `voice_call` not supported. Supported types [`button`, `list`] | Ensure the sender is in one of the [supported countries](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/faq#product-faq). | `400`&lt;br&gt;&lt;br&gt;Bad request |
| `131030` | Recipient phone number is not in allowed list.&lt;br&gt;&lt;br&gt;The error is thrown for both attempted calls and call permission requests.&lt;br&gt;&lt;br&gt;Only occurs when using public test numbers (PTNs). | Add a recipient phone number to recipient list and try again. | `400`&lt;br&gt;&lt;br&gt;Bad request |
| `138000`&lt;br&gt;&lt;br&gt;Calling not enabled | Calling APIs are not enabled for this phone number | Calling is not enabled on the business phone number.&lt;br&gt;&lt;br&gt;[Configure call settings to enable Calling API features](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/call-settings) | `401`&lt;br&gt;&lt;br&gt;Unauthorized |
| `138001`&lt;br&gt;&lt;br&gt;Receiver Uncallable | Receiver is unable to receive calls&lt;br&gt;&lt;br&gt;Reasons can include:&lt;br&gt;&lt;br&gt;* The recipient phone number is not a WhatsApp phone number.&lt;br&gt;* The recipient has not accepted our new Terms of Service and Privacy Policy.&lt;br&gt;* Recipient using an unsupported client. The currently supported clients are Android, iOS, SMB Android, and SMB iOS. | Confirm with the recipient that they agree to be contacted by you over WhatsApp and are using the latest version of WhatsApp. | `400`&lt;br&gt;&lt;br&gt;Bad Request |
| `138002`&lt;br&gt;&lt;br&gt;Concurrent Calls limit | Limit reached for maximum concurrent calls (1000) for the given number | Try again later or reduce the frequency or amount of API calls the app is making. | `429`&lt;br&gt;&lt;br&gt;Too many requests |
| `138003`&lt;br&gt;&lt;br&gt;Duplicate call | A call is already ongoing with the receiver | Try again later when the current call ends. | `400`&lt;br&gt;&lt;br&gt;Bad request |
| `138004`&lt;br&gt;&lt;br&gt;Connection error | Error while connecting the call | Try again later or investigate the connection params provided to the API. | `500`&lt;br&gt;&lt;br&gt;Server error |
| `138005`&lt;br&gt;&lt;br&gt;Call rate limit exceeded | Limit reached for maximum calls that can be initiated by the business phone number | Try again later or reduce the frequency or amount of API calls the app is making. | `429`&lt;br&gt;&lt;br&gt;Too many requests |
| `138006`&lt;br&gt;&lt;br&gt;No approved call permission found | No approved [call permission](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/user-call-permissions) from the recipient | Ensure a call permission has been accepted by the consumer. | `401`&lt;br&gt;&lt;br&gt;Unauthorized |
| `138007`&lt;br&gt;&lt;br&gt;Connect Timeout error | Call was unable to connect due to a timeout | Business did not apply the offer/answer SDP from Cloud API in time.&lt;br&gt;Connect API was not invoked with the answer SDP in time. | `500`&lt;br&gt;&lt;br&gt;Server error |
| `138009`&lt;br&gt;&lt;br&gt;Call Permission Request Limit Hit | Limit reached for call permission request sends for the given business and consumer pair | When a business sends more than the limit of call permission requests per time period, Call Permission Requests are rate limited. A connected call with a consumer will reset the limits. | `400`&lt;br&gt;&lt;br&gt;Bad request |
| `138012`&lt;br&gt;&lt;br&gt;Business Initiated Calls Limit Hit | Limit reached for maximum business initiated calls allowed in 24 hours. Currently 100 connected business initiated calls are allowed within 24 hours. | Exact error details will be listed in the `error_data` section of the response payload.&lt;br&gt;&lt;br&gt;Details will include a timestamp when the next call is allowed. | `400`&lt;br&gt;&lt;br&gt;Bad request |
| `138013`&lt;br&gt;&lt;br&gt;Business-initiated calling is not available | Business-initiated calling is not available for this business account phone number. | Confirm that Business Initiated calling is available for this business account phone number in the [availability section here](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling#availability). | `400`&lt;br&gt;&lt;br&gt;Bad request |
| `138014`&lt;br&gt;&lt;br&gt;Calling is temporarily disabled | Calling APIs for this WhatsApp Business Number have been temporarily disabled for having low quality. | Ensure that your outreach to users is valuable and not spam.&lt;br&gt;&lt;br&gt;Retry the action after the account restrictions are lifted. | `400`&lt;br&gt;&lt;br&gt;Bad request |
| `138015`&lt;br&gt;&lt;br&gt;Calling cannot be enabled | Calling APIs cannot be enabled for this phone number. | Check and make sure [messaging limit](https://developers.facebook.com/documentation/business-messaging/whatsapp/messaging-limits) on your phone number is 2000 or more | `400`&lt;br&gt;&lt;br&gt;Bad request |
| `138017`&lt;br&gt;&lt;br&gt;Call permission request can&#039;t be sent because a permanent permission already exists. | Call permission request cannot be sent because a permanent permission has already been approved by the user for this business account phone number. | No need to send call permission requests. | `400`&lt;br&gt;&lt;br&gt;Bad request |
| `138018`&lt;br&gt;&lt;br&gt;WhatsApp Business calling cannot be enabled because technical pre-requisites are not met. | See [prerequisites](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling#step-1--prerequisites) for more details | [Configure SIP](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/sip) or ensure there is at least one app subscribed to the WhatsApp Business account that also has subscription to the `calls` webhook field. | `400`&lt;br&gt;&lt;br&gt;Bad request |
| `138019`&lt;br&gt;&lt;br&gt;Call setup failed | The WhatsApp client failed to set up the call.&lt;br&gt;&lt;br&gt;Cloud API sends this error in the call terminate webhook. | Try again later. | `400`&lt;br&gt;&lt;br&gt;Bad request |
| `138020`&lt;br&gt;&lt;br&gt;Relay connection failed | The WhatsApp client failed to establish a connection with the relay server.&lt;br&gt;&lt;br&gt;Cloud API sends this error in the call terminate webhook. | Try again later. | `400`&lt;br&gt;&lt;br&gt;Bad request |
| `138021`&lt;br&gt;&lt;br&gt;Media receive timeout | The WhatsApp client terminated the call due to not receiving any media for a long time.&lt;br&gt;&lt;br&gt;Cloud API sends this error in the call terminate webhook. | Confirm that call media is being sent to the relay and try again. | `400`&lt;br&gt;&lt;br&gt;Bad request |
| `138022`&lt;br&gt;&lt;br&gt;Media transmit timeout | The WhatsApp client terminated the call due to not transmitting any media for a long time.&lt;br&gt;&lt;br&gt;Cloud API sends this error in the call terminate webhook. | Try again later. | `400`&lt;br&gt;&lt;br&gt;Bad request |
| `138023`&lt;br&gt;&lt;br&gt;Call accepted but terminated with no media signals | The call was accepted but terminated with no media connection signals.&lt;br&gt;&lt;br&gt;Cloud API could not determine if the call was connected.&lt;br&gt;&lt;br&gt;Cloud API sends this error in the call terminate webhook. | Try again later. | `400`&lt;br&gt;&lt;br&gt;Bad request |
| `131044` | An error webhook is sent on user initiated calls when there is no valid payment method attached | This is similar to the [&quot;Business eligibility payment issue&quot;](https://developers.facebook.com/documentation/business-messaging/whatsapp/support/error-codes) error in messaging. | `400`&lt;br&gt;&lt;br&gt;Bad request |
| `131055` | Method not allowed. Graph API calls are not allowed for SIP enabled numbers | Use [SIP](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/sip) for SIP enabled business numbers. | `400`&lt;br&gt;&lt;br&gt;Bad request |

## SIP errors

Also refer to [SIP specific FAQ](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/faq#session-initiation-protocol--sip--faq) for additional troubleshooting info.

### Business initiated calls

| SIP status and message | Description | Possible Solutions |
| --- | --- | --- |
| `400`&lt;br&gt;&lt;br&gt;Asset not found, invalid business phone number | The phone number in the `From` header of the INVITE is invalid and does not correspond to a registered account on WhatsApp Business Platform | Check the phone number and resend the INVITE with the correct `From` header value. See [examples](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/sip#business-initiated-calls--with-webrtc-media-) |
| `403`&lt;br&gt;&lt;br&gt;SIP server `foo.com` from INVITE does not match any SIP server configured for phone number id [ID] | Your SIP INVITE to Meta has &#039;foo.com&#039; in the `From` header (for example, `15555551234&#064;foo.com`). But there is no SIP server with that hostname [configured](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/sip#configure-update-sip-settings-on-business-phone-number) on the business phone number. | Check your [SIP configuration](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/sip#get-phone-number-calling-settings--sip-) and ensure it matches the domain used in the `From` header. The `hostname` in the [SIP configuration](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/sip#get-phone-number-calling-settings--sip-) should match the domain in the `From` header of your SIP INVITE. It is also valid for the SIP configuration `hostname` to be a sub-domain of the domain in `From` header. For example, SIP server configured is `bar.foo.com` and the From header is `15555551234&#064;foo.com`. |
| `403`&lt;br&gt;&lt;br&gt;No Approved Call Permission Found: No approved call permission from the recipient | There is no WhatsApp user with specified number or user has not accepted our new Terms of Service or there is no permission from user to call the business. Meta can&#039;t reveal if a phone number is on WhatsApp with certainty, due to privacy reasons. So this is the same error you&#039;ll receive for multiple reasons. | Double check the user phone number and make sure you obtain the user permission. See [Obtain User Call Permissions&lt;br&gt;](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/user-call-permissions) for more information |
| `403`&lt;br&gt;&lt;br&gt;The app [APP_ID] configured for SIP server `example.com` is not authorized for phone number id [ID] | When you configure SIP server on a business phone, Meta extracts the app id from the access token and stores a mapping from the SIP server to that app id. When you send a SIP INVITE, Meta consults that mapping to know which app is making the SIP request. This error means that app does not have `whatsapp_business_messaging` permission on the business phone. | Check that you are using the right app and make sure it has permissions on the business phone. If you&#039;re able to [send message](https://developers.facebook.com/documentation/business-messaging/whatsapp/messages/send-messages#requests) using the app&#039;s access token, that means the app has the right permission. Otherwise you may need to delete and add your SIP server with the correct app and retry your SIP INVITE. See [Configuring SIP settings](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/sip#configure-update-sip-settings-on-business-phone-number). |
| `403`&lt;br&gt;&lt;br&gt;Business Initiated Connected Call Per Day Limit Hit | Limit reached for maximum business initiated calls allowed in 24 hours. Currently 5 connected business initiated calls are allowed within 24 hours. | See more info about the [limits](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/user-call-permissions#limits--per-business---whatsapp-user-pair-) and adjust your calling rate accordingly |
| `403`&lt;br&gt;&lt;br&gt;Additional reasons for 403 include&lt;br&gt;&lt;br&gt;* Duplicate call because there can be only one active between a given business and user account&lt;br&gt;* Invalid digest auth response&lt;br&gt;* Business eligibility payment issue&lt;br&gt;* SDP Validation Error | Read each error for details | Consult earlier sections in this page like [Calling error codes](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/troubleshooting#calling-error-codes) |
| `404`&lt;br&gt;&lt;br&gt;Not found | SIP INVITEs using IP in request URI are not allowed | Use the correct request URI. See [examples](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/sip#business-initiated-calls--with-webrtc-media-) |
| `407`&lt;br&gt;&lt;br&gt;Proxy Authentication Required | Meta mandates digest authentication for your SIP INVITEs and this is the standard challenge response from Meta | Resend the SIP INVITE with digest response. See [examples](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/sip#business-initiated-calls--with-webrtc-media-) |
| `408`&lt;br&gt;&lt;br&gt;RTP Timeout | The WhatsApp client terminated the call due to not receiving any media for a long time. | See [Media related issues](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/troubleshooting#media-related-issues) |
| `480`&lt;br&gt;&lt;br&gt;Temporarily Unavailable | WhatsApp user is not reachable or did not answer the call | Try messaging or calling later. Unanswered calls impact call permissions. [Learn more](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/user-call-permissions#call-permission-request-basics) |
| `486`&lt;br&gt;&lt;br&gt;User declined the call | WhatsApp user declined your call | Try messaging or calling later. Rejected calls impact call permissions. [Learn more](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/user-call-permissions#call-permission-request-basics) |
| `487`&lt;br&gt;&lt;br&gt;Request Terminated | Business canceled their SIP INVITE using SIP CANCEL | Expected return for your SIP INVITE when you CANCEL the INVITE before Meta response to your INVITE |
| `503`&lt;br&gt;&lt;br&gt;Service Unavailable | Generic internal error. | Retry your request after some time or consult Meta support |

## Media related issues

| Issue | Description | Possible Solutions |
| --- | --- | --- |
| Call disconnects after 20 seconds into the call | At the start of the call, if there is no media flowing from business to WhatsApp user for 20 seconds, WhatsApp client will disconnect the call due to no media | * Check your media server to make sure it&#039;s initiating the media session and sending media packets&lt;br&gt;* Check your firewall to rule out any packet drops at your edge&lt;br&gt;* Capture network traffic using pcap or a similar tool and attach it to the support ticket for further troubleshooting by Meta |
| Can&#039;t hear audio and call disconnects after 30 seconds | After the call is connected and established, if there is no media packet from business to WhatsApp user for 30 seconds, WhatsApp client will disconnect the call due to no media reception | * Make sure you&#039;re at least sending RTCP packets even when there is silence or when you are waiting for user input (for example, IVR)&lt;br&gt;* Check your media server to understand why it stops sending media packets to Meta&lt;br&gt;* Capture network traffic using pcap or a similar tool and attach it to the support ticket for further troubleshooting by Meta |

## Audio clipping issue and solution

When bridging the WhatsApp Consumer media leg via WebRTC to another media leg, such as [SIP](https://datatracker.ietf.org/doc/html/rfc3261), it&#039;s important to be aware of potential audio clipping issues and how to prevent them. A common symptom of this issue is that the WhatsApp consumer may miss approximately one second of audio from the business. For instance, if an IVR system is playing a sequence like &quot;1-2-3,&quot; the consumer might only hear &quot;2&quot; or &quot;3.&quot; The extent of audio loss varies based on the specific integration and the severity of the issue.

The sequence diagram below illustrates this problem using an example where the WebRTC leg from the Cloud API is bridged with a SIP media leg. Depending on the SIP vendor and implementation, the media from the SIP User Agent might start playing at step 11, but it won&#039;t reach the WhatsApp consumer until step 18.

### Sequence diagram

(_Right click image and choose &quot;Open in new tab&quot; for enlarged image_)


#### Components of the sequence diagram

* _WAConsumer_ is a WhatsApp user calling the business phone number using WhatsApp mobile app
* _Meta_ is the Cloud API product
* _MetaWebrtcEndpoint_ is the WebRTC agent on the Meta infrastructure
* _BizIntegration_ is the webhook server receiving calls related webhooks and the app server with business logic to invoke Cloud API Graph APIs
* _BizWebrtcEndpoint_ is the WebRTC termination point as well as the SIP UAC typically in a media server on your side.
* _BizSipEndpoint_ is the SIP User Agent (UA) often representing IVR or Business Agent.

#### Key points to note

1. Upon receiving the connect webhook (step 5 above), initialize your WebRTC agent, prepare the SDP answer, and make the Pre-accept Graph API call with the SDP answer. This pre-establishes the WebRTC connection in anticipation of an &#039;accept/ok&#039; response from the SIP User Agent (UA). In most cases, the consumer&#039;s call is answered, such as by an automated recording or IVR system.
1. At step 11 above, wait for the ICE process to complete the creation of valid lists, indicating the near completion of ICE connection establishment (RFC reference).
1. If the SIP UA rejects the call instead of returning a 200 OK, use the terminate Graph API to end the call.
1. If the consumer connection is ready before the SIP connection, the consumer may experience a few milliseconds of silence, which is preferable to losing some initial audio from the business.

### Root cause

When bridging two media legs, it&#039;s crucial for the media server to synchronize their readiness to prevent audio clipping. In this scenario, the BizWebrtcEndpoint allows the SIP User Agent (UA) to transmit media significantly earlier than the WebRTC leg is prepared to receive it. As a result, the media server drops packets, leading to audio clipping issues.

### Suggested solutions

Below is a high-level summary of alternative approaches to address audio clipping. Each option has its drawbacks, so assess their feasibility in the context of your specific requirements.

* **Use SDES**: [Configure SDES](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/sip#configure-sdes-for-srtp-key-exchange-protocol) on your business number instead of DTLS. A common reason for delayed setup of the media leg between your infrastructure and Meta is DTLS handshake completion. Meta cannot complete DTLS handshake until it receives your SDP. You may not be able share your SDP with Meta until your internal endpoint responds to your with their SDP. Common DTLS implementations have a retry interval of 1s, 2s, 4s, and so on. After you send Meta the SDP, there is often a ~3 second delay before Meta receives your DTLS client hello packet and this is when your internal endpoint is sending media but it is dropped. When you switch to SDES you can directly send SRTP after you send Meta your SDP.
* **Delayed Audio Playback:** Instruct the SIP User Agent (UA) to wait for an ACK from BizWebrtcEndpoint before playing audio. The ACK would be sent only after receiving a successful response from the Accept API, followed by an artificial delay. This approach ensures that the WebRTC connection is established before audio playback begins.
* **Connection State-Based Delay:** Direct the SIP UA to wait until the WebRTC connection state is &#039;connected&#039; before playing audio. This method relies on the WebRTC connection being fully established before audio playback starts.
* **Buffered Media Packets:** Buffer SIP media packets and send them only after the WebRTC connection is established. This approach ensures that no audio packets are lost due to premature playback.
* **Silence Insertion:** Insert a brief period of silence into the IVR audio before the actual audio content. This method allows the WebRTC connection to establish itself while the IVR is playing silence, reducing the likelihood of audio clipping.
* **Pre-accept**: According to the [RFC recommendation](https://datatracker.ietf.org/doc/html/rfc8839#name-offer-in-invite), a WebRTC agent, such as BizWebrtcEndpoint, should not transmit media until the ICE process is nearly complete. In our context, this means adopting an &#039;optimistic accept&#039; strategy by invoking the [pre-accept API call](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/reference#pre-accept-call) even before sending the SIP INVITE. According to SIP RFCs, a User Agent Client (UAC) should be prepared to receive media immediately after sending the INVITE. Therefore, it&#039;s advisable to initiate the WebRTC connection setup process prior to sending the SIP INVITE.

These solutions may have varying degrees of complexity and impact on your system. It&#039;s essential to evaluate their feasibility and potential trade-offs in the context of your specific use case.

## Support

For support concerning WhatsApp Business Calling API, choose the **WaBiz: Calling API** topic when opening a [Direct Support](https://business.facebook.com/direct-support/) ticket.
