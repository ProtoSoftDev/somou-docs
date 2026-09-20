# Viewing metrics



**Warning:** Conversion metrics will be solely available in the WhatsApp Manager UI and WhatsApp Business Management API that businesses use with Cloud API in October 2025.

As a result, Meta will deprecate the following conversion metrics:

* Viewing conversion metrics via Ads Manager UI (**September 8th, 2025**).
* Viewing conversion metrics via Ads Insights API (**Q1 2026**).

Businesses that use Marketing Messages API for WhatsApp can view metrics from 4 surfaces:

* Via WhatsApp Business Platform surfaces
  * WhatsApp Manager UI
  * [WhatsApp Business Management API](https://developers.facebook.com/documentation/business-messaging/whatsapp/about-the-platform#whatsapp-business-management-api)
* Via Ads surfaces (optional)
  * Ads Manager UI &quot;Marketing Messages&quot; tab
  * Marketing API &quot;[Insights API](https://developers.facebook.com/documentation/ads-commerce/marketing-api/insights)&quot;

| ROI Reporting | WhatsApp Business Management surfaces | Ads surfaces |
| --- | --- | --- |
| Messages sent, delivered, read | Y | Y |
| Total amount spent | Y | Y |
| Cost per delivery | Y | Y |
| CTA URL link clicks | Y | Y |
| Cost per click | Y | Y |
| CTA URL link click rate | N | Y |
| Add to cart (Web + App) | Y | Y`*` |
| Checkout initiated (Web + App) | Y | Y`*` |
| Purchase, purchase value (Web + App) | Y | Y`*` |
| App Activations | Y | Y`*` |
| Quick Replies | Y | Y |

`*` Requires a business to report this conversion event via Meta Pixel or Conversions API for App Events [see Get started with the Meta Pixel and Conversions API](https://www.facebookblueprint.com/student/activity/212737).

## View metrics via UIs

After sending Marketing Messages via Marketing Messages API for WhatsApp, view read-only metrics on sends, clicks, and conversions from two UIs:

1. WhatsApp Manager
1. Ads Manager &quot;Marketing Messages&quot; tab

Marketing Messages API for WhatsApp metrics can be viewed in WhatsApp Manager on both Phone Number and Template screens:

### Benchmarks and recommendations metrics

Benchmark metrics provide insights into how your business is performing compared to similar businesses in your industry. These metrics are based on data from the past 30 days and take into account various factors that define similar businesses. Based on the benchmark metrics, we provide personalized recommendations to help you improve your template&#039;s performance. If your template&#039;s read rate or click rate falls below the benchmark, we provide suggestions to boost engagement.

### Calculating benchmarks

To calculate benchmark metrics, we consider the following characteristics:

* **Business Country or Region**: We use the business country as the default cohort, but if the cohort size is too small, we switch to the business region.
* **Business Industry**: We compare your business with others in the same industry or vertical to provide relevant benchmarks.
* **Template Categories**: We only compare templates within the same category (for example, marketing templates with other marketing templates) to ensure accurate and relevant benchmarks.

We then calculate two key benchmark metrics:

* **Read Rate Benchmark**: We calculate this metric as the 75th percentile of read rates across similar businesses, representing the percentage of messages read out of total messages delivered.
* **Click Rate Benchmark**: We calculate this metric as the 75th percentile of click rates across similar businesses, representing the percentage of link clicks out of total messages delivered.

### Understanding your ranking and how to use benchmark metrics

When you view your benchmark metrics, you will see a ranking that indicates how your template performs compared to templates in the same category. This ranking is calculated by comparing your template&#039;s performance with the read rate or click rate performance of peer templates with high engagement over the past 30 days.

Use the benchmark metrics to compare your template&#039;s performance to templates from similar businesses over the past 30 days. Benchmarks are calculated daily, with a delay of up to 2 days. This ensures that you have access to updated and relevant data to inform your business decisions.

To access the benchmark and recommendations metrics:

1. Go to the WhatsApp Manager and select &quot;Manage templates&quot;.
2. Choose the template you want to view.
3. Select the &quot;Marketing Messages API for WhatsApp&quot; option from the dropdown menu highlighted in red.
4. The benchmark metrics and recommendation cards will be displayed below the preview card in the left panel.

### Error metrics

You can see a summary of error messages your template encountered within a given period of time by navigating to the [**WhatsApp Manager**](https://business.facebook.com/latest/whatsapp_manager/) &gt; **Message templates** &gt; **Manage templates** panel and clicking on the template. The **Error messages** section displays the errors.

The period of time can be defined using the date selector dropdown at the top of the page. See [Cloud API error codes](https://developers.facebook.com/documentation/business-messaging/whatsapp/support/error-codes) for a list of error codes and their descriptions.

The most frequently encountered message delivery errors are displayed in the **Summary** tab:

This information is also displayed as trend lines in the **Trend** tab:

## View metrics via APIs

After registering in Marketing Messages API for WhatsApp, analytics for a business&#039;s marketing message templates sent via the API are available from two APIs:

1. The WhatsApp Business Management API (does not include conversions)
1. The Marketing API &quot;Insights API&quot; (includes conversions)

### Benchmark metrics

You can get benchmark metrics via Insights API for read rates and click rates by requesting the following fields:

- `marketing_messages_read_rate_benchmark`
- `marketing_messages_click_rate_benchmark`

#### Syntax

```html
curl
&#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;AD_GROUP_ID&gt;/insights?fields=&lt;METRICS&gt;&#039; \
-H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039;
```

#### Example query

This example query returns the number of messages read, their read rate, and the benchmark read rate for comparison, as well as the number of button link clicks, their click rate, and the benchmark click rate for comparison, with a default lookback of 30 days:

```curl
curl &#039;https://graph.facebook.com/v17.0/120229306178900226/insights?fields=marketing_messages_read,marketing_messages_read_rate,marketing_messages_read_rate_benchmark,marketing_messages_link_btn_click,marketing_messages_link_btn_click_rate,marketing_messages_click_rate_benchmark&#039; \
-H &#039;Authorization: Bearer EAACE...&#039;
```

#### Example response

```json
&#123;
  &quot;data&quot;: [
    &#123;
      &quot;date_start&quot;: &quot;2025-05-11&quot;,
      &quot;date_stop&quot;: &quot;2025-06-09&quot;,
      &quot;marketing_messages_read&quot;: &quot;265&quot;,
      &quot;marketing_messages_read_rate&quot;: &quot;481.818182&quot;,
      &quot;marketing_messages_read_rate_benchmark&quot;: &quot;70.27&quot;,
      &quot;marketing_messages_link_btn_click&quot;: &quot;59&quot;,
      &quot;marketing_messages_link_btn_click_rate&quot;: &quot;107.272727&quot;,
      &quot;marketing_messages_click_rate_benchmark&quot;: &quot;18.74&quot;
    &#125;
  ],
  &quot;paging&quot;: &#123;
    &quot;cursors&quot;: &#123;
      &quot;after&quot;: &quot;MAZDZD&quot;,
      &quot;before&quot;: &quot;MAZDZD&quot;
    &#125;
  &#125;
&#125;
```

### Measuring ROI with the Marketing API &quot;Insights API&quot; endpoint (richer analytics, recommended)

Businesses can use the [Meta Pixel](https://www.facebook.com/business/tools/meta-pixel) and [Conversions API for App Events](https://developers.facebook.com/documentation/ads-commerce/conversions-api/app-events) to send signals to Meta when customers take an action on their website or app, after clicking a URL in a marketing message. Note that in-thread conversion optimizations and reporting are not yet available for Marketing Messages API for WhatsApp.

The [Insights API](https://developers.facebook.com/documentation/ads-commerce/marketing-api/insights) is an interface to retrieve ads statistics, allowing a business to view all of the metrics of the Template Analytics plus additional metrics on when marketing messages sent via Marketing Messages API for WhatsApp led to an event reported from their website via Meta Pixel or Conversions API, like a user adding to cart or checking out. **It is required that the business that owns the Pixel is the same business that owns the WABA.**

#### Step 1: Fetch Ad IDs for Templates, using the Templates endpoint

When a business has registered for Marketing Messages API for WhatsApp, Meta creates a read-only Ad account for each WABA under their BMID, and links any marketing message templates under the WABA to an Ad object in that Ad account. Meta also links each marketing message template under the WABA to an Ad set. These IDs are needed when calling the Insights API to retrieve metrics on marketing campaigns sent via Marketing Messages API for WhatsApp.

Once a business has registered for the Marketing Messages API for WhatsApp, the Business Management API&#039;s Template endpoint will return an additional parameter reflecting their Ad IDs.

* `ad_id`
* `ad_account_id`
* `ad_campaign_id`
* `ad_adset_id`

These fields indicate the Ad id of linked Ads object for each marketing message template.

Call the Template endpoint to retrieve the Ad IDs for each ad entity linked to marketing message templates, for calling the Insights API later.

| Endpoint | Authentication |
| --- | --- |
| `/WHATSAPP_BUSINESS_ACCOUNT_ID/message_templates` | Developers can authenticate their API calls with the access token generated in the **App Dashboard &gt; WhatsApp &gt; API Setup**.&lt;br&gt;&lt;br&gt;Business messaging partners must authenticate themselves with an access token with the `whatsapp_business_messaging` permission. |

Request Syntax

```curl
GET /&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;/message_templates
  ?fields=category,ad_id,ad_adset_id,ad_campaign_id,ad_account_id
```

See docs: [GET Message Templates](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/overview#retrieve-templates)

If you only want to fetch 1 Template at a time, you can fetch the Ad ID fields at a Template level.

| Endpoint | Authentication |
| --- | --- |
| `/&lt;TEMPLATE_ID&gt;/` | Developers can authenticate their API calls with the access token generated in the **App Dashboard &gt; WhatsApp &gt; API Setup**.&lt;br&gt;&lt;br&gt;Business messaging partners must authenticate themselves with an access token with the `whatsapp_business_management` permission. |

Request Syntax

```curl
GET /&lt;TEMPLATE_ID&gt;
  ?fields=category,ad_id,ad_adset_id,ad_campaign_id,ad_account_id
```

See docs: [GET Templates](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/message-template-api#fields)

#### Step 2: Call the Insights API using the Ad IDs

Direct integrators or business messaging partners retrieve these read-only campaign objects through the existing Insights API endpoints:

| Endpoint | Authentication |
| --- | --- |
| `/AD_CAMPAIGN_ID/insights` | Partners must authenticate themselves with an access token with the `ads_read` permission. |

Get the specified metrics at an ad object level (that is to say, ad account, campaign, ad set, or ad id level) given its ID:

### Example request

```curl
curl --verbose -s -G -d &quot;access_token=$&#123;ACCESS_TOKEN&#125;&quot; https://graph.facebook.com/v19.0/$&#123;AD_ACCOUNT_ID|CAMPAIGN_ID|AD_SET_ID|AD_ID&#125;/insights?fields=marketing_messages_sent%2Cmarketing_messages_read&quot;
```

### Example response

```curl
&#123;
  &quot;data&quot;: [
    &#123;
      &quot;marketing_messages_sent&quot;: &quot;2&quot;,
      &quot;marketing_messages_read&quot;: &quot;1&quot;,
      &quot;date_start&quot;: &quot;2023-09-24&quot;,
      &quot;date_stop&quot;: &quot;2023-10-23&quot;
    &#125;
  ],
  &quot;paging&quot;: &#123;
    &quot;cursors&quot;: &#123;
      &quot;before&quot;: &quot;MAZDZD&quot;,
      &quot;after&quot;: &quot;MAZDZD&quot;
    &#125;
  &#125;
&#125;
```

This is only an example. For all available params of the API see the full doc: [Insights API docs](https://developers.facebook.com/documentation/ads-commerce/marketing-api/insights)

All available insights fields are listed below:

* Sent, Read, Delivered, Click
  * marketing_messages_sent
  * marketing_messages_read
  * marketing_messages_delivered
  * marketing_messages_link_btn_click
* Rates
  * marketing_messages_delivery_rate
  * marketing_messages_read_rate
  * marketing_messages_link_btn_click_rate
* Spend metrics
  * marketing_messages_spend
  * marketing_messages_cost_per_delivered
  * marketing_messages_cost_per_link_btn_click
* Conversion events
  * marketing_messages_website_add_to_cart
  * marketing_messages_website_initiate_checkout
  * marketing_messages_website_purchase
  * marketing_messages_website_purchase_values
  * marketing_messages_app_add_to_cart
  * marketing_messages_app_initiate_checkout
  * marketing_messages_app_purchase
  * marketing_messages_app_purchase_values

### Measuring ROI with the WhatsApp Business Management API &quot;Template Analytics&quot; endpoint (basic analytics)

The [Template Analytics](https://developers.facebook.com/documentation/business-messaging/whatsapp/analytics#template-analytics) endpoint of the WhatsApp Business Management API offers the ability to view metrics including: Sent, Delivered, Read, Clicked, and Cost.

In order to fetch metrics for messages sent via Marketing Messages API for WhatsApp, attach the new parameter &quot;product_type&quot; with the value below. If omitted, the API returns only analytics for Cloud API.

| Endpoint | Authentication |
| --- | --- |
| `/WHATSAPP_BUSINESS_ACCOUNT_ID/conversation_analytics`&lt;br&gt;&lt;br&gt;Use the query parameter `conversation_categories` = `MARKETING_MESSAGES` to include data from the Marketing Messages API for WhatsApp.&lt;br&gt;&lt;br&gt;If omitted, the API will return results for all conversation categories. | Developers can authenticate their API calls with the access token generated in the **App Dashboard &gt; WhatsApp &gt; API Setup**.&lt;br&gt;&lt;br&gt;Business messaging partners must authenticate themselves with an access token with the `whatsapp_business_management` permission. |

Request Syntax

```curl
GET /WHATSAPP_BUSINESS_ACCOUNT_ID/?fields=conversation_analytics.start(&lt;START_TIMESTAMP&gt;).end(&lt;END_TIMESTAMP&gt;).granularity(DAILY).conversation_categories(MARKETING_LITE).dimensions([&quot;CONVERSATION_CATEGORY&quot;])
```

See docs: [GET Conversation Analytics](https://developers.facebook.com/documentation/business-messaging/whatsapp/analytics#conversation-analytics)
