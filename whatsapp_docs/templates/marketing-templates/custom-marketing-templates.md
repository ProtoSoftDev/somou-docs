# Custom marketing templates



Learn how to create and send a custom marketing template.

## Supported components

Custom marketing templates support the following components:

- 1 header (optional; all types supported)
- 1 body (required)
- 1 footer (optional)
- Up to 10 buttons (optional; all types supported)

## Step 1: Create a custom marketing template

Use the [Message Templates API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/message-template-api#post-version-waba-id-message-templates) to create a custom marketing template.

### Request syntax

This example syntax creates a template with an image header, body text with 3 named parameters, a footer, and 3 buttons (url, phone number, and quick-reply).

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
      &quot;format&quot;: &quot;image&quot;,
      &quot;example&quot;: &#123;
        &quot;header_handle&quot;: [
          &quot;&lt;HEADER_ASSET_HANDLE&gt;&quot;
        ]
      &#125;
    &#125;,
    &#123;
      &quot;type&quot;: &quot;body&quot;,
      &quot;text&quot;: &quot;&lt;BODY_TEXT&gt;&quot;,
      &quot;example&quot;: &#123;
        &quot;body_text_named_params&quot;: [
          &#123;
            &quot;param_name&quot;: &quot;&lt;BODY_PARAMETER_NAME&gt;&quot;,
            &quot;example&quot;: &quot;&lt;BODY_PARAMETER_EXAMPLE_VALUE&gt;&quot;
          &#125;,
          &lt;!-- additional parameters and example values go here --&gt;
        ]
      &#125;
    &#125;,
    &#123;
      &quot;type&quot;: &quot;footer&quot;,
      &quot;text&quot;: &quot;&lt;FOOTER_TEXT&gt;&quot;
    &#125;,
    &#123;
      &quot;type&quot;: &quot;buttons&quot;,
      &quot;buttons&quot;: [
        &#123;
          &quot;type&quot;: &quot;url&quot;,
          &quot;text&quot;: &quot;&lt;URL_BUTTON_LABEL_TEXT&gt;&quot;,
          &quot;url&quot;: &quot;&lt;URL_BUTTON_URL&gt;&quot;
        &#125;,
        &#123;
          &quot;type&quot;: &quot;phone_number&quot;,
          &quot;text&quot;: &quot;&lt;PHONE_NUMBER_BUTTON_LABEL_TEXT&gt;&quot;,
          &quot;phone_number&quot;: &quot;&lt;PHONE_NUMBER_BUTTON_PHONE_NUMBER&gt;&quot;
        &#125;,
        &#123;
          &quot;type&quot;: &quot;quick_reply&quot;,
          &quot;text&quot;: &quot;&lt;QUICK_REPLY_BUTTON_LABEL_TEXT&gt;&quot;
        &#125;
      ]
    &#125;
  ]
