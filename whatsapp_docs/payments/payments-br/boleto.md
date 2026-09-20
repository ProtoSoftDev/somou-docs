# Boleto



Payments API also enables businesses to collect payments from their customers via WhatsApp using Boleto.

When using this integration, WhatsApp only facilitates the communication between merchants and buyers. Merchants are responsible for integrating with a PSP from which they can generate Boleto codes, and confirm their payment.

## Before you start
1. Familiarize yourself with the [Orders API](https://developers.facebook.com/documentation/business-messaging/whatsapp/payments/payments-br/orders). Orders are the entrypoint for collecting payments in WhatsApp.
2. You will need an existing integration with a PSP to generate Boleto codes and do automatic reconciliation when a payment is made.
3. You must update the order status as soon as a payment is made.

## Integration steps
The following sequence diagram shows the typical integration with Boleto.

### 1. Send an order details message
Follow the full integration guide in the [Orders API page](https://developers.facebook.com/documentation/business-messaging/whatsapp/payments/payments-br/orders).

If Boleto payment is available on this order, you will need to provide a `boleto` object in the `payment_settings` attribute. You can optionally include an `order` object with itemized products, fees, and discounts, or send a simplified message with just the total amount and payment settings.

The following images show how the order details message with Boleto appears in WhatsApp, in both full and simplified versions.

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
            &quot;type&quot;: &quot;boleto&quot;,
            &quot;boleto&quot;: &#123;
              &quot;digitable_line&quot;: &quot;03399026944140000002628346101018898510000008848&quot;
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
            &quot;type&quot;: &quot;boleto&quot;,
            &quot;boleto&quot;: &#123;
              &quot;digitable_line&quot;: &quot;03399026944140000002628346101018898510000008848&quot;
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
| `type` | Required | String | Must be `boleto`. |
| `boleto` | Required | [Boleto Object](#boletoobject) | Boleto object used to render the option to buyers during the checkout flow. |

#### Boleto object &#123;#boletoobject&#125;
| **Field Name** | **Optional?** | **Type** | **Description** |
| --- | --- | --- | --- |
| `digitable_line` | Required | String | The Boleto digital line / code copied to the clipboard, when the user taps on the Boleto CTA button. |

### 2. Send an order status update

Once the payment is confirmed, you must send an order status update. Follow the integration guide in the [Orders API page](https://developers.facebook.com/documentation/business-messaging/whatsapp/payments/payments-br/orders).
