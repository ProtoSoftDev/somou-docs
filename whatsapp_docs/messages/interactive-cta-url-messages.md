# Interactive Call-to-Action URL Button Messages



WhatsApp users may be hesitant to tap raw URLs containing lengthy or obscure strings in text messages. In these situations, send an interactive call-to-action (CTA) URL button message instead. CTA URL button messages allow you to map any URL to a button so you don&#039;t have to include the raw URL in the message body.

## Request syntax

Use the [Messages API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/message-api#post-version-phone-number-id-messages) to send an interactive CTA URL message.

```html
curl &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;WHATSAPP_BUSINESS_PHONE_NUMBER_ID&gt;/messages&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039; \
-d &#039;
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;recipient_type&quot;: &quot;individual&quot;,
  &quot;to&quot;: &quot;&lt;WHATSAPP_USER_PHONE_NUMBER&gt;&quot;,
  &quot;type&quot;: &quot;interactive&quot;,
  &quot;interactive&quot;: &#123;
    &quot;type&quot;: &quot;cta_url&quot;,

    &lt;!-- If using document header, otherwise omit --&gt;
    &quot;header&quot;: &#123;
      &quot;type&quot;: &quot;document&quot;,
      &quot;document&quot;: &#123;
        &quot;link&quot;: &quot;&lt;ASSET_URL&gt;&quot;
      &#125;
    &#125;,

    &lt;!-- If using image header, otherwise omit --&gt;
    &quot;header&quot;: &#123;
      &quot;type&quot;: &quot;image&quot;,
      &quot;image&quot;: &#123;
        &quot;link&quot;: &quot;&lt;ASSET_URL&gt;&quot;
      &#125;
    &#125;,

    &lt;!-- If using text header, otherwise omit --&gt;
    &quot;header&quot;: &#123;
      &quot;type&quot;: &quot;text&quot;,
      &quot;text&quot;: &quot;&lt;HEADER_TEXT&gt;&quot;
      &#125;
    &#125;,

    &lt;!-- If using video header, otherwise omit --&gt;
    &quot;header&quot;: &#123;
      &quot;type&quot;: &quot;video&quot;,
      &quot;video&quot;: &#123;
        &quot;link&quot;: &quot;&lt;ASSET_URL&gt;&quot;
      &#125;
    &#125;,

    &quot;body&quot;: &#123;
      &quot;text&quot;: &quot;&lt;BODY_TEXT&gt;&quot;
    &#125;,
    &quot;action&quot;: &#123;
      &quot;name&quot;: &quot;cta_url&quot;,
      &quot;parameters&quot;: &#123;
        &quot;display_text&quot;: &quot;&lt;BUTTON_LABEL_TEXT&gt;&quot;,
        &quot;url&quot;: &quot;&lt;BUTTON_URL&gt;&quot;
      &#125;
    &#125;,

    &lt;!-- If using footer text, otherwise omit --&gt;
    &quot;footer&quot;: &#123;
      &quot;text&quot;: &quot;&lt;FOOTER_TEXT&gt;&quot;
    &#125;
  &#125;
&#125;&#039;
```

## Request parameters

| Placeholder | Description | Example Value |
| --- | --- | --- |
| `&lt;ACCESS_TOKEN&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;[System token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#system-user-access-tokens) or [business token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#business-integration-system-user-access-tokens). | `EAAA...` |
| `&lt;API_VERSION&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Graph API version. | v25.0 |
| `&lt;ASSET_URL&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required if using a header with a media asset.**&lt;br&gt;&lt;br&gt;Asset URL on a public server. | `https://www.luckyshrub.com/assets/lucky-shrub-banner-logo-v1.png` |
| `&lt;BODY_TEXT&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Body text. URLs are automatically hyperlinked.&lt;br&gt;&lt;br&gt;Maximum 1024 characters. | `Tap the button below to see available dates.` |
| `&lt;BUTTON_LABEL_TEXT&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Button label text. Must be unique if using multiple buttons.&lt;br&gt;&lt;br&gt;Maximum 20 characters. | `See Dates` |
| `&lt;BUTTON_URL&gt;` | **Required.**&lt;br&gt;&lt;br&gt;URL to load in the device&#039;s default web browser when the WhatsApp user taps the button. | `https://www.luckyshrub.com?clickID=kqDGWd24Q5TRwoEQTICY7W1JKoXvaZOXWAS7h1P76s0R7Paec4` |
| `&lt;FOOTER_TEXT&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required if using a footer.**&lt;br&gt;&lt;br&gt;Footer text. URLs are automatically hyperlinked.&lt;br&gt;&lt;br&gt;Maximum 60 characters. | `Dates subject to change.` |
| `&lt;HEADER_TEXT&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required if using a text header.**&lt;br&gt;&lt;br&gt;Header text.&lt;br&gt;&lt;br&gt;Maximum 60 characters. | `New workshop dates announced!` |
| `&lt;WHATSAPP_BUSINESS_PHONE_NUMBER_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;WhatsApp business phone number ID. | `106540352242922` |
| `&lt;WHATSAPP_USER_PHONE_NUMBER&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;WhatsApp user phone number. | `+16505551234` |

## Example request

```curl
curl &#039;https://graph.facebook.com/v25.0/106540352242922/messages&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039; \
-d &#039;
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;recipient_type&quot;: &quot;individual&quot;,
  &quot;to&quot;: &quot;+16505551234&quot;,
  &quot;type&quot;: &quot;interactive&quot;,
  &quot;interactive&quot;: &#123;
    &quot;type&quot;: &quot;cta_url&quot;,
    &quot;header&quot;: &#123;
      &quot;type&quot;: &quot;image&quot;,
      &quot;image&quot;: &#123;
        &quot;link&quot;: &quot;https://www.luckyshrub.com/assets/lucky-shrub-banner-logo-v1.png&quot;
      &#125;
    &#125;,
    &quot;body&quot;: &#123;
      &quot;text&quot;: &quot;Tap the button below to see available dates.&quot;
    &#125;,
    &quot;action&quot;: &#123;
      &quot;name&quot;: &quot;cta_url&quot;,
      &quot;parameters&quot;: &#123;
        &quot;display_text&quot;: &quot;See Dates&quot;,
        &quot;url&quot;: &quot;https://www.luckyshrub.com?clickID=kqDGWd24Q5TRwoEQTICY7W1JKoXvaZOXWAS7h1P76s0R7Paec4&quot;
      &#125;
    &#125;,
    &quot;footer&quot;: &#123;
      &quot;text&quot;: &quot;Dates subject to change.&quot;
    &#125;
  &#125;
&#125;&#039;
```

## Example response

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
      &quot;id&quot;: &quot;wamid.HBgLMTY0NjcwNDM1OTUVAgARGBI1RjQyNUE3NEYxMzAzMzQ5MkEA&quot;
    &#125;
  ]
&#125;
```

