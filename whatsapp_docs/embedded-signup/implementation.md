# Implementation



**Warning:** **Embedded signup v2 will be deprecated on October 15, 2026.** Migrate your integration to [v4](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/version-4) before that date to avoid disruption. See [Versions](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/versions) for the full upgrade path.

This document explains how to implement Embedded Signup v4 and capture the data it generates to [onboard business customers](#onboarding-business-customers) onto the WhatsApp Business Platform.

## Before you start

* You must already be a [Solution Partner](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/get-started-for-solution-partners) or [Tech Provider](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/get-started-for-tech-providers).
* If your business customers will be using your app to send and receive messages, you should already know how to use the API to send and receive messages using your own WhatsApp Business account and business phone numbers. You should also know how to create and manage templates. In addition, you should have a webhooks callback endpoint properly set up to digest webhooks.
* You must be subscribed to the [account_update](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/account_update) webhook, as this webhook is triggered whenever a customer successfully completes the Embedded Signup flow, and contains their business information that you will need.
* If you are a Solution Partner, you must already have a [line of credit](https://www.facebook.com/business/help/1684730811624773?id=2129163877102343).
* The server where you will be hosting Embedded Signup must have a valid SSL certificate.

## Step 1: Add allowed domains

Load your app in the [App Dashboard](https://developers.facebook.com/apps) and navigate to **Facebook Login for Business** &gt; **Settings** &gt; **Client OAuth settings**:

Set the following toggles to **Yes**:

* **Client OAuth login**
* **Web OAuth login**
* **Enforce HTTPS**
* **Embedded Browser OAuth Login**
* **use Strict Mode for redirect URIs**
* **Login with the JavaScript SDK**

Embedded Signup relies on the JavaScript SDK. When a business customer completes the Embedded Signup flow, Embedded Signup returns the customer&#039;s WhatsApp Business account (WABA) ID, business phone number ID, and an exchangeable token code to the window that spawned the flow, but only if the domain of the page that spawned the flow is listed in the **Allowed domains** and **Valid OAuth redirect URIs** fields.

Add any domains where you plan to host Embedded Signup, including any development domains where you will be testing the flow, to these fields. Only domains that have enabled **HTTPS** are supported.

## Step 2: Create a Facebook Login for Business configuration

A Facebook Login for Business configuration defines which permissions to request, and what additional information to collect, from business customers who access Embedded Signup.

Navigate to **Facebook Login for Business** &gt; **Configurations**:

Click the **Create from template** button and create a configuration from the **WhatsApp Embedded Signup Configuration With 60 Expiration Token** template. The template generates a configuration for the most commonly used permissions and access levels.

Alternatively, you can create a custom configuration. To do this, in the **Configurations** panel, click the **Create configuration** button and provide a name that will help you differentiate the custom configuration from any others you may create in the future. When completing the flow, be sure to select the **WhatsApp Embedded Signup** login variation:

Select the products you want to onboard for this configuration.

When choosing assets and permissions, select only those assets and permissions that you will actually need from your business customers. Assets that are already selected are added by default.

For example, if you select the **Catalogs** asset but don&#039;t actually need access to customer catalogs, your customers will likely abandon the flow at the catalog selection screen and ask you for clarification.

When you complete the configuration flow, capture your configuration ID, as you will need it in the next step.

## Step 3: Add Embedded Signup to your website

Add the following HTML and JavaScript code to your website. This is the complete code needed to implement Embedded Signup. Each portion of the code will be explained in detail below.

```html
&lt;!-- SDK loading --&gt;
&lt;script async defer crossorigin=&quot;anonymous&quot; src=&quot;https://connect.facebook.net/en_US/sdk.js&quot;&gt;&lt;/script&gt;

&lt;script&gt;
  // SDK initialization
  window.fbAsyncInit = function() &#123;
    FB.init(&#123;
      appId: &#039;&lt;APP_ID&gt;&#039;, // your app ID goes here
      autoLogAppEvents: true,
      xfbml: true,
      version: &#039;&lt;GRAPH_API_VERSION&gt;&#039; // Graph API version goes here
    &#125;);
  &#125;;

  // Session logging message event listener
  window.addEventListener(&#039;message&#039;, (event) =&gt; &#123;
    if (!event.origin.endsWith(&#039;facebook.com&#039;)) return;
    try &#123;
      const data = JSON.parse(event.data);
      if (data.type === &#039;WA_EMBEDDED_SIGNUP&#039;) &#123;
        console.log(&#039;message event: &#039;, data); // remove after testing
        // your code goes here
      &#125;
    &#125; catch &#123;
      console.log(&#039;message event: &#039;, event.data); // remove after testing
      // your code goes here
    &#125;
  &#125;);

  // Response callback
  const fbLoginCallback = (response) =&gt; &#123;
    if (response.authResponse) &#123;
      const code = response.authResponse.code;
      console.log(&#039;response: &#039;, code); // remove after testing
      // your code goes here
    &#125; else &#123;
      console.log(&#039;response: &#039;, response); // remove after testing
      // your code goes here
    &#125;
  &#125;

  // Launch method and callback registration
  const launchWhatsAppSignup = () =&gt; &#123;
    FB.login(fbLoginCallback, &#123;
      config_id: &#039;&lt;CONFIGURATION_ID&gt;&#039;, // your configuration ID goes here
      response_type: &#039;code&#039;,
      override_default_response_type: true,
      extras: &#123;
        setup: &#123;&#125;,
      &#125;
    &#125;);
  &#125;
&lt;/script&gt;

&lt;!-- Launch button  --&gt;
&lt;button onclick=&quot;launchWhatsAppSignup()&quot; style=&quot;background-color: #1877f2; border: 0; border-radius: 4px; color: #fff; cursor: pointer; font-family: Helvetica, Arial, sans-serif; font-size: 16px; font-weight: bold; height: 40px; padding: 0 24px;&quot;&gt;Login with Facebook&lt;/button&gt;
```

### SDK loading

The following script tag loads the Facebook JavaScript SDK asynchronously:

```html
&lt;!-- SDK loading --&gt;
&lt;script async defer crossorigin=&quot;anonymous&quot; src=&quot;https://connect.facebook.net/en_US/sdk.js&quot;&gt;&lt;/script&gt;
```

### SDK initialization

This portion of the code initializes the SDK. Add your app ID and the latest Graph API version here.

```js
// SDK initialization
window.fbAsyncInit = function() &#123;
  FB.init(&#123;
    appId: &#039;&lt;APP_ID&gt;&#039;, // your app ID goes here
    autoLogAppEvents: true,
    xfbml: true,
    version: &#039;&lt;GRAPH_API_VERSION&gt;&#039; // Graph API version here
  &#125;);
&#125;;
```

Replace the following placeholders with your own values.

| Placeholder | Description | Example value |
| --- | --- | --- |
| `&lt;APP_ID&gt;` | **Required.**&lt;br&gt;&lt;br&gt;Your app ID. This is displayed at the top of the App Dashboard. | `21202248997039` |
| `&lt;GRAPH_API_VERSION&gt;` | **Required.**&lt;br&gt;&lt;br&gt;Graph API version. This indicates which version of Graph API to call, if you are relying on the SDK&#039;s methods to perform API calls.&lt;br&gt;&lt;br&gt;In the context of Embedded Signup, you won&#039;t be relying on the SDK&#039;s methods to perform API calls. Set this to the latest API version:&lt;br&gt;&lt;br&gt;v25.0 | v25.0 |

### Session logging message event listener

The message event listener captures the following critical information:

* The business customer&#039;s newly generated asset IDs, if they successfully completed the flow
* The name of the screen they abandoned, if they abandoned the flow
* An error ID, if they encountered an error and used the flow to report it

```js
// Session logging message event listener
window.addEventListener(&#039;message&#039;, (event) =&gt; &#123;
  if (!event.origin.endsWith(&#039;facebook.com&#039;)) return;
  try &#123;
    const data = JSON.parse(event.data);
    if (data.type === &#039;WA_EMBEDDED_SIGNUP&#039;) &#123;
      console.log(&#039;message event: &#039;, data); // remove after testing
      // your code goes here
    &#125;
  &#125; catch &#123;
    console.log(&#039;message event: &#039;, event.data); // remove after testing
    // your code goes here
  &#125;
&#125;);
```

Embedded Signup sends this information in a message event object to the window that spawned the flow and assigns it to the data constant. **Add your own custom code to the try-catch statement that can send this object to your server.** The object structure will vary based on flow completion, abandonment, or error reporting, as described below.

**Successful flow completion structure:**

On the final screen, both clicking **Finish** and closing the popup (for example, by clicking the X button) are considered successful onboarding. In both scenarios, Embedded Signup returns the exchangeable token code and the session info object containing the customer&#039;s asset IDs. Exiting on the final screen is not considered a cancel event.

```html
&#123;
  data: &#123;
    phone_number_id: &#039;&lt;CUSTOMER_BUSINESS_PHONE_NUMBER_ID&gt;&#039;,
    waba_id: &#039;&lt;CUSTOMER_WABA_ID&gt;&#039;,
    business_id: &#039;&lt;CUSTOMER_BUSINESS_PORTFOLIO_ID&gt;&#039;,

    &lt;!-- only included if customer selected ad accounts --&gt;
    ad_account_ids: [&#039;&lt;CUSTOMER_AD_ACCOUNT_ID_1&gt;&#039;, &#039;&lt;CUSTOMER_AD_ACCOUNT_ID_2&gt;&#039;],

    &lt;!-- only included if customer selected Facebook Pages --&gt;
    page_ids: [&#039;&lt;CUSTOMER_PAGE_ID_1&gt;&#039;, &#039;&lt;CUSTOMER_PAGE_ID_2&gt;&#039;],

    &lt;!-- only included if customer selected datasets --&gt;
    dataset_ids: [&#039;&lt;CUSTOMER_DATASET_ID_1&gt;&#039;, &#039;&lt;CUSTOMER_DATASET_ID_2&gt;&#039;],

    &lt;!-- only included if customer selected catalogs --&gt;
    catalog_ids: [&#039;&lt;CUSTOMER_CATALOG_ID_1&gt;&#039;, &#039;&lt;CUSTOMER_CATALOG_ID_2&gt;&#039;],

    &lt;!-- only included if customer selected Instagram accounts --&gt;
    instagram_account_ids: [&#039;&lt;CUSTOMER_IG_ACCOUNT_ID_1&gt;&#039;, &#039;&lt;CUSTOMER_IG_ACCOUNT_ID_2&gt;&#039;],

    &lt;!-- only included for multi-WABA flows --&gt;
    waba_ids: [&#039;&lt;CUSTOMER_WABA_ID_1&gt;&#039;, &#039;&lt;CUSTOMER_WABA_ID_2&gt;&#039;]
  &#125;,
  type: &#039;WA_EMBEDDED_SIGNUP&#039;,
  event: &#039;&lt;FLOW_FINISH_TYPE&gt;&#039;,
&#125;
```

| Placeholder | Description | Example value |
| --- | --- | --- |
| `&lt;CUSTOMER_BUSINESS_PHONE_NUMBER_ID&gt;` | The business customer&#039;s business phone number ID | `106540352242922` |
| `&lt;CUSTOMER_WABA_ID&gt;` | The business customer&#039;s WhatsApp Business account ID. | `524126980791429` |
| `&lt;CUSTOMER_BUSINESS_PORTFOLIO_ID&gt;` | The business customer&#039;s business portfolio ID. | `2729063490586005` |
| `&lt;CUSTOMER_AD_ACCOUNT_ID&gt;` | Only included if the customer selected ad accounts during the flow. The business customer&#039;s ad account ID. | `4052175343162067` |
| `&lt;CUSTOMER_PAGE_ID&gt;` | Only included if the customer selected Facebook Pages during the flow. The business customer&#039;s Facebook Page ID. | `1791141545170328` |
| `&lt;CUSTOMER_DATASET_ID&gt;` | Only included if the customer selected datasets during the flow. The business customer&#039;s dataset ID. | `524126980791429` |
| `&lt;CUSTOMER_CATALOG_ID&gt;` | Only included if the customer selected catalogs during the flow. The business customer&#039;s catalog ID. | `8827498273649182` |
| `&lt;CUSTOMER_IG_ACCOUNT_ID&gt;` | Only included if the customer selected Instagram accounts during the flow. The business customer&#039;s Instagram account ID. | `1749204838281942` |
| `&lt;CUSTOMER_WABA_ID&gt;` (in `waba_ids` array) | Only included for multi-WABA flows. Array of the business customer&#039;s WhatsApp Business account IDs. | `524126980791429` |
| `&lt;FLOW_FINISH_TYPE&gt;` | Indicates the customer successfully completed the flow.&lt;br&gt;&lt;br&gt;**Possible Values:**&lt;br&gt;&lt;br&gt;* `FINISH`: Indicates successful completion of [Cloud API flow](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/default-flow).&lt;br&gt;* `FINISH_ONLY_WABA`: Indicates user completed flow [without a phone number](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/bypass-phone-addition).&lt;br&gt;* `FINISH_WHATSAPP_BUSINESS_APP_ONBOARDING`: Indicates user completed flow [with a WhatsApp business app number](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/onboarding-business-app-users).&lt;br&gt;* `FINISH_OBO_MIGRATION`: Indicates user completed an on-behalf-of migration flow.&lt;br&gt;* `FINISH_GRANT_ONLY_API_ACCESS`: Indicates user completed a grant-only API access flow.&lt;br&gt;* `ERROR`: Indicates the user encountered an error during the flow. | `FINISH` |

**Abandoned flow structure:**

```html
&#123;
  data: &#123;
    current_step: &#039;&lt;CURRENT_STEP&gt;&#039;,
  &#125;,
  type: &#039;WA_EMBEDDED_SIGNUP&#039;,
  event: &#039;CANCEL&#039;,
&#125;
```

| Placeholder | Description | Example value |
| --- | --- | --- |
| `&lt;CURRENT_STEP&gt;` | Indicates which screen the business customer was viewing when they abandoned the flow. See [Embedded Signup flow errors](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/errors) for a description of each step. | `PHONE_NUMBER_SETUP` |

**User reported errors:**

```html
&#123;
  data: &#123;
    error_message: &#039;&lt;ERROR_MESSAGE&gt;&#039;,
    error_code: &#039;&lt;ERROR_CODE&gt;&#039;,
    session_id: &#039;&lt;SESSION_ID&gt;&#039;,
    timestamp: &#039;&lt;TIMESTAMP&gt;&#039;,
  &#125;,
  type: &#039;WA_EMBEDDED_SIGNUP&#039;,
  event: &#039;CANCEL&#039;,
&#125;
```

| Placeholder | Description | Example value |
| --- | --- | --- |
| `&lt;ERROR_MESSAGE&gt;` | The error description text displayed to the business customer in the Embedded Signup flow. See [Embedded Signup flow errors](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/errors) for a list of common errors. | Your verified name violates WhatsApp guidelines. Please edit your verified name and try again. |
| `&lt;ERROR_CODE&gt;` | Error code. Include this value if you contact support. | `524126` |
| `&lt;SESSION_ID&gt;` | Unique session ID generated by Embedded Signup. Include this ID if you contact support. | `f34b51dab5e0498` |
| `&lt;TIMESTAMP&gt;` | Unix timestamp indicating when the business customer used Embedded Signup to report the error. Include this value if you are contacting support. | `1746041036` |

Parse this object on your server to extract and capture the customer&#039;s phone number ID and WABA ID, or to determine which screen they abandoned. See [Abandoned flow screens](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/errors#abandoned-flow-screens) for a list of possible `&lt;CURRENT_STEP&gt;` values and the screens they correspond to.

Note that the try-catch statement in the code above has two statements that can be used for testing purposes:

```js
console.log(&#039;message event: &#039;, data); // remove after testing

console.log(&#039;message event: &#039;, event.data); // remove after testing
```

These statements just dump the returned phone number and WABA IDs, or the abandoned screen string, to the JavaScript console. You can leave this code in place and keep the console open to easily see what gets returned when you are testing the flow, but you should remove them when you are done testing.

### Response callback

Whenever a business customer successfully completes the Embedded Signup flow, Meta sends an exchangeable token code in a [JavaScript response](https://developer.mozilla.org/en-US/docs/Web/API/Response) to the window that spawned the flow.

```js
// Response callback
const fbLoginCallback = (response) =&gt; &#123;
  if (response.authResponse) &#123;
    const code = response.authResponse.code;
    console.log(&#039;response: &#039;, code); // remove after testing
    // your code goes here
  &#125; else &#123;
    console.log(&#039;response: &#039;, response); // remove after testing
    // your code goes here
  &#125;
&#125;
```

The callback function assigns the exchangeable token code to a `code` constant.

**Add your own, custom code to the if-else statement that sends this code to your server** so you can later exchange it for the customer&#039;s business token when you [onboard the business customer](#onboarding-business-customers).

**Warning:** The exchangeable token code has a time-to-live of 30 seconds, so make sure you are able to exchange it for the customer&#039;s business token before the code expires. If you are testing and just dumping the response to your JavaScript console, then manually exchanging the code using another app like Postman or your terminal with cURL, set up your token exchange query before you begin testing.

Note that the if-else statement in the code above has two statements that can be used for testing purposes:

```js
console.log(&#039;response: &#039;, code); // remove after testing

console.log(&#039;response: &#039;, response); // remove after testing
```

These statements just dump the code or the raw response to the JavaScript console. You can leave this code in place and keep the console open to easily see what gets returned when you are testing the flow, but you should remove them when you are done testing.

### Launch method and callback registration

This portion of the code defines a method which can be called by an `onclick` event that registers the response callback from the previous step and launches the Embedded Signup flow.

Add your configuration ID here.

```js
// Launch method and callback registration
const launchWhatsAppSignup = () =&gt; &#123;
  FB.login(fbLoginCallback, &#123;
    config_id: &#039;&lt;CONFIGURATION_ID&gt;&#039;, // your configuration ID goes here
    response_type: &#039;code&#039;,
    override_default_response_type: true,
    extras: &#123;
      setup: &#123;&#125;,
    &#125;
  &#125;);
&#125;
```

### Launch button

This portion of the code defines a button that calls the launch method from the previous step when clicked by the business customer.

```html
&lt;!-- Launch button --&gt;
&lt;button onclick=&quot;launchWhatsAppSignup()&quot; style=&quot;background-color: #1877f2; border: 0; border-radius: 4px; color: #fff; cursor: pointer; font-family: Helvetica, Arial, sans-serif; font-size: 16px; font-weight: bold; height: 40px; padding: 0 24px;&quot;&gt;Login with Facebook&lt;/button&gt;
```

## Testing

Once you have completed all of the implementation steps above, you should be able to test the flow by simulating a business customer while using your own Meta credentials. Anyone who you have added as an admin or developer on your app (in the **App Dashboard** &gt; **App roles** &gt; **Roles** panel) can also begin testing the flow, using their own Meta credentials.

## Onboarding business customers

Embedded Signup generates assets for your business customers, and grants your app access to those assets. However, you still need to make a series of API calls to fully onboard new business customers who have completed the flow.

The API calls you must make to onboard customers are different for Solution Partners and Tech Providers/Tech Partners.

* [Onboarding customers as a Solution Partner](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/onboarding-customers-as-a-solution-partner)
* [Onboarding customers as a Tech Provider](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/onboarding-customers-as-a-tech-provider)
