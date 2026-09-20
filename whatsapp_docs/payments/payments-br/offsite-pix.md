# Offsite Pix payments



Payments API also enables businesses to collect payments from their customers via WhatsApp using dynamic Pix codes.

When using this integration, WhatsApp only facilitates the communication between merchants and buyers. Merchants are responsible for integrating with a bank or PSP in order to generate dynamic Pix codes, and confirm their payment.

## Before you start

1. Familiarize yourself with the [Orders API](https://developers.facebook.com/documentation/business-messaging/whatsapp/payments/payments-br/orders). Orders are the entrypoint for collecting payments in WhatsApp.
2. You will need an existing integration with a bank or PSP to generate dynamic Pix codes and do automatic reconciliation when a payment is made. You must be able to update the order status as soon as a payment is made.

## Integration steps
The following sequence diagram shows the typical integration with Pix.

### 1. Send an order details message
Follow the full integration guide in the [Orders API page](https://developers.facebook.com/documentation/business-messaging/whatsapp/payments/payments-br/orders).

If Pix is available on this order, you will need to provide a `pix_dynamic_code` to the `payment_settings` attribute. You can optionally include an `order` object with itemized products, fees, and discounts, or send a simplified message with just the total amount and payment settings.

The following images show how the order details message with Pix appears in WhatsApp, in both full and simplified versions.

#### Endpoint

```bash
POST /&#123;PHONE_NUMBER_ID&#125;/messages
```

#### Payload example

```json
&#123;
  &quot;recipient_type&quot;: &quot;individual&quot;,
  &quot;to&quot;: &quot;&lt;PHONE_NUMBER&gt;&quot;,
  &quot;type&quot;: &quot;interactive&quot;,
  &quot;interactive&quot;: &#123;
    &quot;type&quot;: &quot;order_details&quot;,
    &quot;body&quot;: &#123;
      &quot;text&quot;: &quot;Your message content&quot;
    &#125;,
    &quot;action&quot;: &#123;
      &quot;name&quot;: &quot;review_and_pay&quot;,
      &quot;parameters&quot;: &#123;
        &quot;reference_id&quot;: &quot;unique-reference-id&quot;,
        &quot;type&quot;: &quot;digital-goods&quot;,
        &quot;payment_type&quot;: &quot;br&quot;,
        &quot;payment_settings&quot;: [
          &#123;
            &quot;type&quot;: &quot;pix_dynamic_code&quot;,
            &quot;pix_dynamic_code&quot;: &#123;
              &quot;code&quot;: &quot;00020101021226700014br.gov.bcb.pix2548pix.example.com...&quot;,
              &quot;merchant_name&quot;: &quot;Account holder name&quot;,
              &quot;key&quot;: &quot;39580525000189&quot;,
              &quot;key_type&quot;: &quot;CNPJ&quot;
            &#125;
          &#125;
        ],
        &quot;currency&quot;: &quot;BRL&quot;,
        &quot;total_amount&quot;: &#123;
          &quot;value&quot;: 50000,
          &quot;offset&quot;: 100
        &#125;,
        &quot;order&quot;: &#123;
          &quot;status&quot;: &quot;pending&quot;,
          &quot;tax&quot;: &#123;
            &quot;value&quot;: 0,
            &quot;offset&quot;: 100,
            &quot;description&quot;: &quot;optional text&quot;
          &#125;,
          &quot;items&quot;: [
            &#123;
              &quot;retailer_id&quot;: &quot;1234567&quot;,
              &quot;name&quot;: &quot;Cake&quot;,
              &quot;amount&quot;: &#123;
                &quot;value&quot;: 50000,
                &quot;offset&quot;: 100
              &#125;,
              &quot;quantity&quot;: 1
            &#125;
          ],
          &quot;subtotal&quot;: &#123;
            &quot;value&quot;: 50000,
            &quot;offset&quot;: 100
          &#125;
        &#125;
      &#125;
    &#125;
  &#125;
&#125;
```

#### Simplified payload example

You can send a simplified order details message without the `order` object. This is useful when you don&#039;t need to send itemized product details and only need to collect the total payment amount.

```json
&#123;
  &quot;recipient_type&quot;: &quot;individual&quot;,
  &quot;to&quot;: &quot;&lt;PHONE_NUMBER&gt;&quot;,
  &quot;type&quot;: &quot;interactive&quot;,
  &quot;interactive&quot;: &#123;
    &quot;type&quot;: &quot;order_details&quot;,
    &quot;body&quot;: &#123;
      &quot;text&quot;: &quot;Your message content&quot;
    &#125;,
    &quot;action&quot;: &#123;
      &quot;name&quot;: &quot;review_and_pay&quot;,
      &quot;parameters&quot;: &#123;
        &quot;reference_id&quot;: &quot;unique-reference-id&quot;,
        &quot;type&quot;: &quot;digital-goods&quot;,
        &quot;payment_type&quot;: &quot;br&quot;,
        &quot;payment_settings&quot;: [
          &#123;
            &quot;type&quot;: &quot;pix_dynamic_code&quot;,
            &quot;pix_dynamic_code&quot;: &#123;
              &quot;code&quot;: &quot;00020101021226700014br.gov.bcb.pix2548pix.example.com...&quot;,
              &quot;merchant_name&quot;: &quot;Account holder name&quot;,
              &quot;key&quot;: &quot;39580525000189&quot;,
              &quot;key_type&quot;: &quot;CNPJ&quot;
            &#125;
          &#125;
        ],
        &quot;currency&quot;: &quot;BRL&quot;,
        &quot;total_amount&quot;: &#123;
          &quot;value&quot;: 50000,
          &quot;offset&quot;: 100
        &#125;
      &#125;
    &#125;
  &#125;
&#125;
```

#### Parameters object
| Field Name | Optional? | Type | Description |
| --- | --- | --- | --- |
| `payment_settings` | Optional | [Payment Settings Object](#paymentsettingsobject) | List of payment related configuration objects. |

#### Payment settings &#123;#paymentsettingsobject&#125;
| Field Name | Optional? | Type | Description |
| --- | --- | --- | --- |
| `type` | Required | String | Must be `pix_dynamic_code`. |
| `pix_dynamic_code` | Required | [Dynamic Pix Object](#dynamicpixobject) | Dynamic Pix Code object that will be used to render the option to buyers during the checkout flow. |

#### Dynamic Pix code object &#123;#dynamicpixobject&#125;
| **Field Name** | **Optional?** | **Type** | **Description** |
| --- | --- | --- | --- |
| `code` | Required | String | The dynamic Pix code which will be copied by the buyer. |
| `merchant_name` | Required | String | Account holder name. Displayed in-app for the buyer for informational purposes. |
| `key` | Required | String | Pix key. Displayed in-app for the buyer for informational purposes. |
| `key_type` | Required | String | Pix key type. One of `CPF`, `CNPJ`, `EMAIL`, `PHONE` or `EVP`. |

### 2. Send an order status update

Once the payment is confirmed, you must send an order status update. Follow the integration guide in the [Orders API page](https://developers.facebook.com/documentation/business-messaging/whatsapp/payments/payments-br/orders).
