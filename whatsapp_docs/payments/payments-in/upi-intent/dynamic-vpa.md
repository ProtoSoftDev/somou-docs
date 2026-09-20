# Receive UPI payments through WhatsApp (recommended)


**Note:** For businesses working with Billdesk or Zaakpay payment gateways, use the deeper integration with these PGs. Refer to [Payment Gateway Integration Guide](https://developers.facebook.com/documentation/business-messaging/whatsapp/payments/payments-in/pg)

For businesses working with Razorpay, PayU, or Cashfree payment gateways, use a faster integration path. Refer to [Enhanced Payment Links](https://developers.facebook.com/documentation/business-messaging/whatsapp/payments/payments-in/enhanced-payment-links)

**Note:** This is the recommended UPI Intent integration. Using the older payment configuration method? [See previous docs](https://developers.facebook.com/documentation/business-messaging/whatsapp/payments/payments-in/upi-intent/) and plan to migrate to dynamic VPA.

Your business can enable customers to pay for their orders using all the UPI apps installed on their devices via WhatsApp. Businesses can send customers invoice (`order_details`) messages, then get notified about payment status updates via webhook notifications from Payment Gateway.

## Overview

Currently, customers browse business catalogs, add products to cart, and send orders in with our set of commerce messaging solutions, which includes [Single Product Message, Multi Product Message, and Product Detail Page](https://developers.facebook.com/documentation/business-messaging/whatsapp/catalogs/share-products).

With the WhatsApp Payments API, businesses can send customers a bill so the customer can complete their order with all the UPI apps.

## How it works

The business must send an `order_details` message for the consumer to initiate payment. This type of message is a new type of interactive message, which always contains the same 4 main components: **header**, **body**, **footer**, and **action**. Inside the **action** component, the business includes all the information needed for the customer to complete their payment.

An order_details message contains the following fields:

- `upi_intent_link` - Fields that are supplied by your payment gateway and denote where a payment is sent.
- `reference_id` - This is used to track the lifecycle of the order. Payment statuses are published against this ID. This could be order-id or transaction-id used to create the upi-intent at payment gateway.

Once the message is sent, the business waits for a payment or transaction status updates directly from Payment Gateway. Upon receiving payment signal for an order, Business should relay this payment signal to consumer through interactive order status (`order_status`) message.

Updating users about the payment signal is important as this message updates the order details message and order details view for the consumer reflecting the order confirmation from the merchant. This update flow is shown with an example in subsequent sections.

## Purchase flow in app

In the WhatsApp customer app, the purchase flow has the following steps:

- Customer sends an order with selected products to the business, or the business identifies the products that the customer has shown interest to purchase.

- After receiving the order/identifying the product, if a merchant accepts payment methods other than UPI, such as credit cards and payment wallets, then the merchant will send a message to the user to get their preferred payment method for the order.

- When consumers want to pay using UPI payment method, then merchants should retrieve the UPI payment intent by calling Payment Gateway. Merchant needs to use UPI intent to construct order details messages and send it to the consumer.

- When the consumer taps the Pay now/continue button, they are given the option to choose UPI payment apps - WhatsApp or any other UPI payments apps. Consumers may choose any UPI option to pay for the order.

- Consumer pays for the order and the payment method is saved for the future and automatically selected for the next payment transaction. Also, one quick note is the order details screen order-status will continue to show &quot;Order pending&quot; until the merchant sends order status interactive message.

- Once the payment is complete, the business receives a notification from Payment Gateway and the merchant needs to send order status updates to the consumer client notifying consumers about the progress to the order, this will update the order details message CTAs and order details screen - order status description.

## Integration steps

The steps outlined below assume that the business is about to send order details message to the consumer client.

The following sequence diagram demonstrates the typical integration flow for WA Payments API:

### Step 1: Get UPI intent from payment gateway &#123;#step-1&#125;

Once the consumer has expressed their interest to purchase an item using UPI payment method, merchant needs to call payment gateway to create a UPI intent, the following is the sample UPI intent link:

```html
upi://pay?pa=abc&#064;psp&amp;pn=ABC&amp;tr=877376394&amp;
  am=10.00&amp;cu=INR&amp;mode=00&amp;purpose=00&amp;mc=5399&amp;tn=877376394
```

Merchant/Partner could send the entire UPI intent as it is in the `upi_intent_link` type payload. These parameters will be discussed in detail below.

### Step 2: Assemble the interactive object

To send an `order_details` message, businesses must assemble an [interactive object](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/message-api#interactive-object) of type `order_details` with the following components:

| Object | Description |
| --- | --- |
| `type`&lt;br&gt;&lt;br&gt;object | **Required.**&lt;br&gt;&lt;br&gt;Must be &quot;order_details&quot;. |
| `header`&lt;br&gt;&lt;br&gt;object | **Optional.**&lt;br&gt;&lt;br&gt;Header content displayed on top of a message. If a header is not provided, the API uses an image of the first available product as the header |
| `body`&lt;br&gt;&lt;br&gt;object | **Required.**&lt;br&gt;&lt;br&gt;An object with the body of the message. The object contains the following field:&lt;br&gt;&lt;br&gt;`text` string&lt;br&gt;&lt;br&gt;- **Required** if `body` is present. The content of the message. Emojis and markdown are supported. Maximum length is 1024 characters |
| `footer`&lt;br&gt;&lt;br&gt;object | **Optional.**&lt;br&gt;&lt;br&gt;An object with the footer of the message. The object contains the following fields:&lt;br&gt;&lt;br&gt;`text` string&lt;br&gt;&lt;br&gt;- **Required** if `footer` is present. The footer content. Emojis, markdown, and links are supported. Maximum length is 60 characters |
| `action`&lt;br&gt;&lt;br&gt;object | **Required.**&lt;br&gt;&lt;br&gt;An action object you want the user to perform after reading the message. This action object contains the following fields:&lt;br&gt;&lt;br&gt;`name` string&lt;br&gt;&lt;br&gt;- **Required**. Must be &quot;review_and_pay&quot;&lt;br&gt;&lt;br&gt;`parameters` object&lt;br&gt;&lt;br&gt;- See [Parameters Object](#paramobject) for information |

#### Parameters object &#123;#paramobject&#125;

| Object | Description |
| --- | --- |
| `reference_id`&lt;br&gt;&lt;br&gt;string | **Required.**&lt;br&gt;&lt;br&gt;Unique identifier for the order or invoice provided by the business. It is case sensitive and cannot be an empty string and can only contain English letters, numbers, underscores, dashes, or dots, and should not exceed 35 characters.&lt;br&gt;&lt;br&gt;The reference_id must be unique for each order_details message for a given business. If there is a need to send multiple order_details messages for the same order, it is recommended to include a sequence number in the reference_id (for example, &quot;BM345A-12&quot;) to ensure reference_id uniqueness. |
| `type`&lt;br&gt;&lt;br&gt;object | **Required.**&lt;br&gt;&lt;br&gt;The type of goods being paid for in this order. Current supported options are `digital-goods` and `physical-goods` |
| `beneficiaries`&lt;br&gt;&lt;br&gt;array | **Required for shipped physical-goods.**&lt;br&gt;&lt;br&gt;An array of beneficiaries for this order. A beneficiary is an intended recipient for shipping the physical goods in the order. It contains the following fields:&lt;br&gt;&lt;br&gt;**Warning:** Beneficiary information isn&#039;t shown to users but is needed for legal and compliance reasons.&lt;br&gt;&lt;br&gt;`name` string&lt;br&gt;&lt;br&gt;- **Required.** Name of the individual or business receiving the physical goods. Cannot exceed 200 characters&lt;br&gt;&lt;br&gt;`address_line1` string&lt;br&gt;&lt;br&gt;- **Required.** Shipping address (Door/Tower Number, Street Name, and so on). Cannot exceed 100 characters&lt;br&gt;&lt;br&gt;`address_line2` string&lt;br&gt;&lt;br&gt;- **Optional.** Shipping address (Landmark, Area, and so on). Cannot exceed 100 characters&lt;br&gt;&lt;br&gt;`city` string&lt;br&gt;&lt;br&gt;- **Required.** Name of the city.&lt;br&gt;&lt;br&gt;`state` string&lt;br&gt;&lt;br&gt;- **Required.** Name of the state.&lt;br&gt;&lt;br&gt;`country` string&lt;br&gt;&lt;br&gt;- **Required.** Must be &quot;India&quot;.&lt;br&gt;&lt;br&gt;`postal_code` string&lt;br&gt;&lt;br&gt;- **Required.** 6-digit zip code of shipping address. |
| `currency` | **Required.**&lt;br&gt;&lt;br&gt;The currency for this order. Currently the only supported value is `INR`. |
| `total_amount`&lt;br&gt;&lt;br&gt;object | **Required.**&lt;br&gt;&lt;br&gt;The `total_amount` object contains the following fields:&lt;br&gt;&lt;br&gt;`offset` integer&lt;br&gt;&lt;br&gt;- **Required.** Must be `100` for `INR`.&lt;br&gt;&lt;br&gt;`value` integer&lt;br&gt;&lt;br&gt;- **Required.** Positive integer representing the amount value multiplied by offset. For example, ₹12.34 has value 1234.&lt;br&gt;&lt;br&gt;**Warning:** `total_amount.value` must be equal to `order.subtotal.value` + `order.tax.value` + `order.shipping.value` - `order.discount.value`. |
| `payment_settings`&lt;br&gt;&lt;br&gt;object | **Required.**&lt;br&gt;&lt;br&gt;See [Payment Settings object](#paymentsettingsobject) for more information. |
| `order`&lt;br&gt;&lt;br&gt;object | **Required.**&lt;br&gt;&lt;br&gt;See [order object](#ordobject) for more information. |

#### Payment settings object &#123;#paymentsettingsobject&#125;
You can pass UPI intent as it is or parse the UPI intent parameters and pass them in a json structure. Both formats are supported, so following are the two variants of payments settings objects:

##### Payment settings object for UPI intent link

| Object | Description |
| --- | --- |
| `type`&lt;br&gt;&lt;br&gt;string | **Required.**&lt;br&gt;&lt;br&gt;Must be set to **&quot;upi_intent_link&quot;** |
| `upi_intent_link`&lt;br&gt;&lt;br&gt;object | **Required.**&lt;br&gt;&lt;br&gt;An object that describes payment account information:&lt;br&gt;&lt;br&gt;`link` string&lt;br&gt;&lt;br&gt;- **Required.** The UPI intent that is generated from Payment gateway.&lt;br&gt;The UPI intent only supports the following &quot;`&amp;`&quot; separated attributes-  pa, pn, mc, purpose, and tr&lt;br&gt;&lt;br&gt;Example: `upi://pay?pa=merchant_vpa&amp;pn=Merchant_Name&amp;mc=merchant_category_code&amp;purpose=purpose_code&amp;tr=pg_generated_id` |

#### Order object &#123;#ordobject&#125;

| Object | Description |
| --- | --- |
| `status`&lt;br&gt;&lt;br&gt;string | **Required.**&lt;br&gt;&lt;br&gt;Only supported value in the `order_details` message is `pending`.&lt;br&gt;&lt;br&gt;**Note:** In an `order_status` message, `status` can be: `pending`, `captured`, or `failed`. |
| `type`&lt;br&gt;&lt;br&gt;string | **Optional.**&lt;br&gt;&lt;br&gt;Only supported value is `quick_pay`. When this field is passed in, the &quot;Review and Pay&quot; button is hidden and only the &quot;Pay Now&quot; button is shown in the order details bubble. |
| `items`&lt;br&gt;&lt;br&gt;object | **Required.**&lt;br&gt;&lt;br&gt;An object with the list of items for this order, containing the following fields:&lt;br&gt;&lt;br&gt;`retailer_id` string&lt;br&gt;&lt;br&gt;- **Optional.** Content ID for an item in the order from your catalog.&lt;br&gt;&lt;br&gt;`name` string&lt;br&gt;&lt;br&gt;- **Required.** The item&#039;s name to be displayed to the user. Cannot exceed 60 characters&lt;br&gt;&lt;br&gt;`image` object&lt;br&gt;&lt;br&gt;- **Optional.** Custom image for the item to be displayed to the user. See [item image object](#item_image_object) for information&lt;br&gt;&lt;br&gt;**Warning:** Using this image field will limit the items array to a maximum of 10 items and this cannot be used with `retailer_id` or `catalog_id`.&lt;br&gt;&lt;br&gt;`amount` amount object with value and offset -- refer total amount field above&lt;br&gt;&lt;br&gt;- **Required.** The price per item&lt;br&gt;&lt;br&gt;`sale_amount` amount object&lt;br&gt;&lt;br&gt;- **Optional.** The discounted price per item. This should be less than the original amount. If included, this field is used to calculate the subtotal amount&lt;br&gt;&lt;br&gt;`quantity` integer&lt;br&gt;&lt;br&gt;- **Required.** The number of items in this order, this field cannot be decimal, has to be integer.&lt;br&gt;&lt;br&gt;`country_of_origin` string&lt;br&gt;&lt;br&gt;- **Required** if `catalog_id` is not present. The country of origin of the product&lt;br&gt;&lt;br&gt;`importer_name` string&lt;br&gt;&lt;br&gt;- **Required** if `catalog_id` is not present. Name of the importer company&lt;br&gt;&lt;br&gt;`importer_address` string&lt;br&gt;&lt;br&gt;- **Required** if `catalog_id` is not present. Address of importer company |
| `subtotal`&lt;br&gt;&lt;br&gt;object | **Required.**&lt;br&gt;&lt;br&gt;The value **must be equal** to sum of `order.amount.value` * `order.amount.quantity`. Refer to `total_amount` description for explanation of `offset` and `value` fields&lt;br&gt;&lt;br&gt;The following fields are part of the `subtotal` object:&lt;br&gt;&lt;br&gt;`offset` integer&lt;br&gt;&lt;br&gt;- **Required.** Must be `100` for `INR`&lt;br&gt;&lt;br&gt;`value` integer&lt;br&gt;&lt;br&gt;- **Required.** Positive integer representing the amount value multiplied by offset. For example, ₹12.34 has value 1234 |
| `tax`&lt;br&gt;&lt;br&gt;object | **Required.**&lt;br&gt;&lt;br&gt;The tax information for this order which contains the following fields:&lt;br&gt;&lt;br&gt;`offset` integer&lt;br&gt;&lt;br&gt;- **Required.** Must be `100` for `INR`&lt;br&gt;&lt;br&gt;`value` integer&lt;br&gt;&lt;br&gt;- **Required.** Positive integer representing the amount value multiplied by offset. For example, ₹12.34 has value 1234&lt;br&gt;&lt;br&gt;`description` string&lt;br&gt;&lt;br&gt;- **Optional.** Max character limit is 60 characters |
| `shipping`&lt;br&gt;&lt;br&gt;object | **Optional.**&lt;br&gt;&lt;br&gt;The shipping cost of the order. The object contains the following fields:&lt;br&gt;&lt;br&gt;`offset` integer&lt;br&gt;&lt;br&gt;- **Required.** Must be `100` for `INR`&lt;br&gt;&lt;br&gt;`value` integer&lt;br&gt;&lt;br&gt;- **Required.** Positive integer representing the amount value multiplied by offset. For example, ₹12.34 has value 1234&lt;br&gt;&lt;br&gt;`description` string&lt;br&gt;&lt;br&gt;- **Optional.** Max character limit is 60 characters |
| `discount`&lt;br&gt;&lt;br&gt;object | **Optional.**&lt;br&gt;&lt;br&gt;The discount for the order. The object contains the following fields:&lt;br&gt;&lt;br&gt;`offset` integer&lt;br&gt;&lt;br&gt;- **Required.** Must be `100` for `INR`&lt;br&gt;&lt;br&gt;`value` integer&lt;br&gt;&lt;br&gt;- **Required.** Positive integer representing the amount value multiplied by offset. For example, ₹12.34 has value 1234&lt;br&gt;&lt;br&gt;`description` string&lt;br&gt;&lt;br&gt;- **Optional.** Max character limit is 60 characters&lt;br&gt;&lt;br&gt;`discount_program_name` string&lt;br&gt;&lt;br&gt;- **Optional.** Text used for defining incentivized orders. If order is incentivized, the merchant needs to define this information. Max character limit is 60 characters |
| `catalog_id`&lt;br&gt;&lt;br&gt;object | **Optional.**&lt;br&gt;&lt;br&gt;Unique identifier of the Facebook catalog being used by the business.&lt;br&gt;&lt;br&gt;If you do not provide this field, you must provide the following fields inside the items object: `country_of_origin`, `importer_name`, and `importer_address` |
| `expiration`&lt;br&gt;&lt;br&gt;object | **Optional.**&lt;br&gt;&lt;br&gt;Expiration for that order. Business must define the following fields inside this object:&lt;br&gt;&lt;br&gt;`timestamp` string – UTC timestamp in seconds of time when order should expire. Minimum threshold is 300 seconds&lt;br&gt;&lt;br&gt;`description` string – Text explanation for expiration. Max character limit is 120 characters |

#### Item image object &#123;#item_image_object&#125;
| Object | Description |
| --- | --- |
| `link`&lt;br&gt;string | **Required.**&lt;br&gt;A link to the image that will be shown to the user. Must be an `image/jpeg` or `image/png` and 8-bit, RGB, or RGBA. Follows same requirements as image in [media](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-phone-numbers/media#supported-media-types) |

By the end, the interactive object should look something like this for a merchant upi intent type catalog-based integration:

```json
&#123;
  &quot;interactive&quot;: &#123;
    &quot;type&quot;: &quot;order_details&quot;,
    &quot;header&quot;: &#123;
      &quot;type&quot;: &quot;image&quot;,
      &quot;image&quot;: &#123;
        &quot;link&quot;: &quot;your-media-url-link&quot;
      &#125;
    &#125;,
    &quot;body&quot;: &#123;
      &quot;text&quot;: &quot;your-text-body-content&quot;
    &#125;,
    &quot;footer&quot;: &#123;
      &quot;text&quot;: &quot;your-text-footer-content&quot;
    &#125;,
    &quot;action&quot;: &#123;
      &quot;name&quot;: &quot;review_and_pay&quot;,
      &quot;parameters&quot;: &#123;
        &quot;reference_id&quot;: &quot;reference-id-value&quot;,
        &quot;type&quot;: &quot;digital-goods&quot;,
        &quot;payment_settings&quot;: [
          &#123;
            &quot;type&quot;: &quot;upi_intent_link&quot;,
            &quot;upi_intent_link&quot;: &#123;
              &quot;link&quot;: &quot;upi://pay?pa=merchant_vpa&amp;pn=merchant%20Name&amp;mc=mc_code&amp;purpose=purpose_code&amp;tr=transaction_record&quot;
            &#125;
          &#125;
        ],
        &quot;currency&quot;: &quot;INR&quot;,
        &quot;total_amount&quot;: &#123;
          &quot;value&quot;: 21000,
          &quot;offset&quot;: 100
        &#125;,
        &quot;order&quot;: &#123;
          &quot;status&quot;: &quot;pending&quot;,
          &quot;expiration&quot;: &#123;
            &quot;timestamp&quot;: &quot;utc_timestamp_in_seconds&quot;,
            &quot;description&quot;: &quot;cancellation-explanation&quot;
          &#125;,
          &quot;items&quot;: [
            &#123;
              &quot;name&quot;: &quot;Product name, for example bread&quot;,
              &quot;amount&quot;: &#123;
                &quot;value&quot;: 10000,
                &quot;offset&quot;: 100
              &#125;,
              &quot;quantity&quot;: 1,
              &quot;sale_amount&quot;: &#123;
                &quot;value&quot;: 100,
                &quot;offset&quot;: 100
              &#125;,
              &quot;country_of_origin&quot;: &quot;country-of-origin&quot;,
              &quot;importer_name&quot;: &quot;name-of-importer-business&quot;,
              &quot;importer_address&quot;: &#123;
                &quot;address_line1&quot;: &quot;B8/733 nand nagri&quot;,
                &quot;address_line2&quot;: &quot;police station&quot;,
                &quot;city&quot;: &quot;East Delhi&quot;,
                &quot;zone_code&quot;: &quot;DL&quot;,
                &quot;postal_code&quot;: &quot;110093&quot;,
                &quot;country_code&quot;: &quot;IN&quot;
              &#125;
            &#125;,
            &#123;
              &quot;name&quot;: &quot;Product name, for example bread&quot;,
              &quot;amount&quot;: &#123;
                &quot;value&quot;: 10000,
                &quot;offset&quot;: 100
              &#125;,
              &quot;quantity&quot;: 1,
              &quot;sale_amount&quot;: &#123;
                &quot;value&quot;: 100,
                &quot;offset&quot;: 100
              &#125;,
              &quot;country_of_origin&quot;: &quot;country-of-origin&quot;,
              &quot;importer_name&quot;: &quot;name-of-importer-business&quot;,
              &quot;importer_address&quot;: &#123;
                &quot;address_line1&quot;: &quot;B8/733 nand nagri&quot;,
                &quot;address_line2&quot;: &quot;police station&quot;,
                &quot;city&quot;: &quot;East Delhi&quot;,
                &quot;zone_code&quot;: &quot;DL&quot;,
                &quot;postal_code&quot;: &quot;110093&quot;,
                &quot;country_code&quot;: &quot;IN&quot;
              &#125;
            &#125;
          ],
          &quot;subtotal&quot;: &#123;
            &quot;value&quot;: 20000,
            &quot;offset&quot;: 100
          &#125;,
          &quot;tax&quot;: &#123;
            &quot;value&quot;: 1000,
            &quot;offset&quot;: 100,
            &quot;description&quot;: &quot;optional_text&quot;
          &#125;,
          &quot;shipping&quot;: &#123;
            &quot;value&quot;: 1000,
            &quot;offset&quot;: 100,
            &quot;description&quot;: &quot;optional_text&quot;
          &#125;,
          &quot;discount&quot;: &#123;
            &quot;value&quot;: 1000,
            &quot;offset&quot;: 100,
            &quot;description&quot;: &quot;optional_text&quot;,
            &quot;discount_program_name&quot;: &quot;optional_text&quot;
          &#125;
        &#125;
      &#125;
    &#125;
  &#125;
&#125;
```

### Step 3: Add common message parameters

Once the interactive object is complete, append the other parameters that make a message: `recipient_type`, `to`, and `type`. Remember to set the `type` to `interactive`.

```json
&#123;
   &quot;messaging_product&quot;: &quot;whatsapp&quot;,
   &quot;recipient_type&quot;: &quot;individual&quot;,
   &quot;to&quot;: &quot;PHONE_NUMBER&quot;,
   &quot;type&quot;: &quot;interactive&quot;,
   &quot;interactive&quot;: &#123;
     // interactive object here
   &#125;
 &#125;
```

These are [parameters common to all message types](https://developers.facebook.com/documentation/business-messaging/whatsapp/messages/send-messages#requests).

### Step 4: Make a POST call to messages endpoint

Make a POST call to the [`/[PHONE_NUMBER_ID]/messages`](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/message-api) endpoint with the `JSON` object you have assembled. If your message is sent successfully, you get the following response:

```json
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;contacts&quot;: [ &#123;
      &quot;input&quot;: &quot;[PHONE_NUMBER_ID]&quot;,
      &quot;wa_id&quot;: &quot;[PHONE-NUMBER_ID]&quot;
  &#125; ],
  &quot;messages&quot;: [ &#123;
      &quot;id&quot;: &quot;wamid.HBgLMTY1MDUwNzY1MjAVAgARGBI5QTNDQTVCM0Q0Q0Q2RTY3RTcA&quot;
  &#125; ]
&#125;
```

#### Errors

###### WhatsApp Payments Terms of Service acceptance pending
If you see the following error, accept the WhatsApp Payments terms of service using the link provided in the error message before trying again.

```json
&#123;
  &quot;error&quot;: &#123;
    &quot;message&quot;: &quot;(#134011) WhatsApp Payments terms of service has not been accepted&quot;,
    &quot;type&quot;: &quot;OAuthException&quot;,
    &quot;code&quot;: 134011,
    &quot;error_data&quot;: &#123;
      &quot;messaging_product&quot;: &quot;whatsapp&quot;,
      &quot;details&quot;: &quot;WhatsApp Payments Terms of Service acceptance pending for this WhatsApp Business Account.
Please use the following link to accept terms of service before using Business APIs: https://fb.me/12345&quot;
    &#125;
  &#125;
&#125;
```

For all other errors that can be returned and guidance on how to handle them, see [WhatsApp Cloud API, Error Codes](https://developers.facebook.com/documentation/business-messaging/whatsapp/support/error-codes).

###  Step 5: Consumer pays for the order
Consumers can pay using WhatsApp payment method or using any UPI supported app that is installed on the device.

### Step 6: Get notified about transaction status updates from payment gateway

Businesses receive updates to the invoice via payment gateway webhooks, when the status of the user-initiated transaction changes. The unique identifier reference-id passed in `order_details` message can be used to map the transaction to the consumer invoice or interactive order details message.

Please refer to our PG integration guide for the exact payment signals. [Cashfree](https://developers.facebook.com/documentation/business-messaging/whatsapp/payments/payments-in/upi-intent/pg-guide-cashfree) and [CCAvenue](https://developers.facebook.com/documentation/business-messaging/whatsapp/payments/payments-in/upi-intent/pg-guide-ccavenue).

### Step 7: Update order status
Upon receiving transaction signals from payment gateway through webhook, the business must update the order status to keep the user up to date. The following order status values are supported:

| Value | Description |
| --- | --- |
| `pending` | User has not successfully paid yet |
| `processing` | User payment authorized, merchant/partner is fulfilling the order, performing service, and so on. |
| `partially-shipped` | A portion of the products in the order have been shipped by the merchant |
| `shipped` | All the products in the order have been shipped by the merchant |
| `completed` | The order is completed and no further action is expected from the user or the partner/merchant |
| `canceled` | The partner/merchant would like to cancel the `order_details` message for the order/invoice. The status update will fail if there is already a `successful` or `pending` payment for this `order_details` message |

Typically businesses update the `order_status` using either the WhatsApp payment status change notifications or their own internal processes. To update `order_status`, the partner sends a `order_status` message to the user.

```json
&#123;
  &quot;recipient_type&quot;: &quot;individual&quot;,
  &quot;to&quot;: &quot;whatsapp-id&quot;,
  &quot;type&quot;: &quot;interactive&quot;,
  &quot;interactive&quot;: &#123;
    &quot;type&quot;: &quot;order_status&quot;,
    &quot;body&quot;: &#123;
      &quot;text&quot;: &quot;your-text-body-content&quot;
    &#125;,
    &quot;action&quot;: &#123;
      &quot;name&quot;: &quot;review_order&quot;,
      &quot;parameters&quot;: &#123;
        &quot;reference_id&quot;: &quot;reference-id-value&quot;,
        &quot;order&quot;: &#123;
          &quot;status&quot;: &quot;processing | partially_shipped | shipped | completed | canceled&quot;,
          &quot;description&quot;: &quot;optional-text&quot;
        &#125;
      &#125;
    &#125;
  &#125;
&#125;
```

The following table describes the returned values:
| Value | Description |
| --- | --- |
| `reference_id` | The ID provided by the partner in the `order_details` message |
| `status` | The new order `status` |
| `description` | Optional text for sharing status related information in `order_details`. Could be useful while sending cancellation. Max character limit is 120 characters |

**Warning:** Merchant should always post this order-status message to the consumer after receiving transaction updates for an order. As the `order_details` message and order details screen experience is tied to the order status updates.

## Security considerations

Businesses should comply with local security and regulatory requirements in India. They should not rely solely on the status of the transaction provided in the webhook and must use payment lookup API to retrieve the statuses directly from WhatsApp. Businesses must always sanitize/validate the data in the API responses or webhooks to protect against SSRF attacks.

## Checklist for integrated merchants
* Ensure that an `order_status` message is sent to consumer informing them about updates to an order after receiving transaction updates for an order.

* Ensure the merchant is verified and WABA contact is marked with a verified check.

* Verify the WABA is mapped to appropriate merchant initiated messaging tier (1k, 10k, and 100k per day)

* Merchant should list the customer support information in the profile screen in case the consumer wants to report any issues.
