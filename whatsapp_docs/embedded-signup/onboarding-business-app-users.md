# Onboard WhatsApp Business app users



**Warning:** **Embedded signup v2 will be deprecated on October 15, 2026.** Migrate your integration to [v4](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/version-4) before that date to avoid disruption. See [Versions](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/versions) for the full upgrade path.

This feature is sometimes referred to as &quot;Coexistence&quot; in support channels and Partner documentation.

You can configure [Embedded Signup](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/overview) to allow business customers to onboard using their existing [WhatsApp Business app](https://business.whatsapp.com/products/business-app) account and phone number. After a business customer chooses this option and onboards successfully, they can use your app to send high volumes of messages. They can still send messages on a one-to-one basis using the WhatsApp Business app, and WhatsApp keeps messaging history between both apps in sync.

## How it works

When you configure Embedded Signup for WhatsApp Business app phone numbers, a business customer who goes through the flow will be given the option to connect their existing WhatsApp Business app account to Cloud API:

If the business customer chooses to connect their existing account and enters their WhatsApp Business app phone number, WhatsApp presents a verification code to enter.

The message instructs the business to copy the verification code and follow the steps:

Expect to receive a message from the official Facebook Business Account. Tap **Connect**:

Tap the **Connect to the Business Platform** button to continue the onboarding process.

Tap the **Confirm** button in the app to give the business the option to share their chat history with you.

Paste the verification code.

They can complete the remainder of the Embedded Signup flow. Completing the flow returns their [asset IDs](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/implementation#session-logging-message-event-listener) and [exchangeable token code](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/implementation#response-callback) to the spawning window, as normal. You can then use this information in API calls to onboard the business customer the same way you would any other business customer. You can also synchronize their contacts and messaging history (if permitted by the business) so you can populate it in your app.

## Requirements

- The business customer must use WhatsApp Business app version **2.24.17** or higher.
- You must already be a [Solution Partner](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/get-started-for-solution-partners) or [Tech Provider](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/get-started-for-tech-providers).
- You must know how to use [Cloud API](https://developers.facebook.com/documentation/business-messaging/whatsapp/about-the-platform#whatsapp-cloud-api).
- Your webhook callback must be able to successfully accept and digest webhooks.
- You must use Embedded Signup with [session logging](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/implementation#session-logging-message-event-listener).

## Limitations
- To remain compatible with the WhatsApp Business app, business phone numbers that are in use with both the WhatsApp Business app and Cloud API have a fixed throughput of 20 mps.
- If your business customer worked with a partner in the past and still shares the previous credit line, they may see an error when attempting to switch to a new partner. Follow the [guide](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/support/business-customer-support) to resolve the error.

## Pricing

After a business customer has been onboarded to Cloud API, messages sent by the business via the WhatsApp Business app will continue to be free, but messages sent via Cloud API will be subject to [Cloud API pricing](https://developers.facebook.com/documentation/business-messaging/whatsapp/pricing).

See our [API Solutions for WhatsApp Business App Users](https://developers.facebook.com/resources/API-solutions-for-WhatsApp-Business-App-users.pdf) pricing explainer PDF for breakdowns of common pricing scenarios.

## Customer service window

WhatsApp opens a [customer service window](https://developers.facebook.com/documentation/business-messaging/whatsapp/messages/send-messages#customer-service-windows) only when a WhatsApp user messages a business customer who is already onboarded onto Cloud API. If a WhatsApp user messages a business customer just before the business customer is onboarded onto Cloud API, the business customer can only respond with a template message, since WhatsApp opened no customer service window. If the WhatsApp user messages the business customer after onboarding onto Cloud API, WhatsApp opens a customer service window as normal, and the business customer can then respond with a non-template message.

The 24-hour customer service window restriction applies to messages sent via Cloud API. Messages sent from the WhatsApp Business app are not subject to the customer service window and do not create, extend, or affect Cloud API conversation windows or Cloud API pricing.

## Feature comparison

The following table describes features available to business customers who have been onboarded to Cloud API, as well as any changes to WhatsApp Business app functionality post-onboarding.

| Existing feature on the WhatsApp Business App | Changes to features on the WhatsApp Business App after onboarding to Cloud API | Is the WhatsApp Business app feature supported on Cloud API? |
| --- | --- | --- |
| Individual (1:1) chats | Message Edit/Revoke is now supported. | Supported.&lt;br&gt;&lt;br&gt;All chat messages in the most recent 6 months can be synchronized.&lt;br&gt;&lt;br&gt;Messages sent and received are mirrored between the Cloud API and WhatsApp Business app. |
| Contacts | No change. | Supported.&lt;br&gt;&lt;br&gt;All contacts with a WhatsApp number can be synchronized. |
| Group chats | No change. | Not supported.&lt;br&gt;&lt;br&gt;Group chats will not be synchronized. |
| Disappearing messages | Disappearing messages will be turned off for all individual (1:1) chats | Not supported. |
| [View once message](https://faq.whatsapp.com/1077018839582332) | View once messages will be disabled for all individual (1:1) chats | Not supported. |
| Live location message | Live location messages will be disabled for all individual (1:1) chats | Not supported. |
| Broadcast lists | Broadcast lists will be disabled.&lt;br&gt;&lt;br&gt;Business will not be able to create new Broadcast lists.&lt;br&gt;&lt;br&gt;Existing Broadcast lists will become read-only. | Not supported. |
| Voice and video calls | No change. | Not supported. |
| Business tools (for example, catalog, orders, status) | No change. | Not supported. |
| Messaging tools (for example, marketing messages, greeting message, away message, quick replies, labels) | No change. | Not supported. |
| Business profile (for example, business name, address, website) | No change. | Not supported. |
| Channels | No change. | Not supported. |

## Linked devices

Businesses can link up to four WhatsApp &quot;companion&quot; clients to their WhatsApp Business app account on other devices (described as &quot;[linked devices](https://faq.whatsapp.com/378279804439436/)&quot; in our Help Center).

All companion clients are supported, except for [WhatsApp for Windows](https://faq.whatsapp.com/1317564962315842/?cms_platform=windows-desktop) and [WhatsApp for WearOS](https://faq.whatsapp.com/564431798835071/).

Once a business customer onboards to Cloud API with an existing WhatsApp Business app account and number, all companion apps will be unlinked from the account, and the business can then re-link any supported companion apps.

WhatsApp users who use an unsupported companion client to message an onboarded business can do so, but the message will not trigger [messages](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/messages) webhooks, so the business won&#039;t be able to mirror the message in their own app.

Messages sent from an onboarded business (by any means) that are viewed in an unsupported companion device will appear with placeholder text, instructing the WhatsApp user to view the message in their primary device.

## Setting up your app

### Step 1: Subscribe to webhooks &#123;#step-1-subscribe-to-webhooks&#125;

Navigate to the [**App Dashboard**](https://developers.facebook.com/apps) &gt; **WhatsApp** &gt; **Configuration** panel and subscribe your app to the following WhatsApp Business account webhook topic fields, and make sure your app&#039;s callback code can digest payloads for each of them. These fields are in addition to any fields you are already subscribed to as a partner.

- [history](#history) — describes past messages the business customer has sent/received
- [smb_app_state_sync](#smb-app-state-sync) — describes the business customer&#039;s current and new contacts
- [smb_message_echoes](#smb-message-echoes) — describes any new messages the business customer sends with the WhatsApp Business app after having been onboarded

To verify that you have enabled the feature correctly, access your implementation of Embedded Signup. If the [WABA selection screen](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/default-flow#business-asset-selection-screen) has been replaced with a screen that gives you the option to connect your existing WhatsApp Business account, the feature is enabled:

### Step 3: Surface Embedded Signup to customers

Once you have confirmed that the feature has been enabled, surface Embedded Signup to your business customers.

When a business completes the flow and you [onboard the customer](#onboarding-business-customers), you have 24 hours to [synchronize their messaging history](#synchronizing-whatsapp-business-app-data), otherwise they must be offboarded and they must complete the flow again. For this reason:

- onboard and synchronize as soon as the business completes the flow
- inform the business that you are synchronizing their WhatsApp Business app data
- advise them to keep the WhatsApp Business app open to facilitate the synchronization process

Onboarding and synchronization can take several minutes, depending on a number of factors such as the size of the business&#039;s messaging history, their internet speed, and how quickly you can digest webhooks.

When you complete the **onboarding** process, the WhatsApp Business app will automatically refresh and indicate to the business that their number is now connected to the API:

After you finish synchronizing the business&#039;s messaging history, inform the customer that the process is complete.

## Onboarding business customers

When a business customer successfully completes the Embedded Signup flow, Embedded Signup returns their asset IDs and an exchangeable token code to the window that spawned the flow, as normal, but the [session event](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/implementation#session-logging-message-event-listener) payload sets `event` to `FINISH_WHATSAPP_BUSINESS_APP_ONBOARDING`:

```json
&#123;
  data: &#123;
    waba_id: &quot;&lt;CUSTOMER_WABA_ID&gt;&quot;
  &#125;,
  type: &quot;WA_EMBEDDED_SIGNUP&quot;,
  event: &quot;FINISH_WHATSAPP_BUSINESS_APP_ONBOARDING&quot;,
  version: 3
&#125;
```

Capture the customer&#039;s asset IDs and exchangeable token code and use them to onboard the customer as you normally would, but **skip the phone number registration step**, as the number is already registered.

- [Onboarding business customers as a partner](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/onboarding-customers-as-a-solution-partner)
- [Onboarding business customers as a Tech Provider](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/onboarding-customers-as-a-tech-provider)

Once you have completed these onboarding steps, you can begin the [messaging history synchronization](#synchronizing-whatsapp-business-app-data) process.

### Check onboarding status (optional)

To confirm that the customer&#039;s business phone number is registered for both Cloud API and WhatsApp Business app use, request the `is_on_biz_app` and `platform_type` fields on the business phone number ID:

Example request:

```json
curl &#039;https://graph.facebook.com/v25.0/106540352242922?fields=is_on_biz_app,platform_type&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039;
```

Example response:

If `is_on_biz_app` is `true` and `platform_type` is `CLOUD_API`, the business phone number is able to use Cloud API and the WhatsApp Business app:

```json
&#123;
  &quot;is_on_biz_app&quot;: true,
  &quot;platform_type&quot;: &quot;CLOUD_API&quot;,
  &quot;id&quot;: &quot;106540352242922&quot;
&#125;
```

## Synchronizing WhatsApp Business app data

After you onboard the business customer, you have 24 hours to synchronize their contacts and messaging history, otherwise they must be offboarded and complete the flow again. For this reason, begin the synchronization process as soon as you finish onboarding the business.

As a reminder, make sure that you subscribed to the business&#039;s WABA when you [onboarded the business](#onboarding-business-customers), and that you are [subscribed to the additional webhook fields](#step-1-subscribe-to-webhooks), otherwise you will miss important webhooks.

### Step 1: Initiate contacts synchronization &#123;#step-1-initiate-contacts-synchronization&#125;

Use the [SMB App Data API](https://developers.facebook.com/docs/graph-api/reference/whats-app-business-account-to-number-current-status/smb_app_data#Creating) to request the business customer&#039;s contacts information.

If the request is successful, WhatsApp sends a set of [smb_app_state_sync](#smb-app-state-sync) webhooks describing the WhatsApp contacts in the business&#039;s WhatsApp Business app. Future additions or changes to the business&#039;s [WhatsApp contacts](https://faq.whatsapp.com/1270784217226727/) will trigger a corresponding smb_app_state_sync webhook.

You can only perform this step once. If you need to perform it again, the customer must first offboard, then complete the Embedded Signup flow again.

#### Example request

```html
curl -X  POST \
&#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;BUSINESS_PHONE_NUMBER_ID&gt;/smb_app_data \
-H &#039;Authorization: &lt;ACCESS_TOKEN&gt;&#039; \
-H &#039;Content-Type: application/json&#039; \
-d &#039;
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;sync_type&quot;: &quot;smb_app_state_sync&quot;
&#125;&#039;
```

#### Example response

Upon success:

```json
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;request_id&quot; : &quot;&lt;REQUEST_ID&gt;&quot;
&#125;
```

Store the `request_id` value in case you need to contact support.

### Step 2: Initiate message history synchronization &#123;#step-2-initiate-message-history-synchronization&#125;

Use the [SMB App Data API](https://developers.facebook.com/docs/graph-api/reference/whats-app-business-account-to-number-current-status/smb_app_data#Creating) again, this time to initiate messaging history synchronization.

Upon success, zero, one, or more history webhooks will be triggered, depending on if the business chose to share their messaging history with you.

You can only perform this step once. If you need to perform it again, the customer must first offboard, then complete the Embedded Signup flow again.

#### Messaging history shared

If the business chose to share their messaging history with you, a series of history webhooks will be triggered, describing each message sent to, or received from, WhatsApp users within a set period of time.

See [history](#history) for a description of the contents of these webhooks and how they are organized.

#### Messaging history not shared

If the business chose not to share their messaging history with you, a [history](#history) webhook with error code `2593109` will be triggered instead.

#### Example request

```html
curl -X  POST \
&#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;BUSINESS_PHONE_NUMBER_ID&gt;/smb_app_data \
-H &#039;Authorization: &lt;ACCESS_TOKEN&gt;&#039; \
-H &#039;Content-Type: application/json&#039; \
-d &#039;
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;sync_type&quot;: &quot;history&quot;
&#125;&#039;
```

#### Example response

If the request is successful, the API will respond with the following JSON payload. This response only indicates successful acceptance of the request; it does not indicate whether the business shared their messaging history with you.

```json
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;request_id&quot; : &quot;&lt;REQUEST_ID&gt;&quot;
&#125;
```

Store the `request_id` value in case you need to contact support.

### Step 3: Mirror new WhatsApp Business app messages &#123;#step-3-mirror-new-whatsapp-business-app-messages&#125;

Onboarded businesses are still able to use the WhatsApp Business app and supported [companion devices](#linked-devices) to send and receive messages. Each time a business sends a message with one of these apps, it triggers an [smb_message_echoes](#smb-message-echoes) webhook, which you must digest and display in the contact message thread history in your app.

## Reporting conversion activity

Onboarded business customers may run Click to WhatsApp ads, so report purchase/lead-gen signals on behalf of the business using the Conversions API. See [Conversions API for business messaging](https://developers.facebook.com/documentation/ads-commerce/conversions-api/business-messaging).

## Offboarding business customers

You cannot use the [Deregister API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/phone-number-deregister-api#post-version-phone-number-id-deregister) to deregister a business phone number from Cloud API if it is already in use with both Cloud API and the WhatsApp Business app.

Instead, your clients can use the WhatsApp Business app to disconnect from Cloud API by navigating to the **Settings** &gt; **Account** &gt; **Business Platform** and clicking the **Disconnect Account** button. When your client disconnects from Cloud API, an [account_update](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/account_update) webhook with a `PARTNER_REMOVED` event is triggered. This webhook may include a `disconnection_info` object that indicates the reason for the disconnection and whether it was initiated by your client or the system.

## Errors

If you onboard a business customer with a WhatsApp Business app phone number, you may receive an [unsupported messages](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/messages/unsupported) webhook with error code `131060`. This is expected and can occur in the following scenarios:

- **First-time messaging**: A WhatsApp user messages your business for the first time. This is especially common when users tap one of your [ads that click to WhatsApp](https://business.whatsapp.com/products/create-ads-that-click-to-whatsapp) and immediately send a message. The error typically resolves within a few seconds, after which WhatsApp delivers messages normally.
- **Unsupported companion device**: A WhatsApp user with an unsupported [companion device](#linked-devices) sends or receives a message to or from your business.

If you receive this webhook, instruct the business to check the WhatsApp Business app for the message.

## Webhooks

### account_update

Describes changes to a WhatsApp Business account (&quot;WABA&quot;).

#### Trigger events

- the business phone number associated with the WABA changes
- the WABA&#039;s status changes

#### Payload syntax

```json
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;&lt;WABA_ID&gt;&quot;,
      &quot;time&quot;: &lt;WEBHOOK_TIMESTAMP&gt;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
              &quot;phone_number&quot;: &quot;&lt;BUSINESS_PHONE_NUMBER&gt;&quot;,
              &quot;event&quot;: &quot;&lt;EVENT&gt;&quot;,
              &quot;disconnection_info&quot;: &#123;  // only included for PARTNER_REMOVED events
                &quot;reason&quot;: &quot;&lt;DISCONNECTION_REASON&gt;&quot;,
                &quot;initiated_by&quot;: &quot;&lt;DISCONNECTION_INITIATED_BY&gt;&quot;
              &#125;
           &#125;,
          &quot;field&quot;: &quot;account_update&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

#### Example payload

```json
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;102290129340398&quot;,
      &quot;time&quot;: 1739212624,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
              &quot;phone_number&quot;: &quot;15550783881&quot;,
              &quot;event&quot;: &quot;PARTNER_REMOVED&quot;,
              &quot;disconnection_info&quot;: &#123;
                &quot;reason&quot;: &quot;PRIMARY_INACTIVITY&quot;,
                &quot;initiated_by&quot;: &quot;SYSTEM&quot;
              &#125;
           &#125;,
          &quot;field&quot;: &quot;account_update&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

The `disconnection_info` object contains:

- `reason` — Why your client was disconnected. Values: `ACCOUNT_DISCONNECTED` (your client&#039;s account was disconnected due to [enforcement](https://developers.facebook.com/documentation/business-messaging/whatsapp/policy-enforcement) or because your client explicitly deleted their WhatsApp account; can be `USER` or `SYSTEM` initiated), `BUSINESS_DOWNGRADE` (your client registered their business phone number with the consumer WhatsApp app), `CHANGE_NUMBER` (your client changed their phone number), `COMPANION_INACTIVITY` (companion device inactive for approximately 30 days), `PRIMARY_INACTIVITY` (primary device inactive for approximately 14 days), `USER_RE_REGISTERED` (your client re-registered on a new device).
- `initiated_by` — Whether the disconnection was client- or system-initiated. Values: `USER`, `SYSTEM`.

See the [account_update webhook reference](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/account_update#partner-removed-disconnection) for full details.

### `account_offboarded`

Describes changes to a WhatsApp Business account (&quot;WABA&quot;) when the business offboards due to device change or re-registration.

#### Trigger events

- The business phone number associated with the WABA changes devices and re-registers.
- The WABA&#039;s status changes due to the business offboarding their WhatsApp Business app phone number.

#### Payload syntax

```json
&#123;
 &quot;entry&quot;: [
   &#123;
     &quot;id&quot;: &quot;&lt;WABA_ID&gt;&quot;,
     &quot;time&quot;: &quot;&lt;WEBHOOK_TIMESTAMP&gt;&quot;,
     &quot;changes&quot;: [
       &#123;
         &quot;value&quot;: &#123;
           &quot;event&quot;: &quot;ACCOUNT_OFFBOARDED&quot;
         &#125;,
         &quot;field&quot;: &quot;account_update&quot;
       &#125;
     ]
   &#125;
 ],
 &quot;object&quot;: &quot;whatsapp_business_account&quot;
&#125;

```

#### Example payload

```json
&#123;
 &quot;entry&quot;: [
   &#123;
     &quot;id&quot;: &quot;862475293675413&quot;,
     &quot;time&quot;: 1768477204,
     &quot;changes&quot;: [
       &#123;
         &quot;value&quot;: &#123;
           &quot;event&quot;: &quot;ACCOUNT_OFFBOARDED&quot;
         &#125;,
         &quot;field&quot;: &quot;account_update&quot;
       &#125;
     ]
   &#125;
 ],
 &quot;object&quot;: &quot;whatsapp_business_account&quot;
&#125;

```
### `account_reconnected`

Describes when a WhatsApp Business account (&quot;WABA&quot;) reconnects (re-onboards) after previously offboarding.

#### Trigger events

- The business re-onboards to the same partner after offboarding.
- The WABA&#039;s status changes to reconnected.

#### Payload syntax

```json
&#123;
 &quot;entry&quot;: [
   &#123;
     &quot;id&quot;: &quot;&lt;WABA_ID&gt;&quot;,
     &quot;time&quot;: &quot;&lt;WEBHOOK_TIMESTAMP&gt;&quot;,
     &quot;changes&quot;: [
       &#123;
         &quot;value&quot;: &#123;
           &quot;event&quot;: &quot;ACCOUNT_RECONNECTED&quot;
         &#125;,
         &quot;field&quot;: &quot;account_update&quot;
       &#125;
     ]
   &#125;
 ],
 &quot;object&quot;: &quot;whatsapp_business_account&quot;
&#125;
```

#### Example payload

```json
&#123;
 &quot;entry&quot;: [
   &#123;
     &quot;id&quot;: &quot;862475293675413&quot;,
     &quot;time&quot;: 1768477203,
     &quot;changes&quot;: [
       &#123;
         &quot;value&quot;: &#123;
           &quot;event&quot;: &quot;ACCOUNT_RECONNECTED&quot;
         &#125;,
         &quot;field&quot;: &quot;account_update&quot;
       &#125;
     ]
   &#125;
 ],
 &quot;object&quot;: &quot;whatsapp_business_account&quot;
&#125;
```

### Edit

Describes edit events and payload contents for the WhatsApp Business account messages webhook for replies to interactive messages.

#### Trigger events

- A WhatsApp user edits a previously sent message (text, media with caption).
- A WhatsApp user edits a previously sent message within 15 minutes after being sent.

#### Payload syntax

```json
&#123;
 &quot;object&quot;: &quot;whatsapp_business_account&quot;,
 &quot;entry&quot;: [
   &#123;
     &quot;id&quot;: &quot;&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;&quot;,
     &quot;changes&quot;: [
       &#123;
         &quot;value&quot;: &#123;
           &quot;messaging_product&quot;: &quot;whatsapp&quot;,
           &quot;metadata&quot;: &#123;
             &quot;display_phone_number&quot;: &quot;&lt;BUSINESS_DISPLAY_PHONE_NUMBER&gt;&quot;,
             &quot;phone_number_id&quot;: &quot;&lt;BUSINESS_PHONE_NUMBER_ID&gt;&quot;
           &#125;,
           &quot;contacts&quot;: [
             &#123;
               &quot;profile&quot;: &#123;
                 &quot;name&quot;: &quot;&lt;WHATSAPP_USER_PROFILE_NAME&gt;&quot;
               &#125;,
               &quot;wa_id&quot;: &quot;&lt;WHATSAPP_USER_ID&gt;&quot;
             &#125;
           ],
           &quot;messages&quot;: [
             &#123;
               &quot;from&quot;: &quot;&lt;WHATSAPP_USER_PHONE_NUMBER&gt;&quot;,
               &quot;id&quot;: &quot;&lt;WHATSAPP_MESSAGE_ID&gt;&quot;,
               &quot;timestamp&quot;: &quot;&lt;WEBHOOK_TRIGGER_TIMESTAMP&gt;&quot;,
               &quot;type&quot;: &quot;edit&quot;,
               &quot;edit&quot;: &#123;
                 &quot;original_message_id&quot;: &quot;&lt;ORIGINAL_WHATSAPP_MESSAGE_ID&gt;&quot;,
                 &quot;message&quot;: &#123;
                   &quot;context&quot;: &#123;
                     &quot;id&quot;: &quot;&lt;CONTEXT_ID&gt;&quot;
                   &#125;,
                   &quot;type&quot;: &quot;image&quot;,
                   &quot;image&quot;: &#123;
                     &quot;caption&quot;: &quot;&lt;MEDIA_ASSET_CAPTION&gt;&quot;,
                     &quot;mime_type&quot;: &quot;&lt;MEDIA_ASSET_MIME_TYPE&gt;&quot;,
                     &quot;sha256&quot;: &quot;&lt;MEDIA_ASSET_SHA256_HASH&gt;&quot;,
                     &quot;id&quot;: &quot;&lt;MEDIA_ASSET_ID&gt;&quot;,
                     &quot;url&quot;: &quot;&lt;MEDIA_ASSET_URL&gt;&quot;
                   &#125;
                 &#125;
               &#125;
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

#### Parameters

| Placeholder | Description | Example value |
| --- | --- | --- |
| `&lt;BUSINESS_DISPLAY_PHONE_NUMBER&gt;` | Business display phone number. | 15550783881 |
| `&lt;BUSINESS_PHONE_NUMBER_ID&gt;` | Business phone number ID. | 106540352242922 |
| `&lt;WHATSAPP_USER_PROFILE_NAME&gt;` | WhatsApp user&#039;s profile name. | Sheena Nelson |
| `&lt;WHATSAPP_USER_ID&gt;` | WhatsApp user ID. | 16505551234 |
| `&lt;WHATSAPP_USER_PHONE_NUMBER&gt;` | WhatsApp user phone number. | 16505551234 |
| `&lt;WHATSAPP_MESSAGE_ID&gt;` | WhatsApp message ID for the edit event. | wamid.HBgLMTY1MDM4Nzk0MzkV... |
| `&lt;WEBHOOK_TRIGGER_TIMESTAMP&gt;` | Unix timestamp when the webhook was triggered. | 1739321024 |
| `&lt;ORIGINAL_WHATSAPP_MESSAGE_ID&gt;` | ID of the original message being edited. | wamid.HBgLMTQxMjU1NTA4MjkV... |
| `&lt;CONTEXT_ID&gt;` | Contextual message ID (if applicable). | M0 |
| `&lt;MEDIA_ASSET_CAPTION&gt;` | Caption for the media asset. | Updated image caption |
| `&lt;MEDIA_ASSET_MIME_TYPE&gt;` | MIME type of the media asset. | image/jpeg |
| `&lt;MEDIA_ASSET_SHA256_HASH&gt;` | SHA256 hash of the media asset. | a1b2c3d4e5f6... |
| `&lt;MEDIA_ASSET_ID&gt;` | Media asset ID. | 1234567890 |
| `&lt;MEDIA_ASSET_URL&gt;` | URL to the media asset. | https://media.example.com/... |

#### Example

This example webhook describes an edit made by a user in a message.

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
             &quot;display_phone_number&quot;: &quot;15550783881&quot;,
             &quot;phone_number_id&quot;: &quot;106540352242922&quot;
           &#125;,
           &quot;contacts&quot;: [
             &#123;
               &quot;profile&quot;: &#123;
                 &quot;name&quot;: &quot;Sheena Nelson&quot;
               &#125;,
               &quot;wa_id&quot;: &quot;16505551234&quot;
             &#125;
           ],
           &quot;messages&quot;: [
             &#123;
               &quot;from&quot;: &quot;16505551234&quot;,
               &quot;id&quot;: &quot;wamid.HBgLMTY1MDM4Nzk0MzkVAgASGBQzQUFERjg0NDEzNDdFODU3MUMxMAA=&quot;,
               &quot;timestamp&quot;: &quot;1749854575&quot;,
               &quot;type&quot;: &quot;edit&quot;,
               &quot;edit&quot;: &#123;
                 &quot;original_message_id&quot;: &quot;wamid.HBgLMTQxMjU1NTA4MjkVAgASGBQzQUNCNjk5RDUwNUZGMUZEM0VBRAA=&quot;,
                 &quot;message&quot;: &#123;
                   &quot;context&quot;: &#123;
                     &quot;id&quot;: &quot;M0&quot;
                   &#125;,
                   &quot;type&quot;: &quot;image&quot;,
                   &quot;image&quot;: &#123;
                     &quot;caption&quot;: &quot;Updated image caption&quot;,
                     &quot;mime_type&quot;: &quot;image/jpeg&quot;,
                     &quot;sha256&quot;: &quot;a1b2c3d4e5f6...&quot;,
                     &quot;id&quot;: &quot;1234567890&quot;,
                     &quot;url&quot;: &quot;https://media.example.com/updated-image.jpg&quot;
                   &#125;
                 &#125;
               &#125;
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

### Revoke

This reference describes revoke events and payload contents for the WhatsApp Business account messages webhook for replies to interactive messages.

#### Trigger events

- A WhatsApp user revokes (deletes) a previously sent message.
- A WhatsApp user revokes a previously sent message within two days after being sent.

#### Syntax

```json
&#123;
 &quot;object&quot;: &quot;whatsapp_business_account&quot;,
 &quot;entry&quot;: [
   &#123;
     &quot;id&quot;: &quot;&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;&quot;,
     &quot;changes&quot;: [
       &#123;
         &quot;value&quot;: &#123;
           &quot;messaging_product&quot;: &quot;whatsapp&quot;,
           &quot;metadata&quot;: &#123;
             &quot;display_phone_number&quot;: &quot;&lt;BUSINESS_DISPLAY_PHONE_NUMBER&gt;&quot;,
             &quot;phone_number_id&quot;: &quot;&lt;BUSINESS_PHONE_NUMBER_ID&gt;&quot;
           &#125;,
           &quot;contacts&quot;: [
             &#123;
               &quot;profile&quot;: &#123;
                 &quot;name&quot;: &quot;&lt;WHATSAPP_USER_PROFILE_NAME&gt;&quot;
               &#125;,
               &quot;wa_id&quot;: &quot;&lt;WHATSAPP_USER_ID&gt;&quot;
             &#125;
           ],
           &quot;messages&quot;: [
             &#123;
               &quot;from&quot;: &quot;&lt;WHATSAPP_USER_PHONE_NUMBER&gt;&quot;,
               &quot;id&quot;: &quot;&lt;WHATSAPP_MESSAGE_ID&gt;&quot;,
               &quot;timestamp&quot;: &quot;&lt;WEBHOOK_TRIGGER_TIMESTAMP&gt;&quot;,
               &quot;type&quot;: &quot;revoke&quot;,
               &quot;revoke&quot;: &#123;
                 &quot;original_message_id&quot;: &quot;&lt;ORIGINAL_WHATSAPP_MESSAGE_ID&gt;&quot;
               &#125;
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

#### Parameters

| Placeholder | Description | Example value |
| --- | --- | --- |
| `&lt;BUSINESS_DISPLAY_PHONE_NUMBER&gt;` | Business display phone number. | 15550783881 |
| `&lt;BUSINESS_PHONE_NUMBER_ID&gt;` | Business phone number ID. | 106540352242922 |
| `&lt;WHATSAPP_USER_PROFILE_NAME&gt;` | WhatsApp user&#039;s profile name. | Sheena Nelson |
| `&lt;WHATSAPP_USER_ID&gt;` | WhatsApp user ID. | 16505551234 |
| `&lt;WHATSAPP_USER_PHONE_NUMBER&gt;` | WhatsApp user phone number. | 16505551234 |
| `&lt;WHATSAPP_MESSAGE_ID&gt;` | WhatsApp message ID for the revoke event. | wamid.HBgLMTY1MDM4Nzk0MzkV... |
| `&lt;WEBHOOK_TRIGGER_TIMESTAMP&gt;` | Unix timestamp when the webhook was triggered. | 1739321024 |
| `&lt;ORIGINAL_WHATSAPP_MESSAGE_ID&gt;` | ID of the original message being revoked (deleted). | wamid.HBgLMTQxMjU1NTA4MjkV... |

#### Example

This example webhook describes a delete made by a user in a message.

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
             &quot;display_phone_number&quot;: &quot;15550783881&quot;,
             &quot;phone_number_id&quot;: &quot;106540352242922&quot;
           &#125;,
           &quot;contacts&quot;: [
             &#123;
               &quot;profile&quot;: &#123;
                 &quot;name&quot;: &quot;Sheena Nelson&quot;
               &#125;,
               &quot;wa_id&quot;: &quot;16505551234&quot;
             &#125;
           ],
           &quot;messages&quot;: [
             &#123;
               &quot;from&quot;: &quot;16505551234&quot;,
               &quot;id&quot;: &quot;wamid.HBgLMTY1MDM4Nzk0MzkVAgASGBQzQUFERjg0NDEzNDdFODU3MUMxMAA=&quot;,
               &quot;timestamp&quot;: &quot;1749854575&quot;,
               &quot;type&quot;: &quot;revoke&quot;,
               &quot;revoke&quot;: &#123;
                 &quot;original_message_id&quot;: &quot;wamid.HBgLMTQxMjU1NTA4MjkVAgASGBQzQUNCNjk5RDUwNUZGMUZEM0VBRAA=&quot;
               &#125;
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

### History

Describes the WhatsApp Business app chat history of a business that has chosen to share their chat history with a [partner](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/overview), or the business&#039;s decision to decline chat history sharing.

#### Trigger events

- a partner [synchronizes the WhatsApp Business app chat history](#step-2-initiate-message-history-synchronization) of a business customer who they have onboarded with a WhatsApp Business app phone number, and who has agreed to share their chat history
- a partner [synchronizes the WhatsApp Business app chat history](#step-2-initiate-message-history-synchronization) of a business customer who they have onboarded with a WhatsApp Business app phone number, but the customer has declined to share their chat history

#### Chat history contents

If the business has already approved chat history sharing when the partner requests the business&#039;s chat history, a series of history webhooks will be triggered, describing all messages sent or received within 180 days of the time when the business was onboarded onto Cloud API.

- messages that are part of a group chat will not be included
- media messages will not include media asset IDs; instead, additional history webhooks containing media message asset IDs will be sent separately, but only for media messages sent within 14 days of onboarding

For efficiency purposes, a single webhook could potentially describe thousands of messages, so capture its contents first, then process the contents asynchronously.

#### Phases and chunks &#123;#phases-and-chunks&#125;

Webhooks are divided into three history phases, where day 0 indicates the time when the business was onboarded onto Cloud API:

- phase 0: day 0 through day 1
- phase 1: day 1 through day 90
- phase 2: day 90 through day 180

For each phase, chat history webhooks may be sent in separate chunks, depending on the total number of messages that comprise the thread.

- you can use the `chunk_order` parameter value to arrange these chunks in their sequential order, as they may not be delivered sequentially
- you can use the `phase` parameter value to monitor phase progress. A value of `2` indicates that the current phase is complete.
- you can use the `progress` parameter value to monitor the overall progress. A value of `100` indicates that synchronization is complete.

If there is no chat history available for a given phase, no corresponding webhooks will be sent.

#### Payload syntax — chat history sharing approved

```json
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;&lt;WABA_ID&gt;&quot;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;messaging_product&quot;: &quot;whatsapp&quot;,
            &quot;metadata&quot;: &#123;
              &quot;display_phone_number&quot;: &quot;&lt;BUSINESS_PHONE_NUMBER&gt;&quot;,
              &quot;phone_number_id&quot;: &quot;&lt;BUSINESS_PHONE_NUMBER_ID&gt;&quot;
            &#125;,
            &quot;history&quot;: [
              &#123;
                &quot;metadata&quot;: &#123;
                  &quot;phase&quot;: &lt;PHASE&gt;,
                  &quot;chunk_order&quot;: &lt;CHUNK_ORDER&gt;,
                  &quot;progress&quot;: &lt;PROGRESS&gt;
                &#125;,
                &quot;threads&quot;: [
                  /* First chat history thread object */
                  &#123;
                    &quot;id&quot;: &quot;&lt;WHATSAPP_USER_PHONE_NUMBER&gt;&quot;,
                    &quot;messages&quot;: [
                      /* First message object in thread */
                      &#123;
                        &quot;from&quot;: &quot;&lt;BUSINESS_OR_WHATSAPP_USER_PHONE_NUMBER&gt;&quot;,
                        &quot;to&quot;: &quot;&lt;WHATSAPP_USER_PHONE_NUMBER&gt;&quot;, // only included if SMB message echo
                        &quot;id&quot;: &quot;&lt;WHATSAPP_MESSAGE_ID&gt;&quot;,
                        &quot;timestamp&quot;: &quot;&lt;DEVICE_TIMESTAMP&gt;,
                        &quot;type&quot;: &quot;&lt;MESSAGE_TYPE&gt;&quot;,
                        &quot;&lt;MESSAGE_TYPE&gt;&quot;: &#123;
                          &lt;MESSAGE_CONTENTS&gt;
                        &#125;,
                        &quot;history_context&quot;: &#123;
                          &quot;status&quot;: &quot;&lt;MESSAGE_STATUS&gt;&quot;
                        &#125;
                      &#125;,
                      /* Additional message objects in thread would follow, if any */
                    ]
                  &#125;,
                  /* Additional chat history thread objects would follow, if any */
                ]
              &#125;
            ]
          &#125;,
          &quot;field&quot;: &quot;history&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

#### Payload contents — chat history sharing approved

| Placeholder | Description | Example value |
| --- | --- | --- |
| `&lt;WABA_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | The business customer&#039;s WhatsApp Business account ID. | `102290129340398` |
| `&lt;BUSINESS_PHONE_NUMBER&gt;`&lt;br&gt;&lt;br&gt;_String_ | The business customer&#039;s business phone number. | `15550783881` |
| `&lt;BUSINESS_PHONE_NUMBER_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | The business customer&#039;s business phone number ID. | `106540352242922` |
| `&lt;PHASE&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | Indicates history [phase](#phases-and-chunks). Values can be:&lt;br&gt;&lt;br&gt;- `0` — indicates messages are from day 0 (business onboarding time) through day 1&lt;br&gt;- `1` — indicates messages are from day 1 through day 90&lt;br&gt;- `2` — indicates messages are from day 90 through day 180 | `1` |
| `&lt;CHUNK_ORDER&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | Indicates [chunk](#phases-and-chunks) number, which you can use to order sets of webhooks sequentially. | `1` |
| `&lt;PROGRESS&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | Indicates percentage total of synchronization progress.&lt;br&gt;&lt;br&gt;Minimum `0`, maximum `100`. | `55` |
| `&lt;WHATSAPP_USER_PHONE_NUMBER&gt;`&lt;br&gt;&lt;br&gt;_String_ | The WhatsApp user&#039;s phone number. | `16505551234` |
| `&lt;BUSINESS_OR_WHATSAPP_USER_PHONE_NUMBER&gt;`&lt;br&gt;&lt;br&gt;_String_ | The business customer&#039;s phone number, or the WhatsApp user&#039;s phone number.&lt;br&gt;&lt;br&gt;If the value is the business&#039;s phone number, the message object describes a message sent by the business to a WhatsApp user.&lt;br&gt;&lt;br&gt;If the value is the WhatsApp user&#039;s phone number, the message object describes a message sent by the WhatsApp user to the business. | `15550783881` |
| `&lt;WHATSAPP_USER_PHONE_NUMBER&gt;`&lt;br&gt;&lt;br&gt;_String_ | The WhatsApp user&#039;s phone number.&lt;br&gt;&lt;br&gt;The `to` property is only included if the message object represents an [SMB message echo](#step-3-mirror-new-whatsapp-business-app-messages). | `16505551234` |
| `&lt;WHATSAPP_MESSAGE_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | WhatsApp message ID. | `wamid.HBgLMTY0NjcwNDM1OTUVAgARGBIyNDlBOEI5QUQ4NDc0N0FCNjMA` |
| `&lt;DEVICE_TIMESTAMP&gt;`&lt;br&gt;&lt;br&gt;_String_ | Unix timestamp indicating when the message was received by the recipient&#039;s device. | `1738796547` |
| `&lt;MESSAGE_TYPE&gt;`&lt;br&gt;&lt;br&gt;_String_ | [Message type](https://developers.facebook.com/documentation/business-messaging/whatsapp/messages/send-messages#message-types). This placeholder appears twice in the syntax above, as it serves as a placeholder for the `type` property&#039;s value and its matching property name. See the [example payload below](#example-history-approved) for a thread with various message types.&lt;br&gt;&lt;br&gt;If this value is set to `media_placeholder`, the message object describes a message that contained a media asset. In this case, the message contents will be omitted. Instead, a separate history webhook will follow, describing the content of the message and the media asset ID, but only if the message was sent within the last two weeks of your query. See the [example payload below](#example-media-asset) describing a media message&#039;s contents. | `text` |
| `&lt;MESSAGE_CONTENTS&gt;`&lt;br&gt;&lt;br&gt;_Object_ | An object describing the message&#039;s contents. This value will vary based on the message type, as well as the contents of the message.&lt;br&gt;&lt;br&gt;For example, if a business sends an `image` message without a caption, the object would not include the `caption` property.&lt;br&gt;&lt;br&gt;See [Sending messages](https://developers.facebook.com/documentation/business-messaging/whatsapp/messages/send-messages) for examples of payloads for each message type. | `&#123;&quot;body&quot;:&quot;Here&#039;s the info you requested! https://www.meta.com/quest/quest-3/&quot;&#125;` |
| `&lt;MESSAGE_STATUS&gt;`&lt;br&gt;&lt;br&gt;_String_ | Indicates the message&#039;s most recent delivery stats. Values can be:&lt;br&gt;&lt;br&gt;- `DELIVERED`&lt;br&gt;- `ERROR`&lt;br&gt;- `PENDING`&lt;br&gt;- `PLAYED`&lt;br&gt;- `READ`&lt;br&gt;- `SENT` | `READ` |

#### Example payload — chat history sharing approved &#123;#example-history-approved&#125;

Example payload for two message threads: (1) a thread containing a text message and video message sent to a WhatsApp user, and the WhatsApp user&#039;s response, and (2) a text message sent to a WhatsApp user thanking them for their order.

The media message&#039;s contents in the first thread are not described. Instead, a second webhook is triggered, describing the media message&#039;s contents.

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
              &quot;display_phone_number&quot;: &quot;15550783881&quot;,
              &quot;phone_number_id&quot;: &quot;106540352242922&quot;
            &#125;,
            &quot;history&quot;: [
              &#123;
                &quot;metadata&quot;: &#123;
                  &quot;phase&quot;: 0,
                  &quot;chunk_order&quot;: 1,
                  &quot;progress&quot;: 55
                &#125;,
                &quot;threads&quot;: [
                  &#123;
                    &quot;id&quot;: &quot;16505551234&quot;,
                    &quot;messages&quot;: [
                      &#123;
                        &quot;from&quot;: &quot;15550783881&quot;,
                        &quot;id&quot;: &quot;wamid.HBgLMTY0NjcwNDM1OTUVAgARGBIyNDlBOEI5QUQ4NDc0N0FCNjMA&quot;,
                        &quot;timestamp&quot;: &quot;1739230955&quot;,
                        &quot;type&quot;: &quot;text&quot;,
                        &quot;text&quot;: &#123;
                          &quot;body&quot;: &quot;Here&#039;s the info you requested! https://www.meta.com/quest/quest-3/&quot;
                        &#125;,
                        &quot;history_context&quot;: &#123;
                          &quot;status&quot;: &quot;READ&quot;
                        &#125;
                      &#125;,
                      &#123;
                        &quot;from&quot;: &quot;15550783881&quot;,
                        &quot;id&quot;: &quot;wamid.QyNUEHBgLMTY0NjcwNDM1OTUVAgARGBI1Rj3NEYxMzAzMzQ5MkEA&quot;,
                        &quot;timestamp&quot;: &quot;1739230970&quot;,
                        &quot;type&quot;: &quot;media_placeholder&quot;,
                        &quot;history_context&quot;: &#123;
                          &quot;status&quot;: &quot;PLAYED&quot;
                        &#125;
                      &#125;,
                      &#123;
                        &quot;from&quot;: &quot;16505551234&quot;,
                        &quot;id&quot;: &quot;wamid.N0FCNjMAHBgLMTY0NjcwNDM1OTUVAgARGBIyNDlBOEI5QUQ4NDc0&quot;,
                        &quot;timestamp&quot;: &quot;1739230970&quot;,
                        &quot;type&quot;: &quot;text&quot;,
                        &quot;text&quot;: &#123;
                          &quot;body&quot;: &quot;Thanks!&quot;
                        &#125;,
                        &quot;history_context&quot;: &#123;
                          &quot;status&quot;: &quot;READ&quot;
                        &#125;
                      &#125;
                    ]
                  &#125;,
                  &#123;
                    &quot;id&quot;: &quot;12125557890&quot;,
                    &quot;messages&quot;: [
                      &#123;
                        &quot;from&quot;: &quot;15550783881&quot;,
                        &quot;id&quot;: &quot;wamid.BIyNDlBOEI5N0FCNjMAHBgLMTY0NjcwNDM1OTUVAgARGQUQ4NDc0&quot;,
                        &quot;timestamp&quot;: &quot;1739230970&quot;,
                        &quot;type&quot;: &quot;text&quot;,
                        &quot;text&quot;: &#123;
                          &quot;body&quot;: &quot;Thanks for your order! As a thank you, use code THANKS30 to get 30% of your next order.&quot;
                        &#125;,
                        &quot;history_context&quot;: &#123;
                          &quot;status&quot;: &quot;DELIVERED&quot;
                        &#125;
                      &#125;
                    ]
                  &#125;
                ]
              &#125;
            ]
          &#125;,
          &quot;field&quot;: &quot;history&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

#### Example payload for media message asset &#123;#example-media-asset&#125;

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
              &quot;display_phone_number&quot;: &quot;15550783881&quot;,
              &quot;phone_number_id&quot;: &quot;106540352242922&quot;
            &#125;,
            &quot;messages&quot;: [
              &#123;
                &quot;from&quot;: &quot;16505551234&quot;,
                &quot;id&quot;: &quot;wamid.QyNUEHBgLMTY0NjcwNDM1OTUVAgARGBI1Rj3NEYxMzAzMzQ5MkEA&quot;,
                &quot;timestamp&quot;: &quot;1738796547&quot;,
                &quot;type&quot;: &quot;image&quot;,
                &quot;image&quot;: &#123;
                  &quot;caption&quot;: &quot;Black Prince echeveria&quot;,
                  &quot;mime_type&quot;: &quot;image/jpeg&quot;,
                  &quot;sha256&quot;: &quot;3f9d94d399fa61c191bc1d4ca71375a035cd9b9f5b1128e1f0963a415c16b0cc&quot;,
                  &quot;id&quot;: &quot;24230790383178626&quot;
                &#125;
              &#125;
            ]
          &#125;,
          &quot;field&quot;: &quot;history&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

#### Payload syntax — chat history sharing declined

```json
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;metadata&quot;: &#123;
    &quot;display_phone_number&quot;: &quot;&lt;BUSINESS_PHONE_NUMBER&gt;&quot;,
    &quot;phone_number_id&quot;: &quot;&lt;BUSINESS_PHONE_NUMBER_ID&gt;&quot;
  &#125;,
  &quot;history&quot;: [
    &#123;
      &quot;errors&quot;: [
        &#123;
          &quot;code&quot;: 2593109,
          &quot;title&quot;: &quot;History sync is turned off by the business from the WhatsApp Business App&quot;,
          &quot;message&quot;: &quot;History sync is turned off by the business from the WhatsApp Business App&quot;,
          &quot;error_data&quot;: &#123;
            &quot;details&quot;: &quot;History sharing is turned off by the business&quot;
          &#125;
        &#125;
      ]
    &#125;
  ]
&#125;
```

#### Example payload — chat history sharing declined

```json
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;metadata&quot;: &#123;
    &quot;display_phone_number&quot;: &quot;15550783881&quot;,
    &quot;phone_number_id&quot;: &quot;106540352242922&quot;
  &#125;,
  &quot;history&quot;: [
    &#123;
      &quot;errors&quot;: [
        &#123;
          &quot;code&quot;: 2593109,
          &quot;title&quot;: &quot;History sync is turned off by the business from the WhatsApp Business App&quot;,
          &quot;message&quot;: &quot;History sync is turned off by the business from the WhatsApp Business App&quot;,
          &quot;error_data&quot;: &#123;
            &quot;details&quot;: &quot;History sharing is turned off by the business&quot;
          &#125;
        &#125;
      ]
    &#125;
  ]
&#125;
```

### smb_app_state_sync &#123;#smb-app-state-sync&#125;

Describes one or more [WhatsApp contacts](https://faq.whatsapp.com/1270784217226727/) in a business customer&#039;s WhatsApp Business app.

#### Trigger events:

- a partner [synchronizes the WhatsApp contacts](#step-1-initiate-contacts-synchronization) of a business customer who they have onboarded with a WhatsApp Business app phone number
- a business customer, onboarded by a partner, with a WhatsApp Business app phone number adds, edits, or removes a [WhatsApp contacts](https://faq.whatsapp.com/1270784217226727/)

#### Payload syntax

```json
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;&lt;WABA_ID&gt;&quot;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;messaging_product&quot;: &quot;whatsapp&quot;,
            &quot;metadata&quot;: &#123;
              &quot;display_phone_number&quot;: &quot;&lt;BUSINESS_PHONE_NUMBER&gt;&quot;,
              &quot;phone_number_id&quot;: &quot;&lt;BUSINESS_PHONE_NUMBER_ID&gt;&quot;
            &#125;,
            &quot;state_sync&quot;: [
              &#123;
                &quot;type&quot;: &quot;contact&quot;,
                &quot;contact&quot;: &#123;
                  &quot;full_name&quot;: &quot;&lt;CONTACT_FULL_NAME&gt;&quot;,
                  &quot;first_name&quot;: &quot;&lt;CONTACT_FIRST_NAME&gt;&quot;,
                  &quot;phone_number&quot;: &quot;&lt;CONTACT_PHONE_NUMBER&gt;&quot;
                &#125;,
                &quot;action&quot;: &quot;&lt;ACTION&gt;&quot;,
                &quot;metadata&quot;: &#123;
                  &quot;timestamp&quot;: &quot;&lt;WEBHOOK_TIMESTAMP&gt;&quot;
                &#125;
              &#125;,
              * Additional contacts would follow, if any */
            ]
          &#125;,
          &quot;field&quot;: &quot;smb_app_state_sync&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

#### Payload contents

| Placeholder | Description | Example value |
| --- | --- | --- |
| `&lt;WABA_ID&gt;`&lt;br&gt;_String_ | The business customer&#039;s WhatsApp Business account ID. | `102290129340398` |
| `&lt;BUSINESS_PHONE_NUMBER&gt;`&lt;br&gt;_String_ | The business customer&#039;s business phone number. | `15550783881` |
| `&lt;BUSINESS_PHONE_NUMBER_ID&gt;`&lt;br&gt;_String_ | The business customer&#039;s business phone number ID. | `106540352242922` |
| `&lt;CONTACT_FULL_NAME&gt;`&lt;br&gt;_String_ | The contact&#039;s full name, as it appears in the business customer&#039;s WhatsApp Business app phone address book.&lt;br&gt;&lt;br&gt;Not included when the business customer removes a contact from their WhatsApp Business app phone address book. | `Pablo Morales` |
| `&lt;CONTACT_FIRST_NAME&gt;`&lt;br&gt;_String_ | The contact&#039;s first name, as it appears in the business customer&#039;s WhatsApp Business app phone address book.&lt;br&gt;&lt;br&gt;Not included when the business customer removes a contact from their WhatsApp Business app phone address book. | `Pablo` |
| `&lt;CONTACT_PHONE_NUMBER&gt;`&lt;br&gt;_String_ | The contact&#039;s WhatsApp phone number. | `16505551234` |
| `&lt;ACTION&gt;`&lt;br&gt;_String_ | Indicates if the business customer added, edited, or deleted a contact from their WhatsApp Business app phone address book. Values can be:&lt;br&gt;&lt;br&gt;- `add` — the business added or edited a contact&lt;br&gt;- `remove` — the business removed a contact | `add` |
| `&lt;CONTACT_CHANGE_TIMESTAMP&gt;`&lt;br&gt;_String_ | Unix timestamp indicating when the contact was added, edited, or removed. | `1738346006` |

### smb_message_echoes &#123;#smb-message-echoes&#125;

Describes a message sent by a business customer to a WhatsApp user with the WhatsApp Business app or supported [companion device](#linked-devices).

#### Trigger events

- A business customer uses the WhatsApp Business app or supported companion device to message a WhatsApp user.

#### Payload syntax

```json
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;&lt;WABA_ID&gt;&quot;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;messaging_product&quot;: &quot;whatsapp&quot;,
            &quot;metadata&quot;: &#123;
              &quot;display_phone_number&quot;: &quot;&lt;BUSINESS_PHONE_NUMBER&gt;&quot;,
              &quot;phone_number_id&quot;: &quot;&lt;BUSINESS_PHONE_NUMBER_ID&gt;&quot;
            &#125;,
            &quot;message_echoes&quot;: [
              &#123;
                &quot;from&quot;: &quot;&lt;BUSINESS_PHONE_NUMBER&gt;&quot;,
                &quot;to&quot;: &quot;&lt;WHATSAPP_USER_PHONE_NUMBER&gt;&quot;,
                &quot;id&quot;: &quot;&lt;WHATSAPP_MESSAGE_ID&gt;&quot;,
                &quot;timestamp&quot;: &quot;&lt;WEBHOOK_TIMESTAMP&gt;&quot;,
                &quot;type&quot;: &quot;&lt;MESSAGE_TYPE&gt;&quot;,
                &quot;&lt;MESSAGE_TYPE&gt;&quot;: &#123;
                  &lt;MESSAGE_CONTENTS&gt;
                &#125;
              &#125;
            ]
          &#125;,
          &quot;field&quot;: &quot;smb_message_echoes&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

#### Payload contents

| Placeholder | Description | Example value |
| --- | --- | --- |
| `&lt;WABA_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | The business customer&#039;s WhatsApp Business account ID. | `102290129340398` |
| `&lt;BUSINESS_PHONE_NUMBER&gt;`&lt;br&gt;&lt;br&gt;_String_ | The business customer&#039;s business phone number. | `15550783881` |
| `&lt;BUSINESS_PHONE_NUMBER_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | The business customer&#039;s business phone number ID. | `106540352242922` |
| `&lt;WHATSAPP_USER_PHONE_NUMBER&gt;`&lt;br&gt;&lt;br&gt;_String_ | The WhatsApp user&#039;s phone number. | `16505551234` |
| `&lt;WHATSAPP_MESSAGE_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | WhatsApp message ID. | `wamid.HBgLMTY0NjcwNDM1OTUVAgARGBIyNDlBOEI5QUQ4NDc0N0FCNjMA` |
| `&lt;WEBHOOK_TIMESTAMP&gt;`&lt;br&gt;&lt;br&gt;_String_ | Unix timestamp indicating when the webhook was triggered. | `1738796547` |
| `&lt;MESSAGE_TYPE&gt;`&lt;br&gt;&lt;br&gt;_String_ | [Message type](https://developers.facebook.com/documentation/business-messaging/whatsapp/messages/send-messages#message-types). This placeholder appears twice in the syntax above, as it serves as a placeholder for the `type` property&#039;s value and its matching property name. | `text` |
| `&lt;MESSAGE_CONTENTS&gt;`&lt;br&gt;&lt;br&gt;_Object_ | An object describing the message&#039;s contents.&lt;br&gt;&lt;br&gt;This value will vary based on the message `type`, as well as the contents of the message.&lt;br&gt;&lt;br&gt;For example, if a business sends an `image` message without a caption, the object would not include the `caption` property.&lt;br&gt;&lt;br&gt;See [Sending messages](https://developers.facebook.com/documentation/business-messaging/whatsapp/messages/send-messages) for examples of payloads for each message type. | `&#123;&quot;body&quot;:&quot;Here&#039;s the info you requested! https://www.meta.com/quest/quest-3/&quot;&#125;` |

#### Example payload

This example payload describes a text message (`type` is `text`) sent to a WhatsApp user by a business customer with the WhatsApp Business app.

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
              &quot;display_phone_number&quot;: &quot;15550783881&quot;,
              &quot;phone_number_id&quot;: &quot;106540352242922&quot;
            &#125;,
            &quot;message_echoes&quot;: [
              &#123;
                &quot;from&quot;: &quot;15550783881&quot;,
                &quot;to&quot;: &quot;16505551234&quot;,
                &quot;id&quot;: &quot;wamid.HBgLMTY0NjcwNDM1OTUVAgARGBIyNDlBOEI5QUQ4NDc0N0FCNjMA&quot;,
                &quot;timestamp&quot;: &quot;1700255121&quot;,
                &quot;type&quot;: &quot;text&quot;,
                &quot;text&quot;: &#123;
                  &quot;body&quot;: &quot;Here&#039;s the info you requested! https://www.meta.com/quest/quest-3/&quot;
                &#125;
              &#125;
            ]
          &#125;,
          &quot;field&quot;: &quot;smb_message_echoes&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

## Need support?

For Coexistence *onboarding*, choose:

- Question Topic: &quot;WABiz: Onboarding&quot; and &quot;TechProvider: Onboarding&quot;
- Request Type: &quot;Embedded Signup - Coexistence Onboarding&quot;

For Coexistence *API issues*, choose:

- Question Topic: &quot;WABiz: Cloud API&quot;
- Request Type: &quot;Coexistence Data Synchronization APIs and Webhooks&quot;
