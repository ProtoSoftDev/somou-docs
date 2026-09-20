# history webhook reference



This reference describes trigger events and payload contents for the WhatsApp Business account `history` webhook.

The **history** webhook is used to synchronize the [WhatsApp Business app chat history](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/onboarding-business-app-users) of a business customer onboarded by a solution provider.


## Triggers

- A partner [synchronizes the WhatsApp Business app chat history](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/onboarding-business-app-users#synchronizing-whatsapp-business-app-data) of a business customer who they have onboarded with a WhatsApp Business app phone number, and who has agreed to share their chat history.
- A partner [synchronizes the WhatsApp Business app chat history](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/onboarding-business-app-users#synchronizing-whatsapp-business-app-data) of a business customer who they have onboarded with a WhatsApp Business app phone number, but the customer has declined to share their chat history.

## Chat history sharing approved

### Chat history contents

If the business customer has already approved chat history sharing when the partner requests the business&#039;s chat history, a series of history webhooks will be triggered, describing all messages sent or received within 180 days of the time when the business was onboarded onto Cloud API.

- Messages that are part of a group chat will not be included.
- Media messages will not include media asset IDs. Instead, additional history webhooks containing media message asset IDs will be sent separately, but only for media messages sent within 14 days of onboarding.

Note that for efficiency purposes, a single webhook could potentially describe thousands of messages. Capture the webhook contents first, then process them asynchronously.

### Phases and chunks

Webhooks are divided into three history phases, where day 0 indicates the time when the business was onboarded onto Cloud API:

- phase 0: day 0 through day 1
- phase 1: day 1 through day 90
- phase 2: day 90 through day 180

For each phase, chat history webhooks may be sent in separate chunks, depending on the total number of messages that comprise the thread.

- You can use the `chunk_order` parameter value to arrange these chunks in their sequential order, as they may not be delivered sequentially.
- You can use the `phase` parameter value to monitor phase progress. A value of `2` indicates that the current phase is complete.
- You can use the `progress` parameter value to monitor the overall progress. A value of `100` indicates that synchronization is complete.

If there is no chat history available for a given phase, no corresponding webhooks will be sent.

### Syntax

```html
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;&lt;CUSTOMER_WABA_ID&gt;&quot;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;messaging_product&quot;: &quot;whatsapp&quot;,
            &quot;metadata&quot;: &#123;
              &quot;display_phone_number&quot;: &quot;&lt;CUSTOMER_DISPLAY_PHONE_NUMBER&gt;&quot;,
              &quot;phone_number_id&quot;: &quot;&lt;CUSTOMER_PHONE_NUMBER_ID&gt;&quot;
            &#125;,
            &quot;history&quot;: [
              &#123;
                &quot;metadata&quot;: &#123;
                  &quot;phase&quot;: &lt;PHASE&gt;,
                  &quot;chunk_order&quot;: &lt;CHUNK_ORDER&gt;,
                  &quot;progress&quot;: &lt;PROGRESS&gt;
                &#125;,
                &quot;threads&quot;: [
                  /* First chat history thread object */
                  &#123;
                    &quot;id&quot;: &quot;&lt;WHATSAPP_USER_PHONE_NUMBER&gt;&quot;,
                    &quot;messages&quot;: [
                      /* First message object in thread */
                      &#123;
                        &quot;from&quot;: &quot;&lt;BUSINESS_OR_WHATSAPP_USER_PHONE_NUMBER&gt;&quot;,
                        &quot;to&quot;: &quot;&lt;WHATSAPP_USER_PHONE_NUMBER&gt;&quot;, // only included if SMB message echo
                        &quot;id&quot;: &quot;&lt;WHATSAPP_MESSAGE_ID&gt;&quot;,
                        &quot;timestamp&quot;: &quot;&lt;DEVICE_TIMESTAMP&gt;&quot;,
                        &quot;type&quot;: &quot;&lt;MESSAGE_TYPE&gt;&quot;,
                        &quot;&lt;MESSAGE_TYPE&gt;&quot;: &#123;
                          &lt;MESSAGE_CONTENTS&gt;
                        &#125;,
                        &quot;history_context&quot;: &#123;
                          &quot;status&quot;: &quot;&lt;MESSAGE_STATUS&gt;&quot;
                        &#125;
                      &#125;,
                      /* Additional message objects in thread would follow, if any */
                    ]
                  &#125;,
                  /* Additional chat history thread objects would follow, if any */
                ]
              &#125;
            ]
          &#125;,
          &quot;field&quot;: &quot;history&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

### Parameters

| Placeholder | Description | Example value |
| --- | --- | --- |
| `&lt;BUSINESS_OR_WHATSAPP_USER_PHONE_NUMBER&gt;`&lt;br&gt;&lt;br&gt;_String_ | The business customer&#039;s phone number, or the WhatsApp user&#039;s phone number.&lt;br&gt;&lt;br&gt;If the value is the business&#039;s phone number, the message object describes a message sent by the business to a WhatsApp user.&lt;br&gt;&lt;br&gt;If the value is the WhatsApp user&#039;s phone number, the message object describes a message sent by the WhatsApp user to the business. | `15550783881` |
| `&lt;CHUNK_ORDER&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | Indicates [chunk](#phases-and-chunks) number, which you can use to order sets of webhooks sequentially. | `1` |
| `&lt;CUSTOMER_WABA_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | The business customer&#039;s WhatsApp Business account ID. | `102290129340398` |
| `&lt;CUSTOMER_DISPLAY_PHONE_NUMBER&gt;`&lt;br&gt;&lt;br&gt;_String_ | The business customer&#039;s business phone number. | `15550783881` |
| `&lt;CUSTOMER_PHONE_NUMBER_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | The business customer&#039;s business phone number ID. | `106540352242922` |
| `&lt;DEVICE_TIMESTAMP&gt;`&lt;br&gt;&lt;br&gt;_String_ | Unix timestamp indicating when the message was received by the recipient&#039;s device. | `1738796547` |
| `&lt;MESSAGE_CONTENTS&gt;`&lt;br&gt;&lt;br&gt;_Object_ | An object describing the message&#039;s contents. This value will vary based on the message type, as well as the contents message.&lt;br&gt;&lt;br&gt;For example, if a business sends an `image` message without a caption, the object would not include the `caption` property.&lt;br&gt;&lt;br&gt;See [Sending messages](https://developers.facebook.com/documentation/business-messaging/whatsapp/messages/send-messages) for examples of payloads for each message type. | `&#123;&quot;body&quot;:&quot;Here&#039;s the info you requested! https://www.meta.com/quest/quest-3/&quot;&#125;` |
| `&lt;MESSAGE_STATUS&gt;`&lt;br&gt;&lt;br&gt;_String_ | Indicates the message&#039;s most recent delivery stats. Values can be:&lt;br&gt;&lt;br&gt;- `DELIVERED`&lt;br&gt;- `ERROR`&lt;br&gt;- `PENDING`&lt;br&gt;- `PLAYED`&lt;br&gt;- `READ`&lt;br&gt;- `SENT` | `READ` |
| `&lt;MESSAGE_TYPE&gt;`&lt;br&gt;&lt;br&gt;_String_ | [Message type](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/messages). Note that this placeholder appears twice in the syntax above, as it serves as a placeholder for the `type` property&#039;s value and its matching property name. See the [example payload below](#examples) for a thread with various message types.&lt;br&gt;&lt;br&gt;If this value is set to `media_placeholder`, the message object describes a message that contained a media asset. In this case, the message contents will be omitted. Instead, a separate history webhook will follow, describing the content of the message and the media asset ID, but only if the message was sent within the last two weeks of your query. See the [example payloads below](#examples) describing a media message&#039;s contents. | `text` |
| `&lt;PHASE&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | Indicates history [phase](#phases-and-chunks). Values can be:&lt;br&gt;&lt;br&gt;- `0` — indicates messages are from day 0 (business onboarding time) through day 1&lt;br&gt;- `1` — indicates messages are from day 1 through day 90&lt;br&gt;- `2` — indicates messages are from day 90 through day 180 | `1` |
| `&lt;PROGRESS&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | Indicates percentage total of synchronization progress.&lt;br&gt;&lt;br&gt;Minimum `0`, maximum `100`. | `55` |
| `&lt;WHATSAPP_MESSAGE_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | WhatsApp message ID. | `wamid.HBgLMTY1MDM4Nzk0MzkVAgASGBQzQUFERjg0NDEzNDdFODU3MUMxMAA=` |
| `&lt;WHATSAPP_USER_PHONE_NUMBER&gt;`&lt;br&gt;&lt;br&gt;_String_ | The WhatsApp user&#039;s phone number.&lt;br&gt;&lt;br&gt;The `to` property is only included if the message object represents an [SMB message echo](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/onboarding-business-app-users#step-3--mirror-new-whatsapp-business-app-messages). | `16505551234` |

### Examples

This example webhook describes two message threads: (1) a thread containing a text message and video message sent to a WhatsApp user, and the WhatsApp user&#039;s response, and (2) a text message sent to a different WhatsApp user, thanking them for their order.

Note that the media message&#039;s contents in the first thread are not described. Instead, a [second webhook](#examples) is triggered, describing the media message&#039;s contents.

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
            &quot;history&quot;: [
              &#123;
                &quot;metadata&quot;: &#123;
                  &quot;phase&quot;: 0,
                  &quot;chunk_order&quot;: 1,
                  &quot;progress&quot;: 55
                &#125;,
                &quot;threads&quot;: [
                  &#123;
                    &quot;id&quot;: &quot;16505551234&quot;,
                    &quot;messages&quot;: [
                      &#123;
                        &quot;from&quot;: &quot;15550783881&quot;,
                        &quot;id&quot;: &quot;wamid.HBgLMTY0NjcwNDM1OTUVAgARGBIyNDlBOEI5QUQ4NDc0N0FCNjMA&quot;,
                        &quot;timestamp&quot;: &quot;1739230955&quot;,
                        &quot;type&quot;: &quot;text&quot;,
                        &quot;text&quot;: &#123;
                          &quot;body&quot;: &quot;Here&#039;s the info you requested! https://www.meta.com/quest/quest-3/&quot;
                        &#125;,
                        &quot;history_context&quot;: &#123;
                          &quot;status&quot;: &quot;READ&quot;
                        &#125;
                      &#125;,
                      &#123;
                        &quot;from&quot;: &quot;15550783881&quot;,
                        &quot;id&quot;: &quot;wamid.QyNUEHBgLMTY0NjcwNDM1OTUVAgARGBI1Rj3NEYxMzAzMzQ5MkEA&quot;,
                        &quot;timestamp&quot;: &quot;1739230970&quot;,
                        &quot;type&quot;: &quot;media_placeholder&quot;,
                        &quot;history_context&quot;: &#123;
                          &quot;status&quot;: &quot;PLAYED&quot;
                        &#125;
                      &#125;,
                      &#123;
                        &quot;from&quot;: &quot;16505551234&quot;,
                        &quot;id&quot;: &quot;wamid.N0FCNjMAHBgLMTY0NjcwNDM1OTUVAgARGBIyNDlBOEI5QUQ4NDc0&quot;,
                        &quot;timestamp&quot;: &quot;1739230970&quot;,
                        &quot;type&quot;: &quot;text&quot;,
                        &quot;text&quot;: &#123;
                          &quot;body&quot;: &quot;Thanks!&quot;
                        &#125;,
                        &quot;history_context&quot;: &#123;
                          &quot;status&quot;: &quot;READ&quot;
                        &#125;
                      &#125;
                    ]
                  &#125;,
                  &#123;
                    &quot;id&quot;: &quot;12125557890&quot;,
                    &quot;messages&quot;: [
                      &#123;
                        &quot;from&quot;: &quot;15550783881&quot;,
                        &quot;id&quot;: &quot;wamid.BIyNDlBOEI5N0FCNjMAHBgLMTY0NjcwNDM1OTUVAgARGQUQ4NDc0&quot;,
                        &quot;timestamp&quot;: &quot;1739230970&quot;,
                        &quot;type&quot;: &quot;text&quot;,
                        &quot;text&quot;: &#123;
                          &quot;body&quot;: &quot;Thanks for your order! As a thank you, use code THANKS30 to get 30% of your next order.&quot;
                        &#125;,
                        &quot;history_context&quot;: &#123;
                          &quot;status&quot;: &quot;DELIVERED&quot;
                        &#125;
                      &#125;
                    ]
                  &#125;
                ]
              &#125;
            ]
          &#125;,
          &quot;field&quot;: &quot;history&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

This example webhook describes a media message&#039;s contents.

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
            &quot;messages&quot;: [
              &#123;
                &quot;from&quot;: &quot;16505551234&quot;,
                &quot;id&quot;: &quot;wamid.QyNUEHBgLMTY0NjcwNDM1OTUVAgARGBI1Rj3NEYxMzAzMzQ5MkEA&quot;,
                &quot;timestamp&quot;: &quot;1738796547&quot;,
                &quot;type&quot;: &quot;image&quot;,
                &quot;image&quot;: &#123;
                  &quot;caption&quot;: &quot;Black Prince echeveria&quot;,
                  &quot;mime_type&quot;: &quot;image/jpeg&quot;,
                  &quot;sha256&quot;: &quot;3f9d94d399fa61c191bc1d4ca71375a035cd9b9f5b1128e1f0963a415c16b0cc&quot;,
                  &quot;id&quot;: &quot;24230790383178626&quot;
                &#125;
              &#125;
            ]
          &#125;,
          &quot;field&quot;: &quot;history&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

## Chat history sharing declined

### Syntax

```html
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;metadata&quot;: &#123;
    &quot;display_phone_number&quot;: &quot;&lt;CUSTOMER_DISPLAY_PHONE_NUMBER&gt;&quot;,
    &quot;phone_number_id&quot;: &quot;&lt;CUSTOMER_PHONE_NUMBER_ID&gt;&quot;
  &#125;,
  &quot;history&quot;: [
    &#123;
      &quot;errors&quot;: [
        &#123;
          &quot;code&quot;: 2593109,
          &quot;title&quot;: &quot;History sync is turned off by the business from the WhatsApp Business App&quot;,
          &quot;message&quot;: &quot;History sync is turned off by the business from the WhatsApp Business App&quot;,
          &quot;error_data&quot;: &#123;
            &quot;details&quot;: &quot;History sharing is turned off by the business&quot;
          &#125;
        &#125;
      ]
    &#125;
  ]
&#125;
```

### Parameters

| Placeholder | Description | Example value |
| --- | --- | --- |
| `&lt;CUSTOMER_DISPLAY_PHONE_NUMBER&gt;`&lt;br&gt;&lt;br&gt;_String_ | The business customer&#039;s business phone number. | `15550783881` |
| `&lt;CUSTOMER_PHONE_NUMBER_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | The business customer&#039;s business phone number ID. | `106540352242922` |

### Example

```json
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;metadata&quot;: &#123;
    &quot;display_phone_number&quot;: &quot;15550783881&quot;,
    &quot;phone_number_id&quot;: &quot;106540352242922&quot;
  &#125;,
  &quot;history&quot;: [
    &#123;
      &quot;errors&quot;: [
        &#123;
          &quot;code&quot;: 2593109,
          &quot;title&quot;: &quot;History sync is turned off by the business from the WhatsApp Business App&quot;,
          &quot;message&quot;: &quot;History sync is turned off by the business from the WhatsApp Business App&quot;,
          &quot;error_data&quot;: &#123;
            &quot;details&quot;: &quot;History sharing is turned off by the business&quot;
          &#125;
        &#125;
      ]
    &#125;
  ]
&#125;
```
