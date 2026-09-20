# Migrating customers off of a Multi-Partner Solution using Embedded Signup



If you are a Tech Provider, you can migrate a client off of a [Multi-Partner Solution](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/multi-partner-solutions) by tagging their WhatsApp Business account (&quot;WABA&quot;) for migration and instructing them to use [your implementation of Embedded Signup](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/implementation) to review and accept the request. Once migrated, you can provide messaging services to the client independently.

Migrating a customer off of a solution via Embedded Signup does not require business phone number reverification. Skipping reverification eliminates downtime.

Note that as part of the process, your client may choose to create a new WABA. If they do, templates from their old (&quot;source&quot;) WABA will be duplicated in their new destination WABA. See [Templates](#templates) below for an explanation of template duplication behavior.

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


### Request

```html
curl &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;WABA_ID&gt;/set_solution_migration_intent&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer &lt;BUSINESS_TOKEN&gt;&#039; \
-d &#039;
&#123;
  &quot;app_id&quot;: &quot;&lt;YOUR_APP_ID&gt;&quot;
&#125;&#039;
```


### Response

Upon success:

```html
&#123;
  &quot;id&quot;: &quot;&lt;MIGRATION_INTENT_ID&gt;&quot;
&#125;
```


Capture the migration intent ID.

## Step 3: Instruct the customer to complete Embedded Signup and add a payment method

Send the client a link to your implementation of Embedded Signup and instruct them to complete the flow in order to accept the request and add a payment method.

You can provide the client with these instructions:

1. _In the [business portfolio screen](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/default-flow#business-portfolio-screen), enter your existing business portfolio name._
1. _In the [WABA selection screen](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/default-flow#business-asset-selection-screen), use the **Choose a WhatsApp Business Account** dropdown menu to create a new WhatsApp Business account (WABA), or choose an existing one._
1. _In the same screen, use the **Create or Select a WhatsApp Business Profile** dropdown menu to enter or select your existing business phone number&#039;s display name._
1. _In the [phone number addition](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/default-flow#phone-number-addition-screen) screen, enter your existing business phone number. This should trigger a warning that the number is going to be shared with your Tech Provider._

Until the client adds a payment method, they will be unable to use your app to send template messages to their own customers, so instruct the client to add a payment method after completing the flow.

You can send them the [Add a credit card to your WhatsApp Business Platform account](https://www.facebook.com/business/help/488291839463771) Help Center article, which explains how to add a payment method.

## Step 4: Exchange the token code for a business token

Use the **GET /oauth/access_token** endpoint to exchange the token code [returned](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/implementation#session-logging-message-event-listener) by Embedded Signup for a business integration system user access token (&quot;business token&quot;).


### Request

```html
curl --get &#039;https://graph.facebook.com/v21.0/oauth/access_token&#039; \
-d &#039;client_id=&lt;APP_ID&gt;&#039; \
-d &#039;client_secret=&lt;APP_SECRET&gt;&#039; \
-d &#039;code=&lt;CODE&gt;&#039;
```


### Response

Upon success:

```html
&lt;BUSINESS_TOKEN&gt;
```


## Step 5: Subscribe to webhooks on the customer&#039;s WABA

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


## Step 6: Register the customer&#039;s number

Use the [Register API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/register-api#post-version-phone-number-id-register) to register the customer&#039;s business phone number for use with Cloud API.

### Request

```bash
curl &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;BUSINESS_PHONE_NUMBER_ID&gt;/register&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer &lt;BUSINESS_TOKEN&gt;&#039; \
-d &#039;
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;pin&quot;: &quot;&lt;DESIRED_PIN&gt;&quot;
&#125;&#039;
```

### Response

Upon success:

```json
&#123;
  &quot;success&quot;: true
&#125;
```
