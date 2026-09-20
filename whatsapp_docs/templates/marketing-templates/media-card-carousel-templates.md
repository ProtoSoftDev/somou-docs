# Media card carousel templates



Media card carousel templates allow you to send a single **marketing template** message accompanied by a set of up to 10 product media cards in a horizontally scrollable view:

When a WhatsApp user taps a media card&#039;s **URL** button to buy a product, the URL mapped to the button is loaded in the device&#039;s default web browser, thus taking the WhatsApp user out of the WhatsApp client experience. If you prefer to keep the user in the WhatsApp client, see [Product Card Carousel Templates](https://developers.facebook.com/documentation/business-messaging/whatsapp/catalogs/product-card-carousel-template-messages). Note that carousel cards are only available for marketing template messages.

## Media cards

Carousel templates consist of a message body text and up to 10 product media cards. Each card in the template has an image or video header asset and can optionally include a body text and up to two buttons. Button combinations can be a mix of [quick reply buttons](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/components#quick-reply-buttons), [phone number buttons](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/components#phone-number-buttons), and [URL buttons](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/components#url-buttons).

All cards defined on a template must have the same components.

When WhatsApp users place an order, they do so outside of the WhatsApp client, so no webhooks are triggered describing their order.

## Creating media card carousel templates

Use the [Message Templates API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/message-template-api#post-version-waba-id-message-templates) to create a media card carousel template.

### Request syntax

Define the exact number of product cards (minimum 2, maximum 10) when you create the template. An approved template can only be used to send the same number of cards as defined during its creation. If any card in the carousel includes a card body text, then all cards must include a card body text to ensure consistent card heights.

```json
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
                &quot;format&quot;: &quot;&lt;CARD_HEADER_FORMAT&gt;&quot;,
                &quot;example&quot;: &#123;
                  &quot;header_handle&quot;: [
                    &quot;&lt;CARD_HEADER_ASSET_HANDLE&gt;&quot;
                  ]
                &#125;
              &#125;,
              &#123;
                &quot;type&quot;: &quot;buttons&quot;,
                &quot;buttons&quot;: [
                  &#123;
                    &quot;type&quot;: &quot;quick_reply&quot;,
                    &quot;text&quot;: &quot;&lt;QUICK_REPLY_BUTTON_LABEL_TEXT&gt;&quot;
                  &#125;,
                  &#123;
                    &quot;type&quot;: &quot;url&quot;,
                    &quot;text&quot;: &quot;&lt;URL_BUTTON_LABEL_TEXT&gt;&quot;,
                    &quot;url&quot;: &quot;&lt;URL_BUTTON_URL&gt;&quot;,
                    &quot;example&quot;: [
                      &quot;&lt;URL_BUTTON_URL_VARIABLE_EXAMPLE&gt;&quot;
                    ]
                  &#125;,
                  &#123;
                    &quot;type&quot;: &quot;phone_number&quot;,
                    &quot;text&quot;: &quot;&lt;PHONE_NUMBER_BUTTON_LABEL_TEXT&gt;&quot;,
                    &quot;phone_number&quot;: &quot;&lt;PHONE_NUMBER&gt;&quot;
                  &#125;
                ]
              &#125;
            ]
          &#125;
          // Add additional cards here, following the same structure
        ]
      &#125;
    ]
  &#125;&#039;
```

### Request parameters

| Placeholder | Description | Example value |
| --- | --- | --- |
| `&lt;CARD_HEADER_ASSET_HANDLE&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Uploaded media asset handle. Use the [Resumable Upload API](https://developers.facebook.com/docs/graph-api/guides/upload) to generate an asset handle.&lt;br&gt;&lt;br&gt;Media assets are automatically cropped to a wide ratio based on the WhatsApp user&#039;s device. | `4::anBlZw==:ARa525ZJ1g0J-8egeiRvb4Z4r9RSi9qeKF7-wXsUiaDFsll5CKbu5H7h_9mTW0TDfA8LEGHC4bAeXtJJiVQADMp5Ooe2huQlhpBxMadJiu3qVg:e:1724535430:634974688087057:100089620928913:ARaQoFQMm6BlbI3MYo4` |
| `&lt;CARD_HEADER_FORMAT&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Card header format. Value can be `image` or `video`. | `image` |
| `&lt;MESSAGE_BODY_TEXT&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Message body text. Supports variables.&lt;br&gt;&lt;br&gt;Maximum 1024 characters. | `Rare succulents for sale! &#123;&#123;1&#125;&#125;, add these unique plants to your collection. Each of these rare succulents are &#123;&#123;2&#125;&#125; if you checkout using code &#123;&#123;3&#125;&#125;. Shop now and add some unique and beautiful plants to your collection!` |
| `&lt;MESSAGE_BODY_TEXT_VARIABLE_EXAMPLE&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required if message body text string uses variables.**&lt;br&gt;&lt;br&gt;Message body text example variable string(s). Number of strings must match the number of variable placeholders in the message body text string.&lt;br&gt;&lt;br&gt;If message body text uses a single variable, `body_text` value can be a string, otherwise it must be an array containing an array of strings. | `20OFF` |
| `&lt;PHONE_NUMBER&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required if using a phone number button.**&lt;br&gt;&lt;br&gt;Alphanumeric string. Business phone number to be called when the WhatsApp user taps the button.&lt;br&gt;&lt;br&gt;Maximum 20 characters. | `+15550051310` |
| `&lt;PHONE_NUMBER_BUTTON_LABEL_TEXT&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required if using a phone number button.**&lt;br&gt;&lt;br&gt;Phone number button label text.&lt;br&gt;&lt;br&gt;Maximum 25 characters. | `Call` |
| `&lt;QUICK_REPLY_BUTTON_LABEL_TEXT&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required if using a quick-reply button.**&lt;br&gt;&lt;br&gt;Quick-reply button label text.&lt;br&gt;&lt;br&gt;Maximum 25 characters. | `Send more like this!` |
| `&lt;TEMPLATE_LANGUAGE&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Template [language and locale code](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/supported-languages). | `en_US` |
| `&lt;TEMPLATE_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Template name.&lt;br&gt;&lt;br&gt;Maximum 512 characters. | `carousel_template_media_cards_v1` |
| `&lt;URL_BUTTON_LABEL_TEXT&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required if using a URL button.**&lt;br&gt;&lt;br&gt;URL button label text.&lt;br&gt;&lt;br&gt;25 characters maximum. | `Shop` |
| `&lt;URL_BUTTON_URL&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required if using a URL button.**&lt;br&gt;&lt;br&gt;URL to be loaded in the device&#039;s default web browser when the WhatsApp user taps the button.&lt;br&gt;&lt;br&gt;Supports 1 variable. Variable placeholder must be appended to the end of the URL string.&lt;br&gt;&lt;br&gt;Maximum 2000 characters. | `https://www.luckyshrub.com/rare-succulents/&#123;&#123;1&#125;&#125;` |
| `&lt;URL_BUTTON_URL_VARIABLE_EXAMPLE&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required if URL button URL uses a variable.**&lt;br&gt;&lt;br&gt;URL button URL example variable string.&lt;br&gt;&lt;br&gt;Maximum 2000 characters. | `BUDDHA` |

### Example request

This example creates a media card carousel template with 3 variables and 3 media cards. Each media card has a quick reply button, and a URL button that uses a variable.

```html
curl &#039;https://graph.facebook.com/v25.0/102290129340398/message_templates&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039; \
-d &#039;
&#123;
  &quot;name&quot;: &quot;carousel_template_media_cards_v1&quot;,
  &quot;language&quot;: &quot;en_US&quot;,
  &quot;category&quot;: &quot;marketing&quot;,
  &quot;components&quot;: [
    &#123;
      &quot;type&quot;: &quot;body&quot;,
      &quot;text&quot;: &quot;Rare succulents for sale! &#123;&#123;1&#125;&#125;, add these unique plants to your collection. Each of these rare succulents are &#123;&#123;2&#125;&#125; if you checkout using code &#123;&#123;3&#125;&#125;. Shop now and add some unique and beautiful plants to your collection!&quot;,
      &quot;example&quot;: &#123;
        &quot;body_text&quot;: [
          [
            &quot;Pablo&quot;,
            &quot;30%&quot;,
            &quot;30OFF&quot;
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
              &quot;format&quot;: &quot;image&quot;,
              &quot;example&quot;: &#123;
                &quot;header_handle&quot;: [
                  &quot;4::an...&quot;
                ]
              &#125;
            &#125;,
            &#123;
              &quot;type&quot;: &quot;buttons&quot;,
              &quot;buttons&quot;: [
                &#123;
                  &quot;type&quot;: &quot;quick_reply&quot;,
                  &quot;text&quot;: &quot;Send me more like this!&quot;
                &#125;,
                &#123;
                  &quot;type&quot;: &quot;url&quot;,
                  &quot;text&quot;: &quot;Shop&quot;,
                  &quot;url&quot;: &quot;https://www.luckyshrub.com/rare-succulents/&#123;&#123;1&#125;&#125;&quot;,
                  &quot;example&quot;: [
                    &quot;BLUE_ELF&quot;
                  ]
                &#125;
              ]
            &#125;
          ]
        &#125;,
        &#123;
          &quot;components&quot;: [
            &#123;
              &quot;type&quot;: &quot;header&quot;,
              &quot;format&quot;: &quot;image&quot;,
              &quot;example&quot;: &#123;
                &quot;header_handle&quot;: [
                  &quot;4::an...&quot;
                ]
              &#125;
            &#125;,
            &#123;
              &quot;type&quot;: &quot;buttons&quot;,
              &quot;buttons&quot;: [
                &#123;
                  &quot;type&quot;: &quot;quick_reply&quot;,
                  &quot;text&quot;: &quot;Send me more like this!&quot;
                &#125;,
                &#123;
                  &quot;type&quot;: &quot;url&quot;,
                  &quot;text&quot;: &quot;Shop&quot;,
                  &quot;url&quot;: &quot;https://www.luckyshrub.com/rare-succulents&#123;&#123;1&#125;&#125;&quot;,
                  &quot;example&quot;: [
                    &quot;BUDDHA&quot;
                  ]
                &#125;
              ]
            &#125;
          ]
        &#125;,
        &#123;
          &quot;components&quot;: [
            &#123;
              &quot;type&quot;: &quot;header&quot;,
              &quot;format&quot;: &quot;image&quot;,
              &quot;example&quot;: &#123;
                &quot;header_handle&quot;: [
                  &quot;4::an...&quot;
                ]
              &#125;
            &#125;,
            &#123;
              &quot;type&quot;: &quot;buttons&quot;,
              &quot;buttons&quot;: [
                &#123;
                  &quot;type&quot;: &quot;quick_reply&quot;,
                  &quot;text&quot;: &quot;Send me more like this!&quot;
                &#125;,
                &#123;
                  &quot;type&quot;: &quot;url&quot;,
                  &quot;text&quot;: &quot;Shop&quot;,
                  &quot;url&quot;: &quot;https://www.luckyshrub.com/rare-succulents&#123;&#123;1&#125;&#125;&quot;,
                  &quot;example&quot;: [
                    &quot;BLACK_PRINCE&quot;
                  ]
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

## Sending media card carousel templates

To send approved [media card carousel templates](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/marketing-templates/media-card-carousel-templates) to WhatsApp users, use the request syntax that follows.

### Request syntax

```json
curl -X POST &quot;https://graph.facebook.com/v23.0/&lt;WHATSAPP_BUSINESS_PHONE_NUMBER_ID&gt;/messages&quot; \
  -H &quot;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&quot; \
  -H &quot;Content-Type: application/json&quot; \
  -d &#039;&#123;
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
                    &#123; &quot;type&quot;: &quot;text&quot;, &quot;text&quot;: &quot;&lt;CARD_BODY_VARIABLE_1&gt;&quot; &#125;,
                    &#123; &quot;type&quot;: &quot;text&quot;, &quot;text&quot;: &quot;&lt;CARD_BODY_VARIABLE_2&gt;&quot; &#125;
                  ]
                &#125;,
                &#123;
                  &quot;type&quot;: &quot;button&quot;,
                  &quot;sub_type&quot;: &quot;quick_reply&quot;,
                  &quot;index&quot;: 0,
                  &quot;parameters&quot;: [
                    &#123;
                      &quot;type&quot;: &quot;payload&quot;,
                      &quot;payload&quot;: &quot;&lt;QUICK_REPLY_BUTTON_PAYLOAD&gt;&quot;
                    &#125;
                  ]
                &#125;,
                &#123;
                  &quot;type&quot;: &quot;button&quot;,
                  &quot;sub_type&quot;: &quot;url&quot;,
                  &quot;index&quot;: 1,
                  &quot;parameters&quot;: [
                    &#123;
                      &quot;type&quot;: &quot;text&quot;,
                      &quot;text&quot;: &quot;&lt;URL_BUTTON_URL_VARIABLE&gt;&quot;
                    &#125;
                  ]
                &#125;
              ]
            &#125;
            // Add additional cards here, following the same structure
          ]
        &#125;
      ]
    &#125;
  &#125;&#039;
```

### Request parameters

| Placeholder | Description | Example value |
| --- | --- | --- |
| `&lt;BUTTON_INDEX&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | **Required.**&lt;br&gt;&lt;br&gt;Zero-indexed order in which button appears at the bottom of the template message. `0` indicates the first button, `1` indicates second button, etc.&lt;br&gt;&lt;br&gt;Note that if any buttons use variables, the type and order of buttons must match the type and order defined on the template, so you can&#039;t use the index values to arrange the order of the buttons in the sent template.&lt;br&gt;&lt;br&gt;For example, if the template defines a phone number button first (which equates to index `0`) and a URL button that supports a single variable second (which equates to index `1`), if you attempt to send the template with the URL button index set to `0` , the API would return an error (&quot;Parameter value for URL was expected but was not found&quot;) because it&#039;s expecting a button object with an index of `1` to be present in the post body payload. | `0` |
| `&lt;CARD_BODY_VARIABLE&gt;`&lt;br&gt;&lt;br&gt;_Object_ | **Required if the template card body text uses variables, otherwise omit.**&lt;br&gt;&lt;br&gt;Object describing a card body variable. If the template uses multiple variables, you must define an object for each variable.&lt;br&gt;&lt;br&gt;Supports `text`, `currency`, and `date_time` types. See [Messages Parameters](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/message-api#parameter-object).&lt;br&gt;&lt;br&gt;There is no maximum character limit on this value, but does count against the card body text limit of 160 characters. | `&#123; &quot;type&quot;:&quot;text&quot;, &quot;text&quot;: &quot;Pablo&quot; &#125;`&lt;br&gt; |
| `&lt;CARD_INDEX&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | **Required.**&lt;br&gt;&lt;br&gt;Zero-indexed order in which card should appear within the card carousel. `0` indicates first card, `1` indicates second card, etc. | `0` |
| `&lt;MESSAGE_BODY_TEXT_VARIABLE&gt;`&lt;br&gt;&lt;br&gt;_Object_ | **Required if template message body text uses variables, otherwise omit.**&lt;br&gt;&lt;br&gt;Object describing a message variable. If the template uses multiple variables, you must define an object for each variable.&lt;br&gt;&lt;br&gt;Supports `text`, `currency`, and `date_time` types. See [Messages Parameters](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/message-api#parameter-object).&lt;br&gt;&lt;br&gt;There is no maximum character limit on this value, but it does count against the message body text limit of 1024 characters. | `&#123; &quot;type&quot;:&quot;text&quot;, &quot;text&quot;: &quot;Pablo&quot; &#125;`&lt;br&gt; |
| `&lt;MESSAGE_HEADER_ASSET_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Header asset&#039;s uploaded media asset ID. Use the [**POST /&lt;BUSINESS_PHONE_NUMBER_ID&gt;/media**](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-phone-numbers/media#upload-media) endpoint to generate an asset ID. | `1558081531584829` |
| `&lt;MESSAGE_HEADER_FORMAT&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Indicates header type and a matching property name.&lt;br&gt;&lt;br&gt;Note that the `&lt;MESSAGE_HEADER_FORMAT&gt;` placeholder appears twice in the post body example above, as it serves as a placeholder for the type property&#039;s value and its matching property name.&lt;br&gt;&lt;br&gt;&lt;br&gt;Value can be `image` or `video`. | `image` |
| `&lt;QUICK_REPLY_BUTTON_PAYLOAD&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Value to be included in messages webhooks (`messages.button.payload`) when the button is tapped. | `more-aloes` |
| `&lt;TEMPLATE_LANGUAGE&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Template [language and locale code](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/supported-languages). | `en_US` |
| `&lt;TEMPLATE_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Template name.&lt;br&gt;&lt;br&gt;Maximum 512 characters. | `carousel_template_media_cards_v1` |
| `&lt;URL_BUTTON_URL_VARIABLE&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required if the URL button URL uses a variable.**&lt;br&gt;&lt;br&gt;URL button variable value. | `blue-elf` |
| `&lt;WHATSAPP_USER_PHONE_NUMBER&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;WhatsApp user phone number. | `+16505551234` |

### Example request

This example request sends a media card carousel template named `carousel_template_media_cards_v1`. It supplies three body text variables (which the template requires) and contents for three cards (which the template also requires). For each card, the request supplies an image asset ID, a quick-reply button payload (to be included in webhooks when the button is tapped), and a text string to be injected into the URL mapped to the card&#039;s URL button (which is defined on the template).

```html
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
    &quot;name&quot;: &quot;carousel_template_media_cards_v1&quot;,
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
          &#125;,
          &#123;
            &quot;type&quot;: &quot;text&quot;,
            &quot;text&quot;: &quot;20%&quot;
          &#125;,
          &#123;
            &quot;type&quot;: &quot;text&quot;,
            &quot;text&quot;: &quot;20OFF&quot;
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
                    &quot;type&quot;: &quot;image&quot;,
                    &quot;image&quot;: &#123;
                      &quot;id&quot;: &quot;1558081531584829&quot;
                    &#125;
                  &#125;
                ]
              &#125;,
              &#123;
                &quot;type&quot;: &quot;button&quot;,
                &quot;sub_type&quot;: &quot;quick_reply&quot;,
                &quot;index&quot;: &quot;0&quot;,
                &quot;parameters&quot;: [
                  &#123;
                    &quot;type&quot;: &quot;payload&quot;,
                    &quot;payload&quot;: &quot;more-aloes&quot;
                  &#125;
                ]
              &#125;,
              &#123;
                &quot;type&quot;: &quot;button&quot;,
                &quot;sub_type&quot;: &quot;url&quot;,
                &quot;index&quot;: &quot;1&quot;,
                &quot;parameters&quot;: [
                  &#123;
                    &quot;type&quot;: &quot;text&quot;,
                    &quot;text&quot;: &quot;blue-elf&quot;
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
                    &quot;type&quot;: &quot;image&quot;,
                    &quot;image&quot;: &#123;
                      &quot;id&quot;: &quot;861236878885705&quot;
                    &#125;
                  &#125;
                ]
              &#125;,
              &#123;
                &quot;type&quot;: &quot;button&quot;,
                &quot;sub_type&quot;: &quot;quick_reply&quot;,
                &quot;index&quot;: &quot;0&quot;,
                &quot;parameters&quot;: [
                  &#123;
                    &quot;type&quot;: &quot;payload&quot;,
                    &quot;payload&quot;: &quot;more-crassulas&quot;
                  &#125;
                ]
              &#125;,
              &#123;
                &quot;type&quot;: &quot;button&quot;,
                &quot;sub_type&quot;: &quot;url&quot;,
                &quot;index&quot;: &quot;1&quot;,
                &quot;parameters&quot;: [
                  &#123;
                    &quot;type&quot;: &quot;text&quot;,
                    &quot;text&quot;: &quot;buddhas-temple&quot;
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
                    &quot;type&quot;: &quot;image&quot;,
                    &quot;image&quot;: &#123;
                      &quot;id&quot;: &quot;1587064918516321&quot;
                    &#125;
                  &#125;
                ]
              &#125;,
              &#123;
                &quot;type&quot;: &quot;button&quot;,
                &quot;sub_type&quot;: &quot;quick_reply&quot;,
                &quot;index&quot;: &quot;0&quot;,
                &quot;parameters&quot;: [
                  &#123;
                    &quot;type&quot;: &quot;payload&quot;,
                    &quot;payload&quot;: &quot;more-echeverias&quot;
                  &#125;
                ]
              &#125;,
              &#123;
                &quot;type&quot;: &quot;button&quot;,
                &quot;sub_type&quot;: &quot;url&quot;,
                &quot;index&quot;: &quot;1&quot;,
                &quot;parameters&quot;: [
                  &#123;
                    &quot;type&quot;: &quot;text&quot;,
                    &quot;text&quot;: &quot;black-prince&quot;
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
