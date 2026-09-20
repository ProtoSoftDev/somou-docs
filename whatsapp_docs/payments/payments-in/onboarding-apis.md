# Onboarding APIs



To receive payments on WhatsApp, you must have a payment configuration linked to the corresponding WhatsApp Business account. Each payment configuration is associated with a unique name. As part of the order_details message, you can specify the payment configuration to use for a specific checkout.

Onboarding APIs allows you to programmatically perform certain operations:

* Get all payment configurations linked to a WhatsApp Business account.
* Get a specific payment configuration linked to a WhatsApp Business account.
* Create a payment configuration.
* Regenerate payment gateway OAuth link to link payment configuration to a payment gateway.
* Remove a payment configuration.

## Get all payment configurations

Get a list of payment configurations linked to the WhatsApp Business account.

### Request syntax

```html
GET /&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;/payment_configurations
```

### Sample request

```curl
curl &#039;https://graph.facebook.com/v16.0/102290129340398/payment_configurations&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039;
```

### Sample response

```json
&#123;
  &quot;data&quot;: [
    &#123;
      &quot;payment_configurations&quot;: [
    &#123;
      &quot;configuration_name&quot;: &quot;test-payment-configuration&quot;,
      &quot;merchant_category_code&quot;: &#123;
        &quot;code&quot;: &quot;0000&quot;,
            &quot;description&quot;: &quot;Test MCC Code&quot;
       &#125;,
           &quot;purpose_code&quot;: &#123;
         &quot;code&quot;: &quot;00&quot;,
         &quot;description&quot;: &quot;Test Purpose Code&quot;
        &#125;,
        &quot;status&quot;: &quot;Active&quot;,
         &quot;provider_mid&quot;: &quot;test-payment-gateway-mid&quot;,
        &quot;provider_name&quot;: &quot;RazorPay&quot;,
        &quot;created_timestamp&quot;: 1720203204,
        &quot;updated_timestamp&quot;: 1721088316
    &#125;,
          ....
  ]
     &#125;
  ]
&#125;
```

| Field | Description |
| --- | --- |
| `configuration_name`&lt;br&gt;&lt;br&gt;string | **Required.**&lt;br&gt;&lt;br&gt;The name of the payment configuration to be used in the Order Details message. |
| `merchant_category_code`&lt;br&gt;&lt;br&gt;object | **Required.**&lt;br&gt;&lt;br&gt;Merchant Category Code:&lt;br&gt;&lt;br&gt;`code` string&lt;br&gt;&lt;br&gt;- **Required.** Will be a valid MCC code.&lt;br&gt;&lt;br&gt;`description` string&lt;br&gt;&lt;br&gt;- **Required.** MCC code description. |
| `purpose_code`&lt;br&gt;&lt;br&gt;object | **Required.**&lt;br&gt;&lt;br&gt;Purpose Code:&lt;br&gt;&lt;br&gt;`code` string&lt;br&gt;&lt;br&gt;- **Required.** Will be a valid purpose code.&lt;br&gt;&lt;br&gt;`description` string&lt;br&gt;&lt;br&gt;- **Required.** Purpose code description. |
| `status`&lt;br&gt;&lt;br&gt;string | **Required.**&lt;br&gt;&lt;br&gt;Status of the payment configuration. Must be one of [&quot;Active&quot;, &quot;Needs_Connecting&quot;, &quot;Needs_Testing&quot;]. |
| `provider_mid`&lt;br&gt;&lt;br&gt;string | **Optional.**&lt;br&gt;&lt;br&gt;Payment Gateway Merchant Identification Number (MID). |
| `provider_name`&lt;br&gt;&lt;br&gt;string | **Optional.**&lt;br&gt;&lt;br&gt;Payment Gateway Name. Must be one of [&quot;razorpay&quot;, &quot;payu&quot;, &quot;zaakpay&quot;]. |
| `merchant_vpa`&lt;br&gt;&lt;br&gt;string | **Optional.**&lt;br&gt;&lt;br&gt;Merchant UPI handle. |
| `created_timestamp`&lt;br&gt;&lt;br&gt;integer | **Optional.**&lt;br&gt;&lt;br&gt;Time when payment configuration was created. |
| `updated_timestamp`&lt;br&gt;&lt;br&gt;integer | **Optional.**&lt;br&gt;&lt;br&gt;Time when payment configuration was last updated. |

## Get a specific payment configuration

Get a specific payment configuration linked to the WhatsApp Business account.

### Request syntax

```html
GET /&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;/payment_configuration/&lt;CONFIGURATION_NAME&gt;
```

### Sample request

```curl
curl &#039;https://graph.facebook.com/v16.0/102290129340398/payment_configuration/test-payment-configuration&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039;
```

### Sample response

