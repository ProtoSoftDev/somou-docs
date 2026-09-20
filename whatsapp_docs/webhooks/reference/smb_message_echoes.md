# smb_message_echoes webhook reference



This reference describes trigger events and payload contents for the WhatsApp Business account **smb_message_echoes** webhook.

The **smb_message_echoes** webhook notifies you of messages sent via the WhatsApp Business app or a [companion (&quot;linked&quot;) device](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/onboarding-business-app-users#linked-devices) by a business customer who has been [onboarded to Cloud API](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/onboarding-business-app-users) via a solution provider.


## Triggers

- A business customer with a WhatsApp Business app phone number, who has been [onboarded by a partner](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/onboarding-business-app-users), sends a message using the WhatsApp Business app or a [companion device](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/onboarding-business-app-users#linked-devices) to a WhatsApp user or another business.
- A business customer revokes (deletes) a previously sent message using the WhatsApp Business app.
- A business customer edits a previously sent message using the WhatsApp Business app.

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
            &quot;message_echoes&quot;: [
              &#123;
                &quot;from&quot;: &quot;&lt;BUSINESS_DISPLAY_PHONE_NUMBER&gt;&quot;,
                &quot;to&quot;: &quot;&lt;WHATSAPP_USER_PHONE_NUMBER&gt;&quot;,
                &quot;id&quot;: &quot;&lt;WHATSAPP_MESSAGE_ID&gt;&quot;,
                &quot;timestamp&quot;: &quot;&lt;WEBHOOK_TRIGGER_TIMESTAMP&gt;&quot;,
                &quot;type&quot;: &quot;&lt;MESSAGE_TYPE&gt;&quot;,
                &quot;&lt;MESSAGE_TYPE&gt;&quot;: &#123;
                  &lt;MESSAGE_CONTENTS&gt;
                &#125;
              &#125;
            ]
          &#125;,
          &quot;field&quot;: &quot;smb_message_echoes&quot;
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
| `&lt;MESSAGE_CONTENTS&gt;`&lt;br&gt;&lt;br&gt;_Object_ | An object describing the message&#039;s contents.&lt;br&gt;&lt;br&gt;This value will vary based on the message `type`, as well as the contents of the message.&lt;br&gt;&lt;br&gt;For example, if a business sends an `image` message without a caption, the object would not include the `caption` property.&lt;br&gt;&lt;br&gt;See [Sending messages](https://developers.facebook.com/documentation/business-messaging/whatsapp/messages/send-messages) for examples of payloads for each message type. | `&#123;&quot;body&quot;:&quot;Here&#039;s the info you requested! https://www.meta.com/quest/quest-3/&quot;&#125;` |
| `&lt;MESSAGE_TYPE&gt;`&lt;br&gt;&lt;br&gt;_String_ | [Message type](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/messages). This placeholder appears twice in the syntax above because it serves as both the `type` property value and its matching property name. Supported values include `text`, `image`, `video`, `document`, `revoke`, and `edit`. | `text` |
| `&lt;WEBHOOK_TRIGGER_TIMESTAMP&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | Unix timestamp indicating when the webhook was triggered. | `1739321024` |
| `&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | The business customer&#039;s WhatsApp Business account ID. | `102290129340398` |
| `&lt;WHATSAPP_MESSAGE_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | WhatsApp message ID. | `wamid.HBgLMTY1MDM4Nzk0MzkVAgASGBQzQUFERjg0NDEzNDdFODU3MUMxMAA=` |
| `&lt;WHATSAPP_USER_PHONE_NUMBER&gt;`&lt;br&gt;&lt;br&gt;_String_ | WhatsApp user phone number. This is the same value returned by the API as the `input` value when sending a message to a WhatsApp user. Note that a WhatsApp user&#039;s phone number and ID may not always match. | `+16505551234` |

## Examples

### Text message

This example payload describes a text message sent to a WhatsApp user by a business customer using the WhatsApp Business app.

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
            &quot;message_echoes&quot;: [
              &#123;
                &quot;from&quot;: &quot;15550783881&quot;,
                &quot;to&quot;: &quot;16505551234&quot;,
                &quot;id&quot;: &quot;wamid.HBgLMTY0NjcwNDM1OTUVAgARGBIyNDlBOEI5QUQ4NDc0N0FCNjMA&quot;,
                &quot;timestamp&quot;: &quot;1739321024&quot;,
                &quot;type&quot;: &quot;text&quot;,
                &quot;text&quot;: &#123;
                  &quot;body&quot;: &quot;Here&#039;s the info you requested! https://www.meta.com/quest/quest-3/&quot;
                &#125;
              &#125;
            ]
          &#125;,
          &quot;field&quot;: &quot;smb_message_echoes&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

### Revoke message

This example payload describes a business customer revoking (deleting) a previously sent message using the WhatsApp Business app. The `revoke` object contains the `original_message_id` of the message being deleted.

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
            &quot;message_echoes&quot;: [
              &#123;
                &quot;from&quot;: &quot;15550783881&quot;,
                &quot;to&quot;: &quot;16505551234&quot;,
                &quot;id&quot;: &quot;wamid.HBgLMTY1MDM4Nzk0MzkVAgASGBQzQUFERjg0NDEzNDdFODU3MUMxMAA=&quot;,
                &quot;timestamp&quot;: &quot;1749854575&quot;,
                &quot;type&quot;: &quot;revoke&quot;,
                &quot;revoke&quot;: &#123;
                  &quot;original_message_id&quot;: &quot;wamid.HBgLMTQxMjU1NTA4MjkVAgASGBQzQUNCNjk5RDUwNUZGMUZEM0VBRAA=&quot;
                &#125;
              &#125;
            ]
          &#125;,
          &quot;field&quot;: &quot;smb_message_echoes&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

### Edit message

This example payload describes a business customer editing the caption of a previously sent image message using the WhatsApp Business app. The `edit` object contains the `original_message_id` of the message being edited and a `message` object with the updated content.

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
            &quot;message_echoes&quot;: [
              &#123;
                &quot;from&quot;: &quot;15550783881&quot;,
                &quot;to&quot;: &quot;16505551234&quot;,
                &quot;id&quot;: &quot;wamid.HBgLMTY1MDM4Nzk0MzkVAgASGBQzQkJERjg0NDEzNDdFODU3MUMxMAA=&quot;,
                &quot;timestamp&quot;: &quot;1749854620&quot;,
                &quot;type&quot;: &quot;edit&quot;,
                &quot;edit&quot;: &#123;
                  &quot;original_message_id&quot;: &quot;wamid.HBgLMTQxMjU1NTA4MjkVAgASGBQzQUNCNjk5RDUwNUZGMUZEM0VBRAA=&quot;,
                  &quot;message&quot;: &#123;
                    &quot;context&quot;: &#123;
                      &quot;id&quot;: &quot;M0&quot;
                    &#125;,
                    &quot;type&quot;: &quot;image&quot;,
                    &quot;image&quot;: &#123;
                      &quot;caption&quot;: &quot;Updated image caption&quot;,
                      &quot;mime_type&quot;: &quot;image/jpeg&quot;,
                      &quot;sha256&quot;: &quot;a1b2c3d4e5f6a1b2c3d4e5f6a1b2c3d4e5f6a1b2c3d4e5f6a1b2c3d4e5f6a1b2&quot;,
                      &quot;id&quot;: &quot;1234567890&quot;,
                      &quot;url&quot;: &quot;https://lookaside.fbsbx.com/whatsapp_business/attachments/?mid=133...&quot;
                    &#125;
                  &#125;
                &#125;
              &#125;
            ]
          &#125;,
          &quot;field&quot;: &quot;smb_message_echoes&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```
