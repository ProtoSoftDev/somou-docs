# Tracking click events



_Available using Marketing Messages API for WhatsApp (MM API for WhatsApp) and Ads Manager only_

We deliver a webhook payload when users click on the body or call-to-action of your marketing message. You can subscribe to this webhook to capture this data and use it to inform your campaign decisions.

## Limitations

- At the moment, this feature is not available for all users
- Click events are only available for messages sent in the last 7 days

## Webhooks

To receive this webhook, subscribe to the `whatsapp_business_account` webhook topic. The webhook payload is on the `messages` field and is delivered as below:

```json
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
            &quot;user_actions&quot;: [
              &#123;
                &quot;action_type&quot;: &quot;marketing_messages_link_click&quot;,
                &quot;timestamp&quot;: &quot;&lt;time_of_click&gt;&quot;,
                &quot;marketing_messages_link_click_data&quot;: &#123;
                  &quot;click_component&quot;: &quot;cta&quot; | &quot;body&quot;,
                  &quot;product_id&quot;: &quot;sku_id&quot;,
                  &quot;click_id&quot;: &quot;click_id&quot;,
                  &quot;tracking_token&quot;: &quot;example_token&quot;
                &#125;
              &#125;
            ]
          &#125;,
          &quot;field&quot;: &quot;messages&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

| Field Name | Field Type | Field Description |
| --- | --- | --- |
| `action_type` | **Required**&lt;br&gt;String | Name of the action |
| `timestamp` | **Required**&lt;br&gt;Unix timestamp | Timestamp of when the event happened |
| `click_component` | **Optional**&lt;br&gt;Enum | The click action&lt;br&gt;Can either be `cta` or `body` |
| `click_id` | **Optional**&lt;br&gt;String | The unique identifier for the click. Is also appended to the original URL when the user visits the URL. |
| `tracking_token` | **Optional**&lt;br&gt;String | Internal Meta token for processing and tracking |
| `product_id` | **Optional**&lt;br&gt;String | ID of the product, if it was assigned in Ads Manager or Marketing API. |

