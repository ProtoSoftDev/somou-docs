# phone_number_quality_update webhook reference



This reference describes trigger events and payload contents for the WhatsApp Business account **phone_number_quality_update** webhook.

The **phone_number_quality_update** webhook notifies you of changes to a business phone number&#039;s [throughput level](https://developers.facebook.com/documentation/business-messaging/whatsapp/throughput).


## Triggers

- A business phone number&#039;s throughput level changes.

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
              &quot;display_phone_number&quot;: &quot;&lt;BUSINESS_DISPLAY_PHONE_NUMBER&gt;&quot;,
              &quot;event&quot;: &quot;&lt;EVENT&gt;&quot;,
              &quot;old_limit&quot;: &quot;&lt;OLD_LIMIT&gt;&quot;, &lt;!-- only included for messaging limit changes --&gt;
              &quot;current_limit&quot;: &quot;&lt;CURRENT_LIMIT&gt;&quot;,
              &quot;max_daily_conversations_per_business&quot;: &quot;&lt;MAX_DAILY_MESSAGES_LIMIT&gt;&quot;
            &#125;,
            &quot;field&quot;: &quot;phone_number_quality_update&quot;
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
| `&lt;BUSINESS_DISPLAY_PHONE_NUMBER&gt;`&lt;br&gt;&lt;br&gt;_String_ | Business display phone number. | `15550783881` |
| `&lt;CURRENT_LIMIT&gt;`&lt;br&gt;&lt;br&gt;_String_ | **This field will be removed in February, 2026. Use `max_daily_conversations_per_business` instead.**&lt;br&gt;&lt;br&gt;Indicates current [messaging limit](https://developers.facebook.com/documentation/business-messaging/whatsapp/messaging-limits) or [throughput](https://developers.facebook.com/documentation/business-messaging/whatsapp/throughput) level.&lt;br&gt;&lt;br&gt;Values can be:&lt;br&gt;&lt;br&gt;`TIER_50` — Indicates a messaging limit of 50.&lt;br&gt;&lt;br&gt;`TIER_250` — Indicates a messaging limit of 250.&lt;br&gt;&lt;br&gt;`TIER_2K` — Indicates a messaging limit of 2,000.&lt;br&gt;&lt;br&gt;`TIER_10K` — Indicates a messaging limit of 10,000.&lt;br&gt;&lt;br&gt;`TIER_100K` — Indicates a messaging limit of 100,000.&lt;br&gt;&lt;br&gt;`TIER_NOT_SET` — Indicates the business phone number has not been used to send a message yet.&lt;br&gt;&lt;br&gt;`TIER_UNLIMITED` — Indicates the business phone number has higher throughput. | `TIER_UNLIMITED` |
| `&lt;EVENT&gt;`&lt;br&gt;&lt;br&gt;_String_ | [Messaging limit](https://developers.facebook.com/documentation/business-messaging/whatsapp/messaging-limits) change or [throughput](https://developers.facebook.com/documentation/business-messaging/whatsapp/throughput) change event.&lt;br&gt;&lt;br&gt;Values can be:&lt;br&gt;&lt;br&gt;`ONBOARDING` — Indicates the business phone number is still being registered.&lt;br&gt;&lt;br&gt;`THROUGHPUT_UPGRADE` — Indicates the business phone number&#039;s throughput level has increased to higher throughput. | `THROUGHPUT_UPGRADE` |
| `&lt;MAX_DAILY_MESSAGES_LIMIT&gt;`&lt;br&gt;&lt;br&gt;_String_ | Indicates a change to the owning business portfolio&#039;s [messaging limit](https://developers.facebook.com/documentation/business-messaging/whatsapp/messaging-limits) or [throughput](https://developers.facebook.com/documentation/business-messaging/whatsapp/throughput) change.&lt;br&gt;&lt;br&gt;Values can be:&lt;br&gt;&lt;br&gt;`TIER_50` — Indicates a messaging limit of 50.&lt;br&gt;&lt;br&gt;`TIER_250` — Indicates a messaging limit of 250.&lt;br&gt;&lt;br&gt;`TIER_2K` — Indicates a messaging limit of 2,000.&lt;br&gt;&lt;br&gt;`TIER_10K` — Indicates a messaging limit of 10,000.&lt;br&gt;&lt;br&gt;`TIER_100K` — Indicates a messaging limit of 100,000.&lt;br&gt;&lt;br&gt;`TIER_NOT_SET` — Indicates the business phone number has not been used to send a message yet.&lt;br&gt;&lt;br&gt;`TIER_UNLIMITED` — Indicates the business phone number has higher throughput. | `TIER_2K` |
| `&lt;OLD_LIMIT&gt;`&lt;br&gt;&lt;br&gt;_String_ | **This parameter will be removed in February, 2026. Use `max_daily_conversations_per_business` instead.**&lt;br&gt;&lt;br&gt;Indicates old [messaging limit](https://developers.facebook.com/documentation/business-messaging/whatsapp/messaging-limits).&lt;br&gt;&lt;br&gt;Values can be:&lt;br&gt;&lt;br&gt;`TIER_50` — Indicates a messaging limit of 50.&lt;br&gt;&lt;br&gt;`TIER_250` — Indicates a messaging limit of 250.&lt;br&gt;&lt;br&gt;`TIER_2K` — Indicates a messaging limit of 2,000.&lt;br&gt;&lt;br&gt;`TIER_10K` — Indicates a messaging limit of 10,000.&lt;br&gt;&lt;br&gt;`TIER_100K` — Indicates a messaging limit of 100,000.&lt;br&gt;&lt;br&gt;`TIER_NOT_SET` — Indicates the business phone number has not been used to send a message yet. | `TIER_UNLIMITED` |
| `&lt;WEBHOOK_TRIGGER_TIMESTAMP&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | Unix timestamp indicating when the webhook was triggered. | `1739321024` |
| `&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | WhatsApp Business Account ID. | `102290129340398` |

## Example

```json
&#123;
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;102290129340398&quot;,
      &quot;time&quot;: 1748454394,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;display_phone_number&quot;: &quot;15550783881&quot;,
            &quot;event&quot;: &quot;THROUGHPUT_UPGRADE&quot;,
            &quot;current_limit&quot;: &quot;TIER_UNLIMITED&quot;
          &#125;,
          &quot;field&quot;: &quot;phone_number_quality_update&quot;
        &#125;
      ]
    &#125;
  ],
  &quot;object&quot;: &quot;whatsapp_business_account&quot;
&#125;
```
