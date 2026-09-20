# Receive payments via payment gateways on WhatsApp



Your business can enable customers to pay for their orders through our partner payment gateways without leaving WhatsApp. Businesses can send customers order_details messages, then get notified about payment status updates via webhook notifications.

## Overview

Currently, customers browse business catalogs, add products to cart, and send orders with our set of commerce messaging solutions, which includes [Single Product Message, Multi Product Message, and Product Detail Page](https://developers.facebook.com/documentation/business-messaging/whatsapp/catalogs/share-products). Now, with the Payments API, businesses can send customers a _bill_, so the customer can complete their order by paying the business without having to leave WhatsApp.

**Warning:** Our payments solution is currently enabled by BillDesk, Razorpay, PayU, and Zaakpay, a third-party payments service provider. You must have a BillDesk, Razorpay, PayU, or Zaakpay account in order to receive payments on WhatsApp.

You can expect more payment providers to be added in the future.

## How it works

First, the business composes and sends an `order_details` message. An `order_details` message is a new type of `interactive` message, which always contains the same 4 main components: **header**, **body**, **footer**, and **action**. Inside the `action` component, the business includes all the information needed for the customer to complete their payment.

Each `order_details` message contains a unique `reference_id` provided by the business, and that unique ID is used throughout the flow to track the order.

Once the message is sent, the business waits for a payment status update via webhooks. Businesses get notified when the payment status changes, but they must not solely rely on these webhooks notifications due to security reasons. WhatsApp also provides a payment lookup API that can be used to retrieve the payment statuses directly anytime.

## Purchase flow in app

In the WhatsApp Messenger App, the purchase flow has the following steps:

- Customers send an order with selected products to the business either through simple text messages or using other interactive messages such as [Single Product Message, Multi Product Message, and Product Detail.](https://developers.facebook.com/documentation/business-messaging/whatsapp/catalogs/share-products)

- Once the business receives the order, they send an `order_details`  message to the user. When the user taps on **Review and Pay**, they will see details about the order and total amount to be paid.

- When the user taps the **Continue** button, they are able to choose to pay natively on WhatsApp or any other UPI app.

Checkout with WhatsApp Pay:

Checkout on other UPI Apps:

- Once the payment has been confirmed by your payment gateway (PG) or payment service provider, the business can start processing the order.

- Businesses can then send an `order_status`  message to the consumer informing them about the status of the order. Each message will result in a message bubble (as shown below) that refers to the original order details message and also updates the status displayed on the order details page.

## Link your payment account

To receive payments on WhatsApp, you must add a _payment configuration_ to the corresponding WhatsApp Business account. A payment configuration allows you to link a payment gateway account to WhatsApp. Each payment configuration is associated with a _unique name_. As part of the `order_details` message, you can specify the payment configuration to use for a specific checkout. WhatsApp will then generate a checkout flow using the associated payment gateway account.

After linking your payment partner account, you must integrate with the Payments APIs below. This will allow you to send an `order_details` message to customers with the payment configuration to receive payments.

### Steps to unlink payment configuration

Note: Make sure no new order messages requesting payment from consumer are sent with the payment config you are trying to remove before you perform the unlink action.

## Integration steps

The steps outlined below assume that the business already knows what the user is interested in through earlier chat threads. The Payments API is a standalone API and hence can work with various messages such as [List Messages, Reply Buttons, and Single or Multi-Product Messages](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/message-api).

### Sequence diagram

The following sequence diagram demonstrates the typical integration flow for Payments API. The steps highlighted in green are the key integration steps.

### Step 1: Send order details interactive message &#123;#step-1&#125;

To send an `order_details` message, businesses must assemble an interactive object of type `order_details` with the following components:

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
| `type`&lt;br&gt;&lt;br&gt;object | **Required.**&lt;br&gt;&lt;br&gt;The type of goods being paid for in this order. Current supported options are `digital-goods` and `physical-goods`. |
| `beneficiaries`&lt;br&gt;&lt;br&gt;array | **Required for shipped physical-goods.**&lt;br&gt;&lt;br&gt;An array of beneficiaries for this order. A beneficiary is an intended recipient for shipping the physical goods in the order. It contains the following fields:&lt;br&gt;&lt;br&gt;Note: Beneficiary information isn&#039;t shown to users but is needed for legal and compliance reasons.&lt;br&gt;&lt;br&gt;`name` string&lt;br&gt;&lt;br&gt;- **Required.** Name of the individual or business receiving the physical goods. Cannot exceed 200 characters&lt;br&gt;&lt;br&gt;`address_line1` string&lt;br&gt;&lt;br&gt;- **Required.** Shipping address (Door/Tower Number, Street Name, and so on). Cannot exceed 100 characters&lt;br&gt;&lt;br&gt;`address_line2` string&lt;br&gt;&lt;br&gt;- **Optional.** Shipping address (Landmark, Area, and so on). Cannot exceed 100 characters&lt;br&gt;&lt;br&gt;`city` string&lt;br&gt;&lt;br&gt;- **Required.** Name of the city.&lt;br&gt;&lt;br&gt;`state` string&lt;br&gt;&lt;br&gt;- **Required.** Name of the state.&lt;br&gt;&lt;br&gt;`country` string&lt;br&gt;&lt;br&gt;- **Required.** Must be &quot;India&quot;.&lt;br&gt;&lt;br&gt;`postal_code` string&lt;br&gt;&lt;br&gt;- **Required.** 6-digit zipcode of shipping address. |
| `currency` | **Required.**&lt;br&gt;&lt;br&gt;The currency for this order. Currently the only supported value is `INR`. |
| `total_amount`&lt;br&gt;&lt;br&gt;object | **Required.**&lt;br&gt;&lt;br&gt;The `total_amount` object contains the following fields:&lt;br&gt;&lt;br&gt;`offset` integer&lt;br&gt;&lt;br&gt;- **Required.** Must be `100` for `INR`.&lt;br&gt;&lt;br&gt;`value` integer&lt;br&gt;&lt;br&gt;- **Required.** Positive integer representing the amount value multiplied by offset. For example, ₹12.34 has value 1234.&lt;br&gt;&lt;br&gt;**Warning:** `total_amount.value` must be equal to `order.subtotal.value` + `order.tax.value` + `order.shipping.value` - `order.discount.value`.&lt;br&gt;&lt;br&gt;**Warning:** UPI transactions are limited to ₹5,00,000. For higher amounts, set `enabled_payment_options` to `[&quot;web&quot;]`. See [Restrict Available Payment Options](#restrict-available-payment-options). |
| `payment_settings`&lt;br&gt;&lt;br&gt;object | **Required.**&lt;br&gt;&lt;br&gt;See [Payment Settings object](#paymentsettingsobject) for more information. |
| `order`&lt;br&gt;&lt;br&gt;object | **Required.**&lt;br&gt;&lt;br&gt;See [order object](#ordobject) for more information. |

#### Payment settings object &#123;#paymentsettingsobject&#125;

| Object | Description |
| --- | --- |
| `type`&lt;br&gt;&lt;br&gt;string | **Required.**&lt;br&gt;&lt;br&gt;Must be set to **&quot;payment_gateway&quot;** |
| `payment_gateway`&lt;br&gt;&lt;br&gt;object | **Required.**&lt;br&gt;&lt;br&gt;An object that describes payment account information:&lt;br&gt;&lt;br&gt;`type` string&lt;br&gt;&lt;br&gt;- **Required.** Unique identifier for an item in the order. You must set this to **&quot;billdesk&quot;** or **&quot;razorpay&quot;** or **&quot;payu&quot;** or **zaakpay**, if you have linked your BillDesk, Razorpay, PayU, or Zaakpay payment gateway to accept payments&lt;br&gt;&lt;br&gt;`configuration_name` string&lt;br&gt;&lt;br&gt;- **Required.** The name of the pre-configured payment configuration to use for this order and must not exceed 60 characters. This value must match with a payment configuration set up on the Meta Business Suite.&lt;br&gt;&lt;br&gt;**Warning:** When `configuration_name` is invalid, the customer will be unable to pay for their order. You should conduct extensive testing of this setup during the integration phase.&lt;br&gt;&lt;br&gt;`billdesk/razorpay/payu/zaakpay` object&lt;br&gt;&lt;br&gt;- **Optional.** For merchants/partners that want to use additional_info1/7(for BillDesk), notes and receipt(for Razorpay) and UDF fields(for PayU) and extra1/2(for Zaakpay), they can now pass these values in Order Details message, and WhatsApp uses these to create transaction/order at respective PGs.&lt;br&gt;&lt;br&gt;Please refer [Payment Gateway specific UDF object](#paymentsettingsudfobject) for more information. |

#### BillDesk, RazorPay, PayU, and Zaakpay fields &#123;#paymentsettingsudfobject&#125;

You can now pass `notes`, `receipt`, and `udf` fields in Order Details message and receive this data back in payment signals. Here you will take a look at how merchants can pass additional_info for BillDesk, notes, and receipt fields for Razorpay, udf for PayU, and extra for Zaakpay PGs.

| Object | Description |
| --- | --- |
| `notes`&lt;br&gt;&lt;br&gt;object | **Optional.**&lt;br&gt;&lt;br&gt;- Only supported for Razorpay payment gateway&lt;br&gt;&lt;br&gt;The object can be key value pairs with maximum 15 keys and each value limits to 256 characters. |
| `receipt`&lt;br&gt;&lt;br&gt;String | **Optional.**&lt;br&gt;&lt;br&gt;- Only supported for Razorpay payment gateway&lt;br&gt;&lt;br&gt;Receipt number that corresponds to this order, set for your internal reference. Maximum length of 40 characters supported with minimum length greater than 0 characters. |
| `udf1-4`&lt;br&gt;&lt;br&gt;String | **Optional.**&lt;br&gt;&lt;br&gt;- Only supported for PayU payment gateway&lt;br&gt;&lt;br&gt;User-defined fields (udf) are used to store any information corresponding to a particular order. Each UDF field has a maximum character limit of 255. |
| `extra1-2`&lt;br&gt;&lt;br&gt;String | **Optional.**&lt;br&gt;&lt;br&gt;- Only supported for Zaakpay payment gateway&lt;br&gt;&lt;br&gt;User-defined fields (extra) are used to store any information corresponding to a particular order. Each extra field has a maximum character limit of 180. |
| `additional_info1-7`&lt;br&gt;&lt;br&gt;String | **Optional.**&lt;br&gt;&lt;br&gt;- Only supported for BillDesk payment gateway&lt;br&gt;&lt;br&gt;User-defined fields (extra) are used to store any information corresponding to a particular order. Each extra field has a maximum character limit of 120. |

#### Order object &#123;#ordobject&#125;

| Object | Description |
| --- | --- |
| `status`&lt;br&gt;&lt;br&gt;string | **Required.**&lt;br&gt;&lt;br&gt;Only supported value in the `order_details` message is `pending`.&lt;br&gt;&lt;br&gt;**Note:** In an `order_status` message, `status` can be: `pending`, `captured`, or `failed`. |
| `type`&lt;br&gt;string | **Optional.**&lt;br&gt;&lt;br&gt;Only supported value is `quick_pay`. When this field is passed in, WhatsApp hides the &quot;Review and Pay&quot; button and only shows the &quot;Pay Now&quot; button in the order details bubble. |
| `items`&lt;br&gt;&lt;br&gt;object | **Required.**&lt;br&gt;&lt;br&gt;An object with the list of items for this order, containing the following fields:&lt;br&gt;&lt;br&gt;`retailer_id` string&lt;br&gt;&lt;br&gt;- **Optional.** Content ID for an item in the order from your catalog.&lt;br&gt;&lt;br&gt;`name` string&lt;br&gt;&lt;br&gt;- **Required.** The item&#039;s name to be displayed to the user. Cannot exceed 60 characters&lt;br&gt;&lt;br&gt;`image` object&lt;br&gt;&lt;br&gt;- **Optional.** Custom image for the item to be displayed to the user. See [item image object](#item_image_object) for information&lt;br&gt;&lt;br&gt;**Warning:** Using this image field will limit the items array to a maximum of 10 items and this cannot be used with `retailer_id` or `catalog_id`.&lt;br&gt;&lt;br&gt;`amount` amount object with value and offset -- refer total amount field above&lt;br&gt;&lt;br&gt;- **Required.** The price per item&lt;br&gt;&lt;br&gt;`sale_amount` amount object&lt;br&gt;&lt;br&gt;- **Optional.** The discounted price per item. This should be less than the original amount. If included, this field is used to calculate the subtotal amount.&lt;br&gt;&lt;br&gt;`quantity` integer&lt;br&gt;&lt;br&gt;- **Required.** The number of items in this order. This field must be an integer, not a decimal.&lt;br&gt;&lt;br&gt;`country_of_origin` string&lt;br&gt;&lt;br&gt;- **Required** if `catalog_id` is not present. The country of origin of the product&lt;br&gt;&lt;br&gt;`importer_name` string&lt;br&gt;&lt;br&gt;- **Required** if `catalog_id` is not present. Name of the importer company&lt;br&gt;&lt;br&gt;`importer_adress` string&lt;br&gt;&lt;br&gt;- **Required** if `catalog_id` is not present. Address of importer company |
| `subtotal`&lt;br&gt;&lt;br&gt;object | **Required.**&lt;br&gt;&lt;br&gt;The value **must be equal** to sum of `order.amount.value` * `order.amount.quantity`. Refer to `total_amount` description for explanation of `offset` and `value` fields&lt;br&gt;&lt;br&gt;The following fields are part of the `subtotal` object:&lt;br&gt;&lt;br&gt;`offset` integer&lt;br&gt;&lt;br&gt;- **Required.** Must be `100` for `INR`&lt;br&gt;&lt;br&gt;`value` integer&lt;br&gt;&lt;br&gt;- **Required.** Positive integer representing the amount value multiplied by offset. For example, ₹12.34 has value 1234 |
| `tax`&lt;br&gt;&lt;br&gt;object | **Required.**&lt;br&gt;&lt;br&gt;The tax information for this order which contains the following fields:&lt;br&gt;&lt;br&gt;`offset` integer&lt;br&gt;&lt;br&gt;- **Required.** Must be `100` for `INR`&lt;br&gt;&lt;br&gt;`value` integer&lt;br&gt;&lt;br&gt;- **Required.** Positive integer representing the amount value multiplied by offset. For example, ₹12.34 has value 1234&lt;br&gt;&lt;br&gt;`description` string&lt;br&gt;&lt;br&gt;- **Optional.** Max character limit is 60 characters |
| `shipping`&lt;br&gt;&lt;br&gt;object | **Optional.**&lt;br&gt;&lt;br&gt;The shipping cost of the order. The object contains the following fields:&lt;br&gt;&lt;br&gt;`offset` integer&lt;br&gt;&lt;br&gt;- **Required.** Must be `100` for `INR`&lt;br&gt;&lt;br&gt;`value` integer&lt;br&gt;&lt;br&gt;- **Required.** Positive integer representing the amount value multiplied by offset. For example, ₹12.34 has value 1234&lt;br&gt;&lt;br&gt;`description` string&lt;br&gt;&lt;br&gt;- **Optional.** Max character limit is 60 characters |
| `discount`&lt;br&gt;&lt;br&gt;object | **Optional.**&lt;br&gt;&lt;br&gt;The discount for the order. The object contains the following fields:&lt;br&gt;&lt;br&gt;`offset` integer&lt;br&gt;&lt;br&gt;- **Required.** Must be `100` for `INR`&lt;br&gt;&lt;br&gt;`value` integer&lt;br&gt;&lt;br&gt;- **Required.** Positive integer representing the amount value multiplied by offset. For example, ₹12.34 has value 1234&lt;br&gt;&lt;br&gt;`description` string&lt;br&gt;&lt;br&gt;- **Optional.** Max character limit is 60 characters&lt;br&gt;&lt;br&gt;`discount_program_name` string&lt;br&gt;&lt;br&gt;- **Optional.** Text used for defining incentivised orders. If order is incentivised, the merchant needs to define this information. Max character limit is 60 characters |
| `catalog_id`&lt;br&gt;&lt;br&gt;object | **Optional.**&lt;br&gt;&lt;br&gt;Unique identifier of the Facebook catalog being used by the business.&lt;br&gt;&lt;br&gt;If you do not provide this field, you must provide the following fields inside the items object: `country_of_origin`, `importer_name`, and `importer_address` |
| `expiration`&lt;br&gt;&lt;br&gt;object | **Optional.**&lt;br&gt;&lt;br&gt;Expiration for that order. Business must define the following fields inside this object:&lt;br&gt;&lt;br&gt;`timestamp` string – UTC timestamp in seconds of time when order should expire. Minimum threshold is 300 seconds&lt;br&gt;&lt;br&gt;`description` string – Text explanation for expiration. Max character limit is 120 characters |

#### Item image object &#123;#item_image_object&#125;
| Object | Description |
| --- | --- |
| `link`&lt;br&gt;string | **Required.**&lt;br&gt;A link to the image that will be shown to the user. Must be an `image/jpeg` or `image/png` and 8-bit, RGB, or RGBA. Follows same requirements as image in [media](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-phone-numbers/media#supported-media-types) |

**Warning:** The `parameters` value is a stringified JSON object.

By the end, the interactive object should look something like this for a BillDesk catalog-based integration:

```json
&#123;
  &quot;interactive&quot;: &#123;
    &quot;type&quot;: &quot;order_details&quot;,
    &quot;header&quot;: &#123;
      &quot;type&quot;: &quot;image&quot;,
      &quot;image&quot;: &#123;
        &quot;link&quot;: &quot;http(s)://the-url&quot;,
        &quot;provider&quot;: &#123;
          &quot;name&quot;: &quot;provider-name&quot;
        &#125;
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
            &quot;type&quot;: &quot;payment_gateway&quot;,
            &quot;payment_gateway&quot;: &#123;
              &quot;type&quot;: &quot;billdesk&quot;,
              &quot;configuration_name&quot;: &quot;payment-config-id&quot;,
              &quot;billdesk&quot;: &#123;
                &quot;additional_info1&quot;: &quot;additional_info1-value&quot;,
                &quot;additional_info2&quot;: &quot;additional_info2-value&quot;,
                &quot;additional_info3&quot;: &quot;additional_info3-value&quot;,
                &quot;additional_info4&quot;: &quot;additional_info4-value&quot;,
                &quot;additional_info5&quot;: &quot;additional_info5-value&quot;,
                &quot;additional_info6&quot;: &quot;additional_info6-value&quot;,
                &quot;additional_info7&quot;: &quot;additional_info7-value&quot;
              &#125;
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
          &quot;catalog_id&quot;: &quot;the-catalog_id&quot;,
          &quot;expiration&quot;: &#123;
            &quot;timestamp&quot;: &quot;utc_timestamp_in_seconds&quot;,
            &quot;description&quot;: &quot;cancellation-explanation&quot;
          &#125;,
          &quot;items&quot;: [
            &#123;
              &quot;retailer_id&quot;: &quot;1234567&quot;,
              &quot;name&quot;: &quot;Product name, for example bread&quot;,
              &quot;amount&quot;: &#123;
                &quot;value&quot;: 10000,
                &quot;offset&quot;: 100
              &#125;,
              &quot;quantity&quot;: 1,
              &quot;sale_amount&quot;: &#123;
                &quot;value&quot;: 100,
                &quot;offset&quot;: 100
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

**Warning:** The `parameters` value is a stringified JSON object.

By the end, the interactive object should look something like this for a RazorPay catalog-based integration:

```json
&#123;
  &quot;interactive&quot;: &#123;
    &quot;type&quot;: &quot;order_details&quot;,
    &quot;header&quot;: &#123;
      &quot;type&quot;: &quot;image&quot;,
      &quot;image&quot;: &#123;
        &quot;link&quot;: &quot;http(s)://the-url&quot;,
        &quot;provider&quot;: &#123;
          &quot;name&quot;: &quot;provider-name&quot;
        &#125;
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
            &quot;type&quot;: &quot;payment_gateway&quot;,
            &quot;payment_gateway&quot;: &#123;
              &quot;type&quot;: &quot;razorpay&quot;,
              &quot;configuration_name&quot;: &quot;payment-config-id&quot;,
              &quot;razorpay&quot;: &#123;
                &quot;receipt&quot;: &quot;receipt-value&quot;,
                &quot;notes&quot;: &#123;
                  &quot;key1&quot;: &quot;value1&quot;
                &#125;
              &#125;
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
          &quot;catalog_id&quot;: &quot;the-catalog_id&quot;,
          &quot;expiration&quot;: &#123;
            &quot;timestamp&quot;: &quot;utc_timestamp_in_seconds&quot;,
            &quot;description&quot;: &quot;cancellation-explanation&quot;
          &#125;,
          &quot;items&quot;: [
            &#123;
              &quot;retailer_id&quot;: &quot;1234567&quot;,
              &quot;name&quot;: &quot;Product name, for example bread&quot;,
              &quot;amount&quot;: &#123;
                &quot;value&quot;: 10000,
                &quot;offset&quot;: 100
              &#125;,
              &quot;quantity&quot;: 1,
              &quot;sale_amount&quot;: &#123;
                &quot;value&quot;: 100,
                &quot;offset&quot;: 100
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

**Warning:** The `parameters` value is a stringified JSON object.

For a PayU non-catalog based integration i.e. when catalog-id is not present, an example payload looks as follows:

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
            &quot;type&quot;: &quot;payment_gateway&quot;,
            &quot;payment_gateway&quot;: &#123;
              &quot;type&quot;: &quot;payu&quot;,
              &quot;configuration_name&quot;: &quot;payment-config-id&quot;,
              &quot;payu&quot;: &#123;
                &quot;udf1&quot;: &quot;value1&quot;,
                &quot;udf2&quot;: &quot;value2&quot;,
                &quot;udf3&quot;: &quot;value3&quot;,
                &quot;udf4&quot;: &quot;value4&quot;
              &#125;
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

For a Zaakpay non-catalog based integration i.e. when catalog-id is not present, an example payload looks as follows:

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
            &quot;type&quot;: &quot;payment_gateway&quot;,
            &quot;payment_gateway&quot;: &#123;
              &quot;type&quot;: &quot;zaakpay&quot;,
              &quot;configuration_name&quot;: &quot;payment-config-id&quot;,
              &quot;zaakpay&quot;: &#123;
                &quot;extra1&quot;: &quot;value1&quot;,
                &quot;extra2&quot;: &quot;value2&quot;
              &#125;
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

### Step 2: Add common message parameters

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

### Step 3: Make a POST call to messages endpoint

Make a POST call to the [`/[PHONE_NUMBER_ID]/messages`](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/message-api) endpoint with the `JSON` object you have assembled. If your message is sent successfully, you get the following response:

```json
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;contacts&quot;: [ &#123;
      &quot;input&quot;: &quot;[PHONE_NUMBER_ID]&quot;,
      &quot;wa_id&quot;: &quot;[PHONE_NUMBER_ID]&quot;
  &#125; ],
  &quot;messages&quot;: [ &#123;
      &quot;id&quot;: &quot;wamid.HBgLMTY1MDUwNzY1MjAVAgARGBI5QTNDQTVCM0Q0Q0Q2RTY3RTcA&quot;
  &#125; ]
&#125;
```

For all errors that can be returned and guidance on how to handle them, see [WhatsApp Cloud API, Error Codes](https://developers.facebook.com/documentation/business-messaging/whatsapp/support/error-codes).

#### Product experience

The customer receives an `order_details` message similar to the one below (left). When they click on &quot;Review and Pay&quot;, it opens up the order details screen as shown below (middle). Customer can then pay for their order using &quot;Continue&quot; button that opens up a bottom sheet with the payment options (right).

### Step 4: Receive webhook about transaction status

Businesses receive updates via [messages webhooks](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/messages) when the status of the user-initiated transaction changes in a status of type &quot;payment&quot;. It contains the following fields:

| Object | Description |
| --- | --- |
| `id`&lt;br&gt;&lt;br&gt;string | **Required.**&lt;br&gt;&lt;br&gt;Webhook ID for the notification. |
| `recipient_id `&lt;br&gt;&lt;br&gt;string | **Required.**&lt;br&gt;&lt;br&gt;WhatsApp ID of the customer. |
| `type`&lt;br&gt;&lt;br&gt;string | **Required.**&lt;br&gt;&lt;br&gt;For payment status update webhooks, type is &quot;payment&quot;. |
| `status`&lt;br&gt;&lt;br&gt;string | **Required.**&lt;br&gt;&lt;br&gt;`captured`/`pending`: `captured` - when the payment is successfully completed, `pending` when the user attempted but yet to receive success transactions signal |
| `payment`&lt;br&gt;&lt;br&gt;object | **Required.**&lt;br&gt;&lt;br&gt;Contains the following field:&lt;br&gt;&lt;br&gt;`reference_id` string&lt;br&gt;&lt;br&gt;* Unique reference ID for the order sent in `order_details` message.&lt;br&gt;&lt;br&gt;`amount` object&lt;br&gt;&lt;br&gt;* Has value and offset fields corresponding to total amount that user has paid.&lt;br&gt;&lt;br&gt;`currency ` string&lt;br&gt;&lt;br&gt;* currency is always INR.&lt;br&gt;&lt;br&gt;`transaction ` object&lt;br&gt;Transaction attempt for this payment. Transaction object contains the following fields:&lt;br&gt;&lt;br&gt;- `id` string&lt;br&gt;     **Required.** The alpha-numeric payment gateway order ID.&lt;br&gt;&lt;br&gt;- `pg_transaction_id` string&lt;br&gt;     **Optional.** The alpha-numeric payment gateway payment ID.&lt;br&gt;&lt;br&gt;- `type` string&lt;br&gt;   **Required.** The payment type for this transactions. Only, `billdesk`, `razorpay`, `payu`, or `zaakpay` are supported.&lt;br&gt;&lt;br&gt;- `status` string&lt;br&gt;   **Required.** The status of the transaction. Can be one of `pending` or `success` or `failed`.&lt;br&gt;&lt;br&gt;- `created_timestamp` integer&lt;br&gt;   **Required.** Time when transaction was created in epoch seconds.&lt;br&gt;&lt;br&gt;- `updated_timestamp` integer&lt;br&gt;   **Required.** Time when transaction was last updated in epoch seconds.&lt;br&gt;&lt;br&gt;- `method` object  (**Optional.** the payment method information might not be available for failed payments)&lt;br&gt; - `type` string&lt;br&gt;     **Required.** Describes the type of payment method used by consumer to pay for the order. Can be one of `upi` or `card` or `wallet` or `netbanking`.&lt;br&gt;&lt;br&gt;- `error` object  (**Optional.** the payment error details might not be available for all payments attempts)&lt;br&gt; - `code` string&lt;br&gt;   **Required.** Describes the payment failure reason that is generated by payment gateway and Meta returns this to partners.&lt;br&gt; - `reason` string&lt;br&gt;   **Required.** Describes the payment failure reason in plain text that is generated by payment gateway and Meta returns this to partners.&lt;br&gt;&lt;br&gt;`additional_info1-7` string **Optional.**&lt;br&gt;&lt;br&gt;* Only sent for billdesk payment gateway when the value is sent in order details message. Each of the keys additional_info1-4 has string values in them.&lt;br&gt;&lt;br&gt;`notes `   object **Optional.**&lt;br&gt;&lt;br&gt;* Only sent for razorpay payment gateway when the value is sent in order details message. This contains key-value pair as passed in the Order Details message.&lt;br&gt;&lt;br&gt;`receipt` string **Optional.**&lt;br&gt;&lt;br&gt;* Only sent for razorpay payment gateway when the value is sent in order details message.&lt;br&gt;&lt;br&gt;`udf1-4` string **Optional.**&lt;br&gt;&lt;br&gt;* Only sent for payu payment gateway when the value is sent in order details message. Each of the keys udf1-4 has string values in them.&lt;br&gt;&lt;br&gt;`extra1-2` string **Optional.**&lt;br&gt;&lt;br&gt;* Only sent for zaakpay payment gateway when the value is sent in order details message. Each of the keys extra1-2 has string values in them.&lt;br&gt;&lt;br&gt;`refunds` array **Optional.**&lt;br&gt;&lt;br&gt;The list of refunds for this order. Each refund object contains the following fields:&lt;br&gt;&lt;br&gt;* `id` string&lt;br&gt;     **Required.** The alpha-numeric ID of the refund.&lt;br&gt;* `amount` object&lt;br&gt;     **Required.** The total amount of the refund.&lt;br&gt;* `speed_processed` string&lt;br&gt;     **Required.** Speed by which refund was processed. Can be one of `instant` or `normal`.&lt;br&gt;* `status` string&lt;br&gt;     **Required.** The status of the refund. Can be one of `pending`, `success` or `failed`.&lt;br&gt;* `created_timestamp` integer&lt;br&gt;     **Required.** Time when refund was created in epoch seconds.&lt;br&gt;* `updated_timestamp` integer&lt;br&gt;     **Required.** Time when refund was last updated in epoch seconds. |
| `timestamp`&lt;br&gt;&lt;br&gt;string | **Required.**&lt;br&gt;&lt;br&gt;Timestamp for the webhook. |

Here is an example status webhook of type `payment`:

```json
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [&#123;
    &quot;id&quot;: &quot;WHATSAPP-BUSINESS-ACCOUNT-ID&quot;,
    &quot;changes&quot;: [&#123;
      &quot;value&quot;: &#123;
         &quot;messaging_product&quot;: &quot;whatsapp&quot;,
         &quot;metadata&quot;: &#123;
           &quot;display_phone_number&quot;: &quot;[PHONE_NUMBER]&quot;,
           &quot;phone_number_id&quot;: &quot;[PHONE_NUMBER_ID]&quot;
         &#125;,
         &quot;contacts&quot;: [&#123;...&#125;],
         &quot;errors&quot;: [&#123;...&#125;],
         &quot;messages&quot;: [&#123;...&#125;],
         &quot;statuses&quot;: [&#123;
            &quot;id&quot;: &quot;gBGGFlB5YjhvAgnhuF1qIUvCo7A&quot;,
            &quot;recipient_id&quot;: &quot;[PHONE_NUMBER]&quot;,
            &quot;type&quot;: &quot;payment&quot;,
            &quot;status&quot;: &quot;[TRANSACTION_STATUS]&quot;,
            &quot;payment&quot;: &#123;
               &quot;reference_id&quot;: &quot;[REFERENCE_ID]&quot;,
               &quot;amount&quot;: &#123;
                 &quot;value&quot;: 21000,
                 &quot;offset&quot;: 100
               &#125;,
               &quot;transaction&quot;: &#123;
                 &quot;id&quot;: &quot;[PG-ORDER-ID]&quot;,
                 &quot;pg_transaction_id&quot;: &quot;[PG-PAYMENT-ID]&quot;,
                 &quot;type&quot;: &quot;billdesk/razorpay/payu/zaakpay&quot;,
                 &quot;status&quot;: &quot;success/failed&quot;,
                 &quot;created_timestamp&quot;: &quot;CREATED_TIMESTAMP&quot;,
                 &quot;updated_timestamp&quot;: &quot;UPDATED_TIMESTAMP&quot;,
                 &quot;method&quot;: &#123;
                   &quot;type&quot;: &quot;upi/card/netbanking/wallet&quot;
                 &#125;,
                 &quot;error&quot;: &#123;
                   &quot;code&quot;: &quot;pg-generated-error-code&quot;,
                   &quot;reason&quot;: &quot;pg-generated-descriptive-reason&quot;
                 &#125;
               &#125;,
               &quot;currency&quot;: &quot;INR&quot;,
               &quot;receipt&quot;: &quot;receipt-value&quot;,
               &quot;notes&quot;: &#123;
                 &quot;key1&quot;: &quot;value1&quot;,
                 &quot;key2&quot;: &quot;value2&quot;
               &#125;,
               &quot;udf1&quot;: &quot;udf1-value&quot;,
               &quot;udf2&quot;: &quot;udf2-value&quot;,
               &quot;udf3&quot;: &quot;udf3-value&quot;,
               &quot;udf4&quot;: &quot;udf4-value&quot;,
               &quot;additional_info1&quot;: &quot;additional_info1-value&quot;,
               &quot;additional_info2&quot;: &quot;additional_info2-value&quot;,
               &quot;additional_info3&quot;: &quot;additional_info3-value&quot;,
               &quot;additional_info4&quot;: &quot;additional_info4-value&quot;,
               &quot;additional_info5&quot;: &quot;additional_info5-value&quot;,
               &quot;additional_info6&quot;: &quot;additional_info6-value&quot;,
               &quot;additional_info7&quot;: &quot;additional_info7-value&quot;,
               &quot;refunds&quot;: [&#123;
                 &quot;id&quot;: &quot;[REFUND-ID]&quot;,
                 &quot;amount&quot;: &#123;
                   &quot;value&quot;: 100,
                   &quot;offset&quot;: 100
                 &#125;,
                 &quot;speed_processed&quot;: &quot;instant/normal&quot;,
                 &quot;status&quot;: &quot;success&quot;,
                 &quot;created_timestamp&quot;: &quot;CREATED_TIMESTAMP&quot;,
                 &quot;updated_timestamp&quot;: &quot;UPDATED_TIMESTAMP&quot;
              &#125;]
            &#125;,
            &quot;timestamp&quot;: &quot;notification_timestamp&quot;
         &#125;]
      &#125;,
      &quot;field&quot;: &quot;messages&quot;
    &#125;]
  &#125;]
&#125;
```

For more information about other statuses, see [Messages Webhooks](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/messages).

### Step 5: Confirm payment &#123;#step-3&#125;

After receiving the payment status webhook, or at any time, the business can look up the status of the payment for the order. To do that, businesses must make a GET call to the payments endpoint as shown here:

```html
GET &lt;PHONE_NUMBER_ID&gt;/payments/&lt;PAYMENT_CONFIGURATION&gt;/&lt;REFERENCE_ID&gt;
```

where `payment_configuration` and `reference_id` are same as that sent in the `order_details` message.

Businesses should expect a response in the same HTTP session (not in a webhook notification) that contains the following fields:

| Field | Description |
| --- | --- |
| `reference_id`&lt;br&gt;&lt;br&gt;string | **Required.**&lt;br&gt;&lt;br&gt;The ID sent by the business in the `order_details` message |
| `status`&lt;br&gt;&lt;br&gt;string | **Required.**&lt;br&gt;&lt;br&gt;Status of the payment for the order. Can be one of `pending` or `captured` &lt;br&gt;&lt;br&gt;Refer the table below for what these statuses mean. |
| `currency`&lt;br&gt;&lt;br&gt;string | **Required.**&lt;br&gt;&lt;br&gt;The currency for this payment. Currently the only supported value is `INR`. |
| `amount`&lt;br&gt;&lt;br&gt;object | **Required.**&lt;br&gt;&lt;br&gt;The amount for this payment. It contains the following fields:&lt;br&gt;&lt;br&gt;`offset` integer&lt;br&gt;&lt;br&gt;- **Required.** Must be 100.&lt;br&gt;&lt;br&gt;`value` integer&lt;br&gt;&lt;br&gt;- **Required.** Positive integer representing the amount value multiplied by offset. For example, ₹12.34 has value 1234. |
| `transactions`&lt;br&gt;&lt;br&gt;array | **Optional.**&lt;br&gt;&lt;br&gt;The list of transactions for this payment. This field is only present when at least one payment attempt has been made. If the payment status is `pending` and no payment attempt has occurred, this field will not be returned. Each transaction object contains the following fields:&lt;br&gt;&lt;br&gt;`id` string&lt;br&gt;&lt;br&gt;- **Required.** The alpha-numeric payment gateway order ID.&lt;br&gt;&lt;br&gt;`pg_transaction_id` string&lt;br&gt;&lt;br&gt;- **Required.** The alpha-numeric payment gateway payment ID.&lt;br&gt;&lt;br&gt;`type` string&lt;br&gt;&lt;br&gt;- **Required.** The payment type for this transactions. Only, `billdesk`, `razorpay`, `payu`, or `zaakpay` are supported.&lt;br&gt;&lt;br&gt;`status` string&lt;br&gt;&lt;br&gt;- **Required.** The status of the transaction. Can be one of `pending` or `success` or `failed`.&lt;br&gt;&lt;br&gt;**Warning:** At most one transaction can have a `success` status.&lt;br&gt;&lt;br&gt;`created_timestamp` integer&lt;br&gt;&lt;br&gt;- **Required.** Time when transaction was created in epoch seconds.&lt;br&gt;&lt;br&gt;`updated_timestamp` integer&lt;br&gt;&lt;br&gt;- **Required.** Time when transaction was last updated in epoch seconds.&lt;br&gt;&lt;br&gt;`method` object&lt;br&gt;&lt;br&gt;**Optional.** the payment method information might not be available for failed payments&lt;br&gt;&lt;br&gt;* `type` string&lt;br&gt;     **Required.** Describes the type of payment method used by consumer to pay for the order. Can be one of `upi` or `card` or `wallet` or `netbanking`.&lt;br&gt;&lt;br&gt;`error` object&lt;br&gt;&lt;br&gt;**Optional.** the payment error details might not be available for all payments attempts&lt;br&gt;&lt;br&gt;* `code` string&lt;br&gt;   **Required.** Describes the payment failure reason that is generated by payment gateway and Meta returns this to partners.&lt;br&gt;* `reason` string&lt;br&gt;   **Required.** Describes the payment failure reason in plain text that is generated by payment gateway and Meta returns this to partners.&lt;br&gt;&lt;br&gt;`refunds` array&lt;br&gt;&lt;br&gt;**Optional.** The list of refunds for this order. Each refund object contains the following fields:&lt;br&gt;&lt;br&gt;* `id` string&lt;br&gt;     **Required.** The alpha-numeric ID of the refund.&lt;br&gt;* `amount` object&lt;br&gt;     **Required.** The total amount of the refund.&lt;br&gt;* `speed_processed` string&lt;br&gt;     **Required.** Speed by which refund was processed. Can be one of `instant` or `normal`.&lt;br&gt;* `status` string&lt;br&gt;     **Required.** The status of the refund. Can be one of `pending`, `success` or `failed`.&lt;br&gt;* `created_timestamp` integer&lt;br&gt;     **Required.** Time when refund was created in epoch seconds.&lt;br&gt;* `updated_timestamp` integer&lt;br&gt;     **Required.** Time when refund was last updated in epoch seconds. |
| `additional_info1-7`&lt;br&gt;&lt;br&gt;string | **Optional.**&lt;br&gt;&lt;br&gt;Supported for only BillDesk PG, this contains string values sent as part of Order Details message. |
| `receipt`&lt;br&gt;&lt;br&gt;string | **Optional.**&lt;br&gt;&lt;br&gt;Supported for only Razorpay PG, this contains the receipt-value sent as part of Order Details message. |
| `notes`&lt;br&gt;&lt;br&gt;object | **Optional.**&lt;br&gt;&lt;br&gt;Supported for only Razorpay PG, this contains the key-value pairs sent as part of Order Details message. |
| `udf1-4`&lt;br&gt;&lt;br&gt;string | **Optional.**&lt;br&gt;&lt;br&gt;Supported for only PayU PG, this contains string values sent as part of Order Details message. |
| `extra1-2`&lt;br&gt;&lt;br&gt;string | **Optional.**&lt;br&gt;&lt;br&gt;Supported for only Zaakpay PG, this contains string values sent as part of Order Details message. |

#### Payment status &#123;#payment-status&#125;
| Status | Description |
| --- | --- |
| `pending` | The order has been created but payment has not yet been captured. This status covers two scenarios:&lt;br&gt;&lt;br&gt;- **No payment attempt yet:** The `transactions` array will not be present in the response.&lt;br&gt;- **Payment attempted but failed:** The `transactions` array will contain one or more entries with `status` set to `failed`. |
| `captured` | The payment was successfully captured. The `transactions` array will contain an entry with `status` set to `success`. |

An example successful response looks like this:

```json
&#123;
  &quot;payments&quot;: [&#123;
    &quot;reference_id&quot;: &quot;reference-id-value&quot;,
    &quot;status&quot;: &quot;status-of-payment&quot;,
    &quot;currency&quot;: &quot;INR&quot;,
    &quot;amount&quot;: &#123;
      &quot;value&quot;: 21000,
      &quot;offset&quot;: 100
    &#125;,
    &quot;transactions&quot;: [
      &#123;
        &quot;id&quot;: &quot;[PG-ORDER-ID]&quot;,
        &quot;pg_transaction_id&quot;: &quot;[PG-TXN-ID]&quot;,
        &quot;type&quot;: &quot;billdesk/razorpay/payu/zaakpay&quot;,
        &quot;status&quot;: &quot;success/failed&quot;,
        &quot;created_timestamp&quot;: &quot;CREATED_TIMESTAMP&quot;,
        &quot;updated_timestamp&quot;: &quot;UPDATED_TIMESTAMP&quot;,
        &quot;method&quot;: &#123;
           &quot;type&quot;: &quot;upi/card/netbanking/wallet&quot;
        &#125;,
        &quot;error&quot;: &#123;
           &quot;code&quot;: &quot;pg-generated-error-code&quot;,
           &quot;reason&quot;: &quot;pg-generated-descriptive-reason&quot;
        &#125;,
        &quot;refunds&quot;: [
          &#123;
            &quot;id&quot;: &quot;[REFUND-ID]&quot;,
            &quot;amount&quot;: &#123;
              &quot;value&quot;: 100,
              &quot;offset&quot;: 100
            &#125;,
            &quot;speed_processed&quot;: &quot;instant/normal&quot;,
            &quot;status&quot;: &quot;success&quot;,
            &quot;created_timestamp&quot;: &quot;CREATED_TIMESTAMP&quot;,
            &quot;updated_timestamp&quot;: &quot;UPDATED_TIMESATMP&quot;
          &#125;
        ]
      &#125;
    ],
    &quot;receipt&quot;: &quot;receipt-value&quot;,
    &quot;notes&quot;: &#123;
      &quot;key1&quot;: &quot;value1&quot;,
      &quot;key2&quot;: &quot;value2&quot;
    &#125;,
    &quot;udf1&quot;: &quot;udf1-value&quot;,
    &quot;udf2&quot;: &quot;udf2-value&quot;,
    &quot;udf3&quot;: &quot;udf3-value&quot;,
    &quot;udf4&quot;: &quot;udf4-value&quot;,
    &quot;additional_info1&quot;: &quot;additional_info1-value&quot;,
    &quot;additional_info2&quot;: &quot;additional_info2-value&quot;,
    &quot;additional_info3&quot;: &quot;additional_info3-value&quot;,
    &quot;additional_info4&quot;: &quot;additional_info4-value&quot;,
    &quot;additional_info5&quot;: &quot;additional_info5-value&quot;,
    &quot;additional_info6&quot;: &quot;additional_info6-value&quot;,
    &quot;additional_info7&quot;: &quot;additional_info7-value&quot;
  &#125;]
&#125;
```

Shown here is an example for a generic error:

```json
&#123;
  &quot;errors&quot;: [&#123;
    &quot;code&quot;: 500,
    &quot;title&quot;: &quot;Generic error&quot;,
    &quot;details&quot;: &quot;System error. Please try again.&quot;
  &#125;]
&#125;
```

#### Response by payment stage

The response payload varies depending on the payment stage. Below are examples for each stage.

**No payment attempted** — The user has not yet attempted payment. Only order-level fields are returned; the `transactions` array is not present.

```json
&#123;
  &quot;payments&quot;: [
    &#123;
      &quot;reference_id&quot;: &quot;&lt;your_reference_id&gt;&quot;,
      &quot;status&quot;: &quot;pending&quot;,
      &quot;amount&quot;: &#123;
        &quot;offset&quot;: 100,
        &quot;value&quot;: 1000
      &#125;,
      &quot;currency&quot;: &quot;INR&quot;
    &#125;
  ]
&#125;
```

**Payment successful** — The user completed payment and the payment gateway confirmed capture. The `transactions` array contains the successful transaction with payment method details.

```json
&#123;
  &quot;payments&quot;: [
    &#123;
      &quot;reference_id&quot;: &quot;&lt;your_reference_id&gt;&quot;,
      &quot;status&quot;: &quot;captured&quot;,
      &quot;amount&quot;: &#123;
        &quot;offset&quot;: 100,
        &quot;value&quot;: 1000
      &#125;,
      &quot;currency&quot;: &quot;INR&quot;,
      &quot;transactions&quot;: [
        &#123;
          &quot;id&quot;: &quot;&lt;order_id&gt;&quot;,
          &quot;pg_transaction_id&quot;: &quot;&lt;payment_id&gt;&quot;,
          &quot;type&quot;: &quot;razorpay&quot;,
          &quot;status&quot;: &quot;success&quot;,
          &quot;created_timestamp&quot;: 1772129215,
          &quot;updated_timestamp&quot;: 1772129215,
          &quot;amount&quot;: &#123;
            &quot;offset&quot;: 100,
            &quot;value&quot;: 1000
          &#125;,
          &quot;order_amount&quot;: &#123;
            &quot;offset&quot;: 100,
            &quot;value&quot;: 1000
          &#125;,
          &quot;currency&quot;: &quot;INR&quot;,
          &quot;method&quot;: &#123;
            &quot;type&quot;: &quot;upi&quot;
          &#125;
        &#125;
      ]
    &#125;
  ]
&#125;
```

**Payment failed** — The user attempted payment but it failed. The overall payment status remains `pending` (the order is still awaiting successful payment), but the `transactions` array contains the failed attempt with error details.

```json
&#123;
  &quot;payments&quot;: [
    &#123;
      &quot;reference_id&quot;: &quot;&lt;your_reference_id&gt;&quot;,
      &quot;status&quot;: &quot;pending&quot;,
      &quot;amount&quot;: &#123;
        &quot;offset&quot;: 100,
        &quot;value&quot;: 1000
      &#125;,
      &quot;currency&quot;: &quot;INR&quot;,
      &quot;transactions&quot;: [
        &#123;
          &quot;id&quot;: &quot;&lt;order_id&gt;&quot;,
          &quot;pg_transaction_id&quot;: &quot;&lt;payment_id&gt;&quot;,
          &quot;type&quot;: &quot;razorpay&quot;,
          &quot;status&quot;: &quot;failed&quot;,
          &quot;created_timestamp&quot;: 1772129329,
          &quot;updated_timestamp&quot;: 1772129329,
          &quot;amount&quot;: &#123;
            &quot;offset&quot;: 100,
            &quot;value&quot;: 1000
          &#125;,
          &quot;order_amount&quot;: &#123;
            &quot;offset&quot;: 100,
            &quot;value&quot;: 1000
          &#125;,
          &quot;currency&quot;: &quot;INR&quot;,
          &quot;method&quot;: &#123;
            &quot;type&quot;: &quot;upi&quot;
          &#125;,
          &quot;error&quot;: &#123;
            &quot;code&quot;: &quot;BAD_REQUEST_ERROR&quot;,
            &quot;reason&quot;: &quot;incorrect_pin&quot;
          &#125;
        &#125;
      ]
    &#125;
  ]
&#125;
```

### Step 6: Update order status

**Warning:** Businesses _must_ send updates to their order using the `order_status` message instead of text messages since the latest status of an order displayed on the order details page is only based on `order_status` messages.

To notify the customer with updates to an order, you can send an `interactive` message of type `order_status` as shown below.

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

The following table describes the fields in the `order_status` interactive message:

| Object | Description |
| --- | --- |
| `type`&lt;br&gt;&lt;br&gt;string | **Required.**&lt;br&gt;Must be &quot;order_status&quot; |
| `body`&lt;br&gt;&lt;br&gt;object | **Required.**&lt;br&gt;&lt;br&gt;An object with the body of the message. The object contains the following field:&lt;br&gt;&lt;br&gt;`text` string&lt;br&gt;&lt;br&gt;- **Required** if `body` is present. The content of the message. Emojis and markdown are supported. Maximum length is 1024 characters. |
| `footer`&lt;br&gt;&lt;br&gt;object | **Optional.**&lt;br&gt;&lt;br&gt;An object with the footer of the message. The object contains the following field:&lt;br&gt;&lt;br&gt;`text` string&lt;br&gt;&lt;br&gt;- **Required** if `footer` is present. The footer content. Emojis, markdown, and links are supported. Maximum length is 60 characters. |
| `action`&lt;br&gt;&lt;br&gt;object | **Required.**&lt;br&gt;&lt;br&gt;An action object you want the user to perform after reading the message. This action object contains the following fields:&lt;br&gt;&lt;br&gt;`name` string&lt;br&gt;&lt;br&gt;- **Required**. Must be &quot;review_order&quot;.&lt;br&gt;&lt;br&gt;`parameters` object&lt;br&gt;&lt;br&gt;- See [Parameters Object](#paramobject-orderstatus) for information. |

#### Parameters object &#123;#paramobject-orderstatus&#125;

The `parameters` object contains the following fields:

| Value | Description |
| --- | --- |
| `reference_id`&lt;br&gt;&lt;br&gt;string | **Required.**&lt;br&gt;&lt;br&gt;The ID sent by the business in the `order_details` message. |
| `order`&lt;br&gt;&lt;br&gt;object | **Required.**&lt;br&gt;This object contains the following fields:&lt;br&gt;&lt;br&gt;`status` string&lt;br&gt;* **Required.** The new order `status`. Must be one of `processing`, `partially_shipped`, `shipped`,  `completed`, `canceled`.&lt;br&gt;&lt;br&gt;`description` string&lt;br&gt;* **Optional.** Text for sharing status related information in `order_details`. Could be useful while sending cancellation. Max character limit is 120 characters. |

`order_status` message introduces two new errors that are summarized below.

| Error Code | Description |
| --- | --- |
| `2046` - Invalid status transition | The order status transition is not allowed. |
| `2047` - Cannot cancel order | Cannot cancel the order since the user has already paid for it. |

#### Product experience

Customers receive each `order_status` update as a separate message in their chat thread, that references their original `order_details` message as shown below (left). The order details page always displays the latest valid status communicated to the customer using the `order_status` message as shown below (right).

#### Supported order status and transitions &#123;#valid-order-status-transition&#125;

Currently the following order status values are supported:

| Value | Description |
| --- | --- |
| `pending` | User has not successfully paid yet |
| `processing` | User payment authorized, merchant/partner is fulfilling the order, performing service, and so on. |
| `partially-shipped` | A portion of the products in the order have been shipped by the merchant |
| `shipped` | All the products in the order have been shipped by the merchant |
| `completed` | The order is completed and no further action is expected from the user or the partner/merchant |
| `canceled` | The partner/merchant would like to cancel the `order_details` message for the order/invoice. The status update will fail if there is already a `successful` or `pending` payment for this `order_details` message |

Order status transitions are restricted for consistency of consumer experience. Allowed status transitions are summarized below:

   * Initial status of an order is always `pending`, which is sent in `order_details` message.
 * `canceled` and `completed` are terminal status and cannot be updated to any other status.
 * `pending` can transition to any of the other statuses including `processing`, `shipped`, `partially-shipped`.
* `processing`, `shipped` and `partially-shipped` are equivalent statuses and can transition between one another or to one of the terminal statuses.

Upon sending an `order_status` message with an invalid transition, you will receive an error webhook with the error code `2046` and message &quot;New order status was not correctly transitioned.&quot;

#### Canceling an order &#123;#canceling-order&#125;

An order can be `canceled` by sending an `order_status` message with the status `canceled`. The customer cannot pay for an order that is canceled. The customer receives an `order_status` message  and order details page is updated to show that the order is canceled and the &quot;Continue&quot; button removed. The _optional_ text shown below &quot;Order canceled&quot; on the order details page can be specified using the `description` field in the `order_status` message.

An order can be canceled only if the user has not already paid for the order. If the user has paid and you send an `order_status` message with `canceled` status, you will receive an error webhook with error code `2047` and message &quot;Could not change order status to &#039;canceled&#039;&quot;.

### Step 7: Reconcile payments

WhatsApp does not support payment reconciliations. Businesses should use their payment gateway account to reconcile the payments using the `reference_id` provided in the `order_details` messages and the `id` of the transactions returned as part of the payment lookup query.

### Step 8: Refunds &#123;#step-6&#125;

Business can initiate a refund for an order. To do that, businesses must make a POST call to the `/[PHONE_NUMBER_ID]/payments_refund` endpoint with the following `JSON` object:

```json
&#123;
  &quot;reference_id&quot;: &quot;reference-id-value&quot;,
  &quot;speed&quot;: &quot;normal&quot;,
  &quot;payment_config_id&quot;: &quot;payment-config-id&quot;,
  &quot;amount&quot;: &#123;
    &quot;currency&quot;: &quot;INR&quot;,
    &quot;value&quot;: &quot;100&quot;,
    &quot;offset&quot;: &quot;100&quot;
  &#125;
&#125;
```

The following table describes the fields in the refunds endpoint request object:

| Field | Description |
| --- | --- |
| `reference_id`&lt;br&gt;&lt;br&gt;string | **Required.**&lt;br&gt;&lt;br&gt;Unique reference ID for the order sent in `order_details` message. |
| `speed`&lt;br&gt;&lt;br&gt;string | **Optional.**&lt;br&gt;&lt;br&gt;Speed by which refund should be processed. Can be one of `instant` or `normal`. |
| `payment_config_id`&lt;br&gt;&lt;br&gt;string | **Required.**&lt;br&gt;&lt;br&gt;Payment configuration for the order sent in `order_details` message. |
| `amount`&lt;br&gt;&lt;br&gt;object | **Required.**&lt;br&gt;&lt;br&gt;The `amount` object contains the following fields:&lt;br&gt;&lt;br&gt;`offset` string&lt;br&gt;&lt;br&gt;- **Required.** Must be `100` for `INR`.&lt;br&gt;&lt;br&gt;`value` string&lt;br&gt;&lt;br&gt;- **Required.** Positive integer representing the amount value multiplied by offset. For example, ₹12.34 has value 1234.&lt;br&gt;&lt;br&gt;`currency` string&lt;br&gt;&lt;br&gt;- **Required.** The currency for this refund. Currently the only supported value is INR. |

Businesses should expect a response in the same HTTP session (not in a webhook notification) that contains the following fields:

| Field | Description |
| --- | --- |
| `id`&lt;br&gt;&lt;br&gt;string | **Required.**&lt;br&gt;&lt;br&gt;A unique ID representing the initiated refund. |
| `status`&lt;br&gt;&lt;br&gt;string | **Required.**&lt;br&gt;&lt;br&gt;Status of the refund. Can be one of `pending`, `failed` or `completed` |
| `speed_processed`&lt;br&gt;&lt;br&gt;string | **Required.**&lt;br&gt;&lt;br&gt;Speed by which refund was processed. Can be one of `instant` or `normal`. PGs are the ultimate arbitrator of which speed mode refund goes through. This might NOT always match what was included in the request parameters. |

An example successful response looks like this:

```json
&#123;
  &quot;id&quot;: &quot;refund-id&quot;,
  &quot;status&quot;: &quot;pending&quot;,
  &quot;speed_processed&quot;: &quot;normal&quot;
&#125;
```

## Merchant preferred UPI payment method

Now merchants can specify up to one UPI Payment app to show up in checkout flow. Merchant preferred payment app will be shown on top of the list of available UPI apps in the &quot;Choose payment method&quot; screen. To enable this capability, partners must specify the external app-id in the Order Details or order-invoice message.

Note: This feature is available on consumer apps on and above version: 2.24.21.0

### Updates to order details payload

```json
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;interactive&quot;: &#123;
    &quot;action&quot;: &#123;
      &quot;name&quot;: &quot;review_and_pay&quot;,
      &quot;parameters&quot;: &#123;
        &quot;payment_settings&quot;: [
           &#123;
             &quot;type&quot;: &quot;payment_gateway&quot;,
             &quot;payment_gateway&quot;: &#123;
               &quot;preferred_payment_methods&quot;: [
                 &#123;
                   &quot;method&quot;: &quot;Application-ID&quot;
                 &#125;
               ]
             &#125;
           &#125;

        ],
      &quot;order&quot;: ..
      &#125;
    &#125;
  &#125;
&#125;
```

### List of supported apps:
| UPI Application | Application ID to be passed in Order Details payload |
| --- | --- |
| Google Pay | gpay |
| PhonePe | phonepe |
| PayTm | paytm |
| BHIM | bhim |
| Amazon Pay | amazonpay |
| CRED | cred |
| Mobikwik | mobikwik |

## Restrict available payment options

Merchants can specify which payment options to show in checkout flow between UPI and Web options. This will allow merchants to enable only UPI or credit card(any PG available option) to accept payments for invoices.

**Warning:** UPI transactions are limited to ₹5,00,000. For higher amounts, set `enabled_payment_options` to `[&quot;web&quot;]` to use your payment gateway&#039;s web checkout. Payments with UPI enabled above this limit will fail.

Note: This feature is available on consumer apps on and above version: 2.24.22.4

### Updates to order details payload

```json
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;interactive&quot;: &#123;
    &quot;action&quot;: &#123;
      &quot;name&quot;: &quot;review_and_pay&quot;,
      &quot;parameters&quot;: &#123;
        &quot;payment_settings&quot;: [
           &#123;
             &quot;type&quot;: &quot;payment_gateway&quot;,
             &quot;payment_gateway&quot;: &#123;
                &quot;enabled_payment_options&quot;: [&quot;upi&quot;/&quot;web&quot;]
             &#125;
           &#125;
        ],
      &quot;order&quot;: ...
      &#125;
    &#125;
  &#125;
&#125;
```

### List of payment options
| Enabled Option | Experience in checkout flow |
| --- | --- |
| upi | Only UPI apps are shown in checkout flow. |
| web | Payment gateway webpage is loaded and merchant payment gateway account configured payment options will be shown in the checkout flow. |

**Warning:** Some Payment Gateways allow customization of payment options that are shown in payment link or web based checkout flow. Please contact Payment Gateway to restricting payment options in payment link or web page.

## Third party validation with Razorpay and PayU payment gateways

TPV is now supported for RazorPay and PayU merchants, allowing merchants to specify the consumer accounts from which orders needs to be paid. Since, the consumer bank account information is sensitive, please work with Payment Gateways to procure public encryption key and pass the encryption information as part of Order details message.

To use this feature which is in alpha testing, please reach out to Meta payments team - whatsappindia-bizpayments-support&#064;meta.com

### Updates to order details payload to support TPV for Razorpay merchants

```json
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;interactive&quot;: &#123;
    &quot;action&quot;: &#123;
      &quot;name&quot;: &quot;review_and_pay&quot;,
      &quot;parameters&quot;: &#123;
        &quot;payment_settings&quot;: [
          &#123;
            &quot;type&quot;: &quot;razorpay&quot;,
            &quot;razorpay&quot;: &#123;
              &quot;encrypted_payment_gateway_data&quot;: &quot;encrypted-data&quot;
            &#125;
          &#125;
        ],
        &quot;order&quot;: &#123;&#125;
      &#125;
    &#125;
  &#125;
&#125;
```

The raw value before encryption should look something like the following:

```json
&#123;
  &quot;bank_account&quot;: &#123;
    &quot;account_number&quot;: &quot;account-no&quot;,
    &quot;name&quot;: &quot;consumer-cbs-name&quot;,
    &quot;ifsc&quot;: &quot;ifsc-code&quot;
  &#125;
&#125;
```

### Updates to order details payload to support TPV for PayU merchants

```json
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;interactive&quot;: &#123;
    &quot;action&quot;: &#123;
      &quot;name&quot;: &quot;review_and_pay&quot;,
      &quot;parameters&quot;: &#123;
        &quot;payment_settings&quot;: [
          &#123;
            &quot;type&quot;: &quot;payu&quot;,
            &quot;payu&quot;: &#123;
              &quot;encrypted_payment_gateway_data&quot;: &quot;encrypted-data&quot;
            &#125;
          &#125;
        ],
        &quot;order&quot;: &#123;&#125;
      &#125;
    &#125;
  &#125;
&#125;
```

The raw value before encryption should look like the following:

```json
&#123;
  &quot;beneficiaryDetail&quot; : &#123;
    &quot;beneficiaryAccountNumber&quot; : &quot;account_number1|account_number2&quot;,
    &quot;ifscCode&quot; : &quot;ifsc1|ifsc2&quot;
  &#125;
&#125;
```

**Warning:** Note please closely work with Meta and Payment Gateway teams(RazorPay or PayU) to unlock this feature as this is still in alpha testing phase.

## Security considerations

Businesses should comply with local security and regulatory requirements in India. They should not rely solely on the status of the transaction provided in the webhook and must use payment lookup API to retrieve the statuses directly from WhatsApp. Businesses must always sanitize/validate the data in the API responses or webhooks to protect against SSRF attacks.

## Checklist for integrated merchants
* Ensure that an `order_status` message is sent to the consumer informing them about updates to an order after receiving transaction updates for an order.

* Ensure the merchant is verified and WABA contact is marked with a verified check.

* Verify the WABA is mapped to appropriate merchant initiated messaging tier(1k, 10k, and 100k per day)

* Merchant should list the customer support information in the profile screen in case consumer wants to report any issues.

* Migrate to &quot;payment_settings&quot; in place of &quot;payment_type&quot; and &quot;payment_configuration&quot;. This is the recommended way, and gives access to features likes &quot;notes&quot; and &quot;udf&quot; fields. For an example, [view the payloads above](#step-1).
