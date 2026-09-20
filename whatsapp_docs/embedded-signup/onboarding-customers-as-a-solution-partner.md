# Onboarding business customers as a Solution Partner



**Warning:** **Embedded signup v2 will be deprecated on October 15, 2026.** Migrate your integration to [v4](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/version-4) before that date to avoid disruption. See [Versions](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/versions) for the full upgrade path.

This document describes the steps Solution Partners must perform to onboard new business customers who have completed the Embedded Signup flow.

If you are a Solution Partner, a business customer who completes your implementation of the Embedded Signup flow cannot immediately use your app. They cannot access their WhatsApp assets or send and receive messages until you complete these steps.

## What you will need

* the business customer&#039;s WhatsApp Business account (WABA) ID (returned via [session logging](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/implementation#session-logging-message-event-listener) or [API request](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/manage-accounts#get-shared-waba-id-with-access-token))
* the business customer&#039;s business phone number ID (returned via [session logging](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/implementation#session-logging-message-event-listener) or [API request](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/manage-phone-numbers#getting-phone-numbers))
* your app ID (displayed at the top of the **App Dashboard**)
* your app secret (displayed in the **App Dashboard** &gt; **App settings** &gt; **Basic** panel)
* your credit line ID (displayed in **Meta Business Suite** &gt; **Business Settings** &gt; **Business Info** or returned via [API request](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/share-and-revoke-credit-lines#get-your-credit-line-id))
* your [system user access token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#system-user-access-tokens) (&quot;system token&quot;)

Also, if you want to test messaging capabilities using the customer&#039;s business phone number, you will need a WhatsApp phone number that can already send and receive messages from other WhatsApp numbers.

**Note:** Perform all of the requests described below using server-to-server requests. Do not use client-side requests.

## Step 1: Exchange the token code for a business token

Use the **Access Token API** to exchange the token code [returned](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/implementation#response-callback) by Embedded Signup for a business integration system user access token (&quot;business token&quot;).

### Request

```html
curl --get &#039;https://graph.facebook.com/v21.0/oauth/access_token&#039; \
-d &#039;client_id=&lt;APP_ID&gt;&#039; \
-d &#039;client_secret=&lt;APP_SECRET&gt;&#039; \
-d &#039;code=&lt;CODE&gt;&#039;
```

### Request parameters

| Placeholder | Description | Example value |
| --- | --- | --- |
| `&lt;APP_ID&gt;` | **Required.**&lt;br&gt;&lt;br&gt;Your app ID. This is displayed at the top of the **App Dashboard**. | `236484624622562` |
| `&lt;APP_SECRET&gt;` | **Required.**&lt;br&gt;&lt;br&gt;Your app secret. You can get this from the **App Dashboard** &gt; **App Secret** &gt; **Basic** panel. | `614fc2afde15eee07a26b2fe3eaee9b9` |
| `&lt;CODE&gt;` | **Required.**&lt;br&gt;&lt;br&gt;The code [returned by Embedded Signup](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/implementation#response-callback) when the customer successfully completed the flow. | `AQBhlXsctMxJYbwbrpybxlo9tLPGy-QAmjBJA03jxLos43wxlBlrYozY5C33BXJULd133cOJf_5y6EkJZYMrAmW-EMj3Wdap9-NUM2nS4s8tC-ES7slBhh6QpCFM7-SzpI-iqsjqTGyxbUUW3AeaEyLkeZFIkBgcQ_SOxo9HShm20SDR5_n7AT9ZJ5dcgpqBQykNT-pQ8V7Ne9-sr6RLAWtJMF7-Zx6ABudRcWIN53tUTtquDVNuq3lrco4BlVQAv-54tR83Ae0ODN9Uet6j-BVLuetXhQCM3sz9RdgedlbxkidMbkztvYX1j7baOrJxyLyYGWYgbnUrKRQKCtWTsO5ekIGFgtbpS8UPJNqV6j8E5XKPJ8QA7ZFqzkB0s2O__J5FrjHzc_rDo1EuRbw98ihHDzQnvuXeHapEyfhLDJct0A` |

### Response

Upon success:

```html
&lt;BUSINESS_TOKEN&gt;
```

### Response parameters

| Placeholder | Description | Example value |
| --- | --- | --- |
| `&lt;BUSINESS_TOKEN&gt;` | The customer&#039;s [business token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#business-integration-system-user-access-tokens). | `EAAAN6tcBzAUBOwtDtTfmZCJ9n3FHpSDcDTH86ekf89XnnMZAtaitMUysPDE7LES3CXkA4MmbKCghdQeU1boHr0QZA05SShiILcoUy7ZAb2GE7hrUEpYHKLDuP2sYZCURkZCHGEvEGjScGLHzC4KDm8tq2slt4BsOQE1HHX8DzHahdT51MRDqBw0YaeZByrVFZkVAoVTxXUtuKgDDdrmJQXMnI4jqJYetsZCP1efj5ygGscZBm4OvvuCYB039ZAFlyNn` |

## Step 2: Subscribe to webhooks on the customer&#039;s WABA

Using the business token from Step 1 and the customer&#039;s WABA ID, subscribe your app to webhooks on the customer&#039;s WABA.

Use the [POST /&lt;WABA_ID&gt;/subscribed_apps](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/subscribed-apps-api#post-version-waba-id-subscribed-apps) endpoint to subscribe your app to webhooks on the business customer&#039;s WABA. If you want the customer&#039;s webhooks to be sent to a different callback URL than the one set on your app, you have multiple [webhook override](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/override) options.


### Request

```html
curl -X POST &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;WABA_ID&gt;/subscribed_apps&#039; \
-H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039;
```


### Request parameters

| Placeholder | Description | Example value |
| --- | --- | --- |
| `&lt;BUSINESS_TOKEN&gt;` | **Required.**&lt;br&gt;&lt;br&gt;The customer&#039;s [business token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#business-integration-system-user-access-tokens). | `EAAAN6tcBzAUBOwtDtTfmZCJ9n3FHpSDcDTH86ekf89XnnMZAtaitMUysPDE7LES3CXkA4MmbKCghdQeU1boHr0QZA05SShiILcoUy7ZAb2GE7hrUEpYHKLDuP2sYZCURkZCHGEvEGjScGLHzC4KDm8tq2slt4BsOQE1HHX8DzHahdT51MRDqBw0YaeZByrVFZkVAoVTxXUtuKgDDdrmJQXMnI4jqJYetsZCP1efj5ygGscZBm4OvvuCYB039ZAFlyNn` |
| `&lt;WABA_ID&gt;` | **Required.**&lt;br&gt;&lt;br&gt;The customer&#039;s WABA ID. | `102290129340398` |

### Response

Upon success:

```json
&#123;
  &quot;success&quot;: true
&#125;
```


## Step 3: Share your credit line with the customer

**Warning:** New steps for sharing your credit line with onboarded business customers are currently being tested. These steps will eventually replace this step, so if you wish to implement these steps now, see [Alternate method for sharing your credit line](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/share-and-revoke-credit-lines#alternate-method-for-sharing-your-credit-line).

**Note**: If you are using the `whatsapp_credit_sharing_and_attach` API, you must add your System User to the shared WhatsApp Business Accounts as a prerequisite. See [add your System User to shared WhatsApp Business Accounts](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/manage-system-users).

Once you have added the System user to the WhatsApp Business account, use the [Credit Sharing API](https://developers.facebook.com/docs/marketing-api/reference/extended-credit/whatsapp_credit_sharing_and_attach#Creating) to share your credit line with an onboarded business customer.

### Request

```html
curl -X POST &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;EXTENDED_CREDIT_LINE_ID&gt;/whatsapp_credit_sharing_and_attach?waba_currency=&lt;CUSTOMER_BUSINESS_CURRENCY&gt;&amp;waba_id=&lt;CUSTOMER_WABA_ID&gt;&#039; \
-H &#039;Authorization: Bearer &lt;SYSTEM_TOKEN&gt;&#039;
```

### Request parameters

| Placeholder | Description | Example value |
| --- | --- | --- |
| `&lt;CUSTOMER_BUSINESS_CURRENCY&gt;` | **Required.**&lt;br&gt;&lt;br&gt;The business&#039;s currency, as a three-letter currency code. Supported values are:&lt;br&gt;&lt;br&gt;* `AUD`&lt;br&gt;* `EUR`&lt;br&gt;* `GBP`&lt;br&gt;* `IDR`&lt;br&gt;* `INR`&lt;br&gt;* `USD`&lt;br&gt;&lt;br&gt;This currency is used for invoicing and corresponds to [pricing](https://developers.facebook.com/documentation/business-messaging/whatsapp/pricing) rates. | `USD` |
| `&lt;CUSTOMER_WABA_ID&gt;` | **Required.**&lt;br&gt;&lt;br&gt;The customer&#039;s WABA ID. | `102290129340398` |
| `&lt;EXTENDED_CREDIT_LINE_ID&gt;` | **Required.**&lt;br&gt;&lt;br&gt;Your extended credit line ID. | `1972385232742146` |
| `&lt;SYSTEM_TOKEN&gt;` | **Required.**&lt;br&gt;&lt;br&gt;Your system token. | `EAAAN6tcBzAUBOZC82CW7iR2LiaZBwUHS4Y7FDtQxRUPy1PHZClDGZBZCgWdrTisgMjpFKiZAi1FBBQNO2IqZBAzdZAA16lmUs0XgRcCf6z1LLxQCgLXDEpg80d41UZBt1FKJZCqJFcTYXJvSMeHLvOdZwFyZBrV9ZPHZASSqxDZBUZASyFdzjiy2A1sippEsF4DVV5W2IlkOSr2LrMLuYoNMYBy8xQczzOKDOMccqHEZD` |

### Response

Upon success:

```html
&#123;
  &quot;allocation_config_id&quot;: &quot;&lt;ALLOCATION_CONFIGURATION_ID&gt;&quot;,
  &quot;waba_id&quot;: &quot;&lt;CUSTOMER_WABA_ID&gt;&quot;
&#125;
```

### Response parameters

| Placeholder | Description | Example value |
| --- | --- | --- |
| `&lt;ALLOCATION_CONFIGURATION_ID&gt;` | The extended credit line&#039;s allocation configuration ID.&lt;br&gt;&lt;br&gt;Save this ID if you want to [verify](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/share-and-revoke-credit-lines#verifying-shared-status) that your credit line has been shared with the customer. | `58501441721238` |
| `&lt;CUSTOMER_WABA_ID&gt;` | The customer&#039;s WABA ID. | `102290129340398` |

## Step 4: Register the customer&#039;s phone number

Use the [Register API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/register-api#post-version-phone-number-id-register) to register the customer&#039;s business phone number for use with Cloud API.

### Request

```html
curl &#039;https://graph.facebook.com/v21.0/&lt;BUSINESS_PHONE_NUMBER_ID&gt;/register&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer &lt;BUSINESS_TOKEN&gt;&#039; \
-d &#039;
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;pin&quot;: &quot;&lt;DESIRED_PIN&gt;&quot;
&#125;&#039;
```

### Request parameters

| Placeholder | Description | Example value |
| --- | --- | --- |
| `&lt;BUSINESS_PHONE_NUMBER_ID&gt;` | **Required.**&lt;br&gt;&lt;br&gt;The customer&#039;s business phone number ID that Embedded Signup returned. | `106540352242922` |
| `&lt;BUSINESS_TOKEN&gt;` | **Required.**&lt;br&gt;&lt;br&gt;The customer&#039;s business token. | `EAAAN6tcBzAUBOwtDtTfmZCJ9n3FHpSDcDTH86ekf89XnnMZAtaitMUysPDE7LES3CXkA4MmbKCghdQeU1boHr0QZA05SShiILcoUy7ZAb2GE7hrUEpYHKLDuP2sYZCURkZCHGEvEGjScGLHzC4KDm8tq2slt4BsOQE1HHX8DzHahdT51MRDqBw0YaeZByrVFZkVAoVTxXUtuKgDDdrmJQXMnI4jqJYetsZCP1efj5ygGscZBm4OvvuCYB039ZAFlyNn` |
| `&lt;DESIRED_PIN&gt;` | **Required.**&lt;br&gt;&lt;br&gt;Set this value to a 6-digit number. This will be the business phone number&#039;s two-step verification PIN. | `581063` |

### Response

Upon success:

```html
&#123;
  &quot;success&quot;: true
&#125;
```

## Step 5: Send a test message

_This step is optional._

If you want to test the messaging capabilities of your business customer&#039;s business phone number, send a message to the customer&#039;s number from your own WhatsApp number (this will open a [customer service window](https://developers.facebook.com/documentation/business-messaging/whatsapp/messages/send-messages#customer-service-windows), allowing you to respond with any type of message).

Next, use the [Messages API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/message-api#post-version-phone-number-id-messages) to send a text message in response.

### Request

```html
curl &#039;https://graph.facebook.com/v21.0/&lt;BUSINESS_PHONE_NUMBER_ID&gt;/messages&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer &lt;BUSINESS_TOKEN&gt;&#039; \
-d &#039;
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;recipient_type&quot;: &quot;individual&quot;,
  &quot;to&quot;: &quot;&lt;WHATSAPP_USER_NUMBER&gt;&quot;,
  &quot;type&quot;: &quot;text&quot;,
  &quot;text&quot;: &#123;
    &quot;body&quot;: &quot;&lt;BODY_TEXT&gt;&quot;
  &#125;
&#125;&#039;
```

### Request parameters

| Placeholder | Description | Example value |
| --- | --- | --- |
| `&lt;BODY_TEXT&gt;` | **Required.**&lt;br&gt;&lt;br&gt;Message body text. Supports URLs.&lt;br&gt;&lt;br&gt;Maximum 4096 characters. | `Message received, loud, and clear!` |
| `&lt;BUSINESS_PHONE_NUMBER_ID&gt;` | **Required.**&lt;br&gt;&lt;br&gt;The customer&#039;s business phone number ID. | `106540352242922` |
| `&lt;BUSINESS_TOKEN&gt;` | **Required.**&lt;br&gt;&lt;br&gt;The customer&#039;s business token. | `EAAAN6tcBzAUBOwtDtTfmZCJ9n3FHpSDcDTH86ekf89XnnMZAtaitMUysPDE7LES3CXkA4MmbKCghdQeU1boHr0QZA05SShiILcoUy7ZAb2GE7hrUEpYHKLDuP2sYZCURkZCHGEvEGjScGLHzC4KDm8tq2slt4BsOQE1HHX8DzHahdT51MRDqBw0YaeZByrVFZkVAoVTxXUtuKgDDdrmJQXMnI4jqJYetsZCP1efj5ygGscZBm4OvvuCYB039ZAFlyNn` |
| `&lt;WHATSAPP_USER_NUMBER&gt;` | **Required.**&lt;br&gt;&lt;br&gt;Your WhatsApp phone number that can send and receive messages from other WhatsApp numbers.&lt;br&gt;&lt;br&gt;Note that this cannot be a business phone number already registered for use with Cloud API. | `+16505551234` |

### Response

Upon success:

```html
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;contacts&quot;: [
    &#123;
      &quot;input&quot;: &quot;&lt;WHATSAPP_USER_NUMBER&gt;&quot;,
      &quot;wa_id&quot;: &quot;&lt;WHATSAPP_USER_ID&gt;&quot;
    &#125;
  ],
  &quot;messages&quot;: [
    &#123;
      &quot;id&quot;: &quot;&lt;WHATSAPP_MESSAGE_ID&gt;&quot;
    &#125;
  ]
&#125;
```

### Response parameters

| Placeholder | Description | Example value |
| --- | --- | --- |
| `&lt;WHATSAPP_MESSAGE_ID&gt;` | WhatsApp message ID. | `wamid.HBgLMTY0NjcwNDM1OTUVAgARGBI1RjQyNUE3NEYxMzAzMzQ5MkEA` |
| `&lt;WHATSAPP_USER_ID&gt;` | Your WhatsApp user ID. | `16505551234` |
| `&lt;WHATSAPP_USER_NUMBER&gt;` | Your WhatsApp phone number that the message was sent to. | `+16505551234` |

The customer&#039;s business phone number is working properly if you were able to successfully send and receive messages using it. Confirm that **messages** webhooks were triggered [describing the initial message that you sent](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/messages), as well as the [delivery statuses](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/messages/status) of the message you sent in response.
