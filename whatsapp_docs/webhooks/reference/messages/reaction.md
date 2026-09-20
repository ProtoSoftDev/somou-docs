# Reaction messages webhook reference



This reference describes trigger events and payload contents for the WhatsApp Business account **messages** webhook for messages containing a reaction to a previous message sent by a business.

**Note:** When an end user removes a reaction emoji, a webhook without the &quot;emoji&quot; field will be sent as shown in the sample webhooks below.

## Triggers

- A WhatsApp user reacts to a previous message sent by a business within the last 30 days.
- A WhatsApp user removes a previously sent reaction to a previous message sent by a business within the last 30 days.

## Syntax

```html
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
              &quot;display_phone_number&quot;: &quot;&lt;BUSINESS_DISPLAY_PHONE_NUMBER&gt;&quot;,
              &quot;phone_number_id&quot;: &quot;&lt;BUSINESS_PHONE_NUMBER_ID&gt;&quot;
            &#125;,
            &quot;contacts&quot;: [
              &#123;
                &quot;profile&quot;: &#123;
                  &quot;name&quot;: &quot;&lt;WHATSAPP_USER_PROFILE_NAME&gt;&quot;
                &#125;,
                &quot;wa_id&quot;: &quot;&lt;WHATSAPP_USER_ID&gt;&quot;,
                &quot;identity_key_hash&quot;: &quot;&lt;IDENTITY_KEY_HASH&gt;&quot; &lt;!-- only included if identity change check enabled --&gt;
              &#125;
            ],
            &quot;messages&quot;: [
              &#123;
                &quot;from&quot;: &quot;&lt;WHATSAPP_USER_PHONE_NUMBER&gt;&quot;,
                &quot;id&quot;: &quot;&lt;WHATSAPP_MESSAGE_ID&gt;&quot;,
                &quot;timestamp&quot;: &quot;&lt;REACTION_TIMESTAMP&gt;&quot;,
                &quot;type&quot;: &quot;reaction&quot;,
                &quot;reaction&quot;: &#123;
                  &quot;message_id&quot;: &quot;&lt;CONTEXTUAL_WHATSAPP_MESSAGE_ID&gt;&quot;,
                  &quot;emoji&quot;: &quot;&lt;EMOJI_UNICODE&gt;&quot; &lt;!-- omitted if user removes reaction --&gt;
                &#125;
              &#125;
            ]
          &#125;,
          &quot;field&quot;: &quot;messages&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

## Parameters

| Placeholder | Description | Example value |
| --- | --- | --- |
| `&lt;BUSINESS_DISPLAY_PHONE_NUMBER&gt;`&lt;br&gt;&lt;br&gt;_String_ | Business display phone number. | `15550783881` |
| `&lt;BUSINESS_PHONE_NUMBER_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | Business phone number ID. | `106540352242922` |
| `&lt;CONTEXTUAL_WHATSAPP_MESSAGE_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | WhatsApp message ID of the message the WhatsApp user reacted to. | `wamid.HBgLMTQxMjU1NTA4MjkVAgASGBQzQUNCNjk5RDUwNUZGMUZEM0VBRAA=` |
| `&lt;EMOJI_UNICODE&gt;`&lt;br&gt;&lt;br&gt;_String_ | Unicode of emoji sent by the WhatsApp user as a reaction.&lt;br&gt;&lt;br&gt;If the user removes their initial reaction, another webhook is triggered, but the `emoji` property will be omitted from the payload. | `U+1F44D` |
| `&lt;IDENTITY_KEY_HASH&gt;`&lt;br&gt;&lt;br&gt;_String_ | Identity key hash. Only included if you have enabled the [identity change check](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-phone-numbers/phone-numbers) feature. | `DF2lS5v2W6x=` |
| `&lt;REACTION_TIMESTAMP&gt;`&lt;br&gt;&lt;br&gt;_String_ | Unix timestamp indicating when the WhatsApp user sent the reaction. | `1739321024` |
| `&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | WhatsApp Business Account ID. | `102290129340398` |
| `&lt;WHATSAPP_MESSAGE_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | WhatsApp message ID. | `wamid.HBgLMTY1MDM4Nzk0MzkVAgASGBQzQUFERjg0NDEzNDdFODU3MUMxMAA=` |
| `&lt;WHATSAPP_USER_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | WhatsApp user ID. Note that a WhatsApp user&#039;s ID and phone number may not always match. | `16505551234` |
| `&lt;WHATSAPP_USER_PHONE_NUMBER&gt;`&lt;br&gt;&lt;br&gt;_String_ | WhatsApp user phone number. This is the same value returned by the API as the `input` value when sending a message to a WhatsApp user. Note that a WhatsApp user&#039;s phone number and ID may not always match. | `+16505551234` |
| `&lt;WHATSAPP_USER_PROFILE_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | WhatsApp user&#039;s name as it appears in their profile in the WhatsApp client. | `Sheena Nelson` |

## Sample Webhooks

***Receiving a reaction***

```json
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;102290129340398&quot;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;messaging_product&quot;: &quot;whatsapp&quot;,
            &quot;metadata&quot;: &#123;
              &quot;display_phone_number&quot;: &quot;15550783881&quot;,
              &quot;phone_number_id&quot;: &quot;106540352242922&quot;
            &#125;,
            &quot;contacts&quot;: [
              &#123;
                &quot;profile&quot;: &#123;
                  &quot;name&quot;: &quot;Sheena Nelson&quot;
                &#125;,
                &quot;wa_id&quot;: &quot;16505551234&quot;
              &#125;
            ],
            &quot;messages&quot;: [
              &#123;
                &quot;from&quot;: &quot;16505551234&quot;,
                &quot;id&quot;: &quot;wamid.HBgLMTY1MDM4Nzk0MzkVAgASGBQzQUFERjg0NDEzNDdFODU3MUMxMAA=&quot;,
                &quot;timestamp&quot;: &quot;1749419544&quot;,
                &quot;type&quot;: &quot;reaction&quot;,
                &quot;reaction&quot;: &#123;
                  &quot;message_id&quot;: &quot;wamid.HBgLMTQxMjU1NTA4MjkVAgASGBQzQUNCNjk5RDUwNUZGMUZEM0VBRAA=&quot;,
                  &quot;emoji&quot;: &quot;👍&quot;
                &#125;
              &#125;
            ]
          &#125;,
          &quot;field&quot;: &quot;messages&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

