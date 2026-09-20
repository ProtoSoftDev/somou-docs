# Coupon code templates



Coupon code templates are marketing templates that display a single copy code button. When the app user taps the button, WhatsApp copies the coupon code to the clipboard.

## Limitations

* Coupon code templates are currently not supported by the WhatsApp web client.
* Copy code button text cannot be customized.
* Templates are limited to one copy code button.

To create and send a coupon code template, follow these steps:

1. [Create a coupon code template](#step-1-create-a-coupon-code-template) using the Message Templates API.
2. [Send a coupon code template](#step-2-send-a-coupon-code-template) using the Messages API.

## Properties set at creation vs. send

Some properties are defined when you create the template, while others are provided when you send it. Some properties span both steps.

**Creation only:**
- Header text
- Body text (with parameter placeholders)
- Quick reply button label
- Copy code button example code

**Send only:**
- Body parameter values (`coupon_code`, `discount`)
- Coupon code (copy code button value)

**Both creation and send:**
- Template name, language (referenced at both steps)

## Step 1: Create a coupon code template

Use the [Message Templates API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/message-template-api#post-version-waba-id-message-templates) to create a coupon code template.

### Request syntax

```html
curl &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;/message_templates&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039; \
-d &#039;
&#123;
  &quot;name&quot;: &quot;&lt;TEMPLATE_NAME&gt;&quot;,
  &quot;language&quot;: &quot;&lt;TEMPLATE_LANGUAGE&gt;&quot;,
  &quot;category&quot;: &quot;MARKETING&quot;,
  &quot;parameter_format&quot;: &quot;named&quot;,
  &quot;components&quot;: [
    &#123;
      &quot;type&quot;: &quot;HEADER&quot;,
      &quot;format&quot;: &quot;TEXT&quot;,
      &quot;text&quot;: &quot;&lt;HEADER_TEXT&gt;&quot;
    &#125;,
    &#123;
      &quot;type&quot;: &quot;BODY&quot;,
      &quot;text&quot;: &quot;&lt;BODY_TEXT&gt;&quot;,
      &quot;example&quot;: &#123;
        &quot;body_text_named_params&quot;: [
          &#123;
            &quot;param_name&quot;: &quot;&lt;BODY_PARAMETER_NAME&gt;&quot;,
            &quot;example&quot;: &quot;&lt;BODY_PARAMETER_EXAMPLE_VALUE&gt;&quot;
          &#125;
        ]
      &#125;
    &#125;,
    &#123;
      &quot;type&quot;: &quot;BUTTONS&quot;,
      &quot;buttons&quot;: [
        &#123;
          &quot;type&quot;: &quot;QUICK_REPLY&quot;,
          &quot;text&quot;: &quot;&lt;QUICK_REPLY_BUTTON_LABEL_TEXT&gt;&quot;
        &#125;,
        &#123;
          &quot;type&quot;: &quot;COPY_CODE&quot;,
          &quot;example&quot;: &quot;&lt;COPY_CODE_BUTTON_EXAMPLE_CODE&gt;&quot;
        &#125;
      ]
    &#125;
  ]
&#125;&#039;
```

### Request parameters

| Placeholder | Description | Example Value |
| --- | --- | --- |
| `&lt;ACCESS_TOKEN&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;[System token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#system-user-access-tokens) or [business token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#business-integration-system-user-access-tokens). | `EAAA...` |
| `&lt;API_VERSION&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Graph API version. | v25.0 |
| `&lt;BODY_PARAMETER_EXAMPLE_VALUE&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required if using a body component string that includes one or more parameters.**&lt;br&gt;&lt;br&gt;Example parameter value. You must supply an example for each parameter defined in your body component string. | `WINTER25` |
| `&lt;BODY_PARAMETER_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required if using named parameters.**&lt;br&gt;&lt;br&gt;Parameter name. Must be a unique string, composed of lowercase characters and underscores. | `coupon_code` |
| `&lt;BODY_TEXT&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Template body text. Variables are supported.&lt;br&gt;&lt;br&gt;Maximum 1024 characters. | `Shop now through the end of December and use the one-time use code &#123;&#123;coupon_code&#125;&#125; to get &#123;&#123;discount&#125;&#125; off of your entire order!` |
| `&lt;COPY_CODE_BUTTON_EXAMPLE_CODE&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;String to copy to device clipboard.&lt;br&gt;&lt;br&gt;Maximum 20 characters. | `WINTER25` |
| `&lt;HEADER_TEXT&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required if using a text header component.**&lt;br&gt;&lt;br&gt;Header text.&lt;br&gt;&lt;br&gt;Maximum 60 characters. | `Our Winter Sale is on!` |
| `&lt;QUICK_REPLY_BUTTON_LABEL_TEXT&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required if using a quick-reply button.**&lt;br&gt;&lt;br&gt;Button label text. Maximum 25 characters. Alphanumeric characters only. | `Unsubscribe` |
| `&lt;TEMPLATE_LANGUAGE&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Template [language code](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/supported-languages). | `en_US` |
| `&lt;TEMPLATE_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Template name. Must be unique, unless existing templates with the same name have a different template language.&lt;br&gt;&lt;br&gt;Maximum 512 characters. Lowercase, alphanumeric characters and underscores only. | `winter_sale_coupon` |
| `&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;WhatsApp Business account ID. | `102290129340398` |

### Response syntax

Upon success, the API responds with:

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
curl &#039;https://graph.facebook.com/v25.0/102290129340398/message_templates&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039; \
-d &#039;
&#123;
  &quot;name&quot;: &quot;winter_sale_coupon&quot;,
  &quot;language&quot;: &quot;en_US&quot;,
  &quot;category&quot;: &quot;MARKETING&quot;,
  &quot;parameter_format&quot;: &quot;named&quot;,
  &quot;components&quot;: [
    &#123;
      &quot;type&quot;: &quot;HEADER&quot;,
      &quot;format&quot;: &quot;TEXT&quot;,
      &quot;text&quot;: &quot;Our Winter Sale is on!&quot;
    &#125;,
    &#123;
      &quot;type&quot;: &quot;BODY&quot;,
      &quot;text&quot;: &quot;Shop now through the end of December and use the one-time use code &#123;&#123;coupon_code&#125;&#125; to get &#123;&#123;discount&#125;&#125; off of your entire order!&quot;,
      &quot;example&quot;: &#123;
        &quot;body_text_named_params&quot;: [
          &#123;
            &quot;param_name&quot;: &quot;coupon_code&quot;,
            &quot;example&quot;: &quot;WINTER25&quot;
          &#125;,
          &#123;
            &quot;param_name&quot;: &quot;discount&quot;,
            &quot;example&quot;: &quot;30%&quot;
          &#125;
        ]
      &#125;
    &#125;,
    &#123;
      &quot;type&quot;: &quot;BUTTONS&quot;,
      &quot;buttons&quot;: [
        &#123;
          &quot;type&quot;: &quot;QUICK_REPLY&quot;,
          &quot;text&quot;: &quot;Unsubscribe&quot;
        &#125;,
        &#123;
          &quot;type&quot;: &quot;COPY_CODE&quot;,
          &quot;example&quot;: &quot;WINTER25&quot;
        &#125;
      ]
    &#125;
  ]
&#125;&#039;
```


### Example response

```json
&#123;
  &quot;category&quot; : &quot;MARKETING&quot;,
  &quot;id&quot; : &quot;1924084211297547&quot;,
  &quot;status&quot; : &quot;PENDING&quot;
&#125;
```

## Step 2: Send a coupon code template

Use the [Messages API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/message-api#post-version-phone-number-id-messages) to send an approved coupon template in a template message.

### Request syntax

```html
curl -X POST &quot;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;WHATSAPP_BUSINESS_PHONE_NUMBER_ID&gt;/messages&quot; \
  -H &quot;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&quot; \
  -H &quot;Content-Type: application/json&quot; \
  -d &#039;
&#123;
    &quot;messaging_product&quot;: &quot;whatsapp&quot;,
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
            &#123;
              &quot;type&quot;: &quot;text&quot;,
              &quot;parameter_name&quot;: &quot;&lt;PARAMETER_NAME&gt;&quot;,
              &quot;text&quot;: &quot;&lt;PARAMETER_VALUE&gt;&quot;
            &#125;
          ]
        &#125;,
        &#123;
          &quot;type&quot;: &quot;button&quot;,
          &quot;sub_type&quot;: &quot;copy_code&quot;,
          &quot;index&quot;: &lt;BUTTON_INDEX&gt;,
          &quot;parameters&quot;: [
            &#123;
              &quot;type&quot;: &quot;coupon_code&quot;,
              &quot;coupon_code&quot;: &quot;&lt;COUPON_CODE&gt;&quot;
            &#125;
          ]
        &#125;
      ]
    &#125;
  &#125;&#039;
```

### Request parameters

| Placeholder | Description | Example Value |
| --- | --- | --- |
| `&lt;ACCESS_TOKEN&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;[System token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#system-user-access-tokens) or [business token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#business-integration-system-user-access-tokens). | `EAAA...` |
| `&lt;API_VERSION&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Graph API version. | v25.0 |
| `&lt;WHATSAPP_BUSINESS_PHONE_NUMBER_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;WhatsApp business phone number ID. | `106540352242922` |
| `&lt;BUTTON_INDEX&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | **Required.**&lt;br&gt;&lt;br&gt;Indicates the order in which a button appears, if the template uses multiple buttons.&lt;br&gt;&lt;br&gt;Buttons are zero-indexed, so setting the value to `0` causes the button to appear first, and another button with an index of `1` appears next, and so on. | `0` |
| `&lt;COUPON_CODE&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;String to copy to device clipboard.&lt;br&gt;&lt;br&gt;Maximum 20 characters. | `WINTER25` |
| `&lt;PARAMETER_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required if template uses one or more named parameters.**&lt;br&gt;&lt;br&gt;[Named parameter](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/overview#named-parameters) name. | `coupon_code` |
| `&lt;PARAMETER_VALUE&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required if template uses one or more named parameters.**&lt;br&gt;&lt;br&gt;[Named parameter](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/overview#named-parameters) value. | `WINTER25` |
| `&lt;TEMPLATE_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Name of the template to be sent. | `winter_sale_coupon` |
| `&lt;TEMPLATE_LANGUAGE&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;The template&#039;s language and locale code. | `en_US` |
| `&lt;WHATSAPP_USER_PHONE_NUMBER&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;WhatsApp user phone number. | `+16505551234` |

### Response syntax

Upon success, the API responds with:

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
      &quot;group_id&quot;: &quot;&lt;GROUP_ID&gt;&quot;, &lt;!-- Only included if messaging a group --&gt;
      &quot;message_status&quot;: &quot;&lt;PACING_STATUS&gt;&quot; &lt;!-- Only included if sending a template --&gt;
    &#125;
  ]
&#125;
```


### Response parameters

| Placeholder | Description | Sample Value |
| --- | --- | --- |
| `&lt;GROUP_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | The string identifier of a group made using the Groups API.&lt;br&gt;&lt;br&gt;This field shows when messages are sent, received, or read from a group.&lt;br&gt;&lt;br&gt;[Learn more about the Groups API](https://developers.facebook.com/documentation/business-messaging/whatsapp/groups) | `Y2FwaV9ncm91cDoxNzA1NTU1MDEzOToxMjAzNjM0MDQ2OTQyMzM4MjAZD` |
| `&lt;PACING_STATUS&gt;`&lt;br&gt;&lt;br&gt;_String_ | Indicates [template pacing](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/template-pacing) status. The `message_status` property is only included in responses when sending a [template message](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/overview) that uses a template that is being paced. | `wamid.HBgLMTY0NjcwNDM1OTUVAgARGBI4MjZGRDA0OUE2OTQ3RkEyMzcA` |
| `&lt;WHATSAPP_USER_PHONE_NUMBER&gt;`&lt;br&gt;&lt;br&gt;_String_ | WhatsApp user&#039;s WhatsApp phone number. May not match `wa_id` value. | `+16505551234` |
| `&lt;WHATSAPP_USER_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | WhatsApp user&#039;s WhatsApp ID. May not match `input` value. | `16505551234` |
| `&lt;WHATSAPP_MESSAGE_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | WhatsApp Message ID. This ID appears in associated **messages** webhooks, such as sent, read, and delivered webhooks. | `wamid.HBgLMTY0NjcwNDM1OTUVAgARGBI4MjZGRDA0OUE2OTQ3RkEyMzcA` |

### Example request

```curl
curl &#039;https://graph.facebook.com/v25.0/106540352242922/messages&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039; \
-d &#039;
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;to&quot;: &quot;16505551234&quot;,
  &quot;type&quot;: &quot;template&quot;,
  &quot;template&quot;: &#123;
    &quot;name&quot;: &quot;winter_sale_coupon&quot;,
    &quot;language&quot;: &#123;
      &quot;code&quot;: &quot;en_US&quot;
    &#125;,
    &quot;components&quot;: [
      &#123;
        &quot;type&quot;: &quot;body&quot;,
        &quot;parameters&quot;: [
          &#123;
            &quot;type&quot;: &quot;text&quot;,
            &quot;parameter_name&quot;: &quot;coupon_code&quot;,
            &quot;text&quot;: &quot;WINTER25&quot;
          &#125;,
          &#123;
            &quot;type&quot;: &quot;text&quot;,
            &quot;parameter_name&quot;: &quot;discount&quot;,
            &quot;text&quot;: &quot;30%&quot;
          &#125;
        ]
      &#125;,
      &#123;
        &quot;type&quot;: &quot;button&quot;,
        &quot;sub_type&quot;: &quot;copy_code&quot;,
        &quot;index&quot;: 1,
        &quot;parameters&quot;: [
          &#123;
            &quot;type&quot;: &quot;coupon_code&quot;,
            &quot;coupon_code&quot;: &quot;WINTER25&quot;
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
      &quot;id&quot;: &quot;wamid.HBgLMTY1MDM4Nzk0MzkVAgARGBIxRjk1REYzMDBERDE3RUI0RDYA&quot;
    &#125;
  ]
&#125;
```
