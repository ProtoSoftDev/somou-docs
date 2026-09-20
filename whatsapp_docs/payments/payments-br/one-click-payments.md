# One-Click Payments



**Warning:** This feature is not publicly available yet and is only available for businesses based in Brazil and their Brazilian customers. To enable payments for your businesses, please contact your Solution Partner.

Payments API also enables businesses to collect payments from their customers via WhatsApp using One-Click Payments.

When using this integration, WhatsApp facilitates communication between merchants and buyers. Merchants are responsible for storing payment credentials and integrating with a payment service provider (PSP) to submit these credentials, completing, and confirming their payments.

## Before you start

1. Familiarize yourself with the [Orders API](https://developers.facebook.com/documentation/business-messaging/whatsapp/payments/payments-br/orders). Orders are the entrypoint for collecting payments in WhatsApp.
1. You will need an existing integration with a PSP and do automatic reconciliation when a payment is made.
1. You must update the order status as soon as a payment is made.

## Integration steps
The following sequence diagram shows the typical integration with One-Click Payments.

### 1. Send an order details message
Follow the full integration guide in the [Orders API page](https://developers.facebook.com/documentation/business-messaging/whatsapp/payments/payments-br/orders).

If One-Click Payments is available on this order, you will need to provide an `offsite_card_pay` object in the `payment_settings` attribute. You can optionally include an `order` object with itemized products, fees, and discounts, or send a simplified message with just the total amount and payment settings.

The following images show how the order details message with One-Click Payments appears in WhatsApp, in both full and simplified versions.

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
        &quot;reference_id&quot;: &quot;&lt;UNIQUE_REFERENCE_ID&gt;&quot;,
        &quot;type&quot;: &quot;digital-goods&quot;,
        &quot;payment_type&quot;: &quot;br&quot;,
        &quot;payment_settings&quot;: [
          &#123;
            &quot;type&quot;: &quot;offsite_card_pay&quot;,
            &quot;offsite_card_pay&quot;: &#123;
              &quot;last_four_digits&quot;: &quot;5235&quot;,
              &quot;credential_id&quot;: &quot;1234567&quot;
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

You can send a simplified order details message without the `order` object. The simplified payload is useful when you don&#039;t need to send itemized product details and only need to collect the total payment amount.

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
        &quot;reference_id&quot;: &quot;&lt;UNIQUE_REFERENCE_ID&gt;&quot;,
        &quot;type&quot;: &quot;digital-goods&quot;,
        &quot;payment_type&quot;: &quot;br&quot;,
        &quot;payment_settings&quot;: [
          &#123;
            &quot;type&quot;: &quot;offsite_card_pay&quot;,
            &quot;offsite_card_pay&quot;: &#123;
              &quot;last_four_digits&quot;: &quot;5235&quot;,
              &quot;credential_id&quot;: &quot;1234567&quot;
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
| `type` | Required | String | Must be `offsite_card_pay`. |
| `offsite_card_pay` | Required | [Offsite Card Pay Object](#offsitecardpayobject) | Offsite Card Pay object that will be used to render the option to buyers during the checkout flow. |

#### Offsite card pay object &#123;#offsitecardpayobject&#125;
| **Field Name** | **Optional?** | **Type** | **Description** |
| --- | --- | --- | --- |
| `last_four_digits` | Required | String | The last four digits of the card, which will be displayed to the user for confirmation before they accept the payment (by tapping the &quot;Send payment&quot; CTA button). |
| `credential_id` | Required | String | The ID of the credential associated with the card. After the user taps the &quot;Send Payment&quot; CTA button, the merchant will receive a webhook from Meta notifying that confirmation from the user. The payload of that webhook will contain this credential_id, which the merchant will use to determine the card or credential for payments. |

### 2. Receive the webhook notification

After the WhatsApp user taps &quot;Review payment&quot;, they see the payment confirmation screen shown below.

This webhook confirms the buyer&#039;s intention to make a payment and includes the ID of the credential to use.

#### Webhook notification payload example

```json
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
            &quot;messages&quot;: [
              &#123;
                &quot;from&quot;: &quot;&lt;WHATSAPP_USER_PHONE_NUMBER&gt;&quot;,
                &quot;id&quot;: &quot;&lt;WHATSAPP_MESSAGE_ID&gt;&quot;,
                &quot;timestamp&quot;: &quot;&lt;WEBHOOK_SENT_TIMESTAMP&gt;&quot;,
                &quot;from_logical_id&quot;: &quot;64244926160970&quot;,
                &quot;type&quot;: &quot;interactive&quot;,
                &quot;interactive&quot;: &#123;
                  &quot;type&quot;: &quot;payment_method&quot;,
                  &quot;payment_method&quot;: &#123;
                    &quot;payment_method&quot;: &quot;offsite_card_pay&quot;,
                    &quot;payment_timestamp&quot;: 1726170122,
                    &quot;reference_id&quot;: &quot;pix_test_webhook&quot;,
                    &quot;last_four_digits&quot;: &quot;5235&quot;,
                    &quot;credential_id&quot;: &quot;1234567&quot;
                  &#125;
                &#125;
              &#125;
            ]
          &#125;,
          &quot;field&quot;: &quot;messages&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

### 3. Send an order status update

Once the payment is confirmed, you must send an order status update. Follow the integration guide in the [Orders API page](https://developers.facebook.com/documentation/business-messaging/whatsapp/payments/payments-br/orders).
