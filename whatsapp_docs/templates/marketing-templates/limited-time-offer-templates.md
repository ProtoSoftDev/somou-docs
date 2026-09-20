# Limited-time offer templates



This document describes limited-time offer templates and how to use them.

Limited-time offer templates allow you to display expiration dates and running countdown timers for offer codes in template messages.

## Limitations

* Only templates categorized as `MARKETING` are supported.
* Footer components are not supported.
* Users who view a limited-time offer template message using the WhatsApp web app or desktop app will not see the offer. Instead, they see a message indicating that they have received a message but that the limited-time offer is not supported.

## Creating limited-time offer templates

Use the [Message Templates API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/message-template-api#post-version-waba-id-message-templates) to create a limited-time offer template.

### Request syntax

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
        &quot;type&quot;: &quot;header&quot;,
        &quot;format&quot;: &quot;&lt;HEADER_FORMAT&gt;&quot;,
        &quot;example&quot;: &#123;
          &quot;header_handle&quot;: [
            &quot;&lt;HEADER_ASSET_HANDLE&gt;&quot;
          ]
        &#125;
      &#125;,
      &#123;
        &quot;type&quot;: &quot;limited_time_offer&quot;,
        &quot;limited_time_offer&quot;: &#123;
          &quot;text&quot;: &quot;&lt;LIMITED_TIME_OFFER_TEXT&gt;&quot;,
          &quot;has_expiration&quot;: &lt;HAS_EXPIRATION&gt;
        &#125;
      &#125;,
      &#123;
        &quot;type&quot;: &quot;body&quot;,
        &quot;text&quot;: &quot;&lt;BODY_TEXT&gt;&quot;,
        &quot;example&quot;: &#123;
          &quot;body_text&quot;: [&lt;BODY_TEXT_VARIABLE_EXAMPLES&gt;]
        &#125;
      &#125;,
      &#123;
        &quot;type&quot;: &quot;buttons&quot;,
        &quot;buttons&quot;: [
          &#123;
            &quot;type&quot;: &quot;copy_code&quot;,
            &quot;example&quot;: &quot;&lt;OFFER_CODE_EXAMPLE&gt;&quot;
          &#125;,
          &#123;
            &quot;type&quot;: &quot;url&quot;,
            &quot;text&quot;: &quot;&lt;URL_BUTTON_TEXT&gt;&quot;,
            &quot;url&quot;: &quot;&lt;URL_BUTTON_URL&gt;&quot;,
            &quot;example&quot;: [
              &quot;&lt;URL_EXAMPLE_WITH_VARIABLE_EXAMPLE&gt;&quot;
            ]
          &#125;
        ]
      &#125;
    ]
  &#125;&#039;
