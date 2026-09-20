# Configure Call Settings


Calling is not enabled by default on a business phone number. To enable calling, you must have a [messaging limit](https://developers.facebook.com/documentation/business-messaging/whatsapp/messaging-limits) of 2000 or above.

Use these endpoints to view and configure call settings for the Calling API.

You can also [configure session initiation protocol (SIP)](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/sip) for call signaling instead of using Graph API endpoint calls and webhooks.

## Configure or update business phone number calling settings

Use this endpoint to update call settings configuration for an individual business phone number.

### WhatsApp clients reflecting latest calling config

After you update call configuration, WhatsApp users may take up to 7 days to reflect those changes. Most users refresh much sooner. You can force an immediate refresh in WhatsApp by entering your business chat window and opening the chat info page. Regardless of WhatsApp client behavior, the server still honors the configured settings.

### Request syntax

```html
POST /&lt;PHONE_NUMBER_ID&gt;/settings
```

### Endpoint parameters

| Placeholder | Description | Sample Value |
| --- | --- | --- |
| `&lt;PHONE_NUMBER_ID&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | **Required**&lt;br&gt;&lt;br&gt;ID of the business phone number whose Calling API settings you&#039;re updating. | `106540352242922` |

### Request body

```html
&#123;
  &quot;calling&quot;: &#123;
    &quot;status&quot;: &quot;ENABLED&quot;,
    &quot;call_icon_visibility&quot;: &quot;DEFAULT&quot;,
    &quot;call_icons&quot;: &#123;
      &quot;restrict_to_user_countries&quot;: [
        &quot;US&quot;,
        &quot;BR&quot;
      ]
    &#125;,
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
          &quot;hostname&quot;: &lt;SIP_SERVER_HOSTNAME&gt;,
          &quot;port&quot;: SIP_SERVER_PORT,
          &quot;request_uri_user_params&quot;: &#123;
            &quot;KEY1&quot;: &quot;VALUE1&quot;,
            &quot;KEY2&quot;: &quot;VALUE2&quot;
          &#125;
        &#125;
      ]
    &#125;,
    &quot;audio&quot;: &#123;
      &quot;additional_codecs&quot;: [&quot;PCMA&quot;, &quot;PCMU&quot;]
    &#125;,
    &quot;voicemail&quot;: &#123;
      &quot;status&quot;: &quot;ENABLED&quot;,
      &quot;triggers&quot;: [
        &quot;REJECT&quot;,
        &quot;TIMEOUT&quot;
      ],
      &quot;audio&quot;: &#123;
        &quot;default&quot;: &#123;
          &quot;announcement_media_id&quot;: 938884519013664,
          &quot;timeout_seconds&quot;: 20
        &#125;
      &#125;
    &#125;
  &#125;
&#125;
```

### Body parameters

| Parameter | Description | Sample Value |
| --- | --- | --- |
| `status`&lt;br&gt;&lt;br&gt;_String_ | **Optional**&lt;br&gt;&lt;br&gt;Enable or disable calling on this phone number. | `&quot;ENABLED&quot;`&lt;br&gt;&lt;br&gt;`&quot;DISABLED&quot;` |
| `call_icon_visibility`&lt;br&gt;&lt;br&gt;_String_ | **Optional**&lt;br&gt;&lt;br&gt;Configure whether WhatsApp shows the call button icon to WhatsApp users when they chat with your business.&lt;br&gt;&lt;br&gt;[View call icon visibility behavior details below](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/call-settings#call-icons) | [View call icon visibility behavior details below](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/call-settings#call-icons) |
| `call_icons`&lt;br&gt;&lt;br&gt;_String_ | **Optional**&lt;br&gt;&lt;br&gt;Configure whether WhatsApp call button icon displays for WhatsApp users when chatting with your business.&lt;br&gt;&lt;br&gt;[View call icons visibility behavior details below](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/call-settings#call-icons) | [View call icons behavior details below](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/call-settings#call-icons) |
| `call_hours`&lt;br&gt;&lt;br&gt;_JSON object_ | **Optional**&lt;br&gt;&lt;br&gt;Allows you to specify and trigger call settings for incoming calls based on your timezone, business operating hours, and holiday schedules.&lt;br&gt;&lt;br&gt;Any previously configured values in `call_hours` will be replaced with the values passed in the request body of this API call.&lt;br&gt;&lt;br&gt;[View call hours behavior details below](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/call-settings#call-hours) | [View call hours behavior details below](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/call-settings#call-hours) |
| `callback_permission_status`&lt;br&gt;&lt;br&gt;_String_ | **Optional**&lt;br&gt;&lt;br&gt;Configure whether a WhatsApp user is prompted with a call permission request after calling your business.&lt;br&gt;&lt;br&gt;Note: The call permission request is triggered by either a missed or connected call.&lt;br&gt;&lt;br&gt;[View callback permission status behavior details below](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/call-settings#callback-permissions) | `&quot;ENABLED&quot;`&lt;br&gt;&lt;br&gt;`&quot;DISABLED&quot;` |
| `sip`&lt;br&gt;&lt;br&gt;_JSON object_ | **Optional**&lt;br&gt;&lt;br&gt;Configure call signaling via session initiation protocol (SIP).&lt;br&gt;&lt;br&gt;**Note: When SIP is enabled, you cannot use calling related endpoints and will not receive calling related webhooks.**&lt;br&gt;&lt;br&gt;[Learn how to configure and use SIP call signaling](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/sip) | View [Configure SIP settings on business phone number](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/sip#configure-or-update-sip-settings-on-business-phone-number) |
| `audio`&lt;br&gt;&lt;br&gt;_JSON object_ | **Optional**&lt;br&gt;&lt;br&gt;Configure call audio codec settings. Opus is the default codec and is always present.&lt;br&gt;&lt;br&gt;[View audio codec details below](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/call-settings#audio-codec) | ```json
&quot;audio&quot;: &#123;
  &quot;additional_codecs&quot;: [
    &quot;PCMA&quot;, &quot;PCMU&quot;
  ]
