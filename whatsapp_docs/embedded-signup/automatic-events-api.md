# Automatic Events API



**Warning:** **Embedded signup v2 will be deprecated on October 15, 2026.** Migrate your integration to [v4](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/version-4) before that date to avoid disruption. See [Versions](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/versions) for the full upgrade path.

Business customers who access Embedded Signup can opt in to automatic event identification:

If a business customer opts in, Meta uses a combination of regex and natural language processing to analyze the customer&#039;s new message threads originating from Click-to-WhatsApp ads. If our analysis determines that a lead gen or purchase event occurred, an [automatic_events](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/automatic_events) webhook is triggered, describing the event. You can then report the event for the customer using the [Conversions API](https://developers.facebook.com/documentation/ads-commerce/conversions-api) so the customer can use it on a Meta surface (in 2026, see [Limitations](#limitations) below).

To learn more about how this feature works, see these [additional resources](https://meta.highspot.com/items/6839e4fecd0bb354418ee7ec).

## Limitations

- Automatic event identification is a new feature. Your business customers won&#039;t see or use automatic events reported via Conversions API in Meta surfaces until 2026. However, you can surface this information to your customers using your own solution before then. This allows them to review their own customers&#039; needs, preferences, and ad performance.
- Automatic event identification is not available to business customers in the European Union, United Kingdom, or Japan.

## Requirements

- You have already [implemented](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/implementation) Embedded Signup and are able to onboard business customers who complete the flow.
- Your [webhook server](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/create-webhook-endpoint) is successfully processing webhooks.

## Setup

Automatic event identification is available as an opt-in feature to all business customers automatically. To receive event notifications, you must subscribe your app to the **automatic_events** webhook field. However, as soon as you do this, you may begin receiving these webhooks before you can process them. Therefore, complete these steps using a test app before moving your code to production and subscribing your production app to the webhook field.

### Step 1: Subscribe to the automatic_events webhook field

Navigate to the [App Dashboard](https://developers.facebook.com/apps) &gt; **Webhooks** &gt; **Configuration** panel and subscribe to the [automatic_events](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/automatic_events) webhook field.

### Step 2: Adjust your webhook callback

Adjust your webhook callback code so that it can successfully process **automatic_events** webhook payloads.

#### Lead gen event structure

```html
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;&quot;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;messaging_product&quot;: &quot;whatsapp&quot;,
            &quot;metadata&quot;: &#123;
              &quot;display_phone_number&quot;: &quot;&lt;BUSINESS_DISPLAY_PHONE_NUMBER&gt;&quot;,
              &quot;phone_number_id&quot;: &quot;&lt;BUSINESS_PHONE_NUMBER_ID&gt;&quot;
            &#125;,
            &quot;automatic_events&quot;: [
              &#123;
                &quot;id&quot;: &quot;&lt;WHATSAPP_MESSAGE_ID&gt;&quot;,
                &quot;event_name&quot;: &quot;LeadSubmitted&quot;,
                &quot;timestamp&quot;: &lt;WEBHOOK_TRIGGER_TIMESTAMP&gt;,
                &quot;ctwa_clid&quot;: &quot;&lt;CLICK_ID&gt;&quot;
              &#125;
            ]
          &#125;,
          &quot;field&quot;: &quot;automatic_events&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

#### Lead gen event example

```json
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;102290129340398&quot;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;messaging_product&quot;: &quot;whatsapp&quot;,
            &quot;metadata&quot;: &#123;
              &quot;display_phone_number&quot;: &quot;15550783881&quot;,
              &quot;phone_number_id&quot;: &quot;106540352242922&quot;
            &#125;,
            &quot;automatic_events&quot;: [
              &#123;
                &quot;id&quot;: &quot;wamid.HBgLMTIwNjY3NzQ3OTgVAgASGBQzQUY3MDVCQzFBODE5ODU4MUZEOQA=&quot;,
                &quot;event_name&quot;: &quot;LeadSubmitted&quot;,
                &quot;timestamp&quot;: 1749069089,
                &quot;ctwa_clid&quot;: &quot;Afc3nYt4TTydumlFFsatFz8bR2yHCtVA92Veu_zDE4DgAI-QqCwM6eC3-K3lTGHRiLxRTVXFEsdyKQQSa-2obZyuGBq_EYypt_OwbMihBV0pbUoRmrGnEjwFTHop-Px0TfA&quot;
              &#125;
            ]
          &#125;,
          &quot;field&quot;: &quot;automatic_events&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

#### Purchase event structure

```html
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;&quot;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;messaging_product&quot;: &quot;whatsapp&quot;,
            &quot;metadata&quot;: &#123;
              &quot;display_phone_number&quot;: &quot;&lt;BUSINESS_DISPLAY_PHONE_NUMBER&gt;&quot;,
              &quot;phone_number_id&quot;: &quot;&lt;BUSINESS_PHONE_NUMBER_ID&gt;&quot;
            &#125;,
            &quot;automatic_events&quot;: [
              &#123;
                &quot;id&quot;: &quot;&lt;WHATSAPP_MESSAGE_ID&gt;&quot;,
                &quot;event_name&quot;: &quot;Purchase&quot;,
                &quot;timestamp&quot;: &lt;WEBHOOK_TRIGGER_TIMESTAMP&gt;,
                &quot;ctwa_clid&quot;: &quot;&lt;CLICK_ID&gt;&quot;,
                &quot;custom_data&quot;: &#123;
                  &quot;currency&quot;: &quot;&lt;CURRENCY_CODE&gt;&quot;,
                  &quot;value&quot;: &lt;AMOUNT&gt;
                &#125;
              &#125;
            ]
          &#125;,
          &quot;field&quot;: &quot;automatic_events&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

#### Purchase event example

```json
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;102290129340398&quot;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;messaging_product&quot;: &quot;whatsapp&quot;,
            &quot;metadata&quot;: &#123;
              &quot;display_phone_number&quot;: &quot;15550783881&quot;,
              &quot;phone_number_id&quot;: &quot;106540352242922&quot;
            &#125;,
            &quot;automatic_events&quot;: [
              &#123;
                &quot;id&quot;: &quot;wamid.HBgLMTIwNjY3NzQ3OTgVAgARGBIwRkU4NDI5Nzk3RjZDMzE2RUMA&quot;,
                &quot;event_name&quot;: &quot;Purchase&quot;,
                &quot;timestamp&quot;: 1749069131,
                &quot;ctwa_clid&quot;: &quot;Afc3nYt4TTydumlFFsatFz8bR2yHCtVA92Veu_zDE4DgAI-QqCwM6eC3-K3lTGHRiLxRTVXFEsdyKQQSa-2obZyuGBq_EYypt_OwbMihBV0pbUoRmrGnEjwFTHop-Px0TfA&quot;,
                &quot;custom_data&quot;: &#123;
                  &quot;currency&quot;: &quot;USD&quot;,
                  &quot;value&quot;: 25000
                &#125;
              &#125;
            ]
          &#125;,
          &quot;field&quot;: &quot;automatic_events&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

### Step 3: Trigger webhooks

To trigger an **automatic_events** webhook:

1. Access your test implementation of Embedded Signup.
1. Authenticate the flow using a business that has a Click-to-WhatsApp ad already configured.
1. In the [Business Asset Creation Screen](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/default-flow#business-asset-creation-screen), check the **Instruct Meta to automatically identify order and lead events** checkbox, and complete the flow.
1. Access the Click-to-WhatsApp ad and click it to send a message to the business.
1. Use the business to reply to the message with one of the strings below (must be exact).

- **For a purchase event:** _Your tracking number is AB123456789BR_
- **For a lead gen event:** _I am interested in learning more about the product_

After you have triggered both **automatic_events** webhook payloads, confirm that your webhook callback has processed each webhook according to your business needs.

### Step 4: Report each event using Conversions API (optional)

You can optionally report each event using the [Conversions API](https://developers.facebook.com/documentation/ads-commerce/conversions-api/business-messaging). Include any relevant values from the event webhook, as appropriate.

See [Send events via Conversions API](https://developers.facebook.com/documentation/ads-commerce/conversions-api/business-messaging#send-events-via-the-conversions-api-2) for additional information about reporting events.

#### Lead gen syntax

```html
curl &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;DATASET_ID&gt;/events&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039; \
-d &#039;
&#123;
  &quot;data&quot;: [
    &#123;
      &quot;event_name&quot;: &quot;LeadSubmitted&quot;,
      &quot;event_time&quot;: &lt;WEBHOOK_TRIGGER_TIMESTAMP&gt;,
      &quot;action_source&quot;: &quot;business_messaging&quot;,
      &quot;messaging_channel&quot;: &quot;whatsapp&quot;,
      &quot;user_data&quot;: &#123;
        &quot;ctwa_clid&quot;: &quot;&lt;CLICK_ID&gt;&quot;
      &#125;,
      &quot;messaging_outcome_data&quot;: &#123;
        &quot;outcome_type&quot;: &quot;automatic_events&quot;
      &#125;
    &#125;
  ]
&#125;
&#039;
```

#### Purchase event syntax

```html
curl &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;DATASET_ID&gt;/events&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039; \
-d &#039;
&#123;
  &quot;data&quot;: [
    &#123;
      &quot;event_name&quot;: &quot;Purchase&quot;,
      &quot;event_time&quot;: &lt;WEBHOOK_TRIGGER_TIMESTAMP&gt;,
      &quot;action_source&quot;: &quot;business_messaging&quot;,
      &quot;messaging_channel&quot;: &quot;whatsapp&quot;,
      &quot;user_data&quot;: &#123;
        &quot;ctwa_clid&quot;: &quot;&lt;CLICK_ID&gt;&quot;
      &#125;,
      &quot;custom_data&quot;: &#123;
        &quot;currency&quot;: &quot;&lt;CURRENCY_CODE&gt;&quot;,
        &quot;value&quot;: &lt;AMOUNT&gt;
      &#125;,
      &quot;messaging_outcome_data&quot;: &#123;
        &quot;outcome_type&quot;: &quot;automatic_events&quot;
      &#125;
    &#125;
  ]
&#125;
&#039;
```

## Enabling and disabling via Meta Business Suite

Business customers who have already been onboarded via Embedded Signup can enable automatic event identification using Meta Business Suite.

If a business customer who you have already onboarded wants to enable this feature, you can send them these instructions:

1. Access Meta Business Suite at [https://business.facebook.com](https://business.facebook.com).
1. Navigate to **Settings** &gt; **Accounts** &gt; **WhatsApp accounts** and click your WhatsApp Business account.
1. Scroll down to **Privacy and data sharing** in the **Summary** tab.
1. Use the &quot;**Automatically identify**... &quot; toggles to enable or disable automatic event identification as desired.