```json
&#123;
  &quot;data&quot;: [
    &#123;
      &quot;configuration_name&quot;: &quot;test-payment-configuration&quot;,
      &quot;merchant_category_code&quot;: &#123;
        &quot;code&quot;: &quot;0000&quot;,
        &quot;description&quot;: &quot;Test MCC Code&quot;
      &#125;,
      &quot;purpose_code&quot;: &#123;
        &quot;code&quot;: &quot;00&quot;,
        &quot;description&quot;: &quot;Test Purpose Code&quot;
      &#125;,
      &quot;status&quot;: &quot;Active&quot;,
      &quot;provider_mid&quot;: &quot;test-payment-gateway-mid&quot;,
      &quot;provider_name&quot;: &quot;RazorPay&quot;,
      &quot;created_timestamp&quot;: 1720203204,
      &quot;updated_timestamp&quot;: 1721088316
     &#125;
  ]
&#125;
```

| Field | Description |
| --- | --- |
| `configuration_name`&lt;br&gt;&lt;br&gt;string | **Required.**&lt;br&gt;&lt;br&gt;The name of the payment configuration to be used in the Order Details message. |
| `merchant_category_code`&lt;br&gt;&lt;br&gt;object | **Required.**&lt;br&gt;&lt;br&gt;Merchant Category Code:&lt;br&gt;&lt;br&gt;`code` string&lt;br&gt;&lt;br&gt;- **Required.** Will be a valid MCC code.&lt;br&gt;&lt;br&gt;`description` string&lt;br&gt;&lt;br&gt;- **Required.** MCC code description. |
| `purpose_code`&lt;br&gt;&lt;br&gt;object | **Required.**&lt;br&gt;&lt;br&gt;Purpose Code:&lt;br&gt;&lt;br&gt;`code` string&lt;br&gt;&lt;br&gt;- **Required.** Will be a valid purpose code.&lt;br&gt;&lt;br&gt;`description` string&lt;br&gt;&lt;br&gt;- **Required.** Purpose code description. |
| `status`&lt;br&gt;&lt;br&gt;string | **Required.**&lt;br&gt;&lt;br&gt;Status of the payment configuration. Must be one of [&quot;Active&quot;, &quot;Needs_Connecting&quot;, &quot;Needs_Testing&quot;]. |
| `provider_mid`&lt;br&gt;&lt;br&gt;string | **Optional.**&lt;br&gt;&lt;br&gt;Payment Gateway Merchant Identification Number (MID). |
| `provider_name`&lt;br&gt;&lt;br&gt;string | **Optional.**&lt;br&gt;&lt;br&gt;Payment Gateway Name. Must be one of [&quot;razorpay&quot;, &quot;payu&quot;, &quot;zaakpay&quot;]. |
| `merchant_vpa`&lt;br&gt;&lt;br&gt;string | **Optional.**&lt;br&gt;&lt;br&gt;Merchant UPI handle. |
| `created_timestamp`&lt;br&gt;&lt;br&gt;integer | **Optional.**&lt;br&gt;&lt;br&gt;Time when payment configuration was created. |
| `updated_timestamp`&lt;br&gt;&lt;br&gt;integer | **Optional.**&lt;br&gt;&lt;br&gt;Time when payment configuration was last updated. |

## Create a payment configuration

Create a payment configuration.

### Request syntax

```html
POST /&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;/payment_configuration
```

### Sample request

#### Payment gateway type configuration

```curl
curl -X  POST \
&#039;https://graph.facebook.com/v15.0/102290129340398/payment_configuration&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039; \
-H &#039;Content-Type: application/json&#039; \
-d &#039;&#123;
       &quot;configuration_name&quot;: &quot;test-payment-configuration&quot;,
       &quot;purpose_code&quot;: &quot;00&quot;,
       &quot;merchant_category_code&quot;: &quot;0000&quot;,
       &quot;provider_name&quot;: &quot;razorpay&quot;,
       &quot;redirect_url&quot;: &quot;https://test-redirect-url.com&quot;
    &#125;&#039;
```

#### UPI VPA type configuration

```curl
curl -X  POST \
&#039;https://graph.facebook.com/v15.0/102290129340398/payment_configuration&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039; \
-H &#039;Content-Type: application/json&#039; \
-d &#039;&#123;
       &quot;configuration_name&quot;: &quot;test-payment-configuration&quot;,
       &quot;purpose_code&quot;: &quot;00&quot;,
       &quot;merchant_category_code&quot;: &quot;0000&quot;,
       &quot;provider_name&quot;: &quot;upi_vpa&quot;,
       &quot;merchant_vpa&quot;: &quot;test-upi-merchant-vpa&#064;test&quot;
    &#125;&#039;
```

