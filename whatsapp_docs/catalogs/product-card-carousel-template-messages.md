# Product card carousel templates



Product card carousel templates allow you to send a single text message accompanied by a set of up to 10 product cards in a horizontally scrollable view:

When a WhatsApp user taps the **View** button, they can view more information about the product, add the product to a shopping cart, and place an order, all without leaving the WhatsApp client experience. If instead you prefer to send the user to your website when they click the button, see [Media Card Carousel Templates](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/marketing-templates/media-card-carousel-templates).

## Product cards

Carousel templates support up to 10 product cards, composed of message body text, a product image, product title, product price, and a single View button or URL button. All cards defined on a template must have the same components.

## View buttons

When a WhatsApp user taps the button, the product details view appears, displaying product information pulled from your product catalog.

Users can then add the product to a cart and place an order.

When a user submits the cart, a [webhook](#webhooks) will be triggered describing the order, and an order confirmation message will appear in the message thread.

Users who have placed an order can see the contents of the order by tapping the **View details** button.

## URL buttons

To send users to your website, use **URL** buttons instead of **View** buttons. When a WhatsApp user taps a URL button to buy a product, the URL mapped to the button is loaded in the device&#039;s default web browser, which takes the user out of the WhatsApp client experience. URL button flows can be useful when you want to load the product in your mobile checkout page where users can add promo codes and find related products.

With URL button flows, order placement happens outside of the WhatsApp client, so WhatsApp does not trigger order webhooks for URL-button flows.

## Catalogs

To use product card carousel templates, you must have an e-commerce product catalog, with inventory, connected to your WhatsApp Business account. See the Cloud API [Commerce](https://developers.facebook.com/documentation/business-messaging/whatsapp/catalogs/catalogs-overview) guide to learn more about connecting a catalog to your account.

## Webhooks

If you send a carousel template composed of product cards that use a **View** button, when a customer adds one or more products to their cart and submits an order, an [order messages](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/messages/order) webhook is triggered, describing the order.

## Creating product card carousel templates

Use the [**Message Templates API**](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/message-template-api#post-version-waba-id-message-templates) to create a product card carousel template.

### Request syntax

Define only two product cards when you create the template. An approved template with two product cards can be used to send up to 10 cards in a template message.

```bash
curl -X POST &quot;https://graph.facebook.com/v23.0/&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;/message_templates&quot; \
  -H &quot;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&quot; \
  -H &quot;Content-Type: application/json&quot; \
  -d &#039;
&#123;
    &quot;name&quot;: &quot;&lt;TEMPLATE_NAME&gt;&quot;,
    &quot;language&quot;: &quot;&lt;TEMPLATE_LANGUAGE&gt;&quot;,
    &quot;category&quot;: &quot;marketing&quot;,
    &quot;components&quot;: [
      &#123;
        &quot;type&quot;: &quot;body&quot;,
        &quot;text&quot;: &quot;&lt;MESSAGE_BODY_TEXT&gt;&quot;,
        &quot;example&quot;: &#123;
          &quot;body_text&quot;: [
            [
              &quot;&lt;MESSAGE_BODY_TEXT_VARIABLE_EXAMPLE_1&gt;&quot;,
              &quot;&lt;MESSAGE_BODY_TEXT_VARIABLE_EXAMPLE_2&gt;&quot;
            ]
          ]
        &#125;
      &#125;,
      &#123;
        &quot;type&quot;: &quot;carousel&quot;,
        &quot;cards&quot;: [
          &#123;
            &quot;components&quot;: [
              &#123;
                &quot;type&quot;: &quot;header&quot;,
                &quot;format&quot;: &quot;product&quot;
              &#125;,
              &#123;
                &quot;type&quot;: &quot;buttons&quot;,
                &quot;buttons&quot;: [
                  &#123;
                    &quot;type&quot;: &quot;spm&quot;,
                    &quot;text&quot;: &quot;View&quot;
                  &#125;
                  // OR, for a URL button, use the following instead:
                  // &#123;
                  //   &quot;type&quot;: &quot;url&quot;,
                  //   &quot;text&quot;: &quot;&lt;URL_BUTTON_LABEL_TEXT&gt;&quot;,
                  //   &quot;url&quot;: &quot;&lt;URL_BUTTON_URL&gt;&quot;,
                  //   &quot;example&quot;: [
                  //     &quot;&lt;URL_BUTTON_URL_VARIABLE_EXAMPLE&gt;&quot;
                  //   ]
                  // &#125;
                ]
              &#125;
            ]
          &#125;
          // Add a second product card here, following the same structure as above
        ]
      &#125;
    ]
  &#125;&#039;
```

### Request parameters

| Placeholder | Description | Example Value |
| --- | --- | --- |
| `&lt;MESSAGE_BODY_TEXT&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Message body text. Supports variables.&lt;br&gt;&lt;br&gt;Maximum 1024 characters. | `Rare succulents for sale! &#123;&#123;1&#125;&#125;, add these unique plants to your collection.` |
| `&lt;MESSAGE_BODY_TEXT_VARIABLE_EXAMPLE&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required if message body text string uses variables.**&lt;br&gt;&lt;br&gt;Message body text example variable string(s). Number of strings must match the number of variable placeholders in the message body text string.&lt;br&gt;&lt;br&gt;If message body text uses a single variable, `body_text` value can be a string, otherwise it must be an array containing an array of strings. | `Pablo` |
| `&lt;TEMPLATE_LANGUAGE&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Template [language and locale code](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/supported-languages). | `en_US` |
| `&lt;TEMPLATE_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Template name.&lt;br&gt;&lt;br&gt;Maximum 512 characters. | `carousel_template_product_cards_v1` |
| `&lt;URL_BUTTON_LABEL_TEXT&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required if using a URL button.**&lt;br&gt;&lt;br&gt;URL button label text.&lt;br&gt;&lt;br&gt;25 characters maximum. | `Buy now` |
| `&lt;URL_BUTTON_URL&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required if using a URL button.**&lt;br&gt;&lt;br&gt;URL to be loaded in the device&#039;s default web browser when the WhatsApp user taps the button.&lt;br&gt;&lt;br&gt;Supports 1 variable. Variable placeholder must be appended to the end of the URL string.&lt;br&gt;&lt;br&gt;Maximum 2000 characters. | `https://www.luckyshrub.com/rare-succulents/&#123;&#123;1&#125;&#125;` |
| `&lt;URL_BUTTON_URL_VARIABLE_EXAMPLE&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required if URL button URL uses a variable.**&lt;br&gt;&lt;br&gt;URL button URL example variable string.&lt;br&gt;&lt;br&gt;Maximum 2000 characters. | `BUDDHA` |

### Example request

This example request creates a product card carousel template with a message body that uses a single variable and two product cards. Once approved, it can be used to send up to 10 product cards in a template message.

```json
curl &#039;https://graph.facebook.com/v25.0/161311403722088/message_templates&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039; \
-d &#039;
&#123;
  &quot;name&quot;: &quot;carousel_template_product_cards_v1&quot;,
  &quot;language&quot;: &quot;en_US&quot;,
  &quot;category&quot;: &quot;marketing&quot;,
  &quot;components&quot;: [
    &#123;
      &quot;type&quot;: &quot;body&quot;,
      &quot;text&quot;: &quot;Rare succulents for sale! &#123;&#123;1&#125;&#125;, add these unique plants to your collection. All three of these rare succulents are available for purchase on our website, and they come with a 100% satisfaction guarantee. Whether you&#039;re a seasoned succulent enthusiast or just starting your plant collection, these rare succulents are sure to impress. Shop now and add some unique and beautiful plants to your collection!&quot;,
      &quot;example&quot;: &#123;
        &quot;body_text&quot;: &quot;Pablo&quot;
      &#125;
    &#125;,
    &#123;
      &quot;type&quot;: &quot;carousel&quot;,
      &quot;cards&quot;: [
        &#123;
          &quot;components&quot;: [
            &#123;
              &quot;type&quot;: &quot;header&quot;,
              &quot;format&quot;: &quot;product&quot;
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
        &#125;,
        &#123;
          &quot;components&quot;: [
            &#123;
              &quot;type&quot;: &quot;header&quot;,
              &quot;format&quot;: &quot;product&quot;
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
        &#125;
      ]
    &#125;
  ]
&#125;&#039;
```

## Sending product card carousel templates

### Request syntax

Use the [**Messages API**](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/message-api#post-version-phone-number-id-messages) to send an approved product card carousel template to a WhatsApp user.

```bash
curl -X POST &quot;https://graph.facebook.com/v23.0/&lt;WHATSAPP_BUSINESS_PHONE_NUMBER_ID&gt;/messages&quot; \
  -H &quot;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&quot; \
  -H &quot;Content-Type: application/json&quot; \
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
          &quot;type&quot;: &quot;body&quot;,
          &quot;parameters&quot;: [
            &#123; &quot;type&quot;: &quot;text&quot;, &quot;text&quot;: &quot;&lt;MESSAGE_BODY_TEXT_VARIABLE_1&gt;&quot; &#125;,
            &#123; &quot;type&quot;: &quot;text&quot;, &quot;text&quot;: &quot;&lt;MESSAGE_BODY_TEXT_VARIABLE_2&gt;&quot; &#125;
          ]
        &#125;,
        &#123;
          &quot;type&quot;: &quot;carousel&quot;,
          &quot;cards&quot;: [
            &#123;
              &quot;card_index&quot;: 0,
              &quot;components&quot;: [
                &#123;
                  &quot;type&quot;: &quot;header&quot;,
                  &quot;parameters&quot;: [
                    &#123;
                      &quot;type&quot;: &quot;product&quot;,
                      &quot;product&quot;: &#123;
                        &quot;product_retailer_id&quot;: &quot;&lt;PRODUCT_ID_1&gt;&quot;,
                        &quot;catalog_id&quot;: &quot;&lt;CATALOG_ID&gt;&quot;
                      &#125;
                    &#125;
                  ]
                &#125;
                // Add additional components (e.g., buttons) here if your template defines them
              ]
            &#125;
            // Add additional cards here, incrementing card_index for each
          ]
        &#125;
      ]
    &#125;
  &#125;&#039;
```

### Request parameters

| Placeholder | Description | Example Value |
| --- | --- | --- |
| `&lt;CARD_INDEX&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | **Required.**&lt;br&gt;&lt;br&gt;Zero-indexed order in which card should appear within the card carousel. `0` indicates first card, `1` indicates second card, etc. | `0` |
| `&lt;CATALOG_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;ID of [connected ecommerce catalog](https://www.facebook.com/business/help/158662536425974) containing the product. | `194836987003835` |
| `&lt;MESSAGE_BODY_TEXT_VARIABLE&gt;`&lt;br&gt;&lt;br&gt;_Object_ | **Required if template message body text uses variables, otherwise omit.**&lt;br&gt;&lt;br&gt;Object describing a message variable. If the template uses multiple variables, you must define an object for each variable.&lt;br&gt;&lt;br&gt;Supports `text`, `currency`, and `date_time` types. See [Messages Parameters](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/message-api#parameter-object).&lt;br&gt;&lt;br&gt;There is no maximum character limit on this value, but it does count against the message body text limit of 1024 characters. | `&#123; &quot;type&quot;:&quot;text&quot;, &quot;text&quot;: &quot;Pablo&quot; &#125;`&lt;br&gt; |
| `&lt;PRODUCT_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Product ID. | `vrpj01fvwp` |
| `&lt;TEMPLATE_LANGUAGE&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Template [language and locale code](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/supported-languages). | `en_US` |
| `&lt;TEMPLATE_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Template name.&lt;br&gt;&lt;br&gt;Maximum 512 characters. | `carousel_template_media_cards_v1` |
| `&lt;WHATSAPP_USER_PHONE_NUMBER&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;WhatsApp user phone number. | `+16505551234` |

### Example request

This example request sends an approved template named `carousel_template_product_cards_v1`. It supplies a single body text variable value (which the template requires) and three product cards. Each card identifies where it should appear in the carousel (card_index), as well as the product ID and catalog ID where the card&#039;s product details (such as title, description, and price) can be found.

```bash
curl &#039;https://graph.facebook.com/v25.0/179776755229976/messages&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039; \
-d &#039;
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;recipient_type&quot;: &quot;individual&quot;,
  &quot;to&quot;: &quot;+16505551234&quot;,
  &quot;type&quot;: &quot;template&quot;,
  &quot;template&quot;: &#123;
    &quot;name&quot;: &quot;carousel_template_product_cards_v1&quot;,
    &quot;language&quot;: &#123;
      &quot;code&quot;: &quot;en_US&quot;
    &#125;,
    &quot;components&quot;: [
      &#123;
        &quot;type&quot;: &quot;body&quot;,
        &quot;parameters&quot;: [
          &#123;
            &quot;type&quot;: &quot;text&quot;,
            &quot;text&quot;: &quot;Pablo&quot;
          &#125;
        ]
      &#125;,
      &#123;
        &quot;type&quot;: &quot;carousel&quot;,
        &quot;cards&quot;: [
          &#123;
            &quot;card_index&quot;: 0,
            &quot;components&quot;: [
              &#123;
                &quot;type&quot;: &quot;header&quot;,
                &quot;parameters&quot;: [
                  &#123;
                    &quot;type&quot;: &quot;product&quot;,
                    &quot;product&quot;: &#123;
                      &quot;product_retailer_id&quot;: &quot;vrpj01fvwp&quot;,
                      &quot;catalog_id&quot;: &quot;194836987003835&quot;
                    &#125;
                  &#125;
                ]
              &#125;
            ]
          &#125;,
          &#123;
            &quot;card_index&quot;: 1,
            &quot;components&quot;: [
              &#123;
                &quot;type&quot;: &quot;header&quot;,
                &quot;parameters&quot;: [
                  &#123;
                    &quot;type&quot;: &quot;product&quot;,
                    &quot;product&quot;: &#123;
                      &quot;product_retailer_id&quot;: &quot;va2l5ioeat&quot;,
                      &quot;catalog_id&quot;: &quot;194836987003835&quot;
                    &#125;
                  &#125;
                ]
              &#125;
            ]
          &#125;,
          &#123;
            &quot;card_index&quot;: 2,
            &quot;components&quot;: [
              &#123;
                &quot;type&quot;: &quot;header&quot;,
                &quot;parameters&quot;: [
                  &#123;
                    &quot;type&quot;: &quot;product&quot;,
                    &quot;product&quot;: &#123;
                      &quot;product_retailer_id&quot;: &quot;sqpjv0mgde&quot;,
                      &quot;catalog_id&quot;: &quot;194836987003835&quot;
                    &#125;
                  &#125;
                ]
              &#125;
            ]
          &#125;
        ]
      &#125;
    ]
  &#125;
&#125;&#039;
```
