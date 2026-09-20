# Onboard



Onboarding to the Marketing Messages API for WhatsApp (MM API for WhatsApp) is a low-effort upgrade to sending marketing messages with optimizations on Cloud API. See the directions below to onboard your business, whether you integrate with the API directly or work with a partner.

When a business registers for the MM API for WhatsApp, read-only Ad accounts are created that are linked to each of the marketing templates that exist under their business portfolio.

These linked accounts allow a business to:

* fetch their MM API for WhatsApp insights from the Marketing API &quot;Insights API&quot; to view the same

These read-only ad accounts are kept in sync with any changes to marketing templates, so that any changes to marketing templates are reflected in the linked ad entity.

Follow the steps below to Onboard to MM API for WhatsApp.

## Eligibility requirements

In order to use the Marketing Messages API for WhatsApp (MM API for WhatsApp), a business must comply with applicable legal, vertical, and content restrictions (country dependent) outlined in [WhatsApp Business Messaging Policies](https://business.whatsapp.com/policy).

In addition, the following requirements must be met:

- WABA is active and not restricted from messaging due to any violations
- WABA tax country is not in sanctioned regions
- Owner Business country is not in sanctioned regions

MM API for WhatsApp will continuously update vertical eligibility and policies to comply with various policies and regulations internationally, so these requirements may change.

### Check WABA onboarding status and eligibility

Use the [WhatsApp Business Account API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/whatsapp-business-account-api#get-version-waba-id) and request the `marketing_messages_onboarding_status` field to check the MM API for WhatsApp eligibility status of a WABA.

Eligible WABAs have this field set to `ELIGIBLE`. If this value is set to `ONBOARDED`, it means the business customer WABA has already been onboarded. See the [WhatsApp Business Account API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/whatsapp-business-account-api#get-version-waba-id) reference for all possible values and their meanings.

**Example request**

```curl
curl &#039;https://graph.facebook.com/v25.0/25002526842541/?fields=marketing_messages_onboarding_status&#039; \
    -H &#039;Authorization: Bearer EAAAl...&#039;
```

**Example response**

```json
&#123;
  &quot;marketing_messages_onboarding_status&quot;: &quot;ELIGIBLE&quot;,
  &quot;id&quot;: &quot;25002526842541&quot;
&#125;
```

You can also use the [Client WhatsApp Business Accounts API](https://developers.facebook.com/documentation/ads-commerce/marketing-api/reference/business/client_whatsapp_business_accounts) with the following filtering to get a list of all eligible WABAs that have been shared with you.

**Request syntax**

```html
GET /&lt;BUSINESS_PORTFOLIO_ID&gt;/client_whatsapp_business_accounts
  ?filtering=[
    &#123;
      &#039;field&#039;:&#039;marketing_messages_onboarding_status&#039;,
      &#039;operator&#039;:&#039;IN&#039;,
      &#039;value&#039;:[&#039;ELIGIBLE&#039;]
    &#125;
  ]
```

**Example request**

```curl
curl -g &#039;https://graph.facebook.com/v25.0/19502398688333/client_whatsapp_business_accounts?filtering=[&#123;&#039;field&#039;:&#039;marketing_messages_onboarding_status&#039;,&#039;operator&#039;:&#039;IN&#039;,&#039;value&#039;:[&#039;ELIGIBLE&#039;]&#125;]&#039; \
    -H &#039;Authorization: Bearer EAAAj...&#039;
```

**Example response**

```json
&#123;
  &quot;data&quot;: [
    &#123;
      &quot;id&quot;: &quot;46302397361990&quot;,
      &quot;name&quot;: &quot;San Andreas Roofing&quot;,
      &quot;timezone_id&quot;: &quot;1&quot;,
      &quot;message_template_namespace&quot;: &quot;93d3e793_8a4f_49c4_b903_fd72aac80f71&quot;
    &#125;
  ]
&#125;
```

### Checking eligibility status (alternative)

**Warning:** This field will be deprecated in version 24.0. Use the [`marketing_messages_onboarding_status` field](#check-waba-onboarding-status-and-eligibility) instead.

You can use the [WhatsApp Business Account API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/whatsapp-business-account-api#get-version-waba-id) and request the [`marketing_messages_lite_api_status`](#check-waba-onboarding-status-and-eligibility) field to get eligibility status, but this field will be deprecated at a future date, so use the [method above](https://developers.facebook.com/documentation/business-messaging/whatsapp/marketing-messages/onboarding#eligibility-requirements) instead.

```html
GET /&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;?fields=marketing_messages_lite_api_status
```

For partner-managed WABAs, businesses can find eligible WABAs using the following endpoint:

```html
GET /&lt;BUSINESS_ID&gt;/client_whatsapp_business_accounts?fields=marketing_messages_lite_api_status
```

See the [WhatsApp Business Account API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/whatsapp-business-account-api#get-version-waba-id) reference for a list of returnable values and their meanings.

### If you want to check ToS and intent request status for the Meta Business Suite

Use the [Business API](https://developers.facebook.com/documentation/ads-commerce/marketing-api/reference/business) and request the `marketing_messages_onboarding_status` field to check the MM API for WhatsApp eligibility status.

**Permission**
* `business_management`

#### Example request

```curl
curl &quot;https://graph.facebook.com/v24.0/52002526842524351/?fields=marketing_messages_onboarding_status&quot; \
-H &#039;Authorization: Bearer EAAAl...&#039;
```

#### Example response

```json
&#123;
  &quot;marketing_messages_onboarding_status&quot;:
   &#123;
      &quot;status&quot;: &quot;TERM_OF_SERVICE_SIGNED&quot;,
      &quot;time&quot;: &quot;2025-10-07&quot;
   &#125;
&#125;
```

Use the [WhatsApp Business Account API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/whatsapp-business-account-api#get-version-waba-id) and request the `owner_business_info` field to check the onboarding status of the WABA.

**Permissions**
* `whatsapp_business_management`
* `whatsapp_business_messaging`

#### Example request

```curl
curl GET &quot;https://graph.facebook.com/v24.0/69843579834234?fields=owner_business_info&quot; \
-H &#039;Authorization: Bearer EAAAl...&#039;
```

#### Example response

```json
&#123;
  &quot;owner_business_info&quot;: &#123;
    &quot;name&quot;: &quot;WhatsApp PaidSend Testing&quot;,
    &quot;id&quot;: &quot;&lt;BM_ID&gt;&quot;,
    &quot;marketing_messages_onboarding_status&quot;: &#123;
     &quot;status&quot;: &quot;TERM_OF_SERVICE_SIGNED&quot; | &quot;REQUEST_SENT&quot; | &quot;NOT_STARTED&quot;
     &quot;time&quot;: &quot;2025-08-13&quot;
    &#125;
  &#125;,
&#125;
```

## Register a phone number on Cloud API

In order to send a message via MM API for WhatsApp, a business phone number must also be registered on Cloud API. MM API for WhatsApp and Cloud API are used together on the same phone number:

- Cloud API allows a business to send Authentication, Service, Utility, and non-Optimized Marketing template messages and freeform messages, and receive inbound messages from consumers on a business phone number.

- MM API for WhatsApp allows a business to send marketing messages with optimizations, over the same phone number as is registered on Cloud API.

WhatsApp Business phone numbers that are not registered on Cloud API cannot be used with MM API for WhatsApp.

If a business phone number is already registered on Cloud API, phone number verification is not required when registering for MM API for WhatsApp, as no new phone numbers are registered during the MM API for WhatsApp registration process. Existing Phone Numbers remain registered on Cloud API, and will now be eligible to use MM API for WhatsApp in addition to and simultaneously with Cloud API for sending marketing messages.

## For partners

If you are a partner onboarding your end businesses, refer to [onboard business customers](https://developers.facebook.com/documentation/business-messaging/whatsapp/marketing-messages/onboard-business-customers).

## Onboarding business customers

You can instruct your business customers to have someone with full control to the business portfolio to accept the Terms of Service and onboard MM API for WhatsApp via WhatsApp Manager.

1. Open WhatsApp Manager &gt; Overview.
2. In the Alerts section, click Accept terms to get started for Marketing Messages API for WhatsApp.
3. Follow the steps to finish signing MM API for WhatsApp Terms of Service.

Your business customers should be able to start sending messages via MM API for WhatsApp.

If you are unable to access your WhatsApp Manager, [find your business portfolio admin here](https://developers.facebook.com/documentation/business-messaging/whatsapp/support#i-can-t-find-an-admin-user-at-my-company-to-onboard-to-mm-api-for-whatsapp).

## For business customers without a partner

If your business directly integrates with Cloud API without a partner, follow the instructions below to accept the Terms of Service and onboard to MM API for WhatsApp.

- Navigate to the **[App Dashboard](https://developers.facebook.com/apps)** &gt; **WhatsApp** &gt; **Quickstart** panel.
- On the **Quickstart** page, locate the &quot;Improve ROI with Marketing Messages API for WhatsApp&quot; card and click the &quot;Get started&quot; button.
- Click on &quot;Continue to integration guide&quot; to accept the Terms of Service

## Sharing event activity

Once your business is onboarded, message status events (delivery status, read, clicked) will automatically be shared with Meta as part of event activity. Meta does not sell your or your subscribers&#039; data; this data is used solely to optimize the performance of marketing campaigns.

### Manage via WhatsApp Account settings

If you wish to disable sharing event activity, toggle it off via [WhatsApp Business account setting](https://business.facebook.com/latest/settings/whatsapp_account).

### Configure via API

You can also customize sharing event activity on a per-message basis by including the `&lt;message_activity_sharing&gt;` parameter and setting it to a boolean (True/False) in the `marketing_messages` API call payload. The API call overrides the default account configuration for your WhatsApp Business account.

Use the Marketing Messages API to send a message to a WhatsApp user.

### Request syntax

```html
curl &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;WHATSAPP_BUSINESS_PHONE_NUMBER_ID&gt;/marketing_messages&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039; \
-d &#039;
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;recipient_type&quot;: &quot;individual&quot;,
  &quot;to&quot;: &quot;&lt;WHATSAPP_USER_PHONE_NUMBER&gt;&quot;,
  &quot;message_activity_sharing&quot;: &quot;&lt;BOOLEAN&gt;&quot;,
  &quot;type&quot;: &quot;&lt;MESSAGE_TYPE&quot;,
  &quot;&lt;MESSAGE_TYPE&quot;:&quot;&lt;MESSAGE_CONTENTS&gt;&quot;
&#125;
```

## Receive MM API for WhatsApp Terms of Service signed webhook (preferred)

Note: The ToS event value will be available from September 8th, 2025. Refer to the legacy webhook below.

When the MM API for WhatsApp Terms of Service (ToS) is signed for a business, a new [`account_update`](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/account_update) webhook will be sent for each WhatsApp Business account (WABA) under your business portfolio. The webhook indicates that the WABA&#039;s business has successfully accepted the MM API for WhatsApp ToS. When the webhook is triggered, your WABA will be allowed to send messages through MM API for WhatsApp.

You can use the included business portfolio ID and WABA ID to verify compliance and begin sending messages, or trigger subsequent onboarding actions as needed. This webhook is the preferred webhook to track MM API for WhatsApp onboarding and eligibility status.

```html
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;&lt;SOLUTION_PROVIDER_BUSINESS_ID&gt;&quot;,
      &quot;time&quot;: &quot;&lt;WEBHOOK_TIMESTAMP&gt;&quot;,
      &quot;changes&quot;: [
        &#123;
          &quot;field&quot;: &quot;account_update&quot;,
          &quot;value&quot;: &#123;
            &quot;event&quot;: &quot;MM_LITE_TERMS_SIGNED&quot;,
            &quot;waba_info&quot;: &#123;
              &quot;owner_business_id&quot;: &quot;&lt;BUSINESS_PORTFOLIO_ID&gt;&quot;,
              &quot;waba_id&quot;: &quot;&lt;WABA_ID&gt;&quot;
            &#125;
          &#125;
        &#125;
      ]
    &#125;
  ]
&#125;
```

## Receive onboarding completion webhook (Legacy)

Once you have completed onboarding and linked Ad accounts have been set up, an [`account_update`](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/account_update) webhook will be sent for each WABA under your business portfolio to indicate that onboarding has successfully completed. This webhook contains the ID of the read-only Ad account that each WABA is linked to, for use when calling Insights APIs.

Note: This webhook is considered legacy for MM API for WhatsApp onboarding. Please use the MM API for WhatsApp Terms of Service signed webhook.

Important: The `ad_account_linked` webhook event will no longer be fired since partners will not receive access to ad accounts.

```html
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;&lt;WABA_ID&gt;&quot;,
      &quot;time&quot;: &quot;&lt;WEBHOOK_TIMESTAMP&gt;&quot;,
      &quot;changes&quot;: [
        &#123;
          &quot;field&quot;: &quot;account_update&quot;,
          &quot;value&quot;: &#123;
            &quot;event&quot;: &quot;AD_ACCOUNT_LINKED&quot;,
            &quot;waba_info&quot;: &#123;
              &quot;waba_id&quot;: &quot;&lt;WABA_ID&gt;&quot;,
              &quot;ad_account_id&quot;: &quot;&lt;AD_ACCOUNT_ID&gt;&quot;,
              &quot;owner_business_id&quot;: &quot;&lt;BUSINESS_PORTFOLIO_ID&gt;&quot;
            &#125;
          &#125;
        &#125;
      ]
    &#125;
  ]
&#125;
```
