# Set a max price for marketing messages (BETA)



**Note:** Amidst the introduction of the max price feature on the Marketing Messages API for WhatsApp, there is no change to how Meta charges on the WhatsApp Business Platform. Meta continues to charge on a per-message basis, as outlined in [WhatsApp pricing](https://developers.facebook.com/documentation/business-messaging/whatsapp/pricing).

The max price feature is available in Limited Beta as of May 15 and will be **optional** throughout 2026. Meta plans to make max price generally available — and required — in Q2 2027.

**Warning:** **Action required: `bid_spec` is being replaced by `optimization_spec`.** As of June 18, 2026, set the max price using the new `optimization_spec` object. The `bid_amount` and `bid_strategy` fields are unchanged — only the enclosing object name changes. Both `bid_spec` and `optimization_spec` are accepted until July 31, 2026, after which `bid_spec` is no longer supported. Update your create, retrieve, and update calls before July 31, 2026.

## What is a max price?

In 2026, Meta is introducing new pricing features on the Marketing Messages API for WhatsApp to enable businesses to control and optimize spend for their marketing messaging campaigns.

The first pricing feature allows you to **set a maximum price (max price) per marketing message delivery; when a max price is set, Meta will charge that max price or lower for delivery**. Businesses can choose to set a max price the same as, lower than, or higher than the published rate to achieve their objectives per campaign.

- *Lower costs while maintaining delivery rates similar to current WhatsApp campaigns*, by setting max prices the same as published rates.
- Reach more customer cohorts on WhatsApp at lower cost, by setting max prices lower than published rates.
- Increase delivery rates during periods such as holidays and peak sales periods, by setting max prices higher than published rates.

The second pricing feature is the **reach estimation tool**, which helps businesses set the right max price by helping them understand estimated delivery rates and costs at different max prices.

### Max price explainer

The max price feature allows you to set the maximum price you are willing to pay per message delivery. Meta charges your max price or lower. In the API, you express this as a `bid_amount` value per 1,000 deliveries within the `optimization_spec` object.

- [Max price explainer PDF](https://meta.highspot.com/items/69aedbc000c74039fc1633d7#1)

## Phased roll-out of the max price feature

The max price feature rolls out in three phases:

1. Limited Beta starting **May 15, 2026** -- Any partner and any directly integrated business can integrate and use the max price feature and reach estimation tool. Each partner can enable these features for a limited number of clients.
2. Open Beta starting **October 2026** -- Any partner can enable these features for all their clients.
3. General Availability (GA) as of **Q2 2027** -- The max price feature will become required in eligible geographies and fixed, published rates for marketing messages will only apply on the Cloud API.

## Before you begin

To use the max price feature, you must:

- Have an active WhatsApp Business account that has been [onboarded](https://developers.facebook.com/documentation/business-messaging/whatsapp/marketing-messages/onboarding) to the Marketing Messages API for WhatsApp.
- Be in a [country eligible for MM API for WhatsApp](https://developers.facebook.com/documentation/business-messaging/whatsapp/marketing-messages/get-started#geographic-availability-of-features).

## Recommendations

**Note:** Max price is an early-stage feature; performance may vary as Meta&#039;s systems learn and improve.

1. **Set your max price at the template level.** The `bid_amount` in `optimization_spec` is what Meta&#039;s delivery system optimizes against. Setting the right max price when you create the template gives the system the best signal for delivery optimization.

2. **Use `per_message_bid_multiplier` for individual message adjustments only.** The `per_message_bid_multiplier` scales the template&#039;s `bid_amount` up or down for individual messages, but the delivery system generally gives better performance optimizing based on the original template-level `bid_amount` for bulk changes.

   For example, if you set a template&#039;s `bid_amount` to 50,000 and then apply a multiplier of 2.0 on every single message, delivery performance might differ from setting the template&#039;s `bid_amount` to 100,000 directly — even though the effective max price is the same. For best results, set the bid at the template level and update the template&#039;s `optimization_spec` as needed rather than changing the message level multiplier as a workaround for such cases.

3. **Ramp up traffic gradually.** When sending messages with a new max price template for the first time, increase volume slowly before sending at scale. This aligns with [Template pacing](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/template-pacing) best practices and helps the delivery system optimize effectively.

4. **Start with A/B tests.** This helps you assess the impact of different bidding rates on your delivery rates and cost per delivery. Run a four-arm test (10,000 send requests per arm) with a similar audience and time window, with the following conditions:
   - Arm A: No max price set
   - Arm B: Max price set at rate card
   - Arm C: Max price set at 1.5x rate card (1.2x for India and Saudi Arabia)
   - Arm D: Max price set at 0.9x rate card

## Create templates with max price

Use the [Message Templates API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/message-template-api#post-version-waba-id-message-templates) to set a maximum price and include the `optimization_spec` object in the request body.

**Note:** `optimization_spec` replaces `bid_spec` as of June 18, 2026. `bid_spec` continues to be accepted until July 31, 2026, after which it is no longer supported. The `bid_amount` and `bid_strategy` fields are identical in both objects.

### Request syntax

```html
curl &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;/message_templates&#039; \
  -H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039; \
  -H &#039;Content-Type: application/json&#039; \
  -d &#039;
  &#123;
    &quot;name&quot;: &quot;seasonal_sale_promo&quot;,
    &quot;category&quot;: &quot;MARKETING&quot;,
    &quot;language&quot;: &quot;en&quot;,
    &quot;components&quot;: [
      &#123;
        &quot;type&quot;: &quot;BODY&quot;,
        &quot;text&quot;: &quot;Shop our seasonal sale! Up to 50% off selected items.&quot;
      &#125;
    ],
    &quot;optimization_spec&quot;: &#123;
      &quot;bid_amount&quot;: &lt;BID_AMOUNT&gt;,
      &quot;bid_strategy&quot;: &quot;LOWEST_COST_WITH_BID_CAP&quot;
    &#125;
&#125;&#039;
```

If `optimization_spec` is not included, the template uses standard rate card pricing.

### Request parameters

| Placeholder | Description | Example Value |
| --- | --- | --- |
| `&lt;ACCESS_TOKEN&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;[System token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#system-user-access-tokens) or [business token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#business-integration-system-user-access-tokens). | `EAAA...` |
| `&lt;API_VERSION&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Graph API version. | v25.0 |
| `&lt;WABA_ID&gt;`&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;WhatsApp Business Account ID. | `102290129340398` |
| `&lt;BID_AMOUNT&gt;`&lt;br&gt;&lt;br&gt;_int32_ | **Required.**&lt;br&gt;&lt;br&gt;Maximum price per 1,000 message deliveries, expressed in your WABA currency&#039;s smallest unit (cents for USD, paise for INR, peso for MXN). See [supported currencies](https://developers.facebook.com/documentation/business-messaging/whatsapp/pricing#updates-to-rate-cards) for a list of currencies. | `87000` |
| `&lt;BID_STRATEGY&gt;`&lt;br&gt;&lt;br&gt;_Enum_ | **Required.**&lt;br&gt;&lt;br&gt;The bid strategy to use. Currently supports only `LOWEST_COST_WITH_BID_CAP`. | `LOWEST_COST_WITH_BID_CAP` |

### Calculating max price amounts

The `bid_amount` represents your max price per 1,000 deliveries in your WABA currency&#039;s smallest unit. To convert from your desired per-delivery price:

- Convert your desired per-delivery price to your WABA currency&#039;s smallest unit
- Multiply by 1,000 to express the value per 1,000 deliveries

**Example**: To set a max price of ₹0.87 per delivery:

- Convert to paise: 0.87 Rupees = 87 paise
- Multiply by 1,000: 87 x 1,000 = 87,000

Set `bid_amount` to `87000`.

**Example**: To set a max price of $0.05 USD per delivery:

- Convert to cents: $0.05 = 5 cents
- Multiply by 1,000: 5 x 1,000 = 5,000

Set `bid_amount` to `5000`.

## Retrieve max price information

Use the [Template API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/message-template-api#get-version-template-id) to get the max price setting on an existing template by querying the `optimization_spec` field.

**Note:** Query `optimization_spec` as of June 18, 2026. Querying `bid_spec` continues to return the max price configuration until July 31, 2026, after which only `optimization_spec` is supported.

### Request syntax

```html
curl &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;TEMPLATE_ID&gt;/?fields=optimization_spec&#039; \
  -H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039;
```

### Request parameters

| Placeholder | Description | Example Value |
| --- | --- | --- |
| `&lt;ACCESS_TOKEN&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;[System token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#system-user-access-tokens) or [business token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#business-integration-system-user-access-tokens). | `EAAA...` |
| `&lt;API_VERSION&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Graph API version. | v25.0 |
| `&lt;TEMPLATE_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;ID of the WhatsApp message template. | `1733678867511493` |

### Example response

```json
&#123;
  &quot;optimization_spec&quot;: &#123;
    &quot;bid_strategy&quot;: &quot;LOWEST_COST_WITH_BID_CAP&quot;,
    &quot;bid_amount&quot;: 87000
  &#125;,
  &quot;id&quot;: &quot;1733678867511493&quot;
&#125;
```

## Update max price for templates

Use the [Template API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/message-template-api#post-version-template-id) to update the max price setting on an existing template.

You can update the `optimization_spec` on templates that were originally created with a max price. The same parameters apply.

You cannot add `optimization_spec` to an existing template that was created without it. You must create a new template with `optimization_spec` included.

**Note:** Send updates using `optimization_spec` as of June 18, 2026. Updates sent with `bid_spec` continue to be accepted until July 31, 2026, after which only `optimization_spec` is supported.

Other constraints follow the standard [template editing limits](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/overview):

- **Approved templates**: Up to 100 edits per hour, 2,400 per day. Content edits still follow the existing limit of 1 per day and 10 per 30 days.
- **Rejected or paused templates**: Unlimited edits

### Request syntax

```html
curl &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;TEMPLATE_ID&gt;/&#039; \
  -H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039; \
  -H &#039;Content-Type: application/json&#039; \
  -d &#039;&#123;
  &quot;optimization_spec&quot;: &#123;
    &quot;bid_strategy&quot;: &quot;LOWEST_COST_WITH_BID_CAP&quot;,
    &quot;bid_amount&quot;: &lt;BID_AMOUNT&gt;
  &#125;
&#125;&#039;
```

### Request parameters

| Placeholder | Description | Example Value |
| --- | --- | --- |
| `&lt;ACCESS_TOKEN&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;[System token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#system-user-access-tokens) or [business token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#business-integration-system-user-access-tokens). | `EAAA...` |
| `&lt;API_VERSION&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Graph API version. | v25.0 |
| `&lt;TEMPLATE_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;ID of the WhatsApp message template. The template must have been originally created with `optimization_spec`. | `1733678867511493` |
| `&lt;BID_AMOUNT&gt;`&lt;br&gt;&lt;br&gt;_int32_ | **Required.**&lt;br&gt;&lt;br&gt;Updated maximum price per 1,000 message deliveries, expressed in your WABA currency&#039;s smallest unit. | `4000` |

## Adjust max price when sending messages

**Warning:** The message-level max price multiplier is subject to change during the beta period.

Use the [Marketing Messages API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/marketing-messages-api-for-whatsapp) to apply a multiplier at sending time to adjust the template-level max price for individual messages. This allows you to adjust the max price for individual messages without editing the template.

### Request syntax

```html
curl &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;WHATSAPP_BUSINESS_PHONE_NUMBER_ID&gt;/marketing_messages&#039; \
  -H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039; \
  -H &#039;Content-Type: application/json&#039; \
  -d &#039;&#123;
  &quot;recipient_type&quot;: &quot;individual&quot;,
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;to&quot;: &quot;&lt;WHATSAPP_USER_PHONE_NUMBER&gt;&quot;,
  &quot;type&quot;: &quot;template&quot;,
  &quot;template&quot;: &#123;
    &quot;name&quot;: &quot;seasonal_sale_promo&quot;,
    &quot;language&quot;: &#123;
      &quot;code&quot;: &quot;en&quot;
    &#125;
  &#125;,
  &quot;bid_spec&quot;: &#123;
    &quot;per_message_bid_multiplier&quot;: &quot;&lt;PER_MESSAGE_BID_MULTIPLIER&gt;&quot;
  &#125;
&#125;&#039;
```

In this example, the multiplier of 1.5 increases the template&#039;s `bid_amount` by 50%. If the template&#039;s `bid_amount` is 2000, the effective max price for this message becomes 3000.

### Request parameters

| Placeholder | Description | Example Value |
| --- | --- | --- |
| `&lt;ACCESS_TOKEN&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;[System token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#system-user-access-tokens) or [business token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#business-integration-system-user-access-tokens). | `EAAA...` |
| `&lt;API_VERSION&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Graph API version. | v25.0 |
| `&lt;WHATSAPP_BUSINESS_PHONE_NUMBER_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;WhatsApp business phone number ID. | `106540352242922` |
| `&lt;WHATSAPP_USER_PHONE_NUMBER&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;WhatsApp user phone number. | `+16505551234` |
| `&lt;PER_MESSAGE_BID_MULTIPLIER&gt;`&lt;br&gt;&lt;br&gt;_Float_ | **Optional.** Default: `1`&lt;br&gt;&lt;br&gt;A positive multiplier applied to the template&#039;s `bid_amount`. For example, `1.5` increases the max price by 50%, `0.5` decreases it by 50%, and `1` (default) uses the template&#039;s max price amount unchanged. | `1.5` |

## Estimate reach and costs

The Reach estimation helps you understand your expected deliveries and costs at different max price levels.

### Request syntax

Use the [WhatsApp Business Account API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/whatsapp-business-account-api#get-version-waba-id) to get the estimated delivery ranges and cost ranges at various max price amounts.

Meta generates estimates from historical data, and they are for informational and planning purposes only. They do not guarantee future delivery outcomes, costs, or performance. Actual results may differ due to changes in platform conditions or other variables.

The `targeting_spec` value must be serialized JSON. For example:

```html
curl &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;/reachestimate?targeting_spec=&#123;&quot;geo_locations&quot;:&#123;&quot;countries&quot;:[&quot;IN&quot;]&#125;&#125;&amp;date_interval=&lt;DATE_INTERVAL&gt;&#039; \
  -H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039;
```

### Request parameters

| Placeholder | Description | Example Value |
| --- | --- | --- |
| `&lt;ACCESS_TOKEN&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;[System token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#system-user-access-tokens) or [business token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#business-integration-system-user-access-tokens). | `EAAA...` |
| `&lt;API_VERSION&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Graph API version. | v25.0 |
| `&lt;WABA_ID&gt;`&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;WhatsApp Business Account ID. | `102290129340398` |
| `&lt;DATE_INTERVAL&gt;`&lt;br&gt;&lt;br&gt;_Enum_ | **Required.**&lt;br&gt;&lt;br&gt;Lookback period for the historical data used to generate estimates. One of: `L1D` (last 1 day), `L7D` (last 7 days), `L14D` (last 14 days), `L28D` (last 28 days). | `L7D` |
| `&lt;TARGETING_SPEC&gt;`&lt;br&gt;&lt;br&gt;_JSON_ | **Required.**&lt;br&gt;&lt;br&gt;Serialized JSON specifying geographic targeting. Must include `geo_locations` with a `countries` array. | `&#123;&quot;geo_locations&quot;:&#123;&quot;countries&quot;:[&quot;IN&quot;]&#125;&#125;` |

### Example response

```json
&#123;
  &quot;waba_currency&quot;: &quot;USD&quot;,
  &quot;estimates&quot;: [
    &#123;
      &quot;bid_amount&quot;: 400,
      &quot;users&quot;: 1000,
      &quot;deliveries_lower_bound&quot;: 500,
      &quot;deliveries_upper_bound&quot;: 570,
      &quot;cost_lower_bound&quot;: 389.74,
      &quot;cost_upper_bound&quot;: 390.74
    &#125;,
    &#123;
      &quot;bid_amount&quot;: 520,
      &quot;users&quot;: 1000,
      &quot;deliveries_lower_bound&quot;: 600,
      &quot;deliveries_upper_bound&quot;: 650,
      &quot;cost_lower_bound&quot;: 400.74,
      &quot;cost_upper_bound&quot;: 510.74
    &#125;
  ]
&#125;
```

The response contains multiple `estimates` entries at different max price amounts, allowing you to compare expected delivery volumes and costs across price points.

### Response fields

| Field | Description |
| --- | --- |
| `waba_currency` | The currency of your WhatsApp Business account. |
| `bid_amount` | Max price per 1,000 message deliveries, in the WABA currency&#039;s smallest unit. |
| `users` | Targeted user count. Fixed at 1,000 during beta. |
| `deliveries_lower_bound` | Lower bound of the estimated delivery range for this max price amount. |
| `deliveries_upper_bound` | Upper bound of the estimated delivery range for this max price amount. |
| `cost_lower_bound` | Lower bound of the estimated average cost per 1,000 deliveries, in the WABA currency&#039;s smallest unit. |
| `cost_upper_bound` | Upper bound of the estimated average cost per 1,000 deliveries, in the WABA currency&#039;s smallest unit. |

## Metrics and billing

Messages sent with or without the max price feature use the same **Marketing Lite** product type (SKU) for billing purposes.

Marketing messages sent with max price appear in analytics with the following identifiers:

- **Pricing Analytics** [`/&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;?fields=pricing_analytics`](https://developers.facebook.com/documentation/business-messaging/whatsapp/analytics#pricing-analytics): `pricing_category` = `MARKETING_LITE`
- **Template Analytics** [`/&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;?fields=template_analytics`](https://developers.facebook.com/documentation/business-messaging/whatsapp/analytics#template-analytics): `product_type` = `MARKETING_MESSAGES_LITE_API`

Webhooks use lowercase `marketing_lite` for `pricing.category`, while analytics APIs use uppercase `MARKETING_LITE` for `pricing_category`. When max price is enabled, the delivered and read status webhooks also include a `cost` object with the estimated charge for the message.

### Delivered/read webhook example (with max price)

```json
&quot;pricing&quot;: &#123;
  &quot;billable&quot;: true,
  &quot;pricing_model&quot;: &quot;PMP&quot;,
  &quot;category&quot;: &quot;marketing_lite&quot;,
  &quot;type&quot;: &quot;regular&quot;,
  &quot;cost&quot;: &#123;
    &quot;amount&quot;: 0.035,
    &quot;currency&quot;: &quot;USD&quot;
  &#125;
&#125;
```

#### Webhook cost fields

| Field | Type | Description |
| --- | --- | --- |
| `amount` | Float | Cost of the delivered or read message. Appears on `delivered` and `read` events. The unit is the currency base unit (dollar for USD, rupee for INR), consistent with the analytics API `cost` field. |
| `currency` | String | WABA-supported currency in ISO format. |

*The price reported in the delivered/read webhook may differ from the final charge on your invoice due to small variations in data processing (same as Analytics API). Your billing invoice is the source of truth for final pricing.*

### Pricing analytics response example

```json
&#123;
  &quot;pricing_analytics&quot;: &#123;
    &quot;data&quot;: [
      &#123;
        &quot;data_points&quot;: [
          &#123;
            &quot;start&quot;: 1748761200,
            &quot;end&quot;: 1748847600,
            &quot;country&quot;: &quot;IN&quot;,
            &quot;pricing_type&quot;: &quot;REGULAR&quot;,
            &quot;pricing_category&quot;: &quot;MARKETING_LITE&quot;,
            &quot;volume&quot;: 1,
            &quot;cost&quot;: 10
          &#125;
        ]
      &#125;
    ]
  &#125;
&#125;
```

### Template analytics response example

```json
&#123;
  &quot;data&quot;: [
    &#123;
      &quot;granularity&quot;: &quot;DAILY&quot;,
      &quot;product_type&quot;: &quot;MARKETING_MESSAGES_LITE_API&quot;,
      &quot;data_points&quot;: [
        &#123;
          &quot;template_id&quot;: &quot;1421988012088524&quot;,
          &quot;start&quot;: 1718064000,
          &quot;end&quot;: 1718150400,
          &quot;sent&quot;: 1,
          &quot;delivered&quot;: 1,
          &quot;read&quot;: 1,
          &quot;cost&quot;: [
            &#123;
              &quot;type&quot;: &quot;amount_spent&quot;,
              &quot;value&quot;: 0.01
            &#125;,
            &#123;
              &quot;type&quot;: &quot;cost_per_delivered&quot;,
              &quot;value&quot;: 0.01
            &#125;
          ]
        &#125;
      ]
    &#125;
  ]
&#125;
```

For more details on metrics, see [Viewing metrics](https://developers.facebook.com/documentation/business-messaging/whatsapp/marketing-messages/view-metrics).

## Error codes

| Code | Message | Possible reasons and solutions |
| --- | --- | --- |
| 131061 | Marketing templates containing bid_spec are not supported by the Cloud API. To use templates with bid_spec, please use the Marketing Messages API for WhatsApp. | You are sending a template that has a max price set (`optimization_spec`, or `bid_spec` before its July 31, 2026 deprecation) to the Cloud API `/messages` endpoint. Send to the `/marketing_messages` endpoint instead. |
| 100 | You need to sign the testing legal agreement before sending out messages. | You have not signed the testing legal agreement. Please sign the agreement to gain access to this feature. |

For a full list of error codes, see [Error codes](https://developers.facebook.com/documentation/business-messaging/whatsapp/support/error-codes).
