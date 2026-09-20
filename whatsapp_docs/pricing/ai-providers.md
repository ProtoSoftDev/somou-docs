# New pricing policy for AI Providers leveraging the WhatsApp Business Platform


**Warning:** This page is specific to &quot;AI Providers&quot; using the WhatsApp Business Platform. This does NOT change how Meta charges all other businesses using the WhatsApp Business Platform. Refer to the [pricing page](https://developers.facebook.com/documentation/business-messaging/whatsapp/pricing).

### Who this applies to

This is specific to &quot;AI Providers&quot; using the WhatsApp Business Platform, as defined in our [Terms of Service](https://www.whatsapp.com/legal/business-solution-terms/) updated on January 15, 2026: Providers and developers of artificial intelligence or machine learning technologies, such as large language models, generative artificial intelligence platforms, general-purpose artificial intelligence assistants, or similar technologies who provide certain services on WhatsApp Business Platform.

This does **NOT** change how or what Meta charges all other businesses using the WhatsApp Business Platform. Meta will continue to charge these businesses as outlined in the [pricing explainer](https://developers.facebook.com/documentation/business-messaging/whatsapp/pricing#cloud-api-and-marketing-messages-api-for-whatsapp). This includes *not* being charged for non-template messages sent in an open customer service window. This also does not change the mechanics of the [customer service window](https://developers.facebook.com/documentation/business-messaging/whatsapp/messages/send-messages#customer-service-windows).

### Why Meta is charging


*Specifically for third-party AI Providers:*

* Effective January 15, 2026, WhatsApp&#039;s [Terms of Service update](https://www.whatsapp.com/legal/business-solution-terms/) &quot;AI Providers&quot; are only permitted to offer general-purpose AI assistants on the WhatsApp Business Platform where Meta is legally required to permit this use case.
* Effective February 16, 2026, in countries where Meta is legally required to support AI Providers&#039; usage of the WhatsApp Business Platform, Meta will charge AI Providers for non-template messages sent to WhatsApp users in these countries.


### What and where Meta will charge

**Warning:** Effective May 13, 2026 as of 12 AM local WABA timezone, Meta will **no longer** charge &quot;AI Providers&quot; for non-template messages delivered to users in certain markets, per below.

*Effective February 16, 2026* – Meta will charge for:


* Each non-template message (`&quot;type&quot;:&quot;text&quot;, &quot;type&quot;:&quot;image&quot;`, and so on)
* Delivered from an &quot;AI Provider&quot;
* To a user in a market where Meta is legally required to permit AI Providers to use the WhatsApp Business Platform

Markets and effective dates (updated as of May 12, 2026):

Effective **March 11, 2026**, this applies to Brazil (+55).

Effective **March 11, 2026 until May 12, 2026**, this applied to the following countries:

- Austria (+43)
- Belgium (+32)
- Bulgaria (+359)
- Croatia (+385)
- Cyprus (+357)
- Czech Republic (+420)
- Denmark (+45)
- Estonia (+372)
- Finland (+358)
- France (+33)
- Germany (+49)
- Greece (+30)
- Hungary (+36)
- Iceland (+354)
- Ireland (+353)
- Latvia (+371)
- Liechtenstein (+423)
- Lithuania (+370)
- Luxembourg (+352)
- Malta (+356)
- Netherlands (+31)
- Norway (+47)
- Poland (+48)
- Portugal (+351)
- Romania (+40)
- Slovakia (+421)
- Slovenia (+386)
- Spain (+34)
- Sweden (+46)

Effective **February 16, 2026 until May 12, 2026**, this applied to Italy (+39).


For example: If a user in Italy sends an AI Provider a prompt, and the AI Provider delivers three non-template message responses to the user over a span of 5 minutes, that will incur three charges.

### Rates

| Rates (CSV) | Rates (PDF) |
| --- | --- |
| AI Provider rates for non-template messages CSV (updated May 12, 2026) | AI Provider rates for non-template messages PDF (updated May 12, 2026) |

**Warning:** *These rates are specific to AI Providers using the WhatsApp Business Platform. To see rates for marketing, utility, and authentication messages, please refer to [Pricing on the WhatsApp Business Platform](https://developers.facebook.com/documentation/business-messaging/whatsapp/pricing).*

#### Rates effective July 1, 2026

Below represents future updates to AI Provider rates. See the rate cards above for current rates.

| Rates (CSV) | Rates (PDF) |
| --- | --- |
| AI Provider rates for non-template messages CSV | AI Provider rates for non-template messages PDF |

### Analytics

The Pricing Analytics API will include a new `&lt;PRICING_CATEGORY&gt;` value of `AI_BOT` to reflect AI Provider traffic.

```
&#123;
  &quot;start&quot;: &lt;START_TIMESTAMP&gt;,
  &quot;end&quot;: &lt;END_TIMESTAMP&gt;,
  &quot;phone_number&quot;: &quot;&lt;BUSINESS_PHONE_NUMBER&gt;&quot;,
  &quot;country&quot;: &quot;&lt;COUNTRY_CODE&gt;&quot;,
  &quot;pricing_type&quot;: &quot;REGULAR&quot;,
  &quot;pricing_category&quot;: &quot;AI_BOT&quot;,
  &quot;volume&quot;: &lt;VOLUME&gt;,
  &quot;cost&quot;: &lt;COST&gt;
&#125;
```


### Webhooks


The [webhooks](https://developers.facebook.com/documentation/business-messaging/whatsapp/pricing#webhooks) will reflect the `&lt;PRICING_CATEGORY&gt;` for these non-template messages from &quot;AI Providers&quot; as `general_purpose_ai`.


Billable messages have `type` set to `regular` in the pricing object of status [messages](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/messages/status) webhooks:


```
&quot;pricing&quot;: &#123;
 &quot;billable&quot;: true,
 &quot;pricing_model&quot;: &quot;PMP&quot;,
 &quot;type&quot;: &quot;regular&quot;,
 &quot;category&quot;: &quot;general_purpose_ai&quot;
&#125;
```
