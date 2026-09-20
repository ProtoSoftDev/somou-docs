# Single-product message templates



Single-product message (SPM) templates let you present a single product from your catalog. This guide describes their uses and how to use them.

SPM templates are marketing templates that allow you to present a single product from your ecommerce catalog, accompanied by a product image, product title, and product price (all pulled from your product within your catalog), along with customizable body text, optional footer text, and an interactive **View** button.

WhatsApp users can tap the button to see details about the product, and can add or remove the product from the WhatsApp shopping cart:

If the WhatsApp user adds the product to the cart and submits an order, you will be notified via webhook and the user will see that an order has been placed:

Users who place an order are also able to use the View details button to see information about the order:

## Limitations

* WhatsApp users must be using WhatsApp v2.22.24 or later.
* Message forwarding is disabled for SPM templates.

## Catalogs

You must have an ecommerce product catalog, with inventory, connected to your WhatsApp Business account. See the Cloud API [Commerce](https://developers.facebook.com/documentation/business-messaging/whatsapp/catalogs/catalogs-overview) guide to learn more about connecting a catalog to your account.

## Webhooks

When a WhatsApp user adds one or more products to their cart and submits an order, an [order messages](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/messages/order) webhook is triggered, describing the order.

## Creating SPM templates

Use the [Message Templates API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/message-template-api#post-version-waba-id-message-templates) to create an SPM template.

### Request syntax

```html
curl &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;/message_templates&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039; \
-d &#039;
&#123;
  &quot;name&quot;: &quot;&lt;TEMPLATE_NAME&gt;&quot;,
  &quot;language&quot;: &quot;&lt;TEMPLATE_LANGUAGE&gt;&quot;,
  &quot;category&quot;: &quot;marketing&quot;,
  &quot;parameter_format&quot;: &quot;&lt;PARAMETER_FORMAT&gt;&quot;,
  &quot;components&quot;: [
    &#123;
      &quot;type&quot;: &quot;header&quot;,
      &quot;format&quot;: &quot;product&quot;
    &#125;,
    &#123;
      &quot;type&quot;: &quot;body&quot;,
      &quot;text&quot;: &quot;&lt;CARD_BODY_TEXT&gt;&quot;,

      &lt;!-- Example parameter values required, if body text contains parameters --&gt;
      &quot;example&quot;: &#123;
        &quot;body_text_named_params&quot;: [
          &#123;
            &quot;param_name&quot;: &quot;&lt;PARAMETER_NAME&gt;&quot;,
            &quot;example&quot;: &quot;&lt;PARAMETER_EXAMPLE&gt;&quot;
          &#125;,
          &lt;!-- Additional parameters would follow --&gt;
        ]
      &#125;

    &#125;,
    &#123;
      &quot;type&quot;: &quot;footer&quot;,
      &quot;text&quot;: &quot;&lt;CARD_FOOTER_TEXT&gt;&quot;
    &#125;,
    &#123;
      &quot;type&quot;: &quot;buttons&quot;,
      &quot;buttons&quot;: [
        &#123;
          &quot;type&quot;: &quot;spm&quot;,
          &quot;text&quot;: &quot;View&quot;
        &#125;
      ]
    &#125;
  ]
&#125;&#039;
```

### Request parameters

| Placeholder | Description | Example value |
| --- | --- | --- |
| `&lt;ACCESS_TOKEN&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Access token. | `EAAAN...` |
| `&lt;API_VERSION&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;API version. If omitted, defaults to the newest API version available to your app. | `v23.0` |
| `&lt;CARD_BODY_TEXT&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Card body text. Supports variables.&lt;br&gt;&lt;br&gt;Maximum 160 characters. | `Use code &#123;&#123;1&#125;&#125; to get &#123;&#123;2&#125;&#125; off our newest succulent!` |
| `&lt;CARD_FOOTER_TEXT&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Footer text.&lt;br&gt;&lt;br&gt;Maximum 60 characters. | `September 30, 2024` |
| `&lt;PARAMETER_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required if body text uses parameters.**&lt;br&gt;&lt;br&gt;Example parameter value string(s). You must include a parameter example for each parameter in your body text. | `25OFF` |
| `&lt;PARAMETER_FORMAT&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;[Parameter format](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/overview#parameter-formats). Value can be:&lt;br&gt;&lt;br&gt;- `named`&lt;br&gt;- `positional`&lt;br&gt;&lt;br&gt;If the `parameter_format` property is omitted, the template will use positional formatting. | `Lucky Shrub: Your gateway to succulents!` |
| `&lt;TEMPLATE_LANGUAGE&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Template [language and locale code](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/supported-languages). | `en_US` |
| `&lt;TEMPLATE_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Template name.&lt;br&gt;&lt;br&gt;Maximum 512 characters. | `abandoned_cart_offer` |
| `&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;WhatsApp Business account ID. | `546151681022936` |

### Example request

```html
curl &#039;https://graph.facebook.com/v25.0/161311403722088/message_templates&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039; \
-d &#039;
&#123;
  &quot;name&quot;: &quot;spm_template_named_params&quot;,
  &quot;language&quot;: &quot;en_US&quot;,
  &quot;category&quot;: &quot;marketing&quot;,
  &quot;parameter_format&quot;: &quot;named&quot;,
  &quot;components&quot;: [
    &#123;
      &quot;type&quot;: &quot;header&quot;,
      &quot;format&quot;: &quot;product&quot;
    &#125;,
    &#123;
      &quot;type&quot;: &quot;body&quot;,
      &quot;text&quot;: &quot;Use code &#123;&#123;code&#125;&#125; to get &#123;&#123;percent&#125;&#125; off our newest succulent!&quot;,
      &quot;example&quot;: &#123;
        &quot;body_text_named_params&quot;: [
          &#123;
            &quot;param_name&quot;: &quot;code&quot;,
            &quot;example&quot;: &quot;15OFF&quot;
          &#125;,
          &#123;
            &quot;param_name&quot;: &quot;percent&quot;,
            &quot;example&quot;: &quot;15%&quot;
          &#125;
        ]
      &#125;
    &#125;,
    &#123;
      &quot;type&quot;: &quot;footer&quot;,
      &quot;text&quot;: &quot;Offer ends September 22, 2024&quot;
    &#125;,
    &#123;
      &quot;type&quot;: &quot;buttons&quot;,
      &quot;buttons&quot;: [
        &#123;
          &quot;type&quot;: &quot;spm&quot;,
          &quot;text&quot;: &quot;View&quot;
        &#125;
      ]
    &#125;
  ]
&#125;&#039;
```

## Sending single-product template messages

### Request syntax

Use the [Messages API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/message-api#post-version-phone-number-id-messages) to send an SPM template message.

```html
curl &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;BUSINESS_PHONE_NUMBER_ID&gt;/messages&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039; \
-d &#039;
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;recipient_type&quot;: &quot;individual&quot;,
  &quot;to&quot;: &quot;&lt;WHATSAPP_USER_PHONE_NUMBER&gt;&quot;,
  &quot;type&quot;: &quot;template&quot;,
  &quot;template&quot;: &#123;
    &quot;name&quot;: &quot;&lt;TEMPLATE_NAME&gt;&quot;,
    &quot;language&quot;: &#123;
      &quot;code&quot;: &quot;&lt;TEMPLATE_LANGUAGE&gt;&quot;
    &#125;,
    &quot;components&quot;: [
      &#123;
        &quot;type&quot;: &quot;header&quot;,
        &quot;parameters&quot;: [
          &#123;
            &quot;type&quot;: &quot;product&quot;,
            &quot;product&quot;: &#123;
              &quot;product_retailer_id&quot;: &quot;&lt;PRODUCT_ID&gt;&quot;,
              &quot;catalog_id&quot;: &quot;&lt;CATALOG_ID&gt;&quot;
            &#125;
          &#125;
        ]
      &#125;,
      &#123;
        &quot;type&quot;: &quot;body&quot;,
        &quot;parameters&quot;: [
          &#123;
            &quot;type&quot;: &quot;text&quot;,
            &quot;parameter_name&quot;: &quot;&lt;PARAMETER_NAME&gt;&quot;,
            &quot;text&quot;: &quot;&lt;PARAMETER_VALUE&gt;&quot;
          &#125;,
          &lt;!-- Additional parameter values would follow, if required by template --&gt;
        ]
      &#125;
    ]
  &#125;
&#125;&#039;
```

### Request parameters

| Placeholder | Description | Example value |
| --- | --- | --- |
| `&lt;ACCESS_TOKEN&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Access token | `EAAAN...` |
| `&lt;API_VERSION&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;API version. If omitted, defaults to the newest API version available to your app. | `v23.0` |
| `&lt;BUSINESS_PHONE_NUMBER_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;WhatsApp Business phone number ID. | `106540352242922` |
| `&lt;CATALOG_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;ID of [connected ecommerce catalog](https://www.facebook.com/business/help/158662536425974) containing the product. | `194836987003835` |
| `&lt;PARAMETER_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required if template uses one or more named parameters.**&lt;br&gt;&lt;br&gt;Name of [named parameter](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/overview#named-parameters). | `code` |
| `&lt;PARAMETER_VALUE&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required if template uses one or more named parameters.**&lt;br&gt;&lt;br&gt;[Named parameter](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/overview#named-parameters) value. | `10OFF` |
| `&lt;PRODUCT_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Product ID. | `nqryix03ez` |
| `&lt;TEMPLATE_LANGUAGE&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Template [language and locale code](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/supported-languages). | `en_US` |
| `&lt;TEMPLATE_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Template name.&lt;br&gt;&lt;br&gt;Maximum 512 characters. | `spm_template_named_params` |
| `&lt;WHATSAPP_USER_PHONE_NUMBER&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;WhatsApp user phone number. | `+16505551234` |

### Example request

This example sends an approved template named `spm_template_named_params` which injects parameters (a discount code and the percentage discounted) into the template body, and which includes a footer. The product image is pulled from the catalog and displayed in the message header.

```html
curl &#039;https://graph.facebook.com/v25.0/179776755229976/messages&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039; \
-d &#039;
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;recipient_type&quot;: &quot;individual&quot;,
  &quot;to&quot;: &quot;16505551234&quot;,
  &quot;type&quot;: &quot;template&quot;,
  &quot;template&quot;: &#123;
    &quot;name&quot;: &quot;spm_template_named_params&quot;,
    &quot;language&quot;: &#123;
      &quot;code&quot;: &quot;en_US&quot;
    &#125;,
    &quot;components&quot;: [
      &#123;
        &quot;type&quot;: &quot;header&quot;,
        &quot;parameters&quot;: [
          &#123;
            &quot;type&quot;: &quot;product&quot;,
            &quot;product&quot;: &#123;
              &quot;product_retailer_id&quot;: &quot;nqryix03ez&quot;,
              &quot;catalog_id&quot;: &quot;194836987003835&quot;
            &#125;
          &#125;
        ]
      &#125;,
      &#123;
        &quot;type&quot;: &quot;body&quot;,
        &quot;parameters&quot;: [
          &#123;
            &quot;type&quot;: &quot;text&quot;,
            &quot;parameter_name&quot;: &quot;code&quot;,
            &quot;text&quot;: &quot;25OFF&quot;
          &#125;,
          &#123;
            &quot;type&quot;: &quot;text&quot;,
            &quot;parameter_name&quot;: &quot;percent&quot;,
            &quot;text&quot;: &quot;25%&quot;
          &#125;
        ]
      &#125;
    ]
  &#125;
&#125;&#039;
```
