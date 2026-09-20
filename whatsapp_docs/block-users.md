# Block users


You can block and unblock WhatsApp users and retrieve a list of blocked users using the [Block Users API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/block-api).

## Before you start

When you block a WhatsApp user:

* The user cannot contact your business or see that you are online.
* Your business cannot message the user. Attempts to message a blocked user return an error.

The API returns errors per-number, since blocks might succeed on some numbers and fail on others. The Block Users API is synchronous.

## Limitations

* You can only block users that have messaged your business in the last 24 hours.
* You cannot block another WhatsApp Business account.
* Each request can include a maximum of 1,000 users.
* The blocklist has a 64,000 user limit.

## Block a user

Use the [Block Users API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/block-api#post-version-phone-number-id-block-users) to [block a list of WhatsApp users](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/block-api#post-version-phone-number-id-block-users).

### Request syntax

```html
curl &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;WHATSAPP_BUSINESS_PHONE_NUMBER_ID&gt;/block_users&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039; \
-d &#039;
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;block_users&quot;: [
    &#123;
      &quot;user&quot;: &quot;&lt;WHATSAPP_USER_PHONE_NUMBER&gt;&quot;
    &#125;
  ]
&#125;&#039;
```

### Request parameters

| Placeholder | Description | Example Value |
| --- | --- | --- |
| `&lt;ACCESS_TOKEN&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;[System token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#system-user-access-tokens) or [business token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#business-integration-system-user-access-tokens). | `EAAA...` |
| `&lt;API_VERSION&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Graph API version. | v25.0 |
| `&lt;WHATSAPP_BUSINESS_PHONE_NUMBER_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;WhatsApp business phone number ID. | `106540352242922` |
| `&lt;WHATSAPP_USER_PHONE_NUMBER&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;WhatsApp user phone number. This is the same value returned by the API as the `input` value when sending a message to a WhatsApp user. Note that a WhatsApp user&#039;s phone number and ID may not always match. | `+16505551234` |

### Response syntax

```json
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;block_users&quot;: &#123;
    &quot;added_users&quot;: [
      &#123;
        &quot;input&quot;: &quot;&lt;WHATSAPP_USER_PHONE_NUMBER&gt;&quot;,
        &quot;wa_id&quot;: &quot;&lt;WHATSAPP_USER_ID&gt;&quot;
      &#125;
    ],
    &quot;failed_users&quot;: [
      &#123;
        &quot;input&quot;: &quot;&lt;WHATSAPP_USER_PHONE_NUMBER&gt;&quot;,
        &quot;wa_id&quot;: &quot;&lt;WHATSAPP_USER_ID&gt;&quot;,
        &quot;errors&quot;: [
          &#123;
            &quot;message&quot;: &quot;&lt;MESSAGE&gt;&quot;,
            &quot;code&quot;: &quot;&lt;CODE&gt;&quot;,
            &quot;error_data&quot;: &#123;
              &quot;details&quot;: &quot;&lt;DETAILS&gt;&quot;
            &#125;
          &#125;
        ]
      &#125;
    ]
  &#125;
&#125;
```

### Response parameters

| Field | Description | Example Value |
| --- | --- | --- |
| `&lt;CODE&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | Error code. See [Error codes](#error-codes) below. Only present in `failed_users`. | `131047` |
| `&lt;DETAILS&gt;`&lt;br&gt;&lt;br&gt;_String_ | Additional detail about the error. Only present in `failed_users`. | `User has not messaged in the last 24 hours` |
| `&lt;MESSAGE&gt;`&lt;br&gt;&lt;br&gt;_String_ | Error message describing why the block failed. Only present in `failed_users`. | `Re-engagement required` |
| `&lt;WHATSAPP_USER_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | WhatsApp user ID. Note that a WhatsApp user&#039;s ID and phone number may not always match.&lt;br&gt;&lt;br&gt;&lt;br&gt;Returned as `wa_id`. May not be present in `failed_users` if the number is invalid. | `16505551234` |
| `&lt;WHATSAPP_USER_PHONE_NUMBER&gt;`&lt;br&gt;&lt;br&gt;_String_ | WhatsApp user phone number. This is the same value returned by the API as the `input` value when sending a message to a WhatsApp user. Note that a WhatsApp user&#039;s phone number and ID may not always match.&lt;br&gt;&lt;br&gt;&lt;br&gt;Returned as `input` in both `added_users` and `failed_users` arrays. | `+16505551234` |

### Example request

This example blocks two WhatsApp users.

```curl
curl &#039;https://graph.facebook.com/v25.0/106540352242922/block_users&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039; \
-d &#039;
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;block_users&quot;: [
    &#123;
      &quot;user&quot;: &quot;+16505551234&quot;
    &#125;,
    &#123;
      &quot;user&quot;: &quot;+14155559876&quot;
    &#125;
  ]
&#125;&#039;
```

### Example response

Successful response when all users are blocked:

```json
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;block_users&quot;: &#123;
    &quot;added_users&quot;: [
      &#123;
        &quot;input&quot;: &quot;+16505551234&quot;,
        &quot;wa_id&quot;: &quot;16505551234&quot;
      &#125;,
      &#123;
        &quot;input&quot;: &quot;+14155559876&quot;,
        &quot;wa_id&quot;: &quot;14155559876&quot;
      &#125;
    ]
  &#125;
&#125;
```

Mixed success/failure response when some users cannot be blocked:

```json
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;block_users&quot;: &#123;
    &quot;added_users&quot;: [
      &#123;
        &quot;input&quot;: &quot;+16505551234&quot;,
        &quot;wa_id&quot;: &quot;16505551234&quot;
      &#125;
    ],
    &quot;failed_users&quot;: [
      &#123;
        &quot;input&quot;: &quot;+14155559876&quot;,
        &quot;wa_id&quot;: &quot;14155559876&quot;,
        &quot;errors&quot;: [
          &#123;
            &quot;message&quot;: &quot;Re-engagement required&quot;,
            &quot;code&quot;: 131047,
            &quot;error_data&quot;: &#123;
              &quot;details&quot;: &quot;User has not messaged in the last 24 hours&quot;
            &#125;
          &#125;
        ]
      &#125;
    ]
  &#125;,
  &quot;error&quot;: &#123;
    &quot;message&quot;: &quot;(#139100) Failed to block/unblock users&quot;,
    &quot;type&quot;: &quot;OAuthException&quot;,
    &quot;code&quot;: 139100,
    &quot;error_data&quot;: &#123;
      &quot;details&quot;: &quot;Failed to block some users, see the block_users response list for details&quot;
    &#125;,
    &quot;fbtrace_id&quot;: &quot;&lt;FBTRACE_ID&gt;&quot;
  &#125;
&#125;
```

## Unblock a user

Use the [Block Users API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/block-api#delete-version-phone-number-id-block-users) to [unblock a list of WhatsApp users](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/block-api#delete-version-phone-number-id-block-users).

### Request syntax

```html
curl -X DELETE &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;WHATSAPP_BUSINESS_PHONE_NUMBER_ID&gt;/block_users&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039; \
-d &#039;
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;block_users&quot;: [
    &#123;
      &quot;user&quot;: &quot;&lt;WHATSAPP_USER_PHONE_NUMBER&gt;&quot;
    &#125;
  ]
&#125;&#039;
```

### Request parameters

| Placeholder | Description | Example Value |
| --- | --- | --- |
| `&lt;ACCESS_TOKEN&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;[System token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#system-user-access-tokens) or [business token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#business-integration-system-user-access-tokens). | `EAAA...` |
| `&lt;API_VERSION&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Graph API version. | v25.0 |
| `&lt;WHATSAPP_BUSINESS_PHONE_NUMBER_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;WhatsApp business phone number ID. | `106540352242922` |
| `&lt;WHATSAPP_USER_PHONE_NUMBER&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;WhatsApp user phone number. This is the same value returned by the API as the `input` value when sending a message to a WhatsApp user. Note that a WhatsApp user&#039;s phone number and ID may not always match. | `+16505551234` |

### Response syntax

```json
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;block_users&quot;: &#123;
    &quot;removed_users&quot;: [
      &#123;
        &quot;input&quot;: &quot;&lt;WHATSAPP_USER_PHONE_NUMBER&gt;&quot;,
        &quot;wa_id&quot;: &quot;&lt;WHATSAPP_USER_ID&gt;&quot;
      &#125;
    ],
    &quot;failed_users&quot;: [
      &#123;
        &quot;input&quot;: &quot;&lt;WHATSAPP_USER_PHONE_NUMBER&gt;&quot;,
        &quot;wa_id&quot;: &quot;&lt;WHATSAPP_USER_ID&gt;&quot;,
        &quot;errors&quot;: [
          &#123;
            &quot;message&quot;: &quot;&lt;MESSAGE&gt;&quot;,
            &quot;code&quot;: &quot;&lt;CODE&gt;&quot;,
            &quot;error_data&quot;: &#123;
              &quot;details&quot;: &quot;&lt;DETAILS&gt;&quot;
            &#125;
          &#125;
        ]
      &#125;
    ]
  &#125;
&#125;
```

### Response parameters

| Field | Description | Example Value |
| --- | --- | --- |
| `&lt;WHATSAPP_USER_PHONE_NUMBER&gt;`&lt;br&gt;&lt;br&gt;_String_ | WhatsApp user phone number. This is the same value returned by the API as the `input` value when sending a message to a WhatsApp user. Note that a WhatsApp user&#039;s phone number and ID may not always match.&lt;br&gt;&lt;br&gt;&lt;br&gt;Returned as `input` in both `removed_users` and `failed_users` arrays. | `+16505551234` |
| `&lt;WHATSAPP_USER_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | WhatsApp user ID. Note that a WhatsApp user&#039;s ID and phone number may not always match.&lt;br&gt;&lt;br&gt;&lt;br&gt;Returned as `wa_id`. May not be present in `failed_users` if the number is invalid. | `16505551234` |
| `&lt;MESSAGE&gt;`&lt;br&gt;&lt;br&gt;_String_ | Error message describing why the unblock failed. Only present in `failed_users`. | `Re-engagement required` |
| `&lt;CODE&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | Error code. See [Error codes](#error-codes) below. Only present in `failed_users`. | `131047` |
| `&lt;DETAILS&gt;`&lt;br&gt;&lt;br&gt;_String_ | Additional detail about the error. Only present in `failed_users`. | `User has not messaged in the last 24 hours` |

### Example request

This example unblocks two previously blocked WhatsApp users.

```curl
curl -X DELETE &#039;https://graph.facebook.com/v25.0/106540352242922/block_users&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039; \
-d &#039;
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;block_users&quot;: [
    &#123;
      &quot;user&quot;: &quot;+16505551234&quot;
    &#125;,
    &#123;
      &quot;user&quot;: &quot;+14155559876&quot;
    &#125;
  ]
&#125;&#039;
```

### Example response

Successful response when all users are unblocked:

```json
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;block_users&quot;: &#123;
    &quot;removed_users&quot;: [
      &#123;
        &quot;input&quot;: &quot;+16505551234&quot;,
        &quot;wa_id&quot;: &quot;16505551234&quot;
      &#125;,
      &#123;
        &quot;input&quot;: &quot;+14155559876&quot;,
        &quot;wa_id&quot;: &quot;14155559876&quot;
      &#125;
    ]
  &#125;
&#125;
```

Mixed success/failure response when some users cannot be unblocked:

```json
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;block_users&quot;: &#123;
    &quot;removed_users&quot;: [
      &#123;
        &quot;input&quot;: &quot;+16505551234&quot;,
        &quot;wa_id&quot;: &quot;16505551234&quot;
      &#125;
    ],
    &quot;failed_users&quot;: [
      &#123;
        &quot;input&quot;: &quot;+14155559876&quot;,
        &quot;wa_id&quot;: &quot;14155559876&quot;,
        &quot;errors&quot;: [
          &#123;
            &quot;message&quot;: &quot;Re-engagement required&quot;,
            &quot;code&quot;: 131047,
            &quot;error_data&quot;: &#123;
              &quot;details&quot;: &quot;User has not messaged in the last 24 hours&quot;
            &#125;
          &#125;
        ]
      &#125;
    ]
  &#125;,
  &quot;error&quot;: &#123;
    &quot;message&quot;: &quot;(#139100) Failed to block/unblock users&quot;,
    &quot;type&quot;: &quot;OAuthException&quot;,
    &quot;code&quot;: 139100,
    &quot;error_data&quot;: &#123;
      &quot;details&quot;: &quot;Failed to unblock some users, see the block_users response list for details&quot;
    &#125;,
    &quot;fbtrace_id&quot;: &quot;&lt;FBTRACE_ID&gt;&quot;
  &#125;
&#125;
```

## Get blocked users

Use the [Block Users API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/block-api#get-version-phone-number-id-block-users) to [get a list of blocked users](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/block-api#get-version-phone-number-id-block-users) on your WhatsApp Business phone number.

### Request syntax

```html
curl &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;WHATSAPP_BUSINESS_PHONE_NUMBER_ID&gt;/block_users?limit=&lt;LIMIT&gt;&amp;after=&lt;AFTER_CURSOR&gt;&amp;before=&lt;BEFORE_CURSOR&gt;&#039; \
-H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039;
```

### Request parameters

| Placeholder | Description | Example Value |
| --- | --- | --- |
| `&lt;ACCESS_TOKEN&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;[System token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#system-user-access-tokens) or [business token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#business-integration-system-user-access-tokens). | `EAAA...` |
| `&lt;API_VERSION&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Graph API version. | v25.0 |
| `&lt;WHATSAPP_BUSINESS_PHONE_NUMBER_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;WhatsApp business phone number ID. | `106540352242922` |
| `&lt;LIMIT&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | **Optional.**&lt;br&gt;&lt;br&gt;Maximum number of blocked users to return per request. | `10` |
| `&lt;AFTER_CURSOR&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Cursor for forward pagination. Learn more about [paginated results in Graph API](https://developers.facebook.com/docs/graph-api/results). | `eyJvZAmZAzZAXQ...` |
| `&lt;BEFORE_CURSOR&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Cursor for backward pagination. Learn more about [paginated results in Graph API](https://developers.facebook.com/docs/graph-api/results). | `eyJvZAmZAzZAXQ...` |

### Response syntax

```json
&#123;
  &quot;data&quot;: [
    &#123;
      &quot;messaging_product&quot;: &quot;whatsapp&quot;,
      &quot;wa_id&quot;: &quot;&lt;WHATSAPP_USER_ID&gt;&quot;
    &#125;
  ],
  &quot;paging&quot;: &#123;
    &quot;cursors&quot;: &#123;
      &quot;after&quot;: &quot;&lt;AFTER_CURSOR&gt;&quot;,
      &quot;before&quot;: &quot;&lt;BEFORE_CURSOR&gt;&quot;
    &#125;
  &#125;
&#125;
```

### Response parameters

| Field | Description | Example Value |
| --- | --- | --- |
| `&lt;WHATSAPP_USER_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | WhatsApp user ID. Note that a WhatsApp user&#039;s ID and phone number may not always match.&lt;br&gt;&lt;br&gt;&lt;br&gt;Returned as `wa_id` in each object in the `data` array. | `16505551234` |
| `&lt;AFTER_CURSOR&gt;`&lt;br&gt;&lt;br&gt;_String_ | Cursor for forward pagination. Learn more about [paginated results in Graph API](https://developers.facebook.com/docs/graph-api/results). | `eyJvZAmZAzZAXQ...` |
| `&lt;BEFORE_CURSOR&gt;`&lt;br&gt;&lt;br&gt;_String_ | Cursor for backward pagination. Learn more about [paginated results in Graph API](https://developers.facebook.com/docs/graph-api/results). | `eyJvZAmZAzZAXQ...` |

### Example request

This example retrieves up to 10 blocked users.

```curl
curl &#039;https://graph.facebook.com/v25.0/106540352242922/block_users?limit=10&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039;
```

### Example response

```json
&#123;
  &quot;data&quot;: [
    &#123;
      &quot;messaging_product&quot;: &quot;whatsapp&quot;,
      &quot;wa_id&quot;: &quot;16505551234&quot;
    &#125;,
    &#123;
      &quot;messaging_product&quot;: &quot;whatsapp&quot;,
      &quot;wa_id&quot;: &quot;14155559876&quot;
    &#125;
  ],
  &quot;paging&quot;: &#123;
    &quot;cursors&quot;: &#123;
      &quot;after&quot;: &quot;eyJvZAmZAzZAXQiOjAsInZAlcnNpb25JZACI6IjE3Mzc2Nzk2ODgzODM1ODQifQZDZD&quot;,
      &quot;before&quot;: &quot;eyJvZAmZAzZAXQiOjAsInZAlcnNpb25JZACI6IjE3Mzc2Nzk2ODgzODM1ODQifQZDZD&quot;
    &#125;
  &#125;
&#125;
```

## Error codes

| Code | Description |
| --- | --- |
| `139100`&lt;br&gt;&lt;br&gt;Failed to block/unblock some users | Bulk blocking failed to block some or all of the users. |
| `139101`&lt;br&gt;&lt;br&gt;Blocklist limit reached | The blocklist has reached its 64,000 user limit. |
| `139102`&lt;br&gt;&lt;br&gt;Blocklist concurrent update | Occurs when the blocklist is updated while performing a pagination request and `version_id` does not match. |
| `139103`&lt;br&gt;&lt;br&gt;Internal error | Internal error. Try the request again. |
| `130429`&lt;br&gt;&lt;br&gt;Rate limit hit | Occurs when either too many numbers are in the request or too many requests are made over a short period of time. |
| `131021`&lt;br&gt;&lt;br&gt;Self block | Cannot block your own phone number. |
| `131047`&lt;br&gt;&lt;br&gt;Re-engagement required | The WhatsApp user has not messaged your business in the last 24 hours. This error also occurs if the number is an invalid WhatsApp user. |

