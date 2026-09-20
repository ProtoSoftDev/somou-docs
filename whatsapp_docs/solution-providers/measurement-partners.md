# Measurement Partners



A Measurement Partner is a third-party company that helps businesses measure the effectiveness of their marketing campaigns on our platform.

Measurement Partners gain read-only access to WhatsApp Business account (WABA) analytics data and webhooks. Specifically, they can view phone numbers, message templates, and incoming messages, and can access WABA analytics data.

For a business to share their analytics data with a Measurement Partner, they must already have a WABA. Measurement Partners cannot create WABAs or send messages on behalf of their clients.

## Onboarding flow overview

Follow these steps to onboard as a Measurement Partner:

1. [Complete Tech Provider onboarding.](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/get-started-for-tech-providers)
2. Create your Facebook Login Button using the Measurement Partner ES template instructions below.
3. Embed the Facebook Login Button on your website.

## How to create Facebook Login button using the Measurement Partner ES template

Follow the steps below to create your Facebook Login button that will show the Measurement Partner Embedded Signup flow to your clients.

## Step 1: Load the Facebook JavaScript SDK
**Note:** See [Basic Setup](https://developers.facebook.com/docs/javascript/quickstart#loading) for instructions on loading the basic version of the Facebook JavaScript SDK with the options set to their most common defaults.

The `fbAsyncInit` function must be attached to the `window` object before the line of code loading the JavaScript SDK as the SDK calls this function to set up the Facebook Login information.

This setup uses the following parameters:

* `appId` — The Meta app ID
* `cookie` — Enables cookies to allow the server to access this session
* `xfbml`— Parses social plugins on the page
* `version` — The Graph API version to use

### Example

```js
&lt;script&gt;
  window.fbAsyncInit = function () &#123;
    // JavaScript SDK configuration and setup
    FB.init(&#123;
      appId:    &#039;&lt;i&gt;facebook-app-id&lt;/i&gt;&#039;, // Meta App ID
      cookie:   true, // enable cookies
      xfbml:    true, // parse social plugins on this page
      version:  &#039;v25.0&#039; //Graph API version
    &#125;);
  &#125;;
&lt;/script&gt;
```

## Step 2: Create Facebook Login for Business configuration

### Prerequisites

* You should have created an app in the App Dashboard on [https://developers.facebook.com/](https://developers.facebook.com/)
* Add the **[Facebook Login for Business](https://developers.facebook.com/documentation/facebook-login/facebook-login-for-business)** product to your app
* Follow [best practices](https://developers.facebook.com/documentation/facebook-login/security#enablejssdk) on how to set up **Client OAuth settings**, specifically settings like *Valid OAuth Redirect URIs* and *Allowed Domains for the JavaScript SDK*

### Process

1. In the **App Dashboard**, under **Facebook Login for Business**, click **Templates**.
1. Click the **Use template** button for the **WhatsApp Measurement Partner** template.
1. Since all the template configuration details have been set, click **Create from template**.
1. Copy and retain the **Configuration ID** and set this value in the Facebook Login Button script in the next step.

## Step 3: Set up Facebook Login

[Facebook Login](https://developers.facebook.com/documentation/facebook-login) allows you to place a button on your website or portal to initiate a connection to Facebook. Businesses can use this login flow to associate their Facebook profiles with their business presence (that is, Business Manager) in order to streamline onboarding.

The Facebook Login button should be implemented in a location of your choice, such as a platform portal or landing page, using the instructions below to trigger the Embedded Signup OAuth flow.

After loading the JavaScript SDK and initializing it with the proper information, set up the `FB.login()` function to trigger the Embedded Signup flow.

Make sure the following are included:

* The `response` callback function
* The `config_id` parameter
* The `extras` object with:
  * The `setup` parameter for any [prefilled form data](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/pre-filled-data)

### Example

```js
&lt;script&gt;
  window.fbAsyncInit = function () &#123;
    // JavaScript SDK configuration and setup
    FB.init(&#123;
      appId:    &#039;&lt;i&gt;your-facebook-app-id&lt;/i&gt;&#039;, // Facebook App ID
      cookie:   true, // enable cookies
      xfbml:    true, // parse social plugins on this page
      version:  &#039;v25.0&#039; //Graph API version
    &#125;);
  &#125;;

  // Load the JavaScript SDK asynchronously
  (function (d, s, id) &#123;
    var js, fjs = d.getElementsByTagName(s)[0];
    if (d.getElementById(id)) return;
    js = d.createElement(s); js.id = id;
    js.src = &quot;https://connect.facebook.net/en_US/sdk.js&quot;;
    fjs.parentNode.insertBefore(js, fjs);
  &#125;(document, &#039;script&#039;, &#039;facebook-jssdk&#039;));

  // Facebook Login with JavaScript SDK
  function launchWhatsAppSignup() &#123;
    // Conversion tracking code
    fbq &amp;&amp; fbq(&#039;trackCustom&#039;, &#039;WhatsAppOnboardingStart&#039;, &#123;appId: &#039;&lt;i&gt;your-facebook-app-id&lt;/i&gt;&#039;, feature: &#039;whatsapp_embedded_signup&#039;&#125;);

    // Launch Facebook login
    FB.login(function (response) &#123;
      if (response.authResponse) &#123;
        const code = response.authResponse.code;
        // The returned code must be transmitted to your backend,
  // which will perform a server-to-server call from there to our servers for an access token
      &#125; else &#123;
        console.log(&#039;User cancelled login or did not fully authorize.&#039;);
      &#125;
    &#125;, &#123;
      config_id: &#039;&lt;CONFIG_ID&gt;&#039;, // configuration ID goes here
      response_type: &#039;code&#039;,    // must be set to &#039;code&#039; for System User access token
      override_default_response_type: true, // when true, any response types passed in the &quot;response_type&quot; will take precedence over the default types
      extras: &#123;
        setup: &#123;
          ... // Prefilled data can go here
        &#125;
      &#125;
    &#125;);
  &#125;
&lt;/script&gt;
```

## Step 4: Create a login button

Create a button or link on your website to launch the Embedded Signup flow. Use the `onClick` function to call the `launchWhatsAppSignup()` function set up in Step 3 above.

### Example

```js
&lt;button onclick=&quot;launchWhatsAppSignup()&quot; style=&quot;background-color: #1877f2; border: 0; border-radius: 4px; color: #fff; cursor: pointer; font-family: Helvetica, Arial, sans-serif; font-size: 16px; font-weight: bold; height: 40px; padding: 0 24px;&quot;&gt;Login with Facebook&lt;/button&gt;
```

## Embed your new Facebook Login button

Copy the button code to the desired location on your site.

## Testing the Embedded Signup flow for Measurement Partners

1. On the sidebar under **WhatsApp**, click **ES Integrations** and then scroll down to **Embedded sign-up launch**.
1. Under **Embedded sign-up dialog**, choose your Measurement Partner config and click **Login with Facebook**.
1. Follow the prompts to test the sign-up flow.
