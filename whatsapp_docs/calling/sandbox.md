# Set Up a Sandbox Account for Calling



**Warning:** Sandbox accounts are only available to Tech Partners.

## Overview

A WhatsApp sandbox account is a mock WhatsApp Business account that you can use to test your Calling API integration. Use a calling sandbox account to test the following features:

* Initiate and receive calls using the Calling API
* Validate calling webhook events
* Simulate onboarding flows without creating real business assets

## Sandbox account calling limits

The following table outlines the calling limits for sandbox accounts. These limits are subject to change.

| Limit | Description | Production number limit | Public test number limit |
| --- | --- | --- | --- |
| Connected call limit | Number of calls a business can make on approved permissions. | 100 connected calls per 24 hrs | No change |
| Call Permission Request message limits | Limits the number of call permission request messages that can be sent to the same consumer | 1 request per day&lt;br&gt;&lt;br&gt;2 requests per week | 25 requests per day&lt;br&gt;&lt;br&gt;100 requests per week |
| Unanswered call limits | When the user rejects or misses a business-initiated call. | Nudge on 2 consecutive unanswered calls&lt;br&gt;&lt;br&gt;Revoke permission on 4 consecutive unanswered calls | Nudge on 5 consecutive unanswered calls&lt;br&gt;&lt;br&gt;Revoke on 10 consecutive unanswered calls |
| Temporary call duration | Duration a business can call the user after the user approves permission. | 7 days | No change |

## Set up a sandbox account

### Step 1. Claim a sandbox account

