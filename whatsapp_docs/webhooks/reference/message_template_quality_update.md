# message_template_quality_update webhook reference



This reference describes trigger events and payload contents for the WhatsApp Business account `message_template_quality_update` webhook.

The **message_template_quality_update** webhook notifies you of changes to a template&#039;s [quality score](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/template-quality).


## Triggers

- A template&#039;s quality score changes.

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
            &quot;previous_quality_score&quot;: &quot;&lt;PREVIOUS_QUALITY_SCORE&gt;&quot;,
            &quot;new_quality_score&quot;: &quot;&lt;NEW_QUALITY_SCORE&gt;&quot;,
            &quot;message_template_id&quot;: &lt;TEMPLATE_ID&gt;,
            &quot;message_template_name&quot;: &quot;&lt;TEMPLATE_NAME&gt;&quot;,
            &quot;message_template_language&quot;: &quot;&lt;TEMPLATE_LANGUAGE_AND_LOCALE_CODE&gt;&quot;
          &#125;,
          &quot;field&quot;: &quot;message_template_quality_update&quot;
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
| `&lt;NEW_QUALITY_SCORE&gt;`&lt;br&gt;&lt;br&gt;_String_ | New template [quality score](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/template-quality).&lt;br&gt;&lt;br&gt;Values can be:&lt;br&gt;&lt;br&gt;`GREEN` — Indicates high quality.&lt;br&gt;&lt;br&gt;`RED` — Indicates low quality.&lt;br&gt;&lt;br&gt;`YELLOW` — Indicates medium quality.&lt;br&gt;&lt;br&gt;`UNKNOWN` — Indicates quality pending. | `GREEN` |
| `&lt;PREVIOUS_QUALITY_SCORE&gt;`&lt;br&gt;&lt;br&gt;_String_ | Previous template [quality score](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/template-quality).&lt;br&gt;&lt;br&gt;Values can be:&lt;br&gt;&lt;br&gt;`GREEN` — Indicates high quality.&lt;br&gt;&lt;br&gt;`RED` — Indicates low quality.&lt;br&gt;&lt;br&gt;`YELLOW` — Indicates medium quality.&lt;br&gt;&lt;br&gt;`UNKNOWN` — Indicates quality pending. | `YELLOW` |
| `&lt;TEMPLATE_ID&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | Template ID. | `806312974732579` |
| `&lt;TEMPLATE_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | Template name. | `welcome_template` |
| `&lt;TEMPLATE_LANGUAGE_AND_LOCALE_CODE&gt;`&lt;br&gt;&lt;br&gt;_String_ | Template [language and locale](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/supported-languages) code. | `en-US` |
| `&lt;WEBHOOK_TRIGGER_TIMESTAMP&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | Unix timestamp indicating when the webhook was triggered. | `1739321024` |
| `&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | WhatsApp Business Account ID. | `102290129340398` |

## Example

```json
&#123;
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;102290129340398&quot;,
      &quot;time&quot;: 1674864290,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;previous_quality_score&quot;: &quot;GREEN&quot;,
            &quot;new_quality_score&quot;: &quot;YELLOW&quot;,
            &quot;message_template_id&quot;: 806312974732579,
            &quot;message_template_name&quot;: &quot;welcome_template&quot;,
            &quot;message_template_language&quot;: &quot;en-US&quot;
          &#125;,
          &quot;field&quot;: &quot;message_template_quality_update&quot;
        &#125;
      ]
    &#125;
  ],
  &quot;object&quot;: &quot;whatsapp_business_account&quot;
&#125;
```