&#125;&#039;
```

### Request parameters

| Placeholder | Description | Sample Value |
| --- | --- | --- |
| `&lt;ACCESS_TOKEN&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Access token. | `EAAAN6tcBzAUBOZC82CW7iR2LiaZBwUHS4Y7FDtQxRUPy1PHZClDGZBZCgWdrTisgMjpFKiZAi1FBBQNO2IqZBAzdZAA16lmUs0XgRcCf6z1LLxQCgLXDEpg80d41UZBt1FKJZCqJFcTYXJvSMeHLvOdZwFyZBrV9ZPHZASSqxDZBUZASyFdzjiy2A1sippEsF4DVV5W2IlkOSr2LrMLuYoNMYBy8xQczzOKDOMccqHEZD` |
| `&lt;API_VERSION&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;API version. If omitted, defaults to the newest API version available to your app. | `v23.0` |
| `&lt;BODY_PARAMETER_EXAMPLE_VALUE&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required if using a body component string that includes one or more parameters.**&lt;br&gt;&lt;br&gt;Example parameter value. You must supply an example for each parameter defined in your body component string. | `WELCOME20` |
| `&lt;BODY_PARAMETER_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required if using named parameters.**&lt;br&gt;&lt;br&gt;Parameter name. You must supply a name for each parameter defined in your body component string. Must be a unique string, composed of lowercase characters and underscores, wrapped in double curly brackets. | `&#123;&#123;discount_code&#125;&#125;` |
| `&lt;BODY_TEXT&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Template body text. Variables are supported.&lt;br&gt;&lt;br&gt;Maximum 1024 characters. | `Welcome to Lucky Shrub, &#123;&#123;first_name&#125;&#125;!\\n\\nUse code *&#123;&#123;discount_code&#125;&#125;* to get &#123;&#123;discount_amount&#125;&#125; off of your first purchase!` |
| `&lt;FOOTER_TEXT&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Footer text.&lt;br&gt;&lt;br&gt;Maximum 60 characters. | `Lucky Shrub: Your gateway to succulents!` |
| `&lt;PARAMETER_FORMAT&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;[Parameter format](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/overview#parameter-formats). Value can be:&lt;br&gt;&lt;br&gt;- `named`&lt;br&gt;- `positional`&lt;br&gt;&lt;br&gt;If the `parameter_format` property is omitted, the template will use positional formatting. | `Lucky Shrub: Your gateway to succulents!` |
| `&lt;HEADER_ASSET_HANDLE&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required if using a header with a media asset.**&lt;br&gt;&lt;br&gt;Header [media asset handle](https://developers.facebook.com/docs/graph-api/guides/upload). | `4::aW1hZ2UvcG5n:ARYpf5zqqUjggwGfsZOJ2_o26Zs8ntcO2mss2vKpFb8P_IvskL043YXKpehYTD7IxqEB4t-uZcIzOTxOFRavEcN_tZLhk1WXFb3IOr4S8UKJcQ:e:1759093121:634974688087057:100089620928913:ARYyOAh63uQLhDpqOdk` |
| `&lt;PHONE_NUMBER_BUTTON_LABEL_TEXT&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required if using a phone number button.**&lt;br&gt;&lt;br&gt;Button label text. Maximum 25 characters. Alphanumeric characters only. | `Call us` |
| `&lt;PHONE_NUMBER_BUTTON_PHONE_NUMBER&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required if using a phone number button component.**&lt;br&gt;&lt;br&gt;Business phone number to be called in the WhatsApp user&#039;s default phone app when tapped by the user. Note that some countries have special phone numbers that have leading zeros after the country calling code (for example, +55-0-955-585-95436). If you assign one of these numbers to the button, the leading zero will be stripped from the number. If your number will not work without the leading zero, assign an alternate number to the button, or add the number as message body text.&lt;br&gt;&lt;br&gt;Maximum 20 characters. Alphanumeric characters only. | `15550051310` |
| `&lt;QUICK_REPLY_BUTTON_LABEL_TEXT&gt;` | **Required if using a quick-reply button.**&lt;br&gt;&lt;br&gt;Button label text. Maximum 25 characters. Alphanumeric characters only. | `Unsubscribe` |
| `&lt;TEMPLATE_LANGUAGE&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Template [language code](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/supported-languages). | `en_US` |
| `&lt;TEMPLATE_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Template name. Must be unique, unless existing templates with the same name have a different template language.&lt;br&gt;&lt;br&gt;Maximum 512 characters. Lowercase, alphanumeric characters and underscores only. | `reservation_confirmation` |
| `&lt;URL_BUTTON_URL&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required if including a URL button.**&lt;br&gt;&lt;br&gt;URL to be loaded in WhatsApp user&#039;s default web browser when tapped. | `https://www.luckyshrubeater.com/reservations` |
| `&lt;URL_BUTTON_LABEL_TEXT&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required if using a URL button.**&lt;br&gt;&lt;br&gt;Button label text.&lt;br&gt;&lt;br&gt;Maximum 25 characters. Alphanumeric characters only. | `View deals` |
| `&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;WhatsApp Business account ID. | `546151681022936` |

### Response syntax

Upon success:

```html
&#123;
  &quot;id&quot;: &quot;&lt;TEMPLATE_ID&gt;&quot;,
  &quot;status&quot;: &quot;&lt;TEMPLATE_STATUS&gt;&quot;,
  &quot;category&quot;: &quot;&lt;TEMPLATE_CATEGORY&gt;&quot;
&#125;
```

### Response parameters

| Placeholder | Description | Example value |
| --- | --- | --- |
| `&lt;TEMPLATE_CATEGORY&gt;` | [Template category](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/template-categorization). | `MARKETING` |
| `&lt;TEMPLATE_ID&gt;` | Template ID. | `1627019861106475` |
| `&lt;TEMPLATE_STATUS&gt;` | [Template status](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/overview#template-status). | `PENDING` |

### Example request

```curl
curl &#039;https://graph.facebook.com/v23.0/102290129340398/message_templates&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039; \
-d &#039;
&#123;
  &quot;name&quot;: &quot;welcome_discount_template&quot;,
  &quot;language&quot;: &quot;en_US&quot;,
  &quot;category&quot;: &quot;marketing&quot;,
  &quot;parameter_format&quot;: &quot;named&quot;,
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
      &quot;type&quot;: &quot;body&quot;,
      &quot;text&quot;: &quot;Welcome to Lucky Shrub, &#123;&#123;first_name&#125;&#125;!\n\nUse code *&#123;&#123;discount_code&#125;&#125;* to get &#123;&#123;discount_amount&#125;&#125; off of your first purchase!&quot;,
      &quot;example&quot;: &#123;
        &quot;body_text_named_params&quot;: [
          &#123;
            &quot;param_name&quot;: &quot;first_name&quot;,
            &quot;example&quot;: &quot;Pablo&quot;
          &#125;,
          &#123;
            &quot;param_name&quot;: &quot;discount_code&quot;,
            &quot;example&quot;: &quot;WELCOME20&quot;
          &#125;,
          &#123;
            &quot;param_name&quot;: &quot;discount_amount&quot;,
            &quot;example&quot;: &quot;20%&quot;
          &#125;
        ]
      &#125;
    &#125;,
    &#123;
      &quot;type&quot;: &quot;footer&quot;,
      &quot;text&quot;: &quot;Lucky Shrub: Your gateway to succulents!&quot;
    &#125;,
    &#123;
      &quot;type&quot;: &quot;buttons&quot;,
      &quot;buttons&quot;: [
        &#123;
          &quot;type&quot;: &quot;url&quot;,
          &quot;text&quot;: &quot;View deals&quot;,
          &quot;url&quot;: &quot;https://www.luckyshrub.com/deals&quot;
        &#125;,
        &#123;
          &quot;type&quot;: &quot;phone_number&quot;,
          &quot;text&quot;: &quot;Call us&quot;,
          &quot;phone_number&quot;: &quot;+15550051310&quot;
        &#125;,
        &#123;
          &quot;type&quot;: &quot;quick_reply&quot;,
          &quot;text&quot;: &quot;Unsubscribe&quot;
        &#125;
      ]
    &#125;
  ]
&#125;&#039;
```

### Example response

```json
&#123;
  &quot;id&quot;: &quot;1627019861106475&quot;,
  &quot;status&quot;: &quot;PENDING&quot;,
  &quot;category&quot;: &quot;MARKETING&quot;
&#125;
```

## Step 2: Send a custom marketing template

Use the [Messages API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/message-api#post-version-phone-number-id-messages) to send an approved marketing template.

### Request syntax

This example syntax is for sending the template described in the [create syntax](#request-syntax) above, which expects a header image asset, and 3 body text parameter values which will replace their parameter placeholders in the body text string.

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
            &quot;type&quot;: &quot;image&quot;,
            &quot;image&quot;: &#123;
              &quot;id&quot;: &quot;&lt;HEADER_ASSET_ID&gt;&quot;
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
          &lt;!-- additional parameters and values go here --&gt;
        ]
      &#125;
    ]
  &#125;
&#125;&#039;
```

### Request parameters

| Placeholder | Description | Example value |
| --- | --- | --- |
| `&lt;ACCESS_TOKEN&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Access token. | `EAAAN6tcBzAUBOZC82CW7iR2LiaZBwUHS4Y7FDtQxRUPy1PHZClDGZBZCgWdrTisgMjpFKiZAi1FBBQNO2IqZBAzdZAA16lmUs0XgRcCf6z1LLxQCgLXDEpg80d41UZBt1FKJZCqJFcTYXJvSMeHLvOdZwFyZBrV9ZPHZASSqxDZBUZASyFdzjiy2A1sippEsF4DVV5W2IlkOSr2LrMLuYoNMYBy8xQczzOKDOMccqHEZD` |
| `&lt;API_VERSION&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;API version. If omitted, defaults to the newest API version available to your app. | `v23.0` |
| `&lt;BUSINESS_PHONE_NUMBER_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;WhatsApp Business phone number ID. | `106540352242922` |
| `&lt;HEADER_ASSET_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required if template uses a header media.**&lt;br&gt;&lt;br&gt;Header [media asset ID](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-phone-numbers/media#upload-media). | `1339522734477770` |
| `&lt;PARAMETER_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required if template uses one or more named parameters.**&lt;br&gt;&lt;br&gt;Name of [named parameter](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/overview#named-parameters). | `discount_code` |
| `&lt;PARAMETER_VALUE&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required if template uses one or more named parameters.**&lt;br&gt;&lt;br&gt;[Named parameter](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/overview#named-parameters) value. | `WELCOME25` |
| `&lt;TEMPLATE_LANGUAGE&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;[Template language](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/supported-languages). | `en_US` |
| `&lt;TEMPLATE_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Template name. | `welcome_discount_template` |
| `&lt;WHATSAPP_USER_PHONE_NUMBER&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;WhatsApp user phone number. | `16505551234` |

### Response syntax

Upon success:

```html
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;contacts&quot;: [
    &#123;
      &quot;input&quot;: &quot;&lt;WHATSAPP_USER_PHONE_NUMBER&gt;&quot;,
      &quot;wa_id&quot;: &quot;&lt;WHATSAPP_USER_ID&gt;&quot;
    &#125;
  ],
  &quot;messages&quot;: [
    &#123;
      &quot;id&quot;: &quot;&lt;WHATSAPP_MESSAGE_ID&gt;&quot;,
      &quot;message_status&quot;: &quot;&lt;PACING_STATUS&gt;&quot;
    &#125;
  ]
&#125;
```

### Response parameters

| Placeholder | Description | Example value |
| --- | --- | --- |
| `&lt;PACING_STATUS&gt;` | Template [pacing status](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/template-pacing). | `accepted` |
| `&lt;WHATSAPP_MESSAGE_ID&gt;` | WhatsApp Message ID.&lt;br&gt;&lt;br&gt;This ID is included in status [messages](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/messages/status) webhooks for delivery status purposes. | `wamid.HBgLMTY1MDM4Nzk0MzkVAgARGBJBRkJENzExMTRFRjk2NTI1OTEA` |
| `&lt;WHATSAPP_USER_ID&gt;` | WhatsApp user&#039;s WhatsApp ID. May not match `input` value. | `16505551234` |
| `&lt;WHATSAPP_USER_PHONE_NUMBER&gt;` | WhatsApp user&#039;s WhatsApp phone number. May not match `wa_id` value. | `16505551234` |

### Example request

```curl
curl &#039;https://graph.facebook.com/v23.0/106540352242922/messages&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039; \
-d &#039;
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;recipient_type&quot;: &quot;individual&quot;,
  &quot;to&quot;: &quot;16505551234&quot;,
  &quot;type&quot;: &quot;template&quot;,
  &quot;template&quot;: &#123;
    &quot;name&quot;: &quot;welcome_discount_template&quot;,
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
              &quot;id&quot;: &quot;1339522734477770&quot;
            &#125;
          &#125;
        ]
      &#125;,
      &#123;
        &quot;type&quot;: &quot;body&quot;,
        &quot;parameters&quot;: [
          &#123;
            &quot;type&quot;: &quot;text&quot;,
            &quot;parameter_name&quot;: &quot;first_name&quot;,
            &quot;text&quot;: &quot;Jessica&quot;
          &#125;,
          &#123;
            &quot;type&quot;: &quot;text&quot;,
            &quot;parameter_name&quot;: &quot;discount_code&quot;,
            &quot;text&quot;: &quot;WELCOME25&quot;
          &#125;,
          &#123;
            &quot;type&quot;: &quot;text&quot;,
            &quot;parameter_name&quot;: &quot;discount_amount&quot;,
            &quot;text&quot;: &quot;25%&quot;
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
      &quot;id&quot;: &quot;wamid.HBgLMTY1MDM4Nzk0MzkVAgARGBIyQjM2RTlERTY4QjFGNzUwNDgA&quot;,
      &quot;message_status&quot;: &quot;accepted&quot;
    &#125;
  ]
&#125;
```
