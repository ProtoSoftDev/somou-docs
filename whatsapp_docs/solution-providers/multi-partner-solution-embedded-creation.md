# Multi-Partner Solution — Embedded creation



[Multi-Partner Solutions](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/multi-partner-solutions) (MPS) allow Solution Partners and Tech Providers to jointly manage customer WhatsApp assets in order to provide WhatsApp messaging services to customers.

If you are a Solution Partner, instead of using the app dashboard to create an MPS, you can create one using a snippet of JavaScript and an HTML button which you can embed somewhere on your website. Tech Providers who want to partner with you can use the button to grant your app permission to manage solutions for one or more of their apps, which you can then do using a series of API requests.

## Flow

Tech Providers who visit your website and click the embedded solution creation button will be asked to authenticate, and after doing so, will be presented with an interface that allows them to choose an existing app:

After choosing an app, they can review and confirm that they will be granting your app permission to manage their app&#039;s Multi-Partner Solutions.

Once the Tech Provider dismisses the interface, a user access token will be generated and returned to the flow, where you can capture it. You can then use the token in a series of API calls to get the Tech Provider&#039;s chosen app IDs and create and accept a solution.

## Requirements

* Facebook Login for Business must be [configured on your app](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/implementation#step-2--create-a-facebook-login-for-business-configuration), with **Valid OAuth Redirect URIs** and **Allowed Domains for the JavaScript SDK** set. You should already have set these values when configuring Embedded Signup.
* Your app must undergo App Review and be approved for advanced access for the **manage_app_solution** permission.

## Embedded creation button

### Step 1: Grant permission to app &#123;#step-1-grant-permission-to-app&#125;

Access the Meta Business Suite and use your system user to grant your app the **manage_app_solution** permission.

1. Log into [business.facebook.com](https://business.facebook.com).
1. Use the business portfolio dropdown menu on the left to locate your business portfolio and click the gear icon (for settings).
1. Navigate to **Users** &gt; **System Users**.
1. Click the system user who has business asset access on your app and WhatsApp Business account.
1. Click the **Generate token** button.
1. Select your app.
1. Set an expiration date for the token.
1. Select the **manage_app_solution** permission.
1. Generate a token.

Use this token when accepting any Multi-Partner Solutions you create for your partners (see below).

### Step 2: Add embedded button code

Add the following code to your website or portal, or wherever you plan on directing Tech Providers who will be working with you as part of an MPS. Be sure to replace `&lt;SOLUTION_PARTNER_APP_ID&gt;` with your app ID.

```https
&lt;!-- Load JavaScript SDK asynchronously --&gt;
&lt;script async defer crossorigin=&quot;anonymous&quot; src=&quot;https://connect.facebook.net/en_US/sdk.js&quot;&gt;&lt;/script&gt;

&lt;script&gt;
  // Configure JavaScript SDK
  window.fbAsyncInit = function() &#123;
    FB.init(&#123;
      appId: &quot;&lt;SOLUTION_PARTNER_APP_ID&gt;&quot;, // Replace with your app ID
      cookie: true,
      xfbml: true,
      version: &quot;v20.0&quot;
    &#125;);
  &#125;;

  // Launch MPS creation flow
  function launchSolutionCreationFlow() &#123;
    FB.login(
      function (response) &#123;
        if (response.authResponse) &#123;
          const accessToken = response.authResponse.accessToken;
          console.log(accessToken); // Replace with your code that captures access token
        &#125; else &#123;
          console.log(&quot;User failed to authorize&quot;); // Replace with your code that logs auth failure
        &#125;
      &#125;,
      &#123;
        scope: &quot;manage_app_solution&quot;
      &#125;
    );
  &#125;
&lt;/script&gt;

&lt;button onclick=&quot;launchSolutionCreationFlow()&quot; style=&quot;background-color: #1877f2; border: 0; border-radius: 4px; color: #fff; cursor: pointer; font-family: Helvetica, Arial, sans-serif; font-size: 16px; font-weight: bold; height: 40px; padding: 0 24px;&quot;&gt;Launch Solution Creation&lt;/button&gt;
```

Direct prospective Tech Provider partners to this location and instruct them to complete the flow. Let them know that completing the flow does not create the solution (it requires some API calls on your part) and that you&#039;ll provide them with the solution ID once it has been created.

## Solution creation

### Step 1: Capture user token

Anytime a Tech Provider uses the embedded solution creation button and completes the flow, the flow returns an `authResponse` object (`response.authResponse`) that has an `accessToken` property:

```json
&#123;
  status: &quot;connected&quot;,
  authResponse: &#123;
    accessToken: &quot;&lt;USER_ACCESS_TOKEN&gt;&quot;,
    expiresIn:&quot;&lt;TOKEN_EXPIRATION_TIMESTAMP&gt;&quot;,
    reauthorize_required_in:&quot;&lt;SECONDS_UNTIL_REAUTH_REQUIRED&gt;&quot;,
    signedRequest:&quot;&lt;SIGNED_PARAMETER&gt;&quot;,
    userID:&quot;&lt;USER_ID&gt;&quot;
  &#125;
&#125;
```

Capture the `accessToken` property value. This is the Tech Provider&#039;s user access token, which you will need next.

### Step 2: Get app details

Use the Tech Provider&#039;s user access token and the [Assigned Applications API](https://developers.facebook.com/docs/graph-api/reference/user/assigned_applications) to get a list of app IDs that the Tech Provider selected when they completed the flow.

#### Example request

```curl
curl &#039;https://graph.facebook.com/v20.0/me/application_details&#039; \
-H &#039;Authorization: Bearer EAAJB&#039;
```

#### Example response

Example response of a Tech Provider who selected a single app in the flow.

```json
&#123;
  &quot;data&quot;: [
    &#123;
      &quot;link&quot;: &quot;www.mediamonsoon.com&quot;,
      &quot;name&quot;: &quot;media_monsoon_prod&quot;,
      &quot;id&quot;: &quot;634974688087057&quot;
    &#125;
  ]
&#125;
```

Each object in the response describes an app the Tech Provider selected when completing the flow. Capture the `id` property value of each app for the next step.

### Step 3: Create a solution for Tech Provider

Use the Tech Provider&#039;s access token and an app ID from the previous step to make a request to the [Solution Creation API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/application/solution-creation-api#post-version-application-id-whatsapp-business-solution).

Repeat this request for each app ID returned in the previous step.

#### Request syntax

```https
POST /&lt;APP_ID&gt;/whatsapp_business_solution
```

```json
&#123;
  &quot;owner_permissions&quot;: [&quot;MESSAGING&quot;],
  &quot;partner_app_id&quot;: &quot;&lt;SOLUTION_PARTNER_APP_ID&gt;&quot;,
  &quot;partner_permissions&quot;: [&quot;MESSAGING&quot;],
  &quot;solution_name&quot;: &quot;&lt;SOLUTION_NAME&gt;&quot;
&#125;
```

* `&lt;SOLUTION_PARTNER_APP_ID&gt;` — Your app ID.
* `&lt;SOLUTION_NAME&gt;` — Name to give the solution. This name will appear in the App Dashboard for both you and the Tech Provider, so the name should be unique and distinguishable from other solutions you or the Tech Provider may later initiate or accept.

#### Response

Upon success, the API will create a solution and associate your app and the Tech Provider&#039;s app to it.

```json
&#123;
  &quot;solution_id&quot;: &quot;&lt;SOLUTION_ID&gt;&quot;
&#125;
```

Capture the `solution_id` value. This is the solution ID, which you will need in the next step.

### Step 4: Accept the solution

Use your system user access token from the [Grant Permission to App](#step-1-grant-permission-to-app) step and the solution ID to make a request to the [Solution Accept API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-solution/solution-accept-api#post-version-solution-id-accept) for any solutions you have created for Tech Providers.

#### Example request

```curl
curl -X POST &#039;https://graph.facebook.com/v20.0/795033096057724/accept&#039; \
-H &#039;Authorization: Bearer EAAAT...
```

#### Example response

Upon success:

```json
&#123;
  &quot;success&quot;: true
&#125;
```

Once you have accepted the solution, inform the Tech Provider that the solution has been created successfully, and provide them with any solution IDs you have created and accepted.
