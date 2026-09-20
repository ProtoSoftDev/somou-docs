# Pre-verified phone numbers



**Warning:** **Embedded signup v2 will be deprecated on October 15, 2026.** Migrate your integration to [v4](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/version-4) before that date to avoid disruption. See [Versions](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/versions) for the full upgrade path.

This document explains how to offer your business customers pre-verified phone numbers. A pre-verified phone number is a WhatsApp Business phone number that you have already verified. Pre-verifying a number eliminates the need for customers to contact you for a one-time password.

Note that [WhatsApp Business Pre-Verified Phone Number](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/business/whatsapp-business-pre-verified-phone-numbers-api) objects represent pre-verified numbers. WhatsApp Business Pre-Verified Phone Number objects are **temporary**. When a business customer selects one of these numbers and completes the Embedded Signup flow, WhatsApp replaces the temporary object with a valid [WhatsApp Business Phone Number](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/whatsapp-business-account-phone-number-api) object. You must [get this new object&#039;s ID](#getting-and-registering-claimed-phone-numbers) and use it to [register the number](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/registering-phone-numbers#step-4--register-the-number) within 90 days.

## Requirements

* Your business must be an approved Solution Partner.
* The app user must be a business admin on the business account that pre-verified business phone numbers are added to.
* A [User](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#user-access-tokens) or [System User access token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#system-user-access-tokens).
* The [business_management](https://developers.facebook.com/docs/permissions/reference/business_management) permission.
* Business phone numbers [must be valid](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-phone-numbers/phone-numbers).

## Limitations

* You are responsible for keeping track of who has claimed a pre-verified business phone number.
* If an end client does not claim a pre-verified business phone number in the Embedded Signup flow within 90 days of verification, the number will revert to an unverified status and you must verify it again to restore its status for another 90 days.
* Unclaimed pre-verified business phone numbers can&#039;t be re-verified until 45 days before they are scheduled to revert to an unverified status. The [`verification_expiry_time` field](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/business/whatsapp-business-pre-verified-phone-numbers-api#fields) indicates this time.
* If you add a phone number to your pool of pre-verified business phone numbers (Step 1) but do not verify it within 90 days (Step 3), WhatsApp removes it from your pool and you have to add it again.
* Once a business customer claims a pre-verified business phone number, you have 90 days to register it.

## Creating pre-verified numbers

Follow these steps to create a pre-verified business phone number, surface it in Embedded Signup, and register it after it has been claimed by a business customer.

### Step 1: Create a pre-verified business phone number

Use the [Add Phone Numbers API](https://developers.facebook.com/documentation/ads-commerce/marketing-api/reference/business/add_phone_numbers) to add a pre-verified business phone number to your business portfolio&#039;s pool of business phone numbers.

#### Request syntax

```http
POST /&lt;BUSINESS_PORTFOLIO_ID&gt;/add_phone_numbers
  ?phone_number=&lt;PHONE_NUMBER&gt;
```

#### Response

Upon success, the API will return a [WhatsApp Business Pre-Verified Phone Number](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/business/whatsapp-business-pre-verified-phone-numbers-api) ID. Capture this value for use in the next request.

```json
&#123;
  &quot;id&quot;: &quot;&lt;WHATSAPP_BUSINESS_PRE_VERIFIED_PHONE_NUMBER_ID&gt;&quot;
&#125;
```

#### Sample request

```http
curl -X POST &#039;https://graph.facebook.com/v25.0/506914307656634/add_phone_numbers?phone_number=15550783881&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039;
```

#### Sample response

```json
&#123;
  &quot;id&quot;: &quot;106540352242922&quot;
&#125;
```

### Step 2: Request a verification code

Use the [Request Verification Code API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-pre-verified-phone-number/request-verification-code-api#post-version-pre-verified-phone-number-id-request-code) to request a one-time password over SMS or voice for the newly created pre-verified number. Use the `WHATSAPP_BUSINESS_PRE_VERIFIED_PHONE_NUMBER_ID` returned in the Step 1 response as the path parameter.

#### Request syntax

```http
POST /&lt;WHATSAPP_BUSINESS_PRE_VERIFIED_PHONE_NUMBER_ID&gt;/request_code
  ?code_method=&lt;CODE_METHOD&gt;
  &amp;language=&lt;LANGUAGE&gt;
```

#### Response

Upon success, the API will return `true`.

```json
&#123;
  &quot;success&quot;: &lt;SUCCESS&gt;
&#125;
```

In addition, WhatsApp sends an SMS or voice message containing a one-time password to the phone number. Capture the one-time password for use in the next request.

#### One-time-password SMS syntax

```json
WhatsApp code &lt;CODE&gt;
```

#### One-time-password voice message syntax

Repeated three times.

```json
Verification code is &lt;CODE&gt;
```

#### Sample request

```http
curl -X POST &#039;https://graph.facebook.com/v25.0/106540352242922/request_code?code_method=SMS&amp;language=en_US&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039;
```

#### Sample response

```json
&#123;
  &quot;success&quot;: true
&#125;
```

#### Sample one-time-password SMS message

```json
WhatsApp code 123-456
```

#### Sample one-time-password voice message

Repeated three times.

```json
Verification code is 123456
```

### Step 3: Verify the number

Use the [Verify Code API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-pre-verified-phone-number/verify-code-api#post-version-phone-number-id-verify-code) to verify the number using its one-time-password.

#### Request syntax

```http
POST /&lt;WHATSAPP_BUSINESS_PRE_VERIFIED_PHONE_NUMBER_ID&gt;/verify_code
  ?code=&lt;CODE&gt;
```

#### Response

Upon success, the API will return `true` and the number will have its `code_verification_status` set to `VERIFIED` for 90 days.

```json
&#123;
  &quot;success&quot;: &lt;SUCCESS&gt;
&#125;
```

#### Sample request

```http
curl -X POST &#039;https://graph.facebook.com/v25.0/106540352242922/verify_code?code=123456&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039;
```

#### Sample response

```json
&#123;
  &quot;success&quot;: true
&#125;
```

Once you have a pre-verified business phone number with a verified status (or a set of such numbers), display them in the new Embedded Signup flow.

## Displaying pre-verified numbers in Embedded Signup

You can display pre-verified business phone numbers in the Embedded Signup flow using [pre-filled form data](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/pre-filled-data). To do this, add a `preVerifiedPhone` object with an `ids` property to the `setup` object and assign the IDs of your pre-verified business phone numbers as an array of strings to the `ids` property:

```js
&#123;
  scope: &quot;&lt;SCOPE&gt;&quot;,
  extras: &#123;
    feature: &quot;&lt;FEATURE&gt;&quot;,
    setup: &#123;
      preVerifiedPhone: &#123;
        ids: [&lt;IDS&gt;]
      &#125;
    &#125;
  &#125;
&#125;
```

For example:

```js
&#123;
  scope: &quot;business_management,whatsapp_business_management&quot;,
  extras: &#123;
    feature: &quot;whatsapp_embedded_signup&quot;,
    version: 2,
    setup: &#123;
  business: &#123;
    name: &quot;Acme Inc.&quot;,
    email: &quot;johndoe&#064;acme.com&quot;,
    phone: &#123;
      code: 1,
      number: &quot;6505551234&quot;
        &#125;,
    website: &quot;https://www.acme.com&quot;,
        address: &#123;
          streetAddress1: &quot;1 Acme Way&quot;,
          city: &quot;Acme Town&quot;,
          state: &quot;CA&quot;,
          zipPostal: &quot;94000&quot;,
          country: &quot;US&quot;
        &#125;,
        timezone: &quot;UTC-08:00&quot;
      &#125;,
      phone: &#123;
        displayName: &quot;Acme Inc.&quot;,
        category: &quot;ENTERTAIN&quot;,
        description: &quot;Gears and widgets&quot;
      &#125;,
      preVerifiedPhone: &#123;
        ids: [&quot;106540352242922&quot;,&quot;105954558954427&quot;]
      &#125;
    &#125;
  &#125;
&#125;
```

Note that if no business customer claims a pre-verified business phone number with a status of `VERIFIED` within 90 days of verification, WhatsApp sets its status to `UNVERIFIED` but it will still appear in the Embedded Signup flow. If a business customer attempts to claim an unverified number, they must complete verification on their own, which means they must request a one-time password from you.

To prevent this experience, **keep track of when you verified a number and re-verify it before it reverts to an unverified state.**

If you don&#039;t know when you last verified a given pre-verified business phone number, request the `code_verification_time` and `verification_expiry_time` fields on the pre-verified business phone number ID. These fields indicate its most recent verification time and its verification expiration time.

## Determining if a number has been claimed through Embedded Signup

See [Getting claimed phone number IDs](#getting-and-registering-claimed-phone-numbers).

## Getting and registering claimed phone numbers

Once a business customer claims a pre-verified business phone number, WhatsApp replaces it with a verified WhatsApp Business phone number (a [WhatsApp Business Phone Number](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/whatsapp-business-account-phone-number-api#get-version-phone-number-id) object with a `code_verification_status` set to `VERIFIED`).

You will have 90 days to [register this number](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/registering-phone-numbers#step-4--register-the-number) using its ID. If you do not register it within this time frame, it will revert to an `UNVERIFIED` status and you will have to [request a new verification code](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/registering-phone-numbers#step-2--request-a-verification-code) and use the code to [verify the WhatsApp Business phone number](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/registering-phone-numbers#step-3--verify-the-number) again.

### Getting claimed numbers via session logging

If you are using [session logging](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/implementation#session-logging-message-event-listener), the ID will be returned in a message event and captured by your event listener. Send this ID to your server and then use it to register the WhatsApp Business phone number.

### Getting claimed numbers via API

If you are not using session logging, use the [Phone Numbers API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/phone-number-management-api) to get a list of WhatsApp Business phone numbers on the WhatsApp Business account.

Parse for the `display_phone_number` property on each object returned in the result set. If an object in the result set has a `display_phone_number` value that matches a number you used to create a pre-verified business phone number, the object represents the WhatsApp Business phone number that has replaced the pre-verified business phone number. Copy this object&#039;s ID and use it to register the WhatsApp Business phone number.

Alternatively, you can use the same endpoint with `field` expansion to request the `display_phone_number` field and specify the display phone number. For example:

```http
GET /102290129340398/phone_numbers?display_phone_number=16505551234
```

## Get pre-verified business phone numbers

Use the [Preverified Numbers API](https://developers.facebook.com/documentation/ads-commerce/marketing-api/reference/business/preverified_numbers) to get a list of all [WhatsApp Business Pre-Verified Phone Number](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/business/whatsapp-business-pre-verified-phone-numbers-api) objects, regardless of their verification status, in your business account&#039;s pool of pre-verified business phone numbers:

```http
GET /&lt;BUSINESS_ACCOUNT_ID&gt;/preverified_numbers
```

The API automatically sorts results in order of creation time. You can also use field expansion to request the `code_verification_status` field to have the API only return pre-verified business phone numbers with the indicated verification state:

```http
GET /&lt;BUSINESS_ACCOUNT_ID&gt;/preverified_numbers?code_verification_status=VERIFIED
```

## Sharing and unsharing pre-verified numbers

Use the [Share Preverified Numbers API](https://developers.facebook.com/documentation/ads-commerce/marketing-api/reference/business/share_preverified_numbers) to share pre-verified business phone numbers with a [multi-partner solution](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/multi-partner-solutions) you are a part of, or a DELETE request to the same endpoint to unshare them.

Partners of a solution can surface shared pre-verified business phone numbers in their implementation of Embedded Signup.

If you are sharing numbers with multiple business partners, advise your partners to [get a list of shared pre-verified numbers](#get-pre-verified-business-phone-numbers) before surfacing them in Embedded Signup. This reduces the likelihood of a partner attempting to surface a number that has already been claimed (claimed numbers do not appear in the flow, but the partner might not know this and wonder why it&#039;s not appearing).

### Sharing request syntax

```http
POST /&lt;BUSINESS_ID&gt;/share_preverified_numbers
  ?partner_business_id=&lt;PARTNER_BUSINESS_ID&gt;
  &amp;preverified_id=&lt;PREVERIFIED_ID&gt;
```

### Unsharing request syntax

```http
DELETE /&lt;BUSINESS_ID&gt;/share_preverified_numbers
  ?partner_business_id=&lt;PARTNER_BUSINESS_ID&gt;
  &amp;preverified_id=&lt;PREVERIFIED_ID&gt;
```

### Response

Upon success, the API will return `true`. If sharing, notify your business partner of the newly shared pre-verified number and provide them with the number&#039;s ID. If unsharing, the number will no longer appear in the partner&#039;s implementation of Embedded Signup.

```json
&#123;
  &quot;success&quot;: &lt;SUCCESS&gt;
&#125;
```

### Example sharing request

```http
curl -X POST &#039;https://graph.facebook.com/v17.0/share_preverified_numbers?partner_business_id=506914307656634&amp;preverified_id=1706193509821738&#039; \
-H &#039;Authorization: Bearer EAAH0...&#039;
```

### Example unsharing request

```http
curl -X DELETE &#039;https://graph.facebook.com/v17.0/share_preverified_numbers?partner_business_id=506914307656634&amp;preverified_id=1706193509821738&#039; \
-H &#039;Authorization: Bearer EAAH0...&#039;
```

### Example response

```json
&#123;
  &quot;success&quot;: true
&#125;
```

## Registering pre-verified numbers programmatically

If you have customized Embedded Signup to [bypass the phone number addition screen](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/bypass-phone-addition), you can register pre-verified business phone numbers on an onboarded business customer&#039;s WhatsApp Business account programmatically. To do this, first complete all of the steps to [create a pre-verified number](#creating-pre-verified-numbers), then use the pre-verified number ID to complete **Step 1** and **Step 4** in the [Register Phone Numbers](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/registering-phone-numbers) document.