| Field | Description |
| --- | --- |
| `configuration_name`&lt;br&gt;&lt;br&gt;string | **Required.**&lt;br&gt;&lt;br&gt;The name of the payment configuration to be used in the Order Details message. |
| `merchant_category_code`&lt;br&gt;&lt;br&gt;string | **Optional.**&lt;br&gt;&lt;br&gt;Merchant Category Code. |
| `purpose_code`&lt;br&gt;&lt;br&gt;object | **Optional.**&lt;br&gt;&lt;br&gt;Purpose Code. |
| `provider_name`&lt;br&gt;&lt;br&gt;string | **Required.**&lt;br&gt;&lt;br&gt;Provider name of the payment configuration. Must be one of [&quot;upi_vpa&quot;, &quot;razorpay&quot;, &quot;payu&quot;, &quot;zaakpay&quot;]. |
| `merchant_vpa`&lt;br&gt;&lt;br&gt;string | **Optional.**&lt;br&gt;&lt;br&gt;Merchant UPI handle. |
| `redirect_url`&lt;br&gt;&lt;br&gt;URI | **Optional.**&lt;br&gt;&lt;br&gt;The url which merchant will be redirected to after successfully linking a payment configuration. |

### Sample response

#### Payment gateway type configuration

```json
&#123;
  &quot;oauth_url&quot;: &quot;https://www.facebook.com/payment/onboarding/init/&quot;,
  &quot;expiration&quot;: 1721687287,
  &quot;success&quot;: true
&#125;
```

#### UPI VPA type configuration

```json
&#123;
  &quot;success&quot;: true
&#125;
```

| Field | Description |
| --- | --- |
| `oauth_url`&lt;br&gt;&lt;br&gt;string | **Optional.**&lt;br&gt;&lt;br&gt;OAuth url to be used to link payment configuration to the payment gateway |
| `expiration`&lt;br&gt;&lt;br&gt;integer | **Optional.**&lt;br&gt;&lt;br&gt;Expiration time of the OAuth url. |
| `success`&lt;br&gt;&lt;br&gt;boolean | **Required.**&lt;br&gt;&lt;br&gt;Boolean flag to denote if payment configuration creation was successful or not. |

## Link or update data endpoint

