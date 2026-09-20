# Manage WhatsApp Business accounts



**Warning:** **Embedded signup v2 will be deprecated on October 15, 2026.** Migrate your integration to [v4](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/version-4) before that date to avoid disruption. See [Versions](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/versions) for the full upgrade path.

This document describes how to use the [Client WhatsApp Business Accounts API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/business/client-whatsapp-business-accounts-api), [Owned WhatsApp Business Accounts API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/business/owned-whatsapp-business-accounts), and [WhatsApp Business Account API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/whatsapp-business-account-api) to manage your clients&#039; WhatsApp Business accounts.

The API calls on this page use the `whatsapp_business_management` permission to access WABAs not owned by your business. If your app lacks **Advanced access** for this permission, these calls return error code `200`. See [App Review](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/app-review) to request **Advanced access**.

## Get shared WABA ID with access token

After a business finishes the Embedded Signup flow, you can get the shared WABA ID using the returned `accessToken` with the [Debug Token](https://developers.facebook.com/docs/graph-api/reference/debug_token) endpoint. Include your [System User access token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#generating-system-user-access-tokens) in a request header prepended with `Authorization: Bearer` for this API call.

### Request syntax

```http
GET https://graph.facebook.com/v25.0/debug_token
  ?input_token=&lt;TOKEN_RETURNED_FROM_SIGNUP_FLOW&gt;
```

### Sample request

```curl
curl \
&#039;https://graph.facebook.com/v25.0/debug_token?input_token=EAAFl...&#039; \
-H &#039;Authorization: Bearer EAAJi...&#039;
```

### Sample response

```json
&#123;
  &quot;data&quot; : &#123;
    &quot;app_id&quot; : &quot;670843887433847&quot;,
    &quot;application&quot; : &quot;JaspersMarket&quot;,
    &quot;data_access_expires_at&quot; : 1672092840,
    &quot;expires_at&quot; : 1665090000,
    &quot;granular_scopes&quot; : [
      &#123;
        &quot;scope&quot; : &quot;whatsapp_business_management&quot;,
        &quot;target_ids&quot; : [
          &quot;102289599326934&quot;, // ID of newest WABA to grant app whatsapp_business_management
          &quot;101569239400667&quot;
        ]
      &#125;,
      &#123;
        &quot;scope&quot; : &quot;whatsapp_business_messaging&quot;,
        &quot;target_ids&quot; : [
          &quot;102289599326934&quot;,
          &quot;101569239400667&quot;
        ]
      &#125;
    ],
    &quot;is_valid&quot; : true,
    &quot;scopes&quot; : [
       &quot;whatsapp_business_management&quot;,
       &quot;whatsapp_business_messaging&quot;,
       &quot;public_profile&quot;
    ],
    &quot;type&quot; : &quot;USER&quot;,
    &quot;user_id&quot; : &quot;10222270944537964&quot;
  &#125;
&#125;
```

Each object in the `granular_scopes` array identifies the IDs of every WABA that has granted your app a given permission (`scope`). IDs for the most recently onboarded WABAs appear first, so capture the first ID in the `target_ids` array for the `whatsapp_business_management` scope.

## Get list of shared WABAs

Use the [Client WhatsApp Business Accounts API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/business/client-whatsapp-business-accounts-api) to [retrieve a list of all the WABAs](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/business/client-whatsapp-business-accounts-api#get-version-business-id-client-whatsapp-business-accounts) assigned to or shared with your business portfolio after the business completes the Embedded Signup flow.

You can call this API periodically to track WABAs shared with you as a fallback to the [Debug Token endpoint](https://developers.facebook.com/docs/graph-api/reference/debug_token) approach described in [Get shared WABA ID with access token](#get-shared-waba-id-with-access-token).

For available WABA fields, see the [WhatsApp Business account reference](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/whatsapp-business-account-api#fields).

### Request syntax

```http
GET https://graph.facebook.com/v25.0/&lt;BUSINESS_PORTFOLIO_ID&gt;/client_whatsapp_business_accounts
```

### Sample request

```curl
curl \
&#039;https://graph.facebook.com/v25.0/805021500648488/client_whatsapp_business_accounts/&#039; \
-H &#039;Authorization: Bearer EAAJi...&#039;
```

### Sample response

```json
&#123;
  &quot;data&quot;: [
    &#123;
      &quot;id&quot;: &quot;1906385232743451&quot;,
      &quot;name&quot;: &quot;My WhatsApp Business Account&quot;,
      &quot;currency&quot;: &quot;USD&quot;,
      &quot;timezone_id&quot;: &quot;1&quot;,
      &quot;message_template_namespace&quot;: &quot;abcdefghijk_12lmnop&quot;
    &#125;,
    &#123;
      &quot;id&quot;: &quot;1972385232742141&quot;,
      &quot;name&quot;: &quot;My Regional Account&quot;,
      &quot;currency&quot;: &quot;INR&quot;,
      &quot;timezone_id&quot;: &quot;5&quot;,
      &quot;message_template_namespace&quot;: &quot;12abcdefghijk_34lmnop&quot;
    &#125;
  ],
  &quot;paging&quot;: &#123;
    &quot;cursors&quot;: &#123;
      &quot;before&quot;: &quot;abcdefghij&quot;,
      &quot;after&quot;: &quot;klmnopqr&quot;
    &#125;
  &#125;
&#125;
```

## Understanding shared WABAs

### Permissions

As a partner, you have the following permissions in a shared WABA:

- Add phone numbers
- Create templates
- Send messages to customers
- Assign users to the account
- Access metrics
- View payment information

Your clients onboarding via Embedded Signup can see and do the following:

| Category | What can businesses see? |
| --- | --- |
| Insights | Messaging, cost, and quality state changes. |
| Quality | Quality statuses and ratings. |

| Category | What can businesses do? |
| --- | --- |
| Assets | Add and manage phone numbers and templates. |
| WABA management | Unshare WABA with a Solution Partner, delete WABA, and change settings. |
| Integration with other Meta products | Integrate with Ads that Click to WhatsApp. |

You cannot disable what your clients can see or do, or customize their views.

Your clients can visit [Manage your WhatsApp Solution Partner&#039;s permissions](https://www.facebook.com/business/help/861444384718867) for more information.

### Notifications

You receive relevant notifications via webhooks and through Meta Business Suite when:

- A client shares a WABA.
- Messaging limits or quality rating changes for a client&#039;s WABA.
- A phone number display name or a template is approved.

If a client leaves the Embedded Signup flow before completing it, the client may have shared the WABA but the phone number&#039;s certificate may not be ready. Without a ready certificate, you cannot register the number for API use. Reach out to the client to help them complete the Embedded Signup flow.

## Get list of owned WhatsApp Business accounts

Use the [Owned WhatsApp Business Accounts API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/business/owned-whatsapp-business-accounts) to [get a list of the WABAs](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/business/owned-whatsapp-business-accounts#get-version-business-id-owned-whatsapp-business-accounts) that your business owns. For the request, use your system user&#039;s access token.

### Request syntax

```http
GET https://graph.facebook.com/v25.0/&lt;BUSINESS_PORTFOLIO_ID&gt;/owned_whatsapp_business_accounts
```

### Sample request

```curl
curl \
&#039;https://graph.facebook.com/v25.0/805021500648488/owned_whatsapp_business_accounts/&#039; \
-H &#039;Authorization: Bearer EAAJi...&#039;
```

### Sample response

```json
&#123;
  &quot;data&quot;: [
    &#123;
      &quot;id&quot;: &quot;1906385232743451&quot;,
      &quot;name&quot;: &quot;My WhatsApp Business Account&quot;,
      &quot;currency&quot;: &quot;USD&quot;,
      &quot;timezone_id&quot;: &quot;1&quot;,
      &quot;message_template_namespace&quot;: &quot;abcdefghijk_12lmnop&quot;
    &#125;,
    &#123;
      &quot;id&quot;: &quot;1972385232742141&quot;,
      &quot;name&quot;: &quot;My Regional Account&quot;,
      &quot;currency&quot;: &quot;INR&quot;,
      &quot;timezone_id&quot;: &quot;5&quot;,
      &quot;message_template_namespace&quot;: &quot;12abcdefghijk_34lmnop&quot;
    &#125;
  ],
  &quot;paging&quot;: &#123;
    &quot;cursors&quot;: &#123;
      &quot;before&quot;: &quot;abcdefghij&quot;,
      &quot;after&quot;: &quot;klmnopqr&quot;
    &#125;
  &#125;
&#125;
```

## Filter WABAs by creation time

You can filter client and owned WhatsApp Business accounts based on their creation time using the [Owned WhatsApp Business Accounts API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/business/owned-whatsapp-business-accounts). Use the parameters listed below.

### Request syntax

```http
GET https://graph.facebook.com/v25.0/&lt;BUSINESS_PORTFOLIO_ID&gt;/owned_whatsapp_business_accounts
  ?filtering=&lt;FILTERING&gt;
```

The `filtering` value can be an array containing a single object comprised of the following properties:

### Filtering object properties

| Name | Description |
| --- | --- |
| `field` | Contains the field being used for filtering. Set to `creation_time`. |
| `operator` | Contains how you want to filter the accounts. Supported values:&lt;br&gt;&lt;br&gt;- `LESS_THAN`&lt;br&gt;- `GREATER_THAN` |
| `value` | A UNIX timestamp to filter on. |

### Sample object

```json
[
  &#123;
    &quot;field&quot; : &quot;creation_time&quot;,
    &quot;operator&quot; : &quot;GREATER_THAN&quot;,
    &quot;value&quot; : &quot;1604962813&quot;
  &#125;
]
```

### Sample request

```curl
curl \
&#039;https://graph.facebook.com/v25.0/805021500648488/owned_whatsapp_business_accounts?filtering=%5B%7B%22field%22%3A%22creation_time%22%2C%22operator%22%3A%22GREATER_THAN%22%2C%22value%22%3A%221604962813%22%7D%5D&#039; \
-H &#039;Authorization: Bearer EAAJi...&#039;
```

### Sample response

```json
&#123;
  &quot;data&quot;: [
    &#123;
      &quot;id&quot;: &quot;12312321312&quot;,
      &quot;name&quot;: &quot;test&quot;,
      &quot;currency&quot;: &quot;USD&quot;,
      &quot;timezone_id&quot;: &quot;1&quot;,
      &quot;message_template_namespace&quot;: &quot;46fe_814&quot;
    &#125;
  ],
  &quot;paging&quot;: &#123;
    &quot;cursors&quot;: &#123;
      &quot;before&quot;: &quot;QVFIUm9&quot;,
      &quot;after&quot;: &quot;QVFIUklX&quot;
    &#125;,
    &quot;next&quot;: &quot;https://graph.facebook.com/v25.0/&quot;
  &#125;
&#125;
```

## Sort WABAs by creation time

You can sort shared and owned WhatsApp Business accounts based on their creation time using the [Owned WhatsApp Business Accounts API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/business/owned-whatsapp-business-accounts).

### Request syntax

```http
GET https://graph.facebook.com/v25.0/&lt;BUSINESS_PORTFOLIO_ID&gt;/owned_whatsapp_business_accounts
  ?sort=&lt;SORT&gt;
```

The `sort` value can be `creation_time_ascending` or `creation_time_descending`.

### Sample request

```curl
curl \
&#039;https://graph.facebook.com/v25.0/805021500648488/owned_whatsapp_business_accounts?sort=creation_time_ascending&#039; \
-H &#039;Authorization: Bearer EAAJi...&#039;
```

### Sample response

```json
&#123;
  &quot;data&quot;: [
    &#123;
      &quot;id&quot;: &quot;1906385232743451&quot;,
      &quot;name&quot;: &quot;My WhatsApp Business Account&quot;,
      &quot;currency&quot;: &quot;USD&quot;,
      &quot;timezone_id&quot;: &quot;1&quot;,
      &quot;message_template_namespace&quot;: &quot;abcdefghijk_12lmnop&quot;
    &#125;,
    &#123;
      &quot;id&quot;: &quot;1972385232742141&quot;,
      &quot;name&quot;: &quot;My Regional Account&quot;,
      &quot;currency&quot;: &quot;INR&quot;,
      &quot;timezone_id&quot;: &quot;5&quot;,
      &quot;message_template_namespace&quot;: &quot;12abcdefghijk_34lmnop&quot;
    &#125;
  ],
  &quot;paging&quot;: &#123;
    &quot;cursors&quot;: &#123;
      &quot;before&quot;: &quot;abcdefghij&quot;,
      &quot;after&quot;: &quot;klmnopqr&quot;
    &#125;
  &#125;
&#125;
```

## Retrieve WABA review status

Use the [WhatsApp Business Account API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/whatsapp-business-account-api) to [get a WhatsApp Business account&#039;s review status](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/whatsapp-business-account-api#get-version-waba-id) by requesting the `account_review_status` field.

### Request syntax

```http
GET https://graph.facebook.com/v25.0/&lt;WABA_ID&gt;
  ?fields=account_review_status
```

### Sample request

```curl
curl \
&#039;https://graph.facebook.com/v25.0/106526625562206?fields=account_review_status&#039; \
-H &#039;Authorization: Bearer EAAJi...&#039;
```

### Sample response

```json
&#123;
  &quot;account_review_status&quot;: &quot;APPROVED&quot;,
  &quot;id&quot;: &quot;1111111111111&quot;
&#125;
```

The `account_review_status` property can have one of the following values: `PENDING`, `APPROVED`, and `REJECTED`.

## See also

* [Business Management API](https://developers.facebook.com/documentation/business-messaging/whatsapp/about-the-platform#business-management-api)
* Reference: [Business](https://developers.facebook.com/documentation/ads-commerce/marketing-api/reference/business)
* Reference: [WhatsApp Business account](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/whatsapp-business-account-api)
