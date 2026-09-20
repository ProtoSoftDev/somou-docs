# User-initiated calls



## Overview

The Calling API supports receiving calls made by WhatsApp users to your business.

Your business dictates when calls can be received by [configuring business calling hours and holiday unavailability](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/call-settings#parameter-details).
**Warning:** **Consumer device eligibility**

Currently, the WhatsApp Business Calling API can accept calls from a consumer&#039;s primary and companion iPhone or Android phones.

A **primary device** is the consumer&#039;s main device, typically a mobile phone, which holds the authoritative state for the user&#039;s account. It has full access to messaging history and core functionalities. There is exactly one primary device per user account at any given time.

**Companion devices** are additional devices registered to the user&#039;s account that can operate alongside the primary device. Examples include web clients, desktop apps, tablets, and smart glasses. Companion devices have access to some or all messaging history and core features but are limited compared to the primary device. For Cloud API Calling, **only iPhone and Android phone companion devices are supported for user-initiated calls**.

**Callback permission functionality on companion devices**

For businesses that have the [callback setting](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/call-settings#configure-update-business-phone-number-calling-settings) enabled, this functionality is not supported on companion devices yet.

## Prerequisites

Before you get started with user-initiated calling, ensure that:

* [Subscribe](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/create-webhook-endpoint#configure-webhooks) to the **calls** webhook field
* [Enable Calling API features on your business phone number](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/call-settings)

### Call sequence diagram

## User-initiated calling flow

### Part 1: A WhatsApp user calls your business from their client app

When a WhatsApp user calls your business, a Call Connect webhook will be triggered with an `SDP Offer`:

```https
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;366634483210360&quot;, // WhatsApp Business Account ID associated with the business phone number
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;messaging_product&quot;: &quot;whatsapp&quot;,
            &quot;metadata&quot;: &#123; // ID and display number for the business phone number placing the call (caller)
              &quot;phone_number_id&quot;: &quot;436666719526789&quot;,
              &quot;display_phone_number&quot;: &quot;13175551399&quot;,
            &#125;,
            &quot;contacts&quot;: [
              &#123;
                &quot;profile&quot;: &#123;
                  &quot;name&quot;: &quot;&lt;USER_DISPLAY_NAME&gt;&quot;,
                  &quot;username&quot;: &quot;&lt;USERNAME&gt;&quot;
                &#125;,
                &quot;wa_id&quot;: &quot;&lt;USER_PHONE_NUMBER&gt;&quot;,
                &quot;user_id&quot;: &quot;&lt;BSUID&gt;&quot;,
                &quot;parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot;
              &#125;
            ],
            &quot;calls&quot;: [
              &#123;
                &quot;id&quot;: &quot;wacid.ABGGFjFVU2AfAgo6V-Hc5eCgK5Gh&quot;, // The WhatsApp call ID
                &quot;to&quot;: &quot;16315553601&quot;, // The WhatsApp user&#039;s phone number (callee)
                &quot;from&quot;: &quot;13175551399&quot;,
                &quot;from_user_id&quot;: &quot;&lt;BSUID&gt;&quot;,
                &quot;from_parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot;,
                &quot;event&quot;: &quot;connect&quot;,
                &quot;timestamp&quot;: &quot;1671644824&quot;,
                &quot;session&quot;: &#123;
                  &quot;sdp_type&quot;: &quot;offer&quot;,
                  &quot;sdp&quot;: &quot;&lt;&lt;RFC 8866 SDP&gt;&gt;&quot;
                &#125;
              &#125;
            ]
          &#125;,
          &quot;field&quot;: &quot;calls&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

**Note:** **Usernames and business-scoped user IDs:** The Call Connect webhook may include `from_user_id`, `from_parent_user_id`, and contact-level `user_id`, `parent_user_id`, and `username` fields, and the user&#039;s phone number may be omitted. For details, see [Business-scoped user IDs](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-scoped-user-ids#business-scoped-user-id).

### Part 2: Your business pre-accepts the call (recommended)

When you pre-accept an inbound call, you allow the calling media connection to be established before attempting to send call media through the connection.

Pre-accepting calls is recommended because it facilitates faster connection times and avoids [audio clipping issues](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/troubleshooting#audio-clipping-issue-and-solution).

To pre-accept, use the [Calls API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/calling-api) with the `call_id` from the previous webhook, an `action` of `pre-accept`, and an `SDP Answer`:

```https
POST &lt;PHONE_NUMBER_ID&gt;/calls
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;call_id&quot;: &quot;wacid.ABGGFjFVU2AfAgo6V-Hc5eCgK5Gh&quot;,
  &quot;action&quot;: &quot;pre_accept&quot;,
  &quot;session&quot;: &#123;
     &quot;sdp_type&quot;: &quot;answer&quot;
     &quot;sdp&quot;: &quot;&lt;&lt;RFC 8866 SDP&gt;&gt;&quot;
  &#125;
&#125;
```

If there are no errors, you&#039;ll receive a success response:

```https
&#123;
  &quot;success&quot; : true
&#125;
```

### Part 3: Your business accepts the call after the WebRTC connection is made

Once the WebRTC connection is made on your end, you can accept the call.

Once you accept the call, wait until you receive a `200 OK` back from the endpoint. Media will begin flowing immediately since the connection was established prior to call connect.

Use the [Calls API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/calling-api) with the following request body to accept the call:

```https
POST &lt;PHONE_NUMBER_ID&gt;/calls
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;call_id&quot;: &quot;wacid.ABGGFjFVU2AfAgo6V-Hc5eCgK5Gh&quot;,
  &quot;action&quot;: &quot;accept&quot;,
  &quot;session&quot; : &#123;
      &quot;sdp_type&quot; : &quot;answer&quot;,
      &quot;sdp&quot; : &quot;&lt;&lt;RFC 8866 SDP&gt;&gt;&quot;
   &#125;,
&#125;
```

### Part 4: Your business or the WhatsApp user terminates the call

Either the business or the WhatsApp user can terminate the call at any time.

Use the [Calls API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/calling-api) with the following request body to terminate the call:

```https
POST &lt;PHONE_NUMBER_ID&gt;/calls
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;call_id&quot;: &quot;wacid.ABGGFjFVU2AfAgo6V-Hc5eCgK5Gh&quot;,
  &quot;action&quot; : &quot;terminate&quot;
&#125;
```

If there are no errors, you&#039;ll receive a success response:

```https
&#123;
  &quot;success&quot; : true
&#125;
```

When either the business or the WhatsApp user terminates the call, you receive a Call Terminate webhook:

```https
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;366634483210360&quot;, // WhatsApp Business Account ID associated with the business phone number
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;messaging_product&quot;: &quot;whatsapp&quot;,
            &quot;metadata&quot;: &#123; // ID and display number for the business phone number placing the call (caller)
              &quot;phone_number_id&quot;: &quot;436666719526789&quot;
              &quot;display_phone_number&quot;: &quot;13175551399&quot;,

            &#125;,
            &quot;contacts&quot;: [
              &#123;
                &quot;profile&quot;: &#123;
                  &quot;name&quot;: &quot;&lt;USER_DISPLAY_NAME&gt;&quot;,
                  &quot;username&quot;: &quot;&lt;USERNAME&gt;&quot;
                &#125;,
                &quot;wa_id&quot;: &quot;&lt;USER_PHONE_NUMBER&gt;&quot;,
                &quot;user_id&quot;: &quot;&lt;BSUID&gt;&quot;,
                &quot;parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot;
              &#125;
            ],
            &quot;calls&quot;: [
              &#123;
                &quot;id&quot;: &quot;wacid.ABGGFjFVU2AfAgo6V-Hc5eCgK5Gh&quot;,
                &quot;to&quot;: &quot;16315553601&quot;, // The WhatsApp user&#039;s phone number (callee)
                &quot;from&quot;: &quot;13175551399&quot;, // The business phone number placing the call (caller)
                &quot;from_user_id&quot;: &quot;&lt;BSUID&gt;&quot;,
                &quot;from_parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot;,
                &quot;event&quot;: &quot;terminate&quot;,
                &quot;direction&quot;: &quot;USER_INITIATED&quot;,
                &quot;timestamp&quot;: &quot;1749197480&quot;,
                &quot;status&quot;: [&quot;Failed&quot;, &quot;Completed&quot;],
                &quot;start_time&quot;: &quot;1671644824&quot;, // Call start UNIX timestamp
                &quot;end_time&quot;: &quot;1671644944&quot;, // Call end UNIX timestamp
                &quot;duration&quot;: 480 // Call duration in seconds
              &#125;
            ]
          &#125;,
          &quot;field&quot;: &quot;calls&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

**Note:** **Usernames and business-scoped user IDs:** The Call Terminate webhook may include `from_user_id`, `from_parent_user_id`, and contact-level `user_id`, `parent_user_id`, and `username` fields, and the user&#039;s phone number may be omitted. For details, see [Business-scoped user IDs](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-scoped-user-ids#business-scoped-user-id).

## Endpoints for user-initiated calling

### Pre-accept call

When you pre-accept an inbound call, you allow the calling media connection to be established before attempting to send call media through the connection.

When you then call the accept call endpoint, media begins flowing immediately since the connection has already been established.

Pre-accepting calls is recommended because it facilitates faster connection times and avoids [audio clipping issues](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/troubleshooting#audio-clipping-issue-and-solution).

There is about 30 to 60 seconds after the [Call Connect webhook](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/user-initiated-calls#call-connect-webhook) is sent for the business to accept the phone call. If the business does not respond, the call is terminated on the WhatsApp user side with a &quot;Not Answered&quot; notification and a [Terminate Webhook](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/user-initiated-calls#call-terminate-webhook) is delivered back to you.

**Warning:** **Note:** Since the WebRTC connection is established before calling the [Accept Call endpoint](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/user-initiated-calls#accept-call), make sure to flow the call media only after you receive a 200 OK response back.

If call media flows too early, the caller will miss the first few words of the call. If call media flows too late, callers will hear silence.

#### Request syntax

```https
POST &lt;PHONE_NUMBER_ID&gt;/calls
```

| Placeholder | Description | Sample Value |
| --- | --- | --- |
| `&lt;PHONE_NUMBER_ID&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | **Required**&lt;br&gt;&lt;br&gt;The business phone number which you are using Calling API features from.&lt;br&gt;&lt;br&gt;[Learn more about formatting phone numbers in Cloud API](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-phone-numbers/phone-numbers) | `+12784358810` |

#### Request body

```https
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;call_id&quot;: &quot;wacid.ABGGFjFVU2AfAgo6V-Hc5eCgK5Gh&quot;,
  &quot;action&quot;: &quot;pre_accept&quot;,
  &quot;session&quot; : &#123;
      &quot;sdp_type&quot; : &quot;answer&quot;,
      &quot;sdp&quot; : &quot;&lt;&lt;RFC 8866 SDP&gt;&gt;&quot;
   &#125;
&#125;
```

#### Body parameters

| Parameter | Description | Sample Value |
| --- | --- | --- |
| `call_id`&lt;br&gt;&lt;br&gt;_String_ | **Required**&lt;br&gt;&lt;br&gt;The ID of the phone call.&lt;br&gt;&lt;br&gt;For inbound calls, you receive a call ID from the [Call Connect webhook](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/user-initiated-calls#call-connect-webhook) when a WhatsApp user initiates the call. | `&quot;wacid.ABGGFjFVU2AfAgo6V-Hc5eCgK5Gh&quot;` |
| `action`&lt;br&gt;&lt;br&gt;_String_ | **Optional**&lt;br&gt;&lt;br&gt;The action being taken on the given call ID.&lt;br&gt;&lt;br&gt;Values can be `connect` \| `pre_accept` \| `accept` \| `reject` \| `terminate` | `&quot;pre_accept&quot;` |
| `session`&lt;br&gt;&lt;br&gt;_JSON object_ | **Optional**&lt;br&gt;&lt;br&gt;Contains the session description protocol (SDP) type and description language.&lt;br&gt;&lt;br&gt;Requires two values:&lt;br&gt;&lt;br&gt;`sdp_type` — (_String_) **Required**&lt;br&gt;&lt;br&gt;&quot;offer&quot;, to indicate SDP offer&lt;br&gt;&lt;br&gt;`sdp` — (_String_) **Required**&lt;br&gt;&lt;br&gt;The SDP info of the device on the other end of the call. The SDP must be compliant with [RFC 8866](https://datatracker.ietf.org/doc/html/rfc8866).&lt;br&gt;&lt;br&gt;[Learn more about Session Description Protocol (SDP)](https://www.rfc-editor.org/rfc/rfc8866.html)&lt;br&gt;&lt;br&gt;[View example SDP structures](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/reference#sdp-overview-and-sample-sdp-structures) | ```https
&quot;session&quot; :
&#123;
&quot;sdp_type&quot; : &quot;offer&quot;,
&quot;sdp&quot; : &quot;&lt;&lt;RFC 8866 SDP&gt;&gt;&quot;
&#125;
``` |

#### Success response

```https
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;success&quot; : true
&#125;
```

#### Error response

Possible errors that can occur:

* Invalid `call-id`
* Invalid `phone-number-id`
* Error related to your payment method
* Invalid Connection info, for example, SDP, or ICE
* Accept/Reject an already In Progress/Completed/Failed call
* Permissions/Authorization errors

[View Calling API Error Codes and Troubleshooting for more information](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/troubleshooting)

[View general Cloud API Error Codes here](https://developers.facebook.com/documentation/business-messaging/whatsapp/support/error-codes)

### Accept call

Use this endpoint to connect to a call by providing a call agent&#039;s SDP.

You have about 30 to 60 seconds after the [Call Connect Webhook](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/user-initiated-calls#call-connect-webhook) is sent to accept the phone call. If your business does not respond, the call is terminated on the WhatsApp user side with a &quot;Not Answered&quot; notification and a [Terminate Webhook](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/user-initiated-calls#call-terminate-webhook) is delivered back to you.

#### Request syntax

```https
POST &lt;PHONE_NUMBER_ID&gt;/calls
```

| Placeholder | Description | Sample Value |
| --- | --- | --- |
| `&lt;PHONE_NUMBER_ID&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | **Required**&lt;br&gt;&lt;br&gt;The business phone number which you are using Calling API features from.&lt;br&gt;&lt;br&gt;[Learn more about formatting phone numbers in Cloud API](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-phone-numbers/phone-numbers) | `+12784358810` |

#### Request body

```https
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;call_id&quot;: &quot;wacid.ABGGFjFVU2AfAgo6V-Hc5eCgK5Gh&quot;,
  &quot;action&quot;: &quot;accept&quot;,
  &quot;session&quot; : &#123;
      &quot;sdp_type&quot; : &quot;answer&quot;,
      &quot;sdp&quot; : &quot;&lt;&lt;RFC 8866 SDP&gt;&gt;&quot;
   &#125;,
   &quot;biz_opaque_callback_data&quot;: &quot;random_string&quot;
&#125;
```

#### Body parameters

| Parameter | Description | Sample Value |
| --- | --- | --- |
| `call_id`&lt;br&gt;&lt;br&gt;_String_ | **Required**&lt;br&gt;&lt;br&gt;The ID of the phone call.&lt;br&gt;&lt;br&gt;For inbound calls, you receive a call ID from the [Call Connect webhook](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/user-initiated-calls#call-connect-webhook) when a WhatsApp user initiates the call. | `&quot;wacid.ABGGFjFVU2AfAgo6V-Hc5eCgK5Gh&quot;` |
| `action`&lt;br&gt;&lt;br&gt;_String_ | **Optional**&lt;br&gt;&lt;br&gt;The action being taken on the given call ID.&lt;br&gt;&lt;br&gt;Values can be `connect` \| `pre_accept` \| `accept` \| `reject` \| `terminate` | `&quot;accept&quot;` |
| `session`&lt;br&gt;&lt;br&gt;_JSON object_ | **Optional**&lt;br&gt;&lt;br&gt;Contains the session description protocol (SDP) type and description language.&lt;br&gt;&lt;br&gt;Requires two values:&lt;br&gt;&lt;br&gt;`sdp_type` — (_String_) **Required**&lt;br&gt;&lt;br&gt;&quot;offer&quot;, to indicate SDP offer&lt;br&gt;&lt;br&gt;`sdp` — (_String_) **Required**&lt;br&gt;&lt;br&gt;The SDP info of the device on the other end of the call. The SDP must be compliant with [RFC 8866](https://datatracker.ietf.org/doc/html/rfc8866).&lt;br&gt;&lt;br&gt;[Learn more about Session Description Protocol (SDP)](https://www.rfc-editor.org/rfc/rfc8866.html)&lt;br&gt;&lt;br&gt;[View example SDP structures](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/reference#sdp-overview-and-sample-sdp-structures) | ```https
&quot;session&quot; :
&#123;
&quot;sdp_type&quot; : &quot;offer&quot;,
&quot;sdp&quot; : &quot;&lt;&lt;RFC 8866 SDP&gt;&gt;&quot;
&#125;
``` |
| `biz_opaque_callback_data`&lt;br&gt;&lt;br&gt;_String_ | **Optional**&lt;br&gt;&lt;br&gt;An arbitrary string you can pass in that is useful for tracking and logging purposes.&lt;br&gt;&lt;br&gt;Any app subscribed to the &quot;calls&quot; webhook field on your WhatsApp Business account can receive this string, as it is included in the `calls` object within the subsequent [Terminate webhook](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/user-initiated-calls#call-terminate-webhook) payload.&lt;br&gt;&lt;br&gt;Cloud API does not process this field, it just returns it as part of the [Terminate webhook](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/user-initiated-calls#call-terminate-webhook).&lt;br&gt;&lt;br&gt;Maximum 512 characters | `&quot;8huas8d80nn&quot;` |

#### Success response

```https
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;success&quot; : true
&#125;
```

#### Error response

Possible errors that can occur:

* Invalid `call-id`
* Invalid `phone-number-id`
* Error related to your payment method
* Invalid Connection info, for example, SDP, or ICE
* Accept/Reject an already In Progress/Completed/Failed call
* Permissions/Authorization errors
* SDP answer provided in accept does not match the SDP answer given in the [Pre-Accept endpoint](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/user-initiated-calls#pre-accept-call) for the same `call-id`

[View Calling API Error Codes and Troubleshooting for more information](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/troubleshooting)

[View general Cloud API Error Codes here](https://developers.facebook.com/documentation/business-messaging/whatsapp/support/error-codes)

### Reject call

Use this endpoint to reject a call.

You have about 30 to 60 seconds after the [Call Connect webhook](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/user-initiated-calls#call-connect-webhook) is sent to accept the phone call. If the business does not respond, the call is terminated on the WhatsApp user side with a &quot;Not Answered&quot; notification and a [Terminate Webhook](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/user-initiated-calls#call-terminate-webhook) is delivered back to you.

#### Request syntax

```https
POST &lt;PHONE_NUMBER_ID&gt;/calls
```

| Placeholder | Description | Sample Value |
| --- | --- | --- |
| `&lt;PHONE_NUMBER_ID&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | **Required**&lt;br&gt;&lt;br&gt;The business phone number which you are using Calling API features from.&lt;br&gt;&lt;br&gt;[Learn more about formatting phone numbers in Cloud API](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-phone-numbers/phone-numbers) | `+12784358810` |

#### Request body

```https
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;call_id&quot;: &quot;wacid.ABGGFjFVU2AfAgo6V-Hc5eCgK5Gh&quot;,
  &quot;action&quot;: &quot;reject&quot;
&#125;
```

#### Body parameters

| Parameter | Description | Sample Value |
| --- | --- | --- |
| `call_id`&lt;br&gt;&lt;br&gt;_String_ | **Required**&lt;br&gt;&lt;br&gt;The ID of the phone call.&lt;br&gt;&lt;br&gt;For inbound calls, you receive a call ID from the [Call Connect webhook](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/user-initiated-calls#call-connect-webhook) when a WhatsApp user initiates the call. | `&quot;wacid.ABGGFjFVU2AfAgo6V-Hc5eCgK5Gh&quot;` |
| `action`&lt;br&gt;&lt;br&gt;_String_ | **Optional**&lt;br&gt;&lt;br&gt;The action being taken on the given call ID.&lt;br&gt;&lt;br&gt;Values can be `connect` \| `pre_accept` \| `accept` \| `reject` \| `terminate` | `&quot;reject&quot;` |

#### Success response

```https
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;success&quot; : true
&#125;
```

#### Error response

Possible errors that can occur:

* Invalid `call-id`
* Invalid `phone-number-id`
* Accept/Reject an already In Progress/Completed/Failed call
* Permissions/Authorization errors

[View Calling API Error Codes and Troubleshooting for more information](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/troubleshooting)

[View general Cloud API Error Codes here](https://developers.facebook.com/documentation/business-messaging/whatsapp/support/error-codes)

### Terminate call

Use this endpoint to terminate an active call.

This must be done even if there is an `RTCP BYE` packet in the media path. Ending the call this way also ensures pricing is more accurate.

When the WhatsApp user terminates the call, you do not have to call this endpoint. Once the call is successfully terminated, a [Call Terminate Webhook](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/user-initiated-calls#call-terminate-webhook) will be sent to you.

#### Request syntax

```https
POST &lt;PHONE_NUMBER_ID&gt;/calls
```

| Placeholder | Description | Sample Value |
| --- | --- | --- |
| `&lt;PHONE_NUMBER_ID&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | **Required**&lt;br&gt;&lt;br&gt;The business phone number which you are using Calling API features from.&lt;br&gt;&lt;br&gt;[Learn more about formatting phone numbers in Cloud API](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-phone-numbers/phone-numbers) | `+12784358810` |

#### Request body

```https
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;call_id&quot;: &quot;wacid.ABGGFjFVU2AfAgo6V-Hc5eCgK5Gh&quot;,
  &quot;action&quot;: &quot;terminate&quot;
&#125;
```

#### Body parameters

| Parameter | Description | Sample Value |
| --- | --- | --- |
| `call_id`&lt;br&gt;&lt;br&gt;_String_ | **Required**&lt;br&gt;&lt;br&gt;The ID of the phone call.&lt;br&gt;&lt;br&gt;For inbound calls, you receive a call ID from the [Call Connect webhook](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/user-initiated-calls#call-connect-webhook) when a WhatsApp user initiates the call. | `&quot;wacid.ABGGFjFVU2AfAgo6V-Hc5eCgK5Gh&quot;` |
| `action`&lt;br&gt;&lt;br&gt;_String_ | **Optional**&lt;br&gt;&lt;br&gt;The action being taken on the given call ID.&lt;br&gt;&lt;br&gt;Values can be `connect` \| `pre_accept` \| `accept` \| `reject` \| `terminate` | `&quot;terminate&quot;` |

#### Success response

```https
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;success&quot; : true
&#125;
```

#### Error response

Possible errors that can occur:

* Invalid `call-id`
* Invalid `phone-number-id`
* Accept/Reject an already In Progress/Completed/Failed call
* Reject call is already in progress
* Permissions/Authorization errors

[View Calling API Error Codes and Troubleshooting for more information](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/troubleshooting)

[View general Cloud API Error Codes here](https://developers.facebook.com/documentation/business-messaging/whatsapp/support/error-codes)

## Webhooks for user-initiated calling

With all Calling API webhooks, there is a `&quot;calls&quot;` object inside the `&quot;value&quot;` object of the webhook response. The `&quot;calls&quot;` object contains metadata about the call that is used to action on each call received by your business.

To receive Calling API webhooks, subscribe to the calls webhook field.

[Learn more about Cloud API webhooks here](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/overview)

### Call Connect webhook

A webhook notification is sent in near real-time when a call initiated by your business is ready to be connected to the WhatsApp user (an `SDP Answer`).

Critically, the webhook contains information required to establish a call connection via WebRTC.

Once you receive the Call Connect webhook, you can apply the `SDP Answer` received in the webhook to your WebRTC stack in order to initiate the media connection.

```https
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;&quot;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;messaging_product&quot;: &quot;whatsapp&quot;,
            &quot;metadata&quot;: &#123;
              &quot;display_phone_number&quot;: &quot;16315553601&quot;,
              &quot;phone_number_id&quot;: &quot;&lt;PHONE_NUMBER_ID&gt;&quot;
            &#125;,
            &quot;contacts&quot;: [
              &#123;
                &quot;profile&quot;: &#123;
                  &quot;name&quot;: &quot;&lt;USER_DISPLAY_NAME&gt;&quot;,
                  &quot;username&quot;: &quot;&lt;USERNAME&gt;&quot;
                &#125;,
                &quot;wa_id&quot;: &quot;16315553602&quot;,
                &quot;user_id&quot;: &quot;&lt;BSUID&gt;&quot;,
                &quot;parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot;
              &#125;
            ],
            &quot;calls&quot;: [
              &#123;
                &quot;id&quot;: &quot;wacid.ABGGFjFVU2AfAgo6V-Hc5eCgK5Gh&quot;,
                &quot;to&quot;: &quot;16315553601&quot;,
                &quot;from&quot;: &quot;16315553602&quot;,
                &quot;from_user_id&quot;: &quot;&lt;BSUID&gt;&quot;,
                &quot;from_parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot;,
                &quot;event&quot;: &quot;connect&quot;,
                &quot;timestamp&quot;: &quot;1671644824&quot;,
                &quot;direction&quot;: &quot;USER_INITIATED&quot;,
                &quot;deeplink_payload&quot;: &quot;deeplink_payload&quot;,
                &quot;cta_payload&quot;: &quot;cta_payload&quot;,
                &quot;session&quot;: &#123;
                  &quot;sdp_type&quot;: &quot;offer&quot;,
                  &quot;sdp&quot;: &quot;&lt;&lt;RFC 8866 SDP&gt;&gt;&quot;
                &#125;
              &#125;
            ]
          &#125;,
          &quot;field&quot;: &quot;calls&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

#### Webhook values for `&quot;calls&quot;`

| Placeholder | Description |
| --- | --- |
| `id`&lt;br&gt;&lt;br&gt;_String_ | A unique ID for the call |
| `to`&lt;br&gt;&lt;br&gt;_String_ | The number being called (callee) |
| `from`&lt;br&gt;&lt;br&gt;_String_ | The number of the caller. May be omitted if the user has adopted a username and the phone number cannot be included. |
| `from_user_id`&lt;br&gt;&lt;br&gt;_String_ | The [BSUID](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-scoped-user-ids#business-scoped-user-id) of the WhatsApp user. |
| `from_parent_user_id`&lt;br&gt;&lt;br&gt;_String_ | **Optional.** The [parent BSUID](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-scoped-user-ids#parent-business-scoped-user-ids) of the WhatsApp user. Only included if parent BSUIDs are enabled. |
| `event`&lt;br&gt;&lt;br&gt;_String_ | The calling event that this webhook is notifying the subscriber of |
| `timestamp`&lt;br&gt;&lt;br&gt;_String_ | The UNIX timestamp of the webhook event |
| `direction`&lt;br&gt;&lt;br&gt;_String_ | The direction of the call being made.&lt;br&gt;&lt;br&gt;Can contain either:&lt;br&gt;&lt;br&gt;`BUSINESS_INITIATED`, for calls initiated by your business.&lt;br&gt;&lt;br&gt;`USER_INITIATED`, for calls initiated by a WhatsApp user. |
| `deeplink_payload`&lt;br&gt;&lt;br&gt;_String_ | Arbitrary string specified in `biz_payload` query param on a call deeplink. Will only be returned if call was initiated from a deeplink with such param.&lt;br&gt;&lt;br&gt;See [Call Button Messages and Deep Links&lt;br&gt;](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/call-button-messages-deep-links#send-payload-data-in-call-deeplink) for more details. |
| `cta_payload`&lt;br&gt;&lt;br&gt;_String_ | Arbitrary string specified in `payload` field on a call button. Will only be returned if call was initiated from a call button with payload.&lt;br&gt;&lt;br&gt;See [Call Button Messages and Deep Links&lt;br&gt;](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/call-button-messages-deep-links#send-interactive-message-with-a-whatsapp-call-button) for more details. |
| `session`&lt;br&gt;&lt;br&gt;_JSON object_ | **Optional**&lt;br&gt;&lt;br&gt;Contains the session description protocol (SDP) type and description language.&lt;br&gt;&lt;br&gt;Requires two values:&lt;br&gt;&lt;br&gt;`sdp_type` — (_String_) **Required**&lt;br&gt;&lt;br&gt;&quot;offer&quot;, to indicate SDP offer&lt;br&gt;&lt;br&gt;`sdp` — (_String_) **Required**&lt;br&gt;&lt;br&gt;The SDP info of the device on the other end of the call. The SDP must be compliant with [RFC 8866](https://datatracker.ietf.org/doc/html/rfc8866).&lt;br&gt;&lt;br&gt;[Learn more about Session Description Protocol (SDP)](https://www.rfc-editor.org/rfc/rfc8866.html)&lt;br&gt;&lt;br&gt;[View example SDP structures](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/reference#sdp-overview-and-sample-sdp-structures) |
| `contacts`&lt;br&gt;&lt;br&gt;_JSON object_ | Profile information of the user.&lt;br&gt;&lt;br&gt;Contains the following values:&lt;br&gt;&lt;br&gt;`profile.name` — The WhatsApp profile name of the user.&lt;br&gt;&lt;br&gt;`profile.username` — **Optional.** The username of the user, if the user has adopted a username.&lt;br&gt;&lt;br&gt;`wa_id` — The WhatsApp ID of the user. May be omitted if the user has adopted a username and the phone number cannot be included.&lt;br&gt;&lt;br&gt;`user_id` — The [BSUID](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-scoped-user-ids#business-scoped-user-id) of the WhatsApp user.&lt;br&gt;&lt;br&gt;`parent_user_id` — **Optional.** The [parent BSUID](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-scoped-user-ids#parent-business-scoped-user-ids) of the WhatsApp user. Only included if parent BSUIDs are enabled. |

**Note:** **Usernames and business-scoped user IDs:** The Call Connect webhook may include `from_user_id`, `from_parent_user_id`, and contact-level `user_id`, `parent_user_id`, and `username` fields, and the user&#039;s phone number may be omitted. For details, see [Business-scoped user IDs](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-scoped-user-ids#business-scoped-user-id).

### Call Terminate webhook

A webhook notification is sent whenever the call has been terminated for any reason, such as when the WhatsApp user hangs up, or when the business uses the [Calls API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/calling-api) with an action of `terminate` or `reject`.

```https
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;&quot;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
              &quot;messaging_product&quot;: &quot;whatsapp&quot;,
              &quot;metadata&quot;: &#123;
                   &quot;display_phone_number&quot;: &quot;16505553602&quot;,
                   &quot;phone_number_id&quot;: &quot;&lt;PHONE_NUMBER_ID&gt;&quot;,
              &#125;,
               &quot;contacts&quot;: [
                &#123;
                    &quot;profile&quot;: &#123;
                        &quot;name&quot;: &quot;&lt;USER_DISPLAY_NAME&gt;&quot;,
                        &quot;username&quot;: &quot;&lt;USERNAME&gt;&quot;
                    &#125;,
                    &quot;wa_id&quot;: &quot;16315553602&quot;,
                    &quot;user_id&quot;: &quot;&lt;BSUID&gt;&quot;,
                    &quot;parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot;
                &#125;
              ],
               &quot;calls&quot;: [
                &#123;
                    &quot;id&quot;: &quot;wacid.ABGGFjFVU2AfAgo6V-Hc5eCgK5Gh&quot;,
                    &quot;to&quot;: &quot;16315553601&quot;,
                    &quot;from&quot;: &quot;16315553602&quot;,
                    &quot;from_user_id&quot;: &quot;&lt;BSUID&gt;&quot;,
                    &quot;from_parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot;,
                    &quot;event&quot;: &quot;terminate&quot;
                    &quot;direction&quot;: &quot;USER_INITIATED&quot;,
                    &quot;deeplink_payload&quot;: &quot;deeplink_payload&quot;,
                    &quot;cta_payload&quot;: &quot;cta_payload&quot;,
                    &quot;biz_opaque_callback_data&quot;: &quot;random_string&quot;,
                    &quot;timestamp&quot;: &quot;1671644824&quot;,
                    &quot;status&quot; : [FAILED | COMPLETED],
                    &quot;start_time&quot; : &quot;1671644824&quot;,
                    &quot;end_time&quot; : &quot;1671644944&quot;,
                    &quot;duration&quot; : 120
                &#125;
              ],
              &quot;errors&quot;: [
                &#123;
                    &quot;code&quot;: INT_CODE,
                    &quot;message&quot;: &quot;ERROR_TITLE&quot;,
                    &quot;href&quot;: &quot;ERROR_HREF&quot;,
                    &quot;error_data&quot;: &#123;
                        &quot;details&quot;: &quot;ERROR_DETAILS&quot;
                    &#125;
                &#125;
              ]
          &#125;,
          &quot;field&quot;: &quot;calls&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

#### Webhook values for `&quot;calls&quot;`

| Placeholder | Description |
| --- | --- |
| `id`&lt;br&gt;&lt;br&gt;_String_ | A unique ID for the call |
| `to`&lt;br&gt;&lt;br&gt;_String_ | The number being called (callee) |
| `from`&lt;br&gt;&lt;br&gt;_String_ | The number of the caller. May be omitted if the user has adopted a username and the phone number cannot be included. |
| `from_user_id`&lt;br&gt;&lt;br&gt;_String_ | The [BSUID](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-scoped-user-ids#business-scoped-user-id) of the WhatsApp user. |
| `from_parent_user_id`&lt;br&gt;&lt;br&gt;_String_ | **Optional.** The [parent BSUID](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-scoped-user-ids#parent-business-scoped-user-ids) of the WhatsApp user. Only included if parent BSUIDs are enabled. |
| `event`&lt;br&gt;&lt;br&gt;_String_ | The calling event that this webhook is notifying the subscriber of |
| `timestamp`&lt;br&gt;&lt;br&gt;_String_ | The UNIX timestamp of the webhook event |
| `direction`&lt;br&gt;&lt;br&gt;_String_ | The direction of the call being made.&lt;br&gt;&lt;br&gt;Can contain either:&lt;br&gt;&lt;br&gt;`BUSINESS_INITIATED`, for calls initiated by your business.&lt;br&gt;&lt;br&gt;`USER_INITIATED`, for calls initiated by a WhatsApp user. |
| `deeplink_payload`&lt;br&gt;&lt;br&gt;_String_ | Arbitrary string specified in `biz_payload` query param on a call deeplink. Will only be returned if call was initiated from a deeplink with such param.&lt;br&gt;&lt;br&gt;See [Call Button Messages and Deep Links&lt;br&gt;](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/call-button-messages-deep-links#send-payload-data-in-call-deeplink) for more details. |
| `cta_payload`&lt;br&gt;&lt;br&gt;_String_ | Arbitrary string specified in `payload` field on a call button. Will only be returned if call was initiated from a call button with payload.&lt;br&gt;&lt;br&gt;See [Call Button Messages and Deep Links&lt;br&gt;](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/call-button-messages-deep-links#send-interactive-message-with-a-whatsapp-call-button) for more details. |
| `start_time`&lt;br&gt;&lt;br&gt;_String_ | The UNIX timestamp of when the call started.&lt;br&gt;&lt;br&gt;Only present when the call was picked up by the other party. |
| `end_time`&lt;br&gt;&lt;br&gt;_String_ | The UNIX timestamp of when the call ended.&lt;br&gt;&lt;br&gt;Only present when the call was picked up by the other party. |
| `duration`&lt;br&gt;&lt;br&gt;_Integer_ | Duration of the call in seconds.&lt;br&gt;&lt;br&gt;Only present when the call was picked up by the other party. |
| `biz_opaque_callback_data`&lt;br&gt;&lt;br&gt;_String_ | Arbitrary string your business passes into the call for tracking and logging purposes.&lt;br&gt;&lt;br&gt;Will only be returned if provided through an [Initiate Call request](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/reference#initiate-call) or [Accept Call request](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/user-initiated-calls#accept-call) |
| `errors.code`&lt;br&gt;&lt;br&gt;_Integer_ | The `errors` object is present only for failed calls when there is error information available. Code is one of the [calling error codes](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/troubleshooting#calling-error-codes) |
| `contacts`&lt;br&gt;&lt;br&gt;_JSON object_ | Profile information of the user.&lt;br&gt;&lt;br&gt;Contains the following values:&lt;br&gt;&lt;br&gt;`profile.name` — The WhatsApp profile name of the user.&lt;br&gt;&lt;br&gt;`profile.username` — **Optional.** The username of the user, if the user has adopted a username.&lt;br&gt;&lt;br&gt;`wa_id` — The WhatsApp ID of the user. May be omitted if the user has adopted a username and the phone number cannot be included.&lt;br&gt;&lt;br&gt;`user_id` — The [BSUID](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-scoped-user-ids#business-scoped-user-id) of the WhatsApp user.&lt;br&gt;&lt;br&gt;`parent_user_id` — **Optional.** The [parent BSUID](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-scoped-user-ids#parent-business-scoped-user-ids) of the WhatsApp user. Only included if parent BSUIDs are enabled. |

**Note:** **Usernames and business-scoped user IDs:** The Call Terminate webhook may include `from_user_id`, `from_parent_user_id`, and contact-level `user_id`, `parent_user_id`, and `username` fields, and the user&#039;s phone number may be omitted. For details, see [Business-scoped user IDs](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-scoped-user-ids#business-scoped-user-id).

## Dual tone multi frequency (DTMF) support

**Warning:** **The dialpad provided by the Calling API only supports DTMF use cases.**

It does not support consumer-to-consumer calls and does not change any other calling behaviors. For example, the dialpad cannot be used to dial a number and initiate a call or message on WhatsApp.

WhatsApp Business Calling API supports DTMF tones, with the intention to enable Solution Partner applications to support IVR-based systems.

WhatsApp users can press tone buttons on their client app and these DTMF tones are injected into the WebRTC RTP stream established as a part of the VoIP connection.

Our WebRTC stream conforms to [RFC 4733](https://datatracker.ietf.org/doc/html/rfc4733) for the transfer of DTMF Digits via RTP Payload.

There is no webhook for conveying DTMF digits.

### DTMF clock rate

Only 8000 clock rate is supported in our SDPs. For user-initiated calls, our SDP offer includes only 8000 clock rate. For business-initiated calls, your SDP offer should have 8000 clock rate. Even if it is absent, the API still proceeds with 8000 clock rate against payload type 126.

The RTP packets representing DTMF events will use the same timestamp base and sequence number base as the regular audio packets. So you don&#039;t have to worry about differing clock rates between audio packets and DTMF packets. The [duration field](https://datatracker.ietf.org/doc/html/rfc4733#section-2.3.5) of the DTMF packet is calculated using 8000 clock units.

The API does not support 48000 clock rate for DTMF.

### Sending DTMF digits on consumer WhatsApp client

WhatsApp client applications are enhanced to have a dialpad for calls with CloudAPI business phone numbers. The WhatsApp user can press the buttons on the dialpad and send DTMF tones.

## SDP overview and sample SDP structures

Session Description Protocol (SDP) is a text-based format used to describe the characteristics of multimedia sessions, such as voice and video calls, in real-time communication applications. SDP provides a standardized way to convey information about the session&#039;s media streams, including the type of media, codecs, protocols, and other parameters necessary for establishing and managing the session.

In the context of WebRTC, SDP is used to negotiate the media parameters between the sender and receiver, enabling them to agree on the specifics of the media exchange.

[View SDP sample structures for user-initiated calls](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/reference#sdp-overview-and-sample-sdp-structures)