```

### Request parameters

| Placeholder | Description | Example Value |
| --- | --- | --- |
| `&lt;BODY_TEXT&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Body component text. Supports variables.&lt;br&gt;&lt;br&gt;Maximum 600 characters. | `Good news, &#123;&#123;1&#125;&#125;! Use code &#123;&#123;2&#125;&#125; to get 25% off all Caribbean Destination packages!` |
| `&lt;BODY_TEXT_VARIABLE_EXAMPLES&gt;`&lt;br&gt;&lt;br&gt;_Array of strings_ | **Required if body component text uses variables.**&lt;br&gt;&lt;br&gt;Array of example variable strings.&lt;br&gt;&lt;br&gt;Must supply examples for all placeholders in `&lt;BODY_TEXT&gt;` string.&lt;br&gt;&lt;br&gt;No maximum, but counts against `&lt;BODY_TEXT&gt;` maximum. | `[&quot;Pablo&quot;,&quot;CARIBE25&quot;]` |
| `&lt;HAS_EXPIRATION&gt;`&lt;br&gt;&lt;br&gt;_Boolean_ | **Optional.**&lt;br&gt;&lt;br&gt;Set to `true` to have the [offer expiration details](#offer-expiration-details) appear in the delivered message. | `true` |
| `&lt;HEADER_ASSET_HANDLE&gt;`&lt;br&gt;&lt;br&gt;_Media asset handle_ | **Required if using an image or video header.**&lt;br&gt;&lt;br&gt;Uploaded media asset handle. Use the [Resumable Upload API](https://developers.facebook.com/docs/graph-api/guides/upload) to generate an asset handle. | `4::aW...` |
| `&lt;HEADER_FORMAT&gt;`&lt;br&gt;&lt;br&gt;_Enum_ | **Required if using a header.**&lt;br&gt;&lt;br&gt;Can be `IMAGE`, or `VIDEO`. | `IMAGE` |
| `&lt;LIMITED_TIME_OFFER_TEXT&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Offer details text.&lt;br&gt;&lt;br&gt;Maximum 16 characters. | `Expiring offer!` |
| `&lt;OFFER_CODE_EXAMPLE&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Example offer code.&lt;br&gt;&lt;br&gt;Maximum 15 characters. | `CARIBE25` |
| `&lt;TEMPLATE_LANGUAGE&gt;`&lt;br&gt;&lt;br&gt;_Enum_ | **Required.**&lt;br&gt;&lt;br&gt;Template [language and locale code](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/supported-languages). | `en_US` |
| `&lt;TEMPLATE_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Template name.&lt;br&gt;&lt;br&gt;Maximum 512 characters. | `limited_time_offer_caribbean_pkg_2023` |
| `&lt;URL_BUTTON_TEXT&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;[URL button](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/components#url-buttons) label text. Supports 1 variable.&lt;br&gt;&lt;br&gt;25 characters maximum. | `Book now!` |
| `&lt;URL_BUTTON_URL&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;URL of website that loads in the device&#039;s default mobile web browser when the [URL button](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/components#url-buttons) is tapped by the WhatsApp user.&lt;br&gt;&lt;br&gt;Supports 1 variable appended to the end of the URL string.&lt;br&gt;&lt;br&gt;Maximum 2000 characters. | `https://awesomedestinations.com/offers?code=&#123;&#123;1&#125;&#125;` |
| `&lt;URL_EXAMPLE_WITH_VARIABLE_EXAMPLE&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required if URL uses a variable.**&lt;br&gt;&lt;br&gt;Example URL with example variable appended to the end.&lt;br&gt;&lt;br&gt;No maximum, but value counts against `&lt;URL_BUTTON_URL&gt;` maximum. | `https://awesomedestinations.com/offers?ref=n3mtql` |

### Offer expiration details

The delivered message can display an offer expiration details section with a heading, an optional expiration timer, and the offer code itself.

The expiration timer is a text string that is not customizable, but the expiration timer will change to red text if the message is viewed and the offer code is expiring within the next hour. (You include the actual offer code and its expiration timestamp when you send the template in a template message.)

### Example request

This is an example request to create a limited-time offer template that uses:

* an image header component
* body text component with variables
* the limited-time offer component
* a copy code button
* a button URL with a variable

```curl
curl &#039;https://graph.facebook.com/v17.0/102290129340398/message_templates&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039; \
-d &#039;
&#123;
  &quot;name&quot;: &quot;limited_time_offer_caribbean_pkg_2023&quot;,
  &quot;language&quot;: &quot;en_US&quot;,
  &quot;category&quot;: &quot;marketing&quot;,
  &quot;components&quot;: [
    &#123;
      &quot;type&quot;: &quot;header&quot;,
      &quot;format&quot;: &quot;image&quot;,
      &quot;example&quot;: &#123;
        &quot;header_handle&quot;: [
          &quot;4::aW...&quot;
        ]
      &#125;
    &#125;,
    &#123;
      &quot;type&quot;: &quot;limited_time_offer&quot;,
      &quot;limited_time_offer&quot;: &#123;
        &quot;text&quot;: &quot;Expiring offer!&quot;,
        &quot;has_expiration&quot;: true
      &#125;
    &#125;,
    &#123;
      &quot;type&quot;: &quot;body&quot;,
      &quot;text&quot;: &quot;Good news, &#123;&#123;1&#125;&#125;! Use code &#123;&#123;2&#125;&#125; to get 25% off all Caribbean Destination packages!&quot;,
      &quot;example&quot;: &#123;
        &quot;body_text&quot;: [
          [
            &quot;Pablo&quot;,
            &quot;CARIBE25&quot;
          ]
        ]
      &#125;
    &#125;,
    &#123;
      &quot;type&quot;: &quot;buttons&quot;,
      &quot;buttons&quot;: [
        &#123;
          &quot;type&quot;: &quot;copy_code&quot;,
          &quot;example&quot;: &quot;CARIBE25&quot;
        &#125;,
        &#123;
          &quot;type&quot;: &quot;url&quot;,
          &quot;text&quot;: &quot;Book now!&quot;,
          &quot;url&quot;: &quot;https://awesomedestinations.com/offers?code=&#123;&#123;1&#125;&#125;&quot;,
          &quot;example&quot;: [
            &quot;https://awesomedestinations.com/offers?ref=n3mtql&quot;
          ]
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

## Sending limited-time offer templates

Use the [Messages API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/message-api#post-version-phone-number-id-messages) to send an approved limited-time offer template in a template message.

### Request syntax

### Request parameters

| Placeholder | Description | Example Value |
| --- | --- | --- |
| `&lt;BODY_VARIABLES&gt;`&lt;br&gt;&lt;br&gt;_Array of objects_ | **Required if template body text uses variables.**&lt;br&gt;&lt;br&gt;Body text variable values. Define each variable as an individual object. | `&#123;&quot;type&quot;:&quot;text&quot;,&quot;text&quot;:&quot;Pablo&quot;&#125;,&#123;&quot;type&quot;:&quot;text&quot;,&quot;text&quot;:&quot;CARIBE25&quot;&#125;` |
| `&lt;CUSTOMER_PHONE_NUMBER&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Phone number of the WhatsApp user who the template message should be sent to. | `+16505555555` |
| `&lt;EXPIRATION_TIME&gt;`&lt;br&gt;&lt;br&gt;_Unix timestamp_ | **Required.**&lt;br&gt;&lt;br&gt;Offer code expiration time as a UNIX timestamp in milliseconds. | `1698562800000` |
| `&lt;HEADER_ASSET_ID&gt;`&lt;br&gt;&lt;br&gt;_Media asset ID_ | **Required.**&lt;br&gt;&lt;br&gt;Uploaded media asset ID. Use the [/PHONE_NUMBER_ID/media](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/message-api) endpoint to generate an ID. | `1602186516975000` |
| `&lt;HEADER_TYPE&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Header type used by the template. Values can be `image` or `video`. | `image` |
| `&lt;OFFER_CODE&gt;`&lt;br&gt;&lt;br&gt;_String_ | Offer code.&lt;br&gt;&lt;br&gt;Maximum 15 characters. | `CARIBE25` |
| `&lt;TEMPLATE_LANGUAGE_CODE&gt;`&lt;br&gt;&lt;br&gt;_Enum_ | **Required.**&lt;br&gt;&lt;br&gt;The template&#039;s [language and locale code](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/supported-languages). | `en_US` |
| `&lt;TEMPLATE_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;The template&#039;s name. | `limited_time_offer_caribbean_pkg_2023` |
| `&lt;URL_BUTTON_INDEX&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | **Required.**&lt;br&gt;&lt;br&gt;URL button index. If the template uses a copy code button, value must be `1`.&lt;br&gt;&lt;br&gt;If the template does not use a copy code button, the value must be `0`. | `1` |
| `&lt;URL_VARIABLE&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required if URL uses a variable.**&lt;br&gt;&lt;br&gt;URL variable value.&lt;br&gt;&lt;br&gt;No maximum, but value counts against URL string maximum of 2000 characters. | `n3mtql` |

### Example request

Example request to send a limited-time offer template that uses:

* an image header
* body text variables
* the offer expiration details
* a copy code button
* a URL button with a variable

```curl
curl &#039;https://graph.facebook.com/v17.0/106540352242922/messages&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039; \
-d &#039;
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;recipient_type&quot;: &quot;individual&quot;,
  &quot;to&quot;: &quot;16505555555&quot;,
  &quot;type&quot;: &quot;template&quot;,
  &quot;template&quot;: &#123;
    &quot;name&quot;: &quot;limited_time_offer_caribbean_pkg_2023&quot;,
    &quot;language&quot;: &#123;
      &quot;code&quot;: &quot;en_US&quot;
    &#125;,
    &quot;components&quot;: [
      &#123;
        &quot;type&quot;: &quot;header&quot;,
        &quot;parameters&quot;: [
          &#123;
            &quot;type&quot;: &quot;image&quot;,
            &quot;image&quot;: &#123;
              &quot;id&quot;: &quot;1602186516975000&quot;
            &#125;
          &#125;
        ]
      &#125;,
      &#123;
        &quot;type&quot;: &quot;body&quot;,
        &quot;parameters&quot;: [
          &#123;
            &quot;type&quot;: &quot;text&quot;,
            &quot;text&quot;: &quot;Pablo&quot;
          &#125;,
          &#123;
            &quot;type&quot;: &quot;text&quot;,
            &quot;text&quot;: &quot;CARIBE25&quot;
          &#125;
        ]
      &#125;,
      &#123;
        &quot;type&quot;: &quot;limited_time_offer&quot;,
        &quot;parameters&quot;: [
          &#123;
            &quot;type&quot;: &quot;limited_time_offer&quot;,
            &quot;limited_time_offer&quot;: &#123;
              &quot;expiration_time_ms&quot;: 1209600000
            &#125;
          &#125;
        ]
      &#125;,
      &#123;
        &quot;type&quot;: &quot;button&quot;,
        &quot;sub_type&quot;: &quot;copy_code&quot;,
        &quot;index&quot;: 0,
        &quot;parameters&quot;: [
          &#123;
            &quot;type&quot;: &quot;coupon_code&quot;,
            &quot;coupon_code&quot;: &quot;CARIBE25&quot;
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
            &quot;text&quot;: &quot;n3mtql&quot;
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
      &quot;input&quot;: &quot;16505555555&quot;,
      &quot;wa_id&quot;: &quot;16505555555&quot;
    &#125;
  ],
  &quot;messages&quot;: [
    &#123;
      &quot;id&quot;: &quot;wamid.HBgLMTY1MDUwNzY1MjAVAgARGBI5QTNDQTVCM0Q0Q0Q2RTY3RTcA&quot;
    &#125;
  ]
&#125;
```

## Combining with payment request buttons

**Warning:** This feature is only available for businesses based in Brazil using payment request CTA buttons with Pix, Boleto, or Payment Link.

You can combine limited-time offer templates with payment request CTA buttons to send time-sensitive payment requests that display a countdown timer alongside payment options. This is useful for scenarios such as flash sales or promotional discounts with a deadline where the payment method should be readily accessible within the message.

For template creation payloads, supported payment methods, button configuration, and expiration management details, see [Payment Request CTA Templates (Brazil)](https://developers.facebook.com/documentation/business-messaging/whatsapp/payments/payments-br/payment-request-cta).
