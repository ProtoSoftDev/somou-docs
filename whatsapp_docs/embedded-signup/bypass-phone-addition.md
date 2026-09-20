# Bypassing the phone number addition screen



**Warning:** **Embedded signup v2 will be deprecated on October 15, 2026.** Migrate your integration to [v4](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/version-4) before that date to avoid disruption. See [Versions](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/versions) for the full upgrade path.

This document describes how to customize Embedded Signup to bypass the [phone number addition screen](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/default-flow#phone-number-addition-screen) (shown below). The same customization also bypasses the [phone number verification screen](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/default-flow#phone-number-verification-screen).

You might not want your business customers to enter or choose a business phone number in the phone number addition screen. You can customize Embedded Signup to skip the screen entirely. However, after a customer successfully completes the customized flow, you must register a phone number for them. To do this, you can [programmatically create and register their business phone number](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/registering-phone-numbers). Alternatively, you can build a UI in your app that lets them register a phone number.

## Enabling the feature

The phone number screen bypass is controlled by the `featureType` parameter. To enable it, set `featureType` to `only_waba_sharing` in the [launch method and callback registration](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/implementation#launch-method-and-callback-registration) portion of the Embedded Signup code:

```js
// Launch method and callback registration
const launchWhatsAppSignup = () =&gt; &#123;
  FB.login(fbLoginCallback, &#123;
    config_id: &#039;&lt;CONFIGURATION_ID&gt;&#039;, // your configuration ID goes here
    response_type: &#039;code&#039;,
    override_default_response_type: true,
    extras: &#123;
      setup: &#123;&#125;,
      featureType: &#039;only_waba_sharing&#039;, // set to only_waba_sharing
      sessionInfoVersion: &#039;3&#039;,
    &#125;
  &#125;);
&#125;
```

When a business customer successfully completes the bypass (`only_waba_sharing`) flow, the [session logging message event](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/implementation#session-logging-message-event-listener) will have `event` set to `FINISH_ONLY_WABA`:

```html
&#123;
  data: &#123;
    phone_number_id: &quot;&lt;CUSTOMER_BUSINESS_PHONE_NUMBER_ID&gt;&quot;,
    waba_id: &quot;&lt;CUSTOMER_WABA_ID&gt;&quot;
  &#125;,
  type: &quot;WA_EMBEDDED_SIGNUP&quot;,
  event: &quot;FINISH_ONLY_WABA&quot;,
  version: 3
&#125;
```
