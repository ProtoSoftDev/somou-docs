# Group messages webhook reference



This reference describes trigger events and payload contents for the WhatsApp Business account **messages** webhook for messages that are sent to a group, or received from a group.

## Triggers

- A WhatsApp user or a business sends a message to a group.
- A WhatsApp user or a business receives a message within a group.

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
                &quot;group_id&quot;: &quot;&lt;GROUP_ID&gt;&quot;,
                &quot;id&quot;: &quot;&lt;WHATSAPP_MESSAGE_ID&gt;&quot;,
                &quot;timestamp&quot;: &quot;&lt;WEBHOOK_TRIGGER_TIMESTAMP&gt;&quot;,
                &quot;text&quot;: &#123;
                  &quot;body&quot;: &quot;&lt;MESSAGE_TEXT_BODY&gt;&quot;
                &#125;,
                &quot;type&quot;: &quot;&lt;MESSAGE_TYPE&gt;&quot;
              &#125;
            ],
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
| `&lt;GROUP_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | The string identifier of a group made using the Groups API.&lt;br&gt;&lt;br&gt;This field appears when a WhatsApp user or business sends, receives, or reads a message in a group.&lt;br&gt;&lt;br&gt;[Learn more about the Groups API](https://developers.facebook.com/documentation/business-messaging/whatsapp/groups) | `&quot;Y2FwaV9ncm91cDoxNzA1NTU1MDEzOToxMjAzNjM0MDQ2OTQyMzM4MjAZD&quot;` |
| `&lt;IDENTITY_KEY_HASH&gt;`&lt;br&gt;&lt;br&gt;_String_ | Identity key hash. Only included if you have enabled the [identity change check](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-phone-numbers/phone-numbers) feature. | `DF2lS5v2W6x=` |
| `&lt;MESSAGE_TEXT_BODY&gt;`&lt;br&gt;&lt;br&gt;_String_ | Text body of the message. | `What do you all think about this?` |
| `&lt;MESSAGE_TYPE&gt;`&lt;br&gt;&lt;br&gt;_String_ | The type of message being sent. Will change depending on the message sent to the group.&lt;br&gt;&lt;br&gt;Currently, the Groups API supports:&lt;br&gt;&lt;br&gt;* Text&lt;br&gt;* Media&lt;br&gt;* Text-based templates&lt;br&gt;* Media-based templates | `text` |
| `&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | WhatsApp Business Account ID. | `102290129340398` |
| `&lt;WHATSAPP_MESSAGE_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | WhatsApp message ID. | `wamid.HBgLMTY1MDM4Nzk0MzkVAgASGBQzQUFERjg0NDEzNDdFODU3MUMxMAA=` |
| `&lt;WEBHOOK_TRIGGER_TIMESTAMP&gt;`&lt;br&gt;&lt;br&gt;_String_ | Unix timestamp indicating when the webhook was triggered. | `1739321024` |
| `&lt;WHATSAPP_USER_PHONE_NUMBER&gt;`&lt;br&gt;&lt;br&gt;_String_ | WhatsApp user phone number. This is the same value returned by the API as the `input` value when sending a message to a WhatsApp user. Note that a WhatsApp user&#039;s phone number and ID may not always match. | `+16505551234` |
| `&lt;WHATSAPP_USER_PROFILE_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | WhatsApp user&#039;s name as it appears in their profile in the WhatsApp client. | `Sheena Nelson` |
| `&lt;WHATSAPP_USER_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | WhatsApp user ID. Note that a WhatsApp user&#039;s ID and phone number may not always match. | `16505551234` |

## Example

The following example shows a group messages webhook payload.

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
                  &quot;name&quot;: &quot;Tiago Mingo&quot;
                &#125;,
                &quot;wa_id&quot;: &quot;16505551234&quot;
              &#125;
            ],
            &quot;messages&quot;: [
              &#123;
                &quot;from&quot;: &quot;16505551234&quot;,
                &quot;group_id&quot;: &quot;HBgLMTY1MDM4Nzk0MzkVAgASGBQzQTRBNjU5OUFFRTAzODEwMTQ0RgA&quot;,
                &quot;id&quot;: &quot;wamid.HASDI128HJOPUERIH82ahdasd09ASDHi5&gt;&quot;,
                &quot;timestamp&quot;: &quot;1744344496&quot;,
                &quot;text&quot;: &#123;
                  &quot;body&quot;: &quot;What does everyone think about this?&quot;
                &#125;,
                &quot;type&quot;: &quot;text&quot;
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
