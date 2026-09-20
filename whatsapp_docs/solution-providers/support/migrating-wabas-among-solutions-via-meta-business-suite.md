# Migrating a WABA from one Multi-Partner Solution to another via Meta Business Suite



You can use the API and the Meta Business Suite&#039;s **Business settings** panel to migrate a client&#039;s WhatsApp Business account (WABA) from one [Multi-Partner Solution](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/multi-partner-solutions) (the &quot;source solution&quot;) to another (the &quot;destination solution&quot;).

As part of this process, a new WhatsApp Business account (WABA) will be created for the client, templates within the source WABA will be duplicated in the destination WABA, and access to the WABA and its assets will be granted to the destination solution&#039;s Solution Partner.

Note that both you and the destination solution&#039;s Solution Partner must perform one or more API requests to complete the process.

## Requirements

Your app (or apps, if you are using separate apps) used to create or accept each solution must be associated with the same business portfolio, and the destination solution must be in an active state.

## Templates

Templates are automatically duplicated in the destination WABA and initially granted the same status as their source counterparts.

After duplication however, templates are re-checked to ensure they are correctly categorized according to our [guidelines](https://developers.facebook.com/documentation/business-messaging/whatsapp/pricing). This may result in some duplicated templates having their `status` set to `REJECTED`.

Only templates with both a `status` of `APPROVED` and `quality_score` of `GREEN` are eligible for duplication. If the destination WABA cannot accommodate all of the new templates, we will duplicate as many as we can until the destination WABA&#039;s template limit has been reached. Unduplicated templates must be re-created and submitted for approval if they are to be used by the destination WABA.

Note that **template quality ratings are not duplicated**. All duplicated templates will start with an `UNKNOWN` rating. This rating will remain for the first 24 hours, after which a new rating will be generated if sufficient data is available.


## Billing

Messages delivered before migration is complete are charged to the old Solution Partner. Undelivered messages sent before migration is complete will be charged to the old Solution Partner if they are delivered after migration is complete.

Messages delivered after migration is complete are charged to the business customer.


## Tech Provider steps

### Step 1: Tag the customer&#039;s WABA for migration

Use the [POST /&lt;WHATSAPP_BUSINESS_ACCOUNT&gt;/set_solution_migration_intent](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/set-solution-migration-intent-api#Creating) endpoint to tag the business customer&#039;s WABA for migration. This generates a [migration intent](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/migration-intent-api), which indicates your intent to migrate the WABA.


#### Request

```html
curl &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;WABA_ID&gt;/set_solution_migration_intent&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer &lt;BUSINESS_TOKEN&gt;&#039; \
-d &#039;
&#123;
  &quot;solution_id&quot;: &quot;&lt;DESTINATION_MULTI-PARTNER_SOLUTION_ID&gt;&quot;
&#125;&#039;
```


#### Response

Upon success:

```html
&#123;
  &quot;id&quot;: &quot;&lt;MIGRATION_INTENT_ID&gt;&quot;
&#125;
```


Capture the migration intent ID.

### Step 2: Send the migration intent ID and phone number to be migrated to your Solution Partner

Send the migration intent ID you just captured to the Solution Partner of your destination solution. They will need this ID to determine if the customer has accepted the migration request.

In addition to this, share the phone number on the old WABA that you want migrated to the new WABA. This will be used in step 5 of the solution partner steps below.

### Step 3: Disable two-step verification on the business phone number

If you have access to the client&#039;s WABA in WhatsApp Manager, disable two-step verification on the business phone number associated with their WABA.

Alternatively, instruct the client to do this on their own. You can provide them with these instructions:

1. _Access WhatsApp Manager at [https://business.facebook.com/latest/whatsapp_manager/](https://business.facebook.com/latest/whatsapp_manager/)._
1. _Navigate to **Account tools** &gt; **Phone numbers**, and click the phone number&#039;s settings (gear) icon. If you don&#039;t see your business phone number, click **Overview** in the menu on the left, then locate the number and click it._
1. _Click the **Two-step verification** tab._
1. _Click the **Turn off two-step verification** button and complete the flow._


### Step 4: Instruct the customer to accept the request

Instruct the customer to use the Meta Business Suite to review and accept the request. You can provide them with these instructions:

1. _Access Meta Business Suite&#039;s **Business settings** panel at [https://business.facebook.com/settings/](https://business.facebook.com/settings/)._
1. _Navigate to **Requests** &gt; **Received**._
1. _Locate the request and click the **Review** button._
1. _Complete the flow._

When the customer completes this step, an [account_update webhook](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/manage-webhooks#onboarded-business-customer) indicating that a customer has been onboarded will be triggered and sent to both you and the destination solution&#039;s Solution Partner.

Note that an email from **WhatsApp for Business** (notification&#064;facebookmail.com) will also be sent to anyone who has admin access on the WABA, requesting review and acceptance of the request. The button in the email just loads the Business settings panel in a new window, so any WABA admin can review and accept the request.

## Solution Partner steps

### Step 1: Get the customer&#039;s business token

Use the [GET /&lt;SOLUTION_ID&gt;/access_token](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-solution/access-token-api#Reading) endpoint and request the `business_id` parameter to get an onboarded business customer&#039;s [business token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#business-integration-system-user-access-tokens).


#### Request

```html
curl &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;SOLUTION_ID&gt;/access_token?business_id=&lt;CUSTOMER_BUSINESS_PORTFOLIO_ID&gt;&#039; \
-H &#039;Authorization: Bearer &lt;SYSTEM_TOKEN&gt;&#039;
```


The customer&#039;s business portfolio ID is included in the [account_update webhook](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/manage-webhooks#onboarded-business-customer) triggered when the customer accepts the migration intent.

#### Response

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


### Step 2: Get the status of the migration intent

Use the [Migration Intent API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account-migration-intent/migration-intent-details-api#get-version-migration-intent-id) to get the status of the migration intent that the Tech Provider created. Your Tech Provider should provide you with this ID.

#### Request

```html
curl &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;MIGRATION_INTENT_ID&gt;&#039; \
-H &#039;Authorization: Bearer &lt;BUSINESS_TOKEN&gt;&#039;
```

#### Response

Upon success:

```html
&#123;
  &quot;solution&quot;: &#123;
    &quot;name&quot;: &quot;&lt;DESTINATION_SOLUTION_NAME&gt;&quot;,
    &quot;status&quot;: &quot;&lt;DESTINATION_SOLUTION_STATUS&gt;&quot;,
    &quot;status_for_pending_request&quot;: &quot;&lt;DESTINATION_SOLUTION_PENDING_REQUEST_STATUS&gt;&quot;,
    &quot;id&quot;: &quot;&lt;DESTINATION_SOLUTION_ID&gt;&quot;
  &#125;,
  &quot;status&quot;: &quot;&lt;MIGRATION_INTENT_STATUS&gt;&quot;,
  &quot;destination_waba&quot;: &#123;
    &quot;id&quot;: &quot;&lt;BUSINESS_CUSTOMER_WABA_ID&gt;&quot;,
    &quot;name&quot;: &quot;&lt;BUSINESS_CUSTOMER_WABA_NAME&gt;&quot;,
    &quot;currency&quot;: &quot;&lt;BUSINESS_CUSTOMER_WABA_CURRENCY&gt;&quot;,
    &quot;timezone_id&quot;: &quot;&lt;BUSINESS_CUSTOMER_WABA_TIMEZONE&gt;&quot;,
    &quot;business_type&quot;: &quot;ent&quot;,
    &quot;message_template_namespace&quot;: &quot;&lt;BUSINESS_CUSTOMER_WABA_TEMPLATE_NAMESPACE&gt;&quot;
  &#125;,
  &quot;id&quot;: &quot;&lt;MIGRATION_INTENT_ID&gt;&quot;
&#125;
```

If `&lt;MIGRATION_INTENT_STATUS&gt;` is `ACCEPTED`, it means the customer has reviewed and accepted the migration intent and you can proceed to the next step. **If it&#039;s any other value, do not proceed.**

Note that when a customer accepts the migration intent in the Meta Business Suite, an [account_update webhook](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/manage-webhooks#onboarded-business-customer) is triggered with `status` set to `PARTNER_ADDED`. Although the webhook won&#039;t tell if the customer onboarded via the Meta Business Suite, perform this query when you receive one of these webhooks.

### Step 3: Subscribe to webhooks on the customer&#039;s WABA

Use the [POST /&lt;WABA_ID&gt;/subscribed_apps](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/subscribed-apps-api#post-version-waba-id-subscribed-apps) endpoint to subscribe your app to webhooks on the business customer&#039;s WABA. If you want the customer&#039;s webhooks to be sent to a different callback URL than the one set on your app, you have multiple [webhook override](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/override) options.


Note that `&lt;WABA_ID&gt;` should be the customer&#039;s **destination** WABA ID.

#### Request

```html
curl -X POST &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;WABA_ID&gt;/subscribed_apps&#039; \
-H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039;
```


#### Response

Upon success:

```json
&#123;
  &quot;success&quot;: true
&#125;
```


### Step 4: Share your credit line with the customer

**Warning:** We are currently testing new steps for sharing your credit line with onboarded clients. These steps will eventually replace this step, so if you wish to implement these steps now, see [Alternate method for sharing your credit line](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/share-and-revoke-credit-lines#alternate-method-for-sharing-your-credit-line).

Use the [Credit Sharing API](https://developers.facebook.com/docs/marketing-api/reference/extended-credit/whatsapp_credit_sharing_and_attach) to share your credit line with an onboarded client.

#### Request

```html
curl -X POST &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;EXTENDED_CREDIT_LINE_ID&gt;/whatsapp_credit_sharing_and_attach?waba_currency=&lt;CUSTOMER_BUSINESS_CURRENCY&gt;&amp;waba_id=&lt;CUSTOMER_WABA_ID&gt;&#039; \
-H &#039;Authorization: Bearer &lt;SYSTEM_TOKEN&gt;&#039;
```

Note that `&lt;CUSTOMER_WABA_ID&gt;` should be the customer&#039;s **destination** WABA ID.

#### Response

Upon success:

```html
&#123;
  &quot;allocation_config_id&quot;: &quot;&lt;ALLOCATION_CONFIGURATION_ID&gt;&quot;,
  &quot;waba_id&quot;: &quot;&lt;CUSTOMER_WABA_ID&gt;&quot;
&#125;
```

### Step 5: Migrate the customer&#039;s phone number to their new WABA

Use the [Phone Numbers API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/phone-number-management-api#post-version-waba-id-phone-numbers) to migrate the customer&#039;s business phone number to the destination WABA. You should have received this from the Tech Provider in Step 2 of the Tech Provider steps.

Note that `&lt;WABA_ID&gt;` should be the customer&#039;s **destination** WABA ID.

#### Request

```html
curl &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;WABA_ID&gt;/phone_numbers&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer &lt;BUSINESS_TOKEN&gt;&#039; \
-d &#039;
&#123;
  &quot;migrate_phone_number&quot;: true,
  &quot;cc&quot;: &quot;&lt;BUSINESS_PHONE_NUMBER_COUNTRY_CALLING_CODE&gt;&quot;,
  &quot;phone_number&quot;: &quot;&lt;BUSINESS_PHONE_NUMBER_WITHOUT_COUNTRY_CODE&gt;&quot;
&#125;&#039;
```

#### Response

Upon success:

```html
&#123;
  &quot;id&quot;: &quot;&lt;BUSINESS_PHONE_NUMBER_ID&gt;&quot;
&#125;
```

### Step 6: Register the customer&#039;s phone number for use with Cloud API

Use the [Register API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/register-api#post-version-phone-number-id-register) to register the customer&#039;s business phone number for use with Cloud API.

#### Request

```html
curl &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;BUSINESS_PHONE_NUMBER_ID&gt;/register&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer &lt;BUSINESS_TOKEN&gt;&#039; \
-d &#039;
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;pin&quot;: &quot;&lt;DESIRED_PIN&gt;&quot;
&#125;&#039;
```

#### Response

Upon success:

```html
&#123;
  &quot;success&quot;: true
&#125;
```