&#125;
``` |
| `voicemail`&lt;br&gt;&lt;br&gt;_JSON object_ | **Optional**&lt;br&gt;&lt;br&gt;Configure voicemail collection for missed or rejected user-initiated calls.&lt;br&gt;&lt;br&gt;[View voicemail details below](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/call-settings#voicemail) | ```json
&quot;voicemail&quot;: &#123;
  &quot;status&quot;: &quot;ENABLED&quot;,
  &quot;triggers&quot;: [
    &quot;REJECT&quot;,
    &quot;TIMEOUT&quot;
  ],
  &quot;audio&quot;: &#123;
    &quot;default&quot;: &#123;
      &quot;announcement_media_id&quot;: 938884519013664,
      &quot;timeout_seconds&quot;: 20
    &#125;
  &#125;
&#125;
``` |

### Calling status

When the `status` parameter is set to `&quot;ENABLED&quot;`, calling features are enabled for the business phone number. WhatsApp client apps render the call button icon in both the business chat and business chat profile.

When the `status` parameter is set to `&quot;DISABLED&quot;`, calling features are **disabled**, and both the business chat and business chat profile **do not display the call button icon.**

Updates to `status` update the call button icon in existing business chats in near real-time when the business phone number is in the WhatsApp user&#039;s contacts.

Otherwise, updates are real-time for a limited number of users in conversation with the business, and are eventual for the rest of the conversations.

#### Call button icon visibility

When Calling API features are enabled for a business number, you can still choose whether to show the call button icon or not by using the `call_icon_visibility` parameter. Note: Disabling call button icon visibility **does not** disable a WhatsApp user&#039;s ability to make unsolicited calls to your business.

The behavior for supported options is as follows:

`DEFAULT`

WhatsApp displays the call button icon in the chat menu bar and the business info page, allowing WhatsApp users to make unsolicited calls to the business.

`DISABLE_ALL`

The call button icon is hidden in the chat menu bar and the business info page, and all other entry points external to the chat are also disabled. WhatsApp users cannot make unsolicited calls to your business.

You can still [send interactive messages](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/call-button-messages-deep-links#send-interactive-message-with-a-whatsapp-call-button) or [template messages](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/call-button-messages-deep-links#create-and-send-whatsapp-call-button-template-message) with a Calling API CTA button.

### Callback permissions

Calling a WhatsApp user requires explicit permission from the user. One way to obtain calling permissions is to request permission when a WhatsApp user calls your business.

You can configure the call permission UI to automatically show in the WhatsApp user&#039;s client app when they call your business number. The user may change their permission selection at any time.

### Call icons

With the `call_icons` setting, you can specify the countries where these icons should show up.

```json
&quot;call_icons&quot;: &#123;
  &quot;restrict_to_user_countries&quot;: [
    &quot;US&quot;,
    &quot;BR&quot;
  ]
