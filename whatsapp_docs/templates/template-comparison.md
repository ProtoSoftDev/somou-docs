# Template Comparison



You can compare two templates by examining how often each one is sent, which one has the lower ratio of blocks to sends, and each template&#039;s top reason for being blocked.

## Requirements

* A [User](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#user-access-tokens) or [System User](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#system-user-access-tokens) access token.
* The [whatsapp_business_management](https://developers.facebook.com/docs/permissions/reference/whatsapp_business_management) permission.

## Limitations

* Only two templates can be compared at a time.
* Both templates must be in the same WhatsApp Business account.
* Templates must have been sent at least 1,000 times in the query&#039;s specified timeframe.
* Lookback windows are limited to 7, 30, 60, and 90 days from the time of the request.

## Comparing templates

Use the [Template Comparison API](https://developers.facebook.com/docs/graph-api/reference/whats-app-business-hsm/compare) to target one template and compare it with another.

### Request syntax

```html
GET /&lt;WHATSAPP_MESSAGE_TEMPLATE_ID&gt;/compare
  ?template_ids=[&lt;TEMPLATE_IDS&gt;]
  &amp;start=&lt;START&gt;
  &amp;end=&lt;END&gt;
```

### Query parameters

| Placeholder | Description |
| --- | --- |
| `&lt;WHATSAPP_MESSAGE_TEMPLATE_ID&gt;` | ID of the WhatsApp Message Template to target. |
| `&lt;TEMPLATE_IDS&gt;` | ID of the WhatsApp Message Template to compare the target to. |
| `&lt;START&gt;` | Unix timestamp indicating start of timeframe. See [Timeframes](#timeframes). |
| `&lt;END&gt;` | Unix timestamp indicating end of timeframe. See [Timeframes](#timeframes). |

### Timeframes

Timeframes (lookback windows) are limited to 7, 30, 60, and 90 days from the time of the request. To define a timeframe, set your end date to the current time as a Unix timestamp, then subtract the number of days for your desired timeframe, in seconds, from that value:

Each value below is the number of seconds to subtract from the current Unix end timestamp:

* Subtract `604800` for a 7-day window.
* Subtract `2592000` for a 30-day window.
* Subtract `5184000` for a 60-day window.
* Subtract `7776000` for a 90-day window.

### Response

Upon success, the API returns a list of [WhatsApp Business Template Comparison](https://developers.facebook.com/docs/graph-api/reference/whats-app-business-hsm-comparison) nodes describing each template&#039;s block rate, number of times sent, and top reason for being blocked.

```json
&#123;
  &quot;data&quot;: [
    &#123;
      &quot;metric&quot;: &quot;BLOCK_RATE&quot;,
      &quot;type&quot;: &quot;RELATIVE&quot;,
      &quot;order_by_relative_metric&quot;: [&lt;ORDER_BY_RELATIVE_METRIC&gt;]
    &#125;,
    &#123;
      &quot;metric&quot;: &quot;MESSAGE_SENDS&quot;,
      &quot;type&quot;: &quot;NUMBER_VALUES&quot;,
      &quot;number_values&quot;: [&lt;NUMBER_VALUES&gt;]
    &#125;,
    &#123;
      &quot;metric&quot;: &quot;TOP_BLOCK_REASON&quot;,
      &quot;type&quot;: &quot;STRING_VALUES&quot;,
      &quot;string_values&quot;: [&lt;STRING_VALUES&gt;]
    &#125;
  ]
&#125;
```

### Response contents

| Placeholder | Description |
| --- | --- |
| `&lt;ORDER_BY_RELATIVE_METRIC&gt;` | Array of template ID strings, in increasing order of block rate (ratio of blocks to sends). |
| `&lt;NUMBER_VALUES&gt;` | Array of message send number value objects. Objects have the following properties:&lt;br&gt;&lt;br&gt;* `key` — _String._ WhatsApp Message Template ID.&lt;br&gt;* `value` — _Integer._ Number of times the template was sent. |
| `&lt;STRING_VALUES&gt;` | Array of top block reason string value objects. Objects have the following properties:&lt;br&gt;&lt;br&gt;* `key` — _String._ WhatsApp Message Template ID.&lt;br&gt;* `value` — _String._ Top block reason.&lt;br&gt;&lt;br&gt;Block reasons can be:&lt;br&gt;&lt;br&gt;* `NO_LONGER_NEEDED`&lt;br&gt;* `NO_REASON`&lt;br&gt;* `NO_REASON_GIVEN`&lt;br&gt;* `NO_SIGN_UP`&lt;br&gt;* `OFFENSIVE_MESSAGES`&lt;br&gt;* `OTHER`&lt;br&gt;* `OTP_DID_NOT_REQUEST`&lt;br&gt;* `SPAM`&lt;br&gt;* `UNKNOWN_BLOCK_REASON`&lt;br&gt;&lt;br&gt;See the [View metrics for your WhatsApp Business message template](https://www.facebook.com/business/help/511126334359303/) help center topic for descriptions of these reasons. |

### Example request

```html
curl -X GET &#039;https://graph.facebook.com/v25.0/5289179717853347/compare?template_ids=[1533406637136032]&amp;start=1674844791182&amp;end=1674845395982&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039;
```

### Example response

```json
&#123;
  &quot;data&quot;: [
    &#123;
      &quot;metric&quot;: &quot;BLOCK_RATE&quot;,
      &quot;type&quot;: &quot;RELATIVE&quot;,
      &quot;order_by_relative_metric&quot;: [
        &quot;1533406637136032&quot;,
        &quot;5289179717853347&quot;
      ]
    &#125;,
    &#123;
      &quot;metric&quot;: &quot;MESSAGE_SENDS&quot;,
      &quot;type&quot;: &quot;NUMBER_VALUES&quot;,
      &quot;number_values&quot;: [
        &#123;
          &quot;key&quot;: &quot;5289179717853347&quot;,
          &quot;value&quot;: 1273
        &#125;,
        &#123;
          &quot;key&quot;: &quot;1533406637136032&quot;,
          &quot;value&quot;: 1042
        &#125;
      ]
    &#125;,
    &#123;
      &quot;metric&quot;: &quot;TOP_BLOCK_REASON&quot;,
      &quot;type&quot;: &quot;STRING_VALUES&quot;,
      &quot;string_values&quot;: [
        &#123;
          &quot;key&quot;: &quot;5289179717853347&quot;,
          &quot;value&quot;: &quot;UNKNOWN_BLOCK_REASON&quot;
        &#125;,
        &#123;
          &quot;key&quot;: &quot;1533406637136032&quot;,
          &quot;value&quot;: &quot;UNKNOWN_BLOCK_REASON&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```
