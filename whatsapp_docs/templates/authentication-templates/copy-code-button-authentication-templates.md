# Copy code authentication templates



Copy code authentication templates allow you to send a one-time password or code along with a copy code button to your users. When a WhatsApp user taps the copy code button, the WhatsApp client copies the password or code to the device&#039;s clipboard. The user can then switch to your app and paste the password or code into your app.

**Note:** Effective June 15, 2026, on iOS 26 and later, copy code templates will also trigger [Keyboard suggestions](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/authentication-templates/keyboard-suggestions), giving the WhatsApp user a one-tap autofill option from the push notification in addition to the copy button.

Note: The &quot;I didn&#039;t request a code&quot; button is currently in beta and is being rolled out incrementally to business customers.

Copy code button authentication templates consist of:

* Preset text: _&lt;VERIFICATION_CODE&gt; is your verification code._
* An optional security disclaimer: _For your security, do not share this code._
* An optional expiration warning: _This code expires in &lt;NUM_MINUTES&gt; minutes._
* A copy code button.

## Limitations

URLs, media, and emojis are not supported.

## Creating authentication templates

Use the [Message Templates API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/message-template-api#post-version-waba-id-message-templates) to create authentication templates.

### Request syntax

```json
curl &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;/message_templates&#039; \
  -H &#039;Content-Type: application/json&#039; \
  -H &#039;Authorization: Bearer EAAJB...&#039; \
  -d &#039;
&#123;
    &quot;name&quot;: &quot;&lt;TEMPLATE_NAME&gt;&quot;,
    &quot;language&quot;: &quot;&lt;TEMPLATE_LANGUAGE&gt;&quot;,
    &quot;category&quot;: &quot;authentication&quot;,
    &quot;message_send_ttl_seconds&quot;: &lt;TIME_TO_LIVE&gt;,  // Optional
    &quot;components&quot;: [
      &#123;
        &quot;type&quot;: &quot;body&quot;,
        &quot;add_security_recommendation&quot;: &lt;SECURITY_RECOMMENDATION&gt;  // Optional
      &#125;,
      &#123;
        &quot;type&quot;: &quot;footer&quot;,
        &quot;code_expiration_minutes&quot;: &lt;CODE_EXPIRATION&gt;  // Optional
      &#125;,
      &#123;
        &quot;type&quot;: &quot;buttons&quot;,
        &quot;buttons&quot;: [
          &#123;
            &quot;type&quot;: &quot;otp&quot;,
            &quot;otp_type&quot;: &quot;copy_code&quot;,
            &quot;text&quot;: &quot;&lt;COPY_CODE_BUTTON_TEXT&gt;&quot;  // Optional
          &#125;
        ]
      &#125;
    ]
  &#125;&#039;
```

In your template creation request, you designate the button `type` as `OTP`, but WhatsApp sets the button `type` to `URL` upon creation. To confirm the type change, perform a GET request on a newly created authentication template and analyze its components.

### Request parameters

| Placeholder | Description | Example Value |
| --- | --- | --- |
| `&lt;CODE_EXPIRATION&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | **Optional.**&lt;br&gt;&lt;br&gt;Indicates the number of minutes the password or code is valid.&lt;br&gt;&lt;br&gt;If included, the delivered message displays the code expiration warning and this value.&lt;br&gt;&lt;br&gt;If omitted, the delivered message does not display the code expiration warning.&lt;br&gt;&lt;br&gt;Minimum 1, maximum 90. | `5` |
| `&lt;COPY_CODE_BUTTON_TEXT&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Copy code button label text.&lt;br&gt;&lt;br&gt;If omitted, the text will default to a pre-set value localized to the template&#039;s language. For example, `Copy Code` for English (US).&lt;br&gt;&lt;br&gt;Maximum 25 characters. | `Copy Code` |
| `&lt;SECURITY_RECOMMENDATION&gt;`&lt;br&gt;&lt;br&gt;_Boolean_ | **Optional.**&lt;br&gt;&lt;br&gt;Set to `true` if you want the template to include the string, _For your security, do not share this code._ Set to `false` to exclude the string. | `true` |
| `&lt;TEMPLATE_LANGUAGE&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Template [language and locale code](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/supported-languages). | `en_US` |
| `&lt;TEMPLATE_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Template name.&lt;br&gt;&lt;br&gt;Maximum 512 characters. | `verification_code` |
| `&lt;TIME_TO_LIVE&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | **Optional.**&lt;br&gt;&lt;br&gt;Authentication message time-to-live value, in seconds. See [Time-To-Live](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/authentication-templates/authentication-templates#time-to-live) below. | `60` |

### Example request

```json
curl &#039;https://graph.facebook.com/v25.0/102290129340398/message_templates&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039; \
-d &#039;
&#123;
  &quot;name&quot;: &quot;authentication_code_copy_code_button&quot;,
  &quot;language&quot;: &quot;en_US&quot;,
  &quot;category&quot;: &quot;authentication&quot;,
  &quot;message_send_ttl_seconds&quot;: 60,
  &quot;components&quot;: [
    &#123;
      &quot;type&quot;: &quot;body&quot;,
      &quot;add_security_recommendation&quot;: true
    &#125;,
    &#123;
      &quot;type&quot;: &quot;footer&quot;,
      &quot;code_expiration_minutes&quot;: 5
    &#125;,
    &#123;
      &quot;type&quot;: &quot;buttons&quot;,
      &quot;buttons&quot;: [
        &#123;
          &quot;type&quot;: &quot;otp&quot;,
          &quot;otp_type&quot;: &quot;copy_code&quot;,
          &quot;text&quot;: &quot;Copy Code&quot;
        &#125;
      ]
    &#125;
  ]
&#125;&#039;
```

### Example response

```json
&#123;
  &quot;id&quot;: &quot;594425479261596&quot;,
  &quot;status&quot;: &quot;PENDING&quot;,
  &quot;category&quot;: &quot;AUTHENTICATION&quot;
&#125;
```

## Webhooks

The [button messages webhook](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/messages/button) is triggered whenever a user taps the &quot;I didn&#039;t request a code&quot; button within the message.

### Example webhook

```html
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;320580347795883&quot;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;messaging_product&quot;: &quot;whatsapp&quot;,
            &quot;metadata&quot;: &#123;
              &quot;display_phone_number&quot;: &quot;12345678&quot;,
              &quot;phone_number_id&quot;: &quot;1234567890&quot;
            &#125;,
            &quot;contacts&quot;: [
              &#123;
                &quot;profile&quot;: &#123;
                  &quot;name&quot;: &quot;John&quot;
                &#125;,
                &quot;wa_id&quot;: &quot;12345678&quot;
              &#125;
            ],
            &quot;messages&quot;: [
              &#123;
                &quot;context&quot;: &#123;
                  &quot;from&quot;: &quot;12345678&quot;,
                  &quot;id&quot;: &quot;wamid.HBgLMTIxMTU1NTE0NTYVAgARGBJDMDEyMTFDNTE5NkFCOUU3QTEA&quot;
                &#125;,
                &quot;from&quot;: &quot;12345678&quot;,
                &quot;id&quot;: &quot;wamid.HBgLMTIxMTU1NTE0NTYVAgASGCBBQ0I3MjdCNUUzMTE0QjhFQkM4RkQ4MEU3QkE0MUNEMgA=&quot;,
                &quot;timestamp&quot;: &quot;1753919111&quot;,
                &quot;from_logical_id&quot;: &quot;131063108133020&quot;,
                &quot;type&quot;: &quot;button&quot;,
                &quot;button&quot;: &#123;
                  &quot;payload&quot;: &quot;DID_NOT_REQUEST_CODE&quot;,
                  &quot;text&quot;: &quot;I didn&#039;t request a code&quot;
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


## Sample app

See our [WhatsApp One-Time Password (OTP) Sample App](https://github.com/WhatsApp/WhatsApp-OTP-Sample-App) for Android on GitHub. The sample app demonstrates how to send and receive one-time passwords (OTPs) and codes via the API, how to integrate the one-tap autofill and copy code buttons, how to create a template, and how to start a sample server.

## Sending authentication templates

This document explains how to send approved [authentication templates with one-time password buttons](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/authentication-templates/authentication-templates).

Note that **you must first initiate a handshake** between your app and the WhatsApp client. See [Handshake](#handshake) above.

### Request syntax

```json
curl -X POST &quot;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;WHATSAPP_BUSINESS_PHONE_NUMBER_ID&gt;/messages&quot; \
  -H &quot;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&quot; \
  -H &quot;Content-Type: application/json&quot; \
  -d &#039;
&#123;
    &quot;messaging_product&quot;: &quot;whatsapp&quot;,
    &quot;recipient_type&quot;: &quot;individual&quot;,
    &quot;to&quot;: &quot;&lt;CUSTOMER_PHONE_NUMBER&gt;&quot;,
    &quot;type&quot;: &quot;template&quot;,
    &quot;template&quot;: &#123;
      &quot;name&quot;: &quot;&lt;TEMPLATE_NAME&gt;&quot;,
      &quot;language&quot;: &#123;
        &quot;code&quot;: &quot;&lt;TEMPLATE_LANGUAGE_CODE&gt;&quot;
      &#125;,
      &quot;components&quot;: [
        &#123;
          &quot;type&quot;: &quot;body&quot;,
          &quot;parameters&quot;: [
            &#123;
              &quot;type&quot;: &quot;text&quot;,
              &quot;text&quot;: &quot;&lt;ONE-TIME PASSWORD&gt;&quot;
            &#125;
          ]
        &#125;,
        &#123;
          &quot;type&quot;: &quot;button&quot;,
          &quot;sub_type&quot;: &quot;url&quot;,
          &quot;index&quot;: &quot;0&quot;,
          &quot;parameters&quot;: [
            &#123;
              &quot;type&quot;: &quot;text&quot;,
              &quot;text&quot;: &quot;&lt;ONE-TIME PASSWORD&gt;&quot;
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
| `&lt;CUSTOMER_PHONE_NUMBER&gt;` | The WhatsApp user&#039;s phone number. | `12015553931` |
| `&lt;ONE-TIME PASSWORD&gt;` | The one-time password or verification code that WhatsApp delivers to the customer.&lt;br&gt;&lt;br&gt;Note that this value must appear twice in the payload.&lt;br&gt;&lt;br&gt;Maximum 15 characters. | `J$FpnYnP` |
| `&lt;TEMPLATE_LANGUAGE_CODE&gt;` | The template&#039;s [language and locale code](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/supported-languages). | `en_US` |
| `&lt;TEMPLATE_NAME&gt;` | The template&#039;s name. | `verification_code` |

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

| Placeholder | Description | Example Value |
| --- | --- | --- |
| `&lt;INPUT&gt;`&lt;br&gt;&lt;br&gt;_String_ | The WhatsApp user&#039;s phone number that the message was sent to. This may not match `wa_id`. | `+16315551234` |
| `&lt;WA_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | WhatsApp ID of the WhatsApp user the message was sent to. This may not match `input`. | `+16315551234` |
| `&lt;ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | WhatsApp message ID. You can use the ID listed after &quot;wamid.&quot; to track your message status. | `wamid.HBgLMTY1MDM4Nzk0MzkVAgARGBI3N0EyQUJDMjFEQzZCQUMzODMA` |

### Example request

```curl
curl -L &#039;https://graph.facebook.com/v25.0/105954558954427/messages&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039; \
-d &#039;&#123;
      &quot;messaging_product&quot;: &quot;whatsapp&quot;,
      &quot;recipient_type&quot;: &quot;individual&quot;,
      &quot;to&quot;: &quot;12015553931&quot;,
      &quot;type&quot;: &quot;template&quot;,
      &quot;template&quot;: &#123;
        &quot;name&quot;: &quot;verification_code&quot;,
        &quot;language&quot;: &#123;
          &quot;code&quot;: &quot;en_US&quot;
      &#125;,
      &quot;components&quot;: [
        &#123;
          &quot;type&quot;: &quot;body&quot;,
          &quot;parameters&quot;: [
            &#123;
              &quot;type&quot;: &quot;text&quot;,
              &quot;text&quot;: &quot;J$FpnYnP&quot;
            &#125;
          ]
        &#125;,
        &#123;
          &quot;type&quot;: &quot;button&quot;,
          &quot;sub_type&quot;: &quot;url&quot;,
          &quot;index&quot;: &quot;0&quot;,
          &quot;parameters&quot;: [
            &#123;
              &quot;type&quot;: &quot;text&quot;,
              &quot;text&quot;: &quot;J$FpnYnP&quot;
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
      &quot;input&quot;: &quot;12015553931&quot;,
      &quot;wa_id&quot;: &quot;12015553931&quot;
    &#125;
  ],
  &quot;messages&quot;: [
    &#123;
      &quot;id&quot;: &quot;wamid.HBgLMTY1MDM4Nzk0MzkVAgARGBI4Qzc5QkNGNTc5NTMyMDU5QzEA&quot;
    &#125;
  ]
&#125;
```
