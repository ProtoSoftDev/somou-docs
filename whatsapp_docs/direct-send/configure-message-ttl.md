# Configure message time-to-live (TTL)


**Note:** The Direct Send API is in beta. Features and behavior described here are subject to change and may be released incrementally. Participation is subject to acceptance of the beta terms.

&gt; **Note.** Custom time-to-live (TTL) is only supported with Direct Send.

You can configure TTL when calling `POST /&lt;WHATSAPP_BUSINESS_PHONE_NUMBER_ID&gt;/messages` by adding a `ttl_seconds` field below the `category` field.

## Example request

```html
POST /&lt;WHATSAPP_BUSINESS_PHONE_NUMBER_ID&gt;/messages

&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;recipient_type&quot;: &quot;individual&quot;,
  &quot;to&quot;: &quot;&lt;WHATSAPP_USER_PHONE_NUMBER&gt;&quot;,
  &quot;type&quot;: &quot;text&quot;,
  &quot;text&quot;: &#123;
    &quot;body&quot;: &quot;&lt;BODY_TEXT&gt;&quot;
  &#125;,
  &quot;category&quot;: &quot;utility&quot;,
  &quot;ttl_seconds&quot;: 600
&#125;
```

&gt; **Note.** TTL works with both utility and authentication messages. Set `category` to the value you need. Defaults and limits differ by category — see the table below.

## TTL defaults and limits

TTL defaults and limits depend on the message `category`. Direct Send supports `utility` and `authentication` messages.

| Category | Default | Minimum | Maximum |
|----------|---------|---------|---------|
| `utility` | 30 days | 30 seconds | 43200 seconds (12 hours) |
| `authentication` | 600 seconds (10 minutes) | 30 seconds | 900 seconds (15 minutes) |

Authentication messages use a much shorter default and maximum than utility messages, because authentication codes are time-sensitive.

## Success response

When a message with TTL is sent successfully, you receive the normal message-success response:

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

## Failure response

Messages configured with TTL can fail when:

- You use TTL on a message that isn&#039;t a Direct Send message.
- The TTL expires before the message can be delivered.
- The TTL value is too high or too low.

| Case | Response |
|------|----------|
| TTL set on a non-Direct Send message | Returns [error code 100 &quot;Invalid parameter&quot;](https://developers.facebook.com/docs/whatsapp/cloud-api/support/error-codes/#other-errors) — &quot;The request included one or more unsupported or misspelled parameters.&quot; |
| Message undeliverable within TTL | The message is dropped. |
| TTL value above the category maximum | Returns [error code 100](https://developers.facebook.com/docs/whatsapp/cloud-api/support/error-codes/#other-errors) — &quot;The time to live value must be lower than or equal to `&lt;MAX&gt;`&quot;, where `&lt;MAX&gt;` is the category maximum (43200 for utility, 900 for authentication). |
| TTL value below the category minimum | Returns [error code 100](https://developers.facebook.com/docs/whatsapp/cloud-api/support/error-codes/#other-errors) — &quot;The time to live value must be higher than or equal to 30.&quot; |

## Related

- [Error codes](https://developers.facebook.com/documentation/business-messaging/whatsapp/direct-send/api-reference)
