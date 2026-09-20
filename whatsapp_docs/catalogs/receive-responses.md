# Receive responses from customers



After receiving single- or multi-product messages, WhatsApp users can ask for more information about a product or place an order. WhatsApp sends these information requests and orders through the [messages](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/messages) webhook.

## Sent message status

The [status messages webhook](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/messages/status) reports sent message statuses (sent, delivered, read).

## Asking for information

Whenever a WhatsApp user receives a single- or multi-product message, they can ask for more information by sending you a text message in an existing WhatsApp thread, or by tapping a **Message business** or **Message** button when viewing a specific product.

The [text messages webhook](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/messages/text) describes messages sent after a user taps **Message business** or **Message**, and includes a `context` property whose value is an object describing the product the user was viewing when they tapped the button.

## Orders

When a WhatsApp user adds one or more products to their WhatsApp shopping cart and places an order, WhatsApp triggers the [order messages webhook](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/messages/order), which describes the contents of the order. Use the order contents to fulfill the order.
