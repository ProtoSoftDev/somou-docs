# Adding a WABA to a Multi-Partner Solution



If you are a Solution Partner and are part of an active [multi-partner solution](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/multi-partner-solutions), you can designate a WABA as _eligible_ for the solution (the &quot;destination solution&quot;). This sends a Meta Business Suite request to the client who owns the WABA. The client can then use the Meta Business Suite to accept and confirm the request.

Confirmation associates the WABA with the destination solution, thereby granting permissions (already defined on the destination solution) to any Tech Providers who are part of it.

If you&#039;re unsure of the WABA&#039;s ownership model, request the `ownership_type` field on the WABA ID. A value of `ON_BEHALF_OF` indicates you own the WABA, while `CLIENT_OWNED` indicates that your client owns the WABA.

## Requirements

* The WABA must have been onboarded by you.
* The WABA cannot already be part of an existing active solution.
* The destination solution must be in an active state.

## Step 1: Designate the WABA as solution eligible

Use the [POST /&lt;WHATSAPP_BUSINESS_ACCOUNT&gt;/set_solution_migration_intent](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/set-solution-migration-intent-api#Creating) endpoint to tag the business customer&#039;s WABA for migration. This generates a [migration intent](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/migration-intent-api), which indicates your intent to migrate the WABA.


### Request

```html
curl &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;WABA_ID&gt;/set_solution_migration_intent&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer &lt;BUSINESS_TOKEN&gt;&#039; \
-d &#039;
&#123;
  &quot;solution_id&quot;: &quot;&lt;DESTINATION_MULTI-PARTNER_SOLUTION_ID&gt;&quot;
&#125;&#039;
```


### Response

Upon success:

```html
&#123;
  &quot;id&quot;: &quot;&lt;MIGRATION_INTENT_ID&gt;&quot;
&#125;
```


In addition, Meta sends a confirmation request to the client who owns the WABA.

## Step 2: Instruct the client to confirm

Instruct your client to use the Meta Business Suite to accept and confirm the solution partner access request.

You can send the client the following instructions:

1. _Go to [https://business.facebook.com/settings/requests/](https://business.facebook.com/settings/requests/) and log into your account._
1. _If you have multiple business portfolios, you will be presented with all of them. Click the portfolio that contains your WABA._
1. _In the Received tab, locate the request and complete the flow._

Note that if your client does not complete this step within 90 days, acceptance and confirmation will happen automatically.
