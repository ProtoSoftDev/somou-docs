# Features



This page describes the features that Marketing Messages API for WhatsApp provides and compares them to the equivalent features in Cloud API.

Marketing Messages API for WhatsApp offers added features that are not available on Cloud API, such as performance benchmarks and recommendations, time-to-live, and automated creative optimizations (pilot).

For more detail, see the comparison tables below.

## Optimization features

The following table compares the optimization features available in Marketing Messages API for WhatsApp and Cloud API.

| Description | Marketing Messages API for WhatsApp (Supports Marketing) | Cloud API (Supports Auth, Utility, Service, Marketing) |
| --- | --- | --- |
| **Quality-based delivery:** Improving deliveries of high engagement messages. | **Yes:** Marketing Messages API for WhatsApp factors whether a message is high engagement into delivery decisions, delivering up to 9% more messages versus Cloud API (see the [footnote](#footnote)). A high engagement marketing message is a message that users expect, that is relevant and timely, and that is therefore more likely to be read and clicked. | **No:** Message quality does not factor into per-user marketing message limits. No ability to increase delivery for high engagement messages. |
| **Automated creative optimizations:** Automatic image and text adjustments to increase message performance. | **Yes (pilot):** Automatically apply image and text adjustments to marketing template messages. See the full list of capabilities in [automatic creative optimizations](https://developers.facebook.com/documentation/business-messaging/whatsapp/marketing-messages/send-marketing-messages#automatic-creative-optimizations). | **No** |

## Marketing message formats

The following table compares the marketing message formats supported by Marketing Messages API for WhatsApp and Cloud API.

| Description | Marketing Messages API for WhatsApp (Supports Marketing) | Cloud API (Supports Auth, Utility, Service, Marketing) |
| --- | --- | --- |
| **Animated image (GIF) header:** Marketing message templates support a GIF [media type in the header](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/components#media-headers). | **Yes** | **No** |
| **Android app deep links:** [Links](https://developers.facebook.com/documentation/business-messaging/whatsapp/marketing-messages/deep-links) that open a specified app on a customer&#039;s Android device. | **Yes** | **No** |
| **Customizable message validity periods:** Set a [time-to-live for messages](https://developers.facebook.com/documentation/business-messaging/whatsapp/marketing-messages/send-marketing-messages#create-marketing-templates) so messages expire when WhatsApp cannot deliver them soon enough. | **Yes:** TTL can range from 12 hours to 30 days. | **Limited:** Only supports Authentication and Utility messages. |
| **Basic marketing message formats:** [Formats such as media, carousel, product catalog, flow, interactive list, and interactive reply.](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/marketing-templates) | **Yes** | **Yes** |

## Guidance

The following table compares the guidance features available in Marketing Messages API for WhatsApp and Cloud API.

| Description | Marketing Messages API for WhatsApp (Supports Marketing) | Cloud API (Supports Auth, Utility, Service, Marketing) |
| --- | --- | --- |
| **Benchmarks:** Comparison of read and click rates versus similar templates from other businesses in your region. | **Yes** | **No** |
| **Recommendations:** Data-derived recommendations to improve the performance of your template. | **Yes** | **No** |

## Metrics

The following table compares the metrics available in Marketing Messages API for WhatsApp and Cloud API.

| Description | Marketing Messages API for WhatsApp (Supports Marketing) | Cloud API (Supports Auth, Utility, Service, Marketing) |
| --- | --- | --- |
| **Conversion metrics:** Conversions on Web and App. | **Yes:** [Measure app events](https://developers.facebook.com/documentation/business-messaging/whatsapp/marketing-messages/measure-conversion#measure-app-conversions-with-meta-sdk-or-app-events-api) that users perform after a marketing message, such as &quot;Add to Cart&quot;, &quot;Checkout Initiated&quot;, and &quot;Purchase&quot;. | **No** |
| **Cost metrics:** [Spend per template, Cost per click, Cost per delivery.](https://developers.facebook.com/documentation/business-messaging/whatsapp/marketing-messages/view-metrics) | **Yes** | **Yes** |
| **Basic metrics:** [Sent, delivered, read, clicked, errors.](https://developers.facebook.com/documentation/business-messaging/whatsapp/marketing-messages/view-metrics) | **Yes** | **Yes** |

## Enterprise, security, and compliance features

The following table compares enterprise, security, and compliance features between Marketing Messages API for WhatsApp and Cloud API.

| Description | Marketing Messages API for WhatsApp (Supports Marketing) | Cloud API (Supports Auth, Utility, Service, Marketing) |
| --- | --- | --- |
| **Local Storage Support:** Phone numbers with [Local Storage enabled.](https://developers.facebook.com/documentation/business-messaging/whatsapp/local-storage) | **Yes** | **Yes** |
| **Compliance certification:** Compliance resources available on the [Business Messaging Compliance Center](https://www.facebook.com/business/business-messaging/compliance). | **Yes:** Certification for LGPD, GDPR, System Audit Report, SOC, ISO27001. | **Yes:** Certification for LGPD, GDPR, System Audit Report, SOC, ISO27001. |
| **Automatic throughput upgrades:** Automatic upgrades (and webhook notifications) to a phone number&#039;s [messaging throughput](https://developers.facebook.com/documentation/business-messaging/whatsapp/throughput). | **Yes** | **Yes** |
| **Real-time service status updates:** [Uptime and availability metrics](https://developers.facebook.com/documentation/business-messaging/whatsapp/support/api-status-page) are live on [metastatus.com](https://metastatus.com/whatsapp-business-api). | **Yes** | **Yes** |

## Onboarding

| Description | Marketing Messages API for WhatsApp (Supports Marketing) | Cloud API (Supports Auth, Utility, Service, Marketing) |
| --- | --- | --- |
| **Onboarding options:** [Onboard via Embedded Signup, Intent API, and Intent UI](https://developers.facebook.com/documentation/business-messaging/whatsapp/marketing-messages/onboarding). | **Yes** | **Limited:** Embedded Signup only. |
| **Error Codes:** Graph API [error codes](https://developers.facebook.com/documentation/business-messaging/whatsapp/support/error-codes#marketing-messages-api-for-whatsapp-error-codes). | **Yes:** [Specific Marketing Messages API for WhatsApp error codes](https://developers.facebook.com/documentation/business-messaging/whatsapp/support/error-codes#marketing-messages-api-for-whatsapp-error-codes). | **Yes** |
| **Onboarding Status via API:** Granular [eligibility status data and error codes](https://developers.facebook.com/documentation/business-messaging/whatsapp/marketing-messages/changelog#july-15--2025). | **Yes:** A [new eligibility status field](https://developers.facebook.com/documentation/business-messaging/whatsapp/marketing-messages/onboarding#eligibility-requirements) has been introduced to better report on the API onboarding status. | **Limited** |
| **WhatsApp Business app user onboarding:** [Onboard WhatsApp Business app users.](https://developers.facebook.com/documentation/business-messaging/whatsapp/marketing-messages/onboard-business-customers#onboard-whatsapp-business-app-users) | **Yes** | **Yes** |

## Footnote

The A/B test was conducted with approximately 12 million delivered marketing messages sent by advertisers in India from January 1, 2025 to January 31, 2025. It compared Marketing Messages API for WhatsApp optimized delivery to standard Cloud API delivery for high engagement messages only (specifically, more reads and clicks) and the analysis consisted of a t-test at 95% confidence.
