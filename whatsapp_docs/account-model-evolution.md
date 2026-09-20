# Updates to WhatsApp Business accounts


The WhatsApp Business account is evolving into two unique accounts: one for phone numbers, one for template messages and billing. With the new account model, clients and direct developers can share their phone number with multiple partners, with separate billing for each partner. WhatsApp users see a single trusted contact regardless of how many partners are on the number.

This document is for Solution Partners and direct developers building on the WhatsApp Business Platform. It explains the new account model, what it means for your integration, and how to prepare. This feature is currently in beta. All information described in this document is subject to change.

## Why this is changing

Today, the WABA bundles phone numbers, templates, billing, webhooks, and partner access into a single entity. As a result, working with multiple partners requires a separate phone number for each partner. This means clients appear as multiple contacts in a WhatsApp user&#039;s chat list — one for each phone number — which creates a fragmented experience for WhatsApp users and reflects poorly on the client&#039;s brand. The new model addresses these limitations by separating business identity from template messages and billing.

The new account model benefits both partners and direct developers:

**For partners:**

- **Onboard clients faster** — Onboard clients with existing API integrations more easily. No migration, no number provisioning, no downtime.
- **Grow your client base** — Clients currently sharing a phone number with an agency or a direct API integration can now add you as a partner on the same number. That means more opportunities to expand your customer base beyond first-time WhatsApp Business Platform users.
- **Win through specialization** — Position your platform for specific use cases — marketing, service, authentication, commerce, or calling — to stand out in the ecosystem while other partners handle the rest. Clients using multiple messaging use cases see 2x improvement in customer lifetime value and [20–30% higher engagement](https://business.whatsapp.com/resources/resource-library/bcg-leading-in-the-age-of-messaging). More use cases per client means more billable conversations flowing through your platform.
- **Flexible billing currencies** — Clients can maintain a single phone number while using different billing currencies for different markets. Each currency gets its own Messaging Account, eliminating the need for separate phone numbers per currency. This requires implementing `messaging_account_id` support. See [Messaging](https://developers.facebook.com/documentation/business-messaging/whatsapp/account-model-evolution/messaging/) for details.

**For direct developers:**

- **Add partners without losing your number** — Share your phone number with a partner to outsource specific use cases (for example, marketing messages) while keeping your direct integration for everything else.
- **Isolated billing** — Each integration gets its own Messaging Account, so billing stays separate between your direct integration and any partners you work with.

![Before and after comparison of the WhatsApp Business account model, showing how a single WABA splits into a WhatsApp Business Account (WAAC) for phone numbers and a Messaging Account (PMA) for templates and billing.](https://developers.facebook.com/images/business-messaging/wame-new-model-v2.png)

Each of these benefits is made possible because the new model separates a business&#039;s identity (phone numbers) from template messages and billing. One trusted phone number works across all integrations, and each integration gets its own isolated templates, billing, and analytics.

## New account model

The legacy WhatsApp Business account (WABA) is becoming two distinct accounts. If you have an existing integration, the simplest way to understand the change:

- **WhatsApp Business Account** — a container to hold a single business phone number and in the future a single business username. Your existing phone numbers will be moved into WhatsApp Business Accounts automatically at **Phase 1 — General availability**. WhatsApp Business Accounts have their own IDs, but these are not available until **Phase 2 — New Graph API version**. Your existing phone number IDs continue to work with all APIs, but, starting with **Phase 3 — Mandatory transition**, all APIs that target the business phone number ID in the URL path (including the Messages API and Marketing Messages API) must target the WhatsApp Business Account ID (WAAC) instead.
- **Messaging Account** — a container for your templates, billing, and webhook subscriptions. Your existing WhatsApp Business Account (WABA) transfers to your Messaging Account (same ID) at **Phase 1 — General availability**.

Your existing IDs and API calls continue to work. Meta handles the account migration automatically as part of the **Phase 1 — General availability** launch.

![Before and after comparison showing how one phone number per integration changes to one phone number for multiple integrations with separate billing.](https://developers.facebook.com/images/business-messaging/wame-why-the-change-v1.png)

### WhatsApp Business Account (WAAC)

The WhatsApp Business Account represents a business&#039;s identity (what WhatsApp users see and interact with in the WhatsApp app). It contains:

- **Phone numbers** — the business&#039;s registered WhatsApp numbers
- **Business usernames** — unique identifiers that will supplement phone numbers as an alternate way for WhatsApp users to reach a business (future capability)
- **Business profile information**
- **Product catalogs**

A WAAC is always owned by the client or direct developer and can be shared across direct integrations and multiple partners. When a client shares their WAAC with you, you gain access to the phone number it contains.

Since each WAAC contains a single phone number, multiple WAACs can be linked together in a WAAC hierarchy — a primary WAAC with linked WAACs (sometimes called a &quot;phone set&quot;) — allowing clients to manage a collection of phone numbers as a group. When a client shares a primary WAAC with you, you get access to all linked WAACs and their phone numbers in the set.

Note that WhatsApp Business Account (WAAC) is referred to as &quot;WhatsApp Business Account&quot; in the Terms of Service.

### Messaging Account

The Messaging Account handles template messages and billing. It contains:

- **Message templates**
- **Billing and payment methods** (attached via Meta Business Suite or an API call)

A Messaging Account can be owned by the client or by a partner. Each partner working with a client gets their own Messaging Account, which ensures complete isolation of templates, billing, and metrics between partners. Existing WABA IDs remain the same and denote Messaging Account IDs.

A business phone number can be associated with multiple Messaging Accounts. See [Messaging](https://developers.facebook.com/documentation/business-messaging/whatsapp/account-model-evolution/messaging/) for details on how billing is resolved when multiple Messaging Accounts share a phone number.

**Template isolation**: Templates belong to the Messaging Account in which they were created and are not shared across Messaging Accounts. If you need the same template in multiple Messaging Accounts, you must create it in each Messaging Account separately or use the Template Migration API to copy templates between Messaging Accounts. When templates are migrated to a new Messaging Account, their quality ratings and messaging tier reset to baseline, requiring a ramp-up period of approximately 2–4 weeks.

**Billing isolation**: Each Messaging Account has its own payment method, completely independent from other Messaging Accounts and from ad accounts. If you attach your credit line to a client owned Messaging Account, that Messaging Account cannot be shared with another partner.

**Faster partner integration**: Because the new model decouples phone numbers from Messaging Accounts, clients can add or change partners without migrating their phone number. Existing conversations are uninterrupted when new partners are added. The phone number migration process that previously acted as a natural switching barrier no longer applies. If you extend credit lines, factor this change into your risk management.

Note that Messaging Account is referred to as &quot;Messaging Account&quot; in the Terms of Service.

## What stays the same

Most of the platform surface you use today is unchanged:

- **Cloud API endpoints** — same paths, payloads, and request/response formats
- **Business Management API** — all WhatsApp Business account endpoints continue to work
- **Marketing Messages API** — same endpoints and behavior for WhatsApp marketing messages
- **Phone number IDs** — your existing IDs continue to work
- **Access tokens** — same authentication model
- **Template workflows** — create, edit, delete, and submit for review work the same way
- **Pricing** — conversation-based billing is unaffected
- **Messages API** — the `POST /&lt;BUSINESS_PHONE_NUMBER_ID&gt;/messages` endpoint continues to work the same way
- **Phone number registration** — same process. Re-registering an already-registered phone number succeeds without resetting the PIN
- **Phone number deregistration** — no changes at this time. Any partner can deregister a phone number
- **Webhook subscriptions** — you continue to [subscribe](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/subscribed-apps-api) to the `whatsapp_business_account` topic using your Messaging Account ID (WABA ID)
- **Business verification** — same process
- **Display names, quality ratings, and messaging limits** — unchanged
- **Throughput** — remains at the phone number level. When multiple partners share a phone number, they share the phone number&#039;s throughput capacity
- **Messaging limits** — remain at the business portfolio level. When multiple partners share a phone number, the messaging limit (unique users per 24-hour period) is shared across all partners on that number

## What changes

### Account structure

Your existing WABA is transparently migrated into a WAAC hierarchy (holding phone numbers and usernames) and a Messaging Account hierarchy (holding template messages and billing). This migration preserves all your data — phone numbers, templates, business profiles, and payment methods carry over without disruption. Existing clients do not need to re-onboard; Meta handles the migration automatically.

This migration applies to all WhatsApp Business Accounts, including accounts owned by direct developers, and accounts owned by businesses that use the WhatsApp Business app and have been onboarded by a partner to Cloud API (coexistence businesses). Existing WhatsApp Business app phone numbers using coexistence are migrated into the WhatsApp Business Account model, and new WhatsApp Business app phone numbers onboard directly into the new model.

After migration, [Meta Business Suite](https://business.facebook.com) displays two separate tabs: **WhatsApp Accounts** (phone numbers/usernames) and **Messaging Accounts** (template messages and billing), replacing the current single WABA view.

![Meta Business Suite showing separate WhatsApp Business Accounts and Messaging Accounts tabs under the Accounts menu.](https://developers.facebook.com/images/business-messaging/wame-mbs-acccounts-panel-callout.png)

When you are removed from a WhatsApp Business Account, an `account_update` webhook is sent to notify your app. Email notifications are also sent, though improvements to notification reliability are ongoing.

#### Where assets live after migration

| Asset | Before (WABA) | After (new model) |
|---|---|---|
| Phone numbers | Under WABA | Under WAAC |
| Message templates | Under WABA | Under Messaging Account (per-partner) |
| Billing and payment methods | Under WABA | Under Messaging Account |
| Conversation history | Under phone number | Under WAAC / phone number (unchanged) |
| Quality rating | Under phone number | Under WAAC / phone number (unchanged) |
| Business verification | Under Business Portfolio | Under Business Portfolio (unchanged) |
| Access tokens | Under app + WABA | Under app + Messaging Account (same tokens work) |
| Webhook subscriptions | Under WABA | Under Messaging Account |

#### Billing in multi-partner setups

Each partner is billed through their own Messaging Account, regardless of which partner is handling the conversation. When you send a message, the charge goes to the Messaging Account associated with your app. Delivery receipts return to the sending app regardless of which Messaging Account was charged.

### Embedded Signup

Updated Embedded Signup flows automatically create a separate Messaging Account per partner and return the ID in the existing WABA ID field — no changes to your existing integration needed. For details on phased rollout and new callback fields, see [Onboarding changes](https://developers.facebook.com/documentation/business-messaging/whatsapp/account-model-evolution/onboarding/).

### Registration

Clients may onboard your platform using a phone number already registered and shared with another partner. The re-registration call will silently pass for the pre-registered number and will return an API response clearly denoting when the PIN is not reset. Partners can change the display name without knowing the PIN.

### Messaging Account parameter

A `messaging_account_id` parameter becomes available on Messages API calls. It is optional at **Phase 1 — General availability** and required in multi-Messaging Account scenarios starting with **Phase 2 — New Graph API version**. The parameter `paid_messaging_account_id` is supported as a deprecated, backward-compatible alias; migrate to `messaging_account_id` by December 31, 2026, after which it is planned for removal in a future Graph API version. For details on when it is required, precedence when both parameters are supplied, multi-Messaging Account scenarios, and sample requests, see [Messaging](https://developers.facebook.com/documentation/business-messaging/whatsapp/account-model-evolution/messaging/).

### WhatsApp Account (WAAC) ID

Starting with **Phase 2 — New Graph API version**, a new WhatsApp Account ID is introduced alongside the existing phone number ID:

- WhatsApp Account IDs and phone number IDs have a 1:1 relationship, but are different values
- All existing phone-number-level APIs continue to work with phone number IDs
- New APIs support both WhatsApp Account IDs and phone number IDs
- New API endpoints at the WhatsApp Account level become available with **Phase 2 — New Graph API version** alongside existing phone-number-level endpoints

### New APIs

New endpoints for managing WhatsApp Accounts and Messaging Accounts will be available starting with **Phase 2 — New Graph API version**. Details will follow as the endpoints are finalized.

Template management endpoints (`GET/POST/DELETE /&lt;WABA_ID&gt;/message_templates`) remain on the Messaging Account and do not change.

## Rollout timeline

Use this table to plan integration work. The migration is backward-compatible at Phase 1; mandatory cutover is at Phase 3.

| Phase | Timing | What ships | What you need to do |
| --- | --- | --- | --- |
| **Phase 1 — General availability** | H2 2026 | **Meta handles the account migration automatically**&lt;br&gt;• Embedded Signup creates a separate Messaging Account per partner automatically whenever a client completes the onboarding flow&lt;br&gt;• Meta Business Suite shows WhatsApp Accounts and Messaging Accounts tabs, instead of a single WhatsApp Accounts tab&lt;br&gt;&lt;br&gt;**Your existing IDs and API calls continue to work**&lt;br&gt;• Messaging Accounts keep WABA IDs, and now contain only templates, payment settings, analytics, and webhook subscriptions info&lt;br&gt;• WAACs created (but IDs are still unavailable)&lt;br&gt;• The optional `messaging_account_id` parameter is available on Messages API (`paid_messaging_account_id` is supported as a deprecated, backward-compatible alias)&lt;br&gt; | No code changes required, except for [multiple Messaging Account scenarios](https://developers.facebook.com/documentation/business-messaging/whatsapp/account-model-evolution/messaging/#multiple-messaging-account-scenarios). |
| **Phase 2 — New Graph API version** | H1 2027 | • Newest Messages API version requires `messaging_account_id` for multi-Messaging Account setups&lt;br&gt;• Older Messages API versions still accept calls without it. Meta attempts to resolve the correct Messaging Account from your app&#039;s association and returns an error if unable to&lt;br&gt;• New Graph API endpoints for managing WAACs and Messaging Accounts&lt;br&gt;• New `whatsapp_account` webhook topic&lt;br&gt;• Separate app permissions for WAAC and Messaging Account scopes&lt;br&gt;• Distinct WAAC ID returned alongside phone number ID&lt;br&gt;• Embedded Signup returns WAAC ID alongside the ID of the phone number it contains upon successful completion&lt;br&gt; | • If your app may hold multiple Messaging Accounts on the same phone number, adopt the new Messages API version and start sending `messaging_account_id`&lt;br&gt;• Optionally, subscribe to the new webhook topic and adopt the new endpoints if you need finer-grained control&lt;br&gt; |
| **Phase 3 — Mandatory transition** | H1 2028 | • All Messages API versions require `messaging_account_id` for multi-Messaging Account setups&lt;br&gt;• All APIs that target a phone number ID in the endpoint path, or accept a phone number ID as a query parameter, must use WAAC IDs instead&lt;br&gt; | Plan engineering work in 2027 to be on the new APIs before this date. |

## Learn more

- [Onboarding changes](https://developers.facebook.com/documentation/business-messaging/whatsapp/account-model-evolution/onboarding/) — how client onboarding changes with the new account model
- [Managing messaging accounts](https://developers.facebook.com/documentation/business-messaging/whatsapp/account-model-evolution/messaging/) — how the new account model affects messaging
- [WhatsApp Business Accounts](https://developers.facebook.com/documentation/business-messaging/whatsapp/whatsapp-business-accounts) — current account documentation
- [Webhooks](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/overview) — current webhook documentation
- [About the WhatsApp Business Platform](https://developers.facebook.com/documentation/business-messaging/whatsapp/about-the-platform) — platform overview

## Document changelog

| Date | Change |
|---|---|
| May 1, 2026 | Document released |
