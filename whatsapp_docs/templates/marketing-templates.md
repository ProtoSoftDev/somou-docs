# Marketing templates



Marketing templates are typically used to drive engagement, brand awareness, and sales.
They are the only type of template that can be used with both Cloud API and Marketing Messages API for WhatsApp.

## Custom templates

You can use the [Message Templates API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/message-template-api#post-version-waba-id-message-templates) to create custom marketing templates that suit your business needs from any supported components.

See our [custom marketing templates](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/marketing-templates/custom-marketing-templates) document for example of how to create and send a custom template, and which components are supported.

## Specialty templates

Some templates use a special component that requires or excludes additional components, or require additional configuration. These are described below.

### Catalog templates

[Catalog templates](https://developers.facebook.com/documentation/business-messaging/whatsapp/catalogs/catalog-template-messages) are marketing templates that allow you to showcase your product catalog entirely within WhatsApp.

### Coupon code templates

[Coupon code templates](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/marketing-templates/coupon-templates) are marketing templates that display a single copy code button. When tapped, the code is copied to the customer&#039;s clipboard.

### Media card carousel templates

[Media card carousel templates](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/marketing-templates/media-card-carousel-templates) allow you to send a single text message accompanied by a set of up to 10 media cards in a horizontally scrollable view.

### Call permission request templates

[Call permission request templates](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/marketing-templates/call-permission-request-message-template) allow your business to call your customers outside of the customer service window.

### Limited-time offer templates

[Limited-time offer templates](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/marketing-templates/limited-time-offer-templates) allow you to display expiration dates and running countdown timers for offer codes in template messages, making it easy for you to communicate time-bound offers and drive customer engagement.

### Multi-product message templates

[Multi-product message templates](https://developers.facebook.com/documentation/business-messaging/whatsapp/catalogs/mpm-template-messages) allow you to showcase up to 30 products from your ecommerce catalog, organized in up to 10 sections, in a single message.

### Product card carousel templates

[Product card carousel templates](https://developers.facebook.com/documentation/business-messaging/whatsapp/catalogs/product-card-carousel-template-messages) allow you to send a single text message accompanied by a set of up to 10 product cards in a horizontally scrollable view:

### Single-product message templates

[Single-product message templates](https://developers.facebook.com/documentation/business-messaging/whatsapp/catalogs/spm-template-messages) allow you to send a single text message accompanied by a set of up to 10 product cards in a horizontally scrollable view:

## Per-user marketing template message limits

WhatsApp may limit the number of marketing template messages a person receives from any business in a given period of time, starting with delivering fewer marketing conversations to those users who are less likely to engage with them. In most WhatsApp markets, this is determined based on a number of factors, including a dynamic view of an individual&#039;s marketing message read rate.

Refer to [per-user marketing template message limits](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/marketing-templates/per-user-limits) document for more information.

## User preferences for marketing messages

WhatsApp provides a setting, **Offers and announcements**, that allows WhatsApp users to indicate their interest level in marketing messages sent from your business, and to stop or resume delivery of marketing messages from your business entirely.

### Interested/not interested feedback

WhatsApp users can use the **Offers and announcements** setting to indicate how interested they are in receiving marketing template messages from your business.

If a user chooses **Not interested**, it can affect [per-user marketing template messaging limits](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/marketing-templates/per-user-limits) between you and the user. Choosing this option also displays the second modal, which gives the user the option to stop delivery of marketing messages from your business.

&gt; **Note:** Interested and Not interested feedback does not trigger the [user_preferences webhook](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/user_preferences). Only stop and resume actions trigger the webhook.

### Stop/resume controls

WhatsApp users can use the **Offers and announcements** setting to stop or resume delivery of marketing template messages from your business.

If you attempt to send a marketing template to a WhatsApp user who has stopped marketing template messages from your business, the API will process the request but not send the message. Instead, the API will trigger a [status messages webhook](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/messages/status) with:

* `status` set to `failed`,
* `code` set to `131050`,
* `title` set to `Unable to deliver the message. This recipient has chosen to stop receiving marketing messages on WhatsApp from your business`

To be notified whenever a WhatsApp user stops or resumes delivery of marketing template messages from your business, subscribe to the [user_preferences webhook](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/user_preferences).

## For accounts linked with the WhatsApp Business app

To improve the experience for WhatsApp Business Users, marketing messages sent to business customers will not bump chat threads to the top of the inbox. These messages will still be delivered and visible to business customers, but the thread will only move to the top if the customer responds.

Note: This feature is currently limited and not generally available (GA) to all users.
