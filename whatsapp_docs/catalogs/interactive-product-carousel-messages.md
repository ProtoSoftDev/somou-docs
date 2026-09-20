# Interactive product carousel messages


The interactive product carousel message enables businesses to send horizontally scrollable product cards within WhatsApp conversations, allowing WhatsApp users to browse and engage with products directly in-thread.

The product carousel message integrates with the Product Catalog and supports Single Product Message (SPM) actions on each card, letting WhatsApp users browse and select products without leaving the conversation, through the API and mobile clients.

## How to build a product carousel message

The product carousel message contains a `card` object. You must add two card objects to your message, and can add a maximum of 10. Each card exists in a `cards[]` array and must be given a `&quot;card_index&quot;` value of `0` through `9`.

The type of each card must be set to `&quot;product&quot;`, and each card must reference the same `&quot;catalog_id&quot;`.

You must add a message body to the message. Do not add a header, footer, or buttons.

Lastly, each card must specify the product and catalog identifiers `&quot;product_retailer_id&quot;` and `&quot;catalog_id&quot;`.

### The `card` object

```html
...
&#123;
  &quot;card_index&quot;: 0,
  &quot;type&quot;: &quot;product&quot;,
  &quot;action&quot;: &#123;
    &quot;product_retailer_id&quot;: &quot;abc123xyz&quot;,
    &quot;catalog_id&quot;: &quot;123456789&quot;
&#125;
...
```

## Request syntax

```html
curl &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;WHATSAPP_BUSINESS_PHONE_NUMBER_ID&gt;/messages&#039; \
  -H &#039;Content-Type: application/json&#039; \
  -H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039; \
  -d &#039;&#123;
    &quot;messaging_product&quot;: &quot;whatsapp&quot;,
    &quot;recipient_type&quot;: &quot;individual&quot;,
    &quot;to&quot;: &quot;&lt;WHATSAPP_USER_PHONE_NUMBER&gt;&quot;,
    &quot;type&quot;: &quot;interactive&quot;, // must be interactive
    &quot;interactive&quot;: &#123;
      &quot;type&quot;: &quot;carousel&quot;, // must be carousel
      &quot;body&quot;: &#123;
        &quot;text&quot;: &quot;&lt;MESSAGE_BODY_TEXT&gt;&quot;
      &#125;,
      &quot;action&quot;: &#123;
        &quot;cards&quot;: [
          &#123;
            &quot;card_index&quot;: 0,
            &quot;type&quot;: &quot;product&quot;,
            &quot;action&quot;: &#123;
              &quot;product_retailer_id&quot;: &quot;abc123xyz&quot;,
              &quot;catalog_id&quot;: &quot;123456789&quot;
            &#125;
          &#125;
          // additional product cards
        ]
      &#125;
    &#125;
  &#125;&#039;
```

## Request parameters

| Placeholder | Description | Sample value |
| --- | --- | --- |
| `&lt;ACCESS_TOKEN&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;[System token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#system-user-access-tokens) or [business token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#business-integration-system-user-access-tokens). | `EAAA...` |
| `&lt;API_VERSION&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Graph API version. | v25.0 |
| `&lt;MESSAGE_BODY_TEXT&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Maximum 1024 characters. | `Which option do you prefer?` |
| `&lt;WHATSAPP_BUSINESS_PHONE_NUMBER_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;WhatsApp business phone number ID. | `106540352242922` |
| `&lt;WHATSAPP_USER_PHONE_NUMBER&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;WhatsApp user phone number. | `+16505551234` |

## Card object parameters

```html
...
&#123;
  &quot;card_index&quot;: &lt;INDEX&gt;,
  &quot;type&quot;: &quot;product&quot;,
  &quot;action&quot;: &#123;
    &quot;product_retailer_id&quot;: &quot;&lt;PRODUCT_RETAILER_ID&gt;&quot;,
    &quot;catalog_id&quot;: &quot;&lt;CATALOG_ID&gt;&quot;
&#125;
...
```

| Placeholder | Description | Sample value |
| --- | --- | --- |
| `&lt;INDEX&gt;` &lt;br&gt; _Integer_ | **Required** &lt;br&gt; Unique index for each card (0-9). Must not repeat within the message. | `2` |
| `&lt;PRODUCT_RETAILER_ID&gt;` &lt;br&gt; _String_ | **Required** &lt;br&gt; The unique retailer ID of the product in the catalog. | `&quot;0JkSUu4qizuXv&quot;` |
| `&lt;CATALOG_ID&gt;` &lt;br&gt; _String_ | **Required** &lt;br&gt; The unique ID of the catalog containing the product. | `&quot;Lq1ZtoWL5OkljTerAW&quot;` |

## Example request

```json
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;recipient_type&quot;: &quot;individual&quot;,
  &quot;to&quot;: &quot;1234567890&quot;,
  &quot;type&quot;: &quot;interactive&quot;,
  &quot;interactive&quot;: &#123;
    &quot;type&quot;: &quot;carousel&quot;,
    &quot;body&quot;: &#123;
      &quot;text&quot;: &quot;Check out our featured products!&quot;
    &#125;,
    &quot;action&quot;: &#123;
      &quot;cards&quot;: [
        &#123;
          &quot;card_index&quot;: 0,
          &quot;type&quot;: &quot;product&quot;,
          &quot;action&quot;: &#123;
            &quot;product_retailer_id&quot;: &quot;abc123xyz&quot;,
            &quot;catalog_id&quot;: &quot;123456789&quot;
          &#125;
        &#125;,
        &#123;
          &quot;card_index&quot;: 1,
          &quot;type&quot;: &quot;product&quot;,
          &quot;action&quot;: &#123;
            &quot;product_retailer_id&quot;: &quot;def456uvw&quot;,
            &quot;catalog_id&quot;: &quot;123456789&quot;
          &#125;
        &#125;
      ]
    &#125;
  &#125;
&#125;
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
