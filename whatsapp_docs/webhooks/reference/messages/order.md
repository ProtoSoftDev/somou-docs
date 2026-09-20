# Order messages webhook reference



This reference describes trigger events and payload contents for the WhatsApp Business account **messages** webhook for order messages.

## Triggers

- A WhatsApp user orders one or more products via a [catalog, single-, or multi-product message](https://developers.facebook.com/documentation/business-messaging/whatsapp/catalogs/catalogs-overview).

## Syntax

```html
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;&quot;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;messaging_product&quot;: &quot;whatsapp&quot;,
            &quot;metadata&quot;: &#123;
              &quot;display_phone_number&quot;: &quot;&lt;BUSINESS_DISPLAY_PHONE_NUMBER&gt;&quot;,
              &quot;phone_number_id&quot;: &quot;&lt;BUSINESS_PHONE_NUMBER_ID&gt;&quot;
            &#125;,
            &quot;contacts&quot;: [
              &#123;
                &quot;profile&quot;: &#123;
                  &quot;name&quot;: &quot;&lt;WHATSAPP_USER_PROFILE_NAME&gt;&quot;
                &#125;,
                &quot;wa_id&quot;: &quot;&lt;WHATSAPP_USER_ID&gt;&quot;,
                &quot;identity_key_hash&quot;: &quot;&lt;IDENTITY_KEY_HASH&gt;&quot; &lt;!-- only included if identity change check enabled --&gt;
              &#125;
            ],
            &quot;messages&quot;: [
              &#123;
                &quot;from&quot;: &quot;&lt;WHATSAPP_USER_PHONE_NUMBER&gt;&quot;,
                &quot;id&quot;: &quot;&lt;WHATSAPP_MESSAGE_ID&gt;&quot;,
                &quot;timestamp&quot;: &quot;&lt;WEBHOOK_TRIGGER_TIMESTAMP&gt;&quot;,
                &quot;type&quot;: &quot;order&quot;,
                &quot;order&quot;: &#123;
                  &quot;catalog_id&quot;: &quot;&lt;PRODUCT_CATALOG_ID&gt;&quot;,
                  &quot;text&quot;: &quot;&lt;ORDER_TEXT&gt;&quot;,
                  &quot;product_items&quot;: [
                    &#123;
                      &quot;product_retailer_id&quot;: &quot;&lt;PRODUCT_ID&gt;&quot;,
                      &quot;quantity&quot;: &lt;PRODUCT_QUANTITY&gt;,
                      &quot;item_price&quot;: &lt;PRODUCT_PRICE&gt;,
                      &quot;currency&quot;: &quot;&lt;CURRENCY_CODE&gt;&quot;
                    &#125;
                  ]
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

## Parameters

| Placeholder | Description | Example value |
| --- | --- | --- |
| `&lt;BUSINESS_DISPLAY_PHONE_NUMBER&gt;`&lt;br&gt;&lt;br&gt;_String_ | Business display phone number. | `15550783881` |
| `&lt;BUSINESS_PHONE_NUMBER_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | Business phone number ID. | `106540352242922` |
| `&lt;CURRENCY_CODE&gt;`&lt;br&gt;&lt;br&gt;_String_ | Catalog currency code. | `USD` |
| `&lt;IDENTITY_KEY_HASH&gt;`&lt;br&gt;&lt;br&gt;_String_ | Identity key hash. Only included if you have enabled the [identity change check](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-phone-numbers/phone-numbers) feature. | `DF2lS5v2W6x=` |
| `&lt;ORDER_TEXT&gt;`&lt;br&gt;&lt;br&gt;_String_ | Text accompanying the order. | `Love these!` |
| `&lt;PRODUCT_CATALOG_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | [Product catalog ID](https://developers.facebook.com/documentation/business-messaging/whatsapp/catalogs/catalogs-overview). | `194836987003835` |
| `&lt;PRODUCT_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | [Product ID](https://developers.facebook.com/documentation/business-messaging/whatsapp/catalogs/catalogs-overview). | `di9ozbzfi4` |
| `&lt;PRODUCT_PRICE&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | Individual product price. | `7.99` |
| `&lt;PRODUCT_QUANTITY&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | Product quantity. | `2` |
| `&lt;WEBHOOK_TRIGGER_TIMESTAMP&gt;`&lt;br&gt;&lt;br&gt;_String_ | Unix timestamp indicating when the webhook was triggered. | `1739321024` |
| `&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | WhatsApp Business Account ID. | `102290129340398` |
| `&lt;WHATSAPP_MESSAGE_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | WhatsApp message ID. | `wamid.HBgLMTY1MDM4Nzk0MzkVAgASGBQzQUFERjg0NDEzNDdFODU3MUMxMAA=` |
| `&lt;WHATSAPP_USER_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | WhatsApp user ID. Note that a WhatsApp user&#039;s ID and phone number may not always match. | `16505551234` |
| `&lt;WHATSAPP_USER_PHONE_NUMBER&gt;`&lt;br&gt;&lt;br&gt;_String_ | WhatsApp user phone number. This is the same value returned by the API as the `input` value when sending a message to a WhatsApp user. Note that a WhatsApp user&#039;s phone number and ID may not always match. | `16505551234` |
| `&lt;WHATSAPP_USER_PROFILE_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | WhatsApp user&#039;s name as it appears in their profile in the WhatsApp client. | `Sheena Nelson` |

## Example

This example webhook describes an order placed by a WhatsApp user for 3 products via an interactive catalog message.

```json
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;102290129340398&quot;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;messaging_product&quot;: &quot;whatsapp&quot;,
            &quot;metadata&quot;: &#123;
              &quot;display_phone_number&quot;: &quot;15550783881&quot;,
              &quot;phone_number_id&quot;: &quot;106540352242922&quot;
            &#125;,
            &quot;contacts&quot;: [
              &#123;
                &quot;profile&quot;: &#123;
                  &quot;name&quot;: &quot;Sheena Nelson&quot;
                &#125;,
                &quot;wa_id&quot;: &quot;16505551234&quot;
              &#125;
            ],
            &quot;messages&quot;: [
              &#123;
                &quot;from&quot;: &quot;16505551234&quot;,
                &quot;id&quot;: &quot;wamid.HBgLMTY1MDM4Nzk0MzkVAgASGBQzQUFERjg0NDEzNDdFODU3MUMxMAA=&quot;,
                &quot;timestamp&quot;: &quot;1750096325&quot;,
                &quot;type&quot;: &quot;order&quot;,
                &quot;order&quot;: &#123;
                  &quot;catalog_id&quot;: &quot;194836987003835&quot;,
                  &quot;text&quot;: &quot;Love these!&quot;,
                  &quot;product_items&quot;: [
                    &#123;
                      &quot;product_retailer_id&quot;: &quot;di9ozbzfi4&quot;,
                      &quot;quantity&quot;: 2,
                      &quot;item_price&quot;: 30,
                      &quot;currency&quot;: &quot;USD&quot;
                    &#125;,
                    &#123;
                      &quot;product_retailer_id&quot;: &quot;nqryix03ez&quot;,
                      &quot;quantity&quot;: 1,
                      &quot;item_price&quot;: 25,
                      &quot;currency&quot;: &quot;USD&quot;
                    &#125;
                  ]
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