The following section explains how to link, update, and delete data endpoint to enable coupons, shipping address and real-time inventory offered by [Checkout Button Templates](https://developers.facebook.com/documentation/business-messaging/whatsapp/payments/payments-in/checkout-button-templates#enabling_coupons_inventory).

### Request syntax

```html
POST /&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;/payment_configuration/&lt;CONFIGURATION_NAME&gt;
```

### Sample request

#### Payment gateway type configuration

```curl
curl -X  POST \
&#039;https://graph.facebook.com/v15.0/102290129340398/payment_configuration/test-payment-configuration&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039; \
-H &#039;Content-Type: application/json&#039; \
-d &#039;&#123;
       &quot;data_endpoint_url&quot;: &quot;https://test-data-endpoint-url.com&quot;
    &#125;&#039;
```

| Field | Description |
| --- | --- |
| `data-endpoint-url`&lt;br&gt;&lt;br&gt;URI | **Optional.**&lt;br&gt;&lt;br&gt;The URL endpoint that the WhatsApp client sends a secure HTTPS request to for data exchange purposes in [Checkout Button Templates](https://developers.facebook.com/documentation/business-messaging/whatsapp/payments/payments-in/checkout-button-templates#enabling_coupons_inventory) offering. |

## Regenerate payment configuration OAuth link

Regenerate OAuth link of payment gateway type payment configuration.

### Request syntax

```html
POST /&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;/generate_payment_configuration_oauth_link
```

### Sample request

```curl
curl -X  POST \
&#039;https://graph.facebook.com/v15.0/102290129340398/generate_payment_configuration_oauth_link&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039; \
-H &#039;Content-Type: application/json&#039; \
-d &#039;&#123;
       &quot;configuration_name&quot;: &quot;test-payment-configuration&quot;,
       &quot;redirect_url&quot;: &quot;https://test-redirect-url.com&quot;
    &#125;&#039;
```

| Field | Description |
| --- | --- |
| `configuration_name`&lt;br&gt;&lt;br&gt;string | **Required.**&lt;br&gt;&lt;br&gt;The name of the payment configuration to be used in the Order Details message. |
| `redirect_url`&lt;br&gt;&lt;br&gt;URI | **Optional.**&lt;br&gt;&lt;br&gt;The url which merchant will be redirected to after successfully linking a payment configuration. |

### Sample response

```json
&#123;
  &quot;oauth_url&quot;: &quot;https://www.facebook.com/payment/onboarding/init/&quot;,
  &quot;expiration&quot;: 1721687287
&#125;
```

| Field | Description |
| --- | --- |
| `oauth_url`&lt;br&gt;&lt;br&gt;string | **Optional.**&lt;br&gt;&lt;br&gt;OAuth url to be used to link payment configuration to the payment gateway |
| `expiration`&lt;br&gt;&lt;br&gt;integer | **Optional.**&lt;br&gt;&lt;br&gt;Expiration time of the OAuth url. |

## Remove a payment configuration

Remove a payment configuration.

### Request syntax

```html
DELETE /&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;/payment_configuration
```

| Field | Description |
| --- | --- |
| `configuration_name`&lt;br&gt;&lt;br&gt;string | **Required.**&lt;br&gt;&lt;br&gt;The name of the payment configuration to be used in the Order Details message. |

### Sample request

```curl
curl -X  DELETE \
&#039;https://graph.facebook.com/v15.0/102290129340398/payment_configuration&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039; \
-H &#039;Content-Type: application/json&#039; \
-d &#039;&#123;
       &quot;configuration_name&quot;: &quot;test-payment-configuration&quot;
    &#125;&#039;
```

### Sample response

```json
&#123;
  &quot;success&quot;: true
&#125;
```

| Field | Description |
| --- | --- |
| `success`&lt;br&gt;&lt;br&gt;boolean | **Required.**&lt;br&gt;&lt;br&gt;Boolean flag to denote if payment configuration removal was successful or not. |

## Payment Configuration Webhook
Businesses receive updates via WhatsApp webhooks when the status of the payment configuration changes.

To receive webhook, Businesses must subscribe to &quot;payment_configuration_update&quot; event for their respective application.

Webhook contains the following fields:

| Field | Description |
| --- | --- |
| `configuration_name`&lt;br&gt;&lt;br&gt;string | **Required.**&lt;br&gt;&lt;br&gt;The name of the payment configuration to be used in the Order Details message. |
| `provider_name`&lt;br&gt;&lt;br&gt;string | **Required.**&lt;br&gt;&lt;br&gt;Provider name of the payment configuration. Must be one of [&quot;razorpay&quot;, &quot;payu&quot;, &quot;zaakpay&quot;]. |
| `provider_mid`&lt;br&gt;&lt;br&gt;string | **Required.**&lt;br&gt;&lt;br&gt;Payment gateway account merchant ID. |
| `status`&lt;br&gt;&lt;br&gt;string | **Required.**&lt;br&gt;&lt;br&gt;Status of the payment configuration. Must be one of [&quot;Active&quot;, &quot;Needs_Connecting&quot;, &quot;Needs_Testing&quot;]. |
| `created_timestamp`&lt;br&gt;&lt;br&gt;integer | **Required.**&lt;br&gt;&lt;br&gt;Time when payment configuration was created. |
| `updated_timestamp`&lt;br&gt;&lt;br&gt;integer | **Required.**&lt;br&gt;&lt;br&gt;Time when payment configuration was last updated. |

### Sample payment configuration webhook

```json
&#123;
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;0&quot;,
      &quot;time&quot;: 1725499886,
      &quot;changes&quot;: [
        &#123;
          &quot;field&quot;: &quot;payment_configuration_update&quot;,
          &quot;value&quot;: &#123;
            &quot;configuration_name&quot;: &quot;test-payment-configuration&quot;,
            &quot;provider_name&quot;: &quot;razorpay&quot;,
            &quot;provider_mid&quot;: &quot;test-provider-mid&quot;,
            &quot;status&quot;: &quot;Needs Testing&quot;,
            &quot;created_timestamp&quot;: 123457678,
            &quot;updated_timestamp&quot;: 123457678
          &#125;
        &#125;
      ]
    &#125;
  ],
  &quot;object&quot;: &quot;whatsapp_business_account&quot;
&#125;
```

### Errors

#### WhatsApp Payments terms of service acceptance pending
If you see the following error, accept the WhatsApp Payments terms of service using the link provided in the error message before trying again.

```json
&#123;
  &quot;error&quot;: &#123;
    &quot;message&quot;: &quot;(#131005) Access denied&quot;,
    &quot;type&quot;: &quot;OAuthException&quot;,
    &quot;code&quot;: 131005,
    &quot;error_data&quot;: &#123;
      &quot;messaging_product&quot;: &quot;whatsapp&quot;,
      &quot;details&quot;: &quot;WhatsApp Payments Terms of Service acceptance pending for this WhatsApp Business Account.
Please use the following link to accept terms of service before using Business APIs: https://fb.me/12345&quot;
    &#125;
  &#125;
&#125;
```

For all other errors that can be returned and guidance on how to handle them, see [WhatsApp Cloud API, Error Codes](https://developers.facebook.com/documentation/business-messaging/whatsapp/support/error-codes).
