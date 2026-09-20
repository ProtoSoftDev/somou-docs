# Local storage



Local storage offers an additional layer of data management control, by giving you the option to specify where your message data is stored at rest. If your company is in a regulated industry such as finance, government, or healthcare, you may prefer to have your message data stored in a specific country when at rest because of regulatory or company policies.

## How local storage works

A setting at the WhatsApp Business phone number level controls local storage. Both Cloud API and Marketing Messages API for WhatsApp support local storage, and the setting applies to any messages sent via either API if enabled.

When local storage is enabled, the following constraints apply to message content for a business phone number:

- **Data-in-use:** When message content is sent or received by Cloud API or Marketing Messages API for WhatsApp, message content may be stored on Meta data centers internationally while being processed. The data-in-use period differs between Cloud API and Marketing Messages API for WhatsApp:
  - When using local storage for Cloud API, the data-in-use period is up to 60 minutes.
  - When using local storage for Marketing Messages API for WhatsApp, the data-in-use period is up to 90 minutes.
- **Data-at-rest:** After the data-in-use period, message content is deleted from Meta data centers outside of the specified local storage region, and persisted only in data centers within the local storage region selected.

The local storage feature supplements other [WhatsApp Business Platform privacy and security controls](https://business.whatsapp.com/trust-and-safety), and allows you to ensure a higher level of compliance with local data protection regulations.

## Data in scope

Local storage applies to message content (text and media) sent or received via Cloud API and Marketing Messages API for WhatsApp. The following message content is in scope of the local storage feature:

- Text messages: text payload (message body)
- Media messages: media payload (audio, document, image, or video)
- Template messages (static template and parameters passed at message send time): components with text or media payload

A limited set of metadata attributes is also stored with the message content. These attributes associate the encrypted message payload with the original message and provide localization audit records. The stored metadata is protected with tokenization and encryption.

## Phone number storage via contact information requests

If your business has enabled the [contact book](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-scoped-user-ids#contact-book) feature and a WhatsApp user shares their contact information by tapping the share contact information button, Meta extracts the user&#039;s phone number from the shared contact card (vCard) and stores it in your contact book. The contact book is hosted on Meta data centers, regardless of your local storage configuration. Only the phone number is extracted and stored; no other vCard data is retained on Meta data centers beyond the standard data-in-use period. Contact book retention policies apply to this data, and you can disable the contact book feature from that page.

## Available regions

To see what regions are supported by local storage, see the `data_localization_region` parameter in the [documentation on phone number registration](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-phone-numbers/registration#register).

## Limitations

- Media files uploaded by a business phone number with local storage enabled are only accessible to that specific phone number, and cannot be shared with other phone numbers associated with the business.
- If your business has the contact book feature enabled, phone numbers shared via the share contact information button are extracted from the vCard and stored in your contact book on Meta data centers, regardless of your local storage configuration. See [Phone number storage via contact information requests](#phone-number-storage-via-contact-information-requests) for details.

## Enable local storage

Follow the steps below to enable local storage for an unregistered business phone number using API version 21.0 or newer. If you are using an older API version, see [Enabling local storage (v20 and older)](#enabling-local-storage-using-v20-and-older).

### Step 1: Enable local storage on the number

Local storage can only be enabled or disabled on WhatsApp Business phone numbers when they are in an unregistered state. If a phone number is registered, it must be unregistered and re-registered with local storage enabled.

Use the [**Settings API**](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/settings-api#post-version-phone-number-id-settings) to enable local storage on the unregistered business phone number:

#### Request syntax

```html
curl &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;WHATSAPP_BUSINESS_PHONE_NUMBER_ID&gt;/settings&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039; \
-d &#039;
&#123;
  &quot;storage_configuration&quot;: &#123;
    &quot;status&quot;: &quot;IN_COUNTRY_STORAGE_ENABLED&quot;,
    &quot;data_localization_region&quot;: &quot;&lt;COUNTRY_CODE&gt;&quot;
  &#125;
&#125;&#039;
```

Set `&lt;COUNTRY_CODE&gt;` to the [country code](#available-regions) of the country where data-at-rest should be stored.

#### Example request

```curl
curl &#039;https://graph.facebook.com/v25.0/106540352242922/settings&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039; \
-d &#039;
&#123;
  &quot;storage_configuration&quot;: &#123;
    &quot;status&quot;: &quot;IN_COUNTRY_STORAGE_ENABLED&quot;,
    &quot;data_localization_region&quot;: &quot;BR&quot;
  &#125;
&#125;&#039;
```

#### Example response

```json
&#123;
  &quot;success&quot;: true
&#125;
```

### Step 2: Register the number

Use the [**Register API**](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/register-api#post-version-phone-number-id-register) to register the business phone number.

#### Request syntax

```html
curl &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;WHATSAPP_BUSINESS_PHONE_NUMBER_ID&gt;/register&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039; \
-d &#039;
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;pin&quot;: &quot;&lt;TWO_STEP_PIN&gt;&quot;
&#125;&#039;
```

Set `&lt;TWO_STEP_PIN&gt;` to the desired two-step verification PIN for the business phone number.

#### Example request

```curl
curl &#039;https://graph.facebook.com/v25.0/106540352242922/register&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039; \
-d &#039;
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;pin&quot;: &quot;123456&quot;
&#125;&#039;
```

#### Example response

```json
&#123;
  &quot;success&quot;: true
&#125;
```

## Get local storage settings

Use the [**Settings API**](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/settings-api#get-version-phone-number-id-settings) to get local storage settings on a WhatsApp Business phone number. For example:

```curl
curl &#039;https://graph.facebook.com/v25.0/179776755229976/settings&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039;
```

This request returns a [node that represents the local storage settings](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/settings-api) on the business phone number. For example:

```json
&#123;
  &quot;storage_configuration&quot;: &#123;
    &quot;status&quot;: &quot;IN_COUNTRY_STORAGE_ENABLED&quot;,
    &quot;data_localization_region&quot;: &quot;BR&quot;
  &#125;
&#125;
```

## Disable local storage

Use the [**Settings API**](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/settings-api#post-version-phone-number-id-settings) to disable local storage on an unregistered business phone number using API version 21.0 or newer. If you are using an older API version, see [Disabling local storage (v20 and older)](#disabling-local-storage-using-v20-and-older).

#### Request syntax

```html
curl &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;WHATSAPP_BUSINESS_PHONE_NUMBER_ID&gt;/settings&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039; \
-d &#039;
&#123;
  &quot;storage_configuration&quot;: &#123;
    &quot;status&quot;: &quot;IN_COUNTRY_STORAGE_DISABLED&quot;
  &#125;
&#125;&#039;
```

#### Example request

```curl
curl &#039;https://graph.facebook.com/v25.0/106540352242922/settings&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039; \
-d &#039;
&#123;
  &quot;storage_configuration&quot;: &#123;
    &quot;status&quot;: &quot;IN_COUNTRY_STORAGE_DISABLED&quot;
  &#125;
&#125;&#039;
```

#### Example response

```json
&#123;
  &quot;success&quot;: true
&#125;
```

## Enable local storage using v20 and older

To enable local storage for an unregistered business phone number using API version 20.0 or older:

### Step 1: Check verification status

Use the [**WhatsApp Business Phone Number API**](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/whatsapp-business-account-phone-number-api#get-version-phone-number-id) and request the `code_verification_status` field. If the code verification status is `VERIFIED`, skip to step 4. Otherwise, proceed to step 2.

### Step 2: Request a verification code

Use the [**Request Verification Code API**](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-pre-verified-phone-number/request-verification-code-api#post-version-pre-verified-phone-number-id-request-code) to request a verification code. Upon success, the API responds with `true` and sends a verification code to the business phone number via the method specified in the `code_method` parameter.

For example, this query requests a verification code to be sent via SMS in the English language (US locale).

```curl
curl -X POST &#039;https://graph.facebook.com/v25.0/110200345501442/request_code?code_method=SMS&amp;language=en_US&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039;
```

Use the code in the delivered message in the next step.

### Step 3: Verify the business phone number

Use the [**Verify Code API**](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/verify-code-api#post-version-phone-number-id-verify-code) to verify the business phone number using the verification code included in the message you received from the previous step.

For example:

```curl
curl -X POST &#039;https://graph.facebook.com/v25.0/110200345501442/verify_code?code=123830&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039;
```

### Step 4: Reregister the business phone number

Use the [**Register API**](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/register-api#post-version-phone-number-id-register) to register the business phone number. Indicate the country where data-at-rest should be stored using the `data_localization_region` parameter.

For example, this request enables local storage on a business phone number, and sets the country where data should be stored to India:

```curl
curl &#039;https://graph.facebook.com/v25.0/110200345501442/register&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039; \
-d &#039;
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;pin&quot;: &quot;123456&quot;,
  &quot;data_localization_region&quot;: &quot;IN&quot;
&#125;&#039;
```

## Disable local storage using v20 and older

Use the [**Deregister API**](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/phone-number-deregister-api#post-version-phone-number-id-deregister) to disable local storage on a business phone number using API version 20.0 or older.

For example:

```curl
curl -X POST &#039;https://graph.facebook.com/v25.0/110200345501442/deregister&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039;
```

Note that this deregisters the business phone number so it cannot be used with WhatsApp Cloud API. If you want to continue using it with Cloud API but without local storage enabled, you must reregister it without including the `data_localization_region` parameter.

## FAQs

**Q. What are the migration paths for moving a phone number to the Cloud API version with local storage?**

All migration paths to Cloud API version with local storage are supported, including:

* Existing Cloud API number migrating to Cloud API version with local storage
* New Cloud API number enabling local storage

In all these scenarios, use the [Register API](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-phone-numbers/registration#register) to register the selected phone number, specifying the target country for data localization in the `data_localization_region` parameter.

**Q. Are there any migration risks? Any downtime associated with this?**

No migration risks. Downtime is typically less than five minutes and you don&#039;t need to re-verify the business phone number.

**Q. Is there any downtime associated with enabling local storage for a business phone number?**

If a business phone number is already registered, you need to deregister and re-register the phone number with local storage enabled. This process typically takes less than five minutes. You don&#039;t need to re-verify the business phone number during this process.
