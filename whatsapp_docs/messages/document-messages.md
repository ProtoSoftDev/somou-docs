# Document messages



Document messages are messages that display a document icon, linked to a document, that a WhatsApp user can tap to download.

## Request syntax

Use the [Messages API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/message-api#post-version-phone-number-id-messages) to send a document message to a WhatsApp user.

```html
curl &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;WHATSAPP_BUSINESS_PHONE_NUMBER_ID&gt;/messages&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039; \
-d &#039;
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;recipient_type&quot;: &quot;individual&quot;,
  &quot;to&quot;: &quot;&lt;WHATSAPP_USER_PHONE_NUMBER&gt;&quot;,
  &quot;type&quot;: &quot;document&quot;,
  &quot;document&quot;: &#123;
    &quot;id&quot;: &quot;&lt;MEDIA_ID&gt;&quot;, &lt;!-- Only if using uploaded media --&gt;
    &quot;link&quot;: &quot;&lt;MEDIA_URL&gt;&quot;, &lt;!-- Only if using hosted media (not recommended) --&gt;
    &quot;caption&quot;: &quot;&lt;MEDIA_CAPTION_TEXT&gt;&quot;,
    &quot;filename&quot;: &quot;&lt;MEDIA_FILENAME&gt;&quot;,
    &quot;caption&quot;: &quot;&lt;MEDIA_CAPTION_TEXT&gt;&quot;
  &#125;
&#125;&#039;
```

## Request parameters

| Placeholder | Description | Example Value |
| --- | --- | --- |
| `&lt;ACCESS_TOKEN&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;[System token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#system-user-access-tokens) or [business token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#business-integration-system-user-access-tokens). | `EAAA...` |
| `&lt;API_VERSION&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Graph API version. | v25.0 |
| `&lt;MEDIA_CAPTION_TEXT&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Media asset caption text.&lt;br&gt;&lt;br&gt;Maximum 1024 characters. | `Lucky Shrub Invoice` |
| `&lt;MEDIA_FILENAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Document filename, with extension. The WhatsApp client will use an appropriate file type icon based on the extension. | `lucky-shrub-invoice.pdf` |
| `&lt;MEDIA_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required if using uploaded media, otherwise omit.**&lt;br&gt;&lt;br&gt;ID of the [uploaded media asset](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-phone-numbers/media#upload-media). | `1013859600285441` |
| `&lt;MEDIA_URL&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required if using hosted media, otherwise omit.**&lt;br&gt;&lt;br&gt;URL of the media asset hosted on your public server. For better performance, we recommend using `id` and an [uploaded media asset ID](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-phone-numbers/media#upload-media) instead. | `https://www.luckyshrub.com/invoices/FmOzfD9cKf/lucky-shrub-invoice.pdf` |
| `&lt;WHATSAPP_BUSINESS_PHONE_NUMBER_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;WhatsApp business phone number ID. | `106540352242922` |
| `&lt;WHATSAPP_USER_PHONE_NUMBER&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;WhatsApp user phone number. | `+16505551234` |

## Supported document types

| Document Type | Extension | MIME Type | Max Size |
| --- | --- | --- | --- |
| Text | .txt | text/plain | 100 MB |
| Microsoft Excel | .xls | application/vnd.ms-excel | 100 MB |
| Microsoft Excel | .xlsx | application/vnd.openxmlformats-officedocument.spreadsheetml.sheet | 100 MB |
| Microsoft Word | .doc | application/msword | 100 MB |
| Microsoft Word | .docx | application/vnd.openxmlformats-officedocument.wordprocessingml.document | 100 MB |
| Microsoft PowerPoint | .ppt | application/vnd.ms-powerpoint | 100 MB |
| Microsoft PowerPoint | .pptx | application/vnd.openxmlformats-officedocument.presentationml.presentation | 100 MB |
| PDF | .pdf | application/pdf | 100 MB |

Only the above listed document types are officially supported and guaranteed to display correctly in the WhatsApp client. Other file types may be sent via the API, but they are not supported and may not be handled as expected.

## Example request

Example request to send a PDF in a document message with a caption to a WhatsApp user.

```curl
curl &#039;https://graph.facebook.com/v25.0/106540352242922/messages&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039; \
-d &#039;
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;recipient_type&quot;: &quot;individual&quot;,
  &quot;to&quot;: &quot;+16505551234&quot;,
  &quot;type&quot;: &quot;document&quot;,
  &quot;document&quot;: &#123;
    &quot;id&quot;: &quot;1376223850470843&quot;,
    &quot;filename&quot;: &quot;order_abc123.pdf&quot;,
    &quot;caption&quot;: &quot;Your order confirmation (PDF)&quot;
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

