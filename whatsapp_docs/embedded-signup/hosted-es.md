# Hosted Embedded Signup



You can implement Embedded Signup without adding JavaScript code to your website or customer portal. Instead, use a link that displays a web page describing the onboarding steps. The page includes a button that launches the Embedded Signup flow:

## Limitations

Hosted Embedded Signup (&quot;Hosted ES&quot;) can only be used to onboard business customers to Cloud API, and the flow cannot be customized.

## Requirements

- You must have completed the steps to become a Solution Partner or Tech Provider.
- If your app is for messaging, it must be able to send messages, manage templates, and have a properly configured production webhook endpoint.
- Your app must be subscribed to the [account_update](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/account_update) webhook.
- Solution Partners must have a line of credit.

You will also need:

- Your [system token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#system-user-access-tokens).
- Your app secret.

## Step 1: Create a Facebook Login for Business configuration

If you don&#039;t already have a Facebook Login for Business configuration, you must create one. A Facebook Login for Business configuration defines which permissions to request, and what additional information to collect, from business customers who access Embedded Signup.

Navigate to **Facebook Login for Business** &gt; **Configurations** and click the **+ Create configuration** button to access the configuration flow.

Use a name that will help you differentiate this configuration from any others you may create in the future. When completing the flow, be sure to select the WhatsApp Embedded Signup login variation:

When choosing assets and permissions, select only those assets and permissions that you will actually need from your business customers.

For example, if you select the **Catalogs** asset but don&#039;t actually need access to customer catalogs, your customers will likely abandon the flow at the catalog selection screen and ask you for clarification.

## Step 2: Get the Hosted Embedded Signup URL

Navigate to the **WhatsApp** &gt; **Quickstart** panel and click the **View onboarding** button.

Locate the **Zero integration onboarding** card. The URL displayed in the card is the onboarding page URL:

Click the **Copy** button to copy the URL to your clipboard. Map this URL to a button on your website or customer portal that, when clicked, opens the URL in a new browser window.

To preview the onboarding page, load the URL in a new browser window or tab, or click the blue &quot;new window&quot; icon.

This onboarding page looks like this:

Click the **Get started** button, which launches the same flow that business customers who click the button on your website or customer portal will see. Complete the flow if you want to.

## Step 3: Capture customer asset IDs

When a business customer completes the flow, Meta sends an [account_update](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/account_update) webhook with `event` set to `PARTNER_ADDED`. Capture the customer&#039;s WhatsApp Business account ID and business portfolio ID from the webhook payload.

## Step 4: Generate an HMAC-SHA256 hash

Generate an HMAC-SHA256 hash of your app secret and system token.

### Bash example (Linux and macOS)

```html
echo -n &quot;&lt;SYSTEM_TOKEN&gt;&quot; | openssl dgst -sha256 -hmac &quot;&lt;APP_SECRET&gt;&quot;
```

- `&lt;SYSTEM_TOKEN&gt;` — Your system token.
- `&lt;APP_SECRET&gt;` — Your app secret ([**App Dashboard**](https://developers.facebook.com/apps) &gt; **App settings** &gt; **Basic**).

## Step 5: Get a business token

Use the [System User Access Tokens API](https://developers.facebook.com/documentation/ads-commerce/marketing-api/reference/business/system_user_access_tokens) to get and capture the customer&#039;s [business token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#business-integration-system-user-access-tokens). (Target the customer&#039;s business portfolio ID, not yours).

### Request syntax

```html
curl &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;BUSINESS_PORTFOLIO_ID&gt;/system_user_access_tokens&#039; \
-H &#039;Content-Type: application/x-www-form-urlencoded&#039; \
-H &#039;Authorization: Bearer &lt;SYSTEM_TOKEN&gt;&#039; \
-d &#039;appsecret_proof=&lt;APPSECRET_PROOF&gt;&#039; \
-d &#039;fetch_only=true&#039;
```

- `&lt;API_VERSION&gt;` — API version.
- `&lt;APPSECRET_PROOF&gt;` — HMAC-SHA256 hash of your app secret and system token.
- `&lt;BUSINESS_PORTFOLIO_ID&gt;` — Business customer&#039;s business portfolio ID.
- `&lt;SYSTEM_TOKEN&gt;` — Your system token.

### Response syntax

Upon success:

```html
&#123;
  &quot;access_token&quot;: &quot;&lt;BUSINESS_TOKEN&gt;&quot;
&#125;
```

- `&lt;BUSINESS_TOKEN&gt;` — The business customer&#039;s business token.

## Step 6: Get the customer&#039;s business phone number ID

Use the [Phone Numbers API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/phone-number-management-api) to get and capture the business customer&#039;s business phone number ID.

### Request syntax

```html
curl &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;/phone_numbers&#039; \
-H &#039;Authorization: Bearer &lt;BUSINESS_TOKEN&gt;&#039;
```

- `&lt;API_VERSION&gt;` — API version.
- `&lt;BUSINESS_TOKEN&gt;` — Business customer&#039;s business token.
- `&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;` — Business customer&#039;s WhatsApp Business account ID.

### Response syntax

```html
&#123;
  &quot;data&quot;: [
    &#123;
      &quot;verified_name&quot;: &quot;&lt;VERIFIED_NAME&gt;&quot;,
      &quot;code_verification_status&quot;: &quot;&lt;CODE_VERIFICATION_STATUS&gt;&quot;,
      &quot;display_phone_number&quot;: &quot;&lt;DISPLAY_PHONE_NUMBER&gt;&quot;,
      &quot;quality_rating&quot;: &quot;&lt;QUALITY_RATING&gt;&quot;,
      &quot;platform_type&quot;: &quot;&lt;PLATFORM_TYPE&gt;&quot;,
      &quot;throughput&quot;: &#123;
        &quot;level&quot;: &quot;&lt;THROUGHPUT_LEVEL&gt;&quot;
      &#125;,
      &quot;last_onboarded_time&quot;: &quot;&lt;LAST_ONBOARDED_TIME&gt;&quot;,
      &quot;webhook_configuration&quot;: &#123;
        &quot;application&quot;: &quot;&lt;WEBHOOK_CALLBACK_URL&gt;&quot;
      &#125;,
      &quot;id&quot;: &quot;&lt;BUSINESS_PHONE_NUMBER_ID&gt;&quot;
    &#125;
  ]
&#125;
```

- `&lt;BUSINESS_PHONE_NUMBER_ID&gt;` — Business phone number ID.
- `&lt;CODE_VERIFICATION_STATUS&gt;` — Business phone number verification status.
- `&lt;DISPLAY_PHONE_NUMBER&gt;` — Business display phone number.
- `&lt;LAST_ONBOARDED_TIME&gt;` — Unix timestamp indicating when the number was added to the business customer&#039;s WhatsApp Business account (essentially, when the customer successfully completed the flow).
- `&lt;PLATFORM_TYPE&gt;` — Platform.
- `&lt;QUALITY_RATING&gt;` — Business phone number quality rating.
- `&lt;THROUGHPUT_LEVEL&gt;` — Throughput level.
- `&lt;VERIFIED_NAME&gt;` — Business phone number verified name.
- `&lt;WEBHOOK_CALLBACK_URL&gt;` — Webhook callback URL associated with the number.

## Step 7: Onboard the customer

Onboard the business customer by completing the steps in the appropriate onboarding guide for your partner type:

- [Onboarding business customers as a Tech Provider or Tech Partner](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/onboarding-customers-as-a-tech-provider) (skip step 1)
- [Onboarding business customers as a Solution Partner](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/onboarding-customers-as-a-solution-partner) (skip step 1)
