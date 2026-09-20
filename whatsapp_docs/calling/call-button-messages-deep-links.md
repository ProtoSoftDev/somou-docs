# Send WhatsApp Call Button Messages and Deep Links



## Overview

After you adopt Cloud API Calling features, you can raise awareness with your customers in two core ways:

* Send them a message with a WhatsApp call button
* Embed a calling deep link into your brand surfaces (website, application, and so on)

## Send interactive message with a WhatsApp call button

Use this endpoint to send a free-form interactive message with a WhatsApp call button during a [customer service window](https://developers.facebook.com/documentation/business-messaging/whatsapp/messages/send-messages#customer-service-windows) or an [open conversation window](https://developers.facebook.com/documentation/business-messaging/whatsapp/pricing#opening-conversations).

When a WhatsApp user clicks the call button, the click initiates a WhatsApp call to the business number that sent the message.

WhatsApp sends a standard [message status webhook](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/messages/status) in response to this message send.

#### Request syntax

```html
POST &lt;PHONE_NUMBER_ID&gt;/messages
```

| Placeholder | Description | Sample value |
| --- | --- | --- |
| `&lt;PHONE_NUMBER_ID&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | **Required**&lt;br&gt;&lt;br&gt;The business phone number from which you are sending messages.&lt;br&gt;&lt;br&gt;[Learn more about formatting phone numbers in Cloud API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/message-api) | `+12784358810` |

#### Request body

```json
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;recipient_type&quot;: &quot;individual&quot;,
  &quot;to&quot;: &quot;14085551234&quot;,
  &quot;recipient&quot;: &quot;US.13491208655302741918&quot;,
  &quot;type&quot;: &quot;interactive&quot;,
  &quot;interactive&quot; : &#123;
    &quot;type&quot; : &quot;voice_call&quot;,
    &quot;body&quot; : &#123;
      &quot;text&quot;: &quot;You can call us on WhatsApp now for faster service!&quot;
    &#125;,
    &quot;action&quot;: &#123;
      &quot;name&quot;: &quot;voice_call&quot;,
      &quot;parameters&quot;: &#123;
        &quot;display_text&quot;: &quot;Call on WhatsApp&quot;,
        &quot;ttl_minutes&quot;: 100,
        &quot;payload&quot;: &quot;payload data&quot;
      &#125;
    &#125;
  &#125;
&#125;
```

#### Body parameters

[Learn more about sending interactive free form messages](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/message-api)

| Parameter | Description | Sample value |
| --- | --- | --- |
| `to`&lt;br&gt;&lt;br&gt;_Integer_ | **Required** (unless `recipient` is provided)&lt;br&gt;&lt;br&gt;The phone number of the WhatsApp user you are messaging.&lt;br&gt;&lt;br&gt;[Learn more about formatting phone numbers in Cloud API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/whatsapp-business-account-phone-number-api) | `&quot;17863476655&quot;` |
| `recipient`&lt;br&gt;&lt;br&gt;_String_ | **Optional**&lt;br&gt;&lt;br&gt;The WhatsApp user&#039;s business-scoped user ID (BSUID) or parent BSUID. Use this instead of, or in addition to, `to`. If you include both, `to` takes precedence.&lt;br&gt;&lt;br&gt;[Learn more about business-scoped user IDs](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-scoped-user-ids#business-scoped-user-id) | `&quot;US.13491208655302741918&quot;` |
| `type`&lt;br&gt;&lt;br&gt;_String_ | **Required**&lt;br&gt;&lt;br&gt;The type of interactive message you are sending.&lt;br&gt;&lt;br&gt;In this case, you are sending a `voice_call`.&lt;br&gt;&lt;br&gt;[Learn more about interactive messages](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/message-api) | `&quot;voice_call&quot;` |
| `action`&lt;br&gt;&lt;br&gt;_String_ | **Required**&lt;br&gt;&lt;br&gt;The action of your interactive message.&lt;br&gt;&lt;br&gt;Must be `voice_call`. | `&quot;voice_call&quot;` |
| `parameters`&lt;br&gt;&lt;br&gt;_JSON Object_ | **Optional**&lt;br&gt;&lt;br&gt;Optional parameters for the WhatsApp calling button sent to the user.&lt;br&gt;&lt;br&gt;Contains three values: `display_text`, `ttl_minutes`, and `payload`.&lt;br&gt;&lt;br&gt;`display_text` — (_String_) **Optional**&lt;br&gt;&lt;br&gt;The display text on the WhatsApp calling button sent to the user.&lt;br&gt;&lt;br&gt;Default is `Call Now`.&lt;br&gt;&lt;br&gt;Max length: 20 characters.&lt;br&gt;&lt;br&gt;`ttl_minutes` — (_Integer_) **Optional**&lt;br&gt;&lt;br&gt;Time to live for the call-to-action (CTA) button in minutes.&lt;br&gt;&lt;br&gt;Must be between 1 and 43200 (30 days).&lt;br&gt;&lt;br&gt;Default value is 10080 (7 days).&lt;br&gt;&lt;br&gt;`payload` — (_String_) **Optional**&lt;br&gt;&lt;br&gt;An arbitrary string, useful for tracking.&lt;br&gt;&lt;br&gt;Any app subscribed to the `calls` webhook field on the WhatsApp Business account can get this string. The string is included in the `connect` and `terminate` webhook payloads under the `cta_payload` field.&lt;br&gt;&lt;br&gt;Cloud API does not process the `cta_payload` field; it returns the value in webhook payloads.&lt;br&gt;&lt;br&gt;Maximum 512 characters.&lt;br&gt;&lt;br&gt;Payload is only available to WhatsApp clients starting on version 2.25.27. | ```html
&quot;parameters&quot;: &#123;
&quot;display_text&quot;: &quot;Call on WhatsApp&quot;,
&quot;ttl_minutes&quot;: 100,
&quot;payload&quot;: &quot;payload data&quot;
&#125;
``` |

**Note:** **Usernames and business-scoped user IDs:** The `recipient` field lets you identify the WhatsApp user by their BSUID instead of, or in addition to, their phone number in `to`. For details, see [Business-scoped user IDs](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-scoped-user-ids#business-scoped-user-id).

#### Success response

[Learn more about messaging success responses](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/message-api)

#### Error response

Possible errors:

If you send this message to users on older app versions, Cloud API returns an error webhook with error code `131026`.

[View general Cloud API error codes](https://developers.facebook.com/documentation/business-messaging/whatsapp/support/error-codes)

## Create and send WhatsApp call button template message

Use these endpoints to create and send a WhatsApp call button template message.

Once your call button template message is created, you can send a message to a WhatsApp user, inviting them to call your business.

[Learn more about creating and managing message templates](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/overview)

### Create call button message template

Use this endpoint to create a call button message template.

#### Request syntax

```html
POST /&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;/message_templates
```

| Parameter | Description | Sample value |
| --- | --- | --- |
| `&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required**&lt;br&gt;&lt;br&gt;Your WhatsApp Business account ID.&lt;br&gt;&lt;br&gt;[Learn how to find your WABA ID](https://developers.facebook.com/documentation/business-messaging/whatsapp/whatsapp-business-accounts) | `&quot;waba-90172398162498126&quot;` |

#### Request body

```html
&#123;
  &quot;name&quot;: &quot;&lt;NAME&gt;&quot;,
  &quot;category&quot;: &quot;&lt;CATEGORY&gt;&quot;,
  &quot;language&quot;: &quot;&lt;LANGUAGE&gt;&quot;,
  &quot;components&quot;: [
    &#123;
      &quot;type&quot;: &quot;BODY&quot;,
      &quot;text&quot;: &quot;You can call us on WhatsApp now for faster service!&quot;
    &#125;,
    &#123;
      &quot;type&quot;: &quot;BUTTONS&quot;,
      &quot;buttons&quot;: [
        &#123;
          &quot;type&quot;: &quot;voice_call&quot;,
          &quot;text&quot;: &quot;Call Now&quot;,
          &quot;ttl_minutes&quot;: 1440
        &#125;,
        &#123;
          &quot;type&quot;: &quot;URL&quot;,
          &quot;text&quot;: &quot;Contact Support&quot;,
          &quot;url&quot;: &quot;https://www.luckyshrub.com/support&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

#### Body parameters

You can create and manage template messages through both Cloud API and the Meta Business Suite interface.

When creating your call button template, ensure you configure `type` as `voice_call`.

[Learn more about creating and managing message templates](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/overview)

| Parameter | Description | Sample value |
| --- | --- | --- |
| `type`&lt;br&gt;&lt;br&gt;_String_ | **Required**&lt;br&gt;&lt;br&gt;The type of template message you are creating.&lt;br&gt;&lt;br&gt;In this case, you are creating a `voice_call`. | `&quot;voice_call&quot;` |
| `text`&lt;br&gt;&lt;br&gt;_String_ | **Optional**&lt;br&gt;&lt;br&gt;The display text on the WhatsApp calling button sent to the user.&lt;br&gt;&lt;br&gt;Default is `Call Now`.&lt;br&gt;&lt;br&gt;Max length: 20 characters. | `&quot;Call Now&quot;` |
| `ttl_minutes`&lt;br&gt;&lt;br&gt;_Integer_ | **Optional**&lt;br&gt;&lt;br&gt;Time to live for the CTA button in minutes.&lt;br&gt;&lt;br&gt;Must be between 1440 (1 day) and 43200 (30 days).&lt;br&gt;&lt;br&gt;You can override this value when sending the message. | `1440` |

#### Success response

```html
&#123;
  &quot;id&quot;: &quot;&lt;ID&gt;&quot;,
  &quot;status&quot;: &quot;&lt;STATUS&gt;&quot;,
  &quot;category&quot;: &quot;&lt;CATEGORY&gt;&quot;
&#125;
```

[_Learn more about messaging success responses_](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/message-api)

#### Error response

Possible errors:

* Invalid `whatsapp-business-account-id`
* Permissions/Authorization errors
* Template structure/component validation alerts

[View general Cloud API error codes](https://developers.facebook.com/documentation/business-messaging/whatsapp/support/error-codes)


### Send call button message template

Use this endpoint to **send** a call button message template.

The following is a simplified sample of the send template message request. You can also [learn more about how to send message templates](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/overview).

#### Request syntax

```html
POST /&lt;PHONE_NUMBER_ID&gt;/messages
```

| Parameter | Description | Sample value |
| --- | --- | --- |
| `&lt;PHONE_NUMBER_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required**&lt;br&gt;&lt;br&gt;The business phone number from which you are sending messages.&lt;br&gt;&lt;br&gt;[Learn more about formatting phone numbers in Cloud API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/whatsapp-business-account-phone-number-api) | `+18762639988` |

#### Request body

```json
&#123;
  &quot;to&quot;: &quot;14085551234&quot;,
  &quot;recipient&quot;: &quot;US.13491208655302741918&quot;,
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;type&quot;: &quot;template&quot;,
  &quot;recipient_type&quot;: &quot;individual&quot;,
  &quot;template&quot;: &#123;
    &quot;name&quot;: &quot;wa_voice_call&quot;,
    &quot;language&quot;: &#123;
      &quot;code&quot;: &quot;en&quot;
    &#125;,
    &quot;components&quot;: [
      &#123;
        &quot;type&quot;: &quot;button&quot;,
        &quot;sub_type&quot; : &quot;voice_call&quot;,
        &quot;parameters&quot;: [
          &#123;
            &quot;type&quot;: &quot;ttl_minutes&quot;,
            &quot;ttl_minutes&quot;: 100
          &#125;,
          &#123;
            &quot;type&quot;: &quot;payload&quot;,
            &quot;payload&quot;: &quot;payload data&quot;
          &#125;
        ]
      &#125;
    ]
  &#125;
&#125;
```

#### Request parameters

| Parameter | Description | Sample value |
| --- | --- | --- |
| `recipient`&lt;br&gt;&lt;br&gt;_String_ | **Optional**&lt;br&gt;&lt;br&gt;The WhatsApp user&#039;s business-scoped user ID (BSUID) or parent BSUID. Use this instead of, or in addition to, `to`. If you include both, `to` takes precedence.&lt;br&gt;&lt;br&gt;[Learn more about business-scoped user IDs](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-scoped-user-ids#business-scoped-user-id) | `&quot;US.13491208655302741918&quot;` |
| `ttl_minutes`&lt;br&gt;&lt;br&gt;_Integer_ | **Optional**&lt;br&gt;&lt;br&gt;Time to live for the CTA button in minutes.&lt;br&gt;&lt;br&gt;Must be between 1 and 43200 (30 days).&lt;br&gt;&lt;br&gt;Default value is 10080 (7 days). | `10800` |
| `payload`&lt;br&gt;&lt;br&gt;_String_ | **Optional**&lt;br&gt;&lt;br&gt;An arbitrary string, useful for tracking.&lt;br&gt;&lt;br&gt;Any app subscribed to the `calls` webhook field on the WhatsApp Business account can get this string. The string is included in the `connect` and `terminate` webhook payloads under the `cta_payload` field.&lt;br&gt;&lt;br&gt;Cloud API does not process this field; it returns the value in webhook payloads.&lt;br&gt;&lt;br&gt;Maximum 512 characters.&lt;br&gt;&lt;br&gt;Payload is only available to WhatsApp clients starting on version 2.25.27. | `payload data` |

**Note:** **Usernames and business-scoped user IDs:** The `recipient` field lets you identify the WhatsApp user by their BSUID instead of, or in addition to, their phone number in `to`. For details, see [Business-scoped user IDs](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-scoped-user-ids#business-scoped-user-id).

#### Success response

[Learn more about messaging success responses](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/message-api)

## Calling deep links

Calling deep links are hyperlinks that route WhatsApp users to call your business.

The process to create a calling deep link is similar to a [chat deep link](https://faq.whatsapp.com/5913398998672934/?locale=en_US), except the format for the call deep link is `wa.me/call/&lt;BUSINESS_PHONE_NUMBER&gt;`

Deep links are not supported on WhatsApp desktop clients.

### Embed calling deep links

You can use calling deep links to advertise WhatsApp calling for your business.

Use these links anywhere calling is useful, such as your website, primary application, or a QR code to be shared.

### Send calling deep links

You can also send messages to WhatsApp users with a calling deep link.

Since deep links can be made per business phone number, you can use calling deep links to prompt WhatsApp users to contact a different phone number with voice enabled.

The `wa.me/call/&lt;BUSINESS_PHONE_NUMBER&gt;` format is easy to copy, paste, and send, and does not require you to make a template in Meta Business Suite.

### Send payload data in call deep link

You can also send a payload with the deep link. You can use the `biz_payload` query string when sending the call deep link to any user (`wa.me/call/&lt;BUSINESS_PHONE_NUMBER&gt;?biz_payload=payload`).

When a user calls using the provided deep link with the `biz_payload`, any app subscribed to the `calls` webhook field on the WhatsApp Business account can get this string. The string is included in the `connect` and `terminate` webhook payloads under the `deeplink_payload` field.

Payload in call deep link is only available to WhatsApp clients starting on version 2.25.27.
