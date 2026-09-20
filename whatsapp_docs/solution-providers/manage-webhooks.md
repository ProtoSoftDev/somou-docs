# Managing webhooks



**Warning:** **Embedded signup v2 will be deprecated on October 15, 2026.** Migrate your integration to [v4](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/version-4) before that date to avoid disruption. See [Versions](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/versions) for the full upgrade path.

WhatsApp Business Accounts (WABAs) and their assets are objects in the Facebook Social Graph. When a trigger event occurs to one of those objects, Facebook detects the event and sends a notification to the webhook URL specified in your Facebook App&#039;s dashboard.

In the context of Embedded Signup, you can use webhooks to get notifications of changes to your WABAs, phone numbers, message templates, and messages sent to your phone numbers.

**You must [individually subscribe to every WABA](#subscribe-to-webhooks-on-a-client-waba) for which you want to receive webhooks.** After fetching the client&#039;s WABA ID, subscribe your app to the ID to start receiving webhooks.

See [Webhooks](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/overview) for more information about webhooks and fields.

## Subscribe to webhooks on a client WABA

Use the [POST /&lt;WABA_ID&gt;/subscribed_apps](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/subscribed-apps-api#post-version-waba-id-subscribed-apps) endpoint to subscribe your app to webhooks on the business customer&#039;s WABA. If you want the customer&#039;s webhooks to be sent to a different callback URL than the one set on your app, you have multiple [webhook override](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/override) options.


### Request

```html
curl -X POST &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;WABA_ID&gt;/subscribed_apps&#039; \
-H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039;
```


### Response

Upon success:

```json
&#123;
  &quot;success&quot;: true
&#125;
```


Repeat this process for any other WABAs for which you want to receive webhook notifications. If you subscribe your app to webhooks for multiple WABAs, WhatsApp sends all webhook notifications to the app&#039;s callback URL specified in the **Webhooks** product panel of the App Dashboard, unless you [override webhooks](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/override).

## Get all subscriptions on a WABA

Use the [Subscribed Apps API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/subscribed-apps-api#get-version-waba-id-subscribed-apps) to get a list of apps subscribed to webhooks for a WABA:

### Request syntax

```html
GET https://graph.facebook.com/v25.0/&lt;WABA_ID&gt;/subscribed_apps
```

A successful response includes an array of apps that have subscribed to the WABA, with `link`, `name`, and `id` properties for each app.

### Sample request

```curl
curl \
&#039;https://graph.facebook.com/v25.0/102289599326934/subscribed_apps&#039; \
-H &#039;Authorization: Bearer EAAJi...&#039;
```

### Sample response

```json
&#123;
  &quot;data&quot; : [
    &#123;
      &quot;whatsapp_business_api_data&quot; : &#123;
        &quot;id&quot; : &quot;67084...&quot;,
        &quot;link&quot; : &quot;https://www.facebook.com/games/?app_id=67084...&quot;,
        &quot;name&quot; : &quot;Jaspers Market&quot;
      &#125;
    &#125;,
    &#123;
      &quot;whatsapp_business_api_data&quot; : &#123;
        &quot;id&quot; : &quot;52565...&quot;,
        &quot;link&quot; : &quot;https://www.facebook.com/games/?app_id=52565...&quot;,
        &quot;name&quot; : &quot;Jaspers Fresh Finds&quot;
      &#125;
    &#125;
  ]
&#125;
```

## Unsubscribe from a WABA

Use the [Subscribed Apps API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/subscribed-apps-api#delete-version-waba-id-subscribed-apps) to unsubscribe your app from webhooks for a WhatsApp Business account.

### Request syntax

```html
DELETE https://graph.facebook.com/v25.0/&lt;WABA_ID&gt;/subscribed_apps
```

### Sample request

```curl
curl -X DELETE \
&#039;https://graph.facebook.com/v25.0/102289599326934/subscribed_apps&#039; \
-H &#039;Authorization: Bearer EAAJi...&#039;
```

### Sample response

```json
&#123;
   &quot;success&quot; : true
&#125;
```

## Overriding the callback URL

See [Webhooks Overrides](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/override).

## Set up notifications

You can set up webhooks to send you notifications of changes to your subscribed WhatsApp Business Accounts. The types of notifications you can subscribe to are:

### Available subscription fields

| Field name | Description |
| --- | --- |
| [account_alerts](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/account_alerts) | The **account_alerts** webhook notifies you of changes to a business phone number&#039;s [messaging limit](https://developers.facebook.com/documentation/business-messaging/whatsapp/messaging-limits), [business profile](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-phone-numbers/phone-numbers#business-profiles), and [Official Business Account](https://developers.facebook.com/documentation/business-messaging/whatsapp/whatsapp-business-accounts#official-business-account) status. |
| [account_review_update](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/account_review_update) | The **account_review_update** webhook notifies you when a WhatsApp Business Account has been reviewed against our policy guidelines. |
| [account_update](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/account_update) | The **account_update** webhook notifies of changes to a WhatsApp Business Account&#039;s [partner-led business verification](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/partner-led-business-verification) submission, its [authentication-international rate](https://developers.facebook.com/documentation/business-messaging/whatsapp/pricing/authentication-international-rates) eligibility, or primary business location, when it is shared with a [Solution Partner](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/overview), [policy or terms violations](https://developers.facebook.com/documentation/business-messaging/whatsapp/policy-enforcement), offboarding, reconnection, or when it is deleted. |
| [automatic_events](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/automatic_events) | The **automatic_events** webhook notifies you when we detect a purchase or lead event in a chat thread between you and a WhatsApp user who has messaged you via your Click to WhatsApp ad, if you have opted-in to [Automatic Events](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/automatic-events-api) reporting. |
| [business_capability_update](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/business_capability_update) | The **business_capability_update** webhook notifies you of WhatsApp Business Account or business portfolio capability changes ([messaging limits](https://developers.facebook.com/documentation/business-messaging/whatsapp/messaging-limits#increasing-your-limit), [phone number limits](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-phone-numbers/phone-numbers#registered-number-cap), etc.). |
| [history](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/history) | The **history** webhook is used to synchronize the [WhatsApp Business app chat history](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/onboarding-business-app-users) of a business customer onboarded by a solution provider. |
| [message_template_components_update](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/message_template_components_update) | The **message_template_components_update** webhook notifies you of changes to a template&#039;s components. |
| [message_template_quality_update](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/message_template_quality_update) | The **message_template_quality_update** webhook notifies you of changes to a template&#039;s [quality score](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/template-quality). |
| [message_template_status_update](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/message_template_status_update) | The **message_template_status_update** webhook notifies you of changes to the status of an existing template. |
| [messages](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/messages) | The **messages** webhook describes messages sent from a WhatsApp user to a business and the status of messages sent by a business to a WhatsApp user. |
| [partner_solutions](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/partner_solutions) | The **partner_solutions webhook** describes changes to the status of a [Multi-Partner Solution](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/multi-partner-solutions). |
| [payment_configuration_update](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/payment_configuration_update) | The **payment_configuration_update** webhook notifies you of changes to payment configurations for [Payments API India](https://developers.facebook.com/documentation/business-messaging/whatsapp/payments/payments-in/overview) and [Payments API Brazil](https://developers.facebook.com/documentation/business-messaging/whatsapp/payments/payments-br/overview). |
| [phone_number_name_update](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/phone_number_name_update) | The **phone_number_name_update** webhook notifies you of business phone number [display name verification](https://developers.facebook.com/documentation/business-messaging/whatsapp/display-names#display-name-verificationn) outcomes. |
| [phone_number_quality_update](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/phone_number_quality_update) | The **phone_number_quality_update** webhook notifies you of changes to a business phone number&#039;s [throughput level](https://developers.facebook.com/documentation/business-messaging/whatsapp/throughput). |
| [security](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/security) | The **security** webhook notifies you of changes to a business phone number&#039;s security settings. |
| [smb_app_state_sync](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/smb_app_state_sync) | The **smb_app_state_sync** webhook is used for synchronizing contacts of [WhatsApp Business app users who have been onboarded](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/onboarding-business-app-users) via a solution provider. |
| [smb_message_echoes](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/smb_message_echoes) | The **smb_message_echoes** webhook notifies you of messages sent via the WhatsApp Business app or a [companion (&quot;linked&quot;) device](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/onboarding-business-app-users#linked-devices) by a business customer who has been [onboarded to Cloud API](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/onboarding-business-app-users) via a solution provider. |
| [template_category_update](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/template_category_update) | The **template_category_update** webhook notifies you of changes to template&#039;s [category](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/template-categorization). |
| [user_preferences](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/user_preferences) | The **user_preferences** webhook notifies you of changes to a WhatsApp user&#039;s [marketing message preferences](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/marketing-templates#user-preferences-for-marketing-messages). |

## Examples

### Onboarded client

An **account_update** webhook is triggered with `event` set to `PARTNER_ADDED` when a client successfully completes the Embedded Signup flow.

#### Syntax

```html
&#123;
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;&lt;BUSINESS_PORTFOLIO_ID&gt;&quot;,
      &quot;time&quot;: &lt;WEBHOOK_SENT_TIMESTAMP&gt;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;event&quot;: &quot;&lt;EVENT&gt;&quot;,
            &quot;waba_info&quot;: &#123;
              &quot;waba_id&quot;: &quot;&lt;CUSTOMER_WABA_ID&gt;&quot;,
              &quot;owner_business_id&quot;: &quot;&lt;CUSTOMER_BUSINESS_PORTFOLIO_ID&gt;&quot;
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


#### Example

```json
&#123;
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;35602282435505&quot;,
      &quot;time&quot;: 1731617831,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;event&quot;: &quot;PARTNER_ADDED&quot;,
            &quot;waba_info&quot;: &#123;
              &quot;waba_id&quot;: &quot;495709166956424&quot;,
              &quot;owner_business_id&quot;: &quot;942647313864044&quot;
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

### Phone number updates

#### Name update received

```json
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;495709166956424&quot;,
      &quot;time&quot;: 1731617831,
      &quot;changes&quot;: [
        &#123;
          &quot;field&quot;: &quot;phone_number_name_update&quot;,
          &quot;value&quot;: &#123;
            &quot;display_phone_number&quot;: &quot;124545784358810&quot;,
            &quot;decision&quot;: &quot;APPROVED&quot;,
            &quot;requested_verified_name&quot;: &quot;WhatsApp&quot;,
            &quot;rejection_reason&quot;: null
          &#125;
        &#125;
      ]
    &#125;
  ]
&#125;
```


#### Quality update received

```json
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;495709166956424&quot;,
      &quot;time&quot;: 1731617831,
      &quot;changes&quot;: [
        &#123;
          &quot;field&quot;: &quot;phone_number_quality_update&quot;,
          &quot;value&quot;: &#123;
            &quot;display_phone_number&quot;: &quot;124545784358810&quot;,
            &quot;event&quot;: &quot;FLAGGED&quot;,
            &quot;current_limit&quot;: &quot;TIER_10K&quot;
          &#125;
        &#125;
      ]
    &#125;
  ]
&#125;
```


### WABA updates

#### Sandbox number upgraded to verified account

```json
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;495709166956424&quot;,
      &quot;time&quot;: 1731617831,
      &quot;changes&quot;: [
        &#123;
          &quot;field&quot;: &quot;account_update&quot;,
          &quot;value&quot;: &#123;
            &quot;phone_number&quot;: &quot;124545784358810&quot;,
            &quot;event&quot;: &quot;VERIFIED_ACCOUNT&quot;
          &#125;
        &#125;
      ]
    &#125;
  ]
&#125;
```


#### WhatsApp Business account banned

```json
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;495709166956424&quot;,
      &quot;time&quot;: 1731617831,
      &quot;changes&quot;: [
        &#123;
          &quot;field&quot;: &quot;account_update&quot;,
          &quot;value&quot;: &#123;
            &quot;event&quot;: &quot;DISABLED_UPDATE&quot;
            &quot;ban_info&quot;: &#123;
              &quot;waba_ban_state&quot;: [&quot;SCHEDULE_FOR_DISABLE&quot;, &quot;DISABLE&quot;, &quot;REINSTATE&quot;],
              &quot;waba_ban_date&quot;: &quot;DATE&quot;
            &#125;
          &#125;
        &#125;
      ]
    &#125;
  ]
&#125;
```


#### WhatsApp Business account review completed

```json
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;495709166956424&quot;,
      &quot;time&quot;: 1731617831,
      &quot;changes&quot;: [
        &#123;
          &quot;field&quot;: &quot;account_review_update&quot;,
          &quot;value&quot;: &#123;
            &quot;decision&quot;: &quot;APPROVED&quot;
          &#125;
        &#125;
      ]
    &#125;
  ]
&#125;
```


### Message template updates

#### Approved

```json
&#123;
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;495709166956424&quot;,
      &quot;time&quot;: 1731617831,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;event&quot;: &quot;APPROVED&quot;,
            &quot;message_template_id&quot;: 64244916695,
            &quot;message_template_name&quot;: &quot;Summer 20 Template&quot;,
            &quot;message_template_language&quot;: &quot;en_US&quot;,
            &quot;reason&quot;: &quot;NONE&quot;
          &#125;,
          &quot;field&quot;: &quot;message_template_status_update&quot;
        &#125;
      ]
    &#125;
  ],
  &quot;object&quot;: &quot;whatsapp_business_account&quot;
&#125;
```

