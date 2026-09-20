# Onboarding business customers as a Tech Provider or Tech Partner



**Warning:** **Embedded signup v2 will be deprecated on October 15, 2026.** Migrate your integration to [v4](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/version-4) before that date to avoid disruption. See [Versions](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/versions) for the full upgrade path.

This document describes the steps Tech Providers and Tech Partners must perform to onboard new business customers who have completed the Embedded Signup flow.

If you are a Tech Provider or Tech Partner, any business customer who completes your implementation of the Embedded Signup flow will not be able to use your app to access their WhatsApp assets or send and receive messages (if you are offering messaging services) until you complete these steps.

## What you will need

* the business customer&#039;s WABA ID (returned via [session logging](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/implementation#session-logging-message-event-listener) or [API request](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/manage-accounts#get-shared-waba-id-with-access-token))
* the business customer&#039;s business phone number ID (returned via [session logging](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/implementation#session-logging-message-event-listener) or [API request](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/manage-phone-numbers#getting-phone-numbers))
* your app ID (displayed at the top of the **App Dashboard**)
* your app secret (displayed in the **App Dashboard** &gt; **App settings** &gt; **Basic** panel)

Also, if you wish to test messaging capabilities using the customer&#039;s business phone number, you will need a WhatsApp phone number that can already send and receive messages from other WhatsApp numbers.

**Note:** Perform all of the requests described below using server-to-server requests. Do not use client-side requests.

## Step 1: Exchange the token code for a business token

