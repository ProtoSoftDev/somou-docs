# Migrating customers off a Multi-Partner Solution using Meta Business Suite



If you are a Tech Provider, you can migrate a client off a [Multi-Partner Solution](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/multi-partner-solutions) by tagging their WhatsApp Business account (&quot;WABA&quot;) for migration and instructing them to use [Meta Business Suite](https://business.facebook.com/) to review and accept the request. Once migrated, you can provide messaging services to the client independently.

Migrating a customer off a solution via Meta Business Suite does not require business phone number reverification, so it eliminates any downtime caused by reverification.

## Requirements

Your app must already be approved for advanced access for the **whatsapp_business_management** permission.

## Templates

Templates are automatically duplicated in the destination WABA and initially granted the same status as their source counterparts.

After duplication however, templates are re-checked to ensure they are correctly categorized according to our [guidelines](https://developers.facebook.com/documentation/business-messaging/whatsapp/pricing). This may result in some duplicated templates having their `status` set to `REJECTED`.

Only templates with both a `status` of `APPROVED` and `quality_score` of `GREEN` are eligible for duplication. If the destination WABA cannot accommodate all of the new templates, we will duplicate as many as we can until the destination WABA&#039;s template limit has been reached. Unduplicated templates must be re-created and submitted for approval if they are to be used by the destination WABA.

Note that **template quality ratings are not duplicated**. All duplicated templates will start with an `UNKNOWN` rating. This rating will remain for the first 24 hours, after which a new rating will be generated if sufficient data is available.


## Billing

Messages delivered before migration is complete are charged to the old Solution Partner. Undelivered messages sent before migration is complete will be charged to the old Solution Partner if they are delivered after migration is complete.

Messages delivered after migration is complete are charged to the business customer.


## Step 1: Disable two-step verification on the business phone number

If you have access to the client&#039;s WABA in WhatsApp Manager, disable two-step verification on the business phone number associated with their WABA.

Alternatively, you can instruct the client to do this on their own. You can provide them with these instructions:

1. _Access WhatsApp Manager at [https://business.facebook.com/latest/whatsapp_manager/](https://business.facebook.com/latest/whatsapp_manager/)._
1. _Navigate to **Account tools** &gt; **Phone numbers**, and click the phone number&#039;s settings (gear) icon. If you don&#039;t see your business phone number, click **Overview** in the menu on the left, then locate the number and click it._
1. _Click the **Two-step verification** tab._
1. _Click the **Turn off two-step verification** button and complete the flow._


## Step 2: Tag the customer&#039;s WABA for migration

Use the [POST /&lt;WHATSAPP_BUSINESS_ACCOUNT&gt;/set_solution_migration_intent](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/set-solution-migration-intent-api#Creating) endpoint to tag the business customer&#039;s WABA for migration. This generates a [migration intent](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/migration-intent-api), which indicates your intent to migrate the WABA.


#### Request

```html
curl &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;WABA_ID&gt;/set_solution_migration_intent&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer &lt;BUSINESS_TOKEN&gt;&#039; \
-d &#039;
&#123;
  &quot;app_id&quot;: &quot;&lt;YOUR_APP_ID&gt;&quot;
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

## Step 3: Instruct the customer to accept the request and add a payment method

Instruct the client to use the Meta Business Suite to review and accept the request. Until the client adds a payment method, they will be unable to use your app to send template messages to their own customers.

You can provide them with these instructions:

1. _Access Meta Business Suite&#039;s **Business settings** panel at [https://business.facebook.com/settings/](https://business.facebook.com/settings/)._
1. _Navigate to **Requests** &gt; **Received**._
1. _Locate the request and click the **Review** button._
1. _Complete the flow._

**WhatsApp for Business** (notification&#064;facebookmail.com) also sends an email to anyone who has admin access on the WABA, requesting review and acceptance of the request. The button in the email just loads the Business settings panel in a new window, so any WABA admin can review and accept the request.

## Step 4: Get migration status and WABA ID

Use the [Migration Intent API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account-migration-intent/migration-intent-details-api#get-version-migration-intent-id) to get the status of the migration intent as well as the client&#039;s new WABA ID.

#### Request

```html
curl &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;MIGRATION_INTENT_ID&gt;&#039; \
-H &#039;Authorization: Bearer &lt;SYSTEM_TOKEN&gt;&#039;
```

#### Response

Upon success:

```html
&#123;
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

Be sure to capture the client&#039;s destination WABA ID (`&lt;BUSINESS_CUSTOMER_WABA_ID&gt;`).

If `&lt;MIGRATION_INTENT_STATUS&gt;` is `ACCEPTED`, the client has reviewed and accepted the migration intent and you can proceed to the next step. **If the status is any other value, do not proceed.**

## Step 5: Get the client&#039;s business token

Use the [System User Access Tokens API](https://developers.facebook.com/documentation/facebook-login/facebook-login-for-business#business-integration-system-user-access-tokens) to get the client&#039;s business token.

## Step 6: Subscribe to webhooks on the customer&#039;s WABA

Use the [POST /&lt;WABA_ID&gt;/subscribed_apps](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/subscribed-apps-api#post-version-waba-id-subscribed-apps) endpoint to subscribe your app to webhooks on the business customer&#039;s WABA. If you want the customer&#039;s webhooks to be sent to a different callback URL than the one set on your app, you have multiple [webhook override](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/override) options.


Use the customer&#039;s **destination** WABA ID for `&lt;WABA_ID&gt;`.

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


## Step 7: Get the customer&#039;s connected phone number ID

Use the [Phone Numbers API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/phone-number-management-api#get-version-waba-id-phone-numbers) and request the `status` field to get a list of business phone numbers, and their Cloud API registration statuses, on the client&#039;s **source** WABA.

#### Request

```html
curl &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;WABA_ID&gt;/phone_numbers?fields=status&#039; \
-H &#039;Authorization: Bearer &lt;BUSINESS_TOKEN&gt;&#039;
```

#### Response

Upon success:

```html
&#123;
  &quot;data&quot;: [
    &#123;
      &quot;status&quot;: &quot;&lt;STATUS&gt;&quot;,
      &quot;id&quot;: &quot;&lt;BUSINESS_PHONE_NUMBER_ID&gt;&quot;
    &#125;
  ],
  &quot;paging&quot;: &#123;
    &quot;cursors&quot;: &#123;
      &quot;before&quot;: &quot;&lt;PAGING_BEFORE_CURSOR&gt;&quot;,
      &quot;after&quot;: &quot;&lt;PAGE_AFTER_CURSOR&gt;&quot;
    &#125;
  &#125;
&#125;
```

A `status` of `CONNECTED` indicates that the customer&#039;s business phone number is registered and in use with Cloud API.

## Step 8: Migrate the customer&#039;s phone number to their new WABA

Use the [Phone Numbers API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/phone-number-management-api#post-version-waba-id-phone-numbers) to migrate the customer&#039;s business phone number to their **destination** WABA.

#### Request

```html
curl &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;WABA_ID&gt;/phone_numbers&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer &lt;BUSINESS_TOKEN&gt;&#039; \
-d &#039;
&#123;
  &quot;migrate_phone_number&quot;: true,
  &quot;cc&quot;: &quot;&lt;BUSINESS_PHONE_NUMBER_COUNTRY_CALLING_CODE&gt;&quot;,
  &quot;phone_number&quot;: &quot;&lt;BUSINESS_PHONE_NUMBER&gt;&quot;,
  &quot;display_phone_number&quot;: &quot;&lt;BUSINESS_PHONE_NUMBER_DISPLAY_NUMBER&gt;&quot;,
  &quot;verified_name&quot;: &quot;&lt;BUSINESS_PHONE_NUMBER_VERIFIED_NAME&gt;&quot;
&#125;&#039;
```

* Set `&lt;BUSINESS_PHONE_NUMBER_COUNTRY_CALLING_CODE&gt;` to the business phone number&#039;s country calling code.
* Set `&lt;BUSINESS_PHONE_NUMBER&gt;` to the business phone number without a plus symbol or country calling code.
* Set `&lt;BUSINESS_PHONE_NUMBER_DISPLAY_NUMBER&gt;` to the business phone number, with, or without, a plus symbol and country calling code.
* Set `&lt;BUSINESS_PHONE_NUMBER_VERIFIED_NAME&gt;` to the business phone number&#039;s existing display name.

#### Response

Upon success, the API returns a business phone number ID. Capture this ID for use in the next step.

```html
&#123;
  &quot;id&quot;: &quot;&lt;BUSINESS_PHONE_NUMBER_ID&gt;&quot;
&#125;
```

## Step 9: Register the customer&#039;s number

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
