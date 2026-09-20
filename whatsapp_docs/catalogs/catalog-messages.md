# Catalog messages


Catalog messages let you showcase your product catalog entirely within WhatsApp.

Catalog messages display a product thumbnail header image of your choice, custom body text, a fixed text header, a fixed text sub-header, and a **View catalog** button.

When a customer taps the **View catalog** button, your product catalog appears within WhatsApp.

## Requirements

You must have [inventory uploaded to Meta](https://developers.facebook.com/documentation/business-messaging/whatsapp/catalogs/upload-inventory) in an ecommerce catalog [connected to your WhatsApp Business account](https://www.facebook.com/business/help/158662536425974).

## Request syntax

Use the [Messages API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/message-api#post-version-phone-number-id-messages) to send a catalog message.

```json
POST /&lt;WHATSAPP_BUSINESS_PHONE_NUMBER_ID&gt;/messages
```

## Post body

```json
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;recipient_type&quot;: &quot;individual&quot;,
  &quot;to&quot;: &quot;&lt;TO&gt;&quot;,
  &quot;type&quot;: &quot;interactive&quot;,
  &quot;interactive&quot; : &#123;
    &quot;type&quot; : &quot;catalog_message&quot;,
    &quot;body&quot; : &#123;
      &quot;text&quot;: &quot;&lt;BODY_TEXT&gt;&quot;
    &#125;,
    &quot;action&quot;: &#123;
      &quot;name&quot;: &quot;catalog_message&quot;,

      /* Parameters object is optional */
      &quot;parameters&quot;: &#123;
        &quot;thumbnail_product_retailer_id&quot;: &quot;&lt;THUMBNAIL_PRODUCT_RETAILER_ID&gt;&quot;
      &#125;
    &#125;,

    /* Footer object is optional */
    &quot;footer&quot;: &#123;
      &quot;text&quot;: &quot;&lt;FOOTER_TEXT&gt;&quot;
  &#125;
&#125;
```

## Properties

| Placeholder | Description | Sample Value |
| --- | --- | --- |
| `&lt;BODY_TEXT&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Text to appear in the message body.&lt;br&gt;&lt;br&gt;Maximum 1024 characters. | `Hello! Thanks for your interest. Ordering is easy. Just visit our catalog and add items to purchase.` |
| `&lt;FOOTER_TEXT&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Text to appear in the message footer.&lt;br&gt;&lt;br&gt;Maximum 60 characters. | `Best grocery deals on WhatsApp!` |
| `&lt;THUMBNAIL_PRODUCT_RETAILER_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Item SKU number. Labeled as **Content ID** in the Commerce Manager.&lt;br&gt;&lt;br&gt;WhatsApp uses the thumbnail of this item as the message&#039;s header image.&lt;br&gt;&lt;br&gt;If you omit the `parameters` object, WhatsApp uses the product image of the first item in your catalog. | `2lc20305pt` |
| `&lt;TO&gt;`&lt;br&gt;&lt;br&gt;_String_ | Customer phone number. | `+16505551234` |

## Example request

```curl
curl &#039;https://graph.facebook.com/v17.0/106540352242922/messages&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039; \
-d &#039;
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;recipient_type&quot;: &quot;individual&quot;,
  &quot;to&quot;: &quot;+16505551234&quot;,
  &quot;type&quot;: &quot;interactive&quot;,
  &quot;interactive&quot;: &#123;
    &quot;type&quot;: &quot;catalog_message&quot;,
    &quot;body&quot;: &#123;
      &quot;text&quot;: &quot;Hello! Thanks for your interest. Ordering is easy. Just visit our catalog and add items to purchase.&quot;
    &#125;,
    &quot;action&quot;: &#123;
      &quot;name&quot;: &quot;catalog_message&quot;,
      &quot;parameters&quot;: &#123;
        &quot;thumbnail_product_retailer_id&quot;: &quot;2lc20305pt&quot;
      &#125;
    &#125;,
    &quot;footer&quot;: &#123;
      &quot;text&quot;: &quot;Best grocery deals on WhatsApp!&quot;
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
      &quot;id&quot;: &quot;wamid.HBgLMTY1MDM4Nzk0MzkVAgARGBI0ODVEREUwQzEzQkVBRjQ1RUUA&quot;
    &#125;
  ]
&#125;
```
