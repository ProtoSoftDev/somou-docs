# In-App Signup


In-App Signup lets you create opt-in deep links that WhatsApp users can click to subscribe to your messages. When a WhatsApp user subscribes through a deep link, you receive a webhook notification and the user receives a confirmation message.

## Overview

The In-App Signup API is available in Graph API v22.0 and later. The API supports the following operations:

| Operation | Method | Endpoint | Description |
|---|---|---|---|
| Create signup | `POST` | `/&lt;WABA_ID&gt;/signups` | Create a new signup deep link under a WABA. |
| Get signup details | `GET` | `/signups/&lt;SIGNUP_ID&gt;` | Retrieve metadata for a specific signup. |
| List signups | `GET` | `/&lt;WABA_ID&gt;/signups` | List all signups for a WABA. |
| Update signup | `POST` | `/signups/&lt;SIGNUP_ID&gt;` | Update signup metadata or disable a signup. |

### Deep link format

After creating a signup, the API returns the signup entity ID. Use this ID along with your phone number to construct the deep link URL you share with WhatsApp users:

```html
wa.me/&lt;PHONE_NUMBER&gt;/signup/&lt;SIGNUP_ID&gt;
```

The signup entity isn&#039;t tied to a specific phone number. You can construct deep link URLs with the same signup ID and any phone number associated with your WABA.

### Terms of service

You must accept the Terms of Service before creating your first signup. In your first create request, include the `policy` object with the `tos` field set to the WhatsApp Business Terms of Service URL, `https://www.facebook.com/legal/ads-manager-marketing-messages-terms`, and `accepted` set to `true`. Subsequent requests don&#039;t require the `policy` object.

### Permissions

All endpoints require the `whatsapp_business_management` permission.

### Promo codes

You can include a promotional code in the confirmation message by adding the `&#123;&#123;promo_code&#125;&#125;` placeholder to your `confirmation_message` and providing a `promo_code` value. When a WhatsApp user subscribes, the placeholder is replaced with the promo code value in the delivered message.

If you use `&#123;&#123;promo_code&#125;&#125;` in the `confirmation_message` but don&#039;t provide a `promo_code` value, the API returns an error. Unknown placeholders also return an error.

## Create signup

Use the In-App Signup API to create a signup deep link entity under your WhatsApp Business account.

### Request syntax

```html
curl &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;WABA_ID&gt;/signups&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039; \
-d &#039;
&#123;
  &quot;signup_message&quot;: &quot;&lt;SIGNUP_MESSAGE&gt;&quot;,
  &quot;confirmation_message&quot;: &quot;&lt;CONFIRMATION_MESSAGE&gt;&quot;,
  &quot;privacy_policy_url&quot;: &quot;&lt;PRIVACY_POLICY_URL&gt;&quot;,
  &quot;website_url&quot;: &quot;&lt;WEBSITE_URL&gt;&quot;,
  &quot;promo_code&quot;: &quot;&lt;PROMO_CODE&gt;&quot;,
  &quot;display_name&quot;: &quot;&lt;DISPLAY_NAME&gt;&quot;,
  &quot;policy&quot;: &#123;
    &quot;tos&quot;: &quot;&lt;TOS_URL&gt;&quot;,
    &quot;accepted&quot;: true
  &#125;
&#125;&#039;
```

### Request parameters

