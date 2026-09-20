# Migrating a WABA from one Multi-Partner Solution to another via Embedded Signup



If you are a Tech Provider, you can use the API and [Embedded Signup](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/overview) to migrate a client&#039;s WABA from one Multi-Partner Solution (the &quot;source solution&quot;) to another (the &quot;destination solution&quot;).

As part of this process, a new WhatsApp Business account (WABA) will be created for the client, templates within the source WABA will be duplicated in the destination WABA, and access to the WABA and its assets will be granted to the destination solution&#039;s Solution Partner.

## Requirements

Your app (or apps, if you are using separate apps) used to create or accept the source and destination Multi-Partner Solutions must be associated with the same business portfolio.
The destination solution must also be in an Active state.

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

### Step 2: Disable two-step verification on the business phone number

If you have access to the client&#039;s WABA in WhatsApp Manager, disable two-step verification on the business phone number associated with their WABA.

Alternatively, you can instruct the client to do this on their own. You can provide them with these instructions:

1. _Access WhatsApp Manager at [https://business.facebook.com/latest/whatsapp_manager/](https://business.facebook.com/latest/whatsapp_manager/)._
1. _Navigate to **Account tools** &gt; **Phone numbers**, and click the phone number&#039;s settings (gear) icon. If you don&#039;t see your business phone number, click **Overview** in the menu on the left, then locate the number and click it._
1. _Click the **Two-step verification** tab._
1. _Click the **Turn off two-step verification** button and complete the flow._


### Step 3: Instruct the customer to complete Embedded Signup

Instruct the customer to complete the Solution Partner&#039;s implementation of Embedded Signup.

Make sure that you are directing the customer to the Embedded Signup implementation correctly configured with the destination Multi-Partner Solution ID, otherwise the customer could be onboarded via the wrong solution.

You can provide the customer with these instructions:

1. _Enter your existing business portfolio name in the [business portfolio screen](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/default-flow#business-portfolio-screen)._
1. _Create a new WhatsApp Business account (WABA) in the [WABA selection screen](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/default-flow#business-asset-selection-screen)._
1. _Enter your existing business phone number in the [phone number addition screen](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/default-flow#phone-number-addition-screen). This will trigger a warning that the number will be moved to a new WhatsApp Business account._

## Solution Partner steps

Provide the Tech Provider with a link to your implementation of Embedded Signup configured with the Multi-Partner Solution ID. Whenever a client completes your implementation&#039;s flow successfully, [onboard the client](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/onboarding-customers-as-a-solution-partner) as you normally would.
