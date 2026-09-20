# Single-product messages


Single-product messages are interactive messages that display a single product from your catalog, allowing WhatsApp users to view product details, add the item to a cart, and send an order — all within WhatsApp.

*Single-product message example:*

*Product detail page example:*

## Overview

WhatsApp users who receive single-product messages can perform three main actions:

1. View the product: Whenever a customer clicks on the item, the product&#039;s latest info is fetched and the product displays in a Product Detail Page (PDP) format. Currently, PDPs only support product images — any videos or GIFs added to the product won&#039;t be displayed in the PDP.
1. Add the product to a cart: Whenever a user adds a product to the shopping cart, the item&#039;s latest info is fetched. If there has been a state change, a dialog saying &quot;One or more items in your cart have been updated&quot; is displayed — see [Product updates](https://developers.facebook.com/documentation/business-messaging/whatsapp/catalogs/share-products#product-updates) for more information. A cart persists in a chat thread between you and your customer until the cart is sent to you — see [Shopping cart experience](https://developers.facebook.com/documentation/business-messaging/whatsapp/catalogs/share-products#shopping-cart-experience) for details.
1. Send a shopping cart to you: After adding items, customers can send their cart to you. After that, you can define the next steps, such as requesting delivery info or giving payment options.

If your customer has multiple devices linked to their account, single-product messages are synced between devices. However, the shopping cart is local to each specific device. See [Shopping cart experience](https://developers.facebook.com/documentation/business-messaging/whatsapp/catalogs/share-products#shopping-cart-experience) for details.

Currently, single-product messages can be received on the following platforms:

- iOS: 2.21.210
- Android: 2.21.19
- Web: The web client supports this feature.

If the customer&#039;s app version does not support single-product messages, they instead receive a message explaining that they were unable to receive a message because they are using an outdated version of WhatsApp. You also receive a webhook notification indicating the message was unable to be delivered due to the customer using an outdated version of WhatsApp.

## Expected behavior

Single-product messages can be:

* Forwarded by one user to another.
* Reopened by a user within the same chat thread.

Single-product messages cannot be:

* Sent as notifications. They can only be sent as part of existing chat threads.

## Use cases

Single-product messages are best for guiding WhatsApp users to one specific item from your inventory, offering quick responses from a limited set of options, such as:

- Responding to a customer&#039;s specific request.
- Providing a recommendation.
- Reordering a previous item.

Single-product messages can also be used as part of a human agent flow. However, you need to build the tooling to allow the human agent to generate a single-product message in thread.

### Why you should use them

Single-product messages work best for user experiences that are simple and personalized, where it&#039;s a better experience to guide the customer to a specific item most relevant to them, rather than browsing your full inventory.

#### No templates

Interactive messages do not require templates or pre-approvals. They are generated in real-time and always reflect the latest item details, pricing, and stock levels from your inventory.

## Send a single-product message

Before sending product messages, follow the get started best suited for your needs:

- [Direct developers](https://developers.facebook.com/documentation/business-messaging/whatsapp/get-started)
- [Partners](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/overview)

All API calls mentioned in this guide must be authenticated with an access token. You can authenticate your API calls with the access token generated in the **App Dashboard** &gt; **WhatsApp** &gt; **API Setup** panel. If you are a partner, you must authenticate with an access token with the [whatsapp_business_messaging](https://developers.facebook.com/docs/permissions/reference/whatsapp_business_messaging) permission.

### Step 1: Assemble the interactive object

To send a single-product message, assemble an `interactive` object of type `product` with the following components:

| Required components | Optional components |
| --- | --- |
| - Action object — Must include both catalog_id and product_retailer_id. | - Body object&lt;br&gt;- Footer object |

See [Messages, Interactive Object](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/message-api) for full information. By the end of the process, the interactive object should look something like this:

```json
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;recipient_type&quot;: &quot;individual&quot;,
  &quot;to&quot;: &quot;PHONE_NUMBER&quot;,
  &quot;type&quot;: &quot;interactive&quot;,
  &quot;interactive&quot;: &#123;
    &quot;type&quot;: &quot;product&quot;,
    &quot;body&quot;: &#123;
      &quot;text&quot;: &quot;BODY_TEXT&quot;
    &#125;,
    &quot;footer&quot;: &#123;
      &quot;text&quot;: &quot;FOOTER_TEXT&quot;
    &#125;,
    &quot;action&quot;: &#123;
      &quot;catalog_id&quot;: &quot;CATALOG_ID&quot;,
      &quot;product_retailer_id&quot;: &quot;ID_TEST_ITEM_1&quot;
    &#125;
  &#125;
&#125;
```

If none of the items provided in the API call matches a product from your product catalog, an error message is sent and the single-product message is not sent to the user.

### Step 2: Add common message parameters

Once the interactive object is complete, append the other parameters that make a message: `recipient_type`, `to`, `messaging_product`, and `type`. Remember to set the `type` to `interactive`.

```curl
curl -X  POST https://graph.facebook.com/v25.0/FROM_PHONE_NUMBER/messages \
 -H &#039;Authorization: Bearer ACCESS_TOKEN&#039; \
 - d &#039;&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;recipient_type&quot;: &quot;individual&quot;,
  &quot;to&quot;: &quot;PHONE_NUMBER&quot;,
  &quot;type&quot;: &quot;interactive&quot;,
  &quot;interactive&quot;: &#123;
  // INTERACTIVE OBJECT GOES HERE
&#125;&#039;
```

For all available parameters, see [Reference, Messages](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/message-api).

### Step 3: Send the message

Use the [Messages API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/message-api#post-version-phone-number-id-messages) to send the JSON object you have assembled in steps 1 and 2. If your message is sent successfully, you get the following response:

```json
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;contacts&quot;: [&#123;
      &quot;input&quot;: &quot;PHONE_NUMBER&quot;,
      &quot;wa_id&quot;: &quot;WHATSAPP_ID&quot;
    &#125;],
  &quot;messages&quot;: [&#123;
      &quot;id&quot;: &quot;wamid.ID&quot;
    &#125;]
&#125;
```