&#125;
```

| Parameter | Description | Sample Values |
| --- | --- | --- |
| `restrict_to_user_countries`&lt;br&gt;&lt;br&gt;_List of Strings_ | **Optional**&lt;br&gt;&lt;br&gt;Restrict the visibility of call icons to these countries.&lt;br&gt;&lt;br&gt;_NOTE: For example, if you restrict `restrict_to_user_countries` to &quot;US,&quot; then it will apply to all the people who have a US registered phone number. These people could be physically located inside or outside of the USA._ | Restrict to US and Brazil:&lt;br&gt;&lt;br&gt;```json
&quot;restrict_to_user_countries&quot;: [
  &quot;US&quot;,
  &quot;BR&quot;
]
```&lt;br&gt;&lt;br&gt;No restriction:&lt;br&gt;&lt;br&gt;```json
&quot;restrict_to_user_countries&quot;: []
``` |

### Call hours

With the `call_hours` setting, you can specify the timezone, business operating hours, and holiday schedules that will be enforced for all user-initiated calls.

Configuring this setting restricts calls only to available weekly hours you configure. User-initiated calls are unavailable outside of the weekly hours and holiday schedules you set.

The WhatsApp client app shows WhatsApp users an option to chat with the business, or request a callback, if `callback_permission_status` is `ENABLED`. The user will also be shown the next available calling slot on the option screen.

```json
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
&#125;
```

| Parameter | Description | Sample Values |
| --- | --- | --- |
| `status`&lt;br&gt;&lt;br&gt;_String_ | **Required**&lt;br&gt;&lt;br&gt;Enable or disable the call hours for your business.&lt;br&gt;&lt;br&gt;If call hours are disabled, your business is considered open all 24 hours of the day, 7 days a week. | `&quot;ENABLED&quot;`&lt;br&gt;&lt;br&gt;`&quot;DISABLED&quot;` |
| `timezone_id`&lt;br&gt;&lt;br&gt;_String_ | **Required**&lt;br&gt;&lt;br&gt;The timezone that your business is operating within.&lt;br&gt;&lt;br&gt;[Learn more about supported values for `timezone_id`](https://developers.facebook.com/docs/facebook-business-extension/fbe/reference#time-zones) | `&quot;America/Menominee&quot;`&lt;br&gt;&lt;br&gt;`&quot;Asia/Singapore&quot;` |
| `weekly_operating_hours`&lt;br&gt;&lt;br&gt;_List of JSON objects_ | **Required**&lt;br&gt;&lt;br&gt;The operating hours schedule for each day of the week.&lt;br&gt;&lt;br&gt;Each entry is a JSON object with 3 key-value pairs:&lt;br&gt;&lt;br&gt;`day_of_week` — (_Enum_) **[Required]**&lt;br&gt;&lt;br&gt;The day of the week.&lt;br&gt;&lt;br&gt;Can take one of seven values: `&quot;MONDAY&quot;`, `&quot;TUESDAY&quot;`, `&quot;WEDNESDAY&quot;`, `&quot;THURSDAY&quot;`, `&quot;FRIDAY&quot;`, `&quot;SATURDAY&quot;`, `&quot;SUNDAY&quot;`&lt;br&gt;&lt;br&gt;`open_time` \| `close_time` — (_Integer_) **[Required]**&lt;br&gt;&lt;br&gt;Opening and closing times represented in 24-hour format, for example `&quot;1130&quot;` = 11:30 AM&lt;br&gt;&lt;br&gt;- Maximum of 2 entries allowed per day of week&lt;br&gt;- `open_time` must be before `close_time`&lt;br&gt;- Overlapping entries not allowed | ```json
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
| `holiday_schedule`&lt;br&gt;&lt;br&gt;_List of JSON objects_ | **Optional**&lt;br&gt;&lt;br&gt;An optional override to the weekly schedule.&lt;br&gt;&lt;br&gt;Up to 20 overrides can be specified.&lt;br&gt;&lt;br&gt;Note: If `holiday_schedule` is not passed in the request, then the existing `holiday_schedule` will be deleted and replaced with an empty schedule.&lt;br&gt;&lt;br&gt;`date` — (_String_) **[Required]**&lt;br&gt;&lt;br&gt;Date for which you want to specify the override.&lt;br&gt;&lt;br&gt;YYYY-MM-DD format.&lt;br&gt;&lt;br&gt;`open_time` \| `close_time` — (_Integer_) **[Required]**&lt;br&gt;&lt;br&gt;Opening and closing times represented in 24-hour format, for example, `&quot;1130&quot;` = 11:30 AM&lt;br&gt;&lt;br&gt;- Maximum of 2 entries allowed per day of week&lt;br&gt;- `open_time` must be before `close_time`&lt;br&gt;- Overlapping entries not allowed | ```json
&#123;
&quot;date&quot;: &quot;2026-01-01&quot;,
&quot;start_time&quot;: &quot;0000&quot;,
&quot;end_time&quot;: &quot;2359&quot;
&#125;
...
``` |

### Audio codec

Opus is the default audio codec for all WhatsApp calls. You can enable G.711 (PCMA/PCMU) codecs for interoperability with legacy telephony systems or PSTN gateways.

#### Guidelines and considerations

- **Opus is the recommended codec.** Opus delivers higher audio quality with lower bandwidth usage and is the default for all WhatsApp calls. Use Opus unless you have a specific requirement for G.711.
- **G.711 requires transcoding.** When a G.711 codec is negotiated, audio is transcoded between Opus (on the WhatsApp user side) and G.711 (on the business side), which can add latency to the call.
- **G.711 has lower audio quality.** G.711 encodes audio at a fixed 64 kbps without advanced compression, resulting in lower fidelity compared to Opus.
- **G.711 uses more bandwidth.** G.711 requires approximately 64 kbps per direction, while Opus achieves comparable or better quality at significantly lower bitrates.
- **Use G.711 only when necessary.** The primary use case is interoperability with legacy telephony infrastructure and PSTN gateways that do not support Opus.

```json
&quot;audio&quot;: &#123;
  &quot;additional_codecs&quot;: [&quot;PCMA&quot;, &quot;PCMU&quot;]
&#125;
```

| Parameter | Description | Sample Values |
| --- | --- | --- |
| `additional_codecs`&lt;br&gt;&lt;br&gt;_List of Strings_ | **Optional**&lt;br&gt;&lt;br&gt;Enable additional audio codecs. Supported values: `&quot;PCMA&quot;` (G.711 A-law), `&quot;PCMU&quot;` (G.711 µ-law). Opus is always enabled by default and cannot be removed. After enabling additional codecs, they can be selected during SDP codec negotiation according to RFC 3264. | ```json
&quot;additional_codecs&quot;: [
  &quot;PCMA&quot;,
  &quot;PCMU&quot;
]
```&lt;br&gt;&lt;br&gt;No additional codecs:&lt;br&gt;&lt;br&gt;```json
&quot;additional_codecs&quot;: []
``` |

