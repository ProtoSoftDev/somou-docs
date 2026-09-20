# Supported features and limits


**Note:** The Direct Send API is in beta. Features and behavior described here are subject to change and may be released incrementally. Participation is subject to acceptance of the beta terms.

This page summarizes what the Direct Send API supports during beta.

## Supported message formats

| Category | Support | Features |
|----------|---------|----------|
| Utility messages, per [Meta&#039;s category guidelines](https://developers.facebook.com/docs/whatsapp/updates-to-pricing/new-template-guidelines) | ✅ Supported | Text messages · Interactive call-to-action URL button messages · Interactive reply button messages · Interactive messages with mixed call-to-action URL and reply buttons (up to 10 total, max 2 CTA) · Custom time-to-live (TTL) · Image, video, and document message headers |
| Authentication messages, per [Meta&#039;s category guidelines](https://developers.facebook.com/docs/whatsapp/updates-to-pricing/new-template-guidelines) | ✅ Supported | Text messages |
| Marketing messages | ❌ Not supported | All other button formats · Address, audio, contacts, location, sticker, and reaction messages · Other media formats |

&gt; **Note.** This list is exhaustive for the beta. Any message format or feature not listed here is not yet supported.

&gt; Image, video, and document headers are access-restricted during beta. See [Media message headers](https://developers.facebook.com/documentation/business-messaging/whatsapp/direct-send/media-headers).

## Supported languages

All [WhatsApp Cloud API languages](https://developers.facebook.com/docs/whatsapp/business-management-api/message-templates/supported-languages) are supported.

See the FAQ for [what happens with an unsupported language](https://developers.facebook.com/documentation/business-messaging/whatsapp/direct-send/faq).

## Message format limits

Direct Send aligns with the message-length limits for business-initiated template messages:

- **Body text:** 1024 characters
- **Header:** 60 characters
- **Footer:** 60 characters
- **Button text:** 20 characters

Button counts:

- **Quick reply buttons:** 10 (max)
- **Call-to-action (CTA) buttons:** 2 (max)

## Message throughput

Direct Send supports standard Cloud API throughput, per [Cloud API throughput eligibility](https://developers.facebook.com/docs/whatsapp/throughput).
