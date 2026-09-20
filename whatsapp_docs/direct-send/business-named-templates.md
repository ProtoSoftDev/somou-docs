# Business-named templates


**Note:** The Direct Send API is in beta. Features and behavior described here are subject to change and may be released incrementally. Participation is subject to acceptance of the beta terms.

By default, Direct Send automatically matches each message to a generated template. To make message-to-template attribution predictable — and to integrate more easily with existing systems — you can specify the template name on the send call.

&gt; **Access restricted.** Contact your Meta representative to get access to this feature.

When you provide a name, Direct Send creates (or reuses) a template with that exact name, and all messages using that name are attributed to it.

&gt; **Note.** Business-named templates are supported only for utility messages (`category: &quot;utility&quot;`). The `authentication` category isn&#039;t supported at this time.

## Using direct_send_config.template_name

Add a `direct_send_config` object with a `template_name` field to your Direct Send request.

| Field | Description | Example value |
| --- | --- | --- |
| `direct_send_config`&lt;br&gt;&lt;br&gt;_object_ | **Optional.**&lt;br&gt;&lt;br&gt;Configuration object for Direct Send features. | — |
| `direct_send_config.template_name`&lt;br&gt;&lt;br&gt;_string_ | **Optional.**&lt;br&gt;&lt;br&gt;A unique name for the template. When provided, Direct Send creates or reuses a template with this exact name instead of auto-matching. | `order_shipment_update` |

### Validation rules

- **Format:** lowercase alphanumeric characters and underscores only (`^[a-z0-9_]+$`).
- **Maximum length:** 512 characters.
- **Uniqueness:** must be unique within your WABA. If a template with the same name already exists but was **not** created by Direct Send (for example, created manually via the Business Management API), Direct Send returns error code `132021` asynchronously via the error webhook.

## Example request

```json
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;recipient_type&quot;: &quot;individual&quot;,
  &quot;to&quot;: &quot;&lt;WHATSAPP_USER_PHONE_NUMBER&gt;&quot;,
  &quot;type&quot;: &quot;text&quot;,
  &quot;text&quot;: &#123;
    &quot;body&quot;: &quot;Hi Jane, your order #12345 has been shipped and is expected to arrive on March 20.&quot;
  &#125;,
  &quot;category&quot;: &quot;utility&quot;,
  &quot;direct_send_config&quot;: &#123;
    &quot;template_name&quot;: &quot;order_shipment_update&quot;
  &#125;
&#125;
```

This example uses a text message; other message formats can use `direct_send_config` the same way.

## Behavior with and without template_name

| `direct_send_config` | Behavior |
|----------------------|----------|
| **Present** with `template_name` | Direct Send creates or reuses a template with the specified name. No fallback to placeholder templates. |
| **Not present** | Automatic template matching, with fallback to onboarding templates. |

### No fallback

Unlike the standard Direct Send flow, templates created with `template_name` **do not** fall back to onboarding templates. If template creation fails, the message is retried up to 3 times. If all retries are exhausted, a generic infrastructure-failure webhook is sent (error code `131000` — &quot;Something went wrong&quot;).

## Related

- [View generated templates](https://developers.facebook.com/documentation/business-messaging/whatsapp/direct-send/view-generated-templates)
- [FAQ: template_name conflicts, cross-WABA reuse, and naming rules](https://developers.facebook.com/documentation/business-messaging/whatsapp/direct-send/faq)
