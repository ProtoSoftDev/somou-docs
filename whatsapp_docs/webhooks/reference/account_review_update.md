# account_review_update webhook reference



This reference describes trigger events and payload contents for the WhatsApp Business account (WABA) `account_review_update` webhook.

The **account_review_update** webhook notifies you when a WhatsApp Business Account has been reviewed against our policy guidelines.


## Triggers

WhatsApp triggers the `account_review_update` webhook when:

- A WhatsApp Business account is approved.
- A WhatsApp Business account is rejected.
- A decision on a WhatsApp Business account approval has been deferred or is awaiting more information.

## Syntax

The `account_review_update` field has the following payload syntax:

```html
&#123;
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;&quot;,
      &quot;time&quot;: &lt;WEBHOOK_TRIGGER_TIMESTAMP&gt;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;decision&quot;: &quot;&lt;DECISION&gt;&quot;
          &#125;,
          &quot;field&quot;: &quot;account_review_update&quot;
        &#125;
      ]
    &#125;
  ],
  &quot;object&quot;: &quot;whatsapp_business_account&quot;
&#125;
```

## Payload parameters

| Placeholder | Description | Example value |
| --- | --- | --- |
| `&lt;DECISION&gt;`&lt;br&gt;&lt;br&gt;_String_ | Indicates WABA review outcome.&lt;br&gt;&lt;br&gt;Values can be:&lt;br&gt;&lt;br&gt;`APPROVED` — Indicates the WABA is approved and ready for use.&lt;br&gt;&lt;br&gt;`REJECTED` — Indicates the WABA was rejected because it doesn&#039;t meet our policy requirements and cannot be used with our APIs.&lt;br&gt;&lt;br&gt;`PENDING` — Indicates a review decision is still pending and the WABA currently cannot be used with our APIs.&lt;br&gt;&lt;br&gt;`DEFERRED` — Indicates a review decision has been deferred and the WABA currently cannot be used with our APIs. | `APPROVED` |
| `&lt;WEBHOOK_TRIGGER_TIMESTAMP&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | Unix timestamp indicating when the webhook was triggered. | `1739321024` |
| `&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | WhatsApp Business Account ID. | `102290129340398` |

## Example payload

```json
&#123;
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;102290129340398&quot;,
      &quot;time&quot;: 1739321024,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;decision&quot;: &quot;APPROVED&quot;
          &#125;,
          &quot;field&quot;: &quot;account_review_update&quot;
        &#125;
      ]
    &#125;
  ],
  &quot;object&quot;: &quot;whatsapp_business_account&quot;
&#125;
```
