# Onboarding changes


With the new account model, onboarding flows automatically create a separate Messaging Account per partner and return the ID in the existing WABA ID field. The asset creation and sharing logic is the same — a WhatsApp Business account holds the phone number, and each partner gets their own Messaging Account.

## Embedded Signup

Under the new account model, all available Embedded Signup flows reflect the following changes:

- &quot;WhatsApp Business account&quot; and &quot;Messaging Account&quot; appear in Embedded Signup screens and in Meta Business Suite for onboarded clients
- A WhatsApp Business account and Messaging Account are automatically created when a client completes the flow
- `waac_id` starts appearing in [message events](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/implementation#session-logging-message-event-listener) starting at **Phase 2 — New Graph API version**

## Partner-initiated account creation

With [partner-initiated account creation](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/partner-initiated-waba-creation), the same asset creation and sharing logic applies. Starting at **Phase 1 — General availability**, when a client accepts the partner&#039;s request to create and share a Messaging Account, they must provide a phone number. Adding a phone number is no longer optional.

## Onboarding changes by phase

Onboarding updates roll out in phases. Each phase is additive — your existing integration continues to work without code changes.

| Phase | Timing | What ships | What you need to do |
| --- | --- | --- | --- |
| **Phase 1 — General availability** | H2 2026 | • Embedded Signup creates a separate Messaging Account per partner automatically whenever a client completes the onboarding flow&lt;br&gt;• &quot;Add phone number later&quot; is removed; the flow is phone-number-first&lt;br&gt;• [Partner-initiated account creation](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/partner-initiated-waba-creation): clients must now provide a phone number when accepting a partner&#039;s request to create and share a Messaging Account&lt;br&gt;• Clients already onboarded with one partner can onboard with a different partner using their existing phone number. The `waac_id` is not yet included in message events (available starting Phase 2)&lt;br&gt; | No code changes, but `waba_id` in [message events](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/implementation#session-logging-message-event-listener) now refers to a Messaging Account ID. |
| **Phase 2 — New Graph API version** | H1 2027 | • `waac_id` (WhatsApp Account ID) now included in [message events](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/implementation#session-logging-message-event-listener) alongside `cs_id`&lt;br&gt;• New Embedded Signup settings allow you to control if and where the Messaging Account gets created (details to follow)&lt;br&gt; | • Start capturing `waac_id` (WhatsApp Account ID) alongside `cs_id` (business phone number ID) and `waba_id` (Messaging Account ID) in [message events](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/implementation#session-logging-message-event-listener)&lt;br&gt;• Can now optionally use WhatsApp Account IDs and Messaging Account IDs with new Graph API endpoints&lt;br&gt; |
| **Phase 3 — Mandatory transition** | H1 2028 | • All Messages API versions require `messaging_account_id` (`waba_id` value in message events) for [multiple Messaging Account scenarios](https://developers.facebook.com/documentation/business-messaging/whatsapp/account-model-evolution/messaging/#multiple-messaging-account-scenarios)&lt;br&gt;• All APIs that target a phone number ID in the endpoint path, or accept a phone number ID as a query parameter, now must use WAAC IDs instead (`waac_id` value in message events)&lt;br&gt; | Plan engineering work in 2027 to be on the new APIs before this date. |

## Learn more

- [Updates to WhatsApp Business accounts](https://developers.facebook.com/documentation/business-messaging/whatsapp/account-model-evolution/) — overview of the new account model
- [Managing messaging accounts](https://developers.facebook.com/documentation/business-messaging/whatsapp/account-model-evolution/messaging/) — how the new account model affects messaging
- [Embedded Signup overview](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/overview/) — current Embedded Signup documentation
