# Managing messaging accounts


A business phone number can have multiple Messaging Accounts, and each Messaging Account belongs to an app. When each Messaging Account on a phone number is shared with a unique partner, the Messages API does not require the Messaging Account ID — Meta determines the correct Messaging Account from the app making the request. When multiple Messaging Accounts are shared with a single partner, you must include `messaging_account_id` to specify which Messaging Account to use for billing and analytics.

The parameter is optional at **Phase 1 — General availability** and becomes required for multi-Messaging Account scenarios starting with **Phase 2 — New Graph API version**. If you only hold one Messaging Account per WhatsApp Account, no changes are needed until **Phase 3 — Mandatory transition**.

&gt; **Note:** `paid_messaging_account_id` is a deprecated, backward-compatible alias for `messaging_account_id`, so new integrations should use `messaging_account_id`. Migrate to `messaging_account_id` by December 31, 2026; after that, `paid_messaging_account_id` is planned for removal in a future Graph API version. If you supply both `messaging_account_id` and `paid_messaging_account_id`, `messaging_account_id` takes precedence and `paid_messaging_account_id` is ignored.

## Multiple messaging account scenarios

Your app needs to include `messaging_account_id` when it has access to more than one Messaging Account on the same phone number. There are three scenarios where this occurs:

1. **Direct developer with a partner** — You are a direct developer who uses the same phone number with two or more Messaging Accounts. For example, you outsource marketing messages to a partner, which requires two Messaging Accounts: one for you, and one for the partner. Since your app is associated with both Messaging Accounts, you must include the appropriate Messaging Account ID when sending messages.

2. **Partner with multiple shared Messaging Accounts** — A client shares two or more Messaging Accounts with you, each associated with the same business phone number. You must include the appropriate Messaging Account ID when sending messages.

3. **Multiple billing currencies** — A client uses the same phone number across multiple markets with different billing currencies. Each currency requires its own Messaging Account. The client shares all of these Messaging Accounts with you, so you must specify which Messaging Account to bill when sending.

To get a list of Messaging Account IDs that have been shared with your business portfolio, use the [Client WhatsApp Business Accounts API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/business/client-whatsapp-business-accounts-api).

## When the parameter is required

| Phase | Timing | Messages API version | Behavior |
|---|---|---|---|
| **Phase 1 — General availability** | H2 2026 | All versions | Optional. If omitted, Meta determines the correct Messaging Account from the app making the request. |
| **Phase 2 — New Graph API version** | H1 2027 | Newest version | **Required** when your app has access to more than one Messaging Account on the same WhatsApp Account. Meta rejects calls that omit `messaging_account_id`. |
| | | Older versions | Still optional. Meta attempts to resolve the correct Messaging Account from your app&#039;s association and returns an error if unable to. |
| **Phase 3 — Mandatory transition** | H1 2028 | All versions | All Messages API versions require `messaging_account_id` for multi-Messaging Account setups. |

## Example syntax

To charge a specific Messaging Account, include `messaging_account_id` in the request body:

```html
curl -X POST \
  &#039;https://graph.facebook.com/v22.0/&lt;BUSINESS_PHONE_NUMBER_ID&gt;/messages&#039; \
  -H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039; \
  -H &#039;Content-Type: application/json&#039; \
  -d &#039;&#123;
    &quot;messaging_product&quot;: &quot;whatsapp&quot;,
    &quot;to&quot;: &quot;&lt;WHATSAPP_USER_PHONE_NUMBER&gt;&quot;,
    &quot;messaging_account_id&quot;: &quot;&lt;MESSAGING_ACCOUNT_ID&gt;&quot;,
    &quot;type&quot;: &quot;template&quot;,
    &quot;template&quot;: &#123;
      &quot;name&quot;: &quot;&lt;TEMPLATE_NAME&gt;&quot;,
      &quot;language&quot;: &#123; &quot;code&quot;: &quot;&lt;LANGUAGE_CODE&gt;&quot; &#125;
    &#125;
  &#125;&#039;
```

## Billing and receipts

### Check payment methods

Use the Messaging Account API to check which payment method is attached to a Messaging Account:

```html
curl -X GET \
  &#039;https://graph.facebook.com/v22.0/&lt;MESSAGING_ACCOUNT_ID&gt;?fields=primary_funding_id&#039; \
  -H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039;
```

If `primary_funding_id` is absent, the Messaging Account has no payment method attached and cannot send paid messages.

### Delivery receipts

Delivery receipts return to the sending app. The payload format is unchanged. If you bill end customers based on delivery receipt volume, query `primary_funding_id` on the Messaging Account you sent from to determine which line of credit was charged.

## Learn more

- [Updates to WhatsApp Business accounts](https://developers.facebook.com/documentation/business-messaging/whatsapp/account-model-evolution/) — overview of the new account model
- [Onboarding changes](https://developers.facebook.com/documentation/business-messaging/whatsapp/account-model-evolution/onboarding/) — how client onboarding changes with the new account model
