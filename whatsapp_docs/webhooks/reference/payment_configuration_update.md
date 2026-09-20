# payment_configuration_update webhook reference



This reference describes trigger events and payload contents for the WhatsApp Business account **payment_configuration_update** webhook.

The **payment_configuration_update** webhook notifies you of changes to payment configurations for [Payments API India](https://developers.facebook.com/documentation/business-messaging/whatsapp/payments/payments-in/overview) and [Payments API Brazil](https://developers.facebook.com/documentation/business-messaging/whatsapp/payments/payments-br/overview).


## Triggers

- The payment configuration associated with a WhatsApp Business account has been connected to a payment gateway account.
- The payment configuration associated with a WhatsApp Business account has been disconnected from a payment gateway account.
- The payment configuration associated with a WhatsApp Business account is now active.

## Syntax

```html
&#123;
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;&quot;,
      &quot;time&quot;: &lt;WEBHOOK_TRIGGER_TIMESTAMP&gt;,
      &quot;changes&quot;: [
        &#123;
          &quot;field&quot;: &quot;payment_configuration_update&quot;,
          &quot;value&quot;: &#123;
            &quot;configuration_name&quot;: &quot;&lt;PAYMENT_CONFIGURATION_NAME&gt;&quot;,
            &quot;provider_name&quot;: &quot;&lt;PAYMENT_GATEWAY_PROVIDER_NAME&gt;&quot;,
            &quot;provider_mid&quot;: &quot;&lt;PAYMENT_GATEWAY_MERCHANT_ACCOUNT_ID&gt;&quot;,
            &quot;status&quot;: &quot;&lt;PAYMENT_CONFIGURATION_STATUS&gt;&quot;,
            &quot;created_timestamp&quot;: &lt;PAYMENT_CONFIGURATION_CREATION_TIMESTAMP&gt;,
            &quot;updated_timestamp&quot;: &lt;PAYMENT_CONFIGURATION_UPDATE_TIMESTAMP&gt;
          &#125;
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
| `&lt;PAYMENT_CONFIGURATION_CREATION_TIMESTAMP&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | UNIX timestamp indicated when the payment configuration was created. | `1748827100` |
| `&lt;PAYMENT_CONFIGURATION_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | Payment configuration name to be used in the Order Details messages. | `razorpay-prod` |
| `&lt;PAYMENT_CONFIGURATION_STATUS&gt;`&lt;br&gt;&lt;br&gt;_String_ | Payment configuration status.&lt;br&gt;&lt;br&gt;Values can be:&lt;br&gt;&lt;br&gt;`Active` — Indicates the payment configuration has been tested in WhatsApp Manager and can now be used with Payments API.&lt;br&gt;&lt;br&gt;`Needs Connecting` — Indicates the payment configuration has been disconnected from the payment gateway and needs to be connected again.&lt;br&gt;&lt;br&gt;`Needs Testing` — Indicates the payment configuration has been connected to the payment gateway but still needs testing in WhatsApp Manager. | `Needs Connecting` |
| `&lt;PAYMENT_CONFIGURATION_UPDATE_TIMESTAMP&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | UNIX timestamp indicated when the payment configuration was updated. | `1749320300` |
| `&lt;PAYMENT_GATEWAY_MERCHANT_ACCOUNT_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | Payment gateway merchant account ID. | `acc_GP4lfNA0iIMn5B` |
| `&lt;PAYMENT_GATEWAY_PROVIDER_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | Name of the payment gateway provider associated with the payment configuration. Values can be:&lt;br&gt;&lt;br&gt;- `billdesk`&lt;br&gt;- `payu`&lt;br&gt;- `razorpay`&lt;br&gt;- `zaakpay` | `razorpay` |
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
          &quot;field&quot;: &quot;payment_configuration_update&quot;,
          &quot;value&quot;: &#123;
            &quot;configuration_name&quot;: &quot;razorpay-prod&quot;,
            &quot;provider_name&quot;: &quot;razorpay&quot;,
            &quot;provider_mid&quot;: &quot;acc_GP4lfNA0iIMn5B&quot;,
            &quot;status&quot;: &quot;Needs Testing&quot;,
            &quot;created_timestamp&quot;: 1748827100,
            &quot;updated_timestamp&quot;: 1749320300
          &#125;
        &#125;
      ]
    &#125;
  ],
  &quot;object&quot;: &quot;whatsapp_business_account&quot;
&#125;
```
