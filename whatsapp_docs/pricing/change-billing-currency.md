# Change billing currency via API


The Currency Migration API allows you to change the billing currency or payment method on a WhatsApp Business account (WABA) by creating a new WABA and automatically migrating your phone numbers, message templates, and Flows.

Use this API when you need to:

- **Change currency**: Clone an existing WABA into a new WABA with a different currency while migrating assets (phone numbers, templates, Flows). This can make it easier for you to opt-in to be charged in a [newly-available currency](https://developers.facebook.com/documentation/business-messaging/whatsapp/pricing#updates-to-rate-cards), like Mexican pesos (available as of January 1, 2026).
- **Change payment method**: Move a WABA from one payment method to another while preserving assets.

## Before you begin

- The second call moves phone numbers and deprecates the old WABA. Plan for any operational timing requirements.
- Coexistence and Authorized Agents WABAs are not eligible for migration via this API.
- For payment method changes, only credit card to Line of Credit and Line of Credit to Line of Credit migrations are supported. Migrating from Line of Credit to credit card or credit card to credit card is not supported.
- Lines of Credit are always denominated in USD, regardless of your WABA&#039;s billing currency. You do not need a Line of Credit in the target currency. If your migration requires Line of Credit billing, attach your existing USD Line of Credit.
- You are billed in your WABA&#039;s currency. Your WABA&#039;s currency and your legal entity&#039;s headquarters address together determine which Meta billing entity issues your invoices.
- Phone number limits are preserved on the new WABA (for example, expanded limits of up to 1,200 numbers carry over).

| API Call | What is migrated | What is not migrated | Result | Can WABA be used to send messages? |
| --- | --- | --- | --- | --- |
| First call (Initiate) | - Templates (all statuses: Approved, Pending, Disabled, Rejected, Under Review)&lt;br&gt;- Flows&lt;br&gt;- App installation&lt;br&gt;- Users/permissions | - Templates with outdated formats&lt;br&gt;- Phone numbers in &quot;manual review&quot; state&lt;br&gt;- Insights/analytics | New WABA in selected currency, with assets migrated | Not yet – No phone number attached |
| Second call (Complete) | Phone numbers | Historical insights (remain on old WABA) | Phone number migrated to this new WABA | Yes |

| Insights | Before Migration | After Migration |
| --- | --- | --- |
| Phone Insights | Available on old WABA | Historical data remains on old WABA. New data appears on the new WABA after migration. |
| Template Insights | Available on old WABA | Historical data remains on old WABA. New template analytics appear on new WABA. |
| Message Insights (send, delivery) | Available on old WABA | Historical data remains on old WABA. New message data appears on the new WABA. |
| Pricing Insights (cost, volume) | Available on old WABA | Historical cost and volume data remains on old WABA. New billing data appears on the new WABA in the new currency. |
| Call Insights (cost, duration, count) | Available on old WABA | Historical call data remains on old WABA. New call data appears on the new WABA. |

### Template migration behavior

During migration, note the following behavior:

- New templates created on the old WABA after the phone migration will not be cloned to the new WABA.
- Template status and category are locked during the migration process. Once migration is complete, both can change again.

### Prerequisites

Before initiating a migration, ensure the following:

- You have admin access to the source WABA.
- You have a valid access token with `whatsapp_business_management` and `whatsapp_business_messaging` permissions. No additional Business Portfolio Admin permissions or business approvals are required.
- If your migration requires Line of Credit billing, have your Line of Credit (also called a CreditLine) identifier ready to provide during the [Complete migration](#complete-migration) step. Lines of Credit are always denominated in USD, so you do not need one in the target currency.

## Initiate migration

Use the POST /&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;/`set_payment_method_migration_intent` endpoint to initiate the migration process. This validates the source WABA, creates a cloned WABA, and begins migrating assets such as templates and Flows.

**Request syntax**

```html
curl -X POST &quot;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;/set_payment_method_migration_intent&quot; \
-H &quot;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&quot; \
-H &quot;Content-Type: application/json&quot; \
-d &#039;
&#123;
  &quot;currency&quot;: &quot;&lt;CURRENCY&gt;&quot;,

  &lt;!-- Optional --&gt;
  &quot;extended_credit_id&quot;: &quot;&lt;EXTENDED_CREDIT_ID&gt;&quot;
&#125;&#039;
```

**Response syntax**

```json
&#123;
  &quot;migration_id&quot;: &quot;&lt;MIGRATION_ID&gt;&quot;,
  &quot;migration_status&quot;: &quot;&lt;MIGRATION_STATUS&gt;&quot;
&#125;
```

### Request parameters

| Placeholder | Description | Example Value |
| --- | --- | --- |
| `&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;` | The source WhatsApp Business account (WABA) ID you are initiating migration from. Used in the endpoint path. | `123456789012345` |
| `&lt;ACCESS_TOKEN&gt;` | Access token with permissions to manage the WABA and perform migration actions. Used in the Authorization header. | `EAABsbCS1iHgBA...` |
| `&lt;CURRENCY&gt;` | **Required.**&lt;br&gt;&lt;br&gt;Target currency for the cloned (new) WABA. | `INR` |
| `&lt;EXTENDED_CREDIT_ID&gt;` | **Optional.**&lt;br&gt;&lt;br&gt;Line of Credit identifier. Required only if migrating to Line of Credit billing. If the WABA uses credit card billing, omit this parameter — migration works regardless of the current payment method. | `987654321098765` |

### Example request

```curl
curl &#039;https://graph.facebook.com/v23.0/123456789012345/set_payment_method_migration_intent&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer EAABsbCS1iHgBA...&#039; \
-d &#039;
&#123;
  &quot;currency&quot;: &quot;INR&quot;,
  &quot;extended_credit_id&quot;: &quot;987654321098765&quot;
&#125;&#039;
```

### Example response

```json
&#123;
  &quot;migration_id&quot;: &quot;mig_01HZYK3ABCDEF4567890&quot;,
  &quot;migration_status&quot;: &quot;INITIATED&quot;
&#125;
```

## Check migration status

Use the GET /&lt;MIGRATION_ID&gt; endpoint to check the current status of a migration. Poll this endpoint to monitor progress until the status reaches `READY_TO_COMPLETE` or `COMPLETED`.

### Request syntax

```html
curl &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;MIGRATION_ID&gt;&#039; \
-H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039;
```

### Request parameters

| Placeholder | Description | Example Value |
| --- | --- | --- |
| `&lt;MIGRATION_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;The `migration_id` returned by the initiate migration endpoint. | `mig_01HZYK3ABCDEF4567890` |

### Example request

```curl
curl &#039;https://graph.facebook.com/v23.0/mig_01HZYK3ABCDEF4567890&#039; \
-H &#039;Authorization: Bearer EAABsbCS1iHgBA...&#039;
```

### Example response

```json
&#123;
  &quot;status&quot;: &quot;READY_TO_COMPLETE&quot;,
  &quot;destination_waba&quot;: &#123;
    &quot;id&quot;: &quot;998877665544332&quot;,
    &quot;name&quot;: &quot;Acme WABA (INR)&quot;,
    &quot;currency&quot;: &quot;INR&quot;,
    &quot;timezone_id&quot;: &quot;1&quot;,
    &quot;message_template_namespace&quot;: &quot;1234abcd_namespace&quot;
  &#125;,
  &quot;id&quot;: &quot;mig_01HZYK3ABCDEF4567890&quot;
&#125;
```

## Pre-completion verification

Before completing the migration, verify the following on the new (destination) WABA:

- **Templates**: Confirm that your templates have been cloned to the new WABA. Templates with outdated formats may not migrate. Note that template IDs on the new WABA will differ from the original WABA.
- **App installation**: Verify that your app is installed on the new WABA.
- **Users**: Confirm that users and permissions have been synced correctly to the new WABA.

Once you have verified these items, proceed to [Complete migration](#complete-migration).

## Complete migration

Once the migration status reaches `READY_TO_COMPLETE`, call the resume migration endpoint as the final step. This detaches phone numbers from the old WABA and attaches them to the new WABA.

### Prerequisites

Before completing the migration:

- Verify that the migration status is `READY_TO_COMPLETE` by [checking the migration status](#check-migration-status).
- The new currency in the input is not the same as the existing currency on the WABA.
- If your migration requires Line of Credit billing, have your `CreditLine` identifier ready to attach.

After this step completes:

- The new WABA becomes the active account with the migrated phone numbers.
- The old WABA can no longer send messages.
- Continue referencing the old WABA for historical insights and analytics.

**Request syntax**

```html
curl -X POST &quot;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;MIGRATION_ID&gt;/resume_migration&quot; \
-H &quot;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&quot;
```

**Response example**

```json
&#123;
  &quot;status&quot;: &quot;&lt;STATUS&gt;&quot;,
  &quot;destination_waba&quot;: &#123;
    &quot;id&quot;: &quot;&lt;CLONED_WABA_ID&gt;&quot;,
    &quot;name&quot;: &quot;&lt;CLONED_WABA_NAME&gt;&quot;,
    &quot;currency&quot;: &quot;&lt;NEW_CURRENCY&gt;&quot;,
    &quot;timezone_id&quot;: &quot;&lt;CLONED_WABA_TIMEZONE&gt;&quot;,
    &quot;message_template_namespace&quot;: &quot;&lt;NAMESPACE_FOR_TEMPLATES&gt;&quot;
  &#125;,
  &quot;id&quot;: &quot;&lt;MIGRATION_ID&gt;&quot;
&#125;
```

## Verify migration

After the migration status is `COMPLETED`, run these checks to confirm the new WABA is operational.

### WABA cloning and asset migration

- In Meta Business Suite, under WhatsApp Accounts, verify you see two WABAs: the original (original currency) and the new WABA (new currency).
- In the new WABA, verify that templates have been migrated.
- Confirm all phone numbers that were on the original WABA are now attached to the new WABA.

### Messaging validation

- Send a test message from the new WABA using one of the migrated phone numbers.
- Confirm the message is delivered successfully and that message and cost insights appear under the new WABA.

## Insights and analytics after migration

Insights and analytics do not migrate to the new WABA. The original WABA is not deleted — it remains visible in all linked Business Managers for historical reference. After migration:

- **Historical data**: Continue querying the old WABA ID for all historical insights, analytics, and billing data generated before the migration.
- **New data**: All new messaging insights, analytics, and billing data will appear under the new WABA ID after migration is complete.

## Original WABA after migration

The original WABA is not deleted after migration. It remains visible in all linked Business Managers and accessible for historical reference. There is no plan to automatically delete original WABAs.

- The original WABA can no longer send messages after the migration is complete.
- All historical insights, analytics, and billing data remain on the original WABA.
- The new WABA becomes the active account with migrated phone numbers and assets.

## Migration status values

The following status values are returned by the migration API. For error statuses, see the recommended actions in the [Troubleshooting](#troubleshooting) section below.

| Status | Description |
| --- | --- |
| `INITIATED` | Migration has been initiated. The system is validating the source WABA and beginning asset cloning. |
| `ACCEPTED` | Migration request has been accepted and is queued for processing. |
| `IN_PROGRESS` | Assets such as templates and Flows are being cloned to the new WABA. |
| `READY_TO_COMPLETE` | Asset cloning is finished. You can now call the resume migration endpoint to complete the migration. |
| `COMPLETED` | Migration is complete. Phone numbers have been moved to the new WABA. |
| `FAILED` | Migration failed during asset cloning. |
| `FAILED_TO_COMPLETE` | Migration failed during the final completion step (phone number move). |
| `REJECTED` | Migration request was rejected due to validation errors. |

## Troubleshooting

| Status | Issue | Possible reasons and solutions |
| --- | --- | --- |
| `REJECTED` | Migration request was rejected due to validation errors. | Verify that you have admin access to the source WABA and that your access token has the required permissions. Check that the source WABA is eligible for migration and retry. |
| `FAILED` | Migration failed during asset cloning (templates, Flows). | Initiate a new migration by calling the `set_payment_method_migration_intent` endpoint again. If the issue persists, contact support. |
| `FAILED_TO_COMPLETE` | Migration failed during the final completion step (phone number move). | Call the resume migration endpoint again to retry. If the issue persists, contact support. |
| `IN_PROGRESS` (extended) | Migration has been in progress longer than expected. | Continue polling the [check migration status](#check-migration-status) endpoint. Migration duration varies depending on the number of templates and assets. If the status does not change after an extended period, contact support. |

