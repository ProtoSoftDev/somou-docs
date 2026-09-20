# Pricing for non-template messages


**Warning:** This page explains upcoming pricing updates for the Meta Business Platform. To understand how we currently charge, please see [Pricing on the WhatsApp Business Platform](https://developers.facebook.com/documentation/business-messaging/whatsapp/pricing).

You can respond to WhatsApp users with non-template messages. Non-template messages can only be sent in an open 24-hour [customer service window](https://developers.facebook.com/documentation/business-messaging/whatsapp/messages/send-messages#customer-service-windows), which opens and resets with each user message.

### Summary of timeline

The following timeline summarizes the launch and pricing updates covered on this page:

- **As of July 1, 2026**, Meta launches the Meta Business Agent Platform. Any partner or directly integrated client can integrate with the platform using APIs.
- **Effective August 1, 2026**, Meta will charge all businesses for Meta Business Agent messages, with monthly invoices issued at the end of the billing period.
- **Effective October 1, 2026**, Meta will charge for service messages, which have not been charged since November 2024.
- **Effective October 1, 2026**, Meta will charge for utility messages sent in response to users within an open 24-hour customer service window. These messages have not been charged since July 1, 2025.

### Message categories for non-template messages

Today, non-template message responses to users are all service messages (`category: service`). With the introduction of Meta Business Agent, non-template messages can be one of two categories: Service (existing category) or Meta Business Agent (new category).

- **Meta Business Agent messages** – These are powered by the Meta Business Agent Platform. This will be a new message category on our platform.
- **Service messages** – As today, these can be powered by a person, like a customer service representative, or by a 3rd-party AI solution. This is an existing message category on our platform. Any non-template message that is not powered by Meta Business Agent is a service message.

### Pricing updates

In H2 2026, Meta will charge for non-template messages sent in response to users, based on the category of the message:

- **Effective August 1, 2026**, Meta will charge on a per-token basis for Meta Business Agent messages. Charges reflect the token consumption to ingest the user&#039;s message (the prompt) and to generate a response.
- **Effective October 1, 2026**, Meta will charge on a per-message basis for service messages, consistent with how Meta charges for template messages.
- **Effective October 1, 2026**, Meta will charge on a per-message basis for utility messages sent in response to users within an open 24-hour customer service window. These messages have not been charged since July 1, 2025.

Meta applies one charge for Meta Business Agent messages. Businesses that use a third-party AI solution typically incur separate charges for AI usage from the AI provider and message delivery from the messaging solution provider.

The following table summarizes how Meta charges across message types and categories.

| Business objective | Message type | Who sends the message | Message category | Charge status | Meter | Charge for AI usage | Charge for message delivery | Free in 72-hour free entry point window | Free in 24-hour customer service window |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Reach out to users | Template | Business triggers delivery | Marketing | Live | Per message | N/A | Yes, by Meta | Yes | No |
| Reach out to users | Template | Business triggers delivery | Utility | Live | Per message | N/A | Yes, by Meta | Yes | Free until October 1, 2026 |
| Reach out to users | Template | Business triggers delivery | Authentication | Live | Per message | N/A | Yes, by Meta | Yes | No |
| Respond to users | Non-template | Person or third-party AI agent | Service | As of October 1, 2026 | Per message | Yes, by the third-party AI provider, if applicable | Yes, by Meta | Yes | Free until October 1, 2026 |
| Respond to users | Non-template | Meta Business Agent Platform | Meta Business Agent | As of August 1, 2026 | Per token | Yes, by Meta as one charge | Yes, by Meta as one charge | No | No |

### Rates

#### Meta Business Agent messages

- **Meter**: Per token.
- **Rate**: One global rate of $2.00 USD per 1M tokens. One message typically consumes about 20,000–25,000 tokens, which translates to approximately 4–5 cents (USD) per message.
- **Cost per message**: Varies with complexity. Simple responses consume fewer tokens and cost less; complex responses consume more tokens and cost more.

The following table shows example interactions and their estimated cost.

| Type of interaction | Example user message | Messages to the user | Tokens consumed | Estimated cost |
| --- | --- | --- | --- | --- |
| Simple inquiry | &quot;At what time do you open?&quot; | 4 | ~80,000 (~20,000/message) | ~16–20 cents |
| Complex interaction | &quot;I&#039;m stuck on step 3 of this assembly—can you walk me through it?&quot; | 10 | ~250,000 (~25,000/message) | ~40–50 cents |

#### Service messages

- **Meter**: Per message.
- **Rate**: By market, rates for service messages are the same as the rates for utility and authentication messages. See the current [rate cards and volume tiers](https://developers.facebook.com/documentation/business-messaging/whatsapp/pricing#rate-cards-and-volume-tiers).
- **Volume tiers**: None. Meta does not offer volume tiers for service messages. Meta continues to offer volume tiers for utility and authentication messages.
- **Frequency of updates**: Consistent with the [pricing calendar](https://developers.facebook.com/documentation/business-messaging/whatsapp/pricing#pricing-calendar), Meta may update rates up to quarterly. Meta will announce and publish the rates that take effect October 1, 2026, including rates for service messages, by September 1, 2026.

### Pricing example

Below is an example of an interaction between a business and a user. It starts with the business reaching out with a marketing template, using Meta Business Agent to respond to user questions, escalating to a customer service rep, and ultimately sending an order confirmation, all within one hour.

- *As of July 1, 2026:* Meta charges only for 1 marketing template message.
- *As of August 1, 2026:* Meta charges for 3 messages (1 marketing, 2 Meta Business Agent).
- *As of October 1, 2026:* Meta will charge for each message the business sends – 5 messages thus 5 charges (1 marketing, 2 Meta Business Agent, 1 service, 1 utility).

| Sender | Message content | Message type | Message category | Charge as of August 1, 2026 | Charge as of October 1, 2026 |
| --- | --- | --- | --- | --- | --- |
| Business | Cart abandonment reminder | Template | Marketing | 1 marketing charge | 1 **marketing** charge |
| User | &quot;Do you have this in a Small?&quot; | — | — | Opens 24-hour CSW¹ | — |
| Business | Meta agentic response to user&#039;s question | Non-template | Meta Business Agent | 1 Meta Business Agent charge | 1 **Meta Business Agent** charge |
| User | &quot;Do you have this in white?&quot; | — | — | — | — |
| Business | Meta agentic response to user&#039;s question | Non-template | Meta Business Agent | 1 Meta Business Agent charge | 1 **Meta Business Agent** charge |
| User | &quot;I&#039;m ready to check-out, but my card is expired&quot; | — | — | — | — |
| *Meta Business Agent hands off to customer service rep* | | | | — | — |
| Business | Customer service rep responds to user&#039;s question | Non-template | Service | No charge | 1 **service** charge |
| Business | Order confirmation | Template | Utility | No charge | 1 **utility** charge |
| | | | **Total charges** | **3 charges** | **5 charges** |

### Cost comparison: service and Meta Business Agent messages

The following table compares the expected cost to send 10,000 AI-powered messages in response to users in Brazil. Estimates for third-party AI solutions are based on publicly available information and benchmarks. Message delivery costs are based on the current rate for utility and authentication messages to users in Brazil.

|  | Service (lower complexity) | Service (higher complexity) | Meta Business Agent (higher complexity) |
| --- | --- | --- | --- |
| AI agent cost | ~2 cents per message | ~9 cents per message | ~4–5 cents per message ($2.00/1M tokens × ~25,000 tokens) as one Meta Business Agent charge |
| Message delivery1 | 0.68 cents per message | 0.68 cents per message | ~4–5 cents per message ($2.00/1M tokens × ~25,000 tokens) as one Meta Business Agent charge |
| Total per message | ~2–3 cents | ~9–10 cents | ~4–5 cents |
| Total for 10,000 messages | ~$268 | ~$968 | ~$400–$500 |

1 The 0.68 cent message delivery rate is based on the current published rate for utility and authentication messages to users in Brazil. See the [rate cards and volume tiers](https://developers.facebook.com/documentation/business-messaging/whatsapp/pricing#rate-cards-and-volume-tiers) and the [WhatsApp Business Platform pricing page](https://business.whatsapp.com/products/platform-pricing#rates).

### Important notes

- **Any non-template message is charged as of October 1, 2026.** If the message is powered by Meta Business Agent, it incurs the charge of a Meta Business Agent message. If it is not powered by Meta Business Agent, it incurs the charge of a service message.
- **Only one charge applies per message.** A message to a user—template or non-template—has only one category and is charged only for that category. A non-template message is charged either as a Meta Business Agent message or as a service message, never both. This one-charge rule also applies to non-template messages with promotional content: Meta does not additionally apply a marketing template charge.
- **Not every Meta Business Agent message costs the same.** You pay proportionally to the value delivered: quick answers cost less, and rich, multi-turn problem-solving costs more.
- **The 72-hour free entry point window is unchanged for message delivery.** Messages sent within the 72-hour free entry point window, from Click to WhatsApp Ads or Facebook call-to-action buttons, remain free for message delivery. Meta Business Agent messages are still charged for token usage as of August 1, 2026.

### Technical implementation

Service messages and Meta Business Agent messages use different analytics and webhook category values.

#### Service messages

Service messages are an existing message category on the platform (`category: service`). You can use the Pricing Analytics API to understand how many service messages you have delivered. The category and charge for service messages are also included in message delivery webhooks.

**Analytics**

Query the `&lt;PRICING_CATEGORY&gt;` value of `SERVICE` to understand the count of service messages for a given timeframe.

```
&#123;
  &quot;start&quot;: &lt;START_TIMESTAMP&gt;,
  &quot;end&quot;: &lt;END_TIMESTAMP&gt;,
  &quot;phone_number&quot;: &quot;&lt;BUSINESS_PHONE_NUMBER&gt;&quot;,
  &quot;country&quot;: &quot;&lt;COUNTRY_CODE&gt;&quot;,
  &quot;pricing_type&quot;: &quot;REGULAR&quot;,
  &quot;pricing_category&quot;: &quot;SERVICE&quot;,
  &quot;volume&quot;: &lt;VOLUME&gt;,
  &quot;cost&quot;: &lt;COST&gt;
&#125;
```

**Webhooks**

The [webhooks](https://developers.facebook.com/documentation/business-messaging/whatsapp/pricing#webhooks) reflect the `&lt;PRICING_CATEGORY&gt;` as `service`. Billable messages have `type` set to `regular` in the pricing object of status [messages](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/messages/status) webhooks.

```
&quot;pricing&quot;: &#123;
  &quot;billable&quot;: true,
  &quot;pricing_model&quot;: &quot;PMP&quot;,
  &quot;type&quot;: &quot;regular&quot;,
  &quot;category&quot;: &quot;service&quot;
&#125;
```

#### Meta Business Agent messages

Analytics and webhook details for Meta Business Agent messages are forthcoming and will be published before charging takes effect on August 1, 2026.
