# Experiments



WhatsApp occasionally runs experiments to assess the impact of messaging on WhatsApp user experience and engagement. These are standard practices on the platform and help improve the messaging experience for both you and your customers.

Ongoing experiments are described below and have no fixed end-date. To protect the validity of the experiment and ensure the best possible business and consumer experience, no exceptions or opt-outs are available for these experiments.

## Marketing message experiment

A very small percentage of WhatsApp users do not receive marketing template messages from any business unless one of the following conditions is met:

* a [customer service window](https://developers.facebook.com/documentation/business-messaging/whatsapp/messages/send-messages#customer-service-windows) exists between the user and the business
* an open [free entry point window](https://developers.facebook.com/documentation/business-messaging/whatsapp/pricing#free-entry-point-windows) exists between the user and the business

If you send a marketing template message to a user who is part of the experiment group, WhatsApp does not send your message and does not bill you for it. Instead, a [status messages](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/messages/status) webhook triggers with `status` set to `failed` and error `code` set to `130472`.

Attempting to resend the message will result in the same error. If you must deliver the marketing template message to the user, contact the user by some other, non-WhatsApp means, and ask them to message you so you can resend the message within an open customer service window.