| Placeholder | Description | Example value |
| --- | --- | --- |
| `&lt;ACCESS_TOKEN&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;[System token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#system-user-access-tokens) or [business token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#business-integration-system-user-access-tokens). | `EAAA...` |
| `&lt;API_VERSION&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Graph API version. | v25.0 |
| `&lt;WABA_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Your WhatsApp Business account ID. | `102290129340398` |
| `&lt;SIGNUP_MESSAGE&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;The description shown on the pre-consent screen when a WhatsApp user opens the deep link. Supports WhatsApp formatting. Must be 1-300 characters. | `Get exclusive offers and news delivered directly to your WhatsApp!` |
| `&lt;CONFIRMATION_MESSAGE&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;The message sent to the WhatsApp user immediately after a successful opt-in. Can contain the `&#123;&#123;promo_code&#125;&#125;` placeholder, which is replaced with the `promo_code` value in the delivered message. Must be 1-300 characters. | `Thank you for signing up!` |
| `&lt;PRIVACY_POLICY_URL&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;A link to your privacy policy. Must start with `http://` or `https://`. Immutable after creation. | `https://example-business.com/privacy-policy` |
| `&lt;WEBSITE_URL&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Your business website URL. Must start with `https://`. | `https://example-business.com` |
| `&lt;PROMO_CODE&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;A promotional code value. Must contain only alphanumeric characters (letters and numbers) and be 1-50 characters. Replaces `&#123;&#123;promo_code&#125;&#125;` in the confirmation message. | `WELCOME10` |
| `&lt;DISPLAY_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;A business-facing nickname for the signup link. Not shown to WhatsApp users. Must be 1-256 characters. | `Summer Sale Signup` |
| `&lt;TOS_URL&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required** on the first signup creation per business.&lt;br&gt;&lt;br&gt;The Terms of Service URL. Include the `policy` object with `&quot;accepted&quot;: true`. | `https://www.facebook.com/legal/ads-manager-marketing-messages-terms` |

### Example request

This example creates a signup deep link with a welcome promo code.

```html
curl &#039;https://graph.facebook.com/v25.0/102290129340398/signups&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039; \
-d &#039;
&#123;
  &quot;signup_message&quot;: &quot;Get exclusive offers and news delivered directly to your WhatsApp!&quot;,
  &quot;confirmation_message&quot;: &quot;Thank you for signing up! Here is your welcome code: &#123;&#123;promo_code&#125;&#125;.&quot;,
  &quot;privacy_policy_url&quot;: &quot;https://example-business.com/privacy-policy&quot;,
  &quot;promo_code&quot;: &quot;WELCOME10&quot;,
  &quot;display_name&quot;: &quot;Summer Sale Signup&quot;,
  &quot;website_url&quot;: &quot;https://example-business.com&quot;,
  &quot;policy&quot;: &#123;
    &quot;tos&quot;: &quot;https://www.facebook.com/legal/ads-manager-marketing-messages-terms&quot;,
    &quot;accepted&quot;: true
  &#125;
&#125;&#039;
```

### Example response

Upon success, the API returns the signup entity ID:

```json
&#123;
  &quot;id&quot;: &quot;9876543210123456&quot;
&#125;
```

Use the returned `id` to construct the deep link:

```html
wa.me/15551234567/signup/9876543210123456
```

## Get signup details

Use the In-App Signup API to get the metadata for a specific signup deep link.

### Request syntax

```html
curl &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/signups/&lt;SIGNUP_ID&gt;&#039; \
-H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039;
```

### Request parameters

| Placeholder | Description | Example value |
| --- | --- | --- |
| `&lt;ACCESS_TOKEN&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;[System token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#system-user-access-tokens) or [business token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#business-integration-system-user-access-tokens). | `EAAA...` |
| `&lt;API_VERSION&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Graph API version. | v25.0 |
| `&lt;SIGNUP_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;The ID of the signup entity to retrieve. | `9876543210123456` |

### Response fields

| Field | Type | Description |
| --- | --- | --- |
| `id` | _String_ | The signup entity ID. |
| `waba_id` | _String_ | The parent WhatsApp Business account ID. |
| `signup_message` | _String_ | The description shown on the pre-consent screen. |
| `confirmation_message` | _String_ | The message sent to the WhatsApp user after opt-in. |
| `privacy_policy_url` | _String_ | Your privacy policy URL. |
| `promo_code` | _String_ | The promotional code value, if set. |
| `status` | _String_ | Current status: `ACTIVE` or `DISABLED`. |
| `display_name` | _String_ | The business-facing nickname for the signup link, if set. Not shown to WhatsApp users. |
| `website_url` | _String_ | Your business website URL, if set. |

### Example request

```html
curl &#039;https://graph.facebook.com/v25.0/signups/9876543210123456&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039;
```

### Example response

```json
&#123;
  &quot;id&quot;: &quot;9876543210123456&quot;,
  &quot;waba_id&quot;: &quot;102290129340398&quot;,
  &quot;signup_message&quot;: &quot;Get exclusive offers and news delivered directly to your WhatsApp!&quot;,
  &quot;confirmation_message&quot;: &quot;Thank you for signing up!&quot;,
  &quot;privacy_policy_url&quot;: &quot;https://example-business.com/privacy-policy&quot;,
  &quot;promo_code&quot;: &quot;WELCOME10&quot;,
  &quot;status&quot;: &quot;ACTIVE&quot;,
  &quot;display_name&quot;: &quot;Summer Sale Signup&quot;,
  &quot;website_url&quot;: &quot;https://example-business.com&quot;