### Voicemail

When enabled, Cloud API does the following:
- Waits for a configured delay or a reject signal from you
- Automatically answers the call
- Plays an audio announcement
- Records the caller&#039;s voicemail
- Delivers the voicemail as an audio message via webhook

When voicemail is enabled, turn off call hours, because WhatsApp users can&#039;t place calls outside business hours.

Calling must be enabled on the phone number for the `voicemail` setting to take effect.

```html
&quot;voicemail&quot;: &#123;
  &quot;status&quot;: &quot;ENABLED&quot;,
  &quot;triggers&quot;: [
    &quot;REJECT&quot;,
    &quot;TIMEOUT&quot;
  ],
  &quot;audio&quot;: &#123;
    &quot;default&quot;: &#123;
      &quot;announcement_media_id&quot;: &lt;MEDIA_ID&gt;,
      &quot;timeout_seconds&quot;: 20
    &#125;
  &#125;
&#125;
```

| Parameter | Description | Sample Values |
| --- | --- | --- |
| `status`&lt;br&gt;&lt;br&gt;_String_ | **Required**&lt;br&gt;&lt;br&gt;Enable or disable the voicemail feature for the business phone number. Disabled by default.&lt;br&gt;&lt;br&gt;Calling must be enabled on the phone number, otherwise the voicemail setting is ignored. | `&quot;ENABLED&quot;`&lt;br&gt;&lt;br&gt;`&quot;DISABLED&quot;` |
| `triggers`&lt;br&gt;&lt;br&gt;_List of Strings_ | **Required when `status` is `ENABLED`**&lt;br&gt;&lt;br&gt;Events that trigger voicemail collection. At least one trigger must be specified when voicemail is enabled. Supported values:&lt;br&gt;&lt;br&gt;- `REJECT` — you reject the incoming call.&lt;br&gt;- `TIMEOUT` — you do not accept or reject the call within the configured `timeout_seconds`. | ```json
&quot;triggers&quot;: [
  &quot;REJECT&quot;,
  &quot;TIMEOUT&quot;
]
``` |
| `audio`&lt;br&gt;&lt;br&gt;_JSON object_ | **Required when `status` is `ENABLED`**&lt;br&gt;&lt;br&gt;Voicemail audio configuration.&lt;br&gt;&lt;br&gt;`default` _(JSON object)_ **[Required]** — Default voicemail configuration applied to all WhatsApp users.&lt;br&gt;&lt;br&gt;The `default` configuration accepts the following fields:&lt;br&gt;&lt;br&gt;`announcement_media_id` _(Integer)_ **[Required when `status` is `ENABLED`]** — ID of an uploaded media file played to the WhatsApp user as the voicemail announcement. Upload the file via the [Media Upload API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/media-upload-api) with `use_case=call_voicemail_announcement`. The media file must satisfy the following:&lt;br&gt;&lt;br&gt;- Duration must be less than 60 seconds.&lt;br&gt;- MIME type must be `audio/ogg` with the OPUS codec.&lt;br&gt;- The media must be uploaded with `use_case=call_voicemail_announcement` so it is exempt from the standard 30-day media TTL.&lt;br&gt;&lt;br&gt;`timeout_seconds` _(Integer)_ **[Required when `TIMEOUT` trigger is used]** — Time in seconds after the call starts ringing before the voicemail announcement and recording begin. Only applies to the `TIMEOUT` trigger. Must be between `0` and `30` seconds inclusive. If the `TIMEOUT` trigger is configured without `timeout_seconds`, the trigger is disabled. | ```html
&quot;audio&quot;: &#123;
  &quot;default&quot;: &#123;
    &quot;announcement_media_id&quot;: &lt;MEDIA_ID&gt;,
    &quot;timeout_seconds&quot;: 20
  &#125;
&#125;
``` |

#### Upload a voicemail announcement media file

Voicemail announcement audio files must be uploaded through the [Media Upload API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/media-upload-api) with the `use_case` parameter set to `call_voicemail_announcement`. This skips the standard 30-day TTL applied to messaging media so that announcements remain available for the lifetime of the configuration.

```html
POST /&lt;PHONE_NUMBER_ID&gt;/media
```

Form data parameters:

- `file=&#064;&lt;FILE_PATH&gt;;type=audio/ogg`
- `messaging_product=whatsapp`
- `use_case=call_voicemail_announcement`
- `description=&quot;Default announcement (English)&quot;`

Media uploaded with `use_case=call_voicemail_announcement` can only be used as a voicemail announcement and cannot be sent as a regular message.

### Success response

```json
&#123;
  &quot;success&quot;: true
&#125;
```

### Error response

Possible errors that can occur:

- Permissions/Authorization errors
- Invalid status
- Invalid schedule for `call_hours`
- Holiday given in `call_hours` is a past date
- Timezone is invalid in `call_hours`
- `weekly_operating_hours` in `call_hours` cannot be empty
- Date format in `holiday_schedule` for call_hours is invalid
- More than 2 entries not allowed in `weekly_operating_hours` schedule in `call_hours`
- Overlapping schedule in `call_hours` is not allowed

[View Calling API Error Codes and Troubleshooting for more information](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/troubleshooting)