Follow the instructions in [Embedded Signup Overview — Claiming sandbox accounts](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/overview#claiming-sandbox-accounts) to claim your sandbox account.

### Step 2. Obtain credentials and identifiers for your sandbox account

1. In the [App Dashboard](https://developers.facebook.com/apps), navigate to your app, then select **WhatsApp** &gt; **Embedded Signup Builder** in the sidebar.
    * **Note: Keep this tab open and available as you will use it multiple times throughout this process.**
2. Ensure that the **Features** dropdown is empty, then click **Login with Facebook**.
3. A popup with the Embedded Signup experience will show. Under **Business portfolio** select **Sandbox Business**.
4. Fill in the rest of the required information, then click **Next**.
5. On the next screen, in the **Create or Select a WhatsApp Business Profile** dropdown, select **Test Number**.
6. Once the login flow is complete, in the **Exchange Token** section, click **Get Token**.
    * **Note: Retain this token for future sandbox account API use.**
7. In the **Fetch Shared WhatsApp Business account** section, click **Fetch WABA details**.
8. Under **WhatsApp Business account field**, copy the **Value** for the `id` row.
    * **Note: Retain this ID since it is the WhatsApp Business account ID for the sandbox WABA.**
9. In the **Fetch phone numbers** section, click **Fetch phone numbers**.
10. Under **ID**, copy the value.
    * **Note: Retain this ID since it is the phone number ID for your sandbox account test phone number.**

### Step 3. Register your test phone number and subscribe to your WABA

### Prerequisites

_Ensure that you have the following information from the previous steps:_
* _Your sandbox account token string_
* _Your sandbox account WABA ID_
* _Your test phone number ID_

To complete these next steps, you will use the [Graph API Explorer tool](https://developers.facebook.com/tools/explorer/).

* **Note: Keep this window open as you will use the configuration you created again in later steps in this guide.**

1. Navigate to the [Graph API Explorer tool](https://developers.facebook.com/tools/explorer/).
2. Ensure you are on the latest version of the API.
3. Click **Generate Access Token** and follow the prompts.
4. Under **Permissions**, add the `whatsapp_business_management` and `whatsapp_business_messaging` permissions.
5. In the endpoint builder, enter `/&lt;YOUR_SANDBOX_WABA_ID&gt;/subscribed_apps`, then click **Submit**.

```curl
&#123;
  &quot;success&quot;: true
&#125;
```

6. Next, register your test phone number by entering `/&lt;YOUR_SANDBOX_TEST_PHONE_NUMBER_ID&gt;/register` in the endpoint builder.
7. In the left sidebar, click JSON, then enter the following JSON body, then click Submit:

```curl
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;pin&quot;: &quot;123456&quot;
&#125;
```

8. You should receive a standard `success` response:

```curl
&#123;
  &quot;success&quot;: true
&#125;
```

### Step 4. Test your messaging functionality

1. In the [Graph API Explorer tool](https://developers.facebook.com/tools/explorer/), enter `/&lt;YOUR_SANDBOX_TEST_PHONE_NUMBER_ID&gt;/messages` in the endpoint builder.
2. In the left sidebar, click **JSON**, then enter the following JSON body, then click **Submit**:

```curl
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;to&quot;: &quot;YOUR_NUMBER&quot;, // Replace this value with phone number of your device.
  &quot;recipient&quot;: &quot;US.13491208655302741918&quot;,
  &quot;type&quot;: &quot;template&quot;,
  &quot;template&quot;: &#123;
    &quot;name&quot;: &quot;hello_world&quot;,
    &quot;language&quot;: &#123; &quot;code&quot;: &quot;en_US&quot; &#125;
  &#125;
&#125;
```

**Note:** **Usernames and business-scoped user IDs:** The `recipient` field lets you identify the WhatsApp user by their BSUID instead of, or in addition to, their phone number in `to`. For details, see [Business-scoped user IDs](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-scoped-user-ids#business-scoped-user-id).

3. You will receive a response with a `&quot;message_status&quot;: &quot;accepted&quot;` value, and you should receive a text message on your device.

### Step 5. Configure webhooks and permissions

* Navigate to the [App Dashboard](https://developers.facebook.com/apps).
* Click the app you are using with WhatsApp.
* Select **Use cases** (pencil icon) from the sidebar.
* Under **Connect with customers through WhatsApp**, click **Customize**.
* In the left sidebar, click **Configuration**.
* Under **Callback URL**, add the callback URL for your webhook server.
    * If you do not have a webhook server, you can follow our [instructions to create a test webhook server](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/set-up-whatsapp-echo-bot).
* Under **Verify token**, add an arbitrary verification string.
* Click **Verify and save**.
* On the next page, in the **Select product** dropdown, click **WhatsApp Business Account**.
* Under **Webhook fields**, in the **calls** row, click the toggle button to subscribe to the `calls` webhook field.

### Finish: Enable calling features on your test phone number

1. In the [Graph API Explorer tool](https://developers.facebook.com/tools/explorer/), enter `/&lt;YOUR_SANDBOX_TEST_PHONE_NUMBER_ID&gt;/settings` in the endpoint builder.
2. In the left sidebar, click **JSON**, then enter the following JSON body, then click **Submit**:

```curl
&#123;
  &quot;calling&quot;: &#123;
    &quot;status&quot;: &quot;ENABLED&quot;,
    &quot;call_icon_visibility&quot;: &quot;DEFAULT&quot;,
    &quot;callback_permission_status&quot;: &quot;ENABLED&quot;
  &#125;
&#125;
```

3. You should receive a standard `success` response:

```curl
&#123;
  &quot;success&quot;: true
&#125;
```

4. Finally, under **Access Token**, copy the access token down for future use.
    * **Note: Retain this access token as you will use it to make API calls for testing your Calling API integration.**

## Test business-initiated calling

Before you can test business-initiated calls (BIC), you must provide user calling permissions to your sandbox account.

You can do this on the client device you are using for testing:

1. On your client device, open WhatsApp.
2. Navigate to the message thread you have with your sandbox business phone number.
3. At the top of the screen, tap the sandbox business phone number.
4. Scroll down and tap **Business Calling Permission**.
5. Tap **Allow calls**.

You can now use your Calling API integration to call the client device and test your integration.

[Learn more about business-initiated calls.](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/business-initiated-calls)

## Test user-initiated calling

You can test user-initiated calls (UIC) on the client device you are using for testing:

1. On your client device, open WhatsApp.
2. Navigate to the message thread you have with your sandbox business phone number.
3. Tap the phone icon at the top of the screen to call the sandbox business phone number.
4. Confirm a successful call connection.

[Learn more about user-initiated calls.](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/user-initiated-calls)