&#125;
```

## List signups

Use the In-App Signup API to list all signups for a WABA, with pagination.

### Request syntax

```html
curl &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;WABA_ID&gt;/signups?limit=&lt;LIMIT&gt;&#039; \
-H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039;
```

### Request parameters

| Placeholder | Description | Example value |
| --- | --- | --- |
| `&lt;ACCESS_TOKEN&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;[System token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#system-user-access-tokens) or [business token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#business-integration-system-user-access-tokens). | `EAAA...` |
| `&lt;API_VERSION&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Graph API version. | v25.0 |
| `&lt;WABA_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Your WhatsApp Business account ID. | `102290129340398` |
| `&lt;LIMIT&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | **Optional.**&lt;br&gt;&lt;br&gt;Maximum number of signups to return per page. | `10` |

### Example request

This example lists signups, 10 per page.

```html
curl &#039;https://graph.facebook.com/v25.0/102290129340398/signups?limit=10&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039;
```

### Example response

```json
&#123;
  &quot;data&quot;: [
    &#123;
      &quot;id&quot;: &quot;9876543210123456&quot;,
      &quot;signup_message&quot;: &quot;Get exclusive offers and news...&quot;,
      &quot;status&quot;: &quot;ACTIVE&quot;
    &#125;,
    &#123;
      &quot;id&quot;: &quot;9876543210654321&quot;,
      &quot;signup_message&quot;: &quot;Subscribe for weekly updates...&quot;,
      &quot;status&quot;: &quot;ACTIVE&quot;
    &#125;
  ],
  &quot;paging&quot;: &#123;
    &quot;cursors&quot;: &#123;
      &quot;before&quot;: &quot;xyz789&quot;,
      &quot;after&quot;: &quot;abc123&quot;
    &#125;,
    &quot;next&quot;: &quot;https://graph.facebook.com/v25.0/102290129340398/signups?limit=10&amp;after=abc123&quot;
  &#125;
&#125;
```

## Update signup

Use the In-App Signup API to update a signup deep link. Only the fields you include in the request body are changed.

### Request syntax

```html
curl &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/signups/&lt;SIGNUP_ID&gt;&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039; \
-d &#039;
&#123;
  &quot;status&quot;: &quot;&lt;STATUS&gt;&quot;,
  &quot;signup_message&quot;: &quot;&lt;SIGNUP_MESSAGE&gt;&quot;,
  &quot;confirmation_message&quot;: &quot;&lt;CONFIRMATION_MESSAGE&gt;&quot;,
  &quot;promo_code&quot;: &quot;&lt;PROMO_CODE&gt;&quot;,
  &quot;display_name&quot;: &quot;&lt;DISPLAY_NAME&gt;&quot;,
  &quot;website_url&quot;: &quot;&lt;WEBSITE_URL&gt;&quot;
&#125;&#039;
```

### Request parameters

| Placeholder | Description | Example value |
| --- | --- | --- |
| `&lt;ACCESS_TOKEN&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;[System token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#system-user-access-tokens) or [business token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#business-integration-system-user-access-tokens). | `EAAA...` |
| `&lt;API_VERSION&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Graph API version. | v25.0 |
| `&lt;SIGNUP_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;The ID of the signup entity to update. | `9876543210123456` |
| `&lt;STATUS&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Updated status: `ACTIVE` or `DISABLED`. | `DISABLED` |
| `&lt;SIGNUP_MESSAGE&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Updated pre-consent screen description. Must be 1-300 characters. | `Subscribe for weekly deals!` |
| `&lt;CONFIRMATION_MESSAGE&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Updated post-opt-in message. Supports `&#123;&#123;promo_code&#125;&#125;`. Must be 1-300 characters. | `Welcome! Use code &#123;&#123;promo_code&#125;&#125; for 20% off.` |
| `&lt;PROMO_CODE&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Updated promotional code value. Must contain only alphanumeric characters (letters and numbers) and be 1-50 characters. | `SAVE20` |
| `&lt;DISPLAY_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Updated business-facing nickname for the signup link. Must be 1-256 characters. | `Summer Sale Signup` |
| `&lt;WEBSITE_URL&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Updated business website URL. | `https://example-business.com` |

The `privacy_policy_url` field is immutable and cannot be updated.

### Disable a signup

To deactivate a signup deep link, set `status` to `DISABLED`. WhatsApp users who click a disabled deep link see an error. You can reactivate the signup at any time by setting `status` back to `ACTIVE`.

There is no delete endpoint; setting `status` to `DISABLED` is the only way to stop a signup.

### Example request: disable a signup

```html
curl &#039;https://graph.facebook.com/v25.0/signups/9876543210123456&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039; \
-d &#039;
&#123;
  &quot;status&quot;: &quot;DISABLED&quot;
&#125;&#039;
```

### Example request: update message and promo code

```html
curl &#039;https://graph.facebook.com/v25.0/signups/9876543210123456&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039; \
-d &#039;
&#123;
  &quot;confirmation_message&quot;: &quot;Welcome! Use code &#123;&#123;promo_code&#125;&#125; for 20% off your first order.&quot;,
  &quot;promo_code&quot;: &quot;SAVE20&quot;
&#125;&#039;
```

### Example response

A successful update returns:

```json
&#123;
  &quot;success&quot;: true
&#125;
```

## Error codes

| HTTP status | Error code | Description | Operations |
| --- | --- | --- | --- |
| `400` | `2494164`&lt;br&gt;&lt;br&gt;SIGNUP_NOT_FOUND | The specified signup entity does not exist. | Get, Update |
| `400` | `2494165`&lt;br&gt;&lt;br&gt;SIGNUPS_API_NOT_AVAILABLE | The In-App Signup API is not enabled for this WABA. | Create |
| `400` | `2494166`&lt;br&gt;&lt;br&gt;SIGNUP_UNKNOWN_PLACEHOLDER | The `confirmation_message` contains an unknown placeholder. | Create, Update |
| `400` | `2494167`&lt;br&gt;&lt;br&gt;SIGNUP_MISSING_PLACEHOLDER_VALUE | The `confirmation_message` uses the `&#123;&#123;promo_code&#125;&#125;` placeholder but no `promo_code` value was provided. | Create, Update |
| `400` | `2494168`&lt;br&gt;&lt;br&gt;SIGNUP_TOS_NOT_ACCEPTED | The Terms of Service were not accepted. | Create |
| `400` | `2494177`&lt;br&gt;&lt;br&gt;SIGNUP_TOS_URL_NOT_ALLOWED | The URL provided in `policy.tos` is not an approved Terms of Service URL. Use the URL shown in the [Terms of Service](#terms-of-service) section. | Create |
| `400` | `2494176`&lt;br&gt;&lt;br&gt;SIGNUP_TOS_ALREADY_ACCEPTED | The business has already accepted the Terms of Service, but the request still included the `policy` object. Omit the `policy` object on subsequent create requests. | Create |
| `400` | `2494179`&lt;br&gt;&lt;br&gt;SIGNUP_WEBSITE_URL_SCHEME_NOT_ALLOWED | The `website_url` does not use the `https://` scheme. | Create, Update |
| `403` | — | Missing the `whatsapp_business_management` permission. | All |

## Messaging customer bases

When WhatsApp users opt in through a signup deep link, they are added to a messaging customer base. A default messaging customer base is created automatically when you create your first signup deep link. Use the following endpoints to create additional messaging customer bases, set the default for your WABA, and check which one is active.

## Create a messaging customer base

Use the Messaging Customer Base API to create a new messaging customer base under your business.

### Request syntax

```html
curl &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;BUSINESS_ID&gt;/messaging_customer_base&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039; \
-d &#039;
&#123;
  &quot;messaging_customer_base_name&quot;: &quot;&lt;MESSAGING_CUSTOMER_BASE_NAME&gt;&quot;
&#125;&#039;
```

### Request parameters

| Placeholder | Description | Example value |
| --- | --- | --- |
| `&lt;ACCESS_TOKEN&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;[System token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#system-user-access-tokens) or [business token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#business-integration-system-user-access-tokens). | `EAAA...` |
| `&lt;API_VERSION&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Graph API version. | v25.0 |
| `&lt;BUSINESS_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Your Meta Business ID. | `109876543210` |
| `&lt;MESSAGING_CUSTOMER_BASE_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;A name for the messaging customer base. | `Summer 2026 Subscribers` |

### Example response

```json
&#123;
  &quot;messaging_customer_base_id&quot;: &quot;456789012345678&quot;
&#125;
```

## List messaging customer bases

Use the Messaging Customer Base API to list all messaging customer bases under your business.

### Request syntax

```html
curl &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;BUSINESS_ID&gt;/messaging_customer_base&#039; \
-H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039;
```

### Example response

```json
&#123;
  &quot;messaging_customer_bases&quot;: [
    &#123;
      &quot;id&quot;: &quot;456789012345678&quot;,
      &quot;name&quot;: &quot;Summer 2026 Subscribers&quot;
    &#125;
  ]
&#125;
```

## Set the default messaging customer base for a WABA

Use the Default Messaging Customer Base API to set or update the default messaging customer base for your WABA. Signups associated with this WABA route new subscribers into the default messaging customer base.

### Request syntax

```html
curl &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;WABA_ID&gt;/default_messaging_customer_base&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039; \
-d &#039;
&#123;
  &quot;messaging_customer_base_id&quot;: &quot;&lt;MESSAGING_CUSTOMER_BASE_ID&gt;&quot;
&#125;&#039;
```

### Request parameters

| Placeholder | Description | Example value |
| --- | --- | --- |
| `&lt;ACCESS_TOKEN&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;[System token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#system-user-access-tokens) or [business token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#business-integration-system-user-access-tokens). | `EAAA...` |
| `&lt;API_VERSION&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Graph API version. | v25.0 |
| `&lt;WABA_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Your WhatsApp Business account ID. | `102290129340398` |
| `&lt;MESSAGING_CUSTOMER_BASE_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;The messaging customer base ID to set as default. | `456789012345678` |

### Example response

```json
&#123;
  &quot;default_messaging_customer_base_id&quot;: &quot;456789012345678&quot;,
  &quot;updated_time&quot;: &quot;2026-06-03T19:20:00+0000&quot;
&#125;
```

## Get the default messaging customer base for a WABA

Use the Default Messaging Customer Base API to retrieve the default messaging customer base for your WABA.

### Request syntax

```html
curl &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;WABA_ID&gt;/default_messaging_customer_base&#039; \
-H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039;
```

### Example response

```json
&#123;
  &quot;default_messaging_customer_base_id&quot;: &quot;456789012345678&quot;,
  &quot;updated_time&quot;: &quot;2026-06-03T19:20:00+0000&quot;
&#125;
```