[View general Cloud API Error Codes here](https://developers.facebook.com/documentation/business-messaging/whatsapp/support/error-codes)

## Get phone number calling settings

Use this endpoint to check the configuration of your Calling API feature settings.

This endpoint can return information for other Cloud API feature settings.

### Request syntax

```html
GET /&lt;PHONE_NUMBER_ID&gt;/settings
```

### Endpoint parameters

| Parameter | Description | Sample Value |
| --- | --- | --- |
| `&lt;PHONE_NUMBER_ID&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | **Required**&lt;br&gt;&lt;br&gt;ID of the business phone number for which you are getting Calling API settings. | `106540352242922` |

### App permission required

`whatsapp_business_management`: Advanced access is required to use the API for end business clients

### Response body

```html
&#123;
  &quot;calling&quot;: &#123;
    &quot;status&quot;: &quot;ENABLED&quot;,
    &quot;call_icon_visibility&quot;: &quot;DEFAULT&quot;,
    &quot;callback_permission_status&quot;: &quot;ENABLED&quot;,
    &quot;call_hours&quot;: &#123;
      &quot;status&quot;: &quot;ENABLED&quot;,
      &quot;timezone_id&quot;: &quot;[REDACTED]&quot;,
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
    &quot;sip&quot;: &#123;
      &quot;status&quot;: &quot;ENABLED&quot;,
      &quot;servers&quot;: [
        &#123;
          &quot;hostname&quot;: &quot;[REDACTED]&quot;,
          &quot;sip_user_password&quot;: &quot;[REDACTED]&quot;
        &#125;
      ]
    &#125;,
    &quot;audio&quot;: &#123;
      &quot;additional_codecs&quot;: [&quot;PCMA&quot;, &quot;PCMU&quot;]
    &#125;,
    &quot;voicemail&quot;: &#123;
      &quot;status&quot;: &quot;ENABLED&quot;,
      &quot;triggers&quot;: [
        &quot;REJECT&quot;,
        &quot;TIMEOUT&quot;
      ],
      &quot;audio&quot;: &#123;
        &quot;default&quot;: &#123;
          &quot;announcement_media_id&quot;: &lt;MEDIA_ID&gt;,
          &quot;timeout_seconds&quot;: 20
        &#125;
      &#125;
    &#125;
  &#125;,
  &lt;Other non-calling feature configuration...&gt;
&#125;
```

### Include SIP user password in response

Optionally, you can include SIP user credentials in your response body by adding the SIP credentials query parameter in the POST request:

```html
GET /&lt;PHONE_NUMBER_ID&gt;/settings?include_sip_credentials=true
```

Where the response will look like this:

```json
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

### Response details

The [Settings API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/settings-api#get-version-phone-number-id-settings) returns Calling API settings, along with other configuration information for your WhatsApp Business phone number.

[Learn more about Calling API settings and their values](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/call-settings#body-parameters)

#### Response with calling restrictions

If your business has restrictions enforced, the response body contains information about the restriction along with other calling API settings.

```json
 &#123;
   &quot;calling&quot;: &#123;
     ... // other calling api settings
     &quot;restrictions&quot;: &#123;
       &quot;restrictions_list&quot;: [
         &#123;
           &quot;type&quot;: &quot;[RESTRICTED_BUSINESS_INITIATED_CALLING|RESTRICTED_USER_INITIATED_CALLING]&quot;,
           &quot;reason&quot;: &quot;Business|User initiated calling capability has been temporarily disabled for this phone number due to high negative feedback from users.&quot;,
           &quot;expiration&quot;: 1754072386
         &#125;
       ]
     &#125;
   &#125;
&#125;
```

| Parameter | Description |
| --- | --- |
| `&lt;restrictions&gt;`&lt;br&gt;&lt;br&gt;_JSON Object_ | The restrictions object contains the following values:&lt;br&gt;`restriction_list` _(JSON Object)_: list of currently imposed restrictions with the following values&lt;br&gt;&lt;br&gt;`type` _(string)_ - for calling restriction, this would have the value of `RESTRICTED_BUSINESS_INITIATED_CALLING` or `RESTRICTED_USER_INITIATED_CALLING`&lt;br&gt;&lt;br&gt;`reason` _(string)_ - description of restriction&lt;br&gt;&lt;br&gt;`expiration` _(Integer)_ - The UNIX time at which the restriction will expire in UTC timezone |

### Error response

Possible errors that can occur:

- Permissions/Authorization errors

[View Calling API Error Codes and Troubleshooting for more information](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/troubleshooting)

[View general Cloud API Error Codes here](https://developers.facebook.com/documentation/business-messaging/whatsapp/support/error-codes)

## Call settings in WhatsApp Manager

You can also control your call settings via [WhatsApp Manager](https://business.facebook.com/latest/whatsapp_manager/).

To access calling controls in WhatsApp Manager:

1. Click **Account tools** &gt; **Phone numbers** panel
1. Click the gear icon next to the phone number you are using for calling
1. Click the **Calls** tab

## Configure and use call signaling via session initiation protocol (SIP)

Session Initiation Protocol (SIP) is a signaling protocol used for initiating, maintaining, modifying, and terminating real-time communication sessions between two or more endpoints. You can send and receive call signals using SIP instead of Graph API endpoints.

[Learn more about how to use and configure SIP](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/sip)

## Voicemail webhooks

When a WhatsApp user leaves a voicemail on a business phone number that has voicemail [enabled](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/call-settings#voicemail), Cloud API delivers the recorded audio to your business through the existing `messages` webhook field as an inbound audio message.

The webhook payload follows the same schema as the [Audio messages webhook reference](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/messages/audio), with one difference: `messages[].id` contains the call ID (WACID) of the call that produced the voicemail rather than a regular message ID (WAMID). Use this call ID to correlate the voicemail with the originating call lifecycle webhooks.

No additional webhook subscription is required beyond the standard `messages` field; integrations that already handle inbound audio messages can process voicemails with minimal changes.

Voicemail collection is delivered as best-effort. If voicemail collection fails, Cloud API does not send a voicemail webhook for that call.

### Webhook payload

```html
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;&lt;WABA_ID&gt;&quot;,
      &quot;changes&quot;: [
        &#123;
          &quot;field&quot;: &quot;messages&quot;,
          &quot;value&quot;: &#123;
            &quot;messaging_product&quot;: &quot;whatsapp&quot;,
            &quot;metadata&quot;: &#123;
              &quot;phone_number_id&quot;: &quot;&lt;BUSINESS_PHONE_NUMBER_ID&gt;&quot;,
              &quot;display_phone_number&quot;: &quot;&lt;BUSINESS_PHONE_NUMBER&gt;&quot;
            &#125;,
            &quot;contacts&quot;: [
              &#123;
                &quot;wa_id&quot;: &quot;&lt;USER_PHONE_NUMBER&gt;&quot;,
                &quot;user_id&quot;: &quot;&lt;BSUID&gt;&quot;,
                &quot;parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot;,
                &quot;profile&quot;: &#123;
                  &quot;name&quot;: &quot;&lt;USER_PROFILE_NAME&gt;&quot;,
                  &quot;username&quot;: &quot;&lt;USERNAME&gt;&quot;
                &#125;
              &#125;
            ],
            &quot;messages&quot;: [
              &#123;
                &quot;id&quot;: &quot;wacid.HBgLMTQxMjYxMzYyASG...&quot;,
                &quot;from&quot;: &quot;&lt;USER_PHONE_NUMBER&gt;&quot;,
                &quot;from_user_id&quot;: &quot;&lt;BSUID&gt;&quot;,
                &quot;from_parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot;,
                &quot;timestamp&quot;: &quot;1728932177&quot;,
                &quot;type&quot;: &quot;audio&quot;,
                &quot;audio&quot;: &#123;
                  &quot;id&quot;: &quot;1002764438271669&quot;,
                  &quot;sha256&quot;: &quot;Y9vvGyeo3n76ptkXu3CwDBsnzbRFqpjHskQdMGSVqas=&quot;,
                  &quot;mime_type&quot;: &quot;audio/ogg; codecs=opus&quot;
                &#125;
              &#125;
            ]
          &#125;
        &#125;
      ]
    &#125;
  ]
&#125;
```

**Note:** **Usernames and business-scoped user IDs:** The `user_id`, `parent_user_id`, and `username` fields in `contacts` and the `from_user_id` and `from_parent_user_id` fields in `messages` identify the WhatsApp user by their BSUID; the phone number fields (`wa_id`, `from`) may be omitted if the user has adopted a username. For details, see [Business-scoped user IDs](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-scoped-user-ids#business-scoped-user-id).

For detailed field descriptions, see the [Audio messages webhook reference](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/messages/audio).

## Settings update webhooks

You can subscribe to a new webhook subscription field `account_settings_update` to get notified on updates to phone number settings.

- You&#039;ll be notified even for your own updates
- Currently, only changes to calling settings are supported. Under the calling object, only changes to these fields are observed: `status`, `call_icon_visibility`, `callback_permission_status`, `sip.status`, and `srtp_key_exchange_protocol`.

### Steps to get started

- [Set up your webhook subscription](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/create-webhook-endpoint#configure-webhooks) and subscribe to the `account_settings_update` field.
- The same app should also be subscribed to the WhatsApp Business account of your business phone number.
- Your app should have `whatsapp_business_management` permission to receive the webhooks. Using access token for the same app, if you&#039;re able to [get settings](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/call-settings#get-phone-number-calling-settings) successfully, your app is good to receive the webhooks too.

### Webhook payload

```html
&#123;
    &quot;object&quot;: &quot;whatsapp_business_account&quot;,
    &quot;entry&quot;: [
        &#123;
            &quot;id&quot;: &quot;whatsapp-business-account-id&quot;,
            &quot;changes&quot;: [
                &#123;
                    &quot;value&quot;: &#123;
                        &quot;messaging_product&quot;: &quot;whatsapp&quot;,
                        &quot;timestamp&quot;: &quot;1671644824&quot;,
                        &quot;type&quot;: &quot;[phone_number_settings]&quot;,
                        &quot;phone_number_settings&quot;: &#123;
                            &quot;phone_number_id&quot;: &quot;phone-number-id&quot;,
                            &quot;calling&quot;: &#123;
                                &quot;status&quot;: &quot;ENABLED&quot;,
                                &quot;call_icon_visibility&quot;: &quot;DEFAULT&quot;,
                                &quot;callback_permission_status&quot;: &quot;ENABLED&quot;,
                                &quot;call_hours&quot;: &#123;
                                    &quot;status&quot;: &quot;ENABLED&quot;,
                                    &quot;timezone_id&quot;: &quot;[REDACTED]&quot;,
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
                                &quot;sip&quot;: &#123;
                                    &quot;status&quot;: &quot;ENABLED&quot;,
                                    &quot;servers&quot;: [
                                        &#123;
                                            &quot;hostname&quot;: &quot;[REDACTED]&quot;,
                                            &quot;port&quot;: SIP_SERVER_PORT
                                        &#125;
                                    ]
                                &#125;
                            &#125;
                        &#125;
                    &#125;,
                    &quot;field&quot;: &quot;account_settings_update&quot;
                &#125;
            ]
        &#125;
    ]
&#125;
```

### Webhook values

| Placeholder | Description |
| --- | --- |
| `messaging_product`&lt;br&gt;&lt;br&gt;_String_ | Always `whatsapp`. |
| `timestamp`&lt;br&gt;&lt;br&gt;_String_ | Time when the settings were updated. |
| `type`&lt;br&gt;&lt;br&gt;_String_ | Type of the change. Currently, the only value is `PHONE_NUMBER_SETTINGS`. |
| `phone_number_settings`&lt;br&gt;&lt;br&gt;_Object_ | This field is present if the type is `PHONE_NUMBER_SETTINGS`. Currently, only the `calling` sub-field is supported. |
| `phone_number_settings.phone_number_id`&lt;br&gt;&lt;br&gt;_String_ | The phone number ID whose settings were updated. |
| `phone_number_settings.calling`&lt;br&gt;&lt;br&gt;_Object_ | This is present only if fields related to `calling` are updated. It&#039;s null otherwise. When present, the payload is the same as [Get settings API](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/call-settings#get-phone-number-calling-settings) |

## Calling restrictions for user feedback

If your calls receive high negative user feedback, such as blocks and reports, business-initiated calling, user-initiated calling, or both functionalities on your phone number can be restricted.

### Early warning

You will be notified when the business phone number is close to being paused as an early warning. The early warning notifications will be communicated via the channels below.

#### Email

Enforcement emails are sent to the email addresses of all users and admins associated with the business.
If you did not receive an email, confirm which email you have designated as the contact email for your app and make sure that it is active, can receive new email, and does not flag the email as junk or spam mail.

#### Webhook

A webhook will be sent on the `account_update` field:

```json
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;0&quot;,
      &quot;time&quot;: 1623862418,
      &quot;changes&quot;: [
        &#123;
          &quot;field&quot;: &quot;account_update&quot;,
          &quot;value&quot;: &#123;
            &quot;phone_number&quot;: &quot;PN&quot;,
            &quot;event&quot;: &quot;ACCOUNT_VIOLATION&quot;,
            &quot;violation_info&quot;: &#123;
               &quot;violation_type&quot;: &quot;[LOW_BUSINESS_INITIATED_CALLING_QUALITY|LOW_USER_INITIATED_CALLING_QUALITY]&quot;,
            &#125;
          &#125;
        &#125;
      ]
    &#125;
  ]
&#125;
```

If either business or user initiated calling are close to being paused, you will receive a webhook for the respective violation type. For more information about the webhook, see [account_update](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/account_update).

### Pause in calling functionality

Once the negative user feedback reaches a threshold, Cloud API automatically restricts calling functionality on your phone number for a period of 7 days. While paused, the calling phone number will be unable to:

- Make business-initiated calls to users
- Send call permissions requests

Once your phone number has been paused, notifications will be communicated via the channels below.

Note: Any call permissions approved or declined by the users while paused will still be valid.

#### Email

Enforcement emails are sent to the email addresses of all users and admins associated with the business.
If you did not receive an email, confirm which email you have designated as the contact email for your app and make sure that it is active, can receive new email, and does not flag the email as junk or spam mail.

#### Webhook

A webhook will be sent on the `account_update` field:

```json
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;0&quot;,
      &quot;time&quot;: 1641848059,
      &quot;changes&quot;: [
        &#123;
          &quot;field&quot;: &quot;account_update&quot;,
          &quot;value&quot;: &#123;
            &quot;phone_number&quot;: &quot;PN&quot;,
            &quot;event&quot;: &quot;ACCOUNT_RESTRICTION&quot;,
            &quot;restriction_info&quot;: [
              &#123;
                &quot;restriction_type&quot;: &quot;RESTRICTED_BUSINESS_INITIATED_CALLING&quot;,
                &quot;expiration&quot;: 1641848057
              &#125;
            ]
          &#125;
        &#125;
      ]
    &#125;
  ]
&#125;
```

For more information about the webhook, see [account_update](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/account_update).

### Pause in user initiated calling functionality

Once the negative user feedback reaches a threshold, Cloud API automatically restricts user initiated calling functionality on your phone number for a period of 7 days. While paused, the calling phone number will be unable to:

- Receive calls from users
- Have call icon visible

Once your phone number has been paused, notifications will be communicated via the channels below.

#### Email

Enforcement emails are sent to the email addresses of all users and admins associated with the business.
If you did not receive an email, confirm which email you have designated as the contact email for your app and make sure that it is active, can receive new email, and does not flag the email as junk or spam mail.

#### Webhook

A webhook will be sent on the `account_update` field:

```json
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;0&quot;,
      &quot;time&quot;: 1641848059,
      &quot;changes&quot;: [
        &#123;
          &quot;field&quot;: &quot;account_update&quot;,
          &quot;value&quot;: &#123;
            &quot;phone_number&quot;: &quot;PN&quot;,
            &quot;event&quot;: &quot;ACCOUNT_RESTRICTION&quot;,
            &quot;restriction_info&quot;: [
              &#123;
                &quot;restriction_type&quot;: &quot;RESTRICTED_USER_INITIATED_CALLING&quot;,
                &quot;expiration&quot;: 1641848057
              &#125;
            ]
          &#125;
        &#125;
      ]
    &#125;
  ]
&#125;
```

For more information about the webhook, see [account_update](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/account_update).

## Calling restrictions for low call pickup rates

When calling is enabled on your business phone number, you are expected to pick up calls that WhatsApp users place to you.

If a significant number of calls placed to your calling-enabled business phone number are not picked up, you will be notified and expected to make a change.

### What happens if you do not pick up calls

1. **Warning via Email:** You receive an email notification with options to change how you handle incoming calls.
1. **Calling becomes restricted on the business phone number:** The calling button will be hidden from users.

### How to mitigate the situation

#### If you receive a warning

- **Continue allowing users to call:**
  - Identify and address the cause of calls not being picked up and make sure you are properly resourced to handle expected call volumes.
- **Hide call buttons for user-initiated calls:**
  - You can do so either by working with your partner or going to [WhatsApp Manager](https://business.facebook.com/latest/whatsapp_manager/overview/) &gt; Account tools &gt; Phone numbers &gt; select Phone number [WA phone number] &gt; Calls &gt; toggle off Display call buttons.
- **Turn off calling altogether:**
  - You can do so either by working with your partner or going to [WhatsApp Manager](https://business.facebook.com/latest/whatsapp_manager/overview/) &gt; Account tools &gt; Phone numbers &gt; select Phone number [WA phone number] &gt; Calls &gt; toggle off Allow voice calls.

#### If the call button is hidden for the business phone number

- **Re-display calling buttons:**
  - Identify and address the cause of calls not being picked up and make sure you are properly resourced to handle expected call volumes.
  - Next, display the calling buttons by either working with your partner or going to [WhatsApp Manager](https://business.facebook.com/latest/whatsapp_manager/overview/) &gt; Account tools &gt; Phone numbers &gt; select Phone number [WA phone number] &gt; Calls &gt; toggle on Display call buttons.
- **Turn off calling altogether:**
  - You can do so either by working with your partner or going to [WhatsApp Manager](https://business.facebook.com/latest/whatsapp_manager/overview/) &gt; Account tools &gt; Phone numbers &gt; select Phone number [WA phone number] &gt; Calls &gt; toggle off Allow voice calls.

### Webhooks

#### Warning webhook

```json
[
  &#123;
    &quot;object&quot;: &quot;whatsapp_business_account&quot;,
    &quot;entry&quot;: [
      &#123;
        &quot;id&quot;: &quot;0&quot;,
        &quot;time&quot;: 1641848059,
        &quot;changes&quot;: [
          &#123;
            &quot;field&quot;: &quot;account_update&quot;,
            &quot;value&quot;: &#123;
              &quot;phone_number&quot;: &quot;16505552771&quot;,
              &quot;event&quot;: &quot;ACCOUNT_VIOLATION&quot;,
              &quot;violation_info&quot;: &#123;
                &quot;violation_type&quot;: &quot;USER_INITIATED_CALLS_LOW_PICKUP_RATE&quot;,
                &quot;remediation&quot;: &quot;Please identify and address the cause of user-initiated calls not being picked up and make sure the business is properly resourced to handle expected call volumes.&quot;
              &#125;
            &#125;
          &#125;
        ]
      &#125;
    ]
  &#125;
]
```

#### Enforcement webhook

```json
[
  &#123;
    &quot;object&quot;: &quot;whatsapp_business_account&quot;,
    &quot;entry&quot;: [
      &#123;
        &quot;id&quot;: &quot;0&quot;,
        &quot;time&quot;: 1641848059,
        &quot;changes&quot;: [
          &#123;
            &quot;field&quot;: &quot;account_update&quot;,
            &quot;value&quot;: &#123;
              &quot;phone_number&quot;: &quot;16505552771&quot;,
              &quot;event&quot;: &quot;ACCOUNT_RESTRICTION&quot;,
              &quot;restriction_info&quot;: [
                &#123;
                  &quot;restriction_type&quot;: &quot;RESTRICTED_USER_INITIATED_CALLING_CALL_BUTTON_HIDDEN&quot;,
                  &quot;remediation&quot;: &quot;The call button has been hidden due to low pickup rates. Please identify and address the cause of user-initiated calls not being picked up.  Next, display the calling buttons by either working with your partner or going to WhatsApp Manager &gt; Account tools &gt; Phone numbers &gt; select Phone number &gt; Calls &gt; toggle on Display call buttons&quot;
                &#125;
              ]
            &#125;
          &#125;
        ]
      &#125;
    ]
  &#125;
]
```
