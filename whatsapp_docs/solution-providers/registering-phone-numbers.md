# Registering business phone numbers



**Warning:** **Embedded signup v2 will be deprecated on October 15, 2026.** Migrate your integration to [v4](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/version-4) before that date to avoid disruption. See [Versions](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/versions) for the full upgrade path.

This document describes the steps to programmatically register business phone numbers on WhatsApp Business Accounts (WABA).

**Embedded Signup performs steps 1-3 automatically** (unless you are [bypassing the phone number addition screen](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/bypass-phone-addition)) so you only need to perform step 4 when a client completes the flow. If you have disabled phone number selection, however, you must perform all 4 steps.

Registering business phone numbers is a four-step process:

1. Create the number on a WABA.
1. Get a verification code for that number.
1. Use the code to verify the number.
1. Register the verified number for API use.

The following sections describe each step.

You can also perform all 4 steps repeatedly to register business phone numbers in bulk.

## Limitations

Business phone numbers must meet our [phone number requirements](https://developers.facebook.com/docs/whatsapp/phone-numbers#requirements).

## Step 1: Create the phone number

Use the [Phone Numbers API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/phone-number-management-api#post-version-waba-id-phone-numbers) to create a business phone number on a WABA.

### Request syntax

```https
POST /&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;/phone_numbers
```

### Post body

```json
&#123;
  &quot;cc&quot;: &quot;&lt;CC&gt;&quot;,
  &quot;phone_number&quot;: &quot;&lt;PHONE_NUMBER&gt;&quot;,
  &quot;verified_name&quot;: &quot;&lt;VERIFIED_NAME&gt;&quot;
&#125;
```

### Body properties

| Placeholder | Description | Example Value |
| --- | --- | --- |
| `&lt;CC&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;The phone number&#039;s country calling code. | `1` |
| `&lt;PHONE_NUMBER&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;The phone number, with or without the country calling code. | `15551234` |
| `&lt;VERIFIED_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;The phone number&#039;s [display name](https://www.facebook.com/business/help/338047025165344). | `Lucky Shrub` |

### Response

Upon success, the API returns a business phone number ID. Capture this ID for use in the next step.

```json
&#123;
  &quot;id&quot;: &quot;&lt;ID&gt;&quot;
&#125;
```

### Response properties

| Placeholder | Description | Example Value |
| --- | --- | --- |
| `&lt;ID&gt;` | An unverified [WhatsApp Business Phone Number](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/whatsapp-business-account-phone-number-api) ID. | `106540352242922` |

### Example request

```curl
curl &#039;https://graph.facebook.com/v25.0/102290129340398/phone_numbers&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer EAAH7...&#039; \
-d &#039;&#123;
    &quot;cc&quot;: &quot;1&quot;,
    &quot;phone_number&quot;: &quot;14195551518&quot;,
    &quot;verified_name&quot;: &quot;Lucky Shrub&quot;
&#125;&#039;
```

### Example response

```json
&#123;
  &quot;id&quot;: &quot;110200345501442&quot;
&#125;
```

## Step 2: Request a verification code

Use the [Request Code API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-pre-verified-phone-number/request-verification-code-api#post-version-pre-verified-phone-number-id-request-code) to have a verification code sent to the business phone number.

### Request syntax

```https
POST /&lt;WHATSAPP_BUSINESS_PHONE_NUMBER_ID&gt;/request_code
  ?code_method=&lt;CODE_METHOD&gt;
  &amp;language=&lt;LANGUAGE&gt;
```

### Query string parameters

| Placeholder | Description | Example Value |
| --- | --- | --- |
| `&lt;CODE_METHOD&gt;` | **Required.**&lt;br&gt;&lt;br&gt;Indicates how you want the verification code delivered to the business phone number. Values can be `SMS` or `VOICE`. | `SMS` |
| `&lt;LANGUAGE&gt;` | **Required.**&lt;br&gt;&lt;br&gt;Indicates language used in delivered verification code. | `en_US` |

### Response

```json
&#123;
  &quot;success&quot;: &lt;SUCCESS&gt;
&#125;
```

### Response properties

| Placeholder | Description | Example Value |
| --- | --- | --- |
| `&lt;SUCCESS&gt;` | Boolean indicating success or failure.&lt;br&gt;&lt;br&gt;Upon success, the API responds with `true` and sends a verification code to the business phone number using the method specified in your request. | `true` |

### Example request

```curl
curl -X POST &#039;https://graph.facebook.com/v25.0/110200345501442/request_code?code_method=SMS&amp;language=en_US&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039;
```

### Example response

```json
&#123;
  &quot;success&quot;: true
&#125;
```

### Example SMS delivery

Example of an SMS message in English containing a verification code, delivered to a business phone number:

```json
WhatsApp code 123-830
```

## Step 3: Verify the number

Use the [Verify Code API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/verify-code-api#post-version-phone-number-id-verify-code) to verify the business phone number, using the verification code contained in the SMS or voice message delivered to the number.

### Request syntax

```https
POST /&lt;WHATSAPP_BUSINESS_PHONE_NUMBER_ID&gt;/verify_code
  ?code=&lt;CODE&gt;
```

### Query string parameters

| Placeholder | Description | Example Value |
| --- | --- | --- |
| `&lt;CODE&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Verification code, without the hyphen. | `123830` |

### Response

```json
&#123;
  &quot;success&quot;: &lt;SUCCESS&gt;
&#125;
```

### Response properties

| Placeholder | Description | Example Value |
| --- | --- | --- |
| `&lt;SUCCESS&gt;` | Boolean indicating success or failure.&lt;br&gt;&lt;br&gt;Upon success, the API responds with `true`, indicating that the business phone number has been verified. | `true` |

### Example request

```curl
curl -X POST &#039;https://graph.facebook.com/v25.0/110200345501442/verify_code?code=123830&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039;
```

### Example response

```json
&#123;
  &quot;success&quot;: true
&#125;
```

## Step 4: Register the number

Use the [Register API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/register-api#post-version-phone-number-id-register) to register the business phone number for use with the API.

### Request syntax

```https
POST /&lt;WHATSAPP_BUSINESS_PHONE_NUMBER_ID&gt;/register
```

### Post body

```json
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;pin&quot;: &quot;&lt;PIN&gt;&quot;
&#125;
```

### Body properties

| Placeholder | Description | Example Value |
| --- | --- | --- |
| `&lt;PIN&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;If the verified business phone number already has two-step verification enabled, set this value to the number&#039;s 6-digit two-step verification PIN. If you do not recall the PIN, you can [update](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-phone-numbers/two-step-verification#updating-verification-code) it.&lt;br&gt;&lt;br&gt;If the verified business phone number does not have two-step verification enabled, set this value to a 6-digit number. This 6-digit number becomes the business phone number&#039;s two-step verification PIN. | `123456` |

### Response

Upon success, the API responds with `true`, indicating successful registration.

```json
&#123;
  &quot;success&quot;: &lt;SUCCESS&gt;
&#125;
```

### Response properties

| Placeholder | Description | Example Value |
| --- | --- | --- |
| `&lt;SUCCESS&gt;` | Boolean indicating success or failure.&lt;br&gt;&lt;br&gt;Upon success, the API responds with `true`, indicating successful registration. | `true` |

### Example request

```curl
curl &#039;https://graph.facebook.com/v25.0/110200345501442/register&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039; \
-d &#039;
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;pin&quot;: &quot;123456&quot;
&#125;&#039;
```

### Example response

```json
&#123;
  &quot;success&quot;: true
&#125;
```
