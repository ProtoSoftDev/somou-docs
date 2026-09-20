# Direct Send API FAQ


**Note:** The Direct Send API is in beta. Features and behavior described here are subject to change and may be released incrementally. Participation is subject to acceptance of the beta terms.

## How does Direct Send work with the session-based pricing model?

Sending a Direct Send utility message follows the same session logic as sending a utility template message. When you send a Direct Send utility message, the system checks whether an open utility conversation already exists between you and the WhatsApp user:

- **If an open utility conversation exists:** no new conversation is started, and the message is delivered within the existing session.
- **If no open utility conversation exists:** a new utility conversation opens and lasts 24 hours from the first message.

**Note:** utility conversations are separate from service conversations and are billed independently. [Learn more about pricing on the WhatsApp Business Platform](https://developers.facebook.com/docs/whatsapp/pricing).

## I received a warning email about misusing utility messages as marketing messages. Can I request a review?

Yes. If you believe the action was taken in error, request a review by emailing [wadirectsendapisupport&#064;meta.com](mailto:wadirectsendapisupport&#064;meta.com). Include:

- The names and/or IDs of the auto-created template(s) flagged for non-utility content.
- You can find affected template names with this request:

```html
GET /&lt;WABA_ID&gt;/message_templates?source=AUTO_GENERATED&amp;correct_category=MARKETING
```

The support team will review your request and guide you on next steps.

## If a template pause notification doesn&#039;t include template information, how do I identify which messages will be blocked?

The [pause webhook](https://developers.facebook.com/docs/whatsapp/business-management-api/webhooks/components#template-paused) includes both the template name and language. Use that template name to view the paused template&#039;s content:

```html
GET /&lt;WABA_ID&gt;/message_templates?source=AUTO_GENERATED&amp;name=&lt;TEMPLATE_NAME&gt;
```

Messages that match or closely resemble the paused template&#039;s content are blocked during the specified time window.

## Why does Direct Send auto-generate templates?

Templates are created so that you keep access to insights and granular troubleshooting, helping ensure messages sent through Direct Send comply with utility category guidelines.

## What happens if I send a message in an unsupported language?

The API still works, but Direct Send defaults to the onboarding templates instead of creating proper templates. As a result, those messages won&#039;t have proper templates with classification, integrity checks, or metrics.

## How are auto-generated templates cleaned up?

Auto-generated templates are cleaned up in two phases:

1. Templates created but never used to send a message are deleted after 24 hours.
2. Templates previously used but inactive for a while (based on a configurable threshold) are archived periodically.

## What happens if the template_name conflicts with an existing template?

If a template with the same name already exists but was not created by Direct Send, you receive error code `132021` asynchronously via the error webhook. Choose a different `template_name`. The name must be unique within your WABA, and non–Direct Send templates can&#039;t be used to send Direct Send messages.

## Do template_name templates fall back to onboarding templates?

No. Unlike the standard Direct Send flow, `template_name` templates do **not** fall back to onboarding templates. If template creation fails, the message is retried up to 3 times. If all retries are exhausted, you receive error code `131000` via the error webhook.

## Can I use the same template_name across different WABAs?

Yes. The `template_name` uniqueness constraint is per-WABA, so you can reuse the same name in different WhatsApp Business Accounts without conflict.

## What are the naming rules for template_name?

The `template_name` must use only lowercase alphanumeric characters and underscores (regex: `^[a-z0-9_]+$`) and must not exceed 512 characters. If the format is invalid, you receive a synchronous error code `100` in the API response.
