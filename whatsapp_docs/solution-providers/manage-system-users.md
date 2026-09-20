# Manage System Users



**Warning:** **Embedded signup v2 will be deprecated on October 15, 2026.** Migrate your integration to [v4](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/version-4) before that date to avoid disruption. See [Versions](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/versions) for the full upgrade path.

Adding your System User to shared WhatsApp Business Accounts allows you to programmatically manage the accounts. This guide covers actions Solution Partners may need to perform to manage their system users.

For help creating a system user and generating your system user access token, see [Access Tokens, System User access tokens](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#system-user-access-tokens).

## Retrieve system user IDs &#123;#retrieve-system-user-ids&#125;

You can cache the System User IDs for future use.

### Request

```curl
curl -i -X GET &quot;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;/system_users
  ?access_token=&lt;SYSTEM_USER_ACCESS_TOKEN&gt;&quot;
```

To find the ID of a business, go to [**Business Manager**](https://business.facebook.com/) &gt; **Business Settings** &gt; **Business Info**. There, you will see information about the business, including the ID.

### Response

```json
&#123;
  &quot;data&quot;: [
    &#123;
      &quot;id&quot;: &quot;1972555232742222&quot;,
      &quot;name&quot;: &quot;My System User&quot;,
      &quot;role&quot;: &quot;EMPLOYEE&quot;
    &#125;
  ]
&#125;
```

## Add system users to a WhatsApp Business account

For this API call, **you need to use the access token of a System User with admin permissions**.

### Request syntax

In the following example, use the ID for the assigned WABA.

```html
curl -i -X POST &quot;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;/assigned_users
  ?user=&lt;ASSIGNED_USER_ID&gt;
  &amp;tasks=[&#039;&lt;ASSIGNED_USERS_TASKS_AND_PERMISSIONS&gt;&#039;]
  &amp;access_token=&lt;SYSTEM_USER_ACCESS_TOKEN&gt;&quot;
```

To find the ID of a WhatsApp Business Account, go to [**Business Manager**](https://business.facebook.com/) &gt; **Business Settings** &gt; **Accounts** &gt; **WhatsApp Business Accounts**. Find the account you want to use and click on it. A panel opens, with information about the account, including the ID.

For the `&lt;ASSIGNED_USER_ID&gt;`, use the system user ID returned from [your `/&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;/system_users` call](#retrieve-system-user-ids).

### Permissions

| Name | Description |
| --- | --- |
| `MANAGE` | Provides admin access.&lt;br&gt;&lt;br&gt;Users can have admin access on a WhatsApp Business account that is shared with Admin permissions.&lt;br&gt;&lt;br&gt;Note: If you are a Solution Partner trying to add a user to a WhatsApp Business account that is shared with you via a [Multi-Partner Solution](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/multi-partner-solutions), then you would need to account for the following scenarios:&lt;br&gt;&lt;br&gt;- If you are not granted `MESSAGING` permission on the solution, then you need to decide which granular tasks you need when adding the user to the shared WhatsApp Business account: `DEVELOP`, `MANAGE_TEMPLATES`, `MANAGE_PHONE`, `VIEW_COST`, `MANAGE_EXTENSIONS`, `VIEW_PHONE_ASSETS`, `MANAGE_PHONE_ASSETS`, `VIEW_TEMPLATES`, `VIEW_INSIGHTS`, `MANAGE_USERS`, `MANAGE_BILLING`.&lt;br&gt;- In such scenario, also note that `MANAGE_BILLING` is needed for sharing Line of Credit.&lt;br&gt;- MANAGE will only work if you are given full access on the solution i.e. including `MESSAGING`. |
| `DEVELOP` | Provides developer access.&lt;br&gt;Users can have developer access on a WhatsApp Business account that is shared with Standard permissions. |

### Response

```json
&#123;
  &quot;success&quot;: true
&#125;
```

## Retrieve assigned users

You can fetch the assigned users of the WhatsApp Business account to verify that the user was added. This is not a required step but helps with validation.

### Request syntax

In the following example, use the ID for the assigned WABA.

```html
curl -i -X GET &quot;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;/assigned_users
  ?business=&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;
  &amp;access_token=&lt;SYSTEM_USER_ACCESS_TOKEN&gt;&quot;
```

### Response

```json
&#123;
  &quot;data&quot;: [
    &#123;
      &quot;id&quot;: &quot;1972385232742142&quot;,
      &quot;name&quot;: &quot;Anna Flex&quot;,
      &quot;tasks&quot;: [
        &quot;MANAGE&quot;
      ]
    &#125;,
    &#123;
      &quot;id&quot;: &quot;1972385232752545&quot;,
      &quot;name&quot;: &quot;Jasper Brown&quot;,
      &quot;tasks&quot;: [
        &quot;DEVELOP&quot;
      ]
    &#125;
  ]
&#125;
```

## See also

* Reference: [WhatsApp Business account](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/whatsapp-business-account-api)
