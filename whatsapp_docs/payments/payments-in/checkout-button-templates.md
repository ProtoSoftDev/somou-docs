# Checkout button templates



Checkout button templates are marketing templates that can showcase one or more products along with corresponding checkout buttons that WhatsApp users can use to make purchases without leaving the WhatsApp client.

## Single products

Checkout button templates can show a single product image or video header, along with message body text, message footer, a single checkout button, and up to 9 quick-reply buttons.

WhatsApp users who tap the button will see details of the order:

Users can proceed by selecting shipping information provided by you (if you know their information and supplied it in the send message payload)...

... or can add their own shipping information:

## Enabling coupons, real-time inventory, and pricing updates &#123;#enabling_coupons_inventory&#125;

**Warning:** Enabling coupons, real-time inventory and pricing updates is currently in beta and only available to India businesses and WhatsApp users with an India country calling code. Please reach out to whatsappindia-bizpayments-support&#064;meta.com to know more.

To enable coupons, real-time inventory and pricing updates, you can set up a checkout endpoint that can exchange data in real time to update the order on the WhatsApp client. It enables businesses to receive the shipping address and offers coupons based on the order and allows users to apply the coupon. It also enables businesses to validate inventory and serviceability on the order before the user completes the checkout.

Setting up the checkout endpoint consists of the following steps and it&#039;s the same method that [WhatsApp Flows endpoint](https://developers.facebook.com/documentation/business-messaging/whatsapp/flows/guides/implementingyourflowendpoint#implementing-endpoint-for-flows) uses to share the data with WhatsApp clients.

