# Conversation-based pricing (Deprecated)


**Warning:** **Deprecated** — This document describes conversation-based pricing, which was replaced by per-message pricing on July 1, 2025. It remains available for developers who have historical billing data from the conversation-based pricing period. For current pricing, see [Pricing](https://developers.facebook.com/documentation/business-messaging/whatsapp/pricing).

This document explains how conversation-based pricing works on the WhatsApp Business Platform.

Charges are applied per conversation, not per individual message sent or received.

Conversations are 24-hour message threads between you and your customers. They are opened and charged when messages you send to customers are delivered. The criteria that determines when a conversation is opened and how it is categorized is explained below.

**Warning:** Businesses are responsible for reviewing the category assigned to their approved templates. Whenever a template is used, a business accepts the charges associated with the category applied to the template at time of use.

## Conversation categories

Conversations are categorized with one of the following categories:

* **Marketing** — Enables you to achieve a wide range of goals, from generating awareness to driving sales and retargeting customers. Examples include new product, service, or feature announcements, targeted promotions/offers, and cart abandonment reminders.
* **Utility** — Enables you to follow-up on user actions or requests. Examples include opt-in confirmation, order/delivery management (for example, delivery update); account updates or alerts (for example., payment reminder); or feedback surveys.
* **Authentication** — Enables you authenticate users with one-time pass codes, potentially at multiple steps in the login process (for example, account verification, account recovery, integrity challenges).
* **Service** — Enables you to resolve customer inquiries.

Marketing, utility, and authentication conversations can only be opened with template messages. Service conversations can be opened with any type of message other than a template message.

See [Message Types](https://developers.facebook.com/documentation/business-messaging/whatsapp/messages/send-messages#message-types) to learn more about the various types of messages you can send to customers.

## Opening conversations

Conversations are opened when you send a message to a customer under the following conditions.

### Marketing, Utility, and Authentication Conversations

When you send an approved marketing, utility, or authentication template to a customer, we check if an open conversation matching the template&#039;s **category** already exists between you and the customer. If one exists, no new conversation is opened. If one does not exist, a new **conversation of that category** is opened, lasting 24 hours.

For example:

* **Hour 0:** You send a targeted promotion (marketing [template message](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/overview)) to a customer. No open marketing conversation exists between you and the customer, so a marketing conversation lasting 24 hours is opened.
* **Hour 4:** The customer completes an order on your site, so you send them an order confirmation (utility template message). No open utility conversation exists between you and the customer, so a utility conversation lasting 24 hours is opened.
* **Hour 10:** You send a shipment confirmation (utility template message) to the customer. An open utility conversation already exists between you and the customer, so a new utility conversation is not opened.

To learn more about template categories and how to choose an appropriate category when creating templates, see [Template Categorization](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/template-categorization).

For additional examples, see our pricing explainer PDF.

### Service conversations

**Warning:** Service conversations are now free. This change does not affect how service conversations are opened.

A service conversation is opened when any message other than a template message is delivered to your customer and no open conversation of **any category** exists between you and the customer.

Note that a [customer service window](https://developers.facebook.com/documentation/business-messaging/whatsapp/messages/send-messages#customer-service-windows) must exist between you and the customer before you can send them a non-template message.

For example:

* **Hour 0:** You send a targeted promotion (marketing template) to a customer. No open marketing conversation exists between you and the customer, so a marketing conversation lasting 24 hours is opened.
* **Hour 4:** The customer messages you. This opens a customer service window between you and the customer, allowing you to send them any type of message for the next 24 hours.
* **Hour 5:** You send an interactive list message to the customer. An open conversation already exists between you and the customer (a marketing conversation in this case), so a service conversation is not opened.
* **Hour 24:** The marketing conversation expires.
* **Hour 25:** The 24-hour customer service window is still open, so you send a second text message to the customer. No open conversation exists between you and the customer anymore, so a service conversation is opened, lasting 24 hours.
* **Hour 26:** The 24-hour customer service window is still open, so you send a third text message to the customer. An open service conversation already exists between you and the customer, so a new service conversation is not opened.

For additional examples, see our pricing explainer PDF.

## Customer Service Windows

See [Customer Service Windows](https://developers.facebook.com/documentation/business-messaging/whatsapp/messages/send-messages#customer-service-windows).

## Conversation duration

Marketing, utility, authentication, and service conversations last 24 hours unless closed by a newly opened [free-entry point conversation](#free-entry-point-conversations).

Free-entry point conversations last 72 hours.

## Multiple conversations

It is possible to have multiple open conversations between you and a customer. This can happen in the following situations:

* An open marketing, utility, or authentication conversation exists between you and a customer and you send them a template message of a different category within 24 hours.
* An open service conversation exists between you and a customer and you send them a template message within 24 hours.

## Free Tier conversations

As of November 1, 2024, you can open an unlimited number of service conversations at no charge. See [Free Service Conversations](https://developers.facebook.com/documentation/business-messaging/whatsapp/pricing/conversation-based-pricing) to learn more.

## Free Entry Point conversations

A free entry point conversation is opened if (1) a customer using a device running Android or iOS (the desktop and web clients are not supported) messages you via a [Click to WhatsApp Ad](https://www.facebook.com/business/help/447934475640650/) or [Facebook Page Call-to-Action](https://www.facebook.com/help/977869848936797) button and (2) you respond within 24 hours. If you do not respond within 24 hours, a free entry point conversation is not opened and you must use a template to message the customer, which opens a marketing, utility, or authentication conversation, per the category of the template.

The free entry point conversation is opened as soon as your message is delivered and lasts 72 hours. When a free entry point conversation is opened, it automatically closes all other open conversations between you and the customer, and no new conversations will be opened until the free entry point conversation expires.

Once the free entry point conversation is opened, you can send any type of message to the customer without incurring additional charges. However, you can only send non-templates messages if there is an open customer service window between you and the customer.

For example, if the customer messages you via a Click to WhatsApp Ad at 10am and you respond via a template message at 10pm the same day:

* The free entry point conversation starts at 10pm and lasts 72 hours.
* You can send template messages at no charge in those 72 hours.
* You can send non-template messages until 10am the next day, at which point the [customer service window](https://developers.facebook.com/documentation/business-messaging/whatsapp/messages/send-messages#customer-service-windows) closes, as it is independent of the free entry point conversation (if the customer messages you again, however, it opens another 24-hour customer service window in which you can send any type of message).

## Rates

Rates vary based on conversation category and country/region rate. You can download the rate card below that corresponds to your WhatsApp Business Account&#039;s currency to see our rates by country/region for each conversation category.

These rates apply for any conversation opened on or after June 1, 2023 at 12:00 AM, based on WhatsApp Business Account time zone.

### Rate Cards

These rate cards represent the current rates on our platform.

- Rates in USD

- Rates in INR

- Rates in IDR

- Rates in EUR

- Rates in GBP

- Rates in AUD

### Authentication-International rates

Starting June 1, 2024, we are introducing authentication-international rates. See [Authentication-International Rates](https://developers.facebook.com/documentation/business-messaging/whatsapp/pricing/authentication-international-rates) to learn about these rates and if they apply to you.

**Warning:** Effective April 1, 2025, we are lowering our authentication-international rates in Egypt, Nigeria, Pakistan and South Africa, as part of continued efforts to ensure our prices are on-par with alternate channels.

### Marketing Messages API for WhatsApp pricing

**Warning:** Per-message pricing is coming to Marketing Messages API for WhatsApp. Starting July 1, 2025, Cloud API marketing rates will apply to messages sent via Marketing Messages API for WhatsApp.

Marketing Messages API for WhatsApp has different pricing. View the [Marketing Messages API for WhatsApp pricing document](https://developers.facebook.com/documentation/business-messaging/whatsapp/pricing) for details.

### WhatsApp Business Calling API pricing

The WhatsApp Business Calling API has different pricing. View the [Calling API pricing document](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/pricing) for details.

### Updates to rate cards

As announced in June 2024, we may update rates up-to-quarterly. For marketing, updates are to reflect demand and the value these messages deliver. For utility and authentication, our objective is to price on-par with alternate channels.

To support these efforts, we have made the following updates:

- **Effective April 1, 2025**

- Lowered [authentication-international pricing rates](https://developers.facebook.com/documentation/business-messaging/whatsapp/pricing/authentication-international-rates) for Egypt, Nigeria, Pakistan, and South Africa.

- **Effective February 1, 2025**

- Lowered [authentication pricing rates](https://developers.facebook.com/documentation/business-messaging/whatsapp/pricing#rates) for Egypt, Malaysia, Nigeria, Pakistan, Saudi Arabia, South Africa, and the United Arab Emirates.

- Added [authentication-international pricing rates](https://developers.facebook.com/documentation/business-messaging/whatsapp/pricing/authentication-international-rates) for Egypt, Malaysia, Nigeria, Pakistan, Saudi Arabia, South Africa, and the United Arab Emirates.

- **Effective November 1, 2024**

- [Service conversations](https://developers.facebook.com/documentation/business-messaging/whatsapp/pricing#service-conversations) are now free for all businesses, including via AI-enabled conversational experiences.

- **Effective October 1, 2024**

- Updated [pricing rates](https://developers.facebook.com/documentation/business-messaging/whatsapp/pricing#rates) in India, Saudi Arabia, the United Arab Emirates, and the United Kingdom.

- **Effective August 1, 2024**

- Lowered utility conversation [pricing rates](https://developers.facebook.com/documentation/business-messaging/whatsapp/pricing#rates).

### Country calling codes

Charges for conversations are based on the country of the user&#039;s phone number. We rely on your customer&#039;s country calling code and network prefix (area code) to determine their country. The table below shows how we map country codes to countries or regions. If a country is not listed below, it maps to Other.

| Markets | Calling Code&lt;br&gt;&lt;br&gt;(and network prefix if applicable) |
| --- | --- |
| Countries&lt;br&gt;&lt;br&gt;Argentina&lt;br&gt;&lt;br&gt;Brazil&lt;br&gt;&lt;br&gt;Chile&lt;br&gt;&lt;br&gt;Colombia&lt;br&gt;&lt;br&gt;Egypt&lt;br&gt;&lt;br&gt;France&lt;br&gt;&lt;br&gt;Germany&lt;br&gt;&lt;br&gt;India&lt;br&gt;&lt;br&gt;Indonesia&lt;br&gt;&lt;br&gt;Israel&lt;br&gt;&lt;br&gt;Italy&lt;br&gt;&lt;br&gt;Malaysia&lt;br&gt;&lt;br&gt;Mexico&lt;br&gt;&lt;br&gt;Netherlands&lt;br&gt;&lt;br&gt;Nigeria&lt;br&gt;&lt;br&gt;Pakistan&lt;br&gt;&lt;br&gt;Peru&lt;br&gt;&lt;br&gt;Russia&lt;br&gt;&lt;br&gt;Saudi Arabia&lt;br&gt;&lt;br&gt;South Africa&lt;br&gt;&lt;br&gt;Spain&lt;br&gt;&lt;br&gt;Turkey&lt;br&gt;&lt;br&gt;United Arab Emirates&lt;br&gt;&lt;br&gt;United Kingdom | 54&lt;br&gt;&lt;br&gt;55&lt;br&gt;&lt;br&gt;56&lt;br&gt;&lt;br&gt;57&lt;br&gt;&lt;br&gt;20&lt;br&gt;&lt;br&gt;33&lt;br&gt;&lt;br&gt;49&lt;br&gt;&lt;br&gt;91&lt;br&gt;&lt;br&gt;62&lt;br&gt;&lt;br&gt;972&lt;br&gt;&lt;br&gt;39&lt;br&gt;&lt;br&gt;60&lt;br&gt;&lt;br&gt;52&lt;br&gt;&lt;br&gt;31&lt;br&gt;&lt;br&gt;234&lt;br&gt;&lt;br&gt;92&lt;br&gt;&lt;br&gt;51&lt;br&gt;&lt;br&gt;7&lt;br&gt;&lt;br&gt;966&lt;br&gt;&lt;br&gt;27&lt;br&gt;&lt;br&gt;34&lt;br&gt;&lt;br&gt;90&lt;br&gt;&lt;br&gt;971&lt;br&gt;&lt;br&gt;44 |
| North America&lt;br&gt;&lt;br&gt;Canada&lt;br&gt;&lt;br&gt;United States | 1&lt;br&gt;&lt;br&gt;1 |
| Rest of Africa&lt;br&gt;&lt;br&gt;Algeria&lt;br&gt;&lt;br&gt;Angola&lt;br&gt;&lt;br&gt;Benin&lt;br&gt;&lt;br&gt;Botswana&lt;br&gt;&lt;br&gt;Burkina Faso&lt;br&gt;&lt;br&gt;Burundi&lt;br&gt;&lt;br&gt;Cameroon&lt;br&gt;&lt;br&gt;Chad&lt;br&gt;&lt;br&gt;Republic of the Congo (Brazzaville)&lt;br&gt;&lt;br&gt;Eritrea&lt;br&gt;&lt;br&gt;Ethiopia&lt;br&gt;&lt;br&gt;Gabon&lt;br&gt;&lt;br&gt;Gambia&lt;br&gt;&lt;br&gt;Ghana&lt;br&gt;&lt;br&gt;Guinea-Bissau&lt;br&gt;&lt;br&gt;Ivory Coast&lt;br&gt;&lt;br&gt;Kenya&lt;br&gt;&lt;br&gt;Lesotho&lt;br&gt;&lt;br&gt;Liberia&lt;br&gt;&lt;br&gt;Libya&lt;br&gt;&lt;br&gt;Madagascar&lt;br&gt;&lt;br&gt;Malawi&lt;br&gt;&lt;br&gt;Mali&lt;br&gt;&lt;br&gt;Mauritania&lt;br&gt;&lt;br&gt;Morocco&lt;br&gt;&lt;br&gt;Mozambique&lt;br&gt;&lt;br&gt;Namibia&lt;br&gt;&lt;br&gt;Niger&lt;br&gt;&lt;br&gt;Rwanda&lt;br&gt;&lt;br&gt;Senegal&lt;br&gt;&lt;br&gt;Sierra Leone&lt;br&gt;&lt;br&gt;Somalia&lt;br&gt;&lt;br&gt;South Sudan&lt;br&gt;&lt;br&gt;Sudan&lt;br&gt;&lt;br&gt;Swaziland&lt;br&gt;&lt;br&gt;Tanzania&lt;br&gt;&lt;br&gt;Togo&lt;br&gt;&lt;br&gt;Tunisia&lt;br&gt;&lt;br&gt;Uganda&lt;br&gt;&lt;br&gt;Zambia | 213&lt;br&gt;&lt;br&gt;244&lt;br&gt;&lt;br&gt;229&lt;br&gt;&lt;br&gt;267&lt;br&gt;&lt;br&gt;226&lt;br&gt;&lt;br&gt;257&lt;br&gt;&lt;br&gt;237&lt;br&gt;&lt;br&gt;235&lt;br&gt;&lt;br&gt;242&lt;br&gt;&lt;br&gt;291&lt;br&gt;&lt;br&gt;251&lt;br&gt;&lt;br&gt;241&lt;br&gt;&lt;br&gt;220&lt;br&gt;&lt;br&gt;233&lt;br&gt;&lt;br&gt;245&lt;br&gt;&lt;br&gt;225&lt;br&gt;&lt;br&gt;254&lt;br&gt;&lt;br&gt;266&lt;br&gt;&lt;br&gt;231&lt;br&gt;&lt;br&gt;218&lt;br&gt;&lt;br&gt;261&lt;br&gt;&lt;br&gt;265&lt;br&gt;&lt;br&gt;223&lt;br&gt;&lt;br&gt;222&lt;br&gt;&lt;br&gt;212&lt;br&gt;&lt;br&gt;258&lt;br&gt;&lt;br&gt;264&lt;br&gt;&lt;br&gt;227&lt;br&gt;&lt;br&gt;250&lt;br&gt;&lt;br&gt;221&lt;br&gt;&lt;br&gt;232&lt;br&gt;&lt;br&gt;252&lt;br&gt;&lt;br&gt;211&lt;br&gt;&lt;br&gt;249&lt;br&gt;&lt;br&gt;268&lt;br&gt;&lt;br&gt;255&lt;br&gt;&lt;br&gt;228&lt;br&gt;&lt;br&gt;216&lt;br&gt;&lt;br&gt;256&lt;br&gt;&lt;br&gt;260 |
| Rest of Asia Pacific&lt;br&gt;&lt;br&gt;Afghanistan&lt;br&gt;&lt;br&gt;Australia&lt;br&gt;&lt;br&gt;Bangladesh&lt;br&gt;&lt;br&gt;Cambodia&lt;br&gt;&lt;br&gt;China&lt;br&gt;&lt;br&gt;Hong Kong&lt;br&gt;&lt;br&gt;Japan&lt;br&gt;&lt;br&gt;Laos&lt;br&gt;&lt;br&gt;Mongolia&lt;br&gt;&lt;br&gt;Nepal&lt;br&gt;&lt;br&gt;New Zealand&lt;br&gt;&lt;br&gt;Papua New Guinea&lt;br&gt;&lt;br&gt;Philippines&lt;br&gt;&lt;br&gt;Singapore&lt;br&gt;&lt;br&gt;Sri Lanka&lt;br&gt;&lt;br&gt;Taiwan&lt;br&gt;&lt;br&gt;Tajikistan&lt;br&gt;&lt;br&gt;Thailand&lt;br&gt;&lt;br&gt;Turkmenistan&lt;br&gt;&lt;br&gt;Uzbekistan&lt;br&gt;&lt;br&gt;Vietnam | 93&lt;br&gt;&lt;br&gt;61&lt;br&gt;&lt;br&gt;880&lt;br&gt;&lt;br&gt;855&lt;br&gt;&lt;br&gt;86&lt;br&gt;&lt;br&gt;852&lt;br&gt;&lt;br&gt;81&lt;br&gt;&lt;br&gt;856&lt;br&gt;&lt;br&gt;976&lt;br&gt;&lt;br&gt;977&lt;br&gt;&lt;br&gt;64&lt;br&gt;&lt;br&gt;675&lt;br&gt;&lt;br&gt;63&lt;br&gt;&lt;br&gt;65&lt;br&gt;&lt;br&gt;94&lt;br&gt;&lt;br&gt;886&lt;br&gt;&lt;br&gt;992&lt;br&gt;&lt;br&gt;66&lt;br&gt;&lt;br&gt;993&lt;br&gt;&lt;br&gt;998&lt;br&gt;&lt;br&gt;84 |
| Rest of Central &amp; Eastern Europe&lt;br&gt;&lt;br&gt;Albania&lt;br&gt;&lt;br&gt;Armenia&lt;br&gt;&lt;br&gt;Azerbaijan&lt;br&gt;&lt;br&gt;Belarus&lt;br&gt;&lt;br&gt;Bulgaria&lt;br&gt;&lt;br&gt;Croatia&lt;br&gt;&lt;br&gt;Czech Republic&lt;br&gt;&lt;br&gt;Georgia&lt;br&gt;&lt;br&gt;Greece&lt;br&gt;&lt;br&gt;Hungary&lt;br&gt;&lt;br&gt;Latvia&lt;br&gt;&lt;br&gt;Lithuania&lt;br&gt;&lt;br&gt;Moldova&lt;br&gt;&lt;br&gt;North Macedonia&lt;br&gt;&lt;br&gt;Poland&lt;br&gt;&lt;br&gt;Romania&lt;br&gt;&lt;br&gt;Serbia&lt;br&gt;&lt;br&gt;Slovakia&lt;br&gt;&lt;br&gt;Slovenia&lt;br&gt;&lt;br&gt;Ukraine | 355&lt;br&gt;&lt;br&gt;374&lt;br&gt;&lt;br&gt;994&lt;br&gt;&lt;br&gt;375&lt;br&gt;&lt;br&gt;359&lt;br&gt;&lt;br&gt;385&lt;br&gt;&lt;br&gt;420&lt;br&gt;&lt;br&gt;995&lt;br&gt;&lt;br&gt;30&lt;br&gt;&lt;br&gt;36&lt;br&gt;&lt;br&gt;371&lt;br&gt;&lt;br&gt;370&lt;br&gt;&lt;br&gt;373&lt;br&gt;&lt;br&gt;389&lt;br&gt;&lt;br&gt;48&lt;br&gt;&lt;br&gt;40&lt;br&gt;&lt;br&gt;381&lt;br&gt;&lt;br&gt;421&lt;br&gt;&lt;br&gt;386&lt;br&gt;&lt;br&gt;380 |
| Rest of Western Europe&lt;br&gt;&lt;br&gt;Austria&lt;br&gt;&lt;br&gt;Belgium&lt;br&gt;&lt;br&gt;Denmark&lt;br&gt;&lt;br&gt;Finland&lt;br&gt;&lt;br&gt;Ireland&lt;br&gt;&lt;br&gt;Norway&lt;br&gt;&lt;br&gt;Portugal&lt;br&gt;&lt;br&gt;Sweden&lt;br&gt;&lt;br&gt;Switzerland | 43&lt;br&gt;&lt;br&gt;32&lt;br&gt;&lt;br&gt;45&lt;br&gt;&lt;br&gt;358&lt;br&gt;&lt;br&gt;353&lt;br&gt;&lt;br&gt;47&lt;br&gt;&lt;br&gt;351&lt;br&gt;&lt;br&gt;46&lt;br&gt;&lt;br&gt;41 |
| Rest of Latin America&lt;br&gt;&lt;br&gt;Bolivia&lt;br&gt;&lt;br&gt;Costa Rica&lt;br&gt;&lt;br&gt;Dominican Republic&lt;br&gt;&lt;br&gt;Ecuador&lt;br&gt;&lt;br&gt;El Salvador&lt;br&gt;&lt;br&gt;Guatemala&lt;br&gt;&lt;br&gt;Haiti&lt;br&gt;&lt;br&gt;Honduras&lt;br&gt;&lt;br&gt;Jamaica&lt;br&gt;&lt;br&gt;Nicaragua&lt;br&gt;&lt;br&gt;Panama&lt;br&gt;&lt;br&gt;Paraguay&lt;br&gt;&lt;br&gt;Puerto Rico&lt;br&gt;&lt;br&gt;Uruguay&lt;br&gt;&lt;br&gt;Venezuela | 591&lt;br&gt;&lt;br&gt;506&lt;br&gt;&lt;br&gt;1 (809, 829, 849)&lt;br&gt;&lt;br&gt;593&lt;br&gt;&lt;br&gt;503&lt;br&gt;&lt;br&gt;502&lt;br&gt;&lt;br&gt;509&lt;br&gt;&lt;br&gt;504&lt;br&gt;&lt;br&gt;1 (658, 876)&lt;br&gt;&lt;br&gt;505&lt;br&gt;&lt;br&gt;507&lt;br&gt;&lt;br&gt;595&lt;br&gt;&lt;br&gt;1 (787, 939)&lt;br&gt;&lt;br&gt;598&lt;br&gt;&lt;br&gt;58 |
| Rest of Middle East&lt;br&gt;&lt;br&gt;Bahrain&lt;br&gt;&lt;br&gt;Iraq&lt;br&gt;&lt;br&gt;Jordan&lt;br&gt;&lt;br&gt;Kuwait&lt;br&gt;&lt;br&gt;Lebanon&lt;br&gt;&lt;br&gt;Oman&lt;br&gt;&lt;br&gt;Qatar&lt;br&gt;&lt;br&gt;Yemen | 973&lt;br&gt;&lt;br&gt;964&lt;br&gt;&lt;br&gt;962&lt;br&gt;&lt;br&gt;965&lt;br&gt;&lt;br&gt;961&lt;br&gt;&lt;br&gt;968&lt;br&gt;&lt;br&gt;974&lt;br&gt;&lt;br&gt;967 |
| Other&lt;br&gt;&lt;br&gt;All other countries | Varies by country |

The information in the table above is also available in a CSV file:

- Country Calling Codes and Regional Rate Mapping CSV

## Webhooks

Pricing information is included in all message webhooks. See:

* Cloud API: [Message Status Updates](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/messages/status)
* On-Premises API (deprecated): Message Status Updates

## Billing

Billing and billing-related actions are handled through the Meta Business Suite. See [About Billing For Your WhatsApp Business Account](https://www.facebook.com/business/help/2225184664363779) for more information.

## Marketing Messages API for WhatsApp

If you are using the Marketing Messages API for WhatsApp, such usage is subject to Marketing Messages API for WhatsApp pricing. See the [Marketing Messages API for WhatsApp pricing](https://developers.facebook.com/documentation/business-messaging/whatsapp/pricing) document for pricing information and rate cards.

## See also

* [Conversations](https://developers.facebook.com/documentation/business-messaging/whatsapp/messages/send-messages)
* [About Billing For Your WhatsApp Business Account](https://www.facebook.com/business/help/2225184664363779)
* [Pricing](https://developers.facebook.com/documentation/business-messaging/whatsapp/pricing)
* [Template Categorization](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/template-categorization)
* [Sending messages with Cloud API](https://developers.facebook.com/documentation/business-messaging/whatsapp/messages/send-messages)
