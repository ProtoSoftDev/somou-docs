# user_preferences webhook reference



This reference describes trigger events and payload contents for the WhatsApp Business account **user_preferences** webhook.

The **user_preferences** webhook notifies you of changes to a WhatsApp user&#039;s [marketing message preferences](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/marketing-templates#user-preferences-for-marketing-messages).


## Triggers

- A WhatsApp user stops marketing messages.
- A WhatsApp user resumes marketing messages.

&gt; **Note:** This webhook triggers only when a user stops or resumes marketing messages. It does not trigger when a user indicates **Interested** or **Not interested** feedback through the **Offers and announcements** setting.

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
                  &quot;name&quot;: &quot;&lt;WHATSAPP_USER_NAME&gt;&quot;
                &#125;,
                &quot;wa_id&quot;: &quot;&lt;WHATSAPP_USER_ID&gt;&quot;
              &#125;
            ],
            &quot;user_preferences&quot;: [
              &#123;
                &quot;wa_id&quot;: &quot;&lt;WHATSAPP_USER_ID&gt;&quot;,
                &quot;detail&quot;: &quot;&lt;PREFERENCE_DESCRIPTION&gt;&quot;,
                &quot;category&quot;: &quot;marketing_messages&quot;,
                &quot;value&quot;: &quot;&lt;PREFERENCE&gt;&quot;,
                &quot;timestamp&quot;: &lt;WEBHOOK_SENT_TIMESTAMP&gt;
              &#125;
            ]
          &#125;,
          &quot;field&quot;: &quot;user_preferences&quot;
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
| `&lt;PREFERENCE&gt;`&lt;br&gt;&lt;br&gt;_String_ | [Marketing message preference](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/marketing-templates#user-preferences-for-marketing-messages).&lt;br&gt;&lt;br&gt;Values can be:&lt;br&gt;&lt;br&gt;`stop` — Indicates the WhatsApp user has opted to stop receiving marketing messages from you.&lt;br&gt;&lt;br&gt;`resume` — Indicates the WhatsApp user has opted to resume receiving marketing messages from you. | `stop` |
| `&lt;PREFERENCE_DESCRIPTION&gt;`&lt;br&gt;&lt;br&gt;_String_ | Description of [marketing message preference](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/marketing-templates#user-preferences-for-marketing-messages).&lt;br&gt;&lt;br&gt;Values can be:&lt;br&gt;&lt;br&gt;- `User requested to stop marketing messages`&lt;br&gt;- `User requested to resume marketing messages` | `User requested to stop marketing messages` |
| `&lt;WEBHOOK_TRIGGER_TIMESTAMP&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | Unix timestamp indicating when the webhook was triggered. | `1739321024` |
| `&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | WhatsApp Business Account ID. | `102290129340398` |
| `&lt;WHATSAPP_USER_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | WhatsApp user ID. Note that a WhatsApp user&#039;s ID and phone number may not always match. | `16505551234` |
| `&lt;WHATSAPP_USER_PROFILE_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | WhatsApp user&#039;s name as it appears in their profile in the WhatsApp client. | `Sheena Nelson` |

## Example

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
                &quot;wa_id&quot;: &quot;16505551234&quot;
              &#125;
            ],
            &quot;user_preferences&quot;: [
              &#123;
                &quot;wa_id&quot;: &quot;16505551234&quot;,
                &quot;detail&quot;: &quot;User requested to resume marketing messages&quot;,
                &quot;category&quot;: &quot;marketing_messages&quot;,
                &quot;value&quot;: &quot;resume&quot;,
                &quot;timestamp&quot;: 1731705721
              &#125;
            ]
          &#125;,
          &quot;field&quot;: &quot;user_preferences&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```
