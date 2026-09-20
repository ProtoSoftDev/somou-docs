# App-Only Install



**Warning:** **Embedded signup v2 will be deprecated on October 15, 2026.** Migrate your integration to [v4](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/version-4) before that date to avoid disruption. See [Versions](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/versions) for the full upgrade path.

You can configure Embedded Signup so that only [business tokens](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#business-integration-system-user-access-tokens) can be used to access assets owned by customers onboarded via the flow. This approach scopes access to the customer&#039;s assets rather than relying on [system tokens](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#system-user-access-tokens), simplifies onboarding for other Meta assets, and supports a larger number of onboardings. Because a business token grants access only to a specific customer&#039;s assets, a compromised token affects fewer assets.

App-Only Install can&#039;t be used to [onboard WhatsApp Business app users](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/onboarding-business-app-users).

## Enabling the feature in Embedded Signup v3

To enable this feature, set `features` to `app_only_install` in the Embedded Signup configuration.

```html
&#123;
  &quot;config_id&quot;: &quot;&lt;CONFIGURATION_ID&gt;&quot;,
  &quot;response_type&quot;: &quot;code&quot;,
  &quot;override_default_response_type&quot;: true,
  &quot;extras&quot;: &#123;
    &quot;version&quot;: &quot;v3&quot;,
    &quot;features&quot;: [
      &#123;
        &quot;name&quot;: &quot;app_only_install&quot;
      &#125;
    ]
  &#125;
&#125;
```

To enable this feature along with a [Multi-Partner Solution](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/multi-partner-solutions):

```html
&#123;
  &quot;config_id&quot;: &quot;&lt;CONFIG_ID&gt;&quot;,
  &quot;response_type&quot;: &quot;code&quot;,
  &quot;override_default_response_type&quot;: true,
  &quot;extras&quot;: &#123;
    &quot;version&quot;: &quot;v3&quot;,
    &quot;features&quot;: [
      &#123;
        &quot;name&quot;: &quot;app_only_install&quot;
      &#125;
    ],
    &quot;setup&quot;: &#123;
      &quot;solutionID&quot;: &quot;&lt;SOLUTION_ID&gt;&quot;
    &#125;
  &#125;
&#125;
```

When a business customer successfully completes the flow, the [session logging message event](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/implementation#session-logging-message-event-listener) has `event` set to `FINISH_GRANT_ONLY_API_ACCESS`:

```html
&#123;
  data: &#123;
    phone_number_id: &quot;&lt;CUSTOMER_BUSINESS_PHONE_NUMBER_ID&gt;&quot;,
    waba_id: &quot;&lt;CUSTOMER_WABA_ID&gt;&quot;,
    business_id: &quot;&lt;CUSTOMER_BUSINESS_ID&gt;&quot;,
  &#125;,
  type: &quot;WA_EMBEDDED_SIGNUP&quot;,
  event: &quot;FINISH_GRANT_ONLY_API_ACCESS&quot;,
&#125;
```

When a business customer successfully completes the flow, you receive an **account_update** webhook with `event` set to `PARTNER_APP_INSTALLED`.

```html
&#123;
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;&lt;PARTNER_BUSINESS_ID_1&gt;&quot;,
      &quot;time&quot;: &quot;&lt;WEBHOOK_TRIGGER_TIMESTAMP&gt;&quot;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;event&quot;: &quot;PARTNER_APP_INSTALLED&quot;,
            &quot;waba_info&quot;: &#123;
              &quot;waba_id&quot;: &quot;&lt;WABA_ID&gt;&quot;,
              &quot;owner_business_id&quot;: &quot;&lt;WABA_OWNER_BUSINESS_ID&gt;&quot;,
              &quot;partner_app_id&quot;: &quot;&lt;APP_ID&gt;&quot;,
              &quot;solution_id&quot;: &quot;&lt;SOLUTION_ID&gt;&quot;,
              &quot;solution_partner_business_ids&quot;: [
                &quot;&lt;PARTNER_BUSINESS_ID_1&gt;&quot;,
                &quot;&lt;PARTNER_BUSINESS_ID_2&gt;&quot;
              ]
            &#125;
          &#125;
        &#125;
      ],
      &quot;field&quot;: &quot;account_update&quot;,
      &quot;object&quot;: &quot;whatsapp_business_account&quot;
    &#125;
  ]
&#125;
```

If an onboarded business customer uses [Meta Business Suite](https://business.facebook.com) to uninstall/remove the app, an **account_update** webhook is triggered with `event` set to `PARTNER_APP_UNINSTALLED`.

```html
&#123;
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;&lt;PARTNER_BUSINESS_ID&gt;&quot;,
      &quot;time&quot;: &quot;&lt;WEBHOOK_TRIGGER_TIMESTAMP&gt;&quot;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;event&quot;: &quot;PARTNER_APP_UNINSTALLED&quot;
          &#125;,
          &quot;field&quot;: &quot;account_update&quot;
        &#125;
      ]
    &#125;
  ],
  &quot;object&quot;: &quot;whatsapp_business_account&quot;
&#125;
```

You can use the [System User Access Tokens API](https://developers.facebook.com/documentation/ads-commerce/marketing-api/reference/business/system_user_access_tokens) to get an onboarded business customer&#039;s business token.

```html
curl -i -X POST &quot;https://graph.facebook.com/v22.0/&lt;CUSTOMER_BUSINESS_PORTFOLIO_ID&gt;/system_user_access_tokens
  ?appsecret_proof=&lt;APPSECRET_PROOF_HASH&gt;
  &amp;access_token=&lt;ACCESS_TOKEN&gt;
  &amp;system_user_id=&lt;SYSTEM_USER_ID&gt;
  &amp;fetch_only=true&quot;
```
