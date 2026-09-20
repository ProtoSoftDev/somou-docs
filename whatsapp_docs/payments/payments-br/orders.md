# Orders



Payments API introduces two new types of [interactive messages](https://developers.facebook.com/documentation/business-messaging/whatsapp/messages/send-messages#interactive-messages):  `order_details` and `order_status`. They are the entrypoint to collect payment in WhatsApp.

1. `order_details` messages are sent to create an order in the buyer&#039;s WhatsApp client app. This message includes the payment settings used to collect payment and can optionally include an `order` object with itemized products, fees, and discounts. Without the `order` object, you can send a simplified order details message with just the total amount and payment settings. The payment settings will vary depending on the integration type ([Pix](https://developers.facebook.com/documentation/business-messaging/whatsapp/payments/payments-br/offsite-pix), [payment links](https://developers.facebook.com/documentation/business-messaging/whatsapp/payments/payments-br/payment-links), [Boleto](https://developers.facebook.com/documentation/business-messaging/whatsapp/payments/payments-br/boleto), [One Click Payments](https://developers.facebook.com/documentation/business-messaging/whatsapp/payments/payments-br/one-click-payments)).
2. `order_status` messages are sent when businesses update the order status either based on the WhatsApp payment status change notification or based on their internal processes. You can also send a simplified status update without the `order` object.

When attached to an order details message, orders start in `pending` status. When the merchant has fully fulfilled the order and the buyer should not expect any further updates, the order must be marked as `completed`.

## Sending order messages
Both message types contain the same 4 main components of an interactive message: *header*, *body*, *footer*, and *action*. The parameters in the *action* component will vary based on the message type. In addition, both `order_details` and `order_status` messages can optionally include an `order` object containing a list of items, fees, and other details about the order.

Once the interactive message object is assembled, make a POST call to the [messages endpoint](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/message-api#messages). Remember to set the type to `interactive`.

### Order details example

The following images show how the full and simplified `order_details` messages appear in WhatsApp. The full version includes itemized products, while the simplified version displays only the total amount.

#### Endpoint

```bash
POST /&#123;PHONE_NUMBER_ID&#125;/messages
```

#### Request body

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
            &quot;type&quot;: &quot;payment_link&quot;,
            &quot;payment_link&quot;: &#123;
              &quot;uri&quot;: &quot;https://my-payment-link-url&quot;
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

### Simplified order details message

You can send a simplified order details message without the `order` object. The simplified order details message is useful when you don&#039;t need to send itemized product details and only need to collect the total payment amount.

Simplified order details messages do not support an image header. Instead of the thumbnail image, the total payment amount is displayed prominently at the top of the message. If an image is a required part of your use case, consider using the [Payment Request CTA Templates](https://developers.facebook.com/documentation/business-messaging/whatsapp/payments/payments-br/payment-request-cta) solution instead.

#### Endpoint

```bash
POST /&#123;PHONE_NUMBER_ID&#125;/messages
```

#### Request body

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
            &quot;type&quot;: &quot;payment_link&quot;,
            &quot;payment_link&quot;: &#123;
              &quot;uri&quot;: &quot;https://my-payment-link-url&quot;
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

### Order status example

The following images show how the `order_status` message appears in WhatsApp after the payment is confirmed, for both the full and simplified versions.

#### Endpoint

```bash
POST /&#123;PHONE_NUMBER_ID&#125;/messages
```

#### Request body

```json
&#123;
  &quot;recipient_type&quot;: &quot;individual&quot;,
  &quot;to&quot;: &quot;&lt;PHONE_NUMBER&gt;&quot;,
  &quot;type&quot;: &quot;interactive&quot;,
  &quot;interactive&quot;: &#123;
    &quot;type&quot;: &quot;order_status&quot;,
    &quot;body&quot;: &#123;
      &quot;text&quot;: &quot;your-mandatory-text-body-content&quot;
    &#125;,
    &quot;footer&quot;: &#123;
      &quot;text&quot;: &quot;your-optional-text-footer-content&quot;
    &#125;,
    &quot;action&quot;: &#123;
      &quot;name&quot;: &quot;review_order&quot;,
      &quot;parameters&quot;: &#123;
        &quot;reference_id&quot;: &quot;unique-reference-id&quot;,
        &quot;order&quot;: &#123;
          &quot;status&quot;: &quot;processing&quot;
        &#125;,
        &quot;payment&quot;: &#123;
          &quot;status&quot;: &quot;captured&quot;,
          &quot;timestamp&quot;: 1722445231
        &#125;
      &#125;
    &#125;
  &#125;
&#125;
```

### Simplified order status example

#### Endpoint

```bash
POST /&#123;PHONE_NUMBER_ID&#125;/messages
```

#### Request body

```json
&#123;
  &quot;recipient_type&quot;: &quot;individual&quot;,
  &quot;to&quot;: &quot;&lt;PHONE_NUMBER&gt;&quot;,
  &quot;type&quot;: &quot;interactive&quot;,
  &quot;interactive&quot;: &#123;
    &quot;type&quot;: &quot;order_status&quot;,
    &quot;body&quot;: &#123;
      &quot;text&quot;: &quot;your-mandatory-text-body-content&quot;
    &#125;,
    &quot;footer&quot;: &#123;
      &quot;text&quot;: &quot;your-optional-text-footer-content&quot;
    &#125;,
    &quot;action&quot;: &#123;
      &quot;name&quot;: &quot;review_order&quot;,
      &quot;parameters&quot;: &#123;
        &quot;reference_id&quot;: &quot;unique-reference-id&quot;,
        &quot;payment&quot;: &#123;
          &quot;status&quot;: &quot;captured&quot;,
          &quot;timestamp&quot;: 1722445231
        &#125;
      &#125;
    &#125;
  &#125;
&#125;
```

### Message response
For either type, if your message is sent successfully, you will get the following response:

```json
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;contacts&quot;: [
    &#123;
      &quot;input&quot;: &quot;[PHONE_NUMBER_ID]&quot;,
      &quot;wa_id&quot;: &quot;[PHONE-NUMBER_ID]&quot;
    &#125;
  ],
  &quot;messages&quot;: [
    &#123;
      &quot;id&quot;: &quot;wamid.HBgLMTY1MDUwNzY1MjAVAgARGBI5QTNDQTVCM0Q0Q0Q2RTY3RTcA&quot;
    &#125;
  ]
&#125;
```

For all errors that can be returned and guidance on how to handle them, see [Cloud API Errors Codes](https://developers.facebook.com/documentation/business-messaging/whatsapp/support/error-codes).

## Full API reference

### Order details

To send an order_details message, businesses must assemble an interactive object of type order_details with the following components:

#### Interactive object
| **Field Name** | **Optional?** | **Type** | **Description** |
| --- | --- | --- | --- |
| type | Required | String | Must be `order_details`. |
| header | Optional | Object | Thumbnail image for order details message. It has the following fields:&lt;br&gt;&lt;br&gt;- `type`: Must be `image`.&lt;br&gt;- `image`: See [Image Object](#imageobject).&lt;br&gt;&lt;br&gt;If the header is not present, the API finds the first product with an image and uses the product image for the thumbnail image.&lt;br&gt; |
| body | Required | Object | An object with the body of the message. The object contains the following field:&lt;br&gt;&lt;br&gt;- `text` string: The content of the message. Emojis and markdown are supported. Maximum length is 1024 characters. |
| footer | Optional | Object | An object with the footer of the message. The object contains the following field:&lt;br&gt;&lt;br&gt;- `text` string: **Required** if footer is present. The footer content. Emojis, markdown, and links are supported. Maximum length is 60 characters. |
| action | Required | Action Object | See [Action Object](#actionobject) below. |

#### Image object &#123;#imageobject&#125;
| Field Name | Optional? | Type | Description |
| --- | --- | --- | --- |
| link | Required | String | Url of the image. |

#### Action object &#123;#actionobject&#125;
| Field Name | Optional? | Type | Description |
| --- | --- | --- | --- |
| name | Required | String | Must be `review_and_pay`. |
| parameters | Required | Parameters Object | See [Parameters Object](#paramsobject). |

#### Parameters object &#123;#paramsobject&#125;

| **Field Name** | **Optional?** | **Type** | **Description** |
| --- | --- | --- | --- |
| reference_id | Required | String | Unique identifier for the order or invoice provided by the business. This cannot be an empty string and can only contain English letters, numbers, underscores, dashes, or dots, and should not exceed 60 characters.&lt;br&gt;&lt;br&gt;The reference_id must be unique for each order_details message for the same business. If the partner would like to send multiple order_details messages for the same order, invoice, and so on, include a sequence number in the reference_id to ensure reference_id uniqueness. |
| type | Required | String | Must be one of `digital-goods` or `physical-goods`. |
| payment_type | Required | String | Must be `br`. |
| payment_settings | Optional | [Payment Settings Object](#paymentsettingsobject) | List of payment related configuration objects. |
| currency | Required | String | ISO 4217 currency code for the order. Must be `BRL` (Brazilian Real). |
| total_amount | Required | Amount Object | See [Amount Object](#amountobject).&lt;br&gt;**Warning:** `total_amount.value` must be equal to `order.subtotal.value` + `order.tax.value` + `order.shipping.value` - `order.discount.value` |
| order | Optional | Order Object | See [Order Object](#orderobject). |

#### Payment settings &#123;#paymentsettingsobject&#125;
| Field Name | Optional? | Type | Description |
| --- | --- | --- | --- |
| `type` | Required | String | One of `pix_dynamic_code`, `payment_link`, `boleto`. |
| One of the following objects: `pix_dynamic_code`, `payment_link`, `boleto`. | Required | Object | Payment instructions which will be displayed to buyers during the checkout process. |

#### Order object &#123;#orderobject&#125;

| **Field Name** | **Optional?** | **Type** | **Description** |
| --- | --- | --- | --- |
| status | Required | String | Status of the order. Only supported value here is `pending`. |
| catalog_id | Optional | String | Unique identifier of the Facebook catalog being used by the business. |
| expiration | Optional | Expiration Object | Expiration for that order. The CTA for payment will be disabled after expiry on the user end. See [Expiration Object](#expirationobject). |
| items | Required | List of Item Objects | List must have at least one item. See [Item Object](#itemobject). |
| subtotal | Required | Amount Object | See [Amount Object](#amountobject).&lt;br&gt;&lt;br&gt;**Warning:** The value **must be equal** to sum of (`item.amount.value` or `item.sale_amount.value`) * `item.quantity`.&lt;br&gt;&lt;br&gt;The following fields are part of the `subtotal` object:&lt;br&gt;&lt;br&gt;`offset` string&lt;br&gt;&lt;br&gt;- **Required.** Must be `100` for `BRL`.&lt;br&gt;&lt;br&gt;`value` string&lt;br&gt;&lt;br&gt;- **Required.** Positive integer representing the amount value multiplied by offset. For example, S$12.34 has value 1234. |
| tax | Required | Amount With Description Object | The tax information for this order. Even though the object is required, the amount can be zero. When zero is used, the tax line is not rendered in the client. See [Amount With Description Object](#amountdescriptionobject). |
| shipping | Optional | Amount With Description Object | See [Amount Object](#amountdescriptionobject). |
| discount | Optional | Discount Object | The discount for the order. See [Discount object](#discountobject). |

#### Expiration object &#123;#expirationobject&#125;
| **Field Name** | **Optional?** | **Type** | **Description** |
| --- | --- | --- | --- |
| timestamp | Required | String | UTC time in seconds. Minimum threshold is 300 seconds. |
| description | Required | String | Text explanation for when the order will expire. Max character limit is 120 characters. |

#### Item object &#123;#itemobject&#125;
| **Field Name** | **Optional?** | **Type** | **Description** |
| --- | --- | --- | --- |
| retailer_id | Required | String | Content ID for an item in the order from your catalog. |
| name | Required | String | The item&#039;s name to be displayed to the user. Cannot exceed 60 characters. |
| amount | Required | Amount Object | The price per item. See [Amount Object](#amountobject). |
| quantity | Required | Integer | Number of items in this order. |
| sale_amount | Optional | Amount Object | The discounted price per item. This should be less than the original amount. If included, this field is used to calculate the subtotal amount. See [Amount Object](#amountobject). |

#### Discount object &#123;#discountobject&#125;
| **Field Name** | **Optional?** | **Type** | **Description** |
| --- | --- | --- | --- |
| value | Required | Integer | Positive integer representing the amount value multiplied by offset. For example, 12.34 BRL has value 1234. |
| offset | Required | Integer | Must be `100` for `BRL`. |
| description | Optional | String | Max character limit is 60 characters. |
| discount_program_name | Optional | String | Text used for defining incentivized orders. If order is incentivized, the merchant needs to define this information. Max character limit is 60 characters. |

#### Amount object &#123;#amountobject&#125;
| **Field Name** | **Optional?** | **Type** | **Description** |
| --- | --- | --- | --- |
| value | Required | Integer | Positive integer representing the amount value multiplied by offset. For example, 12.34 BRL has value 1234. |
| offset | Required | Integer | Must be `100` for `BRL`. |

#### Amount object (with description) &#123;#amountdescriptionobject&#125;
| **Field Name** | **Optional?** | **Type** | **Description** |
| --- | --- | --- | --- |
| value | Required | Integer | Positive integer representing the amount value multiplied by offset. For example, 12.34 BRL has value 1234. |
| offset | Required | Integer | Must be `100` for `BRL`. |
| description | Optional | String | Max character limit is 60 characters. |

### Order status
To send an order_status message, businesses must assemble an interactive object of type order_status with the following components:

#### Interactive object
| **Field Name** | **Optional?** | **Type** | **Description** |
| --- | --- | --- | --- |
| type | Required | String | Must be `order_status`. |
| header | Optional | Object | Optional object for the message&#039;s header for the message. |
| body | Required | Object | An object with the body of the message. The object contains the following field:&lt;br&gt;&lt;br&gt;- `text` string: The content of the message. Emojis and markdown are supported. Maximum length is 1024 characters. |
| footer | Optional | Object | An object with the footer of the message. The object contains the following field:&lt;br&gt;&lt;br&gt;- `text` string: **Required** if footer is present. The footer content. Emojis, markdown, and links are supported. Maximum length is 60 characters. |
| action | Required | Action Object | See [Action Object](#statusactionobject) below. |

#### Action object &#123;#statusactionobject&#125;
| Field Name | Optional? | Type | Description |
| --- | --- | --- | --- |
| name | Required | String | Must be `review_order`. |
| parameters | Required | Parameters Object | See [Parameters Object](#statusparamsobject). |

#### Parameters object &#123;#statusparamsobject&#125;
| Field Name | Optional? | Type | Description |
| --- | --- | --- | --- |
| reference_id | Required | String | The unique ID provided in the `order_details` message. |
| order | Optional | Order Object | See [Order Object](#statusorderobject). |
| payment | Optional | Payment Object | See [Payment Object](#statuspaymentobject). |

#### Order object &#123;#statusorderobject&#125;
| Field Name | Optional? | Type | Description |
| --- | --- | --- | --- |
| status | Required | String | The new order status. [See supported order status](#orderstatussupported). |
| description | Optional | String | Optional text for sharing status related information in order-details page. Could be useful while sending cancellation. Length should not exceed 120 characters. |

#### Payment object &#123;#statuspaymentobject&#125;
| Field Name | Optional? | Type | Description |
| --- | --- | --- | --- |
| status | Required | String | The new payment status. [See supported payment status](#paymentstatussupported). |
| timestamp | Optional | Integer | Optional epoch timestamp in seconds. |

#### Supported order status &#123;#orderstatussupported&#125;
The following order status values are supported:

| Value | Description |
| --- | --- |
| `pending` | Order is pending / not processed yet. |
| `processing` | Merchant/partner is fulfilling the order, performing service, and so on. |
| `partially_shipped` | Part of the products in order have been shipped by the merchant. |
| `shipped` | All the products in order have been shipped by the merchant. |
| `completed` | The order is completed and no further action is expected from the user or the partner/merchant. |
| `canceled` | The partner/merchant would like to cancel the order_details message for the order/invoice. The status update will fail if there is already a successful or pending payment for this order_details message. |

#### Supported payment status &#123;#paymentstatussupported&#125;
The following payment status values are supported:

| Value | Description |
| --- | --- |
| `pending` | Payment is pending. |
| `captured` | Payment was successfully captured. Receiving this payment status will update the order bubble to include the &quot;paid&quot; label (with green checkmark). |
| `failed` | Payment failed. |

## Errors and statuses
These are the relevant errors for the WhatsApp Payments API:
| Error Code | Description |
| --- | --- |
| `2040 - Message is not supported` | The message you are trying to send cannot be received by this user |
| `2046 - Order status invalid transition` | The order status cannot be updated from the existing value to the new one |
| `2047 - Order cancellation failure` | The order could not be cancelled |

For a comprehensive list with detailed descriptions of error codes and HTTP status codes, please refer to our [Error Codes](https://developers.facebook.com/documentation/business-messaging/whatsapp/support/error-codes) document.