* Create a key pair and upload and sign the public key using the [Cloud API](https://developers.facebook.com/documentation/business-messaging/whatsapp/flows/cloud-api/reference/whatsapp-business-encryption#set-business-public-key).
* [Setup the endpoint](#setup_the_endpoint)
* [Implement Payload Encryption/Decryption](#implement_encryption_decryption)
* [Link the checkout endpoint with payment configuration](#link_checkout_endpoint)
* [Implement checkout endpoint logic](#implement_checkout_logic)

### Set up the endpoint &#123;#setup_the_endpoint&#125;

WhatsApp client makes an HTTPS request to exchange the data with the business endpoint. You should make sure the endpoint is configured properly to accept the request and link the endpoint url with the [payment configuration](https://developers.facebook.com/documentation/business-messaging/whatsapp/payments/payments-in/pg#link-your-payment-account):

```html
https://business.com/checkout
```

Your server must be enabled to receive and process `POST` requests, use `HTTPS` and have a valid TLS/SSL certificate installed. This certificate does not have to be used in payload encryption/decryption.

### Implement encryption/decryption &#123;#implement_encryption_decryption&#125;

The body of each request contains the encrypted payload and has the following form:

#### Sample endpoint request syntax

```html
&#123;
  encrypted_flow_data: &quot;&lt;ENCRYPTED_FLOW_DATA&gt;&quot;,
  encrypted_aes_key: &quot;&lt;ENCRYPTED_AES_KEY&gt;&quot;,
  initial_vector: &quot;&lt;INITIAL_VECTOR&gt;&quot;
&#125;
```

| Parameter | Description |
| --- | --- |
| `encrypted_flow_data`&lt;br&gt;string | **Required.**&lt;br&gt;The encrypted request payload. |
| `encrypted_aes_key`&lt;br&gt;string | **Required.**&lt;br&gt;The encrypted 128-bit AES key. |
| `initial_vector`&lt;br&gt;string | **Required.**&lt;br&gt;The 128-bit initialization vector. |

After processing the decrypted request, create a response and encrypt it before sending it back to the WhatsApp client. Encrypt the payload using the AES key received in the request and send it back as a Base64 string.

You can refer to examples of how to [decrypt and encrypt](https://developers.facebook.com/documentation/business-messaging/whatsapp/flows/guides/implementingyourflowendpoint#request-decryption-and-encryption).

If a request cannot be decrypted, the endpoint should return HTTP 421 response status code (see [Business Endpoint Error Codes](https://developers.facebook.com/documentation/business-messaging/whatsapp/flows/reference/error-codes#endpoint_error_codes) for more details).

#### Sample endpoint response

```curl
curl -i -H &quot;Content-Type: application/json&quot; -X POST -d &#039;&#123;
&quot;encrypted_flow_data&quot;:&quot;4Wor0bpfvrNqnkH+XQZLn3HnU2Zi7hG\\/UHjISS93Fzn9J7youssaLeXlNUH&quot;,
&quot;encrypted_aes_key&quot;:&quot;ufA0fXD1Wz...&quot;,
&quot;initial_vector&quot;:&quot;G\\/1rq1naEOMR4TJHFvIs\\/Q==.&quot;
&#125;&#039; &#039;https://business.com/checkout&#039;

HTTP/2 200
content-type: text/plain
content-length: 232
date: Wed, 06 Jul 2022 14:03:03 GMT

yZcJQaH3AqfzKgjn64vAcASaJrOMN27S6CESyU68WN/cDCP6abskoMa/pPjszXGKyyh/23lw84HW6ZilMfU6KL3j5AWwOx6GWNwtq8Aj7gz/Y7R+LccmJWxKo2UccMu5xJlduIFlFlOS1gAnOwKrk8wpuprsi4jAOspw3xO2uh3J883aC/csu/MhRPiYCaGGy/tTNvVDmb2Gw1WXFmpvLsZ/SBrgG0cDQJjQzpTO
```

### Link the checkout endpoint with payment configuration &#123;#link_checkout_endpoint&#125;

**Warning:** The business should have payment gateway based [payment configuration](https://developers.facebook.com/documentation/business-messaging/whatsapp/payments/payments-in/pg#link-your-payment-account) and reach out to whatsappindia-bizpayments-support&#064;meta.com to enable the WhatsApp Business account for checkout endpoint linking with payment configuration.

Prior to linking the checkout endpoint, you should create a [payment configuration](https://developers.facebook.com/documentation/business-messaging/whatsapp/payments/payments-in/pg#link-your-payment-account) and link with the payment gateway account. Use the linked payment configuration only with checkout button template integration.

You can achieve the endpoint linking with payment configuration by following [Onboarding API&#039;s - Link data endpoint]( /documentation/business-messaging/whatsapp/payments/payments-in/onboarding-apis#link-or-update-data-endpoint)

### Implement checkout endpoint logic &#123;#implement_checkout_logic&#125;

WhatsApp checkout endpoint integration inherits the &#039;data_exchange&#039; similar to Flows and supports a set of subactions based on the user interaction and passes the relevant information in each of these actions to allow businesses to provide user specific coupons and enable businesses to update the pricing information accordingly.

| Sub Action | Method | Description |
| --- | --- | --- |
| `get_coupons` | Request | When users click on a savings offer CTA, WhatsApp passes [order parameters](https://developers.facebook.com/documentation/business-messaging/whatsapp/payments/payments-in/pg#paramobject) excluding the [payment settings](https://developers.facebook.com/documentation/business-messaging/whatsapp/payments/payments-in/pg#paymentsettingsobject). It also passes the `user phone number` as an input parameter.&lt;br&gt;&lt;br&gt;```json
&#123;
  &quot;input&quot;:
  &#123;
    &quot;user_id&quot;: &quot;user_phone_number&quot;
  &#125;
&#125;
```&lt;br&gt;&lt;br&gt;Refer [get coupons request](#get_coupon_request) example to understand the order and input parameters |
|  | Response | Checkout endpoint expected to pass the list of coupon information, such as code, id and description.&lt;br&gt;&lt;br&gt;```json
&#123;
  &quot;coupons&quot;:
    [
        &#123;
            &quot;code&quot;: &quot;coupon_code&quot;,
            &quot;id&quot;: &quot;coupon_id&quot;,
            &quot;description&quot;: &quot;coupon_description&quot;
        &#125;
    ]

&#125;
```&lt;br&gt;&lt;br&gt;Refer [get coupons response](#get_coupon_response) example to understand the expected response. |
| `apply_coupon` | Request | When users select or enter a coupon, WhatsApp passes [order parameters](https://developers.facebook.com/documentation/business-messaging/whatsapp/payments/payments-in/pg#paramobject) excluding the [payment settings](https://developers.facebook.com/documentation/business-messaging/whatsapp/payments/payments-in/pg#paymentsettingsobject). It also passes the `user phone number` and information about the coupon to be applied as an input parameter.&lt;br&gt;&lt;br&gt;```json
&#123;
  &quot;input&quot;:
  &#123;
    &quot;user_id&quot;: &quot;user_phone_number&quot;,
    &quot;coupon&quot;:
    &#123;
      &quot;code&quot;: &quot;WELCOME70&quot;
    &#125;
  &#125;
&#125;
```&lt;br&gt;&lt;br&gt;Refer [apply coupon request](#apply_coupon_request) example to understand the order and input parameters |
|  | Response | Checkout endpoint expected to update the item and order pricing in [order parameters](https://developers.facebook.com/documentation/business-messaging/whatsapp/payments/payments-in/pg#paramobject) and attach the coupon with the order&lt;br&gt;&lt;br&gt;Refer to [apply coupon response](#apply_coupon_response) example to understand the expected response. |
| `remove_coupon` | Request | When users try to remove an applied coupon, WhatsApp passes [order parameters](https://developers.facebook.com/documentation/business-messaging/whatsapp/payments/payments-in/pg#paramobject) excluding the [payment settings](https://developers.facebook.com/documentation/business-messaging/whatsapp/payments/payments-in/pg#paymentsettingsobject). It also passes the `user phone number` as an input parameter.&lt;br&gt;&lt;br&gt;```json
&#123;
  &quot;input&quot;:
  &#123;
    &quot;user_id&quot;: &quot;user_phone_number&quot;
  &#125;
&#125;
```&lt;br&gt;&lt;br&gt;Refer [remove coupon request](#remove_coupon_request) example to understand the expected response. |
|  | Response | Checkout endpoint expected to update the item and order pricing in [order parameters](https://developers.facebook.com/documentation/business-messaging/whatsapp/payments/payments-in/pg#paramobject) and remove the coupon attached with the order.&lt;br&gt;&lt;br&gt;Refer [remove coupon response](#remove_coupon_response) example to understand the expected response. |
| `apply_shipping` | Request | When users try to submit a shipping address, WhatsApp passes [order parameters](https://developers.facebook.com/documentation/business-messaging/whatsapp/payments/payments-in/pg#paramobject) excluding the [payment settings](https://developers.facebook.com/documentation/business-messaging/whatsapp/payments/payments-in/pg#paymentsettingsobject). It also passes the `user phone number` and shipping information as an input parameter.&lt;br&gt;&lt;br&gt;```json
&#123;
  &quot;input&quot;:
  &#123;
    &quot;user_id&quot;: &quot;user_phone_number&quot;
  &#125;
&#125;
```&lt;br&gt;&lt;br&gt;Refer to the [apply shipping request](#apply_shipping_request) example to understand the expected response. |
|  | Response | Checkout endpoint expected to update the item and shipping pricing in [order parameters](https://developers.facebook.com/documentation/business-messaging/whatsapp/payments/payments-in/pg#paramobject).&lt;br&gt;&lt;br&gt;Refer to the [apply shipping response](#apply_shipping_response) example to understand the expected response. |

A [checkout endpoint example](https://github.com/WhatsApp/WhatsApp-Checkout-Button-Template-Endpoint) in Node.js is available that you can clone (remix) on Glitch to create your own endpoint and quickly prototype your checkout logic. Follow the instructions in the [README.md](https://github.com/WhatsApp/WhatsApp-Checkout-Button-Template-Endpoint/blob/main/README.md) file to get started. Using Glitch is entirely optional. You can clone the example code from Glitch and run it in any environment you prefer.

Upon completing the above steps, when business sends the checkout template with the linked payment configuration, WhatsApp enables the coupons, real-time inventory and pricing updates and allows users to apply coupons and share shipping addresses.

When enabled, the `Apply a savings offer` will appear in the order summary screen.

User can click on `Apply a savings offer` to explore the coupons, at this point WhatsApp makes `get_coupons` request to fetch the list of coupons based on the passed order and `user phone number` information.

When the user tries to apply a coupon, WhatsApp makes `apply_coupon` and allow businesses to update the order or item pricing based on the selected coupon.

Similar to coupons, user can share the shipping address by clicking on `Add shipping address` and select the addresses saved with the businesses or add new address. WhatsApp makes `apply_shipping` request when user tries to submit the address and allow businesses to check inventory and logistics based on the address provided.

Users can then continue to place the order using their preferred payment method set up in the WhatsApp client:

Once the order is processed, a [payment webhook](https://developers.facebook.com/documentation/business-messaging/whatsapp/payments/payments-in/pg#step-2--receive-webhook-about-transaction-status) is triggered.

## Multiple products

You can create a [media card carousel template](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/marketing-templates/media-card-carousel-templates) that showcases up to 10 products in a card carousel, each with their own checkout button. To do this, simply create a media card carousel template as you normally would, but replace one of the buttons with a [checkout button](#checkout-buttons), and make sure that it is the first button in the card.

Checkout buttons in media card carousel templates trigger the same order and payment flow as checkout buttons in templates that showcase a single product.

## Checkout buttons

Each checkout button in a template must correspond to a single product. Checkout buttons, when creating a template, must have the following non-customizable syntax:

```json
&#123;
  &quot;type&quot;: &quot;order_details&quot;,
  &quot;text&quot;: &quot;Buy now&quot;
&#125;
```

Note that this is simply a button definition. The actual details about the product that maps to this button are included when you [send the template](#send-a-checkout-button-template) in a template message. For example:

```json
&#123;
  &quot;type&quot;: &quot;button&quot;,
  &quot;sub_type&quot;: &quot;order_details&quot;,
  &quot;index&quot;: 0,
  &quot;parameters&quot;: [
    &#123;
      &quot;type&quot;: &quot;action&quot;,
      &quot;action&quot;: &#123;
        &quot;order_details&quot;: &#123;
          &quot;reference_id&quot;: &quot;abc.123_xyz-1&quot;,
          &quot;type&quot;: &quot;physical-goods&quot;,
          &quot;currency&quot;: &quot;INR&quot;,
          &quot;payment_settings&quot;: [
            &#123;
              &quot;type&quot;: &quot;payment_gateway&quot;,
              &quot;payment_gateway&quot;: &#123;
                &quot;type&quot;: &quot;razorpay&quot;,
                &quot;configuration_name&quot;: &quot;prod-razor-pay-config-05&quot;
              &#125;
            &#125;
          ],
          &quot;shipping_info&quot;: &#123;
            &quot;country&quot;: &quot;IN&quot;,
            &quot;addresses&quot;: [
              &#123;
                &quot;name&quot;: &quot;Nidhi Tripathi&quot;,
                &quot;phone_number&quot;: &quot;919000090000&quot;,
                &quot;address&quot;: &quot;Bandra Kurla Complex&quot;,
                &quot;city&quot;: &quot;Mumbai&quot;,
                &quot;state&quot;: &quot;Maharastra&quot;,
                &quot;in_pin_code&quot;: &quot;400051&quot;,
                &quot;house_number&quot;: &quot;12&quot;,
                &quot;tower_number&quot;: &quot;5&quot;,
                &quot;building_name&quot;: &quot;One BKC&quot;,
                &quot;landmark_area&quot;: &quot;Near BKC Circle&quot;
              &#125;
            ]
          &#125;,
          &quot;order&quot;: &#123;
            &quot;items&quot;: [
              &#123;
                &quot;amount&quot;: &#123;
                  &quot;offset&quot;: 100,
                  &quot;value&quot;: 200000
                &#125;,
                &quot;sale_amount&quot;: &#123;
                  &quot;offset&quot;: 100,
                  &quot;value&quot;: 150000
                &#125;,
                &quot;name&quot;: &quot;Blue Elf Aloe&quot;,
                &quot;quantity&quot;: 1,
                &quot;country_of_origin&quot;: &quot;India&quot;,
                &quot;importer_name&quot;: &quot;Lucky Shrub Imports and Exports&quot;,
                &quot;importer_address&quot;: &#123;
                  &quot;address_line1&quot;: &quot;One BKC&quot;,
                  &quot;address_line2&quot;: &quot;Bandra Kurla Complex&quot;,
                  &quot;city&quot;: &quot;Mumbai&quot;,
                  &quot;zone_code&quot;: &quot;MH&quot;,
                  &quot;postal_code&quot;: &quot;400051&quot;,
                  &quot;country_code&quot;: &quot;IN&quot;
                &#125;
              &#125;
            ],
            &quot;subtotal&quot;: &#123;
              &quot;offset&quot;: 100,
              &quot;value&quot;: 150000
            &#125;,
            &quot;shipping&quot;: &#123;
              &quot;offset&quot;: 100,
              &quot;value&quot;: 20000
            &#125;,
            &quot;tax&quot;: &#123;
              &quot;offset&quot;: 100,
              &quot;value&quot;: 10000
            &#125;,
            &quot;discount&quot;: &#123;
              &quot;offset&quot;: 100,
              &quot;value&quot;: 15000,
              &quot;description&quot;: &quot;Additional 10% off&quot;
            &#125;,
            &quot;status&quot;: &quot;pending&quot;,
            &quot;expiration&quot;: &#123;
              &quot;timestamp&quot;: &quot;1726627150&quot;
            &#125;
          &#125;,
          &quot;total_amount&quot;: &#123;
            &quot;offset&quot;: 100,
            &quot;value&quot;: 165000
          &#125;
        &#125;
      &#125;
    &#125;
  ]
&#125;
```

If you are sending a [media card carousel template](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/marketing-templates/media-card-carousel-templates) (which can have two or more products), each checkout button must be defined in the template, and the item details that map to each button must be included when sending the template.

## Creating checkout button templates

Use the [**POST /&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;/message_templates**](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/message-template-api#post-version-waba-id-message-templates) endpoint to create a template that uses a checkout button.

### Request syntax

```json
POST /&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;/message_templates
```

### Post body

The post body below is for a checkout button template that shows a single button. See the [Media Card Carousel Templates](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/marketing-templates/media-card-carousel-templates) document to see carousel template post body syntax.

```json
&#123;
  &quot;name&quot;: &quot;&lt;TEMPLATE_NAME&gt;&quot;,
  &quot;language&quot;: &quot;&lt;TEMPLATE_LANGUAGE&gt;&quot;,
  &quot;category&quot;: &quot;marketing&quot;,
  &quot;components&quot;: [
    &#123;
      &quot;type&quot;: &quot;header&quot;,
      &quot;format&quot;: &quot;&lt;MESSAGE_HEADER_FORMAT&gt;&quot;,
      &quot;example&quot;: &#123;
        &quot;header_handle&quot;: [
          &quot;&lt;MESSAGE_HEADER_ASSET_HANDLE&gt;&quot;
        ]
      &#125;
    &#125;,
    &#123;
      &quot;type&quot;: &quot;body&quot;,
      &quot;text&quot;: &quot;&lt;MESSAGE_BODY_TEXT&gt;&quot;,
      &quot;example&quot;: &#123;
        &quot;body_text&quot;: [
          [
            &quot;&lt;MESSAGE_BODY_TEXT_VARIABLE_EXAMPLE&gt;&quot;,
            &quot;&lt;MESSAGE_BODY_TEXT_VARIABLE_EXAMPLE&gt;&quot;
          ]
        ]
      &#125;
    &#125;,

    /* Footer component is optional */
    &#123;
      &quot;type&quot;: &quot;footer&quot;,
      &quot;text&quot;: &quot;&lt;MESSAGE_FOOTER_TEXT&gt;&quot;
    &#125;,

    &#123;
      &quot;type&quot;: &quot;buttons&quot;,
      &quot;buttons&quot;: [
        &#123;
          &quot;type&quot;: &quot;order_details&quot;,
          &quot;text&quot;: &quot;Buy now&quot;
        &#125;,

        /* Quick-reply buttons are optional; up to 9 permitted */
        &#123;
          &quot;type&quot;: &quot;quick_reply&quot;,
          &quot;text&quot;: &quot;&lt;QUICK_REPLY_BUTTON_LABEL_TEXT&gt;&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

### Post body parameters

| Placeholder | Description | Example Value |
| --- | --- | --- |
| `&lt;MESSAGE_BODY_TEXT&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Message body text. Supports variables.&lt;br&gt;&lt;br&gt;Maximum 1024 characters. | `Hi &#123;&#123;1&#125;&#125;! The &#123;&#123;2&#125;&#125; is back in stock! Order now before it&#039;s gone!` |
| `&lt;MESSAGE_BODY_TEXT_VARIABLE_EXAMPLE&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required if message body text string uses variables.**&lt;br&gt;&lt;br&gt;Message body text example variable string(s). Number of strings must match the number of variable placeholders in the message body text string.&lt;br&gt;&lt;br&gt;If message body text uses a single variable, `body_text` value can be a string, otherwise it must be an array containing an array of strings. | `Pablo` |
| `&lt;MESSAGE_FOOTER_TEXT&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required if using a message footer.**&lt;br&gt;&lt;br&gt;Message footer text string.&lt;br&gt;&lt;br&gt;60 characters maximum. | `Tap &#039;Stop&#039; below to stop back-in-stock reminders.` |
| `&lt;MESSAGE_HEADER_ASSET_HANDLE&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required if using a non-text media header.**&lt;br&gt;&lt;br&gt;Uploaded media asset handle. Use the [Resumable Upload API](https://developers.facebook.com/docs/graph-api/guides/upload) to generate an asset handle.&lt;br&gt;&lt;br&gt;Media assets are automatically cropped to a wide ratio based on the WhatsApp user&#039;s device. | `4::anBlZw==:ARa525ZJ1g0J-8egeiRvb4Z4r9RSi9qeKF7-wXsUiaDFsll5CKbu5H7h_9mTW0TDfA8LEGHC4bAeXtJJiVQADMp5Ooe2huQlhpBxMadJiu3qVg:e:1724535430:634974688087057:100089620928913:ARaQoFQMm6BlbI3MYo4` |
| `&lt;MESSAGE_HEADER_FORMAT&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Message header format. Value can be `image` or `video`. | `image` |
| `&lt;QUICK_REPLY_BUTTON_LABEL_TEXT&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required if using a quick-reply button.**&lt;br&gt;&lt;br&gt;Quick-reply button label text.&lt;br&gt;&lt;br&gt;Maximum 25 characters. | `Stop` |
| `&lt;TEMPLATE_LANGUAGE&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Template [language and locale code](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/supported-languages). | `en_US` |
| `&lt;TEMPLATE_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Template name.&lt;br&gt;&lt;br&gt;Maximum 512 characters. | `item_back_in_stock_v1` |

### Example request

This example request creates a checkout button template with a single image message header, message body text that uses two variables, a footer, a single checkout button, and a quick-reply button.

```curl
curl &#039;https://graph.facebook.com/v25.0/102290129340398/message_templates&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039; \
-d &#039;
&#123;
  &quot;name&quot;: &quot;item_back_in_stock_v1&quot;,
  &quot;language&quot;: &quot;en_US&quot;,
  &quot;category&quot;: &quot;marketing&quot;,
  &quot;components&quot;: [
    &#123;
      &quot;type&quot;: &quot;header&quot;,
      &quot;format&quot;: &quot;image&quot;,
      &quot;example&quot;: &#123;
        &quot;header_handle&quot;: [
          &quot;3:NDU...&quot;
        ]
      &#125;
    &#125;,
    &#123;
      &quot;type&quot;: &quot;body&quot;,
      &quot;text&quot;: &quot;Hi &#123;&#123;1&#125;&#125;! The &#123;&#123;2&#125;&#125; is back in stock! Order now before it\&#039;s gone!&quot;,
      &quot;example&quot;: &#123;
        &quot;body_text&quot;: [
          [
            &quot;Pablo&quot;,
            &quot;Blue Elf Aloe&quot;
          ]
        ]
      &#125;
    &#125;,
    &#123;
      &quot;type&quot;: &quot;footer&quot;,
      &quot;text&quot;: &quot;Tap \&#039;Stop\&#039; below to stop back-in-stock reminders.&quot;
    &#125;,
    &#123;
      &quot;type&quot;: &quot;buttons&quot;,
      &quot;buttons&quot;: [
        &#123;
          &quot;type&quot;: &quot;order_details&quot;,
          &quot;text&quot;: &quot;Buy now&quot;
        &#125;,
        &#123;
          &quot;type&quot;: &quot;quick_reply&quot;,
          &quot;text&quot;: &quot;Stop&quot;
        &#125;
      ]
    &#125;
  ]
&#125;&#039;
```

## Send a checkout button template

Once your checkout button template or carousel template has been approved, you can send it in a template message.

### Request syntax

Use the [**POST /&lt;WHATSAPP_BUSINESS_PHONE_NUMBER_ID&gt;/messages**](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/message-api) endpoint to send an approved checkout button template or carousel template to a WhatsApp user.

```json
POST /&lt;WHATSAPP_BUSINESS_PHONE_NUMBER_ID&gt;/messages
```

### Post body

This post body syntax is for a checkout button template. See [Sending Media Card Carousel Templates](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/marketing-templates/media-card-carousel-templates) for media card carousel template post body payload syntax.

```json
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;recipient_type&quot;: &quot;individual&quot;,
  &quot;to&quot;: &quot;&lt;WHATSAPP_USER_PHONE_NUMBER&gt;&quot;,
  &quot;type&quot;: &quot;template&quot;,
  &quot;template&quot;: &#123;
    &quot;name&quot;: &quot;&lt;TEMPLATE_NAME&gt;&quot;,
    &quot;language&quot;: &#123;
      &quot;policy&quot;: &quot;deterministic&quot;,
      &quot;code&quot;: &quot;&lt;TEMPLATE_LANGUAGE&gt;&quot;
    &#125;,
    &quot;components&quot;: [
      &#123;
        &quot;type&quot;: &quot;header&quot;,
        &quot;parameters&quot;: [
          &#123;
            &quot;type&quot;: &quot;&lt;MESSAGE_HEADER_FORMAT&gt;&quot;,
            &quot;&lt;MESSAGE_HEADER_FORMAT&gt;&quot;: &#123;
              &quot;id&quot;: &quot;&lt;MESSAGE_HEADER_ASSET_ID&gt;&quot;
            &#125;
          &#125;
        ]
      &#125;,
      &#123;
        &quot;type&quot;: &quot;body&quot;,
        &quot;parameters&quot;: [
          &#123;
            &lt;MESSAGE_BODY_TEXT_VARIABLE&gt;
          &#125;,
          &#123;
            &lt;MESSAGE_BODY_TEXT_VARIABLE&gt;
          &#125;
        ]
      &#125;,
      &#123;
        &quot;type&quot;: &quot;button&quot;,
        &quot;sub_type&quot;: &quot;order_details&quot;,
        &quot;index&quot;: 0,
        &quot;parameters&quot;: [
          &#123;
            &quot;type&quot;: &quot;action&quot;,
            &quot;action&quot;: &#123;
              &quot;order_details&quot;: &#123;
                &quot;reference_id&quot;: &quot;&lt;REFERENCE_ID&gt;&quot;,
                &quot;currency&quot;: &quot;INR&quot;,
                &quot;type&quot;: &quot;&lt;PRODUCT_TYPE&gt;&quot;,
                &quot;payment_settings&quot;: [
                  &#123;
                    &quot;type&quot;: &quot;payment_gateway&quot;,
                    &quot;payment_gateway&quot;: &#123;
                      &quot;type&quot;: &quot;&lt;PAYMENT_GATEWAY_NAME&gt;&quot;,
                      &quot;configuration_name&quot;: &quot;&lt;PAYMENT_GATEWAY_CONFIGURATION_NAME&gt;&quot;
                    &#125;
                  &#125;
                ],

                /* &quot;shipping_info&quot; required for physical-goods type, else omit */
                &quot;shipping_info&quot;: &#123;
                  &quot;country&quot;: &quot;IN&quot;,
                  &quot;addresses&quot;: [

                    /* object required if you know recipient&#039;s address, otherwise omit (i.e., set &quot;addresses&quot; to an empty array) */
                    &#123;
                      &quot;name&quot;: &quot;&lt;SHIPPING_INFO_NAME&gt;&quot;,
                      &quot;phone_number&quot;: &quot;&lt;SHIPPING_INFO_PHONE_NUMBER&gt;&quot;,
                      &quot;address&quot;: &quot;&lt;SHIPPING_INFO_ADDRESS&gt;&quot;,
                      &quot;city&quot;: &quot;&lt;SHIPPING_INFO_CITY&gt;&quot;,
                      &quot;state&quot;: &quot;&lt;SHIPPING_INFO_STATE&gt;&quot;,
                      &quot;in_pin_code&quot;: &quot;&lt;SHIPPING_INFO_INDIA_PIN&gt;&quot;,
                      &quot;landmark_area&quot;: &quot;&lt;SHIPPING_INFO_LANDMARK_AREA&gt;&quot;,
                      &quot;house_number&quot;: &quot;&lt;SHIPPING_INFO_HOUSE_NUMBER&gt;&quot;,
                      &quot;tower_number&quot;: &quot;&lt;SHIPPING_INFO_TOWER_NUMBER&gt;&quot;,
                      &quot;building_name&quot;: &quot;&lt;SHIPPING_INFO_BUILDING_NAME&gt;&quot;
                    &#125;

                  ]
                &#125;,

                &quot;order&quot;: &#123;
                  &quot;items&quot;: [
                    &#123;
                      &quot;amount&quot;: &#123;
                        &quot;offset&quot;: 100,
                        &quot;value&quot;: &lt;ITEM_PRICE&gt;
                      &#125;,

                      /* &quot;sale_amount&quot; optional */
                      &quot;sale_amount&quot;: &#123;
                        &quot;offset&quot;: 100,
                        &quot;value&quot;: &lt;SALE_PRICE&gt;
                      &#125;,

                      &quot;name&quot;: &quot;&lt;ITEM_NAME&gt;&quot;,
                      &quot;quantity&quot;: &lt;ITEM_QUANTITY&gt;,
                      &quot;country_of_origin&quot;: &quot;&lt;ITEM_COUNTRY_OF_ORIGIN&gt;&quot;,
                      &quot;importer_name&quot;: &quot;&lt;IMPORTER_NAME&gt;&quot;,
                      &quot;importer_address&quot;: &#123;
                        &quot;address_line1&quot;: &quot;&lt;IMPORTER_ADDRESS_LINE_1&gt;&quot;,
                        &quot;address_line2&quot;: &quot;&lt;IMPORTER_ADDRESS_LINE_2&gt;&quot;,
                        &quot;city&quot;: &quot;&lt;IMPORTER_CITY&gt;&quot;,
                        &quot;zone_code&quot;: &quot;&lt;IMPORTER_ZONE_CODE&gt;&quot;,
                        &quot;postal_code&quot;: &quot;&lt;IMPORTER_POSTAL_CODE&gt;&quot;,
                        &quot;country_code&quot;: &quot;IN&quot;
                      &#125;
                    &#125;
                  ],
                  &quot;subtotal&quot;: &#123;
                    &quot;offset&quot;: 100,
                    &quot;value&quot;: &lt;SUBTOTAL_AMOUNT&gt;
                  &#125;
                  &quot;shipping&quot;: &#123;
                    &quot;offset&quot;: 100,
                    &quot;value&quot;: &lt;SHIPPING_AMOUNT&gt;
                  &#125;,
                  &quot;tax&quot;: &#123;
                    &quot;offset&quot;: 100,
                    &quot;value&quot;: &lt;TAX_AMOUNT&gt;,
                    &quot;description&quot;: &quot;&lt;TAX_DESCRIPTION&gt;&quot;
                  &#125;,

                  /* &quot;discount&quot; optional */
                  &quot;discount&quot;: &#123;
                    &quot;offset&quot;: 100,
                    &quot;value&quot;: &lt;DISCOUNT_AMOUNT&gt;,
                    &quot;description&quot;: &quot;&lt;DISCOUNT_DESCRIPTION&gt;&quot;
                  &#125;,
                  &quot;status&quot;: &quot;pending&quot;,

                  /* &quot;expiration&quot; optional */
                  &quot;expiration&quot;: &#123;
                    &quot;timestamp&quot;: &quot;&lt;EXPIRATION_TIMESTAMP&gt;&quot;
                  &#125;
                &#125;,
                &quot;total_amount&quot;: &#123;
                  &quot;offset&quot;: 100,
                  &quot;value&quot;: &lt;TOTAL_AMOUNT&gt;
                &#125;
              &#125;
            &#125;
          &#125;
        ]
      &#125;
    ]
  &#125;
&#125;
```

### Post body parameters

| Placeholder | Description | Example Value |
| --- | --- | --- |
| `&lt;DISCOUNT_AMOUNT&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | **Required if using a discount.**&lt;br&gt;&lt;br&gt;Discount amount, multiplied by discount.offset value.&lt;br&gt;&lt;br&gt;For example, to represent a discount of ₹2, the value would be `200`.&lt;br&gt;&lt;br&gt;Discount amount applies to the order subtotal. | `15000` |
| `&lt;DISCOUNT_DESCRIPTION&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Discount description.&lt;br&gt;&lt;br&gt;Maximum 60 characters. | `Additional 10% off` |
| `&lt;EXPIRATION_TIMESTAMP&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required if using an order expiration.**&lt;br&gt;&lt;br&gt;UTC timestamp indicating when we should disable the **Buy now** button. The timestamp will be used to generate a text string that appears at the bottom of the **Order details** window. For example:&lt;br&gt;&lt;br&gt;_This order expires on September 30, 2024 at 12:00 PM._&lt;br&gt;&lt;br&gt;WhatsApp users who view the message after this time will be unable to purchase the item using the checkout button.&lt;br&gt;&lt;br&gt;Values must represent a UTC time at least 300 seconds from when the send message request is sent to us. | `1726692927` |
| `&lt;IMPORTER_ADDRESS_LINE_1&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Importer address, line 1 (door, tower, number, street, etc.).&lt;br&gt;&lt;br&gt;Maximum 100 characters. | `One BKC` |
| `&lt;IMPORTER_ADDRESS_LINE_2&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Importer address, line 2 (landmark, area, etc.).&lt;br&gt;&lt;br&gt;Maximum 100 characters. | `Bandra Kurla Complex` |
| `&lt;IMPORTER_CITY&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Importer city.&lt;br&gt;&lt;br&gt;Maximum 120 characters. | `Mumbai` |
| `&lt;IMPORTER_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Importer name.&lt;br&gt;&lt;br&gt;Maximum 200 characters. | `Lucky Shrub Imports and Exports` |
| `&lt;IMPORTER_POSTAL_CODE&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Importer 6-digit postal index number.&lt;br&gt;&lt;br&gt;Maximum 6 digits. | `400051` |
| `&lt;IMPORTER_ZONE_CODE&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Importer two-letter zone code. | `MH` |
| `&lt;ITEM_COUNTRY_OF_ORIGIN&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Item&#039;s country of origin.&lt;br&gt;&lt;br&gt;Maximum 100 characters. | `India` |
| `&lt;ITEM_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Item name.&lt;br&gt;&lt;br&gt;Maximum 60 characters. | `Blue Elf Aloe` |
| `&lt;ITEM_PRICE&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | **Required.**&lt;br&gt;&lt;br&gt;Individual item price (price per item), multiplied by amount.offset value.&lt;br&gt;&lt;br&gt;For example, to represent an item price of ₹12.99, the value would be `1299`. | `200000` |
| `&lt;ITEM_QUANTITY&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | **Required.**&lt;br&gt;&lt;br&gt;Number of items in order, if order is placed.&lt;br&gt;&lt;br&gt;Maximum 100 integers. | `1` |
| `&lt;MESSAGE_BODY_TEXT_VARIABLE&gt;`&lt;br&gt;&lt;br&gt;_Object_ | **Required if template message body text uses variables, otherwise omit.**&lt;br&gt;&lt;br&gt;Object describing a message variable. If the template uses multiple variables, you must define an object for each variable.&lt;br&gt;&lt;br&gt;Supports `text`, `currency`, and `date_time` types. See [Messages Parameters](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/message-api#parameter-object).&lt;br&gt;&lt;br&gt;There is no maximum character limit on this value, but it does count against the message body text limit of 1024 characters. | `&#123; &quot;type&quot;:&quot;text&quot;, &quot;text&quot;: &quot;Nidhi&quot; &#125;`&lt;br&gt; |
| `&lt;MESSAGE_HEADER_ASSET_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Header asset&#039;s uploaded media asset ID. Use the [**POST /&lt;BUSINESS_PHONE_NUMBER_ID&gt;/media**](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-phone-numbers/media#upload-media) endpoint to generate an asset ID. | `1558081531584829` |
| `&lt;MESSAGE_HEADER_FORMAT&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Indicates header type and a matching property name.&lt;br&gt;&lt;br&gt;Note that the `&lt;MESSAGE_HEADER_FORMAT&gt;` placeholder appears twice in the post body example above, as it serves as a placeholder for the type property&#039;s value and its matching property name.&lt;br&gt;&lt;br&gt;&lt;br&gt;Value can be `image` or `video`. | `image` |
| `&lt;PAYMENT_GATEWAY_CONFIGURATION_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Configuration name of payment gateway you have [configured](https://developers.facebook.com/documentation/business-messaging/whatsapp/payments/payments-in/onboarding-apis) on your WhatsApp Business Account. | `prod-razor-pay-config-05` |
| `&lt;PAYMENT_GATEWAY_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Name of payment gateway you have [configured](https://developers.facebook.com/documentation/business-messaging/whatsapp/payments/payments-in/onboarding-apis) on your WhatsApp Business Account.&lt;br&gt;&lt;br&gt;&lt;br&gt;Values can be:&lt;br&gt;&lt;br&gt;* `razorpay`&lt;br&gt;* `payu`&lt;br&gt;* `zaakpay` | `razorpay` |
| `&lt;PRODUCT_TYPE&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Product type. Value can be  `digital-goods` or `physical-goods`. | `digital-goods` |
| `&lt;QUICK_REPLY_BUTTON_PAYLOAD&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Value to be included in messages webhooks (`messages.button.payload`) when the button is tapped. | `opt-out` |
| `&lt;REFERENCE_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Your unique order or invoice reference ID. Case-sensitive. Cannot be empty. Will be preceded by a hash (#) symbol in the checkout flow.&lt;br&gt;&lt;br&gt;Value must be unique for each checkout button template message. If sending a carousel template, each checkout button must have a unique reference ID.&lt;br&gt;&lt;br&gt;If you need to send multiple messages for the same order/invoice, it is recommended to append a sequence number to the value (for example, -1).&lt;br&gt;&lt;br&gt;Values can only contain English letters, numbers, underscores, dashes, or dots.&lt;br&gt;&lt;br&gt;Maximum 35 characters. | `abc.123_xyz-1` |
| `&lt;SALE_PRICE&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | **Required if using a sale amount.**&lt;br&gt;&lt;br&gt;Sale price, multiplied by `sale.offset` value.&lt;br&gt;&lt;br&gt;For example, to represent a sale price of ₹10, the value would be `1000`. | `150000` |
| `&lt;SHIPPING_AMOUNT&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | **Required.**&lt;br&gt;&lt;br&gt;Order shipping cost, multiplied by `shipping.offset` value.&lt;br&gt;&lt;br&gt;For example, to represent a shipping cost of ₹.99, the value would be `99`. | `20000` |
| `&lt;SHIPPING_INFO_ADDRESS&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required if you know the recipient&#039;s shipping information.**&lt;br&gt;&lt;br&gt;Product recipient&#039;s address.&lt;br&gt;&lt;br&gt;Maximum 512 characters. | `Bandra Kurla Complex` |
| `&lt;SHIPPING_INFO_BUILDING_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Product recipient&#039;s building name.&lt;br&gt;&lt;br&gt;Maximum 128 characters. | `One BKC` |
| `&lt;SHIPPING_INFO_CITY&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required if you know the recipient&#039;s shipping information.**&lt;br&gt;&lt;br&gt;Full name of product recipient&#039;s city.&lt;br&gt;&lt;br&gt;Maximum 100 characters. | `Mumbai` |
| `&lt;SHIPPING_INFO_FLOOR_NUMBER&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Product recipient&#039;s floor number.&lt;br&gt;&lt;br&gt;Maximum 10 characters. | `2` |
| `&lt;SHIPPING_INFO_HOUSE_NUMBER&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Product recipient&#039;s house number.&lt;br&gt;&lt;br&gt;Maximum 8 characters. | `12` |
| `&lt;SHIPPING_INFO_INDIA_PIN&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required if you know the recipient&#039;s shipping information.**&lt;br&gt;&lt;br&gt;Product recipient&#039;s postal index number.&lt;br&gt;&lt;br&gt;Maximum 6 characters. | `400051` |
| `&lt;SHIPPING_INFO_LANDMARK_AREA&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Product recipient&#039;s landmark area.&lt;br&gt;&lt;br&gt;Maximum 128 characters. | `Near BKC Circle` |
| `&lt;SHIPPING_INFO_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required if you know the recipient&#039;s shipping information.**&lt;br&gt;&lt;br&gt;Product recipient&#039;s full name.&lt;br&gt;&lt;br&gt;Maximum 256 characters. | `Nidhi Tripathi` |
| `&lt;SHIPPING_INFO_PHONE_NUMBER&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required if you know the recipient&#039;s shipping information.**&lt;br&gt;&lt;br&gt;Product recipient&#039;s WhatsApp phone number.&lt;br&gt;&lt;br&gt;Maximum 12 characters. | `919000090000` |
| `&lt;SHIPPING_INFO_STATE&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required if you know the recipient&#039;s shipping information.**&lt;br&gt;&lt;br&gt;Full name of product recipient&#039;s state.&lt;br&gt;&lt;br&gt;Maximum 100 characters. | `Maharastra` |
| `&lt;SHIPPING_INFO_TOWER_NUMBER&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Product recipient&#039;s tower number.&lt;br&gt;&lt;br&gt;Maximum 8 characters. | `2` |
| `&lt;SUBTOTAL_AMOUNT&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | **Required.**&lt;br&gt;&lt;br&gt;Order subtotal. Calculate by multiplying `&lt;ITEM_PRICE&gt;` by `&lt;ITEM_QUANTITY&gt;` by `subtotal.offset`.&lt;br&gt;&lt;br&gt;For example, if the template is for placing a single order containing 2 items priced at ₹12.99, the value would be `2598`. | `150000` |
| `&lt;TAX_AMOUNT&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | **Required.**&lt;br&gt;&lt;br&gt;Tax amount, multiplied by `tax.offset`.&lt;br&gt;&lt;br&gt;For example, to represent a tax amount of ₹5, the value would be `500`. | `10000` |
| `&lt;TAX_DESCRIPTION&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Tax description.&lt;br&gt;&lt;br&gt;Maximum 60 characters. | `Sales tax` |
| `&lt;TEMPLATE_LANGUAGE&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Template [language and locale code](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/supported-languages). | `en_US` |
| `&lt;TEMPLATE_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Template name.&lt;br&gt;&lt;br&gt;Maximum 512 characters. | `item_back_in_stock_v1` |
| `&lt;TOTAL_AMOUNT&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | **Required.**&lt;br&gt;&lt;br&gt;Total amount of order, multiplied by `total_amount.offset` value.&lt;br&gt;&lt;br&gt;For example, to represent a total amount of ₹18, value be `1800`.&lt;br&gt;&lt;br&gt;Must be a sum of:&lt;br&gt;&lt;br&gt;* `order.subtotal.value`&lt;br&gt;* `order.shipping.value`&lt;br&gt;* `order.tax.value`&lt;br&gt;&lt;br&gt;Minus:&lt;br&gt;&lt;br&gt;* `order.discount.value` | `165000` |
| `&lt;WHATSAPP_USER_PHONE_NUMBER&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;WhatsApp user phone number. | `+16505551234` |

### Example request

```curl
curl &#039;https://graph.facebook.com/v25.0/106540352242922/messages&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039; \
-d &#039;
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;recipient_type&quot;: &quot;individual&quot;,
  &quot;to&quot;: &quot;+16505551234&quot;,
  &quot;type&quot;: &quot;template&quot;,
  &quot;template&quot;: &#123;
    &quot;name&quot;: &quot;item_back_in_stock_v2&quot;,
    &quot;language&quot;: &#123;
      &quot;policy&quot;: &quot;deterministic&quot;,
      &quot;code&quot;: &quot;en_US&quot;
    &#125;,
    &quot;components&quot;: [
      &#123;
        &quot;type&quot;: &quot;header&quot;,
        &quot;parameters&quot;: [
          &#123;
            &quot;type&quot;: &quot;image&quot;,
            &quot;image&quot;: &#123;
              &quot;id&quot;: &quot;1558081531584829&quot;
            &#125;
          &#125;
        ]
      &#125;,
      &#123;
        &quot;type&quot;: &quot;body&quot;,
        &quot;parameters&quot;: [
          &#123;
            &quot;type&quot;: &quot;text&quot;,
            &quot;text&quot;: &quot;Nidhi&quot;
          &#125;,
          &#123;
            &quot;type&quot;: &quot;text&quot;,
            &quot;text&quot;: &quot;Blue Elf Aloe&quot;
          &#125;
        ]
      &#125;,
      &#123;
        &quot;type&quot;: &quot;button&quot;,
        &quot;sub_type&quot;: &quot;order_details&quot;,
        &quot;index&quot;: 0,
        &quot;parameters&quot;: [
          &#123;
            &quot;type&quot;: &quot;action&quot;,
            &quot;action&quot;: &#123;
              &quot;order_details&quot;: &#123;
                &quot;reference_id&quot;: &quot;abc.123_xyz-1&quot;,
                &quot;type&quot;: &quot;physical-goods&quot;,
                &quot;currency&quot;: &quot;INR&quot;,
                &quot;payment_settings&quot;: [
                  &#123;
                    &quot;type&quot;: &quot;payment_gateway&quot;,
                    &quot;payment_gateway&quot;: &#123;
                      &quot;type&quot;: &quot;razorpay&quot;,
                      &quot;configuration_name&quot;: &quot;prod-razor-pay-config-05&quot;
                    &#125;
                  &#125;
                ],
                &quot;shipping_info&quot;: &#123;
                  &quot;country&quot;: &quot;IN&quot;,
                  &quot;addresses&quot;: [
                    &#123;
                      &quot;name&quot;: &quot;Nidhi Tripathi&quot;,
                      &quot;phone_number&quot;: &quot;919000090000&quot;,
                      &quot;address&quot;: &quot;Bandra Kurla Complex&quot;,
                      &quot;city&quot;: &quot;Mumbai&quot;,
                      &quot;state&quot;: &quot;Maharastra&quot;,
                      &quot;in_pin_code&quot;: &quot;400051&quot;,
                      &quot;house_number&quot;: &quot;12&quot;,
                      &quot;tower_number&quot;: &quot;5&quot;,
                      &quot;building_name&quot;: &quot;One BKC&quot;,
                      &quot;landmark_area&quot;: &quot;Near BKC Circle&quot;
                    &#125;
                  ]
                &#125;,
                &quot;order&quot;: &#123;
                  &quot;items&quot;: [
                    &#123;
                      &quot;amount&quot;: &#123;
                        &quot;offset&quot;: 100,
                        &quot;value&quot;: 200000
                      &#125;,
                      &quot;sale_amount&quot;: &#123;
                        &quot;offset&quot;: 100,
                        &quot;value&quot;: 150000
                      &#125;,
                      &quot;name&quot;: &quot;Blue Elf Aloe&quot;,
                      &quot;quantity&quot;: 1,
                      &quot;country_of_origin&quot;: &quot;India&quot;,
                      &quot;importer_name&quot;: &quot;Lucky Shrub Imports and Exports&quot;,
                      &quot;importer_address&quot;: &#123;
                        &quot;address_line1&quot;: &quot;One BKC&quot;,
                        &quot;address_line2&quot;: &quot;Bandra Kurla Complex&quot;,
                        &quot;city&quot;: &quot;Mumbai&quot;,
                        &quot;zone_code&quot;: &quot;MH&quot;,
                        &quot;postal_code&quot;: &quot;400051&quot;,
                        &quot;country_code&quot;: &quot;IN&quot;
                      &#125;
                    &#125;
                  ],
                  &quot;subtotal&quot;: &#123;
                    &quot;offset&quot;: 100,
                    &quot;value&quot;: 150000
                  &#125;,
                  &quot;shipping&quot;: &#123;
                    &quot;offset&quot;: 100,
                    &quot;value&quot;: 20000
                  &#125;,
                  &quot;tax&quot;: &#123;
                    &quot;offset&quot;: 100,
                    &quot;value&quot;: 10000
                  &#125;,
                  &quot;discount&quot;: &#123;
                    &quot;offset&quot;: 100,
                    &quot;value&quot;: 15000,
                    &quot;description&quot;: &quot;Additional 10% off&quot;
                  &#125;,
                  &quot;status&quot;: &quot;pending&quot;,
                  &quot;expiration&quot;: &#123;
                    &quot;timestamp&quot;: &quot;1726627150&quot;
                  &#125;
                &#125;,
                &quot;total_amount&quot;: &#123;
                  &quot;offset&quot;: 100,
                  &quot;value&quot;: 165000
                &#125;
              &#125;
            &#125;
          &#125;
        ]
      &#125;
    ]
  &#125;
&#125;&#039;
```

**Warning:** The following sample request and responses are only supported with [Enabling coupons, realtime inventory and pricing updates](#enabling_coupons_inventory) feature and it is currently in beta and only available to India businesses and WhatsApp users with an India country calling code. Please reach out to whatsappindia-bizpayments-support&#064;meta.com to know more.

### Get coupons - endpoint sample request &#123;#get_coupon_request&#125;

```json
      &#123;
    &quot;data&quot;:
    &#123;
        &quot;order_details&quot;:
        &#123;
            &quot;reference_id&quot;: &quot;abc.123_xyz-1&quot;,
            &quot;type&quot;: &quot;physical-goods&quot;,
            &quot;currency&quot;: &quot;INR&quot;,
            &quot;shipping_info&quot;:
            &#123;
                &quot;country&quot;: &quot;IN&quot;,
                &quot;addresses&quot;:
                [
                    &#123;
                        &quot;name&quot;: &quot;Nidhi Tripathi&quot;,
                        &quot;phone_number&quot;: &quot;919000090000&quot;,
                        &quot;address&quot;: &quot;Bandra Kurla Complex&quot;,
                        &quot;city&quot;: &quot;Mumbai&quot;,
                        &quot;state&quot;: &quot;Maharastra&quot;,
                        &quot;in_pin_code&quot;: &quot;400051&quot;,
                        &quot;house_number&quot;: &quot;12&quot;,
                        &quot;tower_number&quot;: &quot;5&quot;,
                        &quot;building_name&quot;: &quot;One BKC&quot;,
                        &quot;landmark_area&quot;: &quot;Near BKC Circle&quot;
                    &#125;
                ]
            &#125;,
            &quot;order&quot;:
            &#123;
                &quot;items&quot;:
                [
                    &#123;
                        &quot;amount&quot;:
                        &#123;
                            &quot;offset&quot;: 100,
                            &quot;value&quot;: 200000
                        &#125;,
                        &quot;sale_amount&quot;:
                        &#123;
                            &quot;offset&quot;: 100,
                            &quot;value&quot;: 150000
                        &#125;,
                        &quot;name&quot;: &quot;Blue Elf Aloe&quot;,
                        &quot;quantity&quot;: 1,
                        &quot;country_of_origin&quot;: &quot;India&quot;,
                        &quot;importer_name&quot;: &quot;Lucky Shrub Imports and Exports&quot;,
                        &quot;importer_address&quot;:
                        &#123;
                            &quot;address_line1&quot;: &quot;One BKC&quot;,
                            &quot;address_line2&quot;: &quot;Bandra Kurla Complex&quot;,
                            &quot;city&quot;: &quot;Mumbai&quot;,
                            &quot;zone_code&quot;: &quot;MH&quot;,
                            &quot;postal_code&quot;: &quot;400051&quot;,
                            &quot;country_code&quot;: &quot;IN&quot;
                        &#125;
                    &#125;
                ],
                &quot;subtotal&quot;:
                &#123;
                    &quot;offset&quot;: 100,
                    &quot;value&quot;: 150000
                &#125;,
                &quot;shipping&quot;:
                &#123;
                    &quot;offset&quot;: 100,
                    &quot;value&quot;: 20000
                &#125;,
                &quot;tax&quot;:
                &#123;
                    &quot;offset&quot;: 100,
                    &quot;value&quot;: 10000
                &#125;,
                &quot;discount&quot;:
                &#123;
                    &quot;offset&quot;: 100,
                    &quot;value&quot;: 15000,
                    &quot;description&quot;: &quot;Additional 10% off&quot;
                &#125;,
                &quot;status&quot;: &quot;pending&quot;,
                &quot;expiration&quot;:
                &#123;
                    &quot;timestamp&quot;: &quot;1726627150&quot;,
                    &quot;description&quot;: &quot;order expires in 5 min&quot;
                &#125;
            &#125;,
            &quot;total_amount&quot;:
            &#123;
                &quot;offset&quot;: 100,
                &quot;value&quot;: 165000
            &#125;
        &#125;,
        &quot;input&quot;:
        &#123;
            &quot;user_id&quot;: &quot;919000090000&quot;
        &#125;
    &#125;,
    &quot;action&quot;: &quot;data_exchange&quot;,
    &quot;sub_action&quot;: &quot;get_coupons&quot;,
    &quot;version&quot;: &quot;1.0&quot;
&#125;
```

### Get coupons - endpoint sample response &#123;#get_coupon_response&#125;

```json
      &#123;
    &quot;version&quot;: &quot;1.0&quot;,
    &quot;sub_action&quot;: &quot;get_coupons&quot;,
    &quot;data&quot;:
    &#123;
        &quot;coupons&quot;:
        [
            &#123;
                &quot;description&quot;: &quot;Save R20 on the order&quot;,
                &quot;code&quot;: &quot;TRYNEW20&quot;,
                &quot;id&quot;: &quot;try_new_20_id&quot;
            &#125;,
            &#123;
                &quot;description&quot;: &quot;Save R30 on the order&quot;,
                &quot;code&quot;: &quot;TRYNEW30&quot;,
                &quot;id&quot;: &quot;try_new_30_id&quot;
            &#125;,
            &#123;
                &quot;description&quot;: &quot;Save R50 on the order&quot;,
                &quot;code&quot;: &quot;TRYNEW50&quot;,
                &quot;id&quot;: &quot;try_new50_id&quot;
            &#125;
        ]
    &#125;
&#125;
```

### Apply coupon - endpoint sample request &#123;#apply_coupon_request&#125;

```json
      &#123;
    &quot;data&quot;:
    &#123;
        &quot;order_details&quot;:
        &#123;
            &quot;reference_id&quot;: &quot;abc.123_xyz-1&quot;,
            &quot;type&quot;: &quot;physical-goods&quot;,
            &quot;currency&quot;: &quot;INR&quot;,
            &quot;shipping_info&quot;:
            &#123;
                &quot;country&quot;: &quot;IN&quot;,
                &quot;addresses&quot;:
                [
                    &#123;
                        &quot;name&quot;: &quot;Nidhi Tripathi&quot;,
                        &quot;phone_number&quot;: &quot;919000090000&quot;,
                        &quot;address&quot;: &quot;Bandra Kurla Complex&quot;,
                        &quot;city&quot;: &quot;Mumbai&quot;,
                        &quot;state&quot;: &quot;Maharastra&quot;,
                        &quot;in_pin_code&quot;: &quot;400051&quot;,
                        &quot;house_number&quot;: &quot;12&quot;,
                        &quot;tower_number&quot;: &quot;5&quot;,
                        &quot;building_name&quot;: &quot;One BKC&quot;,
                        &quot;landmark_area&quot;: &quot;Near BKC Circle&quot;
                    &#125;
                ]
            &#125;,
            &quot;order&quot;:
            &#123;
                &quot;items&quot;:
                [
                    &#123;
                        &quot;amount&quot;:
                        &#123;
                            &quot;offset&quot;: 100,
                            &quot;value&quot;: 200000
                        &#125;,
                        &quot;sale_amount&quot;:
                        &#123;
                            &quot;offset&quot;: 100,
                            &quot;value&quot;: 150000
                        &#125;,
                        &quot;name&quot;: &quot;Blue Elf Aloe&quot;,
                        &quot;quantity&quot;: 1,
                        &quot;country_of_origin&quot;: &quot;India&quot;,
                        &quot;importer_name&quot;: &quot;Lucky Shrub Imports and Exports&quot;,
                        &quot;importer_address&quot;:
                        &#123;
                            &quot;address_line1&quot;: &quot;One BKC&quot;,
                            &quot;address_line2&quot;: &quot;Bandra Kurla Complex&quot;,
                            &quot;city&quot;: &quot;Mumbai&quot;,
                            &quot;zone_code&quot;: &quot;MH&quot;,
                            &quot;postal_code&quot;: &quot;400051&quot;,
                            &quot;country_code&quot;: &quot;IN&quot;
                        &#125;
                    &#125;
                ],
                &quot;subtotal&quot;:
                &#123;
                    &quot;offset&quot;: 100,
                    &quot;value&quot;: 150000
                &#125;,
                &quot;shipping&quot;:
                &#123;
                    &quot;offset&quot;: 100,
                    &quot;value&quot;: 20000
                &#125;,
                &quot;tax&quot;:
                &#123;
                    &quot;offset&quot;: 100,
                    &quot;value&quot;: 10000
                &#125;,
                &quot;discount&quot;:
                &#123;
                    &quot;offset&quot;: 100,
                    &quot;value&quot;: 15000,
                    &quot;description&quot;: &quot;Additional 10% off&quot;
                &#125;,
                &quot;status&quot;: &quot;pending&quot;,
                &quot;expiration&quot;:
                &#123;
                    &quot;timestamp&quot;: &quot;1726627150&quot;,
                    &quot;description&quot;: &quot;order expires in 5 min&quot;
                &#125;
            &#125;,
            &quot;total_amount&quot;:
            &#123;
                &quot;offset&quot;: 100,
                &quot;value&quot;: 165000
            &#125;
        &#125;,
        &quot;input&quot;:
        &#123;
            &quot;user_id&quot;: &quot;919000090000&quot;,
            &quot;coupon&quot;:
            &#123;
                &quot;code&quot;: &quot;TRYNEW10&quot;
            &#125;
        &#125;
    &#125;,
    &quot;action&quot;: &quot;data_exchange&quot;,
    &quot;sub_action&quot;: &quot;apply_coupon&quot;,
    &quot;version&quot;: &quot;1.0&quot;
&#125;
```

### Apply coupon - endpoint sample response &#123;#apply_coupon_response&#125;

```json
      &#123;
    &quot;sub_action&quot;: &quot;apply_coupon&quot;,
    &quot;version&quot;: &quot;1.0&quot;,
    &quot;data&quot;:
    &#123;
        &quot;order_details&quot;:
        &#123;
            &quot;reference_id&quot;: &quot;abc.123_xyz-1&quot;,
            &quot;type&quot;: &quot;physical-goods&quot;,
            &quot;currency&quot;: &quot;INR&quot;,
            &quot;shipping_info&quot;:
            &#123;
                &quot;country&quot;: &quot;IN&quot;,
                &quot;addresses&quot;:
                [
                    &#123;
                        &quot;name&quot;: &quot;Nidhi Tripathi&quot;,
                        &quot;phone_number&quot;: &quot;919000090000&quot;,
                        &quot;address&quot;: &quot;Bandra Kurla Complex&quot;,
                        &quot;city&quot;: &quot;Mumbai&quot;,
                        &quot;state&quot;: &quot;Maharastra&quot;,
                        &quot;in_pin_code&quot;: &quot;400051&quot;,
                        &quot;house_number&quot;: &quot;12&quot;,
                        &quot;tower_number&quot;: &quot;5&quot;,
                        &quot;building_name&quot;: &quot;One BKC&quot;,
                        &quot;landmark_area&quot;: &quot;Near BKC Circle&quot;
                    &#125;
                ]
            &#125;,
            &quot;order&quot;:
            &#123;
                &quot;items&quot;:
                [
                    &#123;
                        &quot;amount&quot;:
                        &#123;
                            &quot;offset&quot;: 100,
                            &quot;value&quot;: 200000
                        &#125;,
                        &quot;sale_amount&quot;:
                        &#123;
                            &quot;offset&quot;: 100,
                            &quot;value&quot;: 150000
                        &#125;,
                        &quot;name&quot;: &quot;Blue Elf Aloe&quot;,
                        &quot;quantity&quot;: 1,
                        &quot;country_of_origin&quot;: &quot;India&quot;,
                        &quot;importer_name&quot;: &quot;Lucky Shrub Imports and Exports&quot;,
                        &quot;importer_address&quot;:
                        &#123;
                            &quot;address_line1&quot;: &quot;One BKC&quot;,
                            &quot;address_line2&quot;: &quot;Bandra Kurla Complex&quot;,
                            &quot;city&quot;: &quot;Mumbai&quot;,
                            &quot;zone_code&quot;: &quot;MH&quot;,
                            &quot;postal_code&quot;: &quot;400051&quot;,
                            &quot;country_code&quot;: &quot;IN&quot;
                        &#125;
                    &#125;
                ],
                &quot;subtotal&quot;:
                &#123;
                    &quot;offset&quot;: 100,
                    &quot;value&quot;: 150000
                &#125;,
                &quot;shipping&quot;:
                &#123;
                    &quot;offset&quot;: 100,
                    &quot;value&quot;: 20000
                &#125;,
                &quot;tax&quot;:
                &#123;
                    &quot;offset&quot;: 100,
                    &quot;value&quot;: 10000
                &#125;,
                &quot;discount&quot;:
                &#123;
                    &quot;offset&quot;: 100,
                    &quot;value&quot;: 15000,
                    &quot;description&quot;: &quot;Additional 10% off&quot;
                &#125;,
                &quot;status&quot;: &quot;pending&quot;,
                &quot;expiration&quot;:
                &#123;
                    &quot;timestamp&quot;: &quot;1726627150&quot;,
                    &quot;description&quot;: &quot;order expires in 5 min&quot;
                &#125;
            &#125;,
            &quot;coupon&quot;:
            &#123;
                &quot;code&quot;: &quot;TRYNEW10&quot;,
                &quot;discount&quot;:
                &#123;
                    &quot;value&quot;: 16500,
                    &quot;offset&quot;: 100
                &#125;
            &#125;,
            &quot;total_amount&quot;:
            &#123;
                &quot;offset&quot;: 100,
                &quot;value&quot;: 148500
            &#125;
        &#125;
    &#125;
&#125;
```

### Remove coupon - endpoint sample request &#123;#remove_coupon_request&#125;

```json
      &#123;
    &quot;data&quot;:
    &#123;
        &quot;order_details&quot;:
        &#123;
            &quot;reference_id&quot;: &quot;abc.123_xyz-1&quot;,
            &quot;type&quot;: &quot;physical-goods&quot;,
            &quot;currency&quot;: &quot;INR&quot;,
            &quot;shipping_info&quot;:
            &#123;
                &quot;country&quot;: &quot;IN&quot;,
                &quot;addresses&quot;:
                [
                    &#123;
                        &quot;name&quot;: &quot;Nidhi Tripathi&quot;,
                        &quot;phone_number&quot;: &quot;919000090000&quot;,
                        &quot;address&quot;: &quot;Bandra Kurla Complex&quot;,
                        &quot;city&quot;: &quot;Mumbai&quot;,
                        &quot;state&quot;: &quot;Maharastra&quot;,
                        &quot;in_pin_code&quot;: &quot;400051&quot;,
                        &quot;house_number&quot;: &quot;12&quot;,
                        &quot;tower_number&quot;: &quot;5&quot;,
                        &quot;building_name&quot;: &quot;One BKC&quot;,
                        &quot;landmark_area&quot;: &quot;Near BKC Circle&quot;
                    &#125;
                ]
            &#125;,
            &quot;order&quot;:
            &#123;
                &quot;items&quot;:
                [
                    &#123;
                        &quot;amount&quot;:
                        &#123;
                            &quot;offset&quot;: 100,
                            &quot;value&quot;: 200000
                        &#125;,
                        &quot;sale_amount&quot;:
                        &#123;
                            &quot;offset&quot;: 100,
                            &quot;value&quot;: 150000
                        &#125;,
                        &quot;name&quot;: &quot;Blue Elf Aloe&quot;,
                        &quot;quantity&quot;: 1,
                        &quot;country_of_origin&quot;: &quot;India&quot;,
                        &quot;importer_name&quot;: &quot;Lucky Shrub Imports and Exports&quot;,
                        &quot;importer_address&quot;:
                        &#123;
                            &quot;address_line1&quot;: &quot;One BKC&quot;,
                            &quot;address_line2&quot;: &quot;Bandra Kurla Complex&quot;,
                            &quot;city&quot;: &quot;Mumbai&quot;,
                            &quot;zone_code&quot;: &quot;MH&quot;,
                            &quot;postal_code&quot;: &quot;400051&quot;,
                            &quot;country_code&quot;: &quot;IN&quot;
                        &#125;
                    &#125;
                ],
                &quot;subtotal&quot;:
                &#123;
                    &quot;offset&quot;: 100,
                    &quot;value&quot;: 150000
                &#125;,
                &quot;shipping&quot;:
                &#123;
                    &quot;offset&quot;: 100,
                    &quot;value&quot;: 20000
                &#125;,
                &quot;tax&quot;:
                &#123;
                    &quot;offset&quot;: 100,
                    &quot;value&quot;: 10000
                &#125;,
                &quot;discount&quot;:
                &#123;
                    &quot;offset&quot;: 100,
                    &quot;value&quot;: 15000,
                    &quot;description&quot;: &quot;Additional 10% off&quot;
                &#125;,
                &quot;status&quot;: &quot;pending&quot;,
                &quot;expiration&quot;:
                &#123;
                    &quot;timestamp&quot;: &quot;1726627150&quot;,
                    &quot;description&quot;: &quot;order expires in 5 min&quot;
                &#125;
            &#125;,
            &quot;coupon&quot;:
            &#123;
                &quot;code&quot;: &quot;TRYNEW10&quot;,
                &quot;discount&quot;:
                &#123;
                    &quot;value&quot;: 16500,
                    &quot;offset&quot;: 100
                &#125;
            &#125;,
            &quot;total_amount&quot;:
            &#123;
                &quot;offset&quot;: 100,
                &quot;value&quot;: 148500
            &#125;
        &#125;,
        &quot;input&quot;:
        &#123;
            &quot;user_id&quot;: &quot;919000090000&quot;
        &#125;
    &#125;,
    &quot;action&quot;: &quot;data_exchange&quot;,
    &quot;sub_action&quot;: &quot;remove_coupon&quot;,
    &quot;version&quot;: &quot;1.0&quot;
&#125;
```

### Remove coupon - endpoint sample response &#123;#remove_coupon_response&#125;

```json
      &#123;
    &quot;sub_action&quot;: &quot;remove_coupon&quot;,
    &quot;version&quot;: &quot;1.0&quot;,
    &quot;data&quot;:
    &#123;
        &quot;order_details&quot;:
        &#123;
            &quot;reference_id&quot;: &quot;abc.123_xyz-1&quot;,
            &quot;type&quot;: &quot;physical-goods&quot;,
            &quot;currency&quot;: &quot;INR&quot;,
            &quot;shipping_info&quot;:
            &#123;
                &quot;country&quot;: &quot;IN&quot;,
                &quot;addresses&quot;:
                [
                    &#123;
                        &quot;name&quot;: &quot;Nidhi Tripathi&quot;,
                        &quot;phone_number&quot;: &quot;919000090000&quot;,
                        &quot;address&quot;: &quot;Bandra Kurla Complex&quot;,
                        &quot;city&quot;: &quot;Mumbai&quot;,
                        &quot;state&quot;: &quot;Maharastra&quot;,
                        &quot;in_pin_code&quot;: &quot;400051&quot;,
                        &quot;house_number&quot;: &quot;12&quot;,
                        &quot;tower_number&quot;: &quot;5&quot;,
                        &quot;building_name&quot;: &quot;One BKC&quot;,
                        &quot;landmark_area&quot;: &quot;Near BKC Circle&quot;
                    &#125;
                ]
            &#125;,
            &quot;order&quot;:
            &#123;
                &quot;items&quot;:
                [
                    &#123;
                        &quot;amount&quot;:
                        &#123;
                            &quot;offset&quot;: 100,
                            &quot;value&quot;: 200000
                        &#125;,
                        &quot;sale_amount&quot;:
                        &#123;
                            &quot;offset&quot;: 100,
                            &quot;value&quot;: 150000
                        &#125;,
                        &quot;name&quot;: &quot;Blue Elf Aloe&quot;,
                        &quot;quantity&quot;: 1,
                        &quot;country_of_origin&quot;: &quot;India&quot;,
                        &quot;importer_name&quot;: &quot;Lucky Shrub Imports and Exports&quot;,
                        &quot;importer_address&quot;:
                        &#123;
                            &quot;address_line1&quot;: &quot;One BKC&quot;,
                            &quot;address_line2&quot;: &quot;Bandra Kurla Complex&quot;,
                            &quot;city&quot;: &quot;Mumbai&quot;,
                            &quot;zone_code&quot;: &quot;MH&quot;,
                            &quot;postal_code&quot;: &quot;400051&quot;,
                            &quot;country_code&quot;: &quot;IN&quot;
                        &#125;
                    &#125;
                ],
                &quot;subtotal&quot;:
                &#123;
                    &quot;offset&quot;: 100,
                    &quot;value&quot;: 150000
                &#125;,
                &quot;shipping&quot;:
                &#123;
                    &quot;offset&quot;: 100,
                    &quot;value&quot;: 20000
                &#125;,
                &quot;tax&quot;:
                &#123;
                    &quot;offset&quot;: 100,
                    &quot;value&quot;: 10000
                &#125;,
                &quot;discount&quot;:
                &#123;
                    &quot;offset&quot;: 100,
                    &quot;value&quot;: 15000,
                    &quot;description&quot;: &quot;Additional 10% off&quot;
                &#125;,
                &quot;status&quot;: &quot;pending&quot;,
                &quot;expiration&quot;:
                &#123;
                    &quot;timestamp&quot;: &quot;1726627150&quot;,
                    &quot;description&quot;: &quot;order expires in 5 min&quot;
                &#125;
            &#125;,
            &quot;total_amount&quot;:
            &#123;
                &quot;offset&quot;: 100,
                &quot;value&quot;: 165000
            &#125;
        &#125;
    &#125;
&#125;
```

### Apply shipping - endpoint sample request &#123;#apply_shipping_request&#125;

```json
      &#123;
    &quot;data&quot;:
    &#123;
        &quot;order_details&quot;:
        &#123;
            &quot;reference_id&quot;: &quot;abc.123_xyz-1&quot;,
            &quot;type&quot;: &quot;physical-goods&quot;,
            &quot;currency&quot;: &quot;INR&quot;,
            &quot;shipping_info&quot;:
            &#123;
                &quot;country&quot;: &quot;IN&quot;,
                &quot;addresses&quot;:
                [
                    &#123;
                        &quot;name&quot;: &quot;Nidhi Tripathi&quot;,
                        &quot;phone_number&quot;: &quot;919000090000&quot;,
                        &quot;address&quot;: &quot;Bandra Kurla Complex&quot;,
                        &quot;city&quot;: &quot;Mumbai&quot;,
                        &quot;state&quot;: &quot;Maharastra&quot;,
                        &quot;in_pin_code&quot;: &quot;400051&quot;,
                        &quot;house_number&quot;: &quot;12&quot;,
                        &quot;tower_number&quot;: &quot;5&quot;,
                        &quot;building_name&quot;: &quot;One BKC&quot;,
                        &quot;landmark_area&quot;: &quot;Near BKC Circle&quot;
                    &#125;
                ]
            &#125;,
            &quot;order&quot;:
            &#123;
                &quot;items&quot;:
                [
                    &#123;
                        &quot;amount&quot;:
                        &#123;
                            &quot;offset&quot;: 100,
                            &quot;value&quot;: 200000
                        &#125;,
                        &quot;sale_amount&quot;:
                        &#123;
                            &quot;offset&quot;: 100,
                            &quot;value&quot;: 150000
                        &#125;,
                        &quot;name&quot;: &quot;Blue Elf Aloe&quot;,
                        &quot;quantity&quot;: 1,
                        &quot;country_of_origin&quot;: &quot;India&quot;,
                        &quot;importer_name&quot;: &quot;Lucky Shrub Imports and Exports&quot;,
                        &quot;importer_address&quot;:
                        &#123;
                            &quot;address_line1&quot;: &quot;One BKC&quot;,
                            &quot;address_line2&quot;: &quot;Bandra Kurla Complex&quot;,
                            &quot;city&quot;: &quot;Mumbai&quot;,
                            &quot;zone_code&quot;: &quot;MH&quot;,
                            &quot;postal_code&quot;: &quot;400051&quot;,
                            &quot;country_code&quot;: &quot;IN&quot;
                        &#125;
                    &#125;
                ],
                &quot;subtotal&quot;:
                &#123;
                    &quot;offset&quot;: 100,
                    &quot;value&quot;: 150000
                &#125;,
                &quot;shipping&quot;:
                &#123;
                    &quot;offset&quot;: 100,
                    &quot;value&quot;: 20000
                &#125;,
                &quot;tax&quot;:
                &#123;
                    &quot;offset&quot;: 100,
                    &quot;value&quot;: 10000
                &#125;,
                &quot;discount&quot;:
                &#123;
                    &quot;offset&quot;: 100,
                    &quot;value&quot;: 15000,
                    &quot;description&quot;: &quot;Additional 10% off&quot;
                &#125;,
                &quot;status&quot;: &quot;pending&quot;,
                &quot;expiration&quot;:
                &#123;
                    &quot;timestamp&quot;: &quot;1726627150&quot;,
                    &quot;description&quot;: &quot;order expires in 5 min&quot;
                &#125;
            &#125;,
            &quot;coupon&quot;:
            &#123;
                &quot;code&quot;: &quot;TRYNEW10&quot;,
                &quot;discount&quot;:
                &#123;
                    &quot;value&quot;: 16500,
                    &quot;offset&quot;: 100
                &#125;
            &#125;,
            &quot;total_amount&quot;:
            &#123;
                &quot;offset&quot;: 100,
                &quot;value&quot;: 148500
            &#125;
        &#125;,
        &quot;input&quot;:
        &#123;
            &quot;user_id&quot;: &quot;919000090000&quot;,
            &quot;selected_address&quot;:
            &#123;
                &quot;name&quot;: &quot;Nidhi Tripathi&quot;,
                &quot;phone_number&quot;: &quot;919000090000&quot;,
                &quot;address&quot;: &quot;Bandra Kurla Complex&quot;,
                &quot;city&quot;: &quot;Mumbai&quot;,
                &quot;state&quot;: &quot;Maharastra&quot;,
                &quot;in_pin_code&quot;: &quot;400051&quot;,
                &quot;house_number&quot;: &quot;12&quot;,
                &quot;tower_number&quot;: &quot;5&quot;,
                &quot;building_name&quot;: &quot;One BKC&quot;,
                &quot;landmark_area&quot;: &quot;Near BKC Circle&quot;
            &#125;
        &#125;
    &#125;,
    &quot;action&quot;: &quot;data_exchange&quot;,
    &quot;sub_action&quot;: &quot;apply_shipping&quot;,
    &quot;version&quot;: &quot;1.0&quot;
&#125;
```

### Apply shipping - endpoint sample response &#123;#apply_shipping_response&#125;

```json
      &#123;
    &quot;sub_action&quot;: &quot;apply_shipping&quot;,
    &quot;version&quot;: &quot;1.0&quot;,
    &quot;data&quot;:
    &#123;
        &quot;order_details&quot;:
        &#123;
            &quot;reference_id&quot;: &quot;abc.123_xyz-1&quot;,
            &quot;type&quot;: &quot;physical-goods&quot;,
            &quot;currency&quot;: &quot;INR&quot;,
            &quot;shipping_info&quot;:
            &#123;
                &quot;country&quot;: &quot;IN&quot;,
                &quot;addresses&quot;:
                [
                    &#123;
                        &quot;name&quot;: &quot;Nidhi Tripathi&quot;,
                        &quot;phone_number&quot;: &quot;919000090000&quot;,
                        &quot;address&quot;: &quot;Bandra Kurla Complex&quot;,
                        &quot;city&quot;: &quot;Mumbai&quot;,
                        &quot;state&quot;: &quot;Maharastra&quot;,
                        &quot;in_pin_code&quot;: &quot;400051&quot;,
                        &quot;house_number&quot;: &quot;12&quot;,
                        &quot;tower_number&quot;: &quot;5&quot;,
                        &quot;building_name&quot;: &quot;One BKC&quot;,
                        &quot;landmark_area&quot;: &quot;Near BKC Circle&quot;
                    &#125;
                ],
                &quot;selected_address&quot;:
                &#123;
                    &quot;name&quot;: &quot;Nidhi Tripathi&quot;,
                    &quot;phone_number&quot;: &quot;919000090000&quot;,
                    &quot;address&quot;: &quot;Bandra Kurla Complex&quot;,
                    &quot;city&quot;: &quot;Mumbai&quot;,
                    &quot;state&quot;: &quot;Maharastra&quot;,
                    &quot;in_pin_code&quot;: &quot;400051&quot;,
                    &quot;house_number&quot;: &quot;12&quot;,
                    &quot;tower_number&quot;: &quot;5&quot;,
                    &quot;building_name&quot;: &quot;One BKC&quot;,
                    &quot;landmark_area&quot;: &quot;Near BKC Circle&quot;
                &#125;
            &#125;,
            &quot;order&quot;:
            &#123;
                &quot;items&quot;:
                [
                    &#123;
                        &quot;amount&quot;:
                        &#123;
                            &quot;offset&quot;: 100,
                            &quot;value&quot;: 200000
                        &#125;,
                        &quot;sale_amount&quot;:
                        &#123;
                            &quot;offset&quot;: 100,
                            &quot;value&quot;: 150000
                        &#125;,
                        &quot;name&quot;: &quot;Blue Elf Aloe&quot;,
                        &quot;quantity&quot;: 1,
                        &quot;country_of_origin&quot;: &quot;India&quot;,
                        &quot;importer_name&quot;: &quot;Lucky Shrub Imports and Exports&quot;,
                        &quot;importer_address&quot;:
                        &#123;
                            &quot;address_line1&quot;: &quot;One BKC&quot;,
                            &quot;address_line2&quot;: &quot;Bandra Kurla Complex&quot;,
                            &quot;city&quot;: &quot;Mumbai&quot;,
                            &quot;zone_code&quot;: &quot;MH&quot;,
                            &quot;postal_code&quot;: &quot;400051&quot;,
                            &quot;country_code&quot;: &quot;IN&quot;
                        &#125;
                    &#125;
                ],
                &quot;subtotal&quot;:
                &#123;
                    &quot;offset&quot;: 100,
                    &quot;value&quot;: 150000
                &#125;,
                &quot;shipping&quot;:
                &#123;
                    &quot;offset&quot;: 100,
                    &quot;value&quot;: 40000
                &#125;,
                &quot;tax&quot;:
                &#123;
                    &quot;offset&quot;: 100,
                    &quot;value&quot;: 10000
                &#125;,
                &quot;discount&quot;:
                &#123;
                    &quot;offset&quot;: 100,
                    &quot;value&quot;: 15000,
                    &quot;description&quot;: &quot;Additional 10% off&quot;
                &#125;,
                &quot;status&quot;: &quot;pending&quot;,
                &quot;expiration&quot;:
                &#123;
                    &quot;timestamp&quot;: &quot;1726627150&quot;,
                    &quot;description&quot;: &quot;order expires in 5 min&quot;
                &#125;
            &#125;,
            &quot;coupon&quot;:
            &#123;
                &quot;code&quot;: &quot;TRYNEW10&quot;,
                &quot;discount&quot;:
                &#123;
                    &quot;value&quot;: 16500,
                    &quot;offset&quot;: 100
                &#125;
            &#125;,
            &quot;total_amount&quot;:
            &#123;
                &quot;offset&quot;: 100,
                &quot;value&quot;: 168500
            &#125;
        &#125;
    &#125;
&#125;
```
