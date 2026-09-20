# Set commerce settings



You can enable or disable the shopping cart and the product catalog on a per-business phone number basis. By default, the shopping cart is enabled and the storefront icon is hidden for all business phone numbers associated with a WhatsApp Business account.

## Get business phone numbers

Use the [Phone Numbers API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/phone-number-management-api) to get a list of all business phone numbers associated with a WhatsApp Business account.

## Enable or disable cart

Use the [Commerce Settings API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/phone-number-api) to enable or disable the shopping cart for a specific business phone number.

When enabled, cart-related buttons appear in the chat, catalog, and product details views:

When the cart is disabled, customers can see products and their details, but cart-related buttons do not appear in any view.

### Request syntax

```html
POST /&lt;BUSINESS_PHONE_NUMBER_ID&gt;/whatsapp_commerce_settings
  ?is_cart_enabled=&lt;IS_CART_ENABLED&gt;
```

### Parameters

| Placeholder | Sample Value | Description |
| --- | --- | --- |
| `&lt;BUSINESS_PHONE_NUMBER_ID&gt;` | `106850078877666` | Business phone number ID. |
| `&lt;IS_CART_ENABLED&gt;` | `true` | Boolean. Set to `true` to enable cart or `false` to disable it. Default value is `true`. |

### Sample request

```html
curl -X POST &#039;https://graph.facebook.com/v25.0/106850078877666/whatsapp_commerce_settings?is_cart_enabled=true&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039;
```

### Sample response

```json
&#123;
  &quot;success&quot;: true
&#125;
```

## Enable or disable catalog

Use the [Commerce Settings API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/phone-number-api) to enable or disable the product catalog for a specific business phone number.

When enabled, the catalog storefront icon and catalog-related buttons appear in chat views and business profile views:

When the catalog is disabled, the storefront icon and catalog-related buttons do not appear in any views and the catalog preview with thumbnails does not appear in the business profile view.

**Note:** If you disable the catalog, wa.me links to your catalog, as well as the **View catalog** button that appears when you send your catalog link in a message will display an **Invalid catalog link** warning when tapped.

### Request syntax

```html
POST /&lt;BUSINESS_PHONE_NUMBER_ID&gt;/whatsapp_commerce_settings
  ?is_catalog_visible=&lt;IS_CATALOG_VISIBLE&gt;
```

### Parameters

| Placeholder | Sample Value | Description |
| --- | --- | --- |
| `&lt;BUSINESS_PHONE_NUMBER_ID&gt;` | `106850078877666` | Business phone number ID. |
| `&lt;IS_CATALOG_VISIBLE&gt;` | `true` | Boolean. Set to `true` to show catalog storefront icon or `false` to hide it. Default value is `false`. |

### Sample request

```html
curl -X POST &#039;https://graph.facebook.com/v25.0/106850078877666/whatsapp_commerce_settings?is_catalog_visible=true&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039;
```

### Sample response

```json
&#123;
  &quot;success&quot;: true
&#125;
```

## Get commerce settings

Use the [Commerce Settings API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/phone-number-api) to get an individual business phone number&#039;s commerce settings.

### Request syntax

```html
GET /&lt;BUSINESS_PHONE_NUMBER_ID&gt;/whatsapp_commerce_settings
```

### Parameters

| Placeholder | Sample Value | Description |
| --- | --- | --- |
| `&lt;BUSINESS_PHONE_NUMBER_ID&gt;` | `106850078877666` | Business phone number ID. |

### Sample request

```html
curl -X GET &#039;https://graph.facebook.com/v25.0/106850078877666/whatsapp_commerce_settings&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039;
```

### Sample response

```json
&#123;
  &quot;data&quot;: [
    &#123;
      &quot;is_cart_enabled&quot;: true,
      &quot;is_catalog_visible&quot;: true,
      &quot;id&quot;: &quot;727705352028726&quot;
    &#125;
  ]
&#125;
```
