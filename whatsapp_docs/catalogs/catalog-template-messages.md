# Catalog templates



This document explains how to create catalog templates. See [Sell Products and Services](https://developers.facebook.com/documentation/business-messaging/whatsapp/catalogs/catalogs-overview) to learn more about product catalogs and ways to showcase your products.

Catalog templates are marketing templates that allow you to showcase your product catalog entirely within WhatsApp. Catalog templates display a product thumbnail header image of your choice and custom body text, along with a fixed text header and fixed text sub-header.

When a customer taps the **View catalog** button in a catalog template message, your product catalog appears within WhatsApp.

## Creating catalog templates

### Requirements

You must have [inventory uploaded to Meta](https://developers.facebook.com/documentation/business-messaging/whatsapp/catalogs/upload-inventory) in an e-commerce catalog [connected to your WhatsApp Business account](https://www.facebook.com/business/help/158662536425974).

### Request syntax

Use the [Message Templates API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/message-template-api#post-version-waba-id-message-templates) to create a catalog template. Once your template is approved, you can use the [Messages API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/message-api#post-version-phone-number-id-messages) to send it in a template message.

```html
curl -X POST &quot;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;/message_templates&quot; \
  -H &quot;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&quot; \
  -H &quot;Content-Type: application/json&quot; \
  -d &#039;
&#123;
    &quot;name&quot;: &quot;&lt;NAME&gt;&quot;,
    &quot;language&quot;: &quot;&lt;LANGUAGE&gt;&quot;,
    &quot;category&quot;: &quot;MARKETING&quot;,
    &quot;components&quot;: [
      &#123;
        &quot;type&quot;: &quot;BODY&quot;,
        &quot;text&quot;: &quot;&lt;BODY_TEXT&gt;&quot;,
        &quot;example&quot;: &#123;
          &quot;body_text&quot;: [
            [
              &quot;&lt;EXAMPLE_BODY_TEXT&gt;&quot;
            ]
          ]
        &#125;
      &#125;,
      &#123;
        &quot;type&quot;: &quot;FOOTER&quot;,
        &quot;text&quot;: &quot;&lt;FOOTER_TEXT&gt;&quot;
      &#125;,
      &#123;
        &quot;type&quot;: &quot;BUTTONS&quot;,
        &quot;buttons&quot;: [
          &#123;
            &quot;type&quot;: &quot;CATALOG&quot;,
            &quot;text&quot;: &quot;View catalog&quot;
          &#125;
        ]
      &#125;
    ]
  &#125;&#039;
```

### Request parameters

| Placeholder | Description | Sample value |
| --- | --- | --- |
| `&lt;BODY_TEXT&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Template body text. Variables are supported.&lt;br&gt;&lt;br&gt;Maximum 1024 characters. | `Now shop for your favorite products right here on WhatsApp! Get Rs &#123;&#123;1&#125;&#125; off on all orders above &#123;&#123;2&#125;&#125;Rs! Valid for your first &#123;&#123;3&#125;&#125; orders placed on WhatsApp!` |
| `&lt;EXAMPLE_BODY_TEXT&gt;`&lt;br&gt;&lt;br&gt;_String (of an array of strings)_ | **Required if body text uses variables.**&lt;br&gt;&lt;br&gt;Sample strings to replace variable placeholders in `&lt;BODY_TEXT&gt;` string.&lt;br&gt;&lt;br&gt;Maximum 1024 characters. | `100` |
| `&lt;FOOTER_TEXT&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Template footer text. Variables are supported.&lt;br&gt;&lt;br&gt;Maximum 60 characters. | `Best grocery deals on WhatsApp!` |
| `&lt;LANGUAGE&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Template [language and locale code](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/supported-languages). | `en_US` |
| `&lt;NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Template name.&lt;br&gt;&lt;br&gt;Maximum 512 characters. | `intro_catalog_offer` |

### Example request

```curl
curl &#039;https://graph.facebook.com/v17.0/102290129340398/message_templates&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039; \
-d &#039;
&#123;
  &quot;name&quot;: &quot;intro_catalog_offer&quot;,
  &quot;language&quot;: &quot;en_US&quot;,
  &quot;category&quot;: &quot;MARKETING&quot;,
  &quot;components&quot;: [
    &#123;
      &quot;type&quot;: &quot;BODY&quot;,
      &quot;text&quot;: &quot;Now shop for your favorite products right here on WhatsApp! Get Rs &#123;&#123;1&#125;&#125; off on all orders above &#123;&#123;2&#125;&#125;Rs! Valid for your first &#123;&#123;3&#125;&#125; orders placed on WhatsApp!&quot;,
      &quot;example&quot;: &#123;
        &quot;body_text&quot;: [
          [
            &quot;100&quot;,
            &quot;400&quot;,
            &quot;3&quot;
          ]
        ]
      &#125;
    &#125;,
    &#123;
      &quot;type&quot;: &quot;FOOTER&quot;,
      &quot;text&quot;: &quot;Best grocery deals on WhatsApp!&quot;
    &#125;,
    &#123;
      &quot;type&quot;: &quot;BUTTONS&quot;,
      &quot;buttons&quot;: [
        &#123;
          &quot;type&quot;: &quot;CATALOG&quot;,
          &quot;text&quot;: &quot;View catalog&quot;
        &#125;
      ]
    &#125;
  ]
&#125;&#039;
```

### Example response

```json
&#123;
  &quot;id&quot;: &quot;546151681022936&quot;,
  &quot;status&quot;: &quot;PENDING&quot;,
  &quot;category&quot;: &quot;MARKETING&quot;
&#125;
```

## Sending catalog template messages

You can send approved [catalog templates](https://developers.facebook.com/documentation/business-messaging/whatsapp/catalogs/catalog-template-messages) in a template message. See [Sell Products and Services](https://developers.facebook.com/documentation/business-messaging/whatsapp/catalogs/catalogs-overview) to learn more about product catalogs and ways to showcase your products.

### Requirements

You must have [inventory uploaded to Meta](https://developers.facebook.com/documentation/business-messaging/whatsapp/catalogs/upload-inventory) in an e-commerce catalog [connected to your WhatsApp Business account](https://www.facebook.com/business/help/158662536425974).

### Request syntax

Use the [Messages API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/message-api#post-version-phone-number-id-messages) to send a catalog template message using a catalog template with an `APPROVED` status.

```html
curl -X POST &quot;https://graph.facebook.com/v19.0/&lt;WHATSAPP_BUSINESS_PHONE_NUMBER_ID&gt;/messages&quot; \
  -H &quot;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&quot; \
  -H &quot;Content-Type: application/json&quot; \
  -d &#039;
&#123;
    &quot;messaging_product&quot;: &quot;whatsapp&quot;,
    &quot;recipient_type&quot;: &quot;individual&quot;,
    &quot;to&quot;: &quot;&lt;TO&gt;&quot;,
    &quot;type&quot;: &quot;template&quot;,
    &quot;template&quot;: &#123;
      &quot;name&quot;: &quot;&lt;NAME&gt;&quot;,
      &quot;language&quot;: &#123;
        &quot;code&quot;: &quot;&lt;CODE&gt;&quot;
      &#125;,
      &quot;components&quot;: [
        &#123;
          &quot;type&quot;: &quot;body&quot;,
          &quot;parameters&quot;: [
            &#123;
              &quot;type&quot;: &quot;&lt;TYPE&gt;&quot;,
              &quot;text&quot;: &quot;&lt;TEXT&gt;&quot;
            &#125;
          ]
        &#125;,
        &#123;
          &quot;type&quot;: &quot;button&quot;,
          &quot;sub_type&quot;: &quot;CATALOG&quot;,
          &quot;index&quot;: 0,
          &quot;parameters&quot;: [
            &#123;
              &quot;type&quot;: &quot;action&quot;,
              &quot;action&quot;: &#123;
                &quot;thumbnail_product_retailer_id&quot;: &quot;&lt;THUMBNAIL_PRODUCT_RETAILER_ID&gt;&quot;
              &#125;
            &#125;
          ]
        &#125;
      ]
    &#125;
  &#125;&#039;
```

### Request parameters

| Placeholder | Description | Sample value |
| --- | --- | --- |
| `&lt;CODE&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Template [language and locale code](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/supported-languages). | `en_US` |
| `&lt;NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Template name. | `intro_catalog_offer` |
| `&lt;THUMBNAIL_PRODUCT_RETAILER_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Item SKU number. Labeled as Content ID in the Commerce Manager.&lt;br&gt;&lt;br&gt;The thumbnail of this item will be used as the message&#039;s header image.&lt;br&gt;&lt;br&gt;If the `parameters` object is omitted, the product image of the first item in your catalog will be used. | `2lc20305pt` |
| `&lt;TEXT&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required if template uses variables.**&lt;br&gt;&lt;br&gt;Template variable. | `100` |
| `&lt;TO&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Customer phone number. | `+16505551234` |
| `&lt;TYPE&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required if template uses variables.**&lt;br&gt;&lt;br&gt;Template variable type. | `text` |

### Example request

```curl
curl &#039;https://graph.facebook.com/v17.0/106540352242922/messages&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039; \
-d &#039;
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;recipient_type&quot;: &quot;individual&quot;,
  &quot;to&quot;: &quot;+16505551234&quot;,
  &quot;type&quot;: &quot;template&quot;,
  &quot;template&quot;: &#123;
    &quot;name&quot;: &quot;intro_catalog_offer&quot;,
    &quot;language&quot;: &#123;
      &quot;code&quot;: &quot;en_US&quot;
    &#125;,
    &quot;components&quot;: [
      &#123;
        &quot;type&quot;: &quot;body&quot;,
        &quot;parameters&quot;: [
          &#123;
            &quot;type&quot;: &quot;text&quot;,
            &quot;text&quot;: &quot;100&quot;
          &#125;,
          &#123;
            &quot;type&quot;: &quot;text&quot;,
            &quot;text&quot;: &quot;400&quot;
          &#125;,
          &#123;
            &quot;type&quot;: &quot;text&quot;,
            &quot;text&quot;: &quot;3&quot;
          &#125;
        ]
      &#125;,
      &#123;
        &quot;type&quot;: &quot;button&quot;,
        &quot;sub_type&quot;: &quot;CATALOG&quot;,
        &quot;index&quot;: 0,
        &quot;parameters&quot;: [
          &#123;
            &quot;type&quot;: &quot;action&quot;,
            &quot;action&quot;: &#123;
              &quot;thumbnail_product_retailer_id&quot;: &quot;2lc20305pt&quot;
            &#125;
          &#125;
        ]
      &#125;
    ]
  &#125;
&#125;&#039;
```

### Example response

```json
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;contacts&quot;: [
    &#123;
      &quot;input&quot;: &quot;+16505551234&quot;,
      &quot;wa_id&quot;: &quot;16505551234&quot;
    &#125;
  ],
  &quot;messages&quot;: [
    &#123;
      &quot;id&quot;: &quot;wamid.HBgLMTY1MDM4Nzk0MzkVAgARGBI5RkEwM0EyODFEQzQ2NDYzQTMA&quot;
    &#125;
  ]
&#125;
```

## See also

* [Sell Products and Services](https://developers.facebook.com/documentation/business-messaging/whatsapp/catalogs/catalogs-overview)
* [Catalog Messages](https://developers.facebook.com/documentation/business-messaging/whatsapp/catalogs/share-products#catalog-messages)
