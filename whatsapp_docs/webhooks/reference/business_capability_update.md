# business_capability_update webhook reference



This reference describes trigger events and payload contents for the WhatsApp Business account **business_capability_update** webhook.

The **business_capability_update** webhook notifies you of WhatsApp Business Account or business portfolio capability changes ([messaging limits](https://developers.facebook.com/documentation/business-messaging/whatsapp/messaging-limits#increasing-your-limit), [phone number limits](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-phone-numbers/phone-numbers#registered-number-cap), etc.).


## Triggers

- A WhatsApp Business account is created.
- A WhatsApp Business account or business portfolio business capability (for example, [messaging limits](https://developers.facebook.com/documentation/business-messaging/whatsapp/messaging-limits#increasing-your-limit), [phone number limits](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-phone-numbers/phone-numbers#registered-number-limits)) is increased or decreased.

## Syntax

```html
&#123;
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;&quot;,
      &quot;time&quot;: &lt;WEBHOOK_TRIGGER_TIMESTAMP&gt;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;max_daily_conversation_per_phone&quot;: &lt;MAX_DAILY_CONVERSATIONS_PER_PHONE&gt;,
            &quot;max_daily_conversations_per_business&quot;: &lt;MAX_DAILY_CONVERSATIONS_PER_BUSINESS&gt;,
            &quot;max_phone_numbers_per_business&quot;: &lt;MAX_PHONES_PER_BUSINESS_PORTFOLIO&gt;,
            &quot;max_phone_numbers_per_waba&quot;: &lt;MAX_PHONES_PER_WHATSAPP_BUSINESS_ACCOUNT&gt;
          &#125;,
          &quot;field&quot;: &quot;business_capability_update&quot;
        &#125;
      ]
    &#125;
  ],
  &quot;object&quot;: &quot;whatsapp_business_account&quot;
&#125;
```

## Parameters

| Placeholder | Description | Example value |
| --- | --- | --- |
| `&lt;MAX_DAILY_CONVERSATIONS_PER_PHONE&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | **This parameter will be removed in February, 2026. Use `max_daily_conversations_per_business` instead.**&lt;br&gt;&lt;br&gt;Business portfolio&#039;s [messaging limit](https://developers.facebook.com/documentation/business-messaging/whatsapp/messaging-limits). Values can be:&lt;br&gt;&lt;br&gt;- `250`&lt;br&gt;- `2000`&lt;br&gt;- `10000`&lt;br&gt;- `100000`&lt;br&gt;- `-1`&lt;br&gt;&lt;br&gt;A value of `-1` indicates unlimited messaging. | `2000` |
| `&lt;MAX_DAILY_CONVERSATIONS_PER_BUSINESS&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | Business portfolio&#039;s [messaging limit](https://developers.facebook.com/documentation/business-messaging/whatsapp/messaging-limits).&lt;br&gt;&lt;br&gt;Value can be:&lt;br&gt;&lt;br&gt;- `TIER_250`&lt;br&gt;- `TIER_2K`&lt;br&gt;- `TIER_10K`&lt;br&gt;- `TIER_100K`&lt;br&gt;- `TIER_UNLIMITED` | `TIER_UNLIMITED` |
| `&lt;MAX_PHONES_PER_BUSINESS_PORTFOLIO&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | Maximum number of business phone numbers the business portfolio can have.&lt;br&gt;&lt;br&gt;This property is only included if `max_daily_conversation_per_phone` is set to `250`. | `2` |
| `&lt;MAX_PHONES_PER_WHATSAPP_BUSINESS_ACCOUNT&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | Maximum number of business phone numbers allowed per WABA.&lt;br&gt;&lt;br&gt;This property is only included if `max_daily_conversation_per_phone` is **not** set to `250`. | `25` |
| `&lt;WEBHOOK_TRIGGER_TIMESTAMP&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | Unix timestamp indicating when the webhook was triggered. | `1739321024` |
| `&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | WhatsApp Business Account ID. | `102290129340398` |

## Payload example

```json
&#123;
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;524126980791429&quot;,
      &quot;time&quot;: 1739321024,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;max_daily_conversations_per_business&quot;: 2000,
            &quot;max_phone_numbers_per_waba&quot;: 25
          &#125;,
          &quot;field&quot;: &quot;business_capability_update&quot;
        &#125;
      ]
    &#125;
  ],
  &quot;object&quot;: &quot;whatsapp_business_account&quot;
&#125;
```
