# Send utility and authentication messages


**Note:** The Direct Send API is in beta. Features and behavior described here are subject to change and may be released incrementally. Participation is subject to acceptance of the beta terms.

To send a utility or authentication message with Direct Send, call the `POST /&lt;WHATSAPP_BUSINESS_PHONE_NUMBER_ID&gt;/messages` endpoint and add the `category` field to the message body.

## Request syntax

```html
POST /&lt;WHATSAPP_BUSINESS_PHONE_NUMBER_ID&gt;/messages
```

## Example request

```json
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;recipient_type&quot;: &quot;individual&quot;,
  &quot;to&quot;: &quot;&lt;WHATSAPP_USER_PHONE_NUMBER&gt;&quot;,
  &quot;type&quot;: &quot;text&quot;,
  &quot;text&quot;: &#123;
    &quot;body&quot;: &quot;&lt;BODY_TEXT&gt;&quot;
  &#125;,
  &quot;category&quot;: &quot;utility&quot;
&#125;
```

To send an authentication message instead, set `&quot;category&quot;: &quot;authentication&quot;` (access restricted — see [Supported values for the category field](#supported-values-for-the-category-field)).

If you call the endpoint **without** the `category` field, the request follows normal [Cloud API send-message behavior](https://developers.facebook.com/docs/whatsapp/cloud-api/guides/send-messages) for free-form messages.

&gt; **Note.** Direct Send uses the standard Cloud API recipient model: address messages with `to` (phone number) or `recipient` (business-scoped user ID). Authentication-category messages can&#039;t be sent to a business-scoped user ID — use `to` with a phone number. See [Business-scoped user IDs](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-scoped-user-ids/) for availability and details.

## Supported values for the category field

The `category` field indicates the category of the message to send as a template.

| Value | Description |
| --- | --- |
| `utility` | Business-initiated message in the utility category. Charged at utility rates. |
| `authentication` | Business-initiated message in the authentication category. Charged at authentication rates.&lt;br&gt;&lt;br&gt;**Access restricted** — contact your partner or client manager to get access. |
| `service` _(or omitted)_ | Service message that follows the existing service-message flow; the message is dropped if the service window isn&#039;t open. Omitting the `category` field is equivalent to sending `category: &quot;service&quot;` — both follow the existing [Cloud API send behavior](https://developers.facebook.com/docs/whatsapp/cloud-api/guides/send-messages). |

## Sending a message with an incorrect category value

If you send an unsupported value (for example, `marketing`), the API returns a synchronous error:

```json
&#123;
  &quot;error&quot;: &#123;
    &quot;message&quot;: &quot;(#100) Param category must be one of &#123;AUTHENTICATION, SERVICE, UTILITY&#125; - got \&quot;marketing\&quot;.&quot;,
    &quot;type&quot;: &quot;OAuthException&quot;,
    &quot;code&quot;: 100,
    &quot;fbtrace_id&quot;: &quot;A2ExvorNrGRh-8NPprvmPop&quot;
  &#125;
&#125;
```

## Related

- [Supported message types](https://developers.facebook.com/documentation/business-messaging/whatsapp/direct-send/supported-message-types) — text, CTA URL, reply, and mixed-button messages
- [Business-named templates](https://developers.facebook.com/documentation/business-messaging/whatsapp/direct-send/business-named-templates) — control template attribution with `template_name`
- [Configure message TTL](https://developers.facebook.com/documentation/business-messaging/whatsapp/direct-send/configure-message-ttl)
- [Error codes](https://developers.facebook.com/documentation/business-messaging/whatsapp/direct-send/api-reference)
