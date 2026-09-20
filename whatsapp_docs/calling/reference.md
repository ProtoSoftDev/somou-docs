# API and Webhook Reference



## Calling API endpoints

### Configure or update calling settings

Use the [Settings API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/settings-api#post-version-phone-number-id-settings) and pass in Calling API parameters to configure settings on a business phone number you designate in the request syntax.

#### Request syntax

```https
POST /&lt;PHONE_NUMBER_ID&gt;/settings
```

#### Endpoint parameters

| Placeholder | Description | Sample Value |
| --- | --- | --- |
| `&lt;PHONE_NUMBER_ID&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | **Required**&lt;br&gt;&lt;br&gt;The business phone number for which you are updating Calling API settings. | `+12784358810` |

#### Request body

```curl
&#123;
  &quot;calling&quot;: &#123;
    &quot;status&quot;: &quot;ENABLED&quot;,
    &quot;call_icon_visibility&quot;: &quot;DEFAULT&quot;,
    &quot;call_hours&quot;: &#123;
      &quot;status&quot;: &quot;ENABLED&quot;,
      &quot;timezone_id&quot;: &quot;America/Manaus&quot;,
      &quot;weekly_operating_hours&quot;: [
        &#123;
          &quot;day_of_week&quot;: &quot;MONDAY&quot;,
          &quot;open_time&quot;: &quot;0400&quot;,
          &quot;close_time&quot;: &quot;1020&quot;
        &#125;,
        &#123;
          &quot;day_of_week&quot;: &quot;TUESDAY&quot;,
          &quot;open_time&quot;: &quot;0108&quot;,
          &quot;close_time&quot;: &quot;1020&quot;
        &#125;
      ],
      &quot;holiday_schedule&quot;: [
        &#123;
          &quot;date&quot;: &quot;2026-01-01&quot;,
          &quot;start_time&quot;: &quot;0000&quot;,
          &quot;end_time&quot;: &quot;2359&quot;
        &#125;
      ]
    &#125;,
    &quot;callback_permission_status&quot;: &quot;ENABLED&quot;,
    &quot;sip&quot;: &#123;
      &quot;status&quot;: &quot;ENABLED | DISABLED (default)&quot;,
      &quot;servers&quot;: [
        &#123;
          &quot;hostname&quot;: SIP_SERVER_HOSTNAME,
          &quot;port&quot;: SIP_SERVER_PORT,
          &quot;request_uri_user_params&quot;: &#123;
            &quot;KEY1&quot;: &quot;VALUE1&quot;,
            &quot;KEY2&quot;: &quot;VALUE2&quot;
          &#125;
        &#125;
      ]
    &#125;
  &#125;
&#125;
```

#### Body parameters

| Parameter | Description | Sample Value |
| --- | --- | --- |
| `status`&lt;br&gt;&lt;br&gt;_String_ | **Optional**&lt;br&gt;&lt;br&gt;Enable or disable the Calling API for the given business phone number. | `&quot;ENABLED&quot;`&lt;br&gt;&lt;br&gt;`&quot;DISABLED&quot;` |
| `call_icon_visibility`&lt;br&gt;&lt;br&gt;_String_ | **Optional**&lt;br&gt;&lt;br&gt;Configure whether the WhatsApp call button icon displays for users when chatting with the business.&lt;br&gt;&lt;br&gt;[View call icon visibility behavior details in the Parameter details section](#configure-call-settings-parameter-details) | [View call icon visibility behavior details below](#configure-call-settings-parameter-details) |
| `call_hours`&lt;br&gt;&lt;br&gt;_JSON object_ | **Optional**&lt;br&gt;&lt;br&gt;Allows you to specify and trigger call settings for incoming calls based on your timezone, business operating hours, and holiday schedules.&lt;br&gt;&lt;br&gt;Any previously configured values in `call_hours` will be replaced with the values passed in the request body of this API call.&lt;br&gt;&lt;br&gt;[View call hours behavior details in the Parameter details section](#configure-call-settings-parameter-details) | [View call hours behavior details below](#configure-call-settings-parameter-details) |
| `callback_permission_status`&lt;br&gt;&lt;br&gt;_String_ | **Optional**&lt;br&gt;&lt;br&gt;Configure whether a WhatsApp user is prompted with a call permission request after calling your business.&lt;br&gt;&lt;br&gt;Note: The call permission request is triggered from either a missed or connected call.&lt;br&gt;&lt;br&gt;[View callback permission status behavior details in the Parameter details section ](#configure-call-settings-parameter-details) | `&quot;ENABLED&quot;`&lt;br&gt;&lt;br&gt;`&quot;DISABLED&quot;` |
| `sip`&lt;br&gt;&lt;br&gt;_JSON object_ | **Optional**&lt;br&gt;&lt;br&gt;Configure call signaling via signal initiation protocol (SIP).&lt;br&gt;&lt;br&gt;**Note: When SIP is enabled, you cannot use calling related endpoints and will not receive calling related webhooks.**&lt;br&gt;&lt;br&gt;[Learn how to configure and use SIP call signaling](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/sip) | ```curl
&quot;sip&quot;: &#123;
   &quot;status&quot;: &quot;ENABLED \| DISABLED (default)&quot;,
   &quot;servers&quot;: [// one server per app]
     &#123;
       &quot;hostname&quot;: SIP_SERVER_HOSTNAME
       &quot;port&quot;: SIP_SERVER_PORT,
       &quot;request_uri_user_params&quot;: &#123;
         &quot;KEY1&quot;: &quot;VALUE1&quot;, // for cases like TGRP
         &quot;KEY2&quot;: &quot;VALUE2&quot;,
       &#125;
     &#125;
   ]
 &#125;
``` |

#### Parameter details: Calling status &#123;#configure-call-settings-parameter-details&#125;

When the `status` parameter is set to `&quot;ENABLED&quot;`, calling features are enabled for the business phone number. WhatsApp client applications render the call button icon in both the business chat and business chat profile.

When the `status` parameter is set to `&quot;DISABLED&quot;`, calling features are **disabled**, and both the business chat and business chat profile **do not display the call button icon.**

Updates to `status` update the call button icon in existing business chats in near real-time when the business phone number is in the WhatsApp user&#039;s contacts.

Otherwise, updates are near real-time for a limited number of users in conversation with the business, and are eventually updated for the rest of the conversations.

#### Parameter details: Call button icon visibility

When Calling API features are enabled for a business number, you can still choose whether to show the call button icon or not by using the `call_icon_visibility` parameter. Note: Disabling call button icon visibility **does not** disable a WhatsApp user&#039;s ability to make unsolicited calls to your business.

The behavior for supported options is as follows:

`DEFAULT`

The call button icon appears in the chat menu bar and the business info page, allowing for unsolicited calls to the business by WhatsApp users.

`DISABLE ALL`

The call button icon is hidden in the chat menu bar and the business info page, and all other entry points external to the chat are also disabled. WhatsApp users cannot make unsolicited calls to the business.

Your business can still [send interactive messages](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/call-button-messages-deep-links#send-interactive-message-with-a-whatsapp-call-button) or [template messages](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/call-button-messages-deep-links#create-and-send-whatsapp-call-button-template-message) with a Calling API CTA button.

##### Callback permissions

Calling a WhatsApp user requires explicit permission from the user. One way to obtain calling permissions is to request permission when a WhatsApp user calls your business.

You can configure the call permission UI to automatically show in the WhatsApp user&#039;s client app when they call your business number. The user may change their permission selection at any time.

#### Call hours

With the `call_hours` setting, you can specify the timezone, business operating hours, and holiday schedules that will be enforced for all user-initiated calls.

Configuring this setting restricts calls only to available weekly hours you configure. User-initiated calls are unavailable outside of the weekly hours and holiday schedules you set.

The WhatsApp client app shows users an option to chat with the business, or request a callback, if `callback_permission_status` is `ENABLED`. The user also sees the next available calling slot on the option screen.

```curl
&quot;call_hours&quot;: &#123;
  &quot;status&quot;: &quot;ENABLED&quot;,
  &quot;timezone_id&quot;: &quot;America/Manaus&quot;,
  &quot;weekly_operating_hours&quot;: [
    &#123;
      &quot;day_of_week&quot;: &quot;MONDAY&quot;,
      &quot;open_time&quot;: &quot;04:00&quot;,
      &quot;close_time&quot;: &quot;10:20&quot;
    &#125;,
    &#123;
      &quot;day_of_week&quot;: &quot;TUESDAY&quot;,
      &quot;open_time&quot;: &quot;01:08&quot;,
      &quot;close_time&quot;: &quot;10:20&quot;
    &#125;
  ],
  &quot;holiday_schedule&quot;: [
    &#123;
      &quot;date&quot;: &quot;2026-01-01&quot;,
      &quot;start_time&quot;: &quot;00:00&quot;,
      &quot;end_time&quot;: &quot;23:59&quot;
    &#125;
  ]
&#125;
```

| Parameter | Description | Sample Values |
| --- | --- | --- |
| `status`&lt;br&gt;&lt;br&gt;_String_ | **Required**&lt;br&gt;&lt;br&gt;&lt;br&gt;Enable or disable the call hours for your business.&lt;br&gt;&lt;br&gt;If call hours are disabled, your business is considered open 24 hours a day, 7 days a week. | `&quot;ENABLED&quot;`&lt;br&gt;&lt;br&gt;`&quot;DISABLED&quot;` |
| `timezone_id`&lt;br&gt;&lt;br&gt;_String_ | **Required**&lt;br&gt;&lt;br&gt;The timezone your business operates in.&lt;br&gt;&lt;br&gt;[Learn more about supported values for `timezone_id`](https://developers.facebook.com/docs/facebook-business-extension/fbe/reference#time-zones) | `&quot;America/Menominee&quot;`&lt;br&gt;&lt;br&gt;`&quot;Asia/Singapore&quot;` |
| `weekly_operating_hours`&lt;br&gt;&lt;br&gt;_List of JSON objects_ | **Required**&lt;br&gt;&lt;br&gt;The operating hours schedule for each day of the week.&lt;br&gt;&lt;br&gt;Each entry is a JSON object with three key-value pairs:&lt;br&gt;&lt;br&gt;`day_of_week` — (_Enum_) **[Required]**&lt;br&gt;&lt;br&gt;The day of the week.&lt;br&gt;&lt;br&gt;Can take one of seven values: `&quot;MONDAY&quot;`, `&quot;TUESDAY&quot;`, `&quot;WEDNESDAY&quot;`, `&quot;THURSDAY&quot;`, `&quot;FRIDAY&quot;`, `&quot;SATURDAY&quot;`, `&quot;SUNDAY&quot;`&lt;br&gt;&lt;br&gt;`open_time` \| `close_time` — (_Integer_) **[Required]**&lt;br&gt;&lt;br&gt;Opening and closing times represented in 24 hour format, for example `&quot;1130&quot;` = 11:30AM&lt;br&gt;&lt;br&gt;* A maximum of two entries is allowed per day of the week&lt;br&gt;* `open_time` must be before `close_time`&lt;br&gt;* Overlapping entries not allowed | ```curl
&#123;
&quot;day_of_week&quot;: &quot;MONDAY&quot;,
&quot;open_time&quot;: &quot;0400&quot;,
&quot;close_time&quot;: &quot;1020&quot;
&#125;,
&#123;
&quot;day_of_week&quot;:&quot;TUESDAY&quot;,
&quot;open_time&quot;: &quot;0108&quot;,
&quot;close_time&quot;: &quot;1020&quot;
&#125;
...
``` |
| `holiday_schedule`&lt;br&gt;&lt;br&gt;_String_ | **Optional**&lt;br&gt;&lt;br&gt;An optional override to the weekly schedule.&lt;br&gt;&lt;br&gt;Up to 20 overrides can be specified.&lt;br&gt;&lt;br&gt;Note: If `holiday_schedule` is not passed in the request, then the existing `holiday_schedule` will be deleted and replaced with an empty schedule.&lt;br&gt;&lt;br&gt;`date` — (_String_) **[Required]**&lt;br&gt;&lt;br&gt;Date for which you want to specify the override.&lt;br&gt;&lt;br&gt;YYYY-MM-DD format.&lt;br&gt;&lt;br&gt;`open_time` \| `close_time` — (_Integer_) **[Required]**&lt;br&gt;&lt;br&gt;Opening and closing times represented in 24 hour format, for example, `&quot;1130&quot;` = 11:30AM&lt;br&gt;&lt;br&gt;* A maximum of two entries is allowed per day of the week&lt;br&gt;* `open_time` must be before `close_time`&lt;br&gt;* Overlapping entries not allowed | ```curl
&#123;
&quot;date&quot;: &quot;2026-01-01&quot;,
&quot;start_time&quot;: &quot;0000&quot;,
&quot;end_time&quot;: &quot;2359&quot;,
&#125;
...
``` |

#### Success response

```curl
&#123;
  &quot;success&quot;: true
&#125;
```

#### Error response

Possible errors that can occur:

* Permissions/Authorization errors
* Invalid status
* Invalid schedule for `call_hours`
* Holiday given in `call_hours` is a past date
* Timezone is invalid in `call_hours`
* `weekly_operating_hours` in `call_hours` cannot be empty
* Date format in `holiday_schedule` for call_hours is invalid
* More than 2 entries not allowed in `weekly_operating_hours` schedule in `call_hours`
* Overlapping schedule in `call_hours` is not allowed

[View Calling API Error Codes and Troubleshooting for more information](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/troubleshooting).

[View general Cloud API Error Codes here](https://developers.facebook.com/documentation/business-messaging/whatsapp/support/error-codes).

### Get phone number calling settings

Use the [Settings API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/settings-api#get-version-phone-number-id-settings) to retrieve Calling API settings on an individual business phone number you designate in the request syntax.

This endpoint can return information for other Cloud API feature settings.

#### Request syntax

```https
POST /&lt;PHONE_NUMBER_ID&gt;/settings
```

#### Endpoint parameters

| Parameter | Description | Sample Value |
| --- | --- | --- |
| `&lt;PHONE_NUMBER_ID&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | **Required**&lt;br&gt;&lt;br&gt;The business phone number for which you are getting Calling API settings.&lt;br&gt;&lt;br&gt;[Learn more about formatting phone numbers in Cloud API](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-phone-numbers/phone-numbers) | `+12784358810` |

#### App permission required

`whatsapp_business_management`: Advanced access is required to use the API for end business clients

#### Response body

```curl
&#123;
  &quot;calling&quot;: &#123;
    &quot;status&quot;: &quot;ENABLED&quot;,
    &quot;call_icon_visibility&quot;: &quot;DEFAULT&quot;,
    &quot;call_hours&quot;: &#123;
      &quot;status&quot;: &quot;ENABLED&quot;,
      &quot;timezone_id&quot;: &quot;America/Manaus&quot;,
      &quot;weekly_operating_hours&quot;: [
        &#123;
          &quot;day_of_week&quot;: &quot;MONDAY&quot;,
          &quot;open_time&quot;: &quot;0400&quot;,
          &quot;close_time&quot;: &quot;1020&quot;
        &#125;,
        &#123;
          &quot;day_of_week&quot;: &quot;TUESDAY&quot;,
          &quot;open_time&quot;: &quot;0108&quot;,
          &quot;close_time&quot;: &quot;1020&quot;
        &#125;
      ],
      &quot;holiday_schedule&quot;: [
        &#123;
          &quot;date&quot;: &quot;2026-01-01&quot;,
          &quot;start_time&quot;: &quot;0000&quot;,
          &quot;end_time&quot;: &quot;2359&quot;
        &#125;
      ]
    &#125;,
    &quot;callback_permission_status&quot;: &quot;ENABLED&quot;,
    &quot;sip&quot;: &#123;
      &quot;status&quot;: &quot;ENABLED | DISABLED (default)&quot;,
      &quot;servers&quot;: [
        &#123;
          &quot;hostname&quot;: SIP_SERVER_HOSTNAME,
          &quot;port&quot;: SIP_SERVER_PORT,
          &quot;request_uri_user_params&quot;: &#123;
            &quot;KEY1&quot;: &quot;VALUE1&quot;,
            &quot;KEY2&quot;: &quot;VALUE2&quot;
          &#125;
        &#125;
      ]
    &#125;
  &#125;
&#125;
```

#### Include SIP user password

To include SIP user credentials in the response body, add the SIP credentials query parameter to the POST request:

```https
POST /&lt;PHONE_NUMBER_ID&gt;/settings?include_sip_credentials=true
```

Where the response will look like this:

```curl
&#123;
  &quot;calling&quot;: &#123;
    ... // other calling api settings
    &quot;sip&quot;: &#123;
      &quot;status&quot;: &quot;ENABLED&quot;,
      &quot;servers&quot;: [
        &#123;
          &quot;hostname&quot;: &quot;sip.example.com&quot;,
          &quot;sip_user_password&quot;: &quot;&#123;SIP_USER_PASSWORD&#125;&quot;
        &#125;
      ]
    &#125;
  &#125;
&#125;
```

#### Response details

The `GET /&lt;PHONE_NUMBER_ID&gt;/settings` endpoint returns Calling API settings, along with other configuration information for your WhatsApp Business phone number.

[Learn more about Calling API settings and their values](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/call-settings#body-parameters)

#### Error response

Possible errors that can occur:

* Permissions/Authorization errors

[View Calling API Error Codes and Troubleshooting for more information](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/troubleshooting)

[View general Cloud API Error Codes here](https://developers.facebook.com/documentation/business-messaging/whatsapp/support/error-codes)

### Pre-accept call

When you pre-accept an inbound call, you allow the calling media connection to be established before attempting to send call media through the connection.

When you then call the accept call endpoint, media begins flowing immediately since the connection has already been established.

Pre-accepting calls is recommended because it facilitates faster connection times and avoids [audio clipping issues](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/troubleshooting#audio-clipping-issue-and-solution).

There is about 30 to 60 seconds after the [Call Connect webhook](#call-connect-webhook) is sent for the business to accept the phone call. If the business does not respond, the call is terminated on the WhatsApp user side with a &quot;Not Answered&quot; notification and a [Terminate Webhook](#call-terminate-webhook) is delivered back to you.

**Warning:** **Note:** Since the WebRTC connection is established before calling the [Accept Call endpoint](#accept-call), make sure to flow the call media only after you receive a 200 OK response back.

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
| `call_id`&lt;br&gt;&lt;br&gt;_String_ | **Required**&lt;br&gt;&lt;br&gt;The ID of the phone call.&lt;br&gt;&lt;br&gt;For inbound calls, you receive a call ID from the [Call Connect webhook](#call-connect-webhook) when a WhatsApp user initiates the call. | `&quot;wacid.ABGGFjFVU2AfAgo6V-Hc5eCgK5Gh&quot;` |
| `action`&lt;br&gt;&lt;br&gt;_String_ | **Optional**&lt;br&gt;&lt;br&gt;The action being taken on the given call ID.&lt;br&gt;&lt;br&gt;Values can be `connect` \| `pre_accept` \| `accept` \| `reject` \| `terminate` | `&quot;pre_accept&quot;` |
| `session`&lt;br&gt;&lt;br&gt;_JSON object_ | **Optional**&lt;br&gt;&lt;br&gt;Contains the session description protocol (SDP) type and description language.&lt;br&gt;&lt;br&gt;Requires two values:&lt;br&gt;&lt;br&gt;`sdp_type` — (_String_) **Required**&lt;br&gt;&lt;br&gt;&quot;offer&quot;, to indicate SDP offer&lt;br&gt;&lt;br&gt;`sdp` — (_String_) **Required**&lt;br&gt;&lt;br&gt;The SDP info of the device on the other end of the call. The SDP must be compliant with [RFC 8866](https://datatracker.ietf.org/doc/html/rfc8866).&lt;br&gt;&lt;br&gt;[Learn more about Session Description Protocol (SDP)](https://www.rfc-editor.org/rfc/rfc8866.html)&lt;br&gt;&lt;br&gt;[View example SDP structures](#sdp-overview-and-sample-sdp-structures) | ```https
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
* Invalid Connection info, for example, `sdp`, `ice`
* Accept/Reject an already In Progress/Completed/Failed call
* Permissions/Authorization errors

[View Calling API Error Codes and Troubleshooting for more information](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/troubleshooting).

[View general Cloud API Error Codes here](https://developers.facebook.com/documentation/business-messaging/whatsapp/support/error-codes).

### Accept call

Use this endpoint to connect to a call by providing a call agent&#039;s SDP.

You have about 30 to 60 seconds after the [Call Connect Webhook](#call-connect-webhook) is sent to accept the phone call. If your business does not respond, the call is terminated on the WhatsApp user side with a &quot;Not Answered&quot; notification and a [Terminate Webhook](#call-terminate-webhook) is delivered back to you.

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
| `call_id`&lt;br&gt;&lt;br&gt;_String_ | **Required**&lt;br&gt;&lt;br&gt;The ID of the phone call.&lt;br&gt;&lt;br&gt;For inbound calls, you receive a call ID from the [Call Connect webhook](#call-connect-webhook) when a WhatsApp user initiates the call. | `&quot;wacid.ABGGFjFVU2AfAgo6V-Hc5eCgK5Gh&quot;` |
| `action`&lt;br&gt;&lt;br&gt;_String_ | **Optional**&lt;br&gt;&lt;br&gt;The action being taken on the given call ID.&lt;br&gt;&lt;br&gt;Values can be `connect` \| `pre_accept` \| `accept` \| `reject` \| `terminate` | `&quot;accept&quot;` |
| `session`&lt;br&gt;&lt;br&gt;_JSON object_ | **Optional**&lt;br&gt;&lt;br&gt;Contains the session description protocol (SDP) type and description language.&lt;br&gt;&lt;br&gt;Requires two values:&lt;br&gt;&lt;br&gt;`sdp_type` — (_String_) **Required**&lt;br&gt;&lt;br&gt;&quot;offer&quot;, to indicate SDP offer&lt;br&gt;&lt;br&gt;`sdp` — (_String_) **Required**&lt;br&gt;&lt;br&gt;The SDP info of the device on the other end of the call. The SDP must be compliant with [RFC 8866](https://datatracker.ietf.org/doc/html/rfc8866).&lt;br&gt;&lt;br&gt;[Learn more about Session Description Protocol (SDP)](https://www.rfc-editor.org/rfc/rfc8866.html)&lt;br&gt;&lt;br&gt;[View example SDP structures](#sdp-overview-and-sample-sdp-structures) | ```https
&quot;session&quot; :
&#123;
&quot;sdp_type&quot; : &quot;offer&quot;,
&quot;sdp&quot; : &quot;&lt;&lt;RFC 8866 SDP&gt;&gt;&quot;
&#125;
``` |
| `biz_opaque_callback_data`&lt;br&gt;&lt;br&gt;_String_ | **Optional**&lt;br&gt;&lt;br&gt;An arbitrary string you can pass in that is useful for tracking and logging purposes.&lt;br&gt;&lt;br&gt;Any app subscribed to the &quot;calls&quot; webhook field on your WhatsApp Business account can receive this string, as it is included in the `calls` object within the subsequent [Terminate webhook](#call-terminate-webhook) payload.&lt;br&gt;&lt;br&gt;Cloud API does not process this field, it just returns it as part of the [Terminate webhook](#call-terminate-webhook).&lt;br&gt;&lt;br&gt;Maximum 512 characters | `&quot;8huas8d80nn&quot;` |

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
* Invalid Connection info, for example, `sdp`, `ice`, or other connection parameters
* Accept/Reject an already In Progress/Completed/Failed call
* Permissions/Authorization errors
* SDP answer provided in accept does not match the SDP answer given in the [Pre-Accept endpoint](#pre-accept-call) for the same `call-id`

[View Calling API Error Codes and Troubleshooting for more information](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/troubleshooting).

[View general Cloud API Error Codes here](https://developers.facebook.com/documentation/business-messaging/whatsapp/support/error-codes).

### Reject call

Use this endpoint to reject a call.

You have about 30 to 60 seconds after the [Call Connect webhook](#call-connect-webhook) is sent to accept the phone call. If the business does not respond, the call is terminated on the WhatsApp user side with a &quot;Not Answered&quot; notification and a [Terminate Webhook](#call-terminate-webhook) is delivered back to you.

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
| `call_id`&lt;br&gt;&lt;br&gt;_String_ | **Required**&lt;br&gt;&lt;br&gt;The ID of the phone call.&lt;br&gt;&lt;br&gt;For inbound calls, you receive a call ID from the [Call Connect webhook](#call-connect-webhook) when a WhatsApp user initiates the call. | `&quot;wacid.ABGGFjFVU2AfAgo6V-Hc5eCgK5Gh&quot;` |
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

[View Calling API Error Codes and Troubleshooting for more information](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/troubleshooting).

[View general Cloud API Error Codes here](https://developers.facebook.com/documentation/business-messaging/whatsapp/support/error-codes).

### Initiate call

Use this endpoint to initiate a call to a WhatsApp user by providing a phone number and a WebRTC call offer.

#### Request syntax

```https
POST &lt;PHONE_NUMBER_ID&gt;/calls
```

| Placeholder | Description | Sample Value |
| --- | --- | --- |
| `&lt;PHONE_NUMBER_ID&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | **Required**&lt;br&gt;&lt;br&gt;The business phone number from which you are initiating a new call.&lt;br&gt;&lt;br&gt;[Learn more about formatting phone numbers in Cloud API](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-phone-numbers/phone-numbers) | `+12784358810` |

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
| `to`&lt;br&gt;&lt;br&gt;_Integer_ | **Required** (unless `recipient` is provided)&lt;br&gt;&lt;br&gt;The phone number being called (callee). You can identify the user by their phone number here, by their business-scoped user ID (BSUID) in `recipient`, or both. | `&quot;17863476655&quot;` |
| `recipient`&lt;br&gt;&lt;br&gt;_String_ | **Optional**&lt;br&gt;&lt;br&gt;The WhatsApp user&#039;s business-scoped user ID (BSUID) or parent BSUID. Use this instead of, or in addition to, `to`. If you include both `to` and `recipient`, `to` takes precedence.&lt;br&gt;&lt;br&gt;[Learn more about business-scoped user IDs](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-scoped-user-ids#business-scoped-user-id) | `US.13491208655302741918` |
| `action`&lt;br&gt;&lt;br&gt;_String_ | **Required**&lt;br&gt;&lt;br&gt;The action being taken on the given call ID.&lt;br&gt;&lt;br&gt;Values can be `connect` \| `pre_accept` \| `accept` \| `reject` \| `terminate` | `&quot;connect&quot;` |
| `session`&lt;br&gt;&lt;br&gt;_JSON object_ | **Optional**&lt;br&gt;&lt;br&gt;Contains the session description protocol (SDP) type and description language.&lt;br&gt;&lt;br&gt;Requires two values:&lt;br&gt;&lt;br&gt;`sdp_type` — (_String_) **Required**&lt;br&gt;&lt;br&gt;&quot;offer&quot;, to indicate SDP offer&lt;br&gt;&lt;br&gt;`sdp` — (_String_) **Required**&lt;br&gt;&lt;br&gt;The SDP info of the device on the other end of the call. The SDP must be compliant with [RFC 8866](https://datatracker.ietf.org/doc/html/rfc8866).&lt;br&gt;&lt;br&gt;[Learn more about Session Description Protocol (SDP)](https://www.rfc-editor.org/rfc/rfc8866.html)&lt;br&gt;&lt;br&gt;[View example SDP structures](#sdp-overview-and-sample-sdp-structures) | ```https
&quot;session&quot; :
&#123;
&quot;sdp_type&quot; : &quot;offer&quot;,
&quot;sdp&quot; : &quot;&lt;&lt;RFC 8866 SDP&gt;&gt;&quot;
&#125;
``` |
| `biz_opaque_callback_data`&lt;br&gt;&lt;br&gt;_String_ | **Optional**&lt;br&gt;&lt;br&gt;An arbitrary string you can pass in that is useful for tracking and logging purposes.&lt;br&gt;&lt;br&gt;Any app subscribed to the &quot;calls&quot; webhook field on your WhatsApp Business account can receive this string, as it is included in the `calls` object within the subsequent [Call Terminate Webhook](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/business-initiated-calls#call-terminate-webhook) payload.&lt;br&gt;&lt;br&gt;Cloud API does not process this field.&lt;br&gt;&lt;br&gt;Maximum 512 characters | `&quot;0fS5cePMok&quot;` |

**Note:** **Usernames and business-scoped user IDs:** You can call a WhatsApp user using their phone number (`to`) and/or their business-scoped user ID (`recipient`). If you include both, `to` takes precedence. For details on usernames and BSUIDs, see [Business-scoped user IDs](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-scoped-user-ids#business-scoped-user-id).

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

* Invalid `phone-number-id`
* Permissions/Authorization errors
* Request format validation errors, for example, connection info, `sdp`, `ice`
* SDP validation errors

[View Calling API Error Codes and Troubleshooting for more information](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/troubleshooting).

[View general Cloud API Error Codes here](https://developers.facebook.com/documentation/business-messaging/whatsapp/support/error-codes).

### Terminate call

Use this endpoint to terminate an active call.

This must be done even if there is an `RTCP BYE` packet in the media path. Ending the call this way also ensures pricing is more accurate.

When the WhatsApp user terminates the call, you do not have to call this endpoint. Once the call is successfully terminated, a [Call Terminate Webhook](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/business-initiated-calls#call-terminate-webhook) will be sent to you.

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

* Invalid `call id`
* Invalid `phone-number-id`
* The WhatsApp user has already terminated the call
* Reject call is already in progress
* Permissions/Authorization errors

[View Calling API Error Codes and Troubleshooting for more information](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/troubleshooting).

[View general Cloud API Error Codes here](https://developers.facebook.com/documentation/business-messaging/whatsapp/support/error-codes).

### Get current call permission state

Use this endpoint to get the call permission state for a business phone number with a single WhatsApp user. You can identify the user by their phone number (`user_wa_id`) or by their business-scoped user ID (`recipient`).

#### Request syntax

```https
GET /&lt;PHONE_NUMBER_ID&gt;/call_permissions?user_wa_id=&lt;CONSUMER_WHATSAPP_ID&gt;
```

Or, identify the user by their business-scoped user ID (BSUID) or parent BSUID:

```https
GET /&lt;PHONE_NUMBER_ID&gt;/call_permissions?recipient=&lt;BSUID&gt;
```

**Note:** **Usernames and business-scoped user IDs:** You can identify the WhatsApp user by their phone number (`user_wa_id`) or their business-scoped user ID (`recipient`). For details on usernames and BSUIDs, see [Business-scoped user IDs](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-scoped-user-ids#business-scoped-user-id).

#### Request parameters
| Parameter | Description | Sample Value |
| --- | --- | --- |
| `&lt;PHONE_NUMBER_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required**&lt;br&gt;&lt;br&gt;The business phone number you are fetching permissions against.&lt;br&gt;&lt;br&gt;[Learn more about formatting phone numbers in Cloud API](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-phone-numbers/phone-numbers) | `+18762639988` |
| `&lt;CONSUMER_WHATSAPP_ID&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | **Required** (unless `recipient` is provided)&lt;br&gt;&lt;br&gt;The phone number of the WhatsApp user who you are requesting call permissions from.&lt;br&gt;&lt;br&gt;[Learn more about formatting phone numbers in Cloud API](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-phone-numbers/phone-numbers) | `+13057765456` |
| `recipient`&lt;br&gt;&lt;br&gt;_String_ | **Optional**&lt;br&gt;&lt;br&gt;The business-scoped user ID (BSUID) or parent BSUID of the WhatsApp user you are requesting call permissions from. Use this instead of `user_wa_id`.&lt;br&gt;&lt;br&gt;[Learn more about business-scoped user IDs](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-scoped-user-ids#business-scoped-user-id) | `US.13491208655302741918` |

#### Response body

```https
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;permission&quot;: &#123;
    &quot;status&quot;: &quot;temporary&quot;,
    &quot;expiration_time&quot;: 1745343479
  &#125;,
  &quot;actions&quot;: [
    &#123;
      &quot;action_name&quot;: &quot;send_call_permission_request&quot;,
      &quot;can_perform_action&quot;: true,
      &quot;limits&quot;: [
        &#123;
          &quot;time_period&quot;: &quot;PT24H&quot;,
          &quot;max_allowed&quot;: 1,
          &quot;current_usage&quot;: 0,
        &#125;,
        &#123;
          &quot;time_period&quot;: &quot;P7D&quot;,
          &quot;max_allowed&quot;: 2,
          &quot;current_usage&quot;: 1,
        &#125;
      ]
    &#125;,
    &#123;
      &quot;action_name&quot;: &quot;start_call&quot;,
      &quot;can_perform_action&quot;: false,
      &quot;limits&quot;: [
        &#123;
          &quot;time_period&quot;: &quot;PT24H&quot;,
          &quot;max_allowed&quot;: 5,
          &quot;current_usage&quot;: 5,
          &quot;limit_expiration_time&quot;: 1745622600,
        &#125;
      ]
    &#125;
  ]
&#125;
```

#### Response parameters

| Parameter | Description |
| --- | --- |
| `permission`&lt;br&gt;&lt;br&gt;_JSON Object_ | The permission object contains two values:&lt;br&gt;&lt;br&gt;`status` _(String)_ — The current status of the permission.&lt;br&gt;&lt;br&gt;Can be either:&lt;br&gt;&lt;br&gt;* `&quot;no_permission&quot;`&lt;br&gt;* `&quot;temporary&quot;`&lt;br&gt;&lt;br&gt;`expiration` _(Integer)_ — The Unix time at which the permission will expire in UTC timezone. |
| `actions`&lt;br&gt;&lt;br&gt;_JSON Object_ | A list of actions a business phone number may undertake to facilitate a call permission or a business initiated call.&lt;br&gt;&lt;br&gt;Current actions are:&lt;br&gt;&lt;br&gt;`send_call_permission_request`: Represents the action of sending new call permissions request messages to the WhatsApp user.&lt;br&gt;&lt;br&gt;`start_call`: Represents the action of establishing a new call with the WhatsApp user. Establishing a new call means that the call was successfully picked up by the WhatsApp user.&lt;br&gt;&lt;br&gt;For example, `send_call_permission_request` having a `can_perform_action` of `true` means that your business can send a call permission request to the WhatsApp user in question.&lt;br&gt;&lt;br&gt;`can_perform_action` (_Boolean_) —&lt;br&gt;&lt;br&gt;A flag indicating whether the action can be performed now, taking into account all limits. |
| `limits`&lt;br&gt;&lt;br&gt;_JSON Object_ | A list of time-bound restrictions for the given `action_name`.&lt;br&gt;&lt;br&gt;Each `action_name` has one or more restrictions depending on the timeframe.&lt;br&gt;&lt;br&gt;For example, your business can send only 2 permission requests in a 24-hour period.&lt;br&gt;&lt;br&gt;`limits` contains the following fields:&lt;br&gt;&lt;br&gt;`time_period` (_String_) — The span of time in which the limit applies, represented in the ISO 8601 format.&lt;br&gt;&lt;br&gt;`max_allowed` (_Integer_) — The maximum number of actions allowed within the specified time period.&lt;br&gt;&lt;br&gt;`current_usage` (_Integer_) — The current number of actions your business has taken within the specified time period.&lt;br&gt;&lt;br&gt;`limit_expiration_time` (_Integer_) — The Unix time at which the limit will expire in UTC timezone.&lt;br&gt;&lt;br&gt;If `current_usage` is under the max allowed for the limit, this field won&#039;t be present. |

#### Error response

Possible errors that can occur:

* Invalid `phone-number-id`
* If the WhatsApp user&#039;s phone number is uncallable, the API response will be `no_permission`.
* Permissions/Authorization errors.
* Rate limit reached. A maximum of 5 requests in a one-second window can be made to the API.
* Calling is not enabled for the business phone number.

[View Calling API Error Codes and Troubleshooting for more information](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/troubleshooting)

[View general Cloud API Error Codes here](https://developers.facebook.com/documentation/business-messaging/whatsapp/support/error-codes)

## Calling API Webhooks

### Call Connect webhook

WhatsApp sends a webhook notification in near real-time when a call initiated by your business is ready to be connected to the WhatsApp user (an `SDP Answer`).

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
| `to_user_id`&lt;br&gt;&lt;br&gt;_String_ | The [BSUID](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-scoped-user-ids) of the WhatsApp user. |
| `to_parent_user_id`&lt;br&gt;&lt;br&gt;_String_ | **Optional.** The [parent BSUID](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-scoped-user-ids#parent-business-scoped-user-ids) of the WhatsApp user. Only included if parent BSUIDs are enabled. |
| `from`&lt;br&gt;&lt;br&gt;_String_ | The number of the caller |
| `event`&lt;br&gt;&lt;br&gt;_String_ | The calling event that this webhook is notifying the subscriber of |
| `timestamp`&lt;br&gt;&lt;br&gt;_String_ | The UNIX timestamp of the webhook event |
| `direction`&lt;br&gt;&lt;br&gt;_String_ | The direction of the call being made.&lt;br&gt;&lt;br&gt;Can contain either:&lt;br&gt;&lt;br&gt;`BUSINESS_INITIATED`, for calls initiated by your business.&lt;br&gt;&lt;br&gt;`USER_INITIATED`, for calls initiated by a WhatsApp user. |
| `session`&lt;br&gt;&lt;br&gt;_JSON object_ | **Optional**&lt;br&gt;&lt;br&gt;Contains the session description protocol (SDP) type and description language.&lt;br&gt;&lt;br&gt;Requires two values:&lt;br&gt;&lt;br&gt;`sdp_type` — (_String_) **Required**&lt;br&gt;&lt;br&gt;&quot;offer&quot;, to indicate SDP offer&lt;br&gt;&lt;br&gt;`sdp` — (_String_) **Required**&lt;br&gt;&lt;br&gt;The SDP info of the device on the other end of the call. The SDP must be compliant with [RFC 8866](https://datatracker.ietf.org/doc/html/rfc8866).&lt;br&gt;&lt;br&gt;[Learn more about Session Description Protocol (SDP)](https://www.rfc-editor.org/rfc/rfc8866.html)&lt;br&gt;&lt;br&gt;[View example SDP structures](#sdp-overview-and-sample-sdp-structures) |
| `contacts`&lt;br&gt;&lt;br&gt;_JSON object_ | Profile information of the callee.&lt;br&gt;&lt;br&gt;`name` — The WhatsApp profile name of the callee.&lt;br&gt;&lt;br&gt;`username` — **Optional.** The username of the callee, if the user has adopted a username.&lt;br&gt;&lt;br&gt;`wa_id` — The WhatsApp ID of the callee. May be omitted if the user has adopted a username and the phone number cannot be included.&lt;br&gt;&lt;br&gt;`user_id` — The [BSUID](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-scoped-user-ids) of the callee.&lt;br&gt;&lt;br&gt;`parent_user_id` — **Optional.** The [parent BSUID](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-scoped-user-ids#parent-business-scoped-user-ids) of the callee. Only included if parent BSUIDs are enabled. |

### Call created webhook

WhatsApp sends a webhook notification when a SIP call is attempted. This applies to both business-initiated and user-initiated SIP calls. For non-SIP calls using the Graph API, see the [Call Connect webhook](#call-connect-webhook) instead.

```json
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
                &quot;event&quot;: &quot;call_created&quot;,
                &quot;timestamp&quot;: &quot;1671644824&quot;,
                &quot;direction&quot;: &quot;BUSINESS_INITIATED&quot;
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

The field descriptions are the same as those in the [Call Connect webhook](#call-connect-webhook) section above, with the exception that SIP call webhooks do not include the `session` object since call signaling is handled via SIP rather than WebRTC.

### Call status webhook

This webhook is sent during the following calling events:

1. Ringing: When the WhatsApp user&#039;s client device begins ringing
1. Accepted: When the WhatsApp user accepts the call
1. Rejected: When the call is rejected by the WhatsApp user

The Webhook structure here is similar to the Status webhooks used for the Cloud API messages.

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
                    &quot;type&quot;: &quot;call&quot;
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
| `recipient_user_id`&lt;br&gt;&lt;br&gt;_String_ | The [BSUID](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-scoped-user-ids) of the WhatsApp user receiving the call. |
| `recipient_parent_user_id`&lt;br&gt;&lt;br&gt;_String_ | **Optional.** The [parent BSUID](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-scoped-user-ids#parent-business-scoped-user-ids) of the WhatsApp user receiving the call. Only included if parent BSUIDs are enabled. |
| `status`&lt;br&gt;&lt;br&gt;_String_ | The current call status.&lt;br&gt;&lt;br&gt;Possible values:&lt;br&gt;&lt;br&gt;`RINGING`: Business initiated call is ringing the user&lt;br&gt;&lt;br&gt;`ACCEPTED`: Business initiated call is accepted by the user&lt;br&gt;&lt;br&gt;`REJECTED`: Business initiated call is rejected by the user |
| `biz_opaque_callback_data`&lt;br&gt;&lt;br&gt;_String_ | Arbitrary string your business passes into the call for tracking and logging purposes.&lt;br&gt;&lt;br&gt;Will only be returned if provided through [Initiate New Call API requests](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/business-initiated-calls#initiate-a-new-call) |

### Call terminate webhook

WhatsApp sends a webhook notification whenever the call has been terminated for any reason, such as when the WhatsApp user hangs up, or when the business calls the `POST /&lt;PHONE_NUMBER_ID&gt;/calls` endpoint with an action of `terminate` or `reject`.

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
                        &quot;name&quot;: &quot;&lt;CALLEE_NAME&gt;&quot;,
                        &quot;username&quot;: &quot;&lt;USERNAME&gt;&quot;
                    &#125;,
                    &quot;wa_id&quot;: &quot;16315553601&quot;,
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
                    &quot;event&quot;: &quot;terminate&quot;
                    &quot;direction&quot;: &quot;BUSINESS_INITIATED&quot;,
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
| `to`&lt;br&gt;&lt;br&gt;_String_ | The number being called (callee). May be omitted if the user has adopted a username and the phone number cannot be included. |
| `to_user_id`&lt;br&gt;&lt;br&gt;_String_ | The [BSUID](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-scoped-user-ids) of the WhatsApp user. |
| `to_parent_user_id`&lt;br&gt;&lt;br&gt;_String_ | **Optional.** The [parent BSUID](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-scoped-user-ids#parent-business-scoped-user-ids) of the WhatsApp user. Only included if parent BSUIDs are enabled. |
| `from`&lt;br&gt;&lt;br&gt;_String_ | The number of the caller |
| `event`&lt;br&gt;&lt;br&gt;_String_ | The calling event that this webhook is notifying the subscriber of |
| `timestamp`&lt;br&gt;&lt;br&gt;_String_ | The UNIX timestamp of the webhook event |
| `direction`&lt;br&gt;&lt;br&gt;_String_ | The direction of the call being made.&lt;br&gt;&lt;br&gt;Can contain either:&lt;br&gt;&lt;br&gt;`BUSINESS_INITIATED`, for calls initiated by your business.&lt;br&gt;&lt;br&gt;`USER_INITIATED`, for calls initiated by a WhatsApp user. |
| `start_time`&lt;br&gt;&lt;br&gt;_String_ | The UNIX timestamp of when the call started.&lt;br&gt;&lt;br&gt;Only present when the call was picked up by the other party. |
| `end_time`&lt;br&gt;&lt;br&gt;_String_ | The UNIX timestamp of when the call ended.&lt;br&gt;&lt;br&gt;Only present when the call was picked up by the other party. |
| `duration`&lt;br&gt;&lt;br&gt;_Integer_ | Duration of the call in seconds.&lt;br&gt;&lt;br&gt;Only present when the call was picked up by the other party. |
| `biz_opaque_callback_data`&lt;br&gt;&lt;br&gt;_String_ | Arbitrary string your business passes into the call for tracking and logging purposes.&lt;br&gt;&lt;br&gt;Will only be returned if provided through [New Call API requests](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/reference#initiate-call) or [Accept Call requests](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/reference#accept-call) |

### User calling permission request webhook

This webhook is sent back after requesting user calling permissions.

The webhook changes depending on if the user:

* accepts or rejects the request
* gives permission by responding to a request or by calling the business

**Note:** **Usernames and business-scoped user IDs:** This webhook also includes the WhatsApp user&#039;s business-scoped user ID in `from_user_id` (and `from_parent_user_id` if parent BSUIDs are enabled), and the user&#039;s phone number may be omitted. For details, see [Business-scoped user IDs](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-scoped-user-ids#business-scoped-user-id).

#### Webhook sample

```https
&#123;
. . .

&quot;messages&quot;: [&#123;
    &quot;from&quot;: &quot;&#123;customer_phone_number&#125;&quot;,
    &quot;from_user_id&quot;: &quot;&lt;BSUID&gt;&quot;,
    &quot;from_parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot;,
    &quot;id&quot;: &quot;wamid.sH0kFlaCGg0xcvZbgmg90lHrg2dL&quot;,
    &quot;timestamp&quot;: &quot;&#123;timestamp&#125;&quot;,
    &quot;context&quot;: &#123;
          &quot;from&quot;: &quot;&#123;customer_phone_number&#125;&quot;,
          &quot;id&quot;: &quot;wacid.gBGGFlaCmZ9plHrf2Mh-o&quot;
    &#125;,
    &quot;interactive&quot;: &#123;
       &quot;type&quot;:  &quot;call_permission_reply&quot;,
        &quot;call_permission_reply&quot;: &#123;
            &quot;response&quot;:&quot;accept&quot;,
            &quot;is_permanent&quot;:false,
            &quot;expiration_timestamp&quot;: &quot;&#123;timestamp&#125;&quot;,
            &quot;response_source&quot;: &quot;[user_action|automatic]&quot;
       &#125;
    &#125;
 ],
. . .
&#125;
```

#### Webhook values

| Placeholder | Description |
| --- | --- |
| `customer_phone_number`&lt;br&gt;&lt;br&gt;_String_ | The phone number of the customer. May be omitted if the user has adopted a username and the phone number cannot be included. |
| `from_user_id`&lt;br&gt;&lt;br&gt;_String_ | The [BSUID](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-scoped-user-ids#business-scoped-user-id) of the WhatsApp user. |
| `from_parent_user_id`&lt;br&gt;&lt;br&gt;_String_ | **Optional.** The [parent BSUID](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-scoped-user-ids#parent-business-scoped-user-ids) of the WhatsApp user. Only included if parent BSUIDs are enabled. |
| `context.id`&lt;br&gt;&lt;br&gt;_String_ | Can be either of two values&lt;br&gt;&lt;br&gt;* Message ID of the permission request message sent by the business to the customer number. Shows when a permission decision is made by the user in response to a call permission request.&lt;br&gt;* Call ID of the missed call placed by the business to the customer number. Shows when callback permission is enabled in settings and the user calls the business. |
| `response`&lt;br&gt;&lt;br&gt;_String_ | The WhatsApp user&#039;s response to the call permission request message&lt;br&gt;&lt;br&gt;Can be `accept` or `reject` |
| `expiration_timestamp`&lt;br&gt;&lt;br&gt;_String_ | Time in seconds when this call permission expires if the WhatsApp user approved it |
| `response_source`&lt;br&gt;&lt;br&gt;_String_ | The source of this permission&lt;br&gt;&lt;br&gt;Possible values for accepted call permissions are:&lt;br&gt;&lt;br&gt;* `user_action`: User approved or rejected the permission&lt;br&gt;* `automatic`: An automatic permission approval due to the WhatsApp user initiating the call |

## SDP overview and sample SDP structures

Session Description Protocol (SDP) is a text-based format that describes multimedia session characteristics, such as voice and video calls, in real-time communication applications. SDP provides a standardized way to convey information about the session&#039;s media streams, including the type of media, codecs, protocols, and other parameters necessary for establishing and managing the session.

In the context of WebRTC, SDP is used to negotiate the media parameters between the sender and receiver, enabling them to agree on the specifics of the media exchange.

### Business-initiated sample SDP structures

#### Sample SDP offer structure

```https
v=0
o=- 3626166318745852955 2 IN IP4 127.0.0.1
s=-
t=0 0
a=group:BUNDLE 0
a=extmap-allow-mixed
a=msid-semantic: WMS d8b26053-4474-4eb7-b3c3-c93d6c8c9b2e
m=audio 9 UDP/TLS/RTP/SAVPF 111 63 9 0 8 110 126
c=IN IP4 0.0.0.0
a=rtcp:9 IN IP4 0.0.0.0
a=ice-ufrag:4g1c
a=ice-pwd:qY/Bb+jQzg5ICn6X4fhJQetk
a=ice-options:trickle
a=fingerprint:sha-256 35:47:24:24:9F:93:C4:3E:DB:37:7F:BB:ED:F8:20:B5:AD:AC:DC:35:C2:7D:67:EE:6C:35:54:DF:A6:00:5C:4A
a=setup:actpass
a=mid:0
a=extmap:1 urn:ietf:params:rtp-hdrext:ssrc-audio-level
a=extmap:2 http://www.webrtc.org/experiments/rtp-hdrext/abs-send-time
a=extmap:3 http://www.ietf.org/id/draft-holmer-rmcat-transport-wide-cc-extensions-01
a=extmap:4 urn:ietf:params:rtp-hdrext:sdes:mid
a=sendrecv
a=msid:d8b26053-4474-4eb7-b3c3-c93d6c8c9b2e 5b4d3d96-ea9b-44a8-87e6-11a1ad21a3bc
a=rtcp-mux
a=rtpmap:111 opus/48000/2
a=rtcp-fb:111 transport-cc
a=fmtp:111 minptime=10;useinbandfec=1
a=rtpmap:63 red/48000/2
a=fmtp:63 111/111
a=rtpmap:9 G722/8000
a=rtpmap:0 PCMU/8000
a=rtpmap:8 PCMA/8000
a=rtpmap:110 telephone-event/48000
a=rtpmap:126 telephone-event/8000
a=ssrc:2220762577 cname:w/zwpg3jXNiTFTdZ
a=ssrc:2220762577 msid:d8b26053-4474-4eb7-b3c3-c93d6c8c9b2e 5b4d3d96-ea9b-44a8-87e6-11a1ad21a3bc
```

#### Sample SDP answer structure

```https
v=0
o=- 741807839102053725 2 IN IP4 127.0.0.1
s=-
t=0 0
a=group:BUNDLE 0
a=extmap-allow-mixed
a=msid-semantic: WMS 798a9670-c0d6-47a8-925e-5f082ef4d8a0
a=ice-lite
m=audio 3482 UDP/TLS/RTP/SAVPF 111 9 0 8 110 126
c=IN IP4 31.13.65.130
a=rtcp:9 IN IP4 0.0.0.0
a=candidate:2754936280 1 udp 2113937151 31.13.65.130 3482 typ host generation 0 network-cost 50 ufrag JHqAXFH4HcAY/8
a=candidate:1581496399 1 udp 2113939711 2a03:2880:f211:d1:face:b00c:0:699c 3482 typ host generation 0 network-cost 50 ufrag JHqAXFH4HcAY/8
a=ice-ufrag:JHqAXFH4HcAY/8
a=ice-pwd:dNNMmR8wUcGezvfBZOO0Qgcwl2m86GP/
a=ice-options:trickle
a=fingerprint:sha-256 9C:97:5C:4C:A9:BE:9E:2F:06:94:F5:BB:38:2C:A1:29:B5:69:B8:FA:94:10:56:1D:0B:5D:80:28:C1:FD:F0:F6
a=setup:active
a=mid:0
a=extmap:1 urn:ietf:params:rtp-hdrext:ssrc-audio-level
a=extmap:2 http://www.webrtc.org/experiments/rtp-hdrext/abs-send-time
a=extmap:3 http://www.ietf.org/id/draft-holmer-rmcat-transport-wide-cc-extensions-01
a=sendrecv
a=rtcp-mux
a=rtpmap:111 opus/48000/2
a=rtcp-fb:111 transport-cc
a=fmtp:111 minptime=10;useinbandfec=1
a=rtpmap:9 G722/8000
a=rtpmap:0 PCMU/8000
a=rtpmap:8 PCMA/8000
a=rtpmap:110 telephone-event/48000
a=rtpmap:126 telephone-event/8000
a=ssrc:3407645770 cname:bg8KQDoIk2UJa6sf
a=ssrc:3407645770 msid:798a9670-c0d6-47a8-925e-5f082ef4d8a0 audio#nuxVMf9EAJX
a=ssrc:3407645770 mslabel:798a9670-c0d6-47a8-925e-5f082ef4d8a0
a=ssrc:3407645770 label:audio#nuxVMf9EAJX
```

### User-initiated sample SDP structures

#### Sample SDP offer structure

```https
v=0
o=- 7602563789789945080 2 IN IP4 127.0.0.1
s=-
t=0 0
a=group:BUNDLE audio
a=msid-semantic: WMS 6932bc1c-db1a-4abe-b437-0c4168be8a13
a=ice-lite
m=audio 40012 UDP/TLS/RTP/SAVPF 111 126
c=IN IP4 31.13.65.60
a=rtcp:9 IN IP4 0.0.0.0
a=candidate:1972637320 1 udp 2113937151 31.13.65.60 40012 typ host generation 0 network-cost 50 ufrag 6k2qP1R6kBfI/2
a=candidate:1652262791 1 udp 2113939711 2a03:2880:f211:cf:face:b00c:0:6443 40012 typ host generation 0 network-cost 50 ufrag 6k2qP1R6kBfI/2
a=ice-ufrag:6k2qP1R6kBfI/2
a=ice-pwd:UApvJw3NcwFRDvIMKdM0vWCdlXah25E9
a=fingerprint:sha-256 1B:B6:6B:40:A5:0B:8C:75:0D:8C:CB:90:2F:99:74:1E:26:45:AE:AF:45:C1:51:60:8F:73:C9:2D:10:6D:8A:88
a=setup:actpass
a=mid:audio
a=extmap:1 urn:ietf:params:rtp-hdrext:ssrc-audio-level
a=extmap:2 http://www.webrtc.org/experiments/rtp-hdrext/abs-send-time
a=extmap:3 http://www.ietf.org/id/draft-holmer-rmcat-transport-wide-cc-extensions-01
a=sendrecv
a=rtcp-mux
a=rtpmap:111 opus/48000/2
a=rtcp-fb:111 transport-cc
a=fmtp:111 minptime=10;useinbandfec=1
a=rtpmap:126 telephone-event/8000
a=ssrc:4208138518 cname:gAXq2V9TKltrnapv
a=ssrc:4208138518 msid:6932bc1c-db1a-4abe-b437-0c4168be8a13 audio#R5wfXFcdmT6
a=ssrc:4208138518 mslabel:6932bc1c-db1a-4abe-b437-0c4168be8a13
a=ssrc:4208138518 label:audio#R5wfXFcdmT6
```

#### Sample SDP answer structure

```https
v=0
o=- 2822644248144643933 2 IN IP4 127.0.0.1
s=-
t=0 0
a=group:BUNDLE audio
a=msid-semantic: WMS eb909cf0-87f0-4358-a4c9-7861680d9431
m=audio 9 UDP/TLS/RTP/SAVPF 111 126
c=IN IP4 0.0.0.0
a=rtcp:9 IN IP4 0.0.0.0
a=ice-ufrag:X1ho
a=ice-pwd:7fJSbV2N5qWiA5QiDKwK3vuh
a=fingerprint:sha-256 2E:35:9F:21:9E:63:72:E5:42:74:76:2D:B3:70:F7:CB:24:14:9B:14:52:71:05:48:DA:4D:67:31:09:58:2A:ED
a=setup:active
a=mid:audio
a=extmap:1 urn:ietf:params:rtp-hdrext:ssrc-audio-level
a=extmap:2 http://www.webrtc.org/experiments/rtp-hdrext/abs-send-time
a=extmap:3 http://www.ietf.org/id/draft-holmer-rmcat-transport-wide-cc-extensions-01
a=sendrecv
a=rtcp-mux
a=rtpmap:111 opus/48000/2
a=rtcp-fb:111 transport-cc
a=fmtp:111 minptime=10;useinbandfec=1
a=rtpmap:126 telephone-event/8000
a=ssrc:330833028 cname:EDc1JutBl8rwHQc2
a=ssrc:330833028 msid:eb909cf0-87f0-4358-a4c9-7861680d9431 ea478c16-d9f7-493c-8cec-19bfac750a36
```

## Sample cURL requests

#### New call

```https
curl -i -X POST &#039;https://graph.facebook.com/v14.0/1234567890/calls&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer EAADUMAze4GIBO1B7B.....&lt;REPLACE_WITH_YOUR_TOKEN&gt;&#039; \
-d &#039;&#123;
   &quot;messaging_product&quot;: &quot;whatsapp&quot;,
   &quot;to&quot;: &quot;14085550000&quot;,
   &quot;recipient&quot;: &quot;US.13491208655302741918&quot;,
   &quot;session&quot;: &#123;
       &quot;sdp&quot;: &quot;v=0\r\no=- 7669997803033704573 2 IN IP4 127.0.0.1\r\ns=-\r\nt=0 0\r\na=group:BUNDLE 0\r\na=extmap-allow-mixed\r\na=msid-semantic: WMS 3c28addc-03b7-4170-b5cd-535bfe767e75\r\nm=audio 9 UDP/TLS/RTP/SAVPF 111 63 9 0 8 110 126\r\nc=IN IP4 0.0.0.0\r\na=rtcp:9 IN IP4 0.0.0.0\r\na=ice-ufrag:6O0H\r\na=ice-pwd:TYCbtfOrBMPpfxFRgSbYnuTI\r\na=ice-options:trickle\r\na=fingerprint:sha-256 9F:45:2C:A8:C3:C0:CC:9B:59:4F:D1:02:56:52:FA:36:00:BE:C0:79:87:B3:D9:9C:3E:BF:60:98:25:B4:26:FC\r\na=setup:active\r\na=mid:0\r\na=extmap:1 urn:ietf:params:rtp-hdrext:ssrc-audio-level\r\na=extmap:2 http://www.webrtc.org/experiments/rtp-hdrext/abs-send-time\r\na=extmap:3 http://www.ietf.org/id/draft-holmer-rmcat-transport-wide-cc-extensions-01\r\na=extmap:4 urn:ietf:params:rtp-hdrext:sdes:mid\r\na=sendrecv\r\na=msid:3c28addc-03b7-4170-b5cd-535bfe767e75 38c455bc-3727-4129-b336-8cd2c6a68486\r\na=rtcp-mux\r\na=rtcp-rsize\r\na=rtpmap:111 opus/48000/2\r\na=rtcp-fb:111 transport-cc\r\na=fmtp:111 minptime=10;useinbandfec=1\r\na=rtpmap:63 red/48000/2\r\na=fmtp:63 111/111\r\na=rtpmap:9 G722/8000\r\na=rtpmap:0 PCMU/8000\r\na=rtpmap:8 PCMA/8000\r\na=rtpmap:110 telephone-event/48000\r\na=rtpmap:126 telephone-event/8000\r\na=ssrc:2430753100 cname:MPddPt/R2ioP4vCm\r\na=ssrc:2430753100 msid:3c28addc-03b7-4170-b5cd-535bfe767e75 38c455bc-3727-4129-b336-8cd2c6a68486\r\n&quot;,
       &quot;sdp_type&quot;: &quot;answer&quot;
   &#125;
&#125;&#039;
```

#### Terminate call

```https
curl -i -X POST &#039;https://graph.facebook.com/v14.0/1234567890/calls&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer EAADUMAze4GIBO1B7B.....&lt;REPLACE_WITH_YOUR_TOKEN&gt;&#039; \
-d &#039;&#123;
   &quot;messaging_product&quot;: &quot;whatsapp&quot;,
   &quot;action&quot;: &quot;terminate&quot;,
   &quot;call_id&quot;: &quot;wacid.HBgLMTY1MDMxMzM5NzQVAgARGCBFRjNEODRBM0Q3NDZDM0Q0QzI4MzAwQjZBRkZGODM3NhwYCzEyMjQ1NTU0NDg5FQIAAA&quot;
&#125;&#039;
```

#### Accept call

```https
curl -i -X POST &#039;https://graph.facebook.com/v14.0/1234567890/calls&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer EAADUMAze4GIBO1B7B.....&lt;REPLACE_WITH_YOUR_TOKEN&gt;&#039; \
-d &#039;&#123;
 &quot;messaging_product&quot;: &quot;whatsapp&quot;,
 &quot;to&quot;: &quot;14085550000&quot;,
 &quot;action&quot;: &quot;accept&quot;,
 &quot;call_id&quot;: &quot;wacid.HBgLMTY1MDMxMzM5NzQVAgASGCA5ODkyMDk2RkM2NUM1QTYwRkM4NjFDQzk0NkQwNDBCRRwYCzEyMjQ1NTU0NDg5FQIAAA==&quot;,
 &quot;session&quot;: &#123;
     &quot;sdp&quot;: &quot;v=0\r\no=- 7669997803033704573 2 IN IP4 127.0.0.1\r\ns=-\r\nt=0 0\r\na=group:BUNDLE 0\r\na=extmap-allow-mixed\r\na=msid-semantic: WMS 3c28addc-03b7-4170-b5cd-535bfe767e75\r\nm=audio 9 UDP/TLS/RTP/SAVPF 111 63 9 0 8 110 126\r\nc=IN IP4 0.0.0.0\r\na=rtcp:9 IN IP4 0.0.0.0\r\na=ice-ufrag:6O0H\r\na=ice-pwd:TYCbtfOrBMPpfxFRgSbYnuTI\r\na=ice-options:trickle\r\na=fingerprint:sha-256 9F:45:2C:A8:C3:C0:CC:9B:59:4F:D1:02:56:52:FA:36:00:BE:C0:79:87:B3:D9:9C:3E:BF:60:98:25:B4:26:FC\r\na=setup:active\r\na=mid:0\r\na=extmap:1 urn:ietf:params:rtp-hdrext:ssrc-audio-level\r\na=extmap:2 http://www.webrtc.org/experiments/rtp-hdrext/abs-send-time\r\na=extmap:3 http://www.ietf.org/id/draft-holmer-rmcat-transport-wide-cc-extensions-01\r\na=extmap:4 urn:ietf:params:rtp-hdrext:sdes:mid\r\na=sendrecv\r\na=msid:3c28addc-03b7-4170-b5cd-535bfe767e75 38c455bc-3727-4129-b336-8cd2c6a68486\r\na=rtcp-mux\r\na=rtcp-rsize\r\na=rtpmap:111 opus/48000/2\r\na=rtcp-fb:111 transport-cc\r\na=fmtp:111 minptime=10;useinbandfec=1\r\na=rtpmap:63 red/48000/2\r\na=fmtp:63 111/111\r\na=rtpmap:9 G722/8000\r\na=rtpmap:0 PCMU/8000\r\na=rtpmap:8 PCMA/8000\r\na=rtpmap:110 telephone-event/48000\r\na=rtpmap:126 telephone-event/8000\r\na=ssrc:2430753100 cname:MPddPt/R2ioP4vCm\r\na=ssrc:2430753100 msid:3c28addc-03b7-4170-b5cd-535bfe767e75 38c455bc-3727-4129-b336-8cd2c6a68486\r\n&quot;,
     &quot;sdp_type&quot;: &quot;answer&quot;
 &#125;
&#125;&#039;
```

#### New call (using legacy connection param)

```https
curl -i -X POST &#039;https://graph.facebook.com/v14.0/123456789/calls&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer EAADUMAze4GIBO1B7B.....&lt;REPLACE_WITH_YOUR_TOKEN&gt;&#039; \
-d &#039;&#123;
   &quot;messaging_product&quot;: &quot;whatsapp&quot;,
   &quot;to&quot;: &quot;14085550000&quot;,
   &quot;connection&quot;: &#123;
       &quot;webrtc&quot;: &#123;
           &quot;sdp&quot;: &quot;&#123;\&quot;sdp\&quot;:\&quot;v=0\\r\\no=- 6314352886888624490 2 IN IP4 127.0.0.1\\r\\ns=-\\r\\nt=0 0\\r\\na=group:BUNDLE 0\\r\\na=extmap-allow-mixed\\r\\na=msid-semantic: WMS ccd3f422-8d7d-49c9-936c-a152979ee4fa\\r\\nm=audio 9 UDP/TLS/RTP/SAVPF 111 63 9 0 8 110 126\\r\\nc=IN IP4 0.0.0.0\\r\\na=rtcp:9 IN IP4 0.0.0.0\\r\\na=ice-ufrag:/PSS\\r\\na=ice-pwd:buBIz+JlbmakiCT7JdJIq/j0\\r\\na=ice-options:trickle\\r\\na=fingerprint:sha-256 43:08:34:16:67:E3:D9:A2:F5:AA:6A:AE:03:97:C8:D5:B8:F2:4B:40:79:C8:1A:44:53:69:4B:9C:89:88:D7:22\\r\\na=setup:active\\r\\na=mid:0\\r\\na=extmap:1 urn:ietf:params:rtp-hdrext:ssrc-audio-level\\r\\na=extmap:2 http://www.webrtc.org/experiments/rtp-hdrext/abs-send-time\\r\\na=extmap:3 http://www.ietf.org/id/draft-holmer-rmcat-transport-wide-cc-extensions-01\\r\\na=extmap:4 urn:ietf:params:rtp-hdrext:sdes:mid\\r\\na=sendrecv\\r\\na=msid:ccd3f422-8d7d-49c9-936c-a152979ee4fa 4e58b2a9-c864-4752-8f4f-23f9ced35971\\r\\na=rtcp-mux\\r\\na=rtcp-rsize\\r\\na=rtpmap:111 opus/48000/2\\r\\na=rtcp-fb:111 transport-cc\\r\\na=fmtp:111 minptime=10;useinbandfec=1\\r\\na=rtpmap:63 red/48000/2\\r\\na=fmtp:63 111/111\\r\\na=rtpmap:9 G722/8000\\r\\na=rtpmap:0 PCMU/8000\\r\\na=rtpmap:8 PCMA/8000\\r\\na=rtpmap:110 telephone-event/48000\\r\\na=rtpmap:126 telephone-event/8000\\r\\na=ssrc:3354317731 cname:zgqSj/r4rlErlW23\\r\\na=ssrc:3354317731 msid:ccd3f422-8d7d-49c9-936c-a152979ee4fa 4e58b2a9-c864-4752-8f4f-23f9ced35971\\r\\n\&quot;,\&quot;type\&quot;:\&quot;offer\&quot;&#125;&quot;
       &#125;
   &#125;
&#125;&#039;
```

## Sample call connect webhook

#### Call connect webhook

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
                               &quot;session&quot;: &#123;
                                   &quot;sdp_type&quot;: &quot;answer&quot;,
                                   &quot;sdp&quot;: &quot;v=0\r\no=- 8076734947255960322 2 IN IP4 127.0.0.1\r\ns=-\r\nt=0 0\r\na=group:BUNDLE 0\r\na=extmap-allow-mixed\r\na=msid-semantic: WMS 68a296ba-41cc-41db-8edb-3ddf4dbbb483\r\na=ice-lite\r\nm=audio 3482 UDP/TLS/RTP/SAVPF 111 9 0 8 110 126\r\nc=IN IP4 31.13.65.130\r\na=rtcp:9 IN IP4 0.0.0.0\r\na=candidate:2754936280 1 udp 2113937151 31.13.65.130 3482 typ host generation 0 network-cost 50 ufrag kv6Jn8vBmEds/8\r\na=candidate:1581496399 1 udp 2113939711 2a03:2880:f211:d1:face:b00c:0:699c 3482 typ host generation 0 network-cost 50 ufrag kv6Jn8vBmEds/8\r\na=ice-ufrag:kv6Jn8vBmEds/8\r\na=ice-pwd:OhY8sT7v6PJe3bbs0Yx2TC/oPb5oatnK\r\na=ice-options:trickle\r\na=fingerprint:sha-256 46:14:2B:31:B1:9D:AF:15:81:E2:EF:45:B1:2B:96:3D:64:0E:63:F1:CC:9A:BD:88:D6:32:8F:E9:2A:13:3A:38\r\na=setup:active\r\na=mid:0\r\na=extmap:1 urn:ietf:params:rtp-hdrext:ssrc-audio-level\r\na=extmap:2 http://www.webrtc.org/experiments/rtp-hdrext/abs-send-time\r\na=extmap:3 http://www.ietf.org/id/draft-holmer-rmcat-transport-wide-cc-extensions-01\r\na=sendrecv\r\na=rtcp-mux\r\na=rtpmap:111 opus/48000/2\r\na=rtcp-fb:111 transport-cc\r\na=fmtp:111 minptime=10;useinbandfec=1\r\na=rtpmap:9 G722/8000\r\na=rtpmap:0 PCMU/8000\r\na=rtpmap:8 PCMA/8000\r\na=rtpmap:110 telephone-event/48000\r\na=rtpmap:126 telephone-event/8000\r\na=ssrc:433528572 cname:VBDcSNi/cg1Wg6D3\r\na=ssrc:433528572 msid:68a296ba-41cc-41db-8edb-3ddf4dbbb483 audio#wx3mq6BITjB\r\na=ssrc:433528572 mslabel:68a296ba-41cc-41db-8edb-3ddf4dbbb483\r\na=ssrc:433528572 label:audio#wx3mq6BITjB\r\n&quot;
                               &#125;,
                               &quot;from&quot;: &quot;15551112222&quot;,
                               &quot;connection&quot;: &#123;
                                   &quot;webrtc&quot;: &#123;
                                       &quot;sdp&quot;: &quot;&#123;\&quot;sdp\&quot;:\&quot;v=0\\r\\no=- 8076734947255960322 2 IN IP4 127.0.0.1\\r\\ns=-\\r\\nt=0 0\\r\\na=group:BUNDLE 0\\r\\na=extmap-allow-mixed\\r\\na=msid-semantic: WMS 68a296ba-41cc-41db-8edb-3ddf4dbbb483\\r\\na=ice-lite\\r\\nm=audio 3482 UDP/TLS/RTP/SAVPF 111 9 0 8 110 126\\r\\nc=IN IP4 31.13.65.130\\r\\na=rtcp:9 IN IP4 0.0.0.0\\r\\na=candidate:2754936280 1 udp 2113937151 31.13.65.130 3482 typ host generation 0 network-cost 50 ufrag kv6Jn8vBmEds/8\\r\\na=candidate:1581496399 1 udp 2113939711 2a03:2880:f211:d1:face:b00c:0:699c 3482 typ host generation 0 network-cost 50 ufrag kv6Jn8vBmEds/8\\r\\na=ice-ufrag:kv6Jn8vBmEds/8\\r\\na=ice-pwd:OhY8sT7v6PJe3bbs0Yx2TC/oPb5oatnK\\r\\na=ice-options:trickle\\r\\na=fingerprint:sha-256 46:14:2B:31:B1:9D:AF:15:81:E2:EF:45:B1:2B:96:3D:64:0E:63:F1:CC:9A:BD:88:D6:32:8F:E9:2A:13:3A:38\\r\\na=setup:active\\r\\na=mid:0\\r\\na=extmap:1 urn:ietf:params:rtp-hdrext:ssrc-audio-level\\r\\na=extmap:2 http://www.webrtc.org/experiments/rtp-hdrext/abs-send-time\\r\\na=extmap:3 http://www.ietf.org/id/draft-holmer-rmcat-transport-wide-cc-extensions-01\\r\\na=sendrecv\\r\\na=rtcp-mux\\r\\na=rtpmap:111 opus/48000/2\\r\\na=rtcp-fb:111 transport-cc\\r\\na=fmtp:111 minptime=10;useinbandfec=1\\r\\na=rtpmap:9 G722/8000\\r\\na=rtpmap:0 PCMU/8000\\r\\na=rtpmap:8 PCMA/8000\\r\\na=rtpmap:110 telephone-event/48000\\r\\na=rtpmap:126 telephone-event/8000\\r\\na=ssrc:433528572 cname:VBDcSNi/cg1Wg6D3\\r\\na=ssrc:433528572 msid:68a296ba-41cc-41db-8edb-3ddf4dbbb483 audio#wx3mq6BITjB\\r\\na=ssrc:433528572 mslabel:68a296ba-41cc-41db-8edb-3ddf4dbbb483\\r\\na=ssrc:433528572 label:audio#wx3mq6BITjB\\r\\n\&quot;,\&quot;type\&quot;:\&quot;answer\&quot;&#125;&quot;
                                   &#125;
                               &#125;,
                               &quot;id&quot;: &quot;wacid.HBgLMTY1MDMxMzM5NzQVAgARGCAwQTJCRDYwNkEzQUNCQUVCMEFGMzYzRTYxNjMxMDdFMxwYCzE0MDg1NTUyODk5FQIAAA==&quot;,
                               &quot;to&quot;: &quot;16501230000&quot;,
                               &quot;to_user_id&quot;: &quot;&lt;BSUID&gt;&quot;,
                               &quot;to_parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot;,
                               &quot;event&quot;: &quot;connect&quot;,
                               &quot;timestamp&quot;: &quot;1724467313&quot;,
                               &quot;direction&quot;: &quot;BUSINESS_INITIATED&quot;
                           &#125;
                       ],
                       &quot;contacts&quot;: [
                           &#123;
                               &quot;profile&quot;: &#123;
                                   &quot;name&quot;: &quot;&lt;CALLEE_NAME&gt;&quot;,
                                   &quot;username&quot;: &quot;&lt;USERNAME&gt;&quot;
                               &#125;,
                               &quot;wa_id&quot;: &quot;16501230000&quot;,
                               &quot;user_id&quot;: &quot;&lt;BSUID&gt;&quot;,
                               &quot;parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot;
                           &#125;
                       ],
                       &quot;metadata&quot;: &#123;
                           &quot;phone_number_id&quot;: &quot;105615555715855&quot;,
                           &quot;display_phone_number&quot;: &quot;15551112222&quot;
                       &#125;,
                       &quot;messaging_product&quot;: &quot;whatsapp&quot;
                   &#125;
               &#125;
           ],
           &quot;id&quot;: &quot;112735964992110&quot;
       &#125;
   ],
   &quot;object&quot;: &quot;whatsapp_business_account&quot;
&#125;
```
