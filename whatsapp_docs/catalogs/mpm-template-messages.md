# Multi-product message templates



This document describes multi-product message (&quot;MPM&quot;) templates, their uses, and how to use them.

MPM templates are marketing templates that allow you to display up to 30 products from your ecommerce catalog, organized in up to 10 sections, in a single message.

WhatsApp users can browse products and sections within the message, view product details, add or remove products from their cart, and submit the cart to place an order. WhatsApp then sends you these orders via a webhook.

See our help center article [About Multi-product message templates on WhatsApp](https://www.facebook.com/business/help/978451836847222) for common use cases and tips on how to use MPM templates effectively.

## Requirements

In order to create and use MPM templates you must have an ecommerce product catalog, with inventory, connected to your WhatsApp Business account. See the Cloud API [Commerce](https://developers.facebook.com/documentation/business-messaging/whatsapp/catalogs/catalogs-overview) guide.

## Limitations

* Customers must use WhatsApp v2.22.24 or later.
* MPM templates cannot be forwarded to other customers.

## Creating MPM templates

You can create MPM templates using the [Message Templates API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/message-template-api#post-version-waba-id-message-templates) or the [**WhatsApp Manager**](https://business.facebook.com/wa/manage/home/) &gt; **Account tools** &gt; **Message templates** panel. Once your template is approved, you can use the [Messages API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/message-api#post-version-phone-number-id-messages) to send it in a template message.

### Request syntax

```html
curl -X POST &quot;https://graph.facebook.com/v23.0/&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;/message_templates&quot; \
  -H &quot;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&quot; \
  -H &quot;Content-Type: application/json&quot; \
  -d &#039;&#123;
    &quot;name&quot;: &quot;&lt;NAME&gt;&quot;,
    &quot;category&quot;: &quot;&lt;CATEGORY&gt;&quot;,
    &quot;language&quot;: &quot;&lt;LANGUAGE&gt;&quot;,
    &quot;components&quot;: [&lt;COMPONENTS&gt;]
  &#125;&#039;
```

### Request parameters

| Placeholder | Description | Sample Value |
| --- | --- | --- |
| `&lt;CATEGORY&gt;` | **Required.**&lt;br&gt;&lt;br&gt;Template category. Set this to `MARKETING`. | `MARKETING` |
| `&lt;COMPONENTS&gt;` | **Required.**&lt;br&gt;&lt;br&gt;Array of objects that describe the components that make up the template. See [Components](#components) below. | See [Components](#components) below. |
| `&lt;LANGUAGE&gt;` | **Required.**&lt;br&gt;&lt;br&gt;Template [language and locale code](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/supported-languages). | `en_US` |
| `&lt;NAME&gt;` | **Required.**&lt;br&gt;&lt;br&gt;Template name.&lt;br&gt;&lt;br&gt;Maximum 512 characters. | `abandoned_cart` |

### Components

The `components` value must be an array of objects that describes each component that makes up the template. MPM templates must have the following components:

* a single header component
* a single body component
* a single footer component (optional)
* a single MPM button component

```json
[
  &#123;
    &quot;type&quot;: &quot;HEADER&quot;,
    &quot;format&quot;: &quot;TEXT&quot;,
    &quot;text&quot;: &quot;&lt;HEADER_TEXT&gt;&quot;,

    /* Example required if header uses a variable */
    &quot;example&quot;: &#123;
      &quot;header_text&quot;: [
        &quot;&lt;HEADER_EXAMPLE_TEXT&gt;&quot;
      ]
    &#125;
  &#125;,
  &#123;
    &quot;type&quot;: &quot;BODY&quot;,
    &quot;text&quot;: &quot;&lt;BODY_TEXT&gt;&quot;,

    /* Example required if body uses variables */
​​    &quot;example&quot;: &#123;
      &quot;body_text&quot;: [
        [
          &quot;&lt;BODY_EXAMPLE_TEXT&gt;&quot;
        ]
      ]
    &#125;
  &#125;,
  &#123;
    &quot;type&quot;: &quot;FOOTER&quot;,
    &quot;text&quot;: &quot;&lt;FOOTER_TEXT&gt;&quot;
  &#125;,
  &#123;
    &quot;type&quot;:&quot;BUTTONS&quot;,
    &quot;buttons&quot;: [
      &#123;
        &quot;type&quot;: &quot;MPM&quot;,
        &quot;text&quot;: &quot;View items&quot;
      &#125;
    ]
  &#125;
]
```

### Request parameters

| Placeholder | Description | Sample Value |
| --- | --- | --- |
| `&lt;BODY_EXAMPLE_TEXT&gt;` | String or array of strings. Example body variable value(s). | `10OFF` |
| `&lt;BODY_TEXT&gt;` | Template body text. Supports multiple variables.&lt;br&gt;&lt;br&gt;If the string contains variables, you must include the example property and sample variable values.&lt;br&gt;&lt;br&gt;1024 characters maximum. | `Forget something, &#123;&#123;1&#125;&#125;?` |
| `&lt;FOOTER_TEXT&gt;` | Template footer text.&lt;br&gt;&lt;br&gt;60 characters maximum. | `Lucky Shrub, 1 Hacker Way, Menlo Park, CA 94025` |
| `&lt;HEADER_EXAMPLE_TEXT&gt;` | Example header variable value. | `Pablo` |
| `&lt;HEADER_TEXT&gt;` | Template header text. Supports 1 variable.&lt;br&gt;&lt;br&gt;If the string contains a variable, you must include the example property and a sample variable value.&lt;br&gt;&lt;br&gt;60 characters maximum. | `Looks like you left these items in your cart, still interested? Use code &#123;&#123;1&#125;&#125; to get 10% off!` |

### Response

Upon success, the API will respond with:

```json
&#123;
  &quot;id&quot;: &quot;&lt;ID&gt;&quot;,
  &quot;status&quot;: &quot;&lt;STATUS&gt;&quot;,
  &quot;category&quot;: &quot;MARKETING&quot;
&#125;
```

### Response parameters

| Placeholder | Description | Sample Value |
| --- | --- | --- |
| `&lt;ID&gt;` | Template ID. | `546151681022936` |
| `&lt;STATUS&gt;` | Template status. Only templates with an `APPROVED` status can be sent in a template message. | `PENDING` |

### Example request

```html
curl &#039;https://graph.facebook.com/v25.0/102290129340398/message_templates&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039; \
-d &#039;
&#123;
  &quot;name&quot;: &quot;abandoned_cart&quot;,
  &quot;language&quot;: &quot;en_US&quot;,
  &quot;category&quot;: &quot;MARKETING&quot;,
  &quot;components&quot;: [
    &#123;
      &quot;type&quot;: &quot;HEADER&quot;,
      &quot;format&quot;: &quot;TEXT&quot;,
      &quot;text&quot;: &quot;Forget something, &#123;&#123;1&#125;&#125;?&quot;,
      &quot;example&quot;: &#123;
        &quot;header_text&quot;: [
          &quot;Pablo&quot;
        ]
      &#125;
    &#125;,
    &#123;
      &quot;type&quot;: &quot;BODY&quot;,
      &quot;text&quot;: &quot;Looks like you left these items in your cart, still interested? Use code &#123;&#123;1&#125;&#125; to get 10% off!&quot;,
      &quot;example&quot;: &#123;
        &quot;body_text&quot;: [
          [
            &quot;10OFF&quot;
          ]
        ]
      &#125;
    &#125;,
    &#123;
      &quot;type&quot;:&quot;BUTTONS&quot;,
      &quot;buttons&quot;: [
        &#123;
          &quot;type&quot;: &quot;MPM&quot;,
          &quot;text&quot;: &quot;View items&quot;
        &#125;
      ]
    &#125;
  ]
&#125;&#039;
```

### Sample response

```json
&#123;
  &quot;id&quot;: &quot;546151681022936&quot;,
  &quot;status&quot;: &quot;PENDING&quot;,
  &quot;category&quot;: &quot;MARKETING&quot;
&#125;
```

## Webhooks

When a customer adds one or more products to their cart and submits an order, WhatsApp sends you a webhook describing the order.

### Webhook syntax

```json
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;&lt;ENTRY.ID&gt;&quot;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;messaging_product&quot;: &quot;whatsapp&quot;,
            &quot;metadata&quot;: &#123;
              &quot;display_phone_number&quot;: &quot;&lt;DISPLAY_PHONE_NUMBER&gt;&quot;,
              &quot;phone_number_id&quot;: &quot;&lt;PHONE_NUMBER_ID&gt;&quot;
            &#125;,
            &quot;contacts&quot;: [
              &#123;
                &quot;profile&quot;: &#123;
                  &quot;name&quot;: &quot;&lt;NAME&gt;&quot;
                &#125;,
                &quot;wa_id&quot;: &quot;&lt;WA_ID&gt;&quot;
              &#125;
            ],
            &quot;messages&quot;: [
              &#123;
                &quot;from&quot;: &quot;&lt;FROM&gt;&quot;,
                &quot;id&quot;: &quot;&lt;MESSAGES.ID&gt;&quot;,
                &quot;timestamp&quot;: &quot;&lt;TIMESTAMP&gt;&quot;,
                &quot;type&quot;: &quot;order&quot;,
                &quot;order&quot;: &#123;
                  &quot;catalog_id&quot;: &quot;&lt;CATALOG_ID&gt;&quot;,
                  &quot;product_items&quot;: [
                    &#123;
                      &quot;product_retailer_id&quot;: &quot;&lt;PRODUCT_RETAILER_ID&gt;&quot;,
                      &quot;quantity&quot;: &lt;QUANTITY&gt;,
                      &quot;item_price&quot;: &lt;ITEM_PRICE&gt;,
                      &quot;currency&quot;: &quot;&lt;CURRENCY&gt;&quot;
                    &#125;
                  ]
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

### Webhook contents

| Placeholder | Description | Sample Value |
| --- | --- | --- |
| `&lt;CATALOG_ID&gt;` | Ecommerce product catalog ID. | `1537566713439863` |
| `&lt;CURRENCY&gt;` | Item currency. | `USD` |
| `&lt;DISPLAY_PHONE_NUMBER&gt;` | Business phone number display number. | `15550051310` |
| `&lt;ENTRY.ID&gt;` | WhatsApp Business account ID. | `102290129340398` |
| `&lt;ITEM_PRICE&gt;` | Item price. | `99.99` |
| `&lt;MESSAGES.ID&gt;` | WhatsApp message ID. | `wamid.HBgLMTY1MDM4Nzk0MzkVAgARGBJDOEI3ODgxNzQzMjJBQTdEQTcA` |
| `&lt;NAME&gt;` | Customer&#039;s name. | `Pablo Morales` |
| `&lt;PHONE_NUMBER_ID&gt;` | Business phone number ID. | `106540352242922` |
| `&lt;PRODUCT_RETAILER_ID&gt;` | The item SKU number. Labeled as **Content ID** in the Commerce Manager. | `2lc20305pt` |
| `&lt;QUANTITY&gt;` | Number of items ordered (for this particular item). | `1` |
| `&lt;TIMESTAMP&gt;` | UNIX timestamp indicating when the webhook was sent. | `1677522117` |
| `&lt;WA_ID&gt;` | Customer&#039;s WhatsApp phone number. | `16505551234` |

### Sample webhook

```json
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;102290129340398&quot;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;messaging_product&quot;: &quot;whatsapp&quot;,
            &quot;metadata&quot;: &#123;
              &quot;display_phone_number&quot;: &quot;15550051310&quot;,
              &quot;phone_number_id&quot;: &quot;106540352242922&quot;
            &#125;,
            &quot;contacts&quot;: [
              &#123;
                &quot;profile&quot;: &#123;
                  &quot;name&quot;: &quot;Pablo Morales&quot;
                &#125;,
                &quot;wa_id&quot;: &quot;16505551234&quot;
              &#125;
            ],
            &quot;messages&quot;: [
              &#123;
                &quot;from&quot;: &quot;16505551234&quot;,
                &quot;id&quot;: &quot;wamid.HBgLMTY1MDM4Nzk0MzkVAgASGBQzQTMxNzA1QzNENEI4ODY0OTY2MAA=&quot;,
                &quot;timestamp&quot;: &quot;1683223069&quot;,
                &quot;type&quot;: &quot;order&quot;,
                &quot;order&quot;: &#123;
                  &quot;catalog_id&quot;: &quot;1537566713439863&quot;,
                  &quot;product_items&quot;: [
                    &#123;
                      &quot;product_retailer_id&quot;: &quot;n6k6x0y7oe&quot;,
                      &quot;quantity&quot;: 1,
                      &quot;item_price&quot;: 99.99,
                      &quot;currency&quot;: &quot;USD&quot;
                    &#125;
                  ]
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

## Sending MPM template messages

You can send [multi-product message (MPM) templates](https://developers.facebook.com/documentation/business-messaging/whatsapp/catalogs/mpm-template-messages) in template messages.

### Components

MPM template messages must have:

* a **header** component (only required if template uses a header variable)
* a **body** component (only required if template uses a body variable)
* a single **MPM button** component

Use the MPM button component to define sections and their titles that will appear when the customer taps the **View items** button, and to designate which products appear in each of those sections.

To send an approved MPM template in a template message, use the [Messages API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/message-api#post-version-phone-number-id-messages). Use the POST body to define the contents of the message and to describe any variables to inject into the template itself.

### Request syntax

```html
curl -X POST &quot;https://graph.facebook.com/v23.0/&lt;BUSINESS_PHONE_NUMBER_ID&gt;/messages&quot; \
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
          &quot;type&quot;: &quot;header&quot;,
          &quot;parameters&quot;: [
            &#123;
              &quot;type&quot;: &quot;text&quot;,
              &quot;text&quot;: &quot;&lt;HEADER_TEXT&gt;&quot;
            &#125;
          ]
        &#125;,
        &#123;
          &quot;type&quot;: &quot;body&quot;,
          &quot;parameters&quot;: [
            &#123;
              &quot;type&quot;: &quot;text&quot;,
              &quot;text&quot;: &quot;&lt;BODY_TEXT&gt;&quot;
            &#125;
          ]
        &#125;,
        &#123;
          &quot;type&quot;: &quot;button&quot;,
          &quot;sub_type&quot;: &quot;mpm&quot;,
          &quot;index&quot;: 0,
          &quot;parameters&quot;: [
            &#123;
              &quot;type&quot;: &quot;action&quot;,
              &quot;action&quot;: &#123;
                &quot;thumbnail_product_retailer_id&quot;: &quot;&lt;THUMBNAIL_PRODUCT_RETAILER_ID&gt;&quot;,
                &quot;sections&quot;: [
                  &#123;
                    &quot;title&quot;: &quot;&lt;TITLE&gt;&quot;,
                    &quot;product_items&quot;: [
                      &#123;
                        &quot;product_retailer_id&quot;: &quot;&lt;PRODUCT_RETAILER_ID_1&gt;&quot;
                      &#125;,
                      &#123;
                        &quot;product_retailer_id&quot;: &quot;&lt;PRODUCT_RETAILER_ID_2&gt;&quot;
                      &#125;
                      // ... Add up to 30 product items per section
                    ]
                  &#125;
                  // ... Add up to 10 section objects as needed
                ]
              &#125;
            &#125;
          ]
        &#125;
      ]
    &#125;
  &#125;&#039;
```

### Request parameters

| Placeholder | Description | Sample Value |
| --- | --- | --- |
| `&lt;BODY_TEXT&gt;` | **Required if template uses variables.**&lt;br&gt;&lt;br&gt;String or array of strings. Text to replace body variable(s) defined in the template. | `10OFF` |
| `&lt;CODE&gt;` | Template [language and locale code](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/supported-languages). | `en_US` |
| `&lt;HEADER_TEXT&gt;` | **Required if template uses a variable.**&lt;br&gt;&lt;br&gt;Text to replace header variable defined in the template. | `Pablo` |
| `&lt;NAME&gt;` | Template name. | `abandoned_cart` |
| `&lt;PRODUCT_RETAILER_ID&gt;` | SKU number of the item you want to appear in the section.&lt;br&gt;&lt;br&gt;SKU numbers are labeled as **Content ID** in the Commerce Manager.&lt;br&gt;&lt;br&gt;Supports up to 30 products total, across all sections. | `2lc20305pt` |
| `&lt;THUMBNAIL_PRODUCT_RETAILER_ID&gt;` | Item SKU number. Labeled as **Content ID** in the Commerce Manager.&lt;br&gt;&lt;br&gt;The thumbnail of this item will be used as the template message&#039;s header image. | `2lc20305pt` |
| `&lt;TITLE&gt;` | Section title text.&lt;br&gt;&lt;br&gt;You can define up to 10 sections.&lt;br&gt;&lt;br&gt;Maximum 24 characters. Markdown is not supported. | `Popular Bundles` |
| `&lt;TO&gt;` | Customer phone number. | `16505551234` |

### Response

Upon success, the API will respond with:

```json
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;contacts&quot;: [
    &#123;
      &quot;input&quot;: &quot;&lt;INPUT&gt;&quot;,
      &quot;wa_id&quot;: &quot;&lt;WA_ID&gt;&quot;
    &#125;
  ],
  &quot;messages&quot;: [
    &#123;
      &quot;id&quot;: &quot;&lt;ID&gt;&quot;
    &#125;
  ]
&#125;
```

### Response parameters

| Placeholder | Description | Sample Value |
| --- | --- | --- |
| `&lt;ID&gt;` | WhatsApp message ID. | `wamid.HBgLMTY1MDM4Nzk0MzkVAgARGBJDOEI3ODgxNzQzMjJBQTdEQTcA` |
| `&lt;INPUT&gt;` | Customer WhatsApp phone number. | `16505551234` |
| `&lt;WA_ID&gt;` | Customer WhatsApp ID. | `16505551234` |

### Example request

This example sends an approved template named &quot;abandoned_cart&quot; and injects a variable (the customer&#039;s first name) into the template header and a discount code into the template body. It also defines two sections (&quot;Popular Bundles&quot; and &quot;Premium Packages&quot;) and identifies the products (a total of 3) that should be injected into those sections.

```html
curl &#039;https://graph.facebook.com/v25.0/106540352242922/messages&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039; \
-d &#039;
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;recipient_type&quot;: &quot;individual&quot;,
  &quot;to&quot;: &quot;16505551234&quot;,
  &quot;type&quot;: &quot;template&quot;,
  &quot;template&quot;: &#123;
    &quot;name&quot;: &quot;abandoned_cart&quot;,
    &quot;language&quot;: &#123;
      &quot;code&quot;: &quot;en_US&quot;
    &#125;,
    &quot;components&quot;: [
      &#123;
        &quot;type&quot;: &quot;header&quot;,
        &quot;parameters&quot;: [
          &#123;
            &quot;type&quot;: &quot;text&quot;,
            &quot;text&quot;: &quot;Pablo&quot;
          &#125;
        ]
      &#125;,
      &#123;
        &quot;type&quot;: &quot;body&quot;,
        &quot;parameters&quot;: [
          &#123;
            &quot;type&quot;: &quot;text&quot;,
            &quot;text&quot;: &quot;10OFF&quot;
          &#125;
        ]
      &#125;,
      &#123;
        &quot;type&quot;: &quot;button&quot;,
        &quot;sub_type&quot;: &quot;mpm&quot;,
        &quot;index&quot;: 0,
        &quot;parameters&quot;: [
          &#123;
            &quot;type&quot;: &quot;action&quot;,
            &quot;action&quot;: &#123;
              &quot;thumbnail_product_retailer_id&quot;: &quot;2lc20305pt&quot;,
              &quot;sections&quot;: [
                &#123;
                  &quot;title&quot;: &quot;Popular Bundles&quot;,
                  &quot;product_items&quot;: [
                    &#123;
                      &quot;product_retailer_id&quot;: &quot;2lc20305pt&quot;
                    &#125;,
                    &#123;
                      &quot;product_retailer_id&quot;: &quot;nseiw1x3ch&quot;
                    &#125;
                  ]
                &#125;,
                &#123;
                  &quot;title&quot;: &quot;Premium Packages&quot;,
                  &quot;product_items&quot;: [
                    &#123;
                      &quot;product_retailer_id&quot;: &quot;n6k6x0y7oe&quot;
                    &#125;
                  ]
                &#125;
              ]
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
      &quot;input&quot;: &quot;16505551234&quot;,
      &quot;wa_id&quot;: &quot;16505551234&quot;
    &#125;
  ],
  &quot;messages&quot;: [
    &#123;
      &quot;id&quot;: &quot;wamid.HBgLMTY1MDM4Nzk0MzkVAgARGBJDOEI3ODgxNzQzMjJBQTdEQTcA&quot;
    &#125;
  ]
&#125;
```
