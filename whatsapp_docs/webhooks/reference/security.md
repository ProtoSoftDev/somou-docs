# Security webhook reference



This reference describes trigger events and payload contents for the WhatsApp Business account **security** webhook.

The **security** webhook notifies you of changes to a business phone number&#039;s security settings.


## Triggers

- A Meta Business Suite user clicks the **Turn off two-step verification** button in [WhatsApp Manager](https://business.facebook.com/latest/whatsapp_manager/).
- A Meta Business Suite user completes the instructions in the **WhatsApp Two-Step Verification Reset** email to turn off two-step verification.
- A Meta Business Suite user changes or enables the business phone number PIN using [WhatsApp Manager](https://business.facebook.com/latest/whatsapp_manager/).

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
            &quot;requester&quot;: &quot;&lt;META_BUSINESS_SUITE_USER_ID&gt;&quot;
          &#125;,
          &quot;field&quot;: &quot;security&quot;
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
| `&lt;EVENT&gt;`&lt;br&gt;&lt;br&gt;String | The security event that triggered the webhook.&lt;br&gt;&lt;br&gt;Values can be:&lt;br&gt;&lt;br&gt;`PIN_CHANGED` — indicates that a Meta Business Suite user changed or enabled the business phone number&#039;s PIN using [WhatsApp Manager](https://business.facebook.com/latest/whatsapp_manager/).&lt;br&gt;&lt;br&gt;`PIN_RESET_REQUEST` — indicates that a Meta Business Suite user clicked the Turn off two-step verification button in [WhatsApp Manager](https://business.facebook.com/latest/whatsapp_manager/).&lt;br&gt;&lt;br&gt;`PIN_REQUEST_SUCCESS` — indicates that a Meta Business Suite user completed the instructions in the WhatsApp Two-Step Verification Reset email to turn off two-step verification. | `PIN_RESET_REQUEST` |
| `&lt;META_BUSINESS_SUITE_USER_ID&gt;`&lt;br&gt;&lt;br&gt;String | The Meta Business Suite user ID of the user who requested to turn off two-step verification using [WhatsApp Manager](https://business.facebook.com/latest/whatsapp_manager/).&lt;br&gt;&lt;br&gt;This parameter is only included for PIN reset requests. | `61555822107539` |
| `&lt;WEBHOOK_TRIGGER_TIMESTAMP&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | Unix timestamp indicating when the webhook was triggered. | `1739321024` |
| `&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | WhatsApp Business Account ID. | `102290129340398` |

## Example

```json
&#123;
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;102290129340398&quot;,
      &quot;time&quot;: 1748811473,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;display_phone_number&quot;: &quot;15550783881&quot;,
            &quot;event&quot;: &quot;PIN_RESET_REQUEST&quot;,
            &quot;requester&quot;: &quot;61555822107539&quot;
          &#125;,
          &quot;field&quot;: &quot;security&quot;
        &#125;
      ]
    &#125;
  ],
  &quot;object&quot;: &quot;whatsapp_business_account&quot;
&#125;
```
