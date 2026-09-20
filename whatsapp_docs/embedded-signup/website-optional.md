# Website field optional



**Warning:** **Embedded signup v2 will be deprecated on October 15, 2026.** Migrate your integration to [v4](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/version-4) before that date to avoid disruption. See [Versions](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/versions) for the full upgrade path.

**Warning:** This feature is currently only available to approved **Select Solution** and **Premier** Solution Partners. See our [Sign up for partner-led business verification](https://www.facebook.com/business/help/1091073752691122) Help Center article to learn how to request approval.

By default, the website field is required in the [business portfolio screen](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/default-flow#business-portfolio-screen). If you have been approved for [Partner-led Business Verification](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/partner-led-business-verification) however, the website field will become optional and will be accompanied by a **My business does not have a website or profile page** checkbox:

When a business customer checks this box and completes the flow, the customer&#039;s WhatsApp assets and exchangeable token code will be generated and returned in a [message event](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/implementation#session-logging-message-event-listener) and [JavaScript response](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/implementation#response-callback), as usual.

However, the [account_update webhook](#webhook) that&#039;s triggered when the customer completes the flow will have `event` set to `PARTNER_CLIENT_CERTIFICATION_NEEDED`, which indicates that you must verify their business as part of the onboarding process.

Onboard the customer as you normally would, and when you&#039;re done, complete the steps described in our [Partner-led Business Verification](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/partner-led-business-verification) document to verify their business. **The customer will not be able to send messages until their business is verified.**

* [Onboarding business customers as a Solution Provider](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/onboarding-customers-as-a-solution-partner)
* [Onboarding business customers as a Tech Provider](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/onboarding-customers-as-a-tech-provider)

Note that if you are unable to verify your customer&#039;s business, the customer must first add a website on their own using [Meta Business Suite &gt; Settings &gt; Business info](https://business.facebook.com/settings/info), or they won&#039;t be able to send messages. Once they have added a website and it has been accepted, they can also [verify their business](https://www.facebook.com/business/help/2058515294227817) on their own, if they choose to do so.

## Webhook

When a business customer successfully completes the flow, an **account_update** webhook will be triggered with `event` set to `PARTNER_CLIENT_CERTIFICATION_NEEDED`.

```html
&#123;
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;&quot;,
      &quot;time&quot;: &lt;WEBHOOK_SENT_TIMESTAMP&gt;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;event&quot;: &quot;PARTNER_CLIENT_CERTIFICATION_NEEDED&quot;,
            &quot;partner_client_certification_needed_info&quot;: &#123;
              &quot;client_business_id&quot;: &quot;&lt;CUSTOMER_BUSINESS_PORTFOLIO_ID&gt;&quot;
            &#125;
          &#125;,
          &quot;field&quot;: &quot;account_update&quot;
        &#125;
      ]
    &#125;
  ],
  &quot;object&quot;: &quot;whatsapp_business_account&quot;
&#125;
```

When you receive this webhook, onboard the customer as you normally would, then complete the steps described in the [Partner-led Business Verification](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/partner-led-business-verification) document to verify the customer&#039;s business.
