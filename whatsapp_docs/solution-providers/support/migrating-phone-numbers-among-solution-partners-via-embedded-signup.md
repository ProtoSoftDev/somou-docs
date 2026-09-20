# Migrating a business phone number from one Solution Partner to another via Embedded Signup



This document describes how to use Embedded Signup to migrate business phone numbers from one Solution Partner and WhatsApp Business account (WABA) to another Solution Partner and WABA.

Clients can migrate their business phone numbers between WhatsApp Business Accounts (WABAs) and retain their display names, quality ratings, template messaging limits, Official Business Account statuses, and approved, high-quality templates. Migration typically is only performed when a client wants to move their business phone number from one Solution Partner to another.

To perform a migration for a client, you have two options: migration via Embedded Signup, or programmatic migration.

**Migration via Embedded Signup is simpler and is the preferred solution** because it can be initiated by your clients, automatically generates and grants ownership of all necessary assets, grants your app access to those assets, and requires fewer API calls.

Programmatic migration must be initiated by you and involves more API calls, as you must verify that dependent assets are configured correctly, must generate all new required assets on your own, and must associate them with other assets. For this reason, programmatic migration is only recommended if you will be working with the client using the On-Behalf-Of model (that is, you will create and own the destination WABA and its assets and share them with the client).

If you would like to migrate client phone numbers programmatically, see our [Migrating Numbers Between WhatsApp Business Accounts Programmatically](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/support/migrating-phone-numbers-among-solution-partners-programmatically) document.

## How it works

Clients can use your implementation of Embedded Signup to start the migration process. Embedded Signup will prompt clients for their business phone number and a new destination WhatsApp Business account (WABA).

When the client completes the flow, Embedded Signup generates their new WABA, associates it with their Meta business portfolio, grants your app access to the WABA, then returns the newly created WABA ID and the business phone number ID.

You must capture these IDs and use them with the API to share your credit line, subscribe to webhooks, and register the number for use with Cloud API. Once you complete the final step (registration), the business phone number is re-associated with the destination WABA and the number can then be used to send and receive messages again.

Since the client&#039;s business phone number is not changing, its display name, quality rating, messaging limit, and Official Business Account status are all preserved.

In addition, all eligible templates are automatically duplicated in the destination WABA and granted the same statuses as their source counterparts, and all media uploaded on the client&#039;s business phone number can continue to be used.

### WhatsApp Business accounts

Embedded Signup automatically generates the client&#039;s new WABA, associates it with their Meta Business Account, and grants your app access to the WABA.

### Templates

Templates are automatically duplicated in the destination WABA and initially granted the same status as their source counterparts.

After duplication however, templates are re-checked to ensure they are correctly categorized according to our [guidelines](https://developers.facebook.com/documentation/business-messaging/whatsapp/pricing). This may result in some duplicated templates having their `status` set to `REJECTED`.

Only templates with both a `status` of `APPROVED` and `quality_score` of `GREEN` are eligible for duplication. If the destination WABA cannot accommodate all of the new templates, we will duplicate as many as we can until the destination WABA&#039;s template limit has been reached. Unduplicated templates must be re-created and submitted for approval if they are to be used by the destination WABA.

Note that **template quality ratings are not duplicated**. All duplicated templates will start with an `UNKNOWN` rating. This rating will remain for the first 24 hours, after which a new rating will be generated if sufficient data is available.


### Billing

Messages delivered before migration is complete are charged to the old Solution Partner. Undelivered messages sent before migration is complete will be charged to the old Solution Partner if they are delivered after migration is complete.

Messages delivered after migration is complete are charged to the business customer.


### Template downtime

Business phone number registration happens instantly, therefore you can continue to send and receive messages without interruption.

However, template duplication takes time, therefore you will not be able to use affected templates until they are migrated.

To avoid this downtime, you can begin template migration first, before phone number registration.

### Rate limits

[Template duplication](#templates) triggered automatically as part of the migration process does not count against your rate limit. API calls that you perform, however, do count against the limit.

## Limitations

* Test business phone numbers issued by WhatsApp cannot be migrated.
* Migrated business phone numbers can only be registered for use with Cloud API.
* Business phone numbers must have an approved display name (`name_status` is `APPROVED`).
* Business phone numbers cannot have any associated pending display name change requests.
* Quality ratings of templates will **NOT** be migrated. All migrated templates will start with an `UNKNOWN` rating. This rating will remain for the **first 24 hours**, after which a new rating will be generated if sufficient data is available.

## Requirements

### Clients

Ask the client who owns the business phone number to confirm that they meet the following requirements. They can do this using the Meta Business Suite, if they own their WABA, by going to [WhatsApp Accounts](https://business.facebook.com/settings/whatsapp-business-accounts/) &gt; (their WABA name) &gt; **Settings**. If they do not own their WABA, they must ask their Solution Partner to confirm.

* Their Meta Business Account must have a **verified** status.
* Their existing WABA must have a status of **approved**.
* Their existing WABA must have a valid payment method attached (in **Payment Settings**).
* Their business phone number must have two-step verification disabled. Clients who own their WABA can use the WhatsApp Manager to [disable two-step verification](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-phone-numbers/phone-numbers#disabling-two-step-verification) on their number, otherwise they must ask their Solution Partner to disable it for them.

### Solution Partners

At least one app must already be subscribed to webhooks on the destination WABA (see [Webhooks](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/manage-webhooks#subscribe-to-a-whatsapp-business-account)), and you must be using Embedded Signup with [session logging](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/implementation#session-logging-message-event-listener) enabled.

## Migration steps

### Step 1: Instruct client to disable two-step verification

If you haven&#039;t already done so, instruct your client to use the WhatsApp Manager to [disable two-step verification](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-phone-numbers/phone-numbers#disabling-two-step-verification) on their business phone number (or to ask their current Solution Partner to disable it for them).

**You cannot complete the remaining steps until two-step verification is disabled.**

### Step 2: Surface Embedded Signup

Direct the client to access your implementation of Embedded Signup and to supply their business phone number and its display name when prompted for them in the flow.

### Step 3: Capture asset IDs

When the client completes the flow, capture the business phone number ID and new WABA [returned in the message event](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/implementation#session-logging-message-event-listener).

### Step 4: Share your credit line

[Share your credit line with the WABA](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/share-and-revoke-credit-lines#sharing-your-credit-line) just like you normally would after onboarding a client via Embedded Signup.

### Step 5: Subscribe webhooks

[Subscribe your app to webhooks](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/manage-webhooks#subscribe-to-a-whatsapp-business-account) on the client&#039;s new WABA.

### Step 6: Register the phone number for Cloud API

[Register the business phone number](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/registering-phone-numbers#step-4--register-the-number) for use with Cloud API.

## Troubleshooting

If the template migration process fails, please refer to our [Template migration](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/template-migration) document for instructions on how to manually trigger template migration.
