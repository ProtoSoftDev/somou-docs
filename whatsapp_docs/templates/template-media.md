# Template media



A media header allows you to add an image, video, GIF, or document at the top of your WhatsApp template message.

Before creating the template, you must upload your media file using the Resumable Upload API. This upload returns a media ID, which you then use as the value for the `header_handle` field in the template&#039;s header component.

## Create a template with a media header

### Request syntax

Use the [Message Templates API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/message-template-api#post-version-waba-id-message-templates) to create a template with a media header.

```html
curl -X POST \
  &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;WABA_ID&gt;/message_templates&#039; \
  -H &#039;Authorization: Bearer ACCESS_TOKEN&#039; \
  -H &#039;Content-Type: application/json&#039; \
  -d &#039;&#123;
    &quot;name&quot;: &quot;limited_time_offer_tuscan_getaway_2023&quot;,
    &quot;language&quot;: &quot;en_US&quot;,
    &quot;category&quot;: &quot;MARKETING&quot;,
    &quot;components&quot;: [
      &#123;
        &quot;type&quot;: &quot;HEADER&quot;,
        &quot;format&quot;: &quot;IMAGE&quot;,
        &quot;example&quot;: &#123;
          &quot;header_handle&quot;: [
            &quot;4::aW...&quot;
          ]
        &#125;
      &#125;
    ]
  &#125;&#039;
```

## Send media-based message template

Use the [Messages API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/message-api#post-version-phone-number-id-messages) to send a media-based template message. Set the `type` property to `template` and use the template property to define your template object and its media object.

When defining your media object, you can either [upload your media asset](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-phone-numbers/media#upload-media) to our servers and use its media ID (using the `id` property), or host the asset on your server and use its URL (using the `link` property). If you&#039;re using link, your asset must be on a publicly accessible server or the message will fail to send.

To reduce the likelihood of errors and avoid unnecessary requests to your public server, Meta recommends that you upload your media assets and use their IDs when sending messages.

You can also cache media assets. See [Media HTTP Caching](https://developers.facebook.com/documentation/business-messaging/whatsapp/messages/send-messages#media-http-caching).

### Request syntax

```html
curl -X  POST \
 &#039;https://graph.facebook.com/v23.0/FROM_PHONE_NUMBER_ID/messages&#039; \
 -H &#039;Authorization: Bearer ACCESS_TOKEN&#039; \
 -H &#039;Content-Type: application/json&#039; \
 -d &#039;&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;recipient_type&quot;: &quot;individual&quot;,
  &quot;to&quot;: &quot;PHONE_NUMBER&quot;,
  &quot;type&quot;: &quot;template&quot;,
  &quot;template&quot;: &#123;
    &quot;name&quot;: &quot;TEMPLATE_NAME&quot;,
    &quot;language&quot;: &#123;
      &quot;code&quot;: &quot;LANGUAGE_AND_LOCALE_CODE&quot;
    &#125;,
    &quot;components&quot;: [
      &#123;
        &quot;type&quot;: &quot;header&quot;,
        &quot;parameters&quot;: [
          &#123;
            &quot;type&quot;: &quot;image&quot;,
            &quot;image&quot;: &#123;
              &quot;link&quot;: &quot;https://URL&quot;
            &#125;
          &#125;
        ]
      &#125;,
      &#123;
        &quot;type&quot;: &quot;body&quot;,
        &quot;parameters&quot;: [
          &#123;
            &quot;type&quot;: &quot;text&quot;,
            &quot;text&quot;: &quot;TEXT-STRING&quot;
          &#125;,
          &#123;
            &quot;type&quot;: &quot;currency&quot;,
            &quot;currency&quot;: &#123;
              &quot;fallback_value&quot;: &quot;VALUE&quot;,
              &quot;code&quot;: &quot;USD&quot;,
              &quot;amount_1000&quot;: NUMBER
            &#125;
          &#125;,
          &#123;
            &quot;type&quot;: &quot;date_time&quot;,
            &quot;date_time&quot;: &#123;
              &quot;fallback_value&quot;: &quot;MONTH DAY, YEAR&quot;
            &#125;
          &#125;
        ]
      &#125;
    ]
  &#125;
&#125;&#039;
```

A successful response includes an object with an identifier prefixed with WAM id. Use the ID listed after wamid to track your message status.

```curl
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;contacts&quot;: [&#123;
      &quot;input&quot;: &quot;PHONE_NUMBER&quot;,
      &quot;wa_id&quot;: &quot;WHATSAPP_ID&quot;,
    &#125;]
  &quot;messages&quot;: [&#123;
      &quot;id&quot;: &quot;wamid.ID&quot;,
    &#125;]
&#125;
```
