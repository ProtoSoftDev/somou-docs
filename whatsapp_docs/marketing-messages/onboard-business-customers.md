# Onboard business customers



The MM API for WhatsApp onboarding process is designed to be simple for you as a partner to adopt, making it quick and easy for [partners](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/overview) (including Solution Partners, Tech Providers, and Tech Partners) to onboard current customers from Cloud API onto the MM API for WhatsApp. If your business directly integrates with Cloud API without a partner, follow the instructions below to accept the Terms of Service and onboard to the MM API for WhatsApp via WhatsApp Manager.

## Before you begin

Your app must have advanced access for the following permissions:

* **`whatsapp_business_messaging`**: This permission allows the app to call the MM API for WhatsApp to send messages.
* **`whatsapp_business_management`**: This permission enables the app to manage WABAs, Phone Numbers, and Templates via [WhatsApp Business Management API](https://developers.facebook.com/documentation/business-messaging/whatsapp/analytics).
* **`ads_read`** (optional): This permission grants the app access to the [Insights API](https://developers.facebook.com/documentation/ads-commerce/marketing-api/insights), allowing partners to retrieve metrics on conversions.

If your app does not already have advanced access for these permissions, request advanced access via [App Review](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/app-review).

## Solution partner integration overview

To assist your customers in using the MM API for WhatsApp, several steps are required:

| Step | Notes |
| --- | --- |
| 1: Onboard yourself | Enroll via [App Dashboard](https://developers.facebook.com/apps) and follow instructions under the [Onboarding yourself](#onboarding-yourself). |
| 2: Send messages | Same Template endpoint and message send payload as Cloud API - only the &#039;send message&#039; endpoint changes. |
| 3: View metrics | **New!** Integrate with the Insights API to view the metrics as Cloud API (sent/read/delivered), plus new metrics like Website and App conversions. |

## Onboarding yourself

### Register yourself for MM API for WhatsApp

To enroll, a partner must:

1. Navigate to the **[App Dashboard](https://developers.facebook.com/apps)** &gt; **WhatsApp** &gt; **Quickstart** panel
1. On the **Quickstart** page, locate the &quot;Improve ROI with Marketing Messages API for WhatsApp&quot; card and click the &quot;Get started&quot; button
1. Request any missing app review permissions by clicking the &quot;Request permission&quot; button. See &quot;[Submit for app review](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/app-review)&quot; for more information
1. Click on &quot;Continue to integration guide&quot; to accept the Terms of Service

### Submit for app review for advanced app permissions

Partners must use an App with the following Advanced App permissions, when using the MM API for WhatsApp.

If you do not already have an App with the following Advanced App Permissions, it is necessary for your App to go through [App review](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/app-review):

| Advanced App Permission | Required in order to do the following on behalf of your customer |
| --- | --- |
| `whatsapp_business_messaging` | Call the MM API for WhatsApp &#039;send messages&#039; endpoint, to send messages via Marketing Messages API for WhatsApp |
| `whatsapp_business_management` | Call WABA, Phone Number, and Template endpoints, for managing WABAs, Phone Numbers, and Templates; and retrieve basic metrics via [WhatsApp Business Management API](https://developers.facebook.com/documentation/business-messaging/whatsapp/analytics) |
| `ads_read` (optional) | This permission is optional, and is only required to call the [Insights API](https://developers.facebook.com/documentation/ads-commerce/marketing-api/insights), allowing a partner to fetch advanced metrics on conversions (for example, Web conversions, App conversions) |

For the app review submission, prepare a screen recording of how each permission is used. Show a sample of each action in the &quot;Required in order to do …&quot; column above, to demonstrate each permission in use.

## Onboarding end businesses

There are multiple ways that a partner can onboard a business they work with to the MM API for WhatsApp, and fetch the required access tokens needed to send messages and fetch metrics on their behalf:

- **via WhatsApp Manager** (recommended)
- **via Embedded Signup**

### Self-onboard to MM API for WhatsApp via WhatsApp Manager (recommended)

If your business integrates with Cloud API with a partner, they can [onboard directly via WhatsApp Manager](https://developers.facebook.com/documentation/business-messaging/whatsapp/marketing-messages/onboarding).

### Onboard a business to MM API for WhatsApp using Embedded Signup

MM API for WhatsApp has its own Embedded Signup flow for partners to use, which:

- allows your clients to register for Cloud API and MM API for WhatsApp
- allows you to request an access token with the necessary permissions to send messages via MM API for WhatsApp and retrieve metrics from the Insights API on behalf of your customers.

#### Step 1: Integrate with the Facebook Login SDK

If you have not already done so for Cloud API, set up the Facebook JavaScript SDK and Facebook Login to embed the signup flow into your website or client portal. You may embed the flow in multiple web pages or portals that you own.

#### Step 2: Set up a new permission configuration

Follow the instructions in [Step 2 of the Embedded Signup docs](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/implementation#step-2--create-a-facebook-login-for-business-configuration) to create a new Facebook Login for Business configuration. Select the following in your configuration:

Assets:
- WhatsApp accounts
- (Do not select the other options of Ad accounts, Pages, Catalogues, Pixels, Instagram accounts)

Permissions:
- `ads_read` (optional)
- `whatsapp_business_management`
- `whatsapp_business_messaging`

#### Step 3: Launch Embedded Signup

Refer to the [Embedded Signup implementation](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/implementation) document and add `marketing_messages_lite` to the list of features before onboarding WhatsApp SMB users.

```json
&#123;
  &quot;config_id&quot;: &quot;&lt;config_id&gt;&quot;,
  &quot;response_type&quot;: &quot;code&quot;,
  &quot;override_default_response_type&quot;: true,
  &quot;extras&quot;: &#123;
    &quot;featureType&quot;: &quot;whatsapp_business_app_onboarding&quot;,
    &quot;sessionInfoVersion&quot;: &quot;3&quot;,
    &quot;features&quot;: [
      &#123;
        &quot;name&quot;: &quot;marketing_messages_lite&quot;
      &#125;
    ],
    &quot;version&quot;: &quot;v3&quot;
  &#125;
&#125;
```

#### Step 4: Wait for the onboarding confirmation webhook

Once a business admin completes the MM API for WhatsApp onboarding flow, and any backend onboarding processes complete successfully, an `account_update` webhook will be sent for each WABA under the BMID, to indicate that onboarding has successfully completed.

See the [Onboarding](https://developers.facebook.com/documentation/business-messaging/whatsapp/marketing-messages/onboarding) for details on the `account_update` webhook.

After a business goes through the Embedded Signup flow for MM API for WhatsApp, the returned system user app token will have permission to send messages via MM API for WhatsApp, and fetch metrics via the Insights API.

## Onboard a business to Marketing Messages API for WhatsApp using the Intent API

Partners can use the Intent API to onboard a business to Marketing Messages API for WhatsApp (MM API for WhatsApp) while automatically marking all On-behalf-of (OBO) WABAs to be transferred to your customer, and receive an async (webhook) notification when the onboarding is complete to fetch the necessary access tokens.

If an end business has any OBO WABAs that do not currently have an OBO migration intent set, the OBO migration intent will automatically be set for all OBO WABAs between the partner and end business through this API call.

1. Call the intent API

Calling the intent API to onboard a BMID to MM API for WhatsApp will trigger an email to all admins of that BMID, directing them to an onboarding flow in Meta Business Suite. Meta will contact admins of the BMID until they either onboard or reject the invitation to onboard.

Partners must use a System User Access Token for the App with advanced permissions to call the intent API for each of their eligible end businesses.

An eligible end business is one that:

- Has not yet signed the MM API for WhatsApp ToS
- Is currently a partner business as dictated by 1+ shared or OBO WABAs with the Solution Partner
- Has 1+ WABAs in an MM API for WhatsApp eligible tax country

Request:

```bash
curl -i -X POST \
&quot;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;END_BUSINESS_ID&gt;/onboard_partners_to_mm_lite?access_token=&lt;SYSTEM_USER_ACCESS_TOKEN&gt;&quot;
```

Optional Body:

If a partner wants to migrate OBO WABAs to a WhatsApp Business Solution as opposed to being shared solely with the partner, an optional solution_id can be passed into the API call.

```json
&#123;
  &quot;solution_id&quot;: &quot;&lt;MULTI-PARTNER_SOLUTION_ID&gt;&quot;
&#125;
```

Response:

```json
&#123;
  &quot;request_id&quot;: &quot;893436979557695&quot;
&#125;
```

- Error: Invalid recipient type
  - Reason: The end business is not eligible to onboard onto MM API for WhatsApp because
    - They have either accepted the ToS already
    - The end business is not in an allowlisted country
    - There are no country eligible WABAs shared with the Solution Partner
- Error: Your business has already sent this request. To follow up on the request, contact the business you&#039;re requesting access from.
  - Reason: There is already a request to this end business to onboard onto MM API for WhatsApp using this flow.
- Error: The app does not have sufficient permissions to call this API.
  - Reason: The app SUAT used to call the API must have advanced access on at least whatsapp_business_messaging and whatsapp_business_management.
- What is the expected behavior for OBO WABAs and the migration intent that gets set?
  - 1 shared, 4 OBO with intent
    - Call succeeds, creating an MM API for WhatsApp intent, does not set new OBO intent
  - 0 shared, 5 OBO with intent
    - Call succeeds, creating an MM API for WhatsApp intent, does not set new OBO intent
  - 1 shared, 4 OBO without intent
    - Call succeeds, creating an MM API for WhatsApp intent, sets new OBO intent
  - 0 shared, 5 OBO without intent
    - Call succeeds, creating an MM API for WhatsApp intent, contingent on setting OBO intent
  - 0 shared, 0 OBO
    - Call fails, no MM API for WhatsApp or OBO intent is set

2. Wait for the onboarding confirmation webhook

Once a business admin completes the MM API for WhatsApp onboarding flow, and any backend onboarding processes complete successfully, a new account_update webhook will be sent for each WABA under the BMID, to indicate that onboarding has successfully completed.

3. Fetch an updated Business Integration System User Access Token (BISUAT)

In order to call the Insights API, you will need a Business Integration System User Access Token scoped to the Ad account that is linked to the WABA you are managing.
Follow the guide in the &quot;Business Integration System User Access Token Management API&quot; section to fetch your existing token for managing ad account IDs received in the webhook.

Request syntax:

```bash
curl -i -X POST &quot;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;CLIENT_BUSINESS_ID&gt;/system_user_access_tokens
    ?appsecret_proof=&quot;&lt;APPSECRET_PROOF_HASH&gt;&quot;
    &amp;access_token=&quot;&lt;SYSTEM_USER_ACCESS_TOKEN&gt;&quot;
    &amp;fetch_only=true
```

| Object | Description | Example Value |
| --- | --- | --- |
| `&lt;API_VERSION&gt;` | Graph API version. | v23.0 |
| `&lt;APPSECRET_PROOF_HASH&gt;` | Required. The appsecret_proof is a sha256 hash of your access token ensuring API calls are from a server are more secure. [Learn more](https://developers.facebook.com/docs/graph-api/guides/secure-requests#generate-the-proof). | e3b0c... |
| `&lt;CLIENT_BUSINESS_ID&gt;` | Required. Client business ID. | 2780023991704 |
| `&lt;SYSTEM_USER_ACCESS_TOKEN&gt;` | Required. This access token requires the business_management permission. | EAAJZC... |

Response syntax

```json
&#123;
  &quot;access_token&quot;: &quot;&lt;NEW_ACCESS_TOKEN&gt;&quot;
&#125;
```

### Onboard WhatsApp Business app users

You can configure Embedded Signup to [allow business customers to onboard using their existing WhatsApp Business app account and phone number:](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/onboarding-business-app-users)

Businesses who are successfully onboarded after choosing this option will then be able to use your app to message their customers at scale, but still have the ability to send messages on a one-to-one basis using the WhatsApp Business app.

Note: To ensure accurate metrics tracking, business customers should avoid clicking links in their own marketing messages. Clicking these links may affect tracking data and could lead to skewed results.

### Offboard WhatsApp Business app users

When you offboard a WhatsApp Business app phone number, you will automatically be offboarded from MM API for WhatsApp. The ad account will be disabled and unlinked which will prevent conversion metrics tracking.

## Help the business set up Conversion measurement

See [Setting up conversion measurement](https://developers.facebook.com/documentation/business-messaging/whatsapp/marketing-messages/measure-conversion) for details on how businesses can measure when a marketing message from MM API for WhatsApp leads to a conversion (for example, add to cart, purchase).

Partners are **strongly recommended** to work with their clients to set up Conversion reporting, so that they can measure the improved metrics and optimizations MM API for WhatsApp provides.

## Sending messages

See [Sending messages](https://developers.facebook.com/documentation/business-messaging/whatsapp/marketing-messages/send-marketing-messages) for documentation on how to send messages and receive webhooks on behalf of your customers via MM API for WhatsApp.

## Viewing metrics

See the Guide to [Viewing metrics](https://developers.facebook.com/documentation/business-messaging/whatsapp/marketing-messages/view-metrics) for documentation on how to:

- Fetch the IDs of the Ad entities mapped to a business&#039; WABAs and Templates, in order to call Insights APIs.
- Fetch metrics for messages sent via MM API for WhatsApp.

Partners are **strongly recommended** to fetch metrics using the Ads Insights APIs (not Business Management APIs), as these APIs provide richer metrics reporting, including conversion reporting from sources such as Web and App conversion events.

After integrating with reporting APIs (Insights API recommended), surface these metrics in your dashboards and APIs for your customers to use.

Reach out to your Partner Manager for suggestions on metrics best practices, including a copy of Meta&#039;s &quot;**Business Messaging Reporting Dashboards Playbook**&quot; for partners.
