# Business-Initiated Calls



## Overview

The Calling API lets your business call WhatsApp users.

The WhatsApp user controls when your business can call them by [granting call permissions to your business phone number](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/user-call-permissions).

### Call sequence diagram

_Note: The `ACCEPTED` call status webhook arrives after the call is established. The Cloud API sends it for call event auditing._

## Prerequisites

Before you get started with business-initiated calling, ensure that:

* You [subscribe](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/create-webhook-endpoint#configure-webhooks) to the &quot;calls&quot; webhook field
* You [enable the Calling APIs on your business phone number](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/call-settings)

Lastly, **before you can call a WhatsApp user, you must obtain their permission to do so.**

[Learn how to obtain WhatsApp user calling permissions](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/user-call-permissions)

## Business-initiated calling flow

### Part 1: Obtain permission to call the WhatsApp user

You can obtain call permissions from the WhatsApp user in one of the following ways:

#### Send a call permission request message

You can request call permissions by sending the WhatsApp user a permission request. Send it as a free form message during an open customer service window, or use a template message.

* [Learn how to send a **free form** call permission request](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/user-call-permissions#how-to-send-a-free-form-call-permission-request-message)
* [Learn how to send a **template** call permission request](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/user-call-permissions#how-to-create-and-send-call-permission-request-template-messages)

#### Enable `callback_permission_status` in call settings

When `callback_permission_status` is enabled, the user automatically provides call permission to your business when they place a call to you.

[Learn how to enable `callback_permission_status`](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/call-settings#configure-update-business-phone-number-calling-settings)

### Part 2: Your business initiates a new call to the WhatsApp user

Now that you have user permission, you can initiate a new call to the WhatsApp user in question.

Use the [Calls API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/calling-api) with the following request body to initiate a new call:

```https
POST &lt;PHONE_NUMBER_ID&gt;/calls
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;to&quot;:&quot;12185552828&quot;, // The WhatsApp user&#039;s phone number (callee)
  &quot;recipient&quot;: &quot;US.13491208655302741918&quot;,
  &quot;action&quot;:&quot;connect&quot;,
  &quot;session&quot; : &#123;
      &quot;sdp_type&quot; : &quot;offer&quot;,
      &quot;sdp&quot; : &quot;&lt;&lt;RFC 8866 SDP&gt;&gt;&quot;
  &#125;
&#125;
```

If there are no errors, you receive a successful response:

```https
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;calls&quot; : [
    &#123; &quot;id&quot; : &quot;wacid.HBgLMTIxODU1NTI4MjgVAgARGCAyODRQIAFRoA&quot; &#125; // The WhatsApp call ID
   ]
&#125;
```

_Note: Response with error code `138006` indicates a lack of a call request permission for this business number from the WhatsApp user._

### Part 3: You establish the call connection using webhook signaling

After you successfully initiate a new call, you receive a Call Connect webhook response containing an `SDP Answer` from Cloud API. Your business then applies the `SDP Answer` from this webhook to your WebRTC stack to initiate the media connection.

```https
&#123;
    &quot;entry&quot;: [
        &#123;
            &quot;changes&quot;: [
                &#123;
                    &quot;field&quot;: &quot;calls&quot;,
                    &quot;value&quot;: &#123;
                        &quot;calls&quot;: [
                            &#123;
                                &quot;biz_opaque_callback_data&quot;: &quot;TRx334DUDFTI4Mj&quot;, // Arbitrary string passed by business for tracking purposes
                                &quot;session&quot;: &#123;
                                    &quot;sdp_type&quot;: &quot;answer&quot;,
                                    &quot;sdp&quot;: &quot;&lt;RFC 8866 SDP&gt;&quot;
                                &#125;,
                                &quot;from&quot;: &quot;13175551399&quot;, // The business phone number placing the call (caller)
                                &quot;connection&quot;: &#123;
                                    &quot;webrtc&quot;: &#123;
                                        &quot;sdp&quot;: &quot;&lt;RFC 8866 SDP&gt;&quot;
                                    &#125;
                                &#125;,
                                &quot;id&quot;: &quot;wacid.HBgLMTIxODU1NTI4MjgVAgARGCAyODRQIAFRoA&quot;, // The WhatsApp call ID
                                &quot;to&quot;: &quot;12185552828&quot;, // The WhatsApp user&#039;s phone number (callee)
                                &quot;to_user_id&quot;: &quot;&lt;BSUID&gt;&quot;,
                                &quot;to_parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot;,
                                &quot;event&quot;: &quot;connect&quot;,
                                &quot;timestamp&quot;: &quot;1749196895&quot;,
                                &quot;direction&quot;: &quot;BUSINESS_INITIATED&quot;
                            &#125;
                        ],
                        &quot;contacts&quot;: [
                            &#123;
                                &quot;profile&quot;: &#123;
                                    &quot;name&quot;: &quot;&lt;CALLEE_NAME&gt;&quot;,
                                    &quot;username&quot;: &quot;&lt;USERNAME&gt;&quot;
                                &#125;,
                                &quot;wa_id&quot;: &quot;&lt;USER_PHONE_NUMBER&gt;&quot;,
                                &quot;user_id&quot;: &quot;&lt;BSUID&gt;&quot;,
                                &quot;parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot;
                            &#125;
                        ],
                        &quot;metadata&quot;: &#123; // ID and display number for the business phone number placing the call (caller)
                            &quot;phone_number_id&quot;: &quot;436666719526789&quot;,
                            &quot;display_phone_number&quot;: &quot;13175551399&quot;
                        &#125;,
                        &quot;messaging_product&quot;: &quot;whatsapp&quot;
                    &#125;
                &#125;
            ],
            &quot;id&quot;: &quot;366634483210360&quot; // WhatsApp Business Account ID associated with the business phone number
        &#125;
    ],
    &quot;object&quot;: &quot;whatsapp_business_account&quot;
&#125;
```

You then receive an appropriate status webhook, indicating that the call is `RINGING`, `ACCEPTED`, or `REJECTED`:

```https
&#123;
  &quot;entry&quot;: [
    &#123;
      &quot;changes&quot;: [
        &#123;
          &quot;field&quot;: &quot;calls&quot;,
          &quot;value&quot;: &#123;
            &quot;statuses&quot;: [
              &#123;
                &quot;id&quot;: &quot;wacid.HBgLMTIxODU1NTI4MjgVAgARGCAyODRQIAFRoA&quot;, // The WhatsApp call ID
                &quot;type&quot;: &quot;call&quot;,
                &quot;status&quot;: &quot;[RINGING|ACCEPTED|REJECTED]&quot;, // The current call status
                &quot;timestamp&quot;: &quot;1749197000&quot;,
                &quot;recipient_id&quot;: &quot;12185552828&quot;, // The WhatsApp user&#039;s phone number (callee)
                &quot;recipient_user_id&quot;: &quot;&lt;BSUID&gt;&quot;,
                &quot;recipient_parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot;
              &#125;
            ],
            &quot;metadata&quot;: &#123; // ID and display number for the business phone number placing the call (caller)
              &quot;phone_number_id&quot;: &quot;436666719526789&quot;,
              &quot;display_phone_number&quot;: &quot;13175551399&quot;
            &#125;,
            &quot;messaging_product&quot;: &quot;whatsapp&quot;
          &#125;
        &#125;
      ],
      &quot;id&quot;: &quot;366634483210360&quot; // WhatsApp Business Account ID associated with the business phone number
    &#125;
  ],
  &quot;object&quot;: &quot;whatsapp_business_account&quot;
&#125;
```

### Part 4: Your business or the WhatsApp user terminates the call

Either you or the WhatsApp user can terminate the call at any time.

Use the [Calls API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/calling-api) with the following request body to terminate the call:

```curl
POST &lt;PHONE_NUMBER_ID&gt;/calls
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;call_id&quot;: &quot;wacid.HBgLMTIxODU1NTI4MjgVAgARGCAyODRQIAFRoA&quot;, // The WhatsApp call ID
  &quot;action&quot; : &quot;terminate&quot;
&#125;
```

If there are no errors, you receive a success response:

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
              &quot;phone_number_id&quot;: &quot;436666719526789&quot;,
              &quot;display_phone_number&quot;: &quot;13175551399&quot;,

            &#125;,
            &quot;calls&quot;: [
              &#123;
                &quot;id&quot;: &quot;wacid.HBgLMTIxODU1NTI4MjgVAgARGCAyODRQIAFRoA&quot;,
                &quot;to&quot;: &quot;12185552828&quot;, // The WhatsApp user&#039;s phone number (callee)
                &quot;to_user_id&quot;: &quot;&lt;BSUID&gt;&quot;,
                &quot;to_parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot;,
                &quot;from&quot;: &quot;13175551399&quot;, // The business phone number placing the call (caller)
                &quot;event&quot;: &quot;terminate&quot;,
                &quot;direction&quot;: &quot;BUSINESS_INITIATED&quot;,
                &quot;timestamp&quot;: &quot;1749197480&quot;,
                &quot;status&quot;: [&quot;Failed&quot;, &quot;Completed&quot;],
                &quot;start_time&quot;: &quot;1671644824&quot;, // Call start UNIX timestamp
                &quot;end_time&quot;: &quot;1671644944&quot;, // Call end UNIX timestamp
                &quot;duration&quot;: 480 // Call duration in seconds
              &#125;
            ],
            &quot;contacts&quot;: [
              &#123;
                &quot;profile&quot;: &#123;
                  &quot;name&quot;: &quot;&lt;CALLEE_NAME&gt;&quot;,
                  &quot;username&quot;: &quot;&lt;USERNAME&gt;&quot;
                &#125;,
                &quot;wa_id&quot;: &quot;&lt;USER_PHONE_NUMBER&gt;&quot;,
                &quot;user_id&quot;: &quot;&lt;BSUID&gt;&quot;,
                &quot;parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot;
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

## Endpoints for business-initiated calling

### Initiate call

Use this endpoint to initiate a call to a WhatsApp user by providing a phone number and a WebRTC call offer. There is a rate limit of 10000 per 24 hours for initiating new calls per business phone number.

#### Request syntax

```https
POST &lt;PHONE_NUMBER_ID&gt;/calls
```

| Placeholder | Description | Sample Value |
| --- | --- | --- |
| `&lt;PHONE_NUMBER_ID&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | **Required**&lt;br&gt;&lt;br&gt;ID of the business phone number from which you are initiating the new call. | `106540352242922` |

#### Request body

```https
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;to&quot;: &quot;14085551234&quot;,
  &quot;recipient&quot;: &quot;US.13491208655302741918&quot;,
  &quot;action&quot;: &quot;connect&quot;,
  &quot;session&quot;: &#123;
    &quot;sdp_type&quot;: &quot;offer&quot;,
    &quot;sdp&quot;: &quot;&lt;&lt;RFC 8866 SDP&gt;&gt;&quot;
  &#125;,
  &quot;biz_opaque_callback_data&quot;: &quot;0fS5cePMok&quot;
&#125;
```

#### Body parameters

| Parameter | Description | Sample Value |
| --- | --- | --- |
| `to`&lt;br&gt;&lt;br&gt;_Integer_ | **Required** (unless `recipient` is provided)&lt;br&gt;&lt;br&gt;The number being called (callee). The user can be identified by phone number (`to`), BSUID (`recipient`), or both. If you include both, `to` takes precedence.&lt;br&gt;&lt;br&gt;[Learn more about formatting phone numbers in Cloud API](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-phone-numbers/phone-numbers)&lt;br&gt;&lt;br&gt;[Learn how business-scoped user IDs apply to business-initiated call requests](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-scoped-user-ids#businesses-initiated-call-requests) | `&quot;17863476655&quot;` |
| `recipient`&lt;br&gt;&lt;br&gt;_String_ | **Optional**&lt;br&gt;&lt;br&gt;The WhatsApp user&#039;s business-scoped user ID (BSUID) or parent BSUID. Use this instead of, or in addition to, `to`. If you include both, `to` takes precedence.&lt;br&gt;&lt;br&gt;[Learn more about business-scoped user IDs](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-scoped-user-ids#business-scoped-user-id) | `&quot;US.13491208655302741918&quot;` |
| `action`&lt;br&gt;&lt;br&gt;_String_ | **Required**&lt;br&gt;&lt;br&gt;The action being taken on the given call ID.&lt;br&gt;&lt;br&gt;Values can be `connect` \| `pre_accept` \| `accept` \| `reject` \| `terminate` | `&quot;connect&quot;` |
| `session`&lt;br&gt;&lt;br&gt;_JSON object_ | **Optional**&lt;br&gt;&lt;br&gt;Contains the session description protocol (SDP) type and description language.&lt;br&gt;&lt;br&gt;Requires two values:&lt;br&gt;&lt;br&gt;`sdp_type` — (_String_) **Required**&lt;br&gt;&lt;br&gt;&quot;offer&quot;, to indicate SDP offer&lt;br&gt;&lt;br&gt;`sdp` — (_String_) **Required**&lt;br&gt;&lt;br&gt;The SDP info of the device on the other end of the call. The SDP must be compliant with [RFC 8866](https://datatracker.ietf.org/doc/html/rfc8866).&lt;br&gt;&lt;br&gt;[Learn more about Session Description Protocol (SDP)](https://www.rfc-editor.org/rfc/rfc8866.html)&lt;br&gt;&lt;br&gt;[View example SDP structures](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/reference#sdp-overview-and-sample-sdp-structures) | ```https
&quot;session&quot; :
&#123;
&quot;sdp_type&quot; : &quot;offer&quot;,
&quot;sdp&quot; : &quot;&lt;&lt;RFC 8866 SDP&gt;&gt;&quot;
&#125;
``` |
| `biz_opaque_callback_data`&lt;br&gt;&lt;br&gt;_String_ | **Optional**&lt;br&gt;&lt;br&gt;An arbitrary string you can pass in that is useful for tracking and logging purposes.&lt;br&gt;&lt;br&gt;Any app subscribed to the &quot;calls&quot; webhook field on your WhatsApp Business account can receive this string, as it is included in the `calls` object within the subsequent [Call Terminate Webhook](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/business-initiated-calls#call-terminate-webhook) payload.&lt;br&gt;&lt;br&gt;Cloud API does not process this field.&lt;br&gt;&lt;br&gt;Maximum 512 characters | `&quot;0fS5cePMok&quot;` |

**Note:** **Usernames and business-scoped user IDs:** When initiating a call, you can identify the recipient by phone number (`to`), BSUID (`recipient`), or both; the user&#039;s phone number may be omitted if a `recipient` is provided. For details, see [Business-scoped user IDs](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-scoped-user-ids#business-scoped-user-id).

#### Success response

```https
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;calls&quot; : [&#123;
     &quot;id&quot; : &quot;wacid.ABGGFjFVU2AfAgo6V&quot;,
   &#125;]
&#125;
```

#### Error response

Possible errors that can occur:

* Invalid `&lt;PHONE_NUMBER_ID&gt;`
* Permissions/Authorization errors
* Request format validation errors, for example connection info, sdp, ice
* SDP validation errors
* Calling restriction errors

[View Calling API Error Codes and Troubleshooting for more information](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/troubleshooting)

[View general Cloud API Error Codes here](https://developers.facebook.com/documentation/business-messaging/whatsapp/support/error-codes)

### Terminate call

Use this endpoint to terminate an active call.

This must be done even if there is an `RTCP BYE` packet in the media path. Ending the call this way also ensures pricing is more accurate.

When the WhatsApp user terminates the call, you do not have to call this endpoint. Once the call is successfully terminated, you receive a [Call Terminate Webhook](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/business-initiated-calls#call-terminate-webhook).

#### Request syntax

```https
POST &lt;PHONE_NUMBER_ID&gt;/calls
```

| Parameter | Description | Sample Value |
| --- | --- | --- |
| `&lt;PHONE_NUMBER_ID&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | **Required**&lt;br&gt;&lt;br&gt;The business phone number which you are terminating a call from.&lt;br&gt;&lt;br&gt;[Learn more about formatting phone numbers in Cloud API](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-phone-numbers/phone-numbers) | `18274459827` |

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
| `call_id`&lt;br&gt;&lt;br&gt;_String_ | **Required**&lt;br&gt;&lt;br&gt;The ID of the phone call.&lt;br&gt;&lt;br&gt;For inbound calls, you receive a call ID from the [Call Connect webhook](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/business-initiated-calls#call-connect-webhook) when a WhatsApp user initiates the call. | `&quot;wacid.ABGGFjFVU2AfAgo6V-Hc5eCgK5Gh&quot;` |
| `action`&lt;br&gt;&lt;br&gt;_String_ | **Required**&lt;br&gt;&lt;br&gt;The action being taken on the given call ID.&lt;br&gt;&lt;br&gt;Values can be `connect` \| `pre_accept` \| `accept` \| `reject` \| `terminate` | `&quot;terminate&quot;` |

#### Success response

```https
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;success&quot; : true
&#125;
```

#### Error response

Possible errors that can occur:

* Invalid `call_id`
* Invalid `&lt;PHONE_NUMBER_ID&gt;`
* The WhatsApp user has already terminated the call
* Reject call is already in progress
* Permissions/Authorization errors

[View Calling API Error Codes and Troubleshooting for more information](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/troubleshooting)

[View general Cloud API Error Codes here](https://developers.facebook.com/documentation/business-messaging/whatsapp/support/error-codes)

## Webhooks for business-initiated calling

With all Calling API webhooks, there is a `&quot;calls&quot;` object inside the `&quot;value&quot;` object of the webhook response. The `&quot;calls&quot;` object contains metadata about the call that is used to action on each call placed or received by your business.

To receive Calling API webhooks, subscribe to the &quot;calls&quot; webhook field.

[Learn more about Cloud API webhooks here](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/messages/status)

### Call connect webhook

You receive a webhook notification in near real-time when a call initiated by your business is ready to connect to the WhatsApp user (an `SDP Answer`).

Critically, the webhook contains information required to establish a call connection via WebRTC.

Once you receive the Call Connect webhook, you can apply the `SDP Answer` received in the webhook to your WebRTC stack to initiate the media connection.

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
                  &quot;name&quot;: &quot;&lt;CALLEE_NAME&gt;&quot;,
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
                &quot;to_user_id&quot;: &quot;&lt;BSUID&gt;&quot;,
                &quot;to_parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot;,
                &quot;from&quot;: &quot;16315553602&quot;,
                &quot;event&quot;: &quot;connect&quot;,
                &quot;timestamp&quot;: &quot;1671644824&quot;,
                &quot;direction&quot;: &quot;BUSINESS_INITIATED&quot;,
                &quot;session&quot;: &#123;
                  &quot;sdp_type&quot;: &quot;answer&quot;,
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
| `to`&lt;br&gt;&lt;br&gt;_String_ | The number being called (callee). May be omitted if the user has adopted a username and the phone number cannot be included. |
| `to_user_id`&lt;br&gt;&lt;br&gt;_String_ | The [BSUID](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-scoped-user-ids#business-scoped-user-id) of the WhatsApp user. |
| `to_parent_user_id`&lt;br&gt;&lt;br&gt;_String_ | **Optional.** The [parent BSUID](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-scoped-user-ids#parent-business-scoped-user-ids) of the WhatsApp user. Only included if parent BSUIDs are enabled. |
| `from`&lt;br&gt;&lt;br&gt;_String_ | The number of the caller |
| `event`&lt;br&gt;&lt;br&gt;_String_ | The calling event that this webhook is notifying the subscriber of |
| `timestamp`&lt;br&gt;&lt;br&gt;_String_ | The UNIX timestamp of the webhook event |
| `direction`&lt;br&gt;&lt;br&gt;_String_ | The direction of the call being made.&lt;br&gt;&lt;br&gt;Can contain either:&lt;br&gt;&lt;br&gt;`BUSINESS_INITIATED`, for calls initiated by your business.&lt;br&gt;&lt;br&gt;`USER_INITIATED`, for calls initiated by a WhatsApp user. |
| `session`&lt;br&gt;&lt;br&gt;_JSON object_ | **Optional**&lt;br&gt;&lt;br&gt;Contains the session description protocol (SDP) type and description language.&lt;br&gt;&lt;br&gt;Requires two values:&lt;br&gt;&lt;br&gt;`sdp_type` — (_String_) **Required**&lt;br&gt;&lt;br&gt;&quot;offer&quot;, to indicate SDP offer&lt;br&gt;&lt;br&gt;`sdp` — (_String_) **Required**&lt;br&gt;&lt;br&gt;The SDP info of the device on the other end of the call. The SDP must be compliant with [RFC 8866](https://datatracker.ietf.org/doc/html/rfc8866).&lt;br&gt;&lt;br&gt;[Learn more about Session Description Protocol (SDP)](https://www.rfc-editor.org/rfc/rfc8866.html)&lt;br&gt;&lt;br&gt;[View example SDP structures](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/reference#sdp-overview-and-sample-sdp-structures) |
| `contacts`&lt;br&gt;&lt;br&gt;_JSON object_ | `profile.name` — The display name of the callee.&lt;br&gt;&lt;br&gt;`profile.username` — **Optional.** The username of the user, if the user has adopted a username.&lt;br&gt;&lt;br&gt;`wa_id` — The WhatsApp ID of the callee. May be omitted if the user has adopted a username and the phone number cannot be included.&lt;br&gt;&lt;br&gt;`user_id` — The [BSUID](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-scoped-user-ids#business-scoped-user-id) of the WhatsApp user.&lt;br&gt;&lt;br&gt;`parent_user_id` — **Optional.** The [parent BSUID](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-scoped-user-ids#parent-business-scoped-user-ids) of the WhatsApp user. Only included if parent BSUIDs are enabled. |

**Note:** **Usernames and business-scoped user IDs:** The Call connect webhook may include `to_user_id`, `to_parent_user_id`, and `contacts` fields containing the user&#039;s BSUID and username; the user&#039;s phone number may be omitted. For details, see [Business-scoped user IDs](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-scoped-user-ids#business-scoped-user-id).

### Call status webhook

WhatsApp sends this webhook during the following calling events:

1. Ringing: When the WhatsApp user&#039;s client device begins ringing
1. Accepted: When the WhatsApp user accepts the call
1. Rejected: When the WhatsApp user rejects the call. You also receive the call terminate webhook when the user rejects the call.

The webhook structure here is similar to the Status webhooks used for the Cloud API messages.

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
                   &quot;phone_number_id&quot;: &quot;&lt;PHONE_NUMBER_ID&gt;&quot;,
              &#125;,
              &quot;statuses&quot;: [&#123;
                    &quot;id&quot;: &quot;wacid.ABGGFjFVU2AfAgo6V&quot;,
                    &quot;timestamp&quot;: &quot;1671644824&quot;,
                    &quot;type&quot;: &quot;call&quot;,
                    &quot;status&quot;: &quot;[RINGING|ACCEPTED|REJECTED]&quot;,
                    &quot;recipient_id&quot;: &quot;163155536021&quot;,
                    &quot;recipient_user_id&quot;: &quot;&lt;BSUID&gt;&quot;,
                    &quot;recipient_parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot;,
                    &quot;biz_opaque_callback_data&quot;: &quot;random_string&quot;,
               &#125;]
          &#125;,
          &quot;field&quot;: &quot;calls&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

[_Learn more about Cloud API status webhooks_](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/messages/status)

#### Webhook values for `&quot;statuses&quot;`

| Placeholder | Description |
| --- | --- |
| `id`&lt;br&gt;&lt;br&gt;_String_ | A unique ID for the call |
| `timestamp`&lt;br&gt;&lt;br&gt;_String_ | The UNIX timestamp of the webhook event |
| `recipient_id`&lt;br&gt;&lt;br&gt;_String_ | The phone number of the WhatsApp user receiving the call. May be omitted if the user has adopted a username and the phone number cannot be included. |
| `recipient_user_id`&lt;br&gt;&lt;br&gt;_String_ | The [BSUID](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-scoped-user-ids#business-scoped-user-id) of the WhatsApp user. |
| `recipient_parent_user_id`&lt;br&gt;&lt;br&gt;_String_ | **Optional.** The [parent BSUID](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-scoped-user-ids#parent-business-scoped-user-ids) of the WhatsApp user. Only included if parent BSUIDs are enabled. |
| `status`&lt;br&gt;&lt;br&gt;_String_ | The current call status.&lt;br&gt;&lt;br&gt;Possible values:&lt;br&gt;&lt;br&gt;`RINGING`: Business initiated call is ringing the user&lt;br&gt;&lt;br&gt;`ACCEPTED`: Business initiated call is accepted by the user&lt;br&gt;&lt;br&gt;`REJECTED`: Business initiated call is rejected by the user |
| `biz_opaque_callback_data`&lt;br&gt;&lt;br&gt;_String_ | Arbitrary string your business passes into the call for tracking and logging purposes.&lt;br&gt;&lt;br&gt;Will only be returned if provided through [Initiate New Call API requests](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/business-initiated-calls#initiate-a-new-call) |

**Note:** **Usernames and business-scoped user IDs:** The Call status webhook may include `recipient_user_id` and `recipient_parent_user_id` fields containing the user&#039;s BSUID; the user&#039;s phone number (`recipient_id`) may be omitted. For details, see [Business-scoped user IDs](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-scoped-user-ids#business-scoped-user-id).

### Call terminate webhook

WhatsApp sends a webhook notification whenever the call is terminated for any reason, such as when the WhatsApp user hangs up, or when the business uses the [Calls API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/calling-api) with an action of `terminate` or `reject`.

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
               &quot;calls&quot;: [
                &#123;
                    &quot;id&quot;: &quot;wacid.ABGGFjFVU2AfAgo6V-Hc5eCgK5Gh&quot;,
                    &quot;to&quot;: &quot;16315553601&quot;,
                    &quot;to_user_id&quot;: &quot;&lt;BSUID&gt;&quot;,
                    &quot;to_parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot;,
                    &quot;from&quot;: &quot;16315553602&quot;,
                    &quot;event&quot;: &quot;terminate&quot;,
                    &quot;direction&quot;: &quot;BUSINESS_INITIATED&quot;,
                    &quot;biz_opaque_callback_data&quot;: &quot;random_string&quot;,
                    &quot;timestamp&quot;: &quot;1671644824&quot;,
                    &quot;status&quot; : [FAILED | COMPLETED],
                    &quot;start_time&quot; : &quot;1671644824&quot;,
                    &quot;end_time&quot; : &quot;1671644944&quot;,
                    &quot;duration&quot; : 120
                &#125;
              ],
              &quot;contacts&quot;: [
                &#123;
                    &quot;profile&quot;: &#123;
                        &quot;name&quot;: &quot;&lt;CALLEE_NAME&gt;&quot;,
                        &quot;username&quot;: &quot;&lt;USERNAME&gt;&quot;
                    &#125;,
                    &quot;wa_id&quot;: &quot;&lt;USER_PHONE_NUMBER&gt;&quot;,
                    &quot;user_id&quot;: &quot;&lt;BSUID&gt;&quot;,
                    &quot;parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot;
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
| `to`&lt;br&gt;&lt;br&gt;_String_ | The number being called (callee). May be omitted if the user has adopted a username and the phone number cannot be included. |
| `to_user_id`&lt;br&gt;&lt;br&gt;_String_ | The [BSUID](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-scoped-user-ids#business-scoped-user-id) of the WhatsApp user. |
| `to_parent_user_id`&lt;br&gt;&lt;br&gt;_String_ | **Optional.** The [parent BSUID](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-scoped-user-ids#parent-business-scoped-user-ids) of the WhatsApp user. Only included if parent BSUIDs are enabled. |
| `from`&lt;br&gt;&lt;br&gt;_String_ | The number of the caller |
| `event`&lt;br&gt;&lt;br&gt;_String_ | The calling event that this webhook is notifying the subscriber of |
| `timestamp`&lt;br&gt;&lt;br&gt;_String_ | The UNIX timestamp of the webhook event |
| `direction`&lt;br&gt;&lt;br&gt;_String_ | The direction of the call being made.&lt;br&gt;&lt;br&gt;Can contain either:&lt;br&gt;&lt;br&gt;`BUSINESS_INITIATED`, for calls initiated by your business.&lt;br&gt;&lt;br&gt;`USER_INITIATED`, for calls initiated by a WhatsApp user. |
| `start_time`&lt;br&gt;&lt;br&gt;_String_ | The UNIX timestamp of when the call started.&lt;br&gt;&lt;br&gt;Only present when the call was picked up by the other party. |
| `end_time`&lt;br&gt;&lt;br&gt;_String_ | The UNIX timestamp of when the call ended.&lt;br&gt;&lt;br&gt;Only present when the call was picked up by the other party. |
| `duration`&lt;br&gt;&lt;br&gt;_Integer_ | Duration of the call in seconds.&lt;br&gt;&lt;br&gt;Only present when the call was picked up by the other party. |
| `biz_opaque_callback_data`&lt;br&gt;&lt;br&gt;_String_ | Arbitrary string your business passes into the call for tracking and logging purposes.&lt;br&gt;&lt;br&gt;Will only be returned if provided through an [Initiate Call API request](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/business-initiated-calls#initiate-call) or [Accept Call request](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/reference#accept-call) |
| `errors.code`&lt;br&gt;&lt;br&gt;_Integer_ | The `errors` object is present only for failed calls when there is error information available. Code is one of the [calling error codes](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/troubleshooting#calling-error-codes) |
| `contacts`&lt;br&gt;&lt;br&gt;_JSON object_ | `profile.name` — The display name of the callee.&lt;br&gt;&lt;br&gt;`profile.username` — **Optional.** The username of the user, if the user has adopted a username.&lt;br&gt;&lt;br&gt;`wa_id` — The WhatsApp ID of the callee. May be omitted if the user has adopted a username and the phone number cannot be included.&lt;br&gt;&lt;br&gt;`user_id` — The [BSUID](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-scoped-user-ids#business-scoped-user-id) of the WhatsApp user.&lt;br&gt;&lt;br&gt;`parent_user_id` — **Optional.** The [parent BSUID](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-scoped-user-ids#parent-business-scoped-user-ids) of the WhatsApp user. Only included if parent BSUIDs are enabled. |

**Note:** **Usernames and business-scoped user IDs:** The Call terminate webhook may include `to_user_id`, `to_parent_user_id`, and `contacts` fields containing the user&#039;s BSUID and username; the user&#039;s phone number may be omitted. For details, see [Business-scoped user IDs](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-scoped-user-ids#business-scoped-user-id).

## SDP overview and sample structures

Session Description Protocol (SDP) is a text-based format used to describe the characteristics of multimedia sessions, such as voice and video calls, in real-time communication applications. SDP provides a standardized way to describe the session&#039;s media streams. The SDP description includes media type, codecs, protocols, and parameters for establishing and managing the session.

In the context of WebRTC, SDP is used to negotiate the media parameters between the sender and receiver, enabling them to agree on the specifics of the media exchange.

[View SDP sample structures for business-initiated calls](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/reference#sdp-overview-and-sample-sdp-structures)
