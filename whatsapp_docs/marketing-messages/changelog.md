# Changelog



## June 28, 2026
_Marketing Messages API for WhatsApp_

The GET response for listing allowlisted end-businesses now includes `end_business_name`, the display name of each enrolled end-business, so Solution Partners no longer need a separate lookup per ID. The field is `null` when the business can&#039;t be resolved. See [Enroll in the max price feature](https://developers.facebook.com/documentation/business-messaging/whatsapp/marketing-messages/pricing/enroll-max-price).

## June 18, 2026
_Marketing Messages API for WhatsApp, Pricing_

The `optimization_spec` object replaces `bid_spec` for setting a max price on marketing templates. The `bid_amount` and `bid_strategy` fields are unchanged — only the enclosing object name changes. Update your create, retrieve, and update calls to use `optimization_spec`. Both objects are accepted during the grace period; `bid_spec` is no longer supported after July 31, 2026. See [Set a max price for marketing messages](https://developers.facebook.com/documentation/business-messaging/whatsapp/marketing-messages/pricing) for details.

## June 17, 2026
_Marketing Messages API for WhatsApp_

Solution Partners can now allowlist up to 15 end-businesses for the max price feature during the Limited Beta period, increased from 5. See [Enroll in the max price feature](https://developers.facebook.com/documentation/business-messaging/whatsapp/marketing-messages/pricing/enroll-max-price).

## May 15, 2026
_Marketing Messages API for WhatsApp_

The max price feature is now available in Limited Beta. Set a maximum price per marketing message delivery to control costs and optimize delivery — when a max price is set, Meta charges that amount or lower. See [Set a max price for marketing messages](https://developers.facebook.com/documentation/business-messaging/whatsapp/marketing-messages/pricing) for details, including how to create templates with a max price, adjust the price at send time using `per_message_bid_multiplier`, and estimate reach and costs using the Reach Estimation API.

To enroll during the Limited Beta period, sign the beta agreement and (for BSPs) allowlist your end-businesses. See [Enroll in the max price feature](https://developers.facebook.com/documentation/business-messaging/whatsapp/marketing-messages/pricing/enroll-max-price).

Updated [geographic availability of features](https://developers.facebook.com/documentation/business-messaging/whatsapp/marketing-messages/get-started#geographic-availability-of-features): added Nigeria and South Africa to the EEA restricted-features group, added Venezuela to the sanctioned-regions list, and expanded the Russia section to include Belarus.

## May 5, 2026
_Marketing Messages API for WhatsApp_

The `/marketing_messages` endpoint now supports sending messages using a [Business-Scoped User ID (BSUID)](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-scoped-user-ids). Use the new optional `recipient` field to specify a BSUID instead of the `to` field. Responses include a new `contacts.user_id` field when sent by BSUID. See [Sending to a BSUID](https://developers.facebook.com/documentation/business-messaging/whatsapp/marketing-messages/send-marketing-messages#sending-to-a-bsuid-business-scoped-user-id) for details.

Added [Profile end card](https://developers.facebook.com/documentation/business-messaging/whatsapp/marketing-messages/send-marketing-messages#profile-end-card) to the list of upcoming [automatic creative optimizations](https://developers.facebook.com/documentation/business-messaging/whatsapp/marketing-messages/send-marketing-messages#automatic-creative-optimizations).

## April 21, 2026
_Marketing Messages API for WhatsApp_

Additional offsite conversion metrics are now available in WhatsApp Manager and in the [Template Analytics API](https://developers.facebook.com/documentation/business-messaging/whatsapp/analytics#template-analytics). New metrics include: registrations completed, adds to wishlist, adds of payment info, levels achieved, ratings submitted, tutorials completed, searches, leads, content views, other, and custom events. See [Viewing metrics](https://developers.facebook.com/documentation/business-messaging/whatsapp/marketing-messages/view-metrics) and [Setting up conversion measurement](https://developers.facebook.com/documentation/business-messaging/whatsapp/marketing-messages/measure-conversion) for details.

[Automatic creative optimizations](https://developers.facebook.com/documentation/business-messaging/whatsapp/marketing-messages/send-marketing-messages#automatic-creative-optimizations) are now disabled by default at the WhatsApp Business Account level. You can opt in via the API or directly in WhatsApp Manager. Updated ACO documentation with refreshed content: added Dynamic CTA and Hyperlink formatting to Coming soon, and updated text formatting description.

## April 6, 2026
_Marketing Messages API for WhatsApp_

Businesses onboarded to Marketing Messages API for WhatsApp can now [disable marketing messages on Cloud API](https://developers.facebook.com/documentation/business-messaging/whatsapp/marketing-messages/send-marketing-messages#disable-marketing-messages-on-cloud-api). When this option is activated, the Cloud API `/messages` endpoint rejects Marketing category templates with error code `131063`. The `/marketing_messages` fallback to Cloud API is also blocked when this setting is enabled.

## April 1, 2026
_Marketing Messages API for WhatsApp_

Added [URL requirements](https://developers.facebook.com/documentation/business-messaging/whatsapp/marketing-messages/measure-conversion#url-requirements-for-conversion-measurement) and [Android deep link guidance](https://developers.facebook.com/documentation/business-messaging/whatsapp/marketing-messages/measure-conversion#android-deep-links-for-conversion-measurement) for conversion measurement, including best practices for preserving click IDs through short links, redirects, and deep link intent handling.

## March 2, 2026
_Marketing Messages API for WhatsApp_

Added [upcoming rates](https://developers.facebook.com/documentation/business-messaging/whatsapp/pricing#upcoming-updates-to-rate-cards) for 2026 for the following countries: Argentina (ARS), Australia (AUD), Chile (CLP), Colombia (COP), Euro (EUR), India (INR), Indonesia (IDR), Malaysia (MYR), Mexico (MXN), Peru (PEN), Saudi Arabia (SAR), Singapore (SGD), United Arab Emirates (AED), United Kingdom (GBP), United States (USD).

## February 19, 2026
_Marketing Messages API for WhatsApp_

Added a new [Automatic Creative Optimization](https://developers.facebook.com/documentation/business-messaging/whatsapp/marketing-messages/send-marketing-messages#auto-promotion-tag) type: Auto promotion tag.

## January 29, 2026
_Marketing Messages API for WhatsApp_

[Multi-solution conversations](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/multi-solution-conversations) are now available for Marketing Messages API for WhatsApp.

## December 18, 2025
_Marketing Messages API for WhatsApp_

Businesses that use Marketing Messages API for WhatsApp can now view [metrics for quick replies](https://developers.facebook.com/documentation/business-messaging/whatsapp/marketing-messages/view-metrics).

## November 19, 2025
_Marketing Messages API for WhatsApp_

[Marketing Messages API for WhatsApp](https://developers.facebook.com/documentation/business-messaging/whatsapp/marketing-messages/overview) (formerly known as Marketing Messages Lite API) is now generally available.

## October 31, 2025

_MM Lite API_

* Added two new [Automatic Creative Optimization types](https://developers.facebook.com/docs/whatsapp/marketing-messages-lite-api/sending-messages#product-extensions): product extensions and text formatting optimization.
* Launched a new endpoint to support [WABA-level opt-out](https://developers.facebook.com/docs/whatsapp/marketing-messages-lite-api/sending-messages#configure-automatic-creative-optimizations--whatsapp-business-account-level-) for automatic creative optimizations.

## October 20, 2025

_MM Lite API_

* [Offsite conversion metrics](https://developers.facebook.com/docs/whatsapp/marketing-messages-lite-api/viewing-metrics) is now available in WhatsApp Business Manager and in the [Template Analytics API](https://developers.facebook.com/documentation/business-messaging/whatsapp/analytics#template-analytics).
* Added two new Automatic Creative Optimization features: [Headline extraction](https://developers.facebook.com/docs/whatsapp/marketing-messages-lite-api/sending-messages#headline-extraction) and [Tap-target title extraction](https://developers.facebook.com/docs/whatsapp/marketing-messages-lite-api/sending-messages#tap-target-title-extraction).

## October 17, 2025

_MM Lite API_

Added [`marketing_messages_onboarding_status`](https://developers.facebook.com/docs/whatsapp/marketing-messages-lite-api/onboarding#if-you-want-to-check-tos-and-intent-request-status-for-the-business-manager) and [`owner_business_info`](https://developers.facebook.com/docs/whatsapp/marketing-messages-lite-api/onboarding#if-you-want-to-check-tos-and-intent-request-status-for-the-business-manager) fields to check Terms of Service and Intent request status for business manager.

## October 14, 2025

_MM Lite API_

The [`product_policy`](https://developers.facebook.com/docs/whatsapp/marketing-messages-lite-api/sending-messages#send-marketing-template-messages) field is now 100% rolled out for businesses customers.

## October 8, 2025

_Embedded Signup_

Embedded Signup version 4 is now available. Version 4 provides a simplified onboarding experience and allows you to onboard business customers to multiple products (WhatsApp Cloud API, Marketing Messages Lite API, Ads that click-to-WhatsApp and the Conversions API). For more information, see the [versions](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/versions) and [v4](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/version-4) page.

_MM Lite_

Updated the [onboarding page](https://developers.facebook.com/docs/whatsapp/marketing-messages-lite-api/onboarding#onboarding-business-customers) with an onboarding video tutorial.

## October 1, 2025

_MM Lite API_

Updated the [support page](https://developers.facebook.com/docs/whatsapp/marketing-messages-lite-api/support) with new troubleshooting guides.

## September 29, 2025

_MM Lite API_

Added a [Coexistence onboarding guide to MM Lite](https://developers.facebook.com/docs/whatsapp/marketing-messages-lite-api/for-solution-providers#onboard-whatsapp-business-app-users--aka--coexistence--). WhatsApp Business users can now onboard with their existing WhatsApp Business app account and phone number.

## September 3, 2025

_MM Lite API_

* On September 8th, 2025, we&#039;re launching a new [&quot;MM Lite ToS signed&quot; webhook](https://developers.facebook.com/docs/whatsapp/marketing-messages-lite-api/onboarding#receive-mm-lite-terms-of-service-signed-webhook--preferred-), which will be sent whenever a business signs the MM Lite ToS via any method (e.g. Embedded Signup, or in WhatsApp Manager). The webhook will have a more descriptive name than the existing `AD_ACCOUNT_LINKED` webhook. The older webhook will be deprecated by Jan 1, 2026.

* [Conversion metrics](https://developers.facebook.com/docs/whatsapp/marketing-messages-lite-api/viewing-metrics) will now also be available in WhatsApp Manager UI and via the WhatsApp Business Management API. This means that we&#039;re removing the ability to view MM Lite metrics for read-only Ad Accounts in Ads Manager.

* The `/marketing_messages` endpoint will [accept marketing messages for both MM Lite API and Cloud API](https://developers.facebook.com/docs/whatsapp/marketing-messages-lite-api/sending-messages#send-marketing-template-messages).

## August 22, 2025

_MM Lite API_

Added a [features page](https://developers.facebook.com/docs/whatsapp/marketing-messages-lite-api/features) comparing MM Lite and Cloud API features.

MM Lite API [uptime and availability metrics](https://developers.facebook.com/documentation/business-messaging/whatsapp/support/api-status-page) are now live on [metastatus.com](https://metastatus.com/whatsapp-business-api?fbclid=IwY2xjawM2tbNleHRuA2FlbQIxMQBicmlkETFPWE5PUldSeE95a2tuMnA1AR66setbfmUOYwOMQ3HtM7k277dWGE5sNlomsS6qAp8WTv_DlOf4Y10k6Dhf2w_aem_ieXDJ6jqZJA6QbtWWAA2Dw), providing visibility into service status.

## August 06, 2025

_MM Lite API_

Added an [image background generation](https://developers.facebook.com/docs/whatsapp/marketing-messages-lite-api/sending-messages) field for automatic creative optimization.

## July 25, 2025

_MM Lite API_

- Updated [references](https://developers.facebook.com/docs/whatsapp/marketing-messages-lite-api/guides/deep-links#form-fields) to &quot;Mobile App ID&quot; to &quot;Meta App ID&quot; when creating a template to avoid confusion.
- Added a [Template-to-ad-syncing guideline](https://developers.facebook.com/docs/whatsapp/marketing-messages-lite-api/measuring-conversion) for clients to follow to ensure all templates be set up for conversion metrics.

## July 22, 2025

_MM Lite API_

Local Storage is now available for MM Lite. If you have already enabled [Local Storage](https://developers.facebook.com/documentation/business-messaging/whatsapp/local-storage) for the Cloud API, your existing settings are automatically applied to the MM Lite API.

## July 16, 2025

_MM Lite API_

Added a troubleshooting guide on how to identify admins of a business portfolio using Meta Business Suite or the API. Both methods return the same results.

- [Meta Business Suite](https://business.facebook.com): Navigate to the Business Settings to view users with **Full Control** access.
- API: [GET /&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/whatsapp-business-account-api) and [GET /&lt;BUSINESS_PORTFOLIO_ID&gt;/business_users](https://developers.facebook.com/documentation/ads-commerce/marketing-api/reference/business/business_users) endpoints to get a list of business portfolio admins.

## July 15, 2025

_MM Lite API_

- Added [`marketing_messages_onboarding_status` field](https://developers.facebook.com/docs/whatsapp/marketing-messages-lite-api/onboarding#checking-eligibility-status) which provides more granular eligibility status data. The field will be a replacement for [`marketing_messages_api_status` field](https://developers.facebook.com/docs/whatsapp/marketing-messages-lite-api/onboarding#checking-eligibility-status--alternative-) which will be deprecated in version 24.0.
- Fixed the `marketing_messages_lite_api_status` field to correct a bug which was erroneously returning `ELIGIBLE` when it should have returned `ONBOARDED`. This field will be deprecated in version 24.0, so we recommend using the new `marketing_onboarding_status` field instead.
- Changed the `marketing_messages_api_status` field to no longer require one or more registered phone numbers, or one or more MM Lite API-ready templates, in order for `ONBOARDED` to be returned.

## July 1, 2025

_MM Lite API_

Per-message pricing now applies to MM Lite API! See [Pricing on the WhatsApp Business Platform](https://developers.facebook.com/documentation/business-messaging/whatsapp/pricing) to learn more.

## June 24, 2025

_MM Lite API_

_Permissions_

The [ads_read](https://developers.facebook.com/docs/permissions#a) permission is now optional for partners. This change impacts the [Intent API](https://developers.facebook.com/docs/whatsapp/marketing-messages-lite-api/for-solution-providers#-recommended--onboard-a-business-to-mm-lite-api-using-the-intent-api) and the [Embedded Signup](https://developers.facebook.com/docs/whatsapp/marketing-messages-lite-api/for-solution-providers#onboard-a-business-to-mm-lite-api-using-embedded-signup) onboarding flows. Prior to this change, partners had to apply for App Review to get advanced access for this new permission, regardless of whether or not their app intended to call the [Insights API](https://developers.facebook.com/documentation/ads-commerce/marketing-api/insights) for conversion metrics. Now, partners only need to request advanced access for this permission via App Review if their app intends to use the Insights API.

_Automatic Creative Optimizations_

Added [text overlays](https://developers.facebook.com/docs/whatsapp/marketing-messages-lite-api/sending-messages#text-overlays) and [image animation](https://developers.facebook.com/docs/whatsapp/marketing-messages-lite-api/sending-messages#image-animation) Automatic Creative Optimizations. Like other optimizations, these are enabled by default on all templates, but can be disabled upon template creation, or when editing a template.

## June 20, 2025

_MM Lite API_

Businesses in Russia who were previously unable to send messages via MM Lite API can now do so. Note that these businesses will not have access to some advanced features, but can still take advantage of all other benefits, such as exclusive [marketing features](https://developers.facebook.com/docs/whatsapp/marketing-messages-lite-api). For more details, see [Geographic availability of features](https://developers.facebook.com/docs/whatsapp/marketing-messages-lite-api/get-started#russia).

## June 10, 2025

_MM Lite API_

Added two new Insights API fields so you can [get read rate and click rate benchmarks via API](https://developers.facebook.com/docs/whatsapp/marketing-messages-lite-api/viewing-metrics#benchmark-metrics) instead of only via WhatsApp Manager:

- `marketing_messages_read_rate_benchmark`
- `marketing_messages_click_rate_benchmark`

## May 23, 2025

_MM Lite API_

Added [Android deep link](https://developers.facebook.com/docs/whatsapp/marketing-messages-lite-api/guides/deep-links) support.

## May 21, 2025

_MM Lite API_

Added an [Error messages](https://developers.facebook.com/docs/whatsapp/marketing-messages-lite-api/viewing-metrics#error-metrics) section to WhatsApp Manager that shows a summary of errors encountered by your templates within a given period of time.

## May 20, 2025

_MM Lite API_

Added [new error codes](https://developers.facebook.com/docs/whatsapp/marketing-messages-lite-api/support#error-codes) to help diagnose messaging errors. These will be available with Graph API version 23.0.

- `134100`
- `134101`
- `134102`
- `134103`

## April 16, 2025

_MM Lite API_

_Limited access available for [tracking click events](https://developers.facebook.com/docs/whatsapp/marketing-messages-lite-api/guides/tracking-click-events)._

We are offering a limited roll-out of webhook access to click events on marketing messages sent using MM Lite. [Read the &quot;Tracking click events&quot; page for more information](https://developers.facebook.com/docs/whatsapp/marketing-messages-lite-api/guides/tracking-click-events)

### April 11, 2025

_MM Lite API_

_Early access to Automated Creative Optimizations_

We are piloting a new optimization capability exclusive to MM Lite API (not available on Cloud API), which automatically enhances the visual appeal and engagement of marketing messages. Similarly to Advantage+ creative, this capability tests minor variations of your existing image header with different crop orientations or color filters, and automatically selects the variant which is getting the highest click-through rate over time with no input needed from you.

A small group of businesses will be getting early access to this feature starting May 5, with the ability to opt Templates out via the message template API. For more details, see [Automated Creative Optimizations](https://developers.facebook.com/docs/whatsapp/marketing-messages-lite-api/sending-messages#automatic-creative-optimizations).

Please reach out to your partner, if you would like to be included in early access to this feature.

## April 1, 2025

_MM Lite API_

_MM lite is now in Open Beta_

With this update, MM lite has:

* **Self-signup for all partners and businesses**. All businesses and solution providers (including Solution Partners, Tech Providers and Tech Partners) can now use self-serve onboarding flows to onboard to MM Lite API. See documentation on [Onboarding](https://developers.facebook.com/docs/whatsapp/marketing-messages-lite-api/onboarding) for more details.
* **Global availability**. MM Lite API is now available in all regions where Cloud API is also available. Note that while the API is available, some geographic variation of features may apply, [see details here](https://developers.facebook.com/docs/whatsapp/marketing-messages-lite-api/get-started#geographic-availability-of-features).

## March 27, 2025

_MM Lite API_

_Updates to character and emoji limits in Templates_

This update applies across all Templates on the Business Messaging API, and is not specific to MM Lite API (also applies on Cloud API).

As part of our ongoing efforts to improve the performance and user experience of our messaging platform, we are introducing changes to the body component of marketing templates via Cloud API and MM Lite API. These changes will impact the character limits and emoji usage in the body component, depending on the format and tag of the template.

**Key Updates**:

- Character limits for the body component will vary based on the template format and tag
- The number of emojis allowed in the body component may also be limited.

These updates are designed to enhance the performance and user experience of our messaging platform. While these changes may require some adjustments to your template creation process, they can ultimately lead to better performance and engagement with your customers.

## Dec 1, 2024

_MM Lite API_

_USA availability, and checking MM Lite eligibility via API_

Businesses in the USA are now eligible to use the MM Lite API.

In addition, a new MM Lite enrolment parameter allows businesses and partners to programmatically check MM Lite eligibility. See API docs for details.

## November 18, 2024

_MM Lite API_

_Reduced sync and async latency_

MM Lite team has quickly responded to feedback on &quot;async&quot; or &quot;delivery&quot; latency. This is defined as the time between an API call being received by MM Lite API, and MM Lite API dispatching a &quot;delivered&quot; webhook, assuming a user is online when the message is sent.

MM Lite previously had a p99 &quot;async&quot; delivery time of 12s, vs. p99 of 5s on Cloud API. This time has now been reduced to 9s. No action is required on a business or partner&#039;s part.

## November 15, 2024

_MM Lite API_

_New easier onboarding flow, for partner-managed businesses_

MM Lite is rolling out a new way for partners to guide a business through signing the MM Lite Terms of Service and moving to MM Lite.

In parallel with the Embedded Signup flow already available, a partner will also alternatively be able to initiate the following flow:

1. Call an &#039;Intent API&#039; endpoint to indicate a BMID the partner wishes to assist in migrating to MM Lite. (If a BMID contains any OBO WABAs, these must be migrated to &#039;shared&#039; prior to this event).
1. Admins of that BMID will receive a notification that they have been invited to start sending marketing messages with optimizations via MM Lite. This notification is in Business Manager and via email.
1. Once accepted, and MM Lite setup and Ad account syncing is complete, a webhook will be triggered to the partner and all subscribers indicating that MM Lite onboarding is complete, and including the linked Ad IDs.
1. The partner may now call the &quot;send&quot; endpoint of MM Lite on this business&#039; behalf, and call an API to fetch an updated token with permission to access the business&#039; Ad account metrics.

See API docs for details on this new onboarding flow.

## November 8, 2024

_MM Lite API_

_App Conversion reporting now supported_

Businesses can now use MM Lite API to measure when marketing messages lead users to perform app events, such as purchases, searches, or achieving levels in games. See Integrate with App Conversions for details.

## November 1, 2024

_MM Lite API_

_Metrics in WhatsApp Manager_

To represent this new conversation type, MM Lite API conversations are available in every surface where reporting is offered:

1. Ads Manager UI [recommended]
1. Marketing API &quot;Insights API&quot; [recommended]
1. WhatsApp Manager UI &quot;WABA Insights&quot; page and &quot;Template Insights&quot; page
1. Business Management API
1. Pricing webhooks

For full details on how to see MM Lite API metrics via API and in pricing webhooks, consult the MM Lite API docs.

- In the Marketing API &quot;Insights API&quot; response, MM Lite events can be return using fields named `marketing_messages_[event]`
- In the Business Management API &quot;Template Analytics&quot; endpoint, MM Lite events can be returned using the query parameter `MARKETING_MESSAGES_LITE_API`
- In the Business Management API &quot;Conversation Analytics&quot; endpoints, MM Lite events can be returned using the `product_type` query parameter `MARKETING_LITE&#039;
- In message status webhooks, the `conversation:origin:type` and `pricing:category` fields will show as `marketing_lite`

Businesses can now see MM Lite metrics as &quot;Marketing - lite&quot; in the WhatsApp Manager UI:

We recommend you integrate with the Marketing API &quot;Insights API&quot; for MM Lite Metrics, and encourage end businesses to log into Ads Manager UI to see their metrics, instead of using the WhatsApp business surface. Ads Mgr UI and Insights API show conversion metrics that are not available on WhatsApp surfaces, and will continue to support new metrics and features as the primary surface for MM Lite API reporting as the API grows.

API docs for partners have been updated to reflect how to fetch MM Lite metrics via API.

API docs have been updated to reflect how to fetch MM Lite metrics via API, see [Viewing metrics](https://developers.facebook.com/docs/whatsapp/marketing-messages-lite-api/viewing-metrics).

## October 30, 2024

_MM Lite API_

_Exclusive feature: TTLs for Marketing messages_

We continue to invest to improve consumer experiences and business outcomes. We are introducing customizable message validity periods (time-to-live or TTL) for marketing messages on MM Lite API, to ensure marketing messages are always timely and relevant for users thus performant for businesses. We are also updating our customizable range for TTL for utility and authentication templates, to provide businesses more control and flexibility.

- Marketing: From 12 hours to 30 days, for businesses on MM Lite API
- Utility: From 30 seconds to 12 hours, for businesses on Cloud API
- Authentication: From 30 seconds to 15 minutes, for businesses on Cloud API

Businesses can customize the TTL of marketing, utility and authentication templates during template creation via WhatsApp Manager (via pre-set increments) and via API (in 1-second increments). This is reflected in our [dev docs](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/overview#time-to-live--customization-and-defaults) and [Business Help Center](https://www.facebook.com/business/help/1305007343713790).

## October 15, 2024

_MM Lite API_

_New rate cards and pricing category for MM Lite_

Marketing message conversations initiated via MM Lite API are counted and billed separately from Marketing message conversations initiated via Cloud API. This includes updates to pricing webhooks. For details, please see [Pricing](https://developers.facebook.com/docs/whatsapp/marketing-messages-lite-api/mm-lite-api-pricing).

## June 25, 2024

_MM Lite API_

_Show MM Lite metrics on the Template Analytics API_

MM Lite metrics are now available from the Template Analytics API endpoint. See documentation for details.
