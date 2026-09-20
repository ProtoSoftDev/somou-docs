# Get started as a Solution Partner



This guide goes over the steps [Solution Partners](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/overview#solution-partners) need to take in order to offer the Cloud API to their clients. There are four main stages:

1. [Prepare and plan](#prepare-plan)
2. [Set up Assets](#set-up-assets)
3. [Sign Contracts](#sign-contracts)
4. [Build Integration](#build-integration)

After you&#039;re done, please [keep up with monthly updates](#keep-up-with-monthly-updates).

## Prepare and plan &#123;#prepare-plan&#125;

### Read documentation

Before you start, read through the [developer documentation](https://developers.facebook.com/documentation/business-messaging/whatsapp/about-the-platform#whatsapp-cloud-api) and the [Postman collection](https://www.postman.com/meta/workspace/whatsapp-business-platform/collection/13382743-84d01ff8-4253-4720-b454-af661f36acc2). This helps you understand how the Cloud API works, including how to get started and migrate numbers.

### Plan onboarding and migration &#123;#plan-onboarding-migration&#125;

**Use Embedded Signup to onboard new clients to the Cloud API.** If you haven&#039;t already, implement [Embedded Signup](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/overview). Embedded Signup is the fastest and easiest way to register clients, enabling them to start sending messages in less than five minutes.

## Set up assets &#123;#set-up-assets&#125;

To use the Cloud API, you need to have the following assets:

| Asset | Specific Instructions |
| --- | --- |
| **Business portfolio** | You can use an existing one, or [set up a new one](https://www.facebook.com/business/help/1710077379203657). Save the business portfolio ID. |
| **WhatsApp Business account** (WABA) | See [Create a WhatsApp Business account for the WhatsApp Business API](https://www.facebook.com/business/help/2087193751603668) for help. |
| [**Meta App**](https://developers.facebook.com/apps/) | If you don&#039;t have an app, you need to [create one](https://developers.facebook.com/docs/development/create-an-app) with the **Business** type. Remember to add a display name and a contact email to your app.&lt;br&gt;&lt;br&gt;As a Solution Partner, your app must go through [App Review](https://developers.facebook.com/docs/app-review) and request **Advanced access** to the following permissions:&lt;br&gt;&lt;br&gt;- [`whatsapp_business_management`](https://developers.facebook.com/docs/permissions/reference/whatsapp_business_management) — Manage phone numbers, message templates, registration, and business profiles under a WhatsApp Business account. If your app uses this permission to access WABAs not owned by your business, you must have **Advanced access**. Without it, API calls return error code `200`. To get **Advanced access**, submit your app for [App Review](https://developers.facebook.com/docs/app-review).&lt;br&gt;- [`whatsapp_business_messaging`](https://developers.facebook.com/docs/permissions/reference/whatsapp_business_messaging) — Used to send/receive messages from WhatsApp users, upload/download media under a WhatsApp Business account. To get this permission, your app must go through [App Review](https://developers.facebook.com/docs/app-review).&lt;br&gt;- [`whatsapp_business_manage_events`](https://developers.facebook.com/docs/permissions#whatsapp_business_manage_events) — Used to log events — such as purchases, add-to-cart actions, leads, and more under a WhatsApp Business account. Only request this permission if you are using the [Marketing Messages API for WhatsApp](https://developers.facebook.com/documentation/business-messaging/whatsapp/marketing-messages/overview) with [Conversions API](https://developers.facebook.com/documentation/ads-commerce/conversions-api). To get this permission, your app must go through [App Review](https://developers.facebook.com/docs/app-review).&lt;br&gt;&lt;br&gt;As a Solution Partner, you can also reuse the same Meta app across different clients and WABAs. But be aware that each app can only have one webhook endpoint and each app needs to go through App Review. |
| **System User** | See [Add system users to your business portfolio](https://www.facebook.com/business/help/503306463479099) for help.&lt;br&gt;&lt;br&gt;Currently, a Meta App with `whatsapp_business_messaging`, `whatsapp_business_management`, `whatsapp_business_manage_events`, and `business_messaging` permissions has access to up to:&lt;br&gt;&lt;br&gt;- 1 admin system user&lt;br&gt;- 1 employee system user&lt;br&gt;&lt;br&gt;Use the admin system user for your production deployment. See [About business portfolio access](https://www.facebook.com/business/help/442345745885606) for more information. |
| **Business Phone Number** | This is the phone number the business will use to send messages. Phone numbers need to be verified through SMS/voice call.&lt;br&gt;&lt;br&gt;If you wish to use your own number, [add a phone number](https://www.facebook.com/business/help/456220311516626) in WhatsApp Manager and verify it with the [Verify Code API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/verify-code-api#post-version-phone-number-id-verify-code).&lt;br&gt;&lt;br&gt;If your clients wish to use their own numbers, add and verify their numbers using your [Embedded Signup flow](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/overview).&lt;br&gt;&lt;br&gt;There is no limit to the amount of business phone numbers that can be onboarded to the Cloud API. |
| **Consumer Phone Number** | This is a phone number that is currently using the consumer WhatsApp app. This number will be receiving the messages sent by your business phone number. |

## Sign contracts &#123;#sign-contracts&#125;

### Accepting terms of service

In order to access the WhatsApp Business Messaging Cloud API you need to first accept the WhatsApp Business Platform Terms of Service on behalf of your business.

To do so, navigate to [WhatsApp Manager](https://business.facebook.com/wa/manage/) and accept the Terms of Service in the informational banner.

For any new Cloud API businesses, you will need to accept Terms of Service before you can start using Cloud API. Registration calls will fail until you accept the Terms of Service.

**Note:** You as a developer need to accept the Terms of Service. If you are a Solution Partner, you do not need your clients to accept.

## Build integration &#123;#build-integration&#125;

### Step 1: Get system user access token &#123;#get-access-token&#125;

Graph API calls use access tokens for authentication. For more information, see [Access Tokens](https://developers.facebook.com/documentation/facebook-login/guides/access-tokens). Use your system user to generate your token.

To generate a system user access token:

- Go to [**Business portfolio**](https://business.facebook.com/) &gt; **Business Settings** &gt; **Users** &gt; **System Users** to view the system user you created.

- Click on that user and select **Add Assets**. This action launches a new window.

- Under **Select Asset Type** on the left side pane, select **Apps**. Under **Select Assets**, choose the Meta app you want to use (your app must have the correct permissions). Enable **Develop App** for that app.

- Select **Save Changes** to save your settings and return to the system user main screen.

- Now you are ready to generate your token. In the system user main screen, click **Generate Token** and select your Meta app.

- After selecting the app, you will see a list of available permissions.
Select `whatsapp_business_management` , `whatsapp_business_messaging` , and `whatsapp_business_manage_events` . Click **Generate Token**.

- A new window opens with your system user, assigned app and access token. Save your token.

- Optionally, you can click on your token and see the Token Debugger. In your debugger, you should see the permissions you have selected. You can also directly paste your token into the [Access Token Debugger](https://developers.facebook.com/tools/debug/accesstoken).

### Step 2: Set up webhooks

With Webhooks set up, you can receive real-time HTTP notifications from the WhatsApp Business Platform. This means you get notified when, for example, you get a message from a customer or there are changes to your WhatsApp Business account (WABA).

To set up your webhook endpoint, you need to create an internet-facing web server with a URL that meets Meta&#039;s and WhatsApp&#039;s requirements. See our [Webhooks](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/overview) document for more information. If you need an endpoint for testing purposes, [you can deploy a test app](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/set-up-whatsapp-echo-bot) that simply dumps webhook payloads to your console.

#### App setup

Once the endpoint is ready, configure it to be used by your Meta app:

In the App Dashboard, go to **WhatsApp** &gt; **Configuration**, then click the **Edit** button.

- Callback URL: This is the URL Meta will be sending the events to. See the [Webhooks, Getting Started](https://developers.facebook.com/docs/graph-api/webhooks/getting-started) guide for information on creating the URL.
- Verify Token: This string is set up by you, when you create your webhook endpoint.

After adding the information, click **Verify and Save**.

After saving, back in the **Configuration** panel, click the **Manage** button and subscribe to individual webhook fields. To receive notifications of customer messages, be sure to subscribe to the **messages** webhook field.


You only need to set up Webhooks once for every application you have. You can use the same Webhook to receive multiple event types from multiple WhatsApp Business Accounts, or set up an override. For more information, see our Webhooks section.

### Step 3: Subscribe to your WABA &#123;#subscribe-waba&#125;

To make sure you get notifications for the correct account, subscribe your app:

```html
curl -X POST \
&#039;https://graph.facebook.com/v25.0/&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;/subscribed_apps&#039; \
-H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039;
```

If you get the response below, all Webhook events for the phone numbers under this account will be sent to your configured Webhooks endpoint.

```json
&#123;
  &quot;success&quot;: true
&#125;
```

### Step 4: Get phone number ID

To send messages, you need to register the phone number you want to use. Before you can register it, you need to get the phone number&#039;s ID. To get your phone number&#039;s ID, make the following API call:

```html
curl -X GET \
&#039;https://graph.facebook.com/v25.0/&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;/phone_numbers&#039; \
-H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039;
```

If the request is successful, the response includes all phone numbers connected to your WABA:

```json
&#123;
  &quot;data&quot;: [
    &#123;
      &quot;verified_name&quot;: &quot;Jasper&#039;s Market&quot;,
      &quot;display_phone_number&quot;: &quot;+1 631-555-5555&quot;,
      &quot;id&quot;: &quot;1906385232743451&quot;,
      &quot;quality_rating&quot;: &quot;GREEN&quot;
    &#125;,
    &#123;
      &quot;verified_name&quot;: &quot;Jasper&#039;s Ice Cream&quot;,
      &quot;display_phone_number&quot;: &quot;+1 631-555-5556&quot;,
      &quot;id&quot;: &quot;1913623884432103&quot;,
      &quot;quality_rating&quot;: &quot;NA&quot;
    &#125;
  ]
&#125;
```

Save the ID for the phone number you want to register. See [Read Phone Numbers](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-phone-numbers/phone-numbers) for more information about this endpoint.

#### Migration exception

If you are migrating a phone number from the On-Premises API to the Cloud API, there are extra steps you need to perform before registering a phone number with the Cloud API. See [Migrate From On-Premises API to Cloud API](https://developers.facebook.com/documentation/business-messaging/whatsapp/support/migrating-from-onprem-to-cloud) for the full process.

### Step 5: Register phone number

With the phone number&#039;s ID in hand, you can register it. In the registration API call, you perform two actions at the same time:

1. Register the phone.
1. [Enable two-step verification](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-phone-numbers/two-step-verification) by setting a 6-digit registration code — you must set this code on your end. Save and memorize this code as it can be requested later.

**Warning:** **Setting up two-step verification is a requirement to use the Cloud API. If you do not set it up, you will get an onboarding failure message:**

Sample request:

```html
curl -X POST \
&#039;https://graph.facebook.com/v25.0/&lt;FROM_PHONE_NUMBER_ID&gt;/register&#039; \
-H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039; \
-H &#039;Content-Type: application/json&#039; \
-d &#039;&#123;&quot;messaging_product&quot;: &quot;whatsapp&quot;,&quot;pin&quot;: &quot;&lt;6_DIGIT_PIN&gt;&quot;&#125;&#039;
```

Sample response:

```json
&#123;
  &quot;success&quot;: true
&#125;
```

#### Embedded Signup users

A phone number **must** be registered up to 14 days after going through the Embedded Signup flow. If a number is not registered during that window, the phone must go through to the Embedded Signup flow again prior to registration.


### Step 6: Receive a message from consumer app

Once participating customers send a message to your business, you get **24 hours of free messages with them** — that window of time is called the customer service window. For testing purposes, enable this window so you can send as many messages as you would like.

From a personal WhatsApp iOS/Android app, send a message to the phone number you just registered. Once the message is sent, you should receive an incoming message to your Webhook with a notification in the following format.

```json
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;102290129340398&quot;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;messaging_product&quot;: &quot;whatsapp&quot;,
            &quot;metadata&quot;: &#123;
              &quot;display_phone_number&quot;: &quot;16315551234&quot;,
              &quot;phone_number_id&quot;: &quot;PHONE_NUMBER_ID&quot;
            &#125;,
            &quot;contacts&quot;: [
              &#123;
                &quot;profile&quot;: &#123;
                  &quot;name&quot;: &quot;Kerry Fisher&quot;
                &#125;,
                &quot;wa_id&quot;: &quot;16315555555&quot;
              &#125;
            ],
            &quot;messages&quot;: [
              &#123;
                &quot;from&quot;: &quot;16315555555&quot;,
                &quot;id&quot;: &quot;wamid.ABGGFlA5FpafAgo6tHcNmNjXmuSf&quot;,
                &quot;timestamp&quot;: &quot;1602139392&quot;,
                &quot;text&quot;: &#123;
                  &quot;body&quot;: &quot;Hello!&quot;
                &#125;,
                &quot;type&quot;: &quot;text&quot;
                &#125;
            ]
          &#125;,
        &quot;field&quot;: &quot;messages&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

### Step 7: Send a test message

Once you have enabled the customer service window, you can send a test message to the consumer number you used in the previous step. To do that, make the following API call:

```html
curl -X  POST \
&#039;https://graph.facebook.com/v25.0/&lt;FROM_PHONE_NUMBER_ID&gt;/messages&#039; \
-H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039; \
-H &#039;Content-Type: application/json&#039; \
-d &#039;&#123;&quot;messaging_product&quot;: &quot;whatsapp&quot;, &quot;to&quot;: &quot;16315555555&quot;,&quot;text&quot;: &#123;&quot;body&quot; : &quot;hello world!&quot;&#125;&#125;&#039;
```

If your call is successful, your response will include a message ID. Use that ID to track the progress of your messages through Webhooks. The maximum length of the ID is 128 characters.

Sample response:

```json
&#123;
  &quot;id&quot;:&quot;wamid.gBGGFlaCGg0xcvAdgmZ9plHrf2Mh-o&quot;
&#125;
```

**Note:** With the Cloud API, there is no longer a way to explicitly check if a phone number has a WhatsApp ID. To send someone a message using the Cloud API, just send it directly to the WhatsApp user&#039;s phone number after they have [opted-in](https://developers.facebook.com/documentation/business-messaging/whatsapp/getting-opt-in). See [Sending messages](https://developers.facebook.com/documentation/business-messaging/whatsapp/messages/send-messages).

## Onboard WhatsApp Business app users

If your clients already use the [WhatsApp Business app](https://business.whatsapp.com/products/business-app), you can configure Embedded Signup to [onboard them using their existing account and phone number](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/onboarding-business-app-users). Onboarded clients can use your app to message at scale while continuing to use the WhatsApp Business app for one-to-one conversations.

## Keep up with monthly updates &#123;#keep-up-with-monthly-updates&#125;

Cloud API updates are released on the first Tuesday of every month. These updates include new features and improvements. You don&#039;t need to do any work to use any of the new features, since the Cloud API updates automatically.

## FAQs

### General FAQs

**Which company will be providing the Cloud API? **

WhatsApp develops and operates the WhatsApp Business API, which enables businesses to communicate with WhatsApp consumer users on the WhatsApp network. When using the Cloud API, Meta will host the WhatsApp Business API for you and provide an endpoint for the WhatsApp service for your incoming and outgoing WhatsApp communications.

**Are there any additional costs for the Cloud API?**

Access to Cloud API is free, and we expect it to generate additional cost savings for developers, as Meta hosts and maintains the Cloud API.

### Technical implementation FAQs

**What is the architecture of the Cloud API?**

The Cloud API architecture significantly simplifies the Solution Partner&#039;s operational and infrastructure requirements to integrate with WhatsApp Business Platform. First, it removes the infrastructure requirements to run Business API docker containers (CAPEX savings). Second, it obviates the need of operational responsibilities to manage the deployment (OPEX savings).

**What will disaster recovery look like: if a region is unavailable, how much time does it take to move messages to another region?**

We will have disaster recovery and data replication across multiple regions. The expected downtime would be within our SLA and usually in the order of less than a minute to less than five minutes.

### Data privacy and security FAQs

**Where are the servers for Cloud API? **

Cloud API processes messages on servers in [Meta data centers](https://datacenters.atmeta.com/all-locations/). If a business opts to use Cloud API Local Storage, message data is stored in data centers located in another [designated country](https://developers.facebook.com/documentation/business-messaging/whatsapp/local-storage).

**Is the Cloud API end-to-end encrypted? What is the encryption model? **

See [Cloud API Overview, Encryption](https://developers.facebook.com/documentation/business-messaging/whatsapp/about-the-platform#encryption).

**What happens to message data at rest? How long is it stored?**

Cloud API messages at rest are encrypted. Messages have a maximum retention period of 30 days in order to provide the base features and functionality of the Cloud API service; for example, retransmissions.

**Does Meta have access to encryption keys?**

In order to send and receive messages through Cloud API, Cloud API manages the encryption/decryption keys on behalf of the business. For more detail, see the [WhatsApp Encryption Overview technical whitepaper](https://www.whatsapp.com/security/WhatsApp-Security-Whitepaper.pdf).

### Regulatory compliance FAQs

**How does Cloud API comply with regional data protection laws (such as GDPR, LGPD, and PDPB)?**

Meta takes data protection and people&#039;s privacy very seriously and we comply with applicable legal, industry, and regulatory requirements governing data protection, as well as industry best practices. Cloud API customers must meet their own obligations under data protection laws, such as the General Data Protection Regulation (GDPR). Please visit our [Meta Business Messaging Compliance Center](https://www.facebook.com/business/business-messaging/compliance) to learn more.

