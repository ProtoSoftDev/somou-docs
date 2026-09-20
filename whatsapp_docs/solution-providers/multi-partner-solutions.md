# Multi-Partner Solutions



This document explains how to set up Multi-Partner Solutions (&quot;solutions&quot;) and how to use them with [Embedded Signup](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/overview).

Multi-Partner Solutions allow Solution Partners and Tech Providers to jointly manage client WhatsApp assets in order to provide WhatsApp messaging services to their clients. For example, if you are a Tech Provider and are unable to offer custom or full WhatsApp messaging services to your clients, you can work with a Solution Partner to offer your clients the Solution Partner&#039;s services.

Once created and accepted via API or App Dashboard, the solution&#039;s ID can be used to customize the Embedded Signup flow. Any clients onboarded via the customized flow can grant asset access to all of the solution&#039;s partners.

Note that solutions can also be set up via an embedded button that triggers an interface that gathers app information from Tech Providers. This flow and the API calls involved are described in the [Multi-Partner Solution — Embedded Creation](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/multi-partner-solution-embedded-creation) document, but the information below is still relevant and should be read first.

## Requirements

You must be an approved [Solution Partner](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/overview#solution-partners), a Tech Provider who has completed the steps in our [Get Started for Tech Providers](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/get-started-for-tech-providers) document appropriate for your intended usage, or a Tech Provider who has been upgraded to a [Tech Partner](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/overview#tech-partners).

If your app will be calling our APIs to access onboarded client data:

* The app must be the same app whose token will be used in API requests.
* The app must have undergone App Review and been approved for the [whatsapp_business_management](https://developers.facebook.com/docs/permissions#w) and [whatsapp_business_messaging](https://developers.facebook.com/docs/permissions#w) permissions.
* The app must be subscribed to the **account_updates** webhooks field and be able to successfully digest webhooks for onboarded clients.

## Creating Multi-Partner Solutions &#123;#creating-multi-partner-solutions&#125;

Use the **App Dashboard** &gt; **WhatsApp** &gt; **Partner Solutions** panel to create, accept, and manage solutions.

Solutions can be created by either partner of the solution. Once created, a solution request is sent to the invited partner, who can then use the panel in their App Dashboard to accept or decline the request. Once accepted, either partner can use the solution ID to customize the Embedded Signup flow and onboard clients.

### Solution states

Solutions states are displayed in the **Partner solutions** panel. Solutions can have the following states:

| State | Description |
| --- | --- |
| **Active** | The solution has been accepted by the invited party and can be used to configure Embedded Signup for client onboarding. |
| **Deactivated** | The solution has been deactivated.&lt;br&gt;&lt;br&gt;Clients who attempt to access Embedded Signup configured for a solution in this state will see an error informing them that it cannot be used for onboarding at this time. |
| **Draft** | The solution has been initiated and saved, but you have not sent it to your partner.&lt;br&gt;&lt;br&gt;Clients who attempt to access Embedded Signup configured for a solution in this state will see an error informing them that it cannot be used for onboarding at this time. |
| **Inactive** | The solution request was declined by your partner.&lt;br&gt;&lt;br&gt;Clients who attempt to access Embedded Signup configured for a solution in this state will see an error informing them that it cannot be used for onboarding at this time. |
| **Pending** | Solution has not been accepted or declined by your partner.&lt;br&gt;&lt;br&gt;Clients who attempt to access Embedded Signup configured for a solution in this state will see an error informing them that it cannot be used for onboarding at this time. |
| **Pending deactivation** | Your partner has requested to deactivate the solution. You can accept or decline this request. |

### Onboarding Limits

Tech Providers who are part of a solution can onboard up to 200 total new clients in a rolling one week period. Only clients who are new to the WhatsApp Business Platform count against this limit.

## Embedded Signup

[Embedded Signup](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/overview) can be configured and hosted by either of the solution&#039;s partners, or both partners. Once implemented, clients who access it will see a customized version of the Embedded Signup flow, which makes it clear that by completing the flow they are granting WhatsApp data access to both partners:

When a client completes the flow, all of the client&#039;s WhatsApp assets that we need are automatically generated, and access to those assets is granted to both partners of the solution.

## Billing

Clients onboarded via Embedded Signup configured with a solution ID share the credit line of the Solution Partner associated with the solution.

## Step 1: Determine Solution Details

Contact your potential partner and work together to determine:

* A solution name. The solution name will appear in the **Partner Solutions** panel in the App Dashboard for both you and your partner, so you should both agree on a name that can be distinguished from other solutions you may initiate or accept.
* Who will create and initiate the solution request. Either partner can do this. If you are initiating the request, you will need your partner&#039;s app ID.
* Who will host Embedded Signup configured with the solution ID. Either or both partners can do this.
* Anything else, such as contracts, service level agreements, services provided, billing processes, etc. This is left to the discretion of you and your partner, subject to each of your separate agreements with Meta.

## Step 2: Subscribe to Webhooks

Subscribe to the **account_update** and **partner_solutions** webhooks fields. These webhooks will inform you when new clients are onboarded, and when partner solutions that you are associated with are created or edited.

See the [Webhooks](#webhooks) section below for example payloads and what to look for when you receive any of these webhooks.

## Step 3: Create a Solution

If you are creating the solution, navigate to the **App Dashboard** &gt; **WhatsApp** &gt; **Partner solutions** panel and click the **Create a partner solution** button.

Use your partner&#039;s app ID to complete the flow. As part of the creation flow you can designate which solution partner apps can be used by onboarded clients to send messages (**Only me**, **Only my partner**).

Upon creation, an email and Meta Business Suite notification will be sent to your partner, and a **partner_solutions** webhook will be triggered.

The partner solution will appear in the **Partner solutions** panel with a **Pending** status until accepted by your partner. If accepted, its status will change to **Active**. If declined, its status will change to **Inactive**.

## Step 4: Accept The Solution Request

Everyone with admin (**Full control**) privileges on your business portfolio will be notified by email and Meta Business Suite notification when your partner sends you a partner solution request.

In addition, a [**partner_solutions**](#partner-solutions-webhook) webhook will be triggered with `event` set to `SOLUTION_CREATED` and `solution_status` set to `INITIATED`. Capture the included solution ID (`solution_id`) if you will be accepting/rejecting and managing the solution via API.

You can use either the App Dashboard or the API to accept the partner solution request.

### Via App Dashboard

The request will appear in the **App Dashboard** &gt; **WhatsApp** &gt; **Partner solutions** panel with a **Pending** status.

If you have multiple solutions and are having a hard time locating the solution request, use the dropdown menu in the top-right corner of the panel and filter by **Pending**.

Confirm that everything is correct before accepting the request, as solutions cannot be declined once they have been accepted.

Once you accept the solution, its status will be set to **Active** and you and your partner can use its ID to [configure Embedded Signup](#step-5-configure-embedded-signup).

If any information is incorrect, decline the request and ask your partner to submit a new request with the correct settings. Your partner will automatically be notified by email and Meta Business Suite notification if you decline the request.

### Via API

Before accepting the solution, use the [Solution API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-solution/solution-details-api#get-version-solution-id) to get details about the solution and confirm that everything is correct, as solutions cannot be declined once they have been accepted.

Include the following fields in your query:

* `name`
* `owner_permissions`
* `partners&#123;partner_permissions,partner_app&#125;`

#### Example request

```curl
curl -g &#039;https://graph.facebook.com/v25.0/795033096057724&amp;fields=name,owner_permissions,partners&#123;partner_permissions,partner_app&#125;&#039; \
-H &#039;Authorization: Bearer EAAAT...&#039;
```

#### Example response

```json
&#123;
  &quot;name&quot;: &quot;Social OVD with Lucky Shrub&quot;,
  &quot;owner_permissions&quot;: [
    &quot;MANAGE&quot;,
    &quot;DEVELOP&quot;,
    &quot;MANAGE_TEMPLATES&quot;,
    &quot;MANAGE_PHONE&quot;,
    &quot;VIEW_COST&quot;,
    &quot;MANAGE_EXTENSIONS&quot;,
    &quot;VIEW_PHONE_ASSETS&quot;,
    &quot;MANAGE_PHONE_ASSETS&quot;,
    &quot;VIEW_TEMPLATES&quot;,
    &quot;VIEW_INSIGHTS&quot;
  ],
  &quot;partners&quot;: &#123;
    &quot;data&quot;: [
      &#123;
        &quot;partner_permissions&quot;: [
          &quot;MANAGE&quot;,
          &quot;DEVELOP&quot;,
          &quot;MANAGE_TEMPLATES&quot;,
          &quot;MANAGE_PHONE&quot;,
          &quot;VIEW_COST&quot;,
          &quot;MANAGE_EXTENSIONS&quot;,
          &quot;VIEW_PHONE_ASSETS&quot;,
          &quot;MANAGE_PHONE_ASSETS&quot;,
          &quot;VIEW_TEMPLATES&quot;,
          &quot;VIEW_INSIGHTS&quot;
        ],
        &quot;partner_app&quot;: &#123;
          &quot;link&quot;: &quot;https://www.facebook.com/games/?app_id=21202248997039&quot;,
          &quot;name&quot;: &quot;Lucky Shrub&quot;,
          &quot;id&quot;: &quot;21202248997039&quot;
        &#125;,
        &quot;id&quot;: &quot;795033099391057&quot;
      &#125;
    ],
    &quot;paging&quot;: &#123;
      &quot;cursors&quot;: &#123;
        &quot;before&quot;: &quot;QVFIUl9hX0RqLUZAPemJQVWdsYTl5WlBsY0lCb0FNTExOY2N2NzJtRENZAbDd3azBNXzhPZAndqaU5sSXdfWWJaSXJ1S2pqMi0tQUdUdm1LTGZATUDNIdGRNNE1B&quot;,
        &quot;after&quot;: &quot;QVFIUl9hX0RqLUZAPemJQVWdsYTl5WlBsY0lCb0FNTExOY2N2NzJtRENZAbDd3azBNXzhPZAndqaU5sSXdfWWJaSXJ1S2pqMi0tQUdUdm1LTGZATUDNIdGRNNE1B&quot;
      &#125;
    &#125;
  &#125;,
  &quot;id&quot;: &quot;795033096057724&quot;
&#125;
```

* `name` — the name of the solution, as it appears in the App Dashboard.
* `owner_permissions` — the permissions your partner&#039;s app will be granted by clients who onboard via Embedded Signup.
* `partner_permissions` — the permissions your app will be granted by clients who onboard via Embedded Signup.
* `partner_app` — the app (your app) that will be granted the permissions identified in `partner_permissions`.

If everything is correct, use the [Solution Accept API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-solution/solution-accept-api#post-version-solution-id-accept) to accept the solution request, otherwise use the [Solution Reject API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-solution/solution-reject-api#post-version-solution-id-reject) to reject it.

#### Example accept request

```curl
curl -X POST &#039;https://graph.facebook.com/v25.0/795033096057724/accept&#039; \
-H &#039;Authorization: Bearer EAAAT...&#039;
```

#### Example reject request

```curl
curl -X POST &#039;https://graph.facebook.com/v25.0/795033096057724/reject&#039; \
-H &#039;Authorization: Bearer EAAAT...&#039;
```

#### Example response

Upon success:

```json
&#123;
  &quot;success&quot;: true
&#125;
```

## Step 5: Configure Embedded Signup &#123;#step-5-configure-embedded-signup&#125;

Assign the solution ID to the `solutionID` property in the `extras.setup` object within the [launch method and callback registration](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/implementation#launch-method-and-callback-registration) portion of the Embedded Signup code.

```html
// Launch method and callback registration
const launchWhatsAppSignup = () =&gt; &#123;
  FB.login(fbLoginCallback, &#123;
    config_id: &#039;&lt;CONFIGURATION_ID&gt;&#039;, // your configuration ID goes here, ensure it is in quotes
    response_type: &#039;code&#039;,
    override_default_response_type: true,
    extras: &#123;
      setup: &#123;
        solutionID: &#039;&lt;SOLUTION_ID&gt;&#039; // add solution ID here, ensure it is in quotes
      &#125;,
      featureType: &#039;&#039;,
      sessionInfoVersion: &#039;3&#039;,
    &#125;
  &#125;);
&#125;
```

Both you and your partner&#039;s business portfolio (**Business Settings** &gt; **Business Info**) will appear throughout the Embedded Signup flow.

Once configured, surface the customized Embedded Signup flow to clients on your platform wherever you feel it is appropriate. Note that if you have multiple active partner solutions, it is your responsibility to inject the correct solution ID into your Embedded Signup configuration and surface it to your intended clients, otherwise a client could be onboarded using the wrong solution.

## Step 6: Listen for onboarded clients

To listen for onboarded clients, your app must be subscribed to the [**account_update**](#account-update-webhook) webhook field.

When a client completes the Embedded Signup flow configured with your solution, an account update webhook is triggered with a `PARTNER_ADDED` or `PARTNER_APP_INSTALLED` event. Capture the `waba_id`, `solution_id`, and `owner_business_id` property values contained in the webhook payload, as well as any other values you may need in order to provide the client with WhatsApp messaging services.

In addition, we will send an email to admins of the business portfolio that owns the app, and a Meta Business Suite notification to the business portfolio that owns the app.

## Step 7: Share Your Credit Line (Solution Partners only)

If you are a Solution Partner, [share your line of credit](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/share-and-revoke-credit-lines#sharing-your-credit-line) with any clients newly onboarded via the partner solution.

**Note**: If you are a Solution Partner trying to add a user to a WhatsApp Business Account that is shared with you, you would need to account for the following scenarios:

- If you are not granted the `MESSAGING` permission on the solution, then you need to decide which granular tasks you need when adding the user to the shared WhatsApp Business Account: `DEVELOP`, `MANAGE_TEMPLATES`, `MANAGE_PHONE`, `VIEW_COST`, `MANAGE_EXTENSIONS`, `VIEW_PHONE_ASSETS`, `MANAGE_PHONE_ASSETS`, `VIEW_TEMPLATES`, `VIEW_INSIGHTS`, `MANAGE_USERS`, and `MANAGE_BILLING`.
- In this scenario, also note that `MANAGE_BILLING` is needed for credit line sharing.
- `MANAGE` will only work if you are given **Full access** on the solution, that is, including `MESSAGING`.

## Webhooks

### account_update &#123;#account-update-webhook&#125;

When a new client has successfully completed the Embedded Signup flow, an **account_update** webhook will be triggered with the `event` property set to `PARTNER_ADDED`.

```json
&#123;
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;&lt;BUSINESS_PORTFOLIO_ID&gt;&quot;,
      &quot;time&quot;: &lt;TIMESTAMP&gt;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;event&quot;: &quot;&lt;EVENT&gt;&quot;,
            &quot;waba_info&quot;: &#123;
              &quot;waba_id&quot;: &quot;&lt;BUSINESS_CUSTOMER_WABA_ID&gt;&quot;,
              &quot;owner_business_id&quot;: &quot;&lt;BUSINESS_CUSTOMER_BUSINESS_PORTFOLIO_ID&gt;&quot;,
              &quot;solution_id&quot;: &quot;&lt;SOLUTION_ID&gt;&quot;,
              &quot;solution_partner_business_ids&quot;: [&lt;SOLUTION_BUSINESS_PORTFOLIO_IDS&gt;]
            &#125;
          &#125;,
          &quot;field&quot;: &quot;account_update&quot;
        &#125;
      ]
    &#125;
  ],
  &quot;object&quot;: &quot;whatsapp_business_account&quot;
&#125;
```

#### Payload Properties

| Placeholder | Description | Example |
| --- | --- | --- |
| `&lt;BUSINESS_PORTFOLIO_ID&gt;` | Your business portfolio ID. | `506914307656634` |
| `&lt;BUSINESS_CUSTOMER_BUSINESS_PORTFOLIO_ID&gt;` | Onboarded client&#039;s business portfolio ID. | `6143763655652543` |
| `&lt;BUSINESS_CUSTOMER_WABA_ID&gt;` | Onboarded client&#039;s WhatsApp Business Account ID. | `102290129340398` |
| `&lt;EVENT&gt;` | If set to `PARTNER_ADDED`, indicates that the client has successfully completed the Embedded Signup flow. | `PARTNER_ADDED` |
| `&lt;SOLUTION_BUSINESS_PORTFOLIO_IDS&gt;` | Strings of business portfolio IDs of the Tech Provider (or Tech Partner) and Solution Partner associated with the solution. | `&quot;506914307656634&quot;,&quot;116133292427920&quot;` |
| `&lt;SOLUTION_ID&gt;` | Solution ID. | `303610109049230` |
| `&lt;TIMESTAMP&gt;` | UNIX timestamp indicating when the client successfully completed the Embedded Signup flow. | `1690592557` |

### partner_solutions &#123;#partner-solutions-webhook&#125;

When a Multi-Partner Solution is created or modified, a **partner_solutions** webhook describing the change will be triggered.

```json
&#123;
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;&lt;BUSINESS_PORTFOLIO_ID&gt;&quot;,
      &quot;time&quot;: &lt;TIMESTAMP&gt;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;event&quot;: &quot;SOLUTION_CREATED&quot;,
            &quot;solution_id&quot;: &quot;&lt;SOLUTION_ID&gt;&quot;,
            &quot;solution_status&quot;: &quot;INITIATED&quot;
          &#125;,
          &quot;field&quot;: &quot;partner_solutions&quot;
        &#125;
      ]
    &#125;
  ],
  &quot;object&quot;: &quot;whatsapp_business_account&quot;
&#125;
```

#### Payload Properties

| Placeholder | Description | Example |
| --- | --- | --- |
| `&lt;BUSINESS_PORTFOLIO_ID&gt;` | Your business portfolio ID. | `506914307656634` |
| `&lt;EVENT&gt;` | Event description. Values can be:&lt;br&gt;&lt;br&gt;* `SOLUTION_CREATED` — Solution created.&lt;br&gt;* `SOLUTION_UPDATED` — The `solution_status` has changed. | `SOLUTION_CREATED` |
| `&lt;SOLUTION_ID&gt;` | Solution ID. | `774485461512159` |
| `&lt;SOLUTION_STATUS&gt;` | Solution status. Values can be:&lt;br&gt;&lt;br&gt;* `ACTIVE` — The solution partner has accepted the solution request and it can be used with Embedded Signup.&lt;br&gt;* `DEACTIVATED` — The solution has been deactivated and cannot be used with Embedded Signup to onboard clients.&lt;br&gt;* `DRAFT` — Solution has been drafted but invitation has not been sent to the solution partner.&lt;br&gt;* `INITIATED` — The solution partner has been invited to accept the solution, but has yet to accept or reject the request.&lt;br&gt;* `PENDING` — The solution partner has yet to accept or reject the solution request.&lt;br&gt;* `PENDING_DEACTIVATION`  — The solution owner requested for an active solution to be deactivated but the solution partner has yet to accept the deactivation request.&lt;br&gt;* `REJECTED` — The solution partner has rejected the solution request. | `INITIATED` |
| `&lt;TIMESTAMP&gt;` | UNIX timestamp indicating when the client successfully completed the Embedded Signup flow. | `1718143652` |

## Editing or Deactivating Solutions

You can use the App Dashboard or API to edit or deactivate a solution.

When you request deactivation, the solution&#039;s status will change to **Pending deactivation** and your partner will be notified by email and Meta Business Suite notification. In addition, a [**partner_solutions**](#partner-solutions-webhook) webhook will be triggered with `event` set to `SOLUTION_UPDATED` and `solution_status` set to `PENDING_DEACTIVATION`. Your partner can then accept or reject your request.

Note that partner solutions can still be used to onboard clients until your partner accepts the deactivation request.

If the deactivation request is rejected, the solution will remain in an **Active** state and can continue to be used to onboard clients.

If the deactivation request is accepted, the solution status will be set to **Deactivated** and can no longer be used to onboard clients, so make sure that neither you nor your partner are surfacing it to clients.

### Limitations

* You can only edit solutions that were created by you.
* You can request deactivation of any solutions that you create which are in an **Active** state.

### Via App Dashboard

Use the **App Dashboard** &gt; **WhatsApp** &gt; **Partner solutions** panel to edit or deactivate a solution. Note that you can only edit solutions that were initiated by you.

| State | Permitted actions |
| --- | --- |
| **Active** | You may edit the solution name, or deactivate the solution. |
| **Deactivated** | Solutions in this state cannot be edited. |
| **Draft** | You may edit the solution name. |
| **Inactive** | You may edit the solution name. |
| **Pending** | Solutions in this state cannot be edited until accepted or declined by your partner. |
| **Pending deactivation** | You may accept or decline the partner&#039;s deactivation request. |

### Via API

### Send deactivation request

Use the [Send Deactivation Request API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-solution/send-deactivation-request-api#post-version-solution-id-send-deactivation-request) to send a solution deactivation request. You must be the solution owner in order to send this request.

#### Example request

```curl
curl -X POST &#039;https://graph.facebook.com/v20.0/795033096057724/send_deactivation_request \
-H &#039;Authorization: Bearer EAAAT...&#039;
```

#### Example response

Upon success:

```json
&#123;
    &quot;success&quot;: true
&#125;
```

### Accept deactivation request

Use the [Accept Deactivation Request API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-solution/accept-deactivation-request-api#post-version-solution-id-accept-deactivation-request) to accept a solution deactivation request. You must be the solution owner in order to send this request.

#### Example request

```curl
curl -X POST &#039;https://graph.facebook.com/v20.0/795033096057724/accept_deactivation_request \
-H &#039;Authorization: Bearer EAAAT...&#039;
```

#### Example response

Upon success:

```json
&#123;
    &quot;success&quot;: true
&#125;
```

### Reject deactivation request

Use the [Reject Deactivation Request API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-solution/reject-deactivation-request-api#post-version-solution-id-reject-deactivation-request) to reject a solution deactivation request. You must be the solution owner in order to send this request.

#### Example request

```curl
curl -X POST &#039;https://graph.facebook.com/v20.0/795033096057724/reject_deactivation_request \
-H &#039;Authorization: Bearer EAAAT...&#039;
```

#### Example response

Upon success:

```json
&#123;
    &quot;success&quot;: true
&#125;
```

## Manually checking for onboarded clients

As a fallback in case of webhook problems, you can manually check for onboarded clients using the [Client WhatsApp Business Accounts API](https://developers.facebook.com/documentation/ads-commerce/marketing-api/reference/business/client_whatsapp_business_accounts#get-version-business-id-client-whatsapp-business-accounts), which returns WABA IDs of all clients newly onboarded via the solution.

### Request Syntax

```https
GET /&lt;BUSINESS_PORTFOLIO_ID&gt;/client_whatsapp_business_accounts
  ?filtering=[
    &#123;
      &quot;field&quot;:&quot;partners&quot;,
      &quot;operator&quot;:&quot;ALL&quot;,
      &quot;value&quot;:[
        &quot;&lt;PARTNER_BUSINESS_PORTFOLIO_ID&gt;&quot;
      ]
    &#125;
  ]
```

Replace `&lt;PARTNER_BUSINESS_PORTFOLIO_ID&gt;` with your partner&#039;s business portfolio ID.

### Response

```json
&#123;
  &quot;data&quot;: [
    &#123;
      &quot;id&quot;: &quot;&lt;CUSTOMER_WABA_ID&gt;&quot;,
      &quot;name&quot;: &quot;&lt;CUSTOMER_WABA_NAME&gt;&quot;,
      &quot;timezone_id&quot;: &quot;&lt;CUSTOMER_WABA_TIMEZONE_ID&gt;&quot;,
      &quot;business_type&quot;: &quot;ent&quot;,
      &quot;message_template_namespace&quot;: &quot;&lt;MESSAGE_TEMPLATE_NAMESPACE&gt;&quot;
    &#125;
    ...
  ],
  &quot;paging&quot;: &#123;
    &quot;cursors&quot;: &#123;
      &quot;before&quot;: &quot;&lt;BEFORE&gt;&quot;,
      &quot;after&quot;: &quot;&lt;AFTER&gt;&quot;
    &#125;,
    &quot;next&quot;: &quot;&lt;NEXT&gt;&quot;
  &#125;
&#125;
```

| Placeholder | Description | Example Value |
| --- | --- | --- |
| `&lt;CUSTOMER_WABA_ID&gt;` | Client WhatsApp Business Account ID. | `102290129340398` |
| `&lt;CUSTOMER_WABA_NAME&gt;` | Client WhatsApp Business Account name. | `Cool New Customer 2` |
| `&lt;CUSTOMER_WABA_TIMEZONE_ID&gt;` | Client&#039;s WhatsApp Business Account timezone ID. | `7` |
| `&lt;BEFORE&gt;` | Paginated results cursor. See [Paginated Results](https://developers.facebook.com/docs/graph-api/results). | `QVFIU...` |
| `&lt;AFTER&gt;` | Paginated results cursor. See [Paginated Results](https://developers.facebook.com/docs/graph-api/results). | `QVFIU...` |
| `&lt;NEXT&gt;` | Paginated results link. See [Paginated Results](https://developers.facebook.com/docs/graph-api/results). | `https://graph.facebook.com/v18.0/50691...` |

### Example Request

```curl
curl -g &#039;https://graph.facebook.com/v25.0/506914307656634/client_whatsapp_business_accounts?filtering=[&#123;%22field%22%3A%22partners%22%2C%20%22operator%22%3A%20%22ALL%22%2C%20%22value%22%3A%20[%22520744086200222%22]&#125;]&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039;
```

### Example Response

```json
&#123;
  &quot;data&quot;: [
    &#123;
      &quot;id&quot;: &quot;102290129340398&quot;,
      &quot;name&quot;: &quot;Cool New Customer 2&quot;,
      &quot;timezone_id&quot;: &quot;7&quot;,
      &quot;business_type&quot;: &quot;ent&quot;,
      &quot;message_template_namespace&quot;: &quot;&lt;MESSAGE_TEMPLATE_NAMESPACE&gt;&quot;
    &#125;,
    &#123;
      &quot;id&quot;: &quot;112077945305052&quot;,
      &quot;name&quot;: &quot;Cool New Customer 1&quot;,
      &quot;timezone_id&quot;: &quot;7&quot;,
      &quot;business_type&quot;: &quot;ent&quot;,
      &quot;message_template_namespace&quot;: &quot;&lt;MESSAGE_TEMPLATE_NAMESPACE&gt;&quot;
    &#125;
    ...
  ],
  &quot;paging&quot;: &#123;
    &quot;cursors&quot;: &#123;
      &quot;before&quot;: &quot;QVFIU...&quot;,
      &quot;after&quot;: &quot;QVFIU...&quot;
    &#125;,
    &quot;next&quot;: &quot;https://graph.facebook.com/v25.0/50691...&quot;
  &#125;
&#125;
```

## Getting Solution Data

### Get fields on a solution

Use the [Solution API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-solution/solution-details-api#get-version-solution-id) to get default fields on a solution, or use the `fields` query string parameter to request specific fields.

#### Example Request

```curl
curl &#039;https://graph.facebook.com/v25.0/17602267745700?fields=name,status,partners&#039; \
-H &#039;Authorization: Bearer EAAAT...&#039;
```

#### Example Response

```json
&#123;
  &quot;name&quot;: &quot;Social OVD with Lucky Shrub&quot;,
  &quot;status&quot;: &quot;ACTIVE&quot;,
  &quot;partners&quot;: &#123;
    &quot;data&quot;: [
      &#123;
        &quot;partner_app&quot;: &#123;
          &quot;link&quot;: &quot;https://www.socialoverdrive.com/&quot;,
          &quot;name&quot;: &quot;Social Overdrive&quot;,
          &quot;id&quot;: &quot;637576208107267&quot;
        &#125;,
        &quot;status&quot;: &quot;ACCEPTED&quot;,
        &quot;id&quot;: &quot;17602267745704&quot;
      &#125;
    ],
    &quot;paging&quot;: &#123;
      &quot;cursors&quot;: &#123;
        &quot;before&quot;: &quot;QVFIUmxnSE9LUFliNzlUTWdhTlYzQjBtekprSC0wQUdoZAGRYbFlzeUpDMG9yNkF1OHYyel9tcUlBbGhFckxJQ1Y3UFZA4dUkycEk0WDJwRGYzT2JYbVhEdFdB&quot;,
        &quot;after&quot;: &quot;QVFIUmxnSE9LUFliNzlUTWdhTlYzQjBtekprSC0wQUdoZAGRYbFlzeUpDMG9yNkF1OHYyel9tcUlBbGhFckxJQ1Y3UFZA4dUkycEk0WDJwRGYzT2JYbVhEdFdB&quot;
      &#125;
    &#125;
  &#125;,
  &quot;id&quot;: &quot;17602267745700&quot;
&#125;
```

### Get solutions associated with your app

Use the [WhatsApp Business Solutions API](https://developers.facebook.com/docs/graph-api/reference/application/whatsapp_business_solutions) to get a list of solutions your app is associated with.

#### Example Request

```curl
curl &#039;https://graph.facebook.com/v25.0/21202248997039/whatsapp_business_solutions&#039; \
-H &#039;Authorization: Bearer EAAAT...&#039;
```

#### Example Response

```json
&#123;
  &quot;data&quot;: [
    &#123;
      &quot;name&quot;: &quot;Social OVD with Lucky Shrub&quot;,
      &quot;status&quot;: &quot;INITIATED&quot;,
      &quot;status_for_pending_request&quot;: &quot;PENDING_ACTIVATION&quot;,
      &quot;id&quot;: &quot;19702253086782&quot;
    &#125;,
    &#123;
      &quot;name&quot;: &quot;Social OVD with Social Brew&quot;,
      &quot;status&quot;: &quot;ACTIVE&quot;,
      &quot;status_for_pending_request&quot;: &quot;NONE&quot;,
      &quot;id&quot;: &quot;17602267745700&quot;
    &#125;
  ],
  &quot;paging&quot;: &#123;
    &quot;cursors&quot;: &#123;
      &quot;before&quot;: &quot;QVFIUkxlbkhTZA1VleGwyWHd3SmlSMnlnelhlbUVSSjVYQmU2aXVmb1YyWk9JTkx3b2gwNE9FS3J2ejMzNENxbmh1bWZAqSkZAJUzNfbmF4NmtPaFYxQldaaXR3&quot;,
      &quot;after&quot;: &quot;QVFIUlgyLTlQYWV0eTNGWXVhcTJnOEhzY1lvUDloVV8wUUxVQk9YMVJ5UGlBZAmx1Q1BjaEVwd0tWdmNvRU9jdGRiNnlrc193alRNaDV2SXZAfN1kybDBibEFR&quot;
    &#125;
  &#125;
&#125;
```

### Get solutions that onboarded a WABA

Use the [Solutions API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/whatsapp-business-account-solutions-list-api#get-version-waba-id-solutions) to get a list of solutions that onboarded a specific WABA.

#### Example Request

```curl
curl &#039;https://graph.facebook.com/v25.0/102290129340398/solutions&#039; \
-H &#039;Authorization: Bearer EAAAT...&#039;
```

#### Example Response

```json
&#123;
  &quot;data&quot;: [
    &#123;
      &quot;name&quot;: &quot;Social OVD with Social Brew&quot;,
      &quot;status&quot;: &quot;ACTIVE&quot;,
      &quot;status_for_pending_request&quot;: &quot;NONE&quot;,
      &quot;id&quot;: &quot;17602267745700&quot;
    &#125;
  ],
  &quot;paging&quot;: &#123;
    &quot;cursors&quot;: &#123;
      &quot;before&quot;: &quot;QVFIUjZACTFNmWURVTHN2NFVaM2ZApd2RaOGIxOU5wenpQZADFkbVdtSEJDSGFDelhDOU5hT28xcmJLS05TM3U0UUFmdVNGUWFfdjdJb1o2OTVNY083ZAHYtc2x3&quot;,
      &quot;after&quot;: &quot;QVFIUjZACTFNmWURVTHN2NFVaM2ZApd2RaOGIxOU5wenpQZADFkbVdtSEJDSGFDelhDOU5hT28xcmJLS05TM3U0UUFmdVNGUWFfdjdJb1o2OTVNY083ZAHYtc2x3&quot;
    &#125;
  &#125;
&#125;
```

### Get partners of a solution

Use the [Partners API](https://developers.facebook.com/docs/graph-api/reference/whats-app-business-solution/partners) to get a list of partners of a solution.

#### Example Request

```curl
curl &#039;https://graph.facebook.com/v25.0/17602267745700/partners&#039; \
-H &#039;Authorization: Bearer EAAAT...&#039;
```

#### Example Response

```json
&#123;
  &quot;data&quot;: [
    &#123;
      &quot;partner_app&quot;: &#123;
        &quot;link&quot;: &quot;https://www.socialoverdrive.com&quot;,
        &quot;name&quot;: &quot;Social Overdrive&quot;,
        &quot;id&quot;: &quot;637576208107267&quot;
      &#125;,
      &quot;status&quot;: &quot;ACCEPTED&quot;,
      &quot;id&quot;: &quot;17602267745704&quot;
    &#125;
  ],
  &quot;paging&quot;: &#123;
    &quot;cursors&quot;: &#123;
      &quot;before&quot;: &quot;QVFIUmxnSE9LUFliNzlUTWdhTlYzQjBtekprSC0wQUdoZAGRYbFlzeUpDMG9yNkF1OHYyel9tcUlBbGhFckxJQ1Y3UFZA4dUkycEk0WDJwRGYzT2JYbVhEdFdB&quot;,
      &quot;after&quot;: &quot;QVFIUmxnSE9LUFliNzlUTWdhTlYzQjBtekprSC0wQUdoZAGRYbFlzeUpDMG9yNkF1OHYyel9tcUlBbGhFckxJQ1Y3UFZA4dUkycEk0WDJwRGYzT2JYbVhEdFdB&quot;
    &#125;
  &#125;
&#125;
```

## Getting client business tokens

If you are not hosting Embedded Signup but want to get an onboarded client&#039;s [business integration system user access token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#business-integration-system-user-access-tokens) (&quot;business token&quot;), you can get their token using the business portfolio ID and solution ID contained in the [account_update](#account-update-webhook) webhook that was triggered when the client completed the Embedded Signup flow.

### Request

Use the [GET /&lt;SOLUTION_ID&gt;/access_token](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-solution/access-token-api#Reading) endpoint and request the `business_id` parameter to get an onboarded business customer&#039;s [business token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#business-integration-system-user-access-tokens).


```html
curl &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;SOLUTION_ID&gt;/access_token?business_id=&lt;CUSTOMER_BUSINESS_PORTFOLIO_ID&gt;&#039; \
-H &#039;Authorization: Bearer &lt;SYSTEM_TOKEN&gt;&#039;
```


### Request parameters

| Placeholder | Description | Example Value |
| --- | --- | --- |
| `&lt;API_VERSION&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Graph API version. | v25.0 |
| `&lt;CUSTOMER_BUSINESS_PORTFOLIO_ID&gt;` | **Required.**&lt;br&gt;&lt;br&gt;The onboarded business customer&#039;s business portfolio ID.&lt;br&gt;&lt;br&gt;This is included in [account_update webhooks](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/manage-webhooks#onboarded-business-customer) when the business customer completes Embedded Signup. | `2729063490586005` |
| `&lt;SOLUTION_ID&gt;` | **Required.**&lt;br&gt;&lt;br&gt;Your Multi-Partner Solution ID. | `303610109049230` |
| `&lt;SYSTEM_TOKEN&gt;` | **Required.**&lt;br&gt;&lt;br&gt;Your [system user access token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#system-user-access-tokens). | `EAAAN6tcBzAUBOZC82CW7iR2LiaZBwUHS4Y7FDtQxRUPy1PHZClDGZBZCgWdrTisgMjpFKiZAi1FBBQNO2IqZBAzdZAA16lmUs0XgRcCf6z1LLxQCgLXDEpg80d41UZBt1FKJZCqJFcTYXJvSMeHLvOdZwFyZBrV9ZPHZASSqxDZBUZASyFdzjiy2A1sippEsF4DVV5W2IlkOSr2LrMLuYoNMYBy8xQczzOKDOMccqHEZD` |

### Response

Upon success:

```html
&#123;
  &quot;data&quot;: [
    &#123;
      &quot;access_token&quot;: &quot;&lt;CUSTOMER_BUSINESS_TOKEN&gt;&quot;
    &#125;
  ]
&#125;
```


## Migrating client assets among solutions

You have several options for migrating client assets to and from Multi-Partner Solutions. See [Migrating client assets](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/support#migrating-client-assets).