Use the **GET /oauth/access_token** endpoint to exchange the token code [returned](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/implementation#session-logging-message-event-listener) by Embedded Signup for a business integration system user access token (&quot;business token&quot;).


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
| `&lt;CODE&gt;` | **Required.**&lt;br&gt;&lt;br&gt;The code [returned by Embedded Signup](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/implementation#session-logging-message-event-listener) when the customer successfully completed the flow. | `AQBhlXsctMxJYbwbrpybxlo9tLPGy-QAmjBJA03jxLos43wxlBlrYozY5C33BXJULd133cOJf_5y6EkJZYMrAmW-EMj3Wdap9-NUM2nS4s8tC-ES7slBhh6QpCFM7-SzpI-iqsjqTGyxbUUW3AeaEyLkeZFIkBgcQ_SOxo9HShm20SDR5_n7AT9ZJ5dcgpqBQykNT-pQ8V7Ne9-sr6RLAWtJMF7-Zx6ABudRcWIN53tUTtquDVNuq3lrco4BlVQAv-54tR83Ae0ODN9Uet6j-BVLuetXhQCM3sz9RdgedlbxkidMbkztvYX1j7baOrJxyLyYGWYgbnUrKRQKCtWTsO5ekIGFgtbpS8UPJNqV6j8E5XKPJ8QA7ZFqzkB0s2O__J5FrjHzc_rDo1EuRbw98ihHDzQnvuXeHapEyfhLDJct0A` |

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

Use the [POST /&lt;WABA_ID&gt;/subscribed_apps](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/subscribed-apps-api#post-version-waba-id-subscribed-apps) endpoint to subscribe your app to webhooks on the business customer&#039;s WABA. If you want the customer&#039;s webhooks to be sent to a different callback URL than the one set on your app, you have multiple [webhook override](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/override) options.


### Request

```html
curl -X POST &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;WABA_ID&gt;/subscribed_apps&#039; \
-H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039;
```


### Request parameters

| Placeholder | Description | Example value |
| --- | --- | --- |
| `&lt;API_VERSION&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Graph API version. | v25.0 |
| `&lt;BUSINESS_TOKEN&gt;`&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;The business customer&#039;s [business token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#business-integration-system-user-access-tokens). | `EAAAN6tcBzAUBOwtDtTfmZCJ9n3FHpSDcDTH86ekf89XnnMZAtaitMUysPDE7LES3CXkA4MmbKCghdQeU1boHr0QZA05SShiILcoUy7ZAb2GE7hrUEpYHKLDuP2sYZCURkZCHGEvEGjScGLHzC4KDm8tq2slt4BsOQE1HHX8DzHahdT51MRDqBw0YaeZByrVFZkVAoVTxXUtuKgDDdrmJQXMnI4jqJYetsZCP1efj5ygGscZBm4OvvuCYB039ZAFlyNn` |
| `&lt;WABA_ID&gt;`&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;WhatsApp Business Account ID. | `102290129340398` |

### Response

Upon success:

```json
&#123;
  &quot;success&quot;: true
&#125;
```


## Step 3: Register the customer&#039;s phone number

Use the [Register API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/register-api#post-version-phone-number-id-register) to register the business customer&#039;s business phone number for use with Cloud API.

### Request

```html
curl &#039;https://graph.facebook.com/v21.0/&lt;BUSINESS_CUSTOMER_PHONE_NUMBER_ID&gt;/register&#039; \
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
| `&lt;BUSINESS_CUSTOMER_PHONE_NUMBER_ID&gt;`&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;The business customer&#039;s business phone number ID. | `106540352242922` |
| `&lt;BUSINESS_TOKEN&gt;`&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;The business customer&#039;s [business token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#business-integration-system-user-access-tokens). | `EAAAN6tcBzAUBOwtDtTfmZCJ9n3FHpSDcDTH86ekf89XnnMZAtaitMUysPDE7LES3CXkA4MmbKCghdQeU1boHr0QZA05SShiILcoUy7ZAb2GE7hrUEpYHKLDuP2sYZCURkZCHGEvEGjScGLHzC4KDm8tq2slt4BsOQE1HHX8DzHahdT51MRDqBw0YaeZByrVFZkVAoVTxXUtuKgDDdrmJQXMnI4jqJYetsZCP1efj5ygGscZBm4OvvuCYB039ZAFlyNn` |
| `&lt;DESIRED_PIN&gt;`&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Set this value to a 6-digit number. This will be the business phone number&#039;s two-step verification PIN. | `581063` |

### Response

Upon success:

```html
&#123;
  &quot;success&quot;: true
&#125;
```

## Step 4: Send a test message

_This step is optional._

If you wish to test the messaging capabilities of your business customer&#039;s business phone number, send a message to the customer&#039;s number from your own WhatsApp number (this will open a [customer service window](https://developers.facebook.com/documentation/business-messaging/whatsapp/messages/send-messages#customer-service-windows), allowing you to respond with any type of message).

Next, use the [Messages API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/message-api#post-version-phone-number-id-messages) to send a text message in response.

### Request

```html
curl &#039;https://graph.facebook.com/v21.0/&lt;BUSINESS_CUSTOMER_PHONE_NUMBER_ID&gt;/messages&#039; \
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
| `&lt;BODY_TEXT&gt;`&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Message body text. Supports URLs.&lt;br&gt;&lt;br&gt;Maximum 4096 characters. | `Message received, loud and clear!` |
| `&lt;BUSINESS_CUSTOMER_PHONE_NUMBER_ID&gt;`&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;The business customer&#039;s business phone number ID. | `106540352242922` |
| `&lt;BUSINESS_TOKEN&gt;`&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;The business customer&#039;s [business token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#business-integration-system-user-access-tokens). | `EAAAN6tcBzAUBOwtDtTfmZCJ9n3FHpSDcDTH86ekf89XnnMZAtaitMUysPDE7LES3CXkA4MmbKCghdQeU1boHr0QZA05SShiILcoUy7ZAb2GE7hrUEpYHKLDuP2sYZCURkZCHGEvEGjScGLHzC4KDm8tq2slt4BsOQE1HHX8DzHahdT51MRDqBw0YaeZByrVFZkVAoVTxXUtuKgDDdrmJQXMnI4jqJYetsZCP1efj5ygGscZBm4OvvuCYB039ZAFlyNn` |
| `&lt;WHATSAPP_USER_NUMBER&gt;`&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Your WhatsApp phone number that can send and receive messages from other WhatsApp numbers.&lt;br&gt;&lt;br&gt;Note that this cannot be a business phone number already registered for use with Cloud API. | `+16505551234` |

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

If you were able to successfully send and receive messages using the customer&#039;s business phone number, and if **messages** webhooks were triggered [describing the initial message that you sent](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/messages), as well as the [delivery statuses](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/messages/status) of the message you sent in response, the customer&#039;s business phone number is working properly.

## Step 5: Instruct the customer to add a payment method

Instruct your customer to use the WhatsApp Manager to add a payment method. You can provide them with the following Help Center link:

[https://www.facebook.com/business/help/488291839463771](https://www.facebook.com/business/help/488291839463771)

Alternatively, you can instruct them to:

1. Access the **WhatsApp Manager** &gt; **Overview** panel at [https://business.facebook.com/wa/manage/home/](https://business.facebook.com/wa/manage/home/)
1. Click the **Add payment method** button
1. Complete the flow

Once your customer adds a payment method, they are fully onboarded onto the WhatsApp Business Platform and can begin using your app to access their WhatsApp assets and send and receive messages (if you are providing them with that service).