***Reaction removed by end user***

```json
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;102290129340398&quot;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;messaging_product&quot;: &quot;whatsapp&quot;,
            &quot;metadata&quot;: &#123;
              &quot;display_phone_number&quot;: &quot;15550783881&quot;,
              &quot;phone_number_id&quot;: &quot;106540352242922&quot;
            &#125;,
            &quot;contacts&quot;: [
              &#123;
                &quot;profile&quot;: &#123;
                  &quot;name&quot;: &quot;Sheena Nelson&quot;
                &#125;,
                &quot;wa_id&quot;: &quot;16505551234&quot;
              &#125;
            ],
            &quot;messages&quot;: [
              &#123;
                &quot;from&quot;: &quot;16505551234&quot;,
                &quot;id&quot;: &quot;wamid.HBgLMTY1MDM4Nzk0MzkVAgASGBQzQUFERjg0NDEzNDdFODU3MUMxMAA=&quot;,
                &quot;timestamp&quot;: &quot;1749419544&quot;,
                &quot;type&quot;: &quot;reaction&quot;,
                &quot;reaction&quot;: &#123;
                  &quot;message_id&quot;: &quot;wamid.HBgLMTQxMjU1NTA4MjkVAgASGBQzQUNCNjk5RDUwNUZGMUZEM0VBRAA=&quot;
                &#125;
              &#125;
            ]
          &#125;,
          &quot;field&quot;: &quot;messages&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```
