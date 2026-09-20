# Analytics


**Warning:** Starting December 1, 2025, the maximum lookback window for messaging, conversation, and pricing analytics is changing from 10 years to 1 year. The lookback window for template and template group analytics will be unaffected and will continue to be 90 days.

This document describes how to get messaging, conversation, template, and group analytics, such as the number of messages sent from a business phone number, the number of conversations and their costs for a WhatsApp Business account (WABA), or the number of times a given template has been read.

Only metrics for business phone numbers and templates associated with your WABA at the time of the request will be included in responses.

## Get data

Use the [WhatsApp Business account API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/whatsapp-business-account-api#get-version-waba-id) to get analytics.

### Request syntax

```html
curl -g &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;?fields=&lt;FIELD&gt;.&lt;FILTERS&gt;&#039; \
-H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039;
```

### Request parameters

| Placeholder | Description | Example Value |
| --- | --- | --- |
| `&lt;FIELD&gt;` | **Required.**&lt;br&gt;&lt;br&gt;Metric. Value can be one of:&lt;br&gt;&lt;br&gt;* [`analytics`](#messaging-analytics)&lt;br&gt;* [`conversation_analytics`](#conversation-analytics)&lt;br&gt;* [`pricing_analytics`](#pricing-analytics)&lt;br&gt;* [`template_analytics`](#template-analytics)&lt;br&gt;* [`template_group_analytics`](#template-group-analytics)&lt;br&gt;* [`call_analytics`](#call-analytics)&lt;br&gt;* [`group_analytics`](#group-analytics) | `analytics` |
| `&lt;FILTERS&gt;` | **Required.**&lt;br&gt;&lt;br&gt;Metric filtering parameter. Append additional filtering parameters using dots.&lt;br&gt;&lt;br&gt;For possible values, see:&lt;br&gt;&lt;br&gt;* [Messaging analytics parameters](#messaging-analytics-parameters)&lt;br&gt;* [Conversation analytics parameters](#conversation-analytics-parameters)&lt;br&gt;* [Template analytics parameters](#template-analytics-parameters)&lt;br&gt;* [Template group analytics parameters](#template-group-analytics-parameters)&lt;br&gt;* [Call analytics parameters](#call-analytics-parameters)&lt;br&gt;* [Group analytics parameters](#group-analytics-parameters) | `.start(1543543200).end(1544148000).granularity(DAY)` |

## Messaging analytics

The `analytics` field provides the number and type of messages sent and delivered by the phone numbers associated with a specific WABA — for conversation metrics, see [Conversation Analytics](#conversation-analytics). When requesting the `analytics` field, you can attach the following filtering parameters.

### Messaging analytics parameters

| Name | Description |
| --- | --- |
| `start`&lt;br&gt;&lt;br&gt;type: UNIX Timestamp | **Required.**&lt;br&gt;&lt;br&gt;The start date for the date range for which you are retrieving analytics. |
| `end`&lt;br&gt;&lt;br&gt;type: UNIX Timestamp | **Required.**&lt;br&gt;&lt;br&gt;The end date for the date range for which you are retrieving analytics. |
| `granularity`&lt;br&gt;&lt;br&gt;type: String | **Required.**&lt;br&gt;&lt;br&gt;The granularity at which you would like to retrieve the analytics. Supported Options:&lt;br&gt;&lt;br&gt;- `HALF_HOUR`&lt;br&gt;- `DAY`&lt;br&gt;- `MONTH` |
| `phone_numbers`&lt;br&gt;&lt;br&gt;type: Array | **Optional.**&lt;br&gt;&lt;br&gt;An array of phone numbers for which you would like to retrieve analytics. If not provided, all phone numbers added to your WABA are included. |
| `product_types`&lt;br&gt;&lt;br&gt;type: Array | **Optional.**&lt;br&gt;&lt;br&gt;The types of messages (notification messages and/or customer support messages) for which you want to retrieve notifications. If not provided, analytics will be returned for all messages together.&lt;br&gt;&lt;br&gt;Supported values:&lt;br&gt;&lt;br&gt;- `0` — for template messages sent to WhatsApp users&lt;br&gt;- `2` — for non-templates messages sent to WhatsApp users&lt;br&gt;- `100` — for incoming messages sent from WhatsApp users to you |
| `country_codes`&lt;br&gt;&lt;br&gt;type: Array | **Optional.**&lt;br&gt;&lt;br&gt;The countries for which you would like to retrieve analytics. Provide an array with 2-letter country codes for the countries you would like to include. If not provided, analytics will be returned for all countries you have communicated with. |

### Example

**Scenario:** You need to get the number of messages sent and delivered by all phone numbers associated with your WABA.

**Suggested Solution:** Use following filtering parameters: `start`, `end`, `granularity`.

```curl
curl -i -X GET &quot;https://graph.facebook.com/v25.0/102290129340398
  ?fields=analytics
  .start(1543543200)
  .end(1544148000)
  .granularity(DAY)
  &amp;access_token=BLI8lkj...&quot;
```

A successful response returns an `analytics` object with the data you have requested:

```json
&#123;
  &quot;analytics&quot;: &#123;
    &quot;phone_numbers&quot;: [
      &quot;16505550111&quot;,
      &quot;16505550112&quot;,
      &quot;16505550113&quot;
    ],
    &quot;country_codes&quot;: [
      &quot;US&quot;,
      &quot;BR&quot;
    ],
    &quot;granularity&quot;: &quot;DAY&quot;,
    &quot;data_points&quot;: [
      &#123;
        &quot;start&quot;: 1543543200,
        &quot;end&quot;: 1543629600,
        &quot;sent&quot;: 196093,
        &quot;delivered&quot;: 179715
      &#125;,
      &#123;
        &quot;start&quot;: 1543629600,
        &quot;end&quot;: 1543716000,
        &quot;sent&quot;: 147649,
        &quot;delivered&quot;: 139032
      &#125;,
      &#123;
        &quot;start&quot;: 1543716000,
        &quot;end&quot;: 1543802400,
        &quot;sent&quot;: 61988,
        &quot;delivered&quot;: 58830
      &#125;,
      &#123;
        &quot;start&quot;: 1543802400,
        &quot;end&quot;: 1543888800,
        &quot;sent&quot;: 132465,
        &quot;delivered&quot;: 124392
      &#125;
      # more data points
    ]
  &#125;,
  &quot;id&quot;: &quot;102290129340398&quot;
&#125;
```

## Conversation analytics

The `conversation_analytics` field provides cost and [conversation](https://developers.facebook.com/documentation/business-messaging/whatsapp/pricing/conversation-based-pricing) information for a specific WABA. When requesting the `conversation_analytics` field, you can attach the following filtering parameters.

### Conversation analytics parameters

| Name | Description *(Click the arrow in the left column for supported options.)* |
| --- | --- |
| `start`&lt;br&gt;&lt;br&gt;type: UNIX Timestamp | **Required.**&lt;br&gt;&lt;br&gt;The start date for the date range for which you are retrieving analytics. |
| `end`&lt;br&gt;&lt;br&gt;type: UNIX Timestamp | **Required.**&lt;br&gt;&lt;br&gt;The end date for the date range for which you are retrieving analytics. |
| `granularity`&lt;br&gt;&lt;br&gt;type: String | **Required.**&lt;br&gt;&lt;br&gt;The granularity at which you would like to retrieve the analytics. Supported Options:&lt;br&gt;&lt;br&gt;- `HALF_HOUR`&lt;br&gt;- `DAILY`&lt;br&gt;- `MONTHLY` |
| `phone_numbers`&lt;br&gt;&lt;br&gt;type: Array | **Optional.**&lt;br&gt;&lt;br&gt;An array of phone numbers for which you would like to retrieve analytics. If not provided, all phone numbers added to your WABA are included. |
| `metric_types` | **Optional.**&lt;br&gt;&lt;br&gt;List of metrics you would like to receive. If you send an empty list, the API returns results for all metric types.&lt;br&gt;&lt;br&gt;Supported Options: &#123;#supported&#125;&lt;br&gt;&lt;br&gt;- `COST`: Includes approximate charges for that time range, in the WABA&#039;s currency.&lt;br&gt;- `CONVERSATION`: Includes the count of conversations for that time range.&lt;br&gt;&lt;br&gt;**Exception:**&lt;br&gt;&lt;br&gt;**`COST` will not be returned for WABAs that share a Solution Partner&#039;s credit line. If your WABA shares a Solution Partner&#039;s credit line, reach out to your Solution Partner to understand your charges.** If you a querying a WABA that shares a Solution Partner&#039;s credit line:&lt;br&gt;&lt;br&gt;1. If no `metric_types` are specified in your request, only `CONVERSATION` is returned.&lt;br&gt;1. If only `CONVERSATION` is specified, only `CONVERSATION` is returned.&lt;br&gt;1. If only `COST` is specified, the following exception is returned:&lt;br&gt;- Title: &quot;Cost not available&quot;&lt;br&gt;- Message: &quot;Cost is no longer shown for businesses who bill through a partner (i.e., Solution Partner). To understand your charges, please reach out to your partner.&quot;&lt;br&gt;&lt;br&gt;If you query a time period that includes dates on or after July 1, 2023, (for example, May 1, 2023 through August 1, 2023), the response will include the above exception.&lt;br&gt;&lt;br&gt;This does not apply if querying the `conversation_analytics` endpoint. |
| `conversation_categories` | **Optional.**&lt;br&gt;&lt;br&gt;List of [conversation categories](https://developers.facebook.com/documentation/business-messaging/whatsapp/pricing#conversation-categories). If you send an empty list, the API returns results for all conversation categories.&lt;br&gt;&lt;br&gt;Supported Options:&lt;br&gt;&lt;br&gt;- `AUTHENTICATION`&lt;br&gt;- `MARKETING`&lt;br&gt;- `SERVICE`&lt;br&gt;- `UTILITY` |
| `conversation_types` | **Optional.**&lt;br&gt;&lt;br&gt;List of conversation types. If you send an empty list, the API returns results for all conversation types. Supported Options:&lt;br&gt;&lt;br&gt;- `FREE_ENTRY_POINT`: Conversations originating from a [free entry point](https://developers.facebook.com/documentation/business-messaging/whatsapp/pricing#free-entry-point-windows).&lt;br&gt;- `FREE_TIER`: Conversations within the monthly [free tier](https://developers.facebook.com/documentation/business-messaging/whatsapp/pricing#free-entry-point-windows).&lt;br&gt;- `REGULAR`: Any conversations that did not originate from a [free entry point](https://developers.facebook.com/documentation/business-messaging/whatsapp/pricing#free-entry-point-windows) or are above the monthly free tier allotment. |
| `conversation_directions` | **Optional.**&lt;br&gt;&lt;br&gt;List of conversation directions. If you send an empty list, the API returns results for all conversation directions. Supported Options:&lt;br&gt;&lt;br&gt;- `BUSINESS_INITIATED`: Conversations initiated by the business.&lt;br&gt;- `USER_INITIATED`: Conversations initiated by an end user/customer.&lt;br&gt;- `UNKNOWN`: System cannot determine direction. |
| `dimensions` | **Optional.**&lt;br&gt;&lt;br&gt;List of breakdowns you would like to apply to your metrics. If you send an empty list, the API returns results without any breakdowns. Supported Options:&lt;br&gt;&lt;br&gt;- `CONVERSATION_CATEGORY`&lt;br&gt;- `CONVERSATION_DIRECTION`&lt;br&gt;- `CONVERSATION_TYPE`&lt;br&gt;- `COUNTRY`&lt;br&gt;- `PHONE` |

**Note:** Analytics data is approximate and may differ from what&#039;s shown on invoices due to small variations in data processing.

### Examples

Given a time range, you can get conversation and cost information associated with your WABA. If you want, you can filter and break down your results. See the code samples below for examples.

#### Get monthly data, using all breakdowns

**Scenario:**  Given a month, you want to retrieve all conversation and cost information for all phone numbers associated with a WABA.

**Suggested Solution:** Use the following filtering parameters:

- `start`: Start of your time range. In this case, the beginning of the month you want metrics for.
- `end`: End of your time range. In this case, the end of the month you want metrics for.
- `granularity`: How granular you want your data points to be. In the example below, use `MONTHLY`, so each datapoint will represent a month&#039;s worth of data.
- `phone_numbers`: Send an empty array to get information for all phone numbers associated with the WABA.
- `dimensions`: Set it to all available breakdowns: `&quot;CONVERSATION_CATEGORY&quot;`, `&quot;CONVERSATION_TYPE&quot;`, `&quot;COUNTRY&quot;`, and `&quot;PHONE&quot;`.

In this case, you do not need to specify `country_codes`, `metric_types`, `conversation_types`, and `conversation_categories`. If you don&#039;t send anything for those fields, the API returns all available options. Once you set up the URL, make a GET request:

```curl
curl -i -X GET
&quot;https://graph.facebook.com/v25.0/102290129340398
  ?fields=conversation_analytics
  .start(1685602800).end(1688194800)
  .granularity(MONTHLY)
  .phone_numbers([])
  .dimensions([&quot;CONVERSATION_CATEGORY&quot;,&quot;CONVERSATION_TYPE&quot;,&quot;COUNTRY&quot;,&quot;PHONE&quot;])
  &amp;access_token=BLI8lkj...&quot;
```

A successful response returns a `conversation_analytics` object with the data you have requested. In the following example, the WABA contains only one phone number.

```json
&#123;
  &quot;conversation_analytics&quot;: &#123;
    &quot;data&quot;: [
      &#123;
        &quot;data_points&quot;: [
          &#123;
            &quot;start&quot;: 1685602800,
            &quot;end&quot;: 1688194800,
            &quot;conversation&quot;: 1558,
            &quot;phone_number&quot;: &quot;15550458206&quot;,
            &quot;country&quot;: &quot;US&quot;,
            &quot;conversation_type&quot;: &quot;REGULAR&quot;,
            &quot;conversation_direction&quot;: &quot;UNKNOWN&quot;,
            &quot;conversation_category&quot;: &quot;AUTHENTICATION&quot;,
            &quot;cost&quot;: 15.58
          &#125;,
          &#123;
            &quot;start&quot;: 1685602800,
            &quot;end&quot;: 1688194800,
            &quot;conversation&quot;: 2636,
            &quot;phone_number&quot;: &quot;15550458206&quot;,
            &quot;country&quot;: &quot;US&quot;,
            &quot;conversation_type&quot;: &quot;REGULAR&quot;,
            &quot;conversation_category&quot;: &quot;MARKETING&quot;,
            &quot;cost&quot;: 26.36
          &#125;,
          &#123;
            &quot;start&quot;: 1685602800,
            &quot;end&quot;: 1688194800,
            &quot;conversation&quot;: 2238,
            &quot;phone_number&quot;: &quot;15550458206&quot;,
            &quot;country&quot;: &quot;US&quot;,
            &quot;conversation_type&quot;: &quot;REGULAR&quot;,
            &quot;conversation_category&quot;: &quot;SERVICE&quot;,
            &quot;cost&quot;: 22.38
          &#125;,
          &#123;
            &quot;start&quot;: 1685602800,
            &quot;end&quot;: 1688194800,
            &quot;conversation&quot;: 1782,
            &quot;phone_number&quot;: &quot;15550458206&quot;,
            &quot;country&quot;: &quot;US&quot;,
            &quot;conversation_type&quot;: &quot;REGULAR&quot;,
            &quot;conversation_category&quot;: &quot;UTILITY&quot;,
            &quot;cost&quot;: 17.82
          &#125;,
          &#123;
            &quot;start&quot;: 1685602800,
            &quot;end&quot;: 1688194800,
            &quot;conversation&quot;: 1568,
            &quot;phone_number&quot;: &quot;15550458206&quot;,
            &quot;country&quot;: &quot;US&quot;,
            &quot;conversation_type&quot;: &quot;FREE_TIER&quot;,
            &quot;conversation_category&quot;: &quot;AUTHENTICATION&quot;,
            &quot;cost&quot;: 15.68
          &#125;,
          &#123;
            &quot;start&quot;: 1685602800,
            &quot;end&quot;: 1688194800,
            &quot;conversation&quot;: 2716,
            &quot;phone_number&quot;: &quot;15550458206&quot;,
            &quot;country&quot;: &quot;US&quot;,
            &quot;conversation_type&quot;: &quot;FREE_TIER&quot;,
            &quot;conversation_category&quot;: &quot;MARKETING&quot;,
            &quot;cost&quot;: 27.16
          &#125;,
          &#123;
            &quot;start&quot;: 1685602800,
            &quot;end&quot;: 1688194800,
            &quot;conversation&quot;: 2180,
            &quot;phone_number&quot;: &quot;15550458206&quot;,
            &quot;country&quot;: &quot;US&quot;,
            &quot;conversation_type&quot;: &quot;FREE_TIER&quot;,
            &quot;conversation_category&quot;: &quot;SERVICE&quot;,
            &quot;cost&quot;: 21.8
          &#125;,
          &#123;
            &quot;start&quot;: 1685602800,
            &quot;end&quot;: 1688194800,
            &quot;conversation&quot;: 1465,
            &quot;phone_number&quot;: &quot;15550458206&quot;,
            &quot;country&quot;: &quot;US&quot;,
            &quot;conversation_type&quot;: &quot;FREE_TIER&quot;,
            &quot;conversation_category&quot;: &quot;UTILITY&quot;,
            &quot;cost&quot;: 14.65
          &#125;,
          &#123;
            &quot;start&quot;: 1685602800,
            &quot;end&quot;: 1688194800,
            &quot;conversation&quot;: 1433,
            &quot;phone_number&quot;: &quot;15550458206&quot;,
            &quot;country&quot;: &quot;US&quot;,
            &quot;conversation_type&quot;: &quot;FREE_ENTRY_POINT&quot;,
            &quot;conversation_category&quot;: &quot;SERVICE&quot;,
            &quot;cost&quot;: 14.33
          &#125;
        ]
      &#125;
    ]
  &#125;,
  &quot;id&quot;: &quot;102290129340398&quot;
&#125;
```

#### Get data for a specific phone number, using all breakdowns and half hour granularity

**Scenario**:  Given a time range, you want to retrieve all conversation and cost information for a specific phone number associated with a WABA. In the results, you want to use all possible breakdowns. You need each data point to represent half an hour&#039;s worth of data.

**Suggested Solution**: Use the following filtering parameters:

- `start`: Start of your time range.
- `end`: End of your time range.
- `granularity`: How granular you want your data points to be. In the example below, use `HALF_HOUR`, so each datapoint represents half an hour&#039;s worth of data.
- `phone_numbers`: The phone number you need information for.
- `dimensions`: Set it to all available breakdowns: `CONVERSATION_CATEGORY`, `CONVERSATION_TYPE`, `COUNTRY`, and `PHONE`.

In this case, you do not need to specify `country_codes`, `metric_types`, `conversation_types`, or `conversation_categories`. If you don&#039;t send anything for those fields, the API returns all available options. Once you set up the URL, make a GET request:

```curl
curl -i -X GET \
&quot;https://graph.facebook.com/v25.0/102290129340398
  ?fields=conversation_analytics
  .start(1685602800)
  .end(1685689200)
  .granularity(HALF_HOUR)
  .phone_numbers([&quot;19195552584&quot;])
  .dimensions([&quot;CONVERSATION_CATEGORY&quot;,&quot;CONVERSATION_TYPE&quot;,&quot;COUNTRY&quot;,&quot;PHONE&quot;])
  &amp;access_token=BLI8lkj...&quot;
```

A successful response returns a `conversation_analytics` object with the data you have requested:

```json
&#123;
  &quot;conversation_analytics&quot;: &#123;
    &quot;data&quot;: [
      &#123;
        &quot;data_points&quot;: [
          &#123;
            &quot;start&quot;: 1685602800,
            &quot;end&quot;: 1685604600,
            &quot;conversation&quot;: 4,
            &quot;phone_number&quot;: &quot;19195552584&quot;,
            &quot;country&quot;: &quot;US&quot;,
            &quot;conversation_type&quot;: &quot;REGULAR&quot;,
            &quot;conversation_direction&quot;: &quot;UNKNOWN&quot;,
            &quot;conversation_category&quot;: &quot;SERVICE&quot;,
            &quot;cost&quot;: 0.0232
          &#125;,
          &#123;
            &quot;start&quot;: 1685602800,
            &quot;end&quot;: 1685604600,
            &quot;conversation&quot;: 4,
            &quot;phone_number&quot;: &quot;19195552584&quot;,
            &quot;country&quot;: &quot;US&quot;,
            &quot;conversation_type&quot;: &quot;REGULAR&quot;,
            &quot;conversation_direction&quot;: &quot;UNKNOWN&quot;,
            &quot;conversation_category&quot;: &quot;MARKETING&quot;,
            &quot;cost&quot;: 0.0232
          &#125;,
         # ... more data points
        ]
      &#125;
    ]
  &#125;,
  &quot;id&quot;: &quot;102290129340398&quot;
&#125;
```

#### Get monthly data, using conversation type breakdowns

**Scenario**:  Given a time range, you want to retrieve all conversation and cost information for all phone numbers associated with a WABA. In the results, you want to break down by conversation type.

**Suggested Solution**: Use the following filtering parameters:

- `start`: Start of your time range.
- `end`: End of your time range.
- `granularity`: How granular you want your data points to be. In the example below, use `MONTHLY`, so each datapoint represents half a month&#039;s worth of data.
- `phone_numbers`: Send an empty array to get information for all phone numbers associated with the WABA.
- `dimensions`: Set it to `CONVERSATION_TYPE`.

In this case, you do not need to specify `country_codes`, `metric_types`, `conversation_types`, `conversation_directions`, or `conversation_categories`. If you don&#039;t send anything for those fields, the API returns all available options. Once you set up the URL, make a GET request:

```curl
curl -i -X GET &quot;https://graph.facebook.com/v25.0/102290129340398
      ?fields=conversation_analytics
      .start(1643702400).end(1646121600)
      .granularity(MONTHLY)
      .phone_numbers([])
      .dimensions([CONVERSATION_TYPE])
      &amp;access_token=BLI8lkj...&quot;
```

A successful response returns a `conversation_analytics` object with the data you have requested:

```json
&#123;
  &quot;data&quot;: [
    &#123;
      &quot;data_points&quot;: [
        &#123;
          &quot;start&quot;: 1643702400,
          &quot;end&quot;: 1646121600,
          &quot;conversation&quot;: 8500,
          &quot;conversation_type&quot;: &quot;REGULAR&quot;,
          &quot;cost&quot;: 88.1010
        &#125;,
        &#123;
          &quot;start&quot;: 1643702400,
          &quot;end&quot;: 1646121600,
          &quot;conversation&quot;: 1000,
          &quot;conversation_type&quot;: &quot;FREE_TIER&quot;,
          &quot;cost&quot;: 0.0000
        &#125;,
        &#123;
          &quot;start&quot;: 1643702400,
          &quot;end&quot;: 1646121600,
          &quot;conversation&quot;: 250,
          &quot;conversation_type&quot;: &quot;FREE_ENTRY_POINT&quot;,
          &quot;cost&quot;: 0.0000
        &#125;
      ]
    &#125;
  ]
&#125;
```

#### Get half-hour data broken down by conversation category

Request:

```curl
curl -i -X GET &quot;https://graph.facebook.com/v25.0/102290129340398
  ?fields=conversation_analytics
  .start(1685527200)
  .end(1685613600)
  .granularity(HALF_HOUR)
  .conversation_categories([&quot;MARKETING&quot;,&quot;AUTHENTICATION&quot;])
  .dimensions([&quot;CONVERSATION_CATEGORY&quot;])
  &amp;access_token=BLI8lkj...&quot;
```

Response:

```json
&#123;
  &quot;conversation_analytics&quot;: &#123;
    &quot;data&quot;: [
      &#123;
        &quot;data_points&quot;: [
          &#123;
            &quot;start&quot;: 1685529000,
            &quot;end&quot;: 1685530800,
            &quot;conversation&quot;: 2,
            &quot;conversation_category&quot;: &quot;AUTHENTICATION&quot;,
            &quot;cost&quot;: 0.0128
          &#125;,
          &#123;
            &quot;start&quot;: 1685527200,
            &quot;end&quot;: 1685529000,
            &quot;conversation&quot;: 3,
            &quot;conversation_category&quot;: &quot;MARKETING&quot;,
            &quot;cost&quot;: 0.0432
          &#125;
        ]
      &#125;
    ]
  &#125;,
  &quot;id&quot;: &quot;102290129340398&quot;
&#125;
```

#### Get half-hour data broken down by conversation category and conversation type

Request:

```curl
curl -i -X GET \
 &quot;https://graph.facebook.com/v25.0/102290129340398
  ?fields=conversation_analytics
  .start(1685527200)
  .end(1685613600)
  .granularity(HALF_HOUR)
  .conversation_categories([&quot;MARKETING&quot;,&quot;AUTHENTICATION&quot;])
  .dimensions([&quot;CONVERSATION_CATEGORY&quot;,&quot;CONVERSATION_TYPE&quot;])
  &amp;access_token=BLI8lkj...&quot;
```

Response:

```json
&#123;
  &quot;conversation_analytics&quot;: &#123;
    &quot;data&quot;: [
      &#123;
        &quot;data_points&quot;: [
          &#123;
            &quot;start&quot;: 1685527200,
            &quot;end&quot;: 1685529000,
            &quot;conversation&quot;: 3,
            &quot;conversation_type&quot;: &quot;REGULAR&quot;,
            &quot;conversation_category&quot;: &quot;MARKETING&quot;,
            &quot;cost&quot;: 0.0432
          &#125;,
          &#123;
            &quot;start&quot;: 1685529000,
            &quot;end&quot;: 1685530800,
            &quot;conversation&quot;: 2,
            &quot;conversation_type&quot;: &quot;REGULAR&quot;,
            &quot;conversation_category&quot;: &quot;AUTHENTICATION&quot;,
            &quot;cost&quot;: 0.0128
          &#125;
        ]
      &#125;
    ]
  &#125;,
  &quot;id&quot;: &quot;102290129340398&quot;
&#125;
```

## Pricing analytics

The `pricing_analytics` field allows you to get pricing breakdowns for any messages delivered within a specified date range.

### Request syntax

```html
GET /&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;
  ?fields=pricing_analytics
  .start(&lt;START&gt;)
  .end(&lt;END&gt;)
  .granularity(&lt;GRANULARITY&gt;)
  .phone_numbers(&lt;PHONE_NUMBERS&gt;)
  .country_codes(&lt;COUNTRY_CODES&gt;)
  .metric_types(&lt;METRIC_TYPES&gt;)
  .pricing_types(&lt;PRICING_TYPES&gt;)
  .pricing_categories(&lt;PRICING_CATEGORIES&gt;)
  .dimensions(&lt;DIMENSIONS&gt;)
```

### Pricing analytics parameters

| Filter | Description | Example Value |
| --- | --- | --- |
| `&lt;COUNTRY_CODES&gt;`&lt;br&gt;&lt;br&gt;_Array of strings_ | **Optional.**&lt;br&gt;&lt;br&gt;The countries for which you would like to retrieve analytics. Provide an array with 2-letter country codes for the countries you would like to include. If not provided, analytics will be returned for all countries you have communicated with. | `[&lt;br&gt;US,&lt;br&gt;BR&lt;br&gt;]` |
| `&lt;DIMENSIONS&gt;`&lt;br&gt;&lt;br&gt;_Array of strings_ | **Optional.**&lt;br&gt;&lt;br&gt;List of breakdowns you would like to apply to your metrics. If you send an empty list, the API returns results without any breakdowns.&lt;br&gt;&lt;br&gt;Values can be:&lt;br&gt;&lt;br&gt;- `COUNTRY`&lt;br&gt;- `PHONE`&lt;br&gt;- `PRICING_CATEGORY`&lt;br&gt;- `PRICING_TYPE`&lt;br&gt;- `TIER` | `[&lt;br&gt;PRICING_CATEGORY,&lt;br&gt;PRICING_TYPE,&lt;br&gt;COUNTRY&lt;br&gt;]` |
| `&lt;END&gt;`&lt;br&gt;&lt;br&gt;_UNIX timestamp_ | **Required.**&lt;br&gt;&lt;br&gt;UNIX timestamp indicating the end date for the date range you are retrieving analytics for. | `1728581152` |
| `&lt;GRANULARITY&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;The granularity at which you would like to retrieve the analytics. Value can be one of:&lt;br&gt;&lt;br&gt;- `DAILY`&lt;br&gt;- `HALF_HOUR`&lt;br&gt;- `MONTHLY` | `DAILY` |
| `&lt;METRIC_TYPES&gt;`&lt;br&gt;&lt;br&gt;_Array of strings_ | **Optional.**&lt;br&gt;&lt;br&gt;Array of metrics you would like to receive. If you send an empty array, the API returns results for all metric types.&lt;br&gt;&lt;br&gt;Values can be:&lt;br&gt;&lt;br&gt;- `COST`: Approximate charges for messages delivered in that time range, in your WABA&#039;s currency.&lt;br&gt;- `VOLUME`: Includes the number of messages delivered for that time range.&lt;br&gt;&lt;br&gt;**Note that `COST` will not be returned for WABAs that share a Solution Partner&#039;s credit line. If your WABA shares a Solution Partner&#039;s credit line, reach out to your Solution Partner to understand your charges.** | `[COST, VOLUME]` |
| `&lt;PHONE_NUMBERS&gt;`&lt;br&gt;&lt;br&gt;_Array of strings_ | **Optional.**&lt;br&gt;&lt;br&gt;An array of phone numbers for which you would like to retrieve analytics. If not provided, data for all business phone numbers associated with your WABA are included. | `[&lt;br&gt;15550783881,&lt;br&gt;15550783882,&lt;br&gt;15550783883&lt;br&gt;]` |
| `&lt;PRICING_CATEGORIES&gt;`&lt;br&gt;&lt;br&gt;_Array of strings_ | **Optional.**&lt;br&gt;&lt;br&gt;Array of pricing categories. If you send an empty array, the API returns results for all pricing categories.&lt;br&gt;&lt;br&gt;Values can be:&lt;br&gt;&lt;br&gt;- `AUTHENTICATION`: Messages charged the authentication rate.&lt;br&gt;- `AUTHENTICATION_INTERNATIONAL`: Messages charged the authentication-international rate.&lt;br&gt;- `MARKETING`: Messages charged the marketing rate.&lt;br&gt;- `MARKETING_LITE`: Messages charged the marketing-lite rate.&lt;br&gt;- `SERVICE`: Messages that were not charged. Includes all non-template messages and utility messages sent inside of a customer service window.&lt;br&gt;- `UTILITY`: Messages charged the utility rate.&lt;br&gt;- `REFERRAL_CONVERSION`: Messages that have been received through a [free entry point](https://developers.facebook.com/documentation/business-messaging/whatsapp/pricing#free-entry-point-windows) | `[&lt;br&gt;AUTHENTICATION,&lt;br&gt;MARKETING,&lt;br&gt;UTILITY&lt;br&gt;]` |
| `&lt;PRICING_TYPES&gt;`&lt;br&gt;&lt;br&gt;_Array of strings_ | **Optional.**&lt;br&gt;&lt;br&gt;Array of pricing types. If you send an empty array, the API returns results for all pricing types.&lt;br&gt;&lt;br&gt;Values can be:&lt;br&gt;&lt;br&gt;* `FREE_CUSTOMER_SERVICE`: Free messages. These are non-template messages and utility messages sent  within customer service windows.&lt;br&gt;* `FREE_ENTRY_POINT`: All messages sent within free entry point customer service windows.&lt;br&gt;* `REGULAR`: Billable messages. Includes all authentication and marketing template messages, and any utility template messages sent outside of a customer service window. Excludes all messages sent within free entry point customer service windows. | `[&lt;br&gt;REGULAR,&lt;br&gt;FREE_CUSTOMER_SERVICE&lt;br&gt;]` |
| `&lt;START&gt;`&lt;br&gt;&lt;br&gt;_UNIX timestamp_ | **Required.**&lt;br&gt;&lt;br&gt;UNIX timestamp indicating the start date for the date range you are retrieving analytics for. | `1726014453` |
| `&lt;WABA_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;WhatsApp Business account ID. | `102290129340398` |

### Volume tier information

Include the `TIER`, `PRICING_CATEGORY`, and `COUNTRY` parameters in the `dimensions` array to get volume tier information. Data points representing messages affected by volume tier pricing will have a `tier` property in the response.

#### Example response syntax with tier information

```html
&#123;
  &quot;start&quot;: &lt;START_TIMESTAMP&gt;,
  &quot;end&quot;: &lt;END_TIMESTAMP&gt;,
  &quot;phone_number&quot;: &quot;&lt;BUSINESS_PHONE_NUMBER&gt;&quot;,
  &quot;country&quot;: &quot;&lt;COUNTRY_CODE&gt;&quot;,
  &quot;tier&quot;: &quot;&lt;LOWER&gt;:&lt;UPPER&gt;&quot;,
  &quot;pricing_type&quot;: &quot;&lt;PRICING_TYPE&gt;&quot;,
  &quot;pricing_category&quot;: &quot;&lt;PRICING_CATEGORY&gt;&quot;,
  &quot;volume&quot;: &lt;VOLUME&gt;,
  &quot;cost&quot;: &lt;COST&gt;
&#125;
```

The `tier` property value represents a concatenation of the lower and upper bounds for the tier specific to the market–category pair (`country` and `pricing_category`) that that data point represents.

- `&lt;LOWER&gt;` – An integer representing the lower bound of the tier (inclusive).
- `&lt;UPPER&gt;` – An integer representing the upper bound of the tier (inclusive), or the string `MAX`.

**Notes**

- To determine your current volume tier, read the `tier`, `country`, and `pricing_category` values. The  `tier` value&#039;s `&lt;UPPER&gt;` integer (the integer after the colon) tells you your current tier for the `country` and `pricing_category` (for example, (India, and utility, respectively).
- To determine how many messages you need to send to reach the next tier for a given `country` and `pricing_category`, subtract the `volume` integer from the tier value&#039;s `&lt;UPPER&gt;` integer.
- Volume tiers will only be available for utility and authentication template messages. For marketing template messages (where volume tiers will not apply), tier will be set to `0:MAX`.
- The `tier` property will be omitted for data points that represent free messages, since free messages don&#039;t contribute to tiering counts.
- Volume tiers will be determined solely by Meta. All insights data is approximate due to small variations in data processing. Undue reliance should not be placed on insights data.

### Example request

```curl
curl &#039;https://graph.facebook.com/v25.0/161311403722088?fields=pricing_analytics.start(1748761200).end(1749687703).granularity(DAILY).dimensions(PRICING_CATEGORY,PRICING_TYPE,TIER,COUNTRY).country_codes(US,IN)&#039; \
-H &#039;Authorization: Bearer EAAJB&#039;
```

### Example response

```json
&#123;
  &quot;pricing_analytics&quot;: &#123;
    &quot;data&quot;: [
      &#123;
        &quot;data_points&quot;: [
          &#123;
            &quot;start&quot;: 1749193200,
            &quot;end&quot;: 1749279600,
            &quot;country&quot;: &quot;IN&quot;,
            &quot;pricing_type&quot;: &quot;FREE_CUSTOMER_SERVICE&quot;,
            &quot;pricing_category&quot;: &quot;SERVICE&quot;,
            &quot;volume&quot;: 2,
            &quot;cost&quot;: 0
          &#125;,
          &#123;
            &quot;start&quot;: 1749106800,
            &quot;end&quot;: 1749193200,
            &quot;country&quot;: &quot;IN&quot;,
            &quot;tier&quot;: &quot;0:750000&quot;,
            &quot;pricing_type&quot;: &quot;REGULAR&quot;,
            &quot;pricing_category&quot;: &quot;AUTHENTICATION_INTERNATIONAL&quot;,
            &quot;volume&quot;: 2,
            &quot;cost&quot;: 4.6
          &#125;,
          &#123;
            &quot;start&quot;: 1749106800,
            &quot;end&quot;: 1749193200,
            &quot;country&quot;: &quot;IN&quot;,
            &quot;pricing_type&quot;: &quot;FREE_CUSTOMER_SERVICE&quot;,
            &quot;pricing_category&quot;: &quot;SERVICE&quot;,
            &quot;volume&quot;: 2,
            &quot;cost&quot;: 0
          &#125;,
          &#123;
            &quot;start&quot;: 1748934000,
            &quot;end&quot;: 1749020400,
            &quot;country&quot;: &quot;US&quot;,
            &quot;tier&quot;: &quot;0:MAX&quot;,
            &quot;pricing_type&quot;: &quot;REGULAR&quot;,
            &quot;pricing_category&quot;: &quot;MARKETING&quot;,
            &quot;volume&quot;: 1,
            &quot;cost&quot;: 10
          &#125;,
          &#123;
            &quot;start&quot;: 1748847600,
            &quot;end&quot;: 1748934000,
            &quot;country&quot;: &quot;US&quot;,
            &quot;pricing_type&quot;: &quot;FREE_CUSTOMER_SERVICE&quot;,
            &quot;pricing_category&quot;: &quot;SERVICE&quot;,
            &quot;volume&quot;: 1,
            &quot;cost&quot;: 0
          &#125;,
          &#123;
            &quot;start&quot;: 1748847600,
            &quot;end&quot;: 1748934000,
            &quot;country&quot;: &quot;US&quot;,
            &quot;pricing_type&quot;: &quot;FREE_ENTRY_POINT&quot;,
            &quot;pricing_category&quot;: &quot;SERVICE&quot;,
            &quot;volume&quot;: 6,
            &quot;cost&quot;: 0
          &#125;,
          &#123;
            &quot;start&quot;: 1748847600,
            &quot;end&quot;: 1748934000,
            &quot;country&quot;: &quot;US&quot;,
            &quot;tier&quot;: &quot;0:2&quot;,
            &quot;pricing_type&quot;: &quot;REGULAR&quot;,
            &quot;pricing_category&quot;: &quot;AUTHENTICATION&quot;,
            &quot;volume&quot;: 1,
            &quot;cost&quot;: 10
          &#125;,
          &#123;
            &quot;start&quot;: 1748847600,
            &quot;end&quot;: 1748934000,
            &quot;country&quot;: &quot;IN&quot;,
            &quot;tier&quot;: &quot;0:750000&quot;,
            &quot;pricing_type&quot;: &quot;REGULAR&quot;,
            &quot;pricing_category&quot;: &quot;AUTHENTICATION_INTERNATIONAL&quot;,
            &quot;volume&quot;: 1,
            &quot;cost&quot;: 2.3
          &#125;,
          &#123;
            &quot;start&quot;: 1748761200,
            &quot;end&quot;: 1748847600,
            &quot;country&quot;: &quot;US&quot;,
            &quot;pricing_type&quot;: &quot;FREE_CUSTOMER_SERVICE&quot;,
            &quot;pricing_category&quot;: &quot;SERVICE&quot;,
            &quot;volume&quot;: 2,
            &quot;cost&quot;: 0
          &#125;,
          &#123;
            &quot;start&quot;: 1748761200,
            &quot;end&quot;: 1748847600,
            &quot;country&quot;: &quot;US&quot;,
            &quot;tier&quot;: &quot;0:2&quot;,
            &quot;pricing_type&quot;: &quot;REGULAR&quot;,
            &quot;pricing_category&quot;: &quot;AUTHENTICATION&quot;,
            &quot;volume&quot;: 1,
            &quot;cost&quot;: 10
          &#125;,
          &#123;
            &quot;start&quot;: 1748761200,
            &quot;end&quot;: 1748847600,
            &quot;country&quot;: &quot;US&quot;,
            &quot;pricing_type&quot;: &quot;FREE_CUSTOMER_SERVICE&quot;,
            &quot;pricing_category&quot;: &quot;UTILITY&quot;,
            &quot;volume&quot;: 1,
            &quot;cost&quot;: 0
          &#125;,
          &#123;
            &quot;start&quot;: 1748761200,
            &quot;end&quot;: 1748847600,
            &quot;country&quot;: &quot;US&quot;,
            &quot;tier&quot;: &quot;0:2&quot;,
            &quot;pricing_type&quot;: &quot;REGULAR&quot;,
            &quot;pricing_category&quot;: &quot;UTILITY&quot;,
            &quot;volume&quot;: 1,
            &quot;cost&quot;: 10
          &#125;,
          &#123;
            &quot;start&quot;: 1748761200,
            &quot;end&quot;: 1748847600,
            &quot;country&quot;: &quot;US&quot;,
            &quot;tier&quot;: &quot;0:MAX&quot;,
            &quot;pricing_type&quot;: &quot;REGULAR&quot;,
            &quot;pricing_category&quot;: &quot;MARKETING&quot;,
            &quot;volume&quot;: 4,
            &quot;cost&quot;: 40
          &#125;,
          &#123;
            &quot;start&quot;: 1748761200,
            &quot;end&quot;: 1748847600,
            &quot;country&quot;: &quot;US&quot;,
            &quot;tier&quot;: &quot;0:MAX&quot;,
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

## Template analytics

Template analytics describe the number of times a template has been sent, delivered, and read, and the number of times [URL buttons](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/components#url-buttons) or [Quick Reply buttons](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/components#quick-reply-buttons) in the template have been clicked. Additionally, onboarded [MM API for WhatsApp](https://developers.facebook.com/documentation/business-messaging/whatsapp/marketing-messages/overview) businesses can track offsite conversion metrics.

Data is returned with a daily granularity in the default timezone of UTC and WABA&#039;s timezone, with a lookback window of up to 90 days. To show data in the WABA&#039;s configured timezone, pass in the use_waba_timezone param with a value of true.

Display data in the WABA&#039;s configured timezone by passing in the `use_waba_timezone` param with a value of `true`.

```json
&#123;
 &quot;data&quot;: [
   &#123;
     &quot;waba_timezone&quot;: &quot;America/Los_Angeles&quot;,
     &quot;granularity&quot;: &quot;DAILY&quot;,
     &quot;product_type&quot;: &quot;cloud_api&quot;,
     &quot;data_points&quot;: [
         ...
     ]
   &#125;
&#125;
```

### Limitations

* Button click analytics are only available for templates categorized as `MARKETING` or `UTILITY`.
* WABAs owned by or shared with Meta Business Accounts in the European Union or Japan, or that have a business phone number with a country calling code from any of those countries or regions, are not supported.
* Offsite conversion metrics are available exclusively for businesses onboarded to MM API for WhatsApp.
* Read and click event data for WhatsApp template messages is only available for up to 7 days from the date the message is sent. After this 7-day window, the corresponding read/click counts reset to zero and no further updates are recorded for those messages.

### Confirming template analytics

You must confirm template analytics on your WhatsApp Business account before you can get template analytics. You can confirm template analytics using the WhatsApp Manager or the API.

**Note:** By confirming access via the API, you direct Meta to add insights to your WhatsApp Business account. These insights include link tracking to report website clicks. You can turn off link tracking on each message template. You also direct Meta to collect and anonymize data from your chats with customers. Meta will anonymize this data to improve services it provides you and other businesses.

To confirm via API, send the following request:

```html
POST /&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;?is_enabled_for_insights=true
```

Once confirmed, the API begins capturing template analytics for the WhatsApp Business account. Once confirmed, template analytics cannot be disabled.

Upon success, the API will respond with your WhatsApp Business account ID. For example:

```json
&#123;
  &quot;id&quot;: 102290129340398
&#125;
```

### Template analytics parameters

| Name | Description | Example Value |
| --- | --- | --- |
| `start`&lt;br&gt;&lt;br&gt;_UNIX Timestamp or date string_ | **Required.**&lt;br&gt;&lt;br&gt;The start time for the date range you are retrieving analytics for. Can be represented as either a UNIX timestamp integer or a date string in the format YYYY-MM-DD.&lt;br&gt;As template analytics are being provided with a daily granularity in the UTC timezone, a start UNIX timestamp that does not correspond to 0:00 UTC will be adjusted back to the current day&#039;s 00:00 UTC.&lt;br&gt;&lt;br&gt;If `use_waba_timezone` param has a value of true, this value must be a date string in the format YYYY-MM-DD. | `1543536000` |
| `end`&lt;br&gt;&lt;br&gt;_UNIX Timestamp or date string_ | **Required.**&lt;br&gt;&lt;br&gt;The end time for the date range you are retrieving analytics for. Can be represented as either a UNIX timestamp integer or a date string in the format YYYY-MM-DD. As template analytics are being provided with a daily granularity in the UTC timezone, an end UNIX timestamp that does not correspond to 0:00 UTC will be adjusted back to the current day&#039;s 00:00 UTC.&lt;br&gt;&lt;br&gt;If `use_waba_timezone` param has a value of true, this value must be a date string in the format YYYY-MM-DD. | `1543708800` |
| `granularity`&lt;br&gt;&lt;br&gt;_Enum_ | **Required.**&lt;br&gt;&lt;br&gt;The granularity at which you would like to retrieve the analytics. Value must be `DAILY`. | `DAILY` |
| `template_ids`&lt;br&gt;&lt;br&gt;_Array of IDs_ | **Required.**&lt;br&gt;&lt;br&gt;An array of template IDs for which you would like to retrieve analytics for.&lt;br&gt;&lt;br&gt;Maximum 10. | `[1924084211297547,954638012257287,969725530748535]` |
| `metric_types`&lt;br&gt;&lt;br&gt;_Array of enums_ | **Optional.**&lt;br&gt;&lt;br&gt;The types of metrics which you want to retrieve. If omitted or an empty array, analytics for all metric types will be returned.&lt;br&gt;&lt;br&gt;Possible values:&lt;br&gt;&lt;br&gt;* `COST`&lt;br&gt;* `CLICKED`&lt;br&gt;* `DELIVERED`&lt;br&gt;* `READ`&lt;br&gt;* `SENT`&lt;br&gt;* `APP_ACTIVATIONS (MM API for WhatsApp only)`&lt;br&gt;* `APP_ADD_TO_CART (MM API for WhatsApp only)`&lt;br&gt;* `APP_CHECKOUTS_INITIATED (MM API for WhatsApp only)`&lt;br&gt;* `APP_PURCHASES (MM API for WhatsApp only)`&lt;br&gt;* `APP_PURCHASES_CONVERSION_VALUE (MM API for WhatsApp only)`&lt;br&gt;* `WEBSITE_ADD_TO_CART (MM API for WhatsApp only)`&lt;br&gt;* `WEBSITE_CHECKOUTS_INITIATED (MM API for WhatsApp only)`&lt;br&gt;* `WEBSITE_PURCHASES (MM API for WhatsApp only)`&lt;br&gt;* `WEBSITE_PURCHASES_CONVERSION_VALUE (MM API for WhatsApp only)`&lt;br&gt;&lt;br&gt;You can [learn more about cost and click metrics here.](#template-analytics-cost-and-click-metrics).&lt;br&gt;&lt;br&gt;**Note that `COST` will not be returned for WABAs that share a Solution Partner&#039;s credit line. If your WABA shares a Solution Partner&#039;s credit line, reach out to your Solution Partner to understand your charges.** | `[SENT,DELIVERED,READ]` |
| `product_type`&lt;br&gt;&lt;br&gt;_Enum_ | **Optional.**&lt;br&gt;&lt;br&gt;The product type of the metrics you want to retrieve. If omitted, only analytics for Cloud API will be returned.&lt;br&gt;&lt;br&gt;Possible values:&lt;br&gt;&lt;br&gt;* `CLOUD_API`: Use this product type to filter for template metrics sent via Cloud API&lt;br&gt;* `MARKETING_MESSAGES_API_FOR_WHATSAPP`: Use this product type to filter for template metrics sent via Marketing Messages API for WhatsApp | `MARKETING_MESSAGES_API_FOR_WHATSAPP` |
| `&lt;USE_WABA_TIMEZONE&gt;`&lt;br&gt;&lt;br&gt;_Boolean_ | **Optional.**&lt;br&gt;&lt;br&gt;Whether to show metrics in the WABA&#039;s configured timezone. If false or omitted, metrics will be shown in UTC.&lt;br&gt;&lt;br&gt;If true, params start and end must be in the format YYYY-MM-DD. | `true` |

### Examples

#### Getting all template analytics

**Scenario:** Given a 1-day timeframe, get all template analytics metric types for an authentication template and a marketing template with a URL button.

Example Request:

```curl
curl -g &#039;https://graph.facebook.com/v25.0/109259195336416/template_analytics?start=1718064000&amp;end=1718122745&amp;granularity=daily&amp;metric_types=cost%2Cclicked%2Cdelivered%2Cread%2Csent&amp;template_ids=[1421988012088524%2C2632273056924580]&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039;
```

Example response:

```json
&#123;
  &quot;data&quot;: [
    &#123;
      &quot;granularity&quot;: &quot;DAILY&quot;,
      &quot;product_type&quot;: &quot;cloud_api&quot;, // Only available to businesses in the Marketing Messages API for WhatsApp alpha
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
        &#125;,
        &#123;
          &quot;template_id&quot;: &quot;2632273056924580&quot;,
          &quot;start&quot;: 1718064000,
          &quot;end&quot;: 1718150400,
          &quot;sent&quot;: 1,
          &quot;delivered&quot;: 1,
          &quot;read&quot;: 1,
          &quot;clicked&quot;: [
            &#123;
              &quot;type&quot;: &quot;quick_reply_button&quot;,
              &quot;button_content&quot;: &quot;Contact Support&quot;,
              &quot;count&quot;: 108
            &#125;,
            &#123;
              &quot;type&quot;: &quot;unique_url_button&quot;,
              &quot;button_content&quot;: &quot;Tell me more&quot;,
              &quot;count&quot;: 16
            &#125;
          ],
          &quot;cost&quot;: [
            &#123;
              &quot;type&quot;: &quot;amount_spent&quot;,
              &quot;value&quot;: 0.03
            &#125;,
            &#123;
              &quot;type&quot;: &quot;cost_per_delivered&quot;,
              &quot;value&quot;: 0.03
            &#125;,
            &#123;
              &quot;type&quot;: &quot;cost_per_url_button_click&quot;,
              &quot;value&quot;: 0.03
            &#125;
          ]
        &#125;
      ]
    &#125;
  ],
  &quot;paging&quot;: &#123;
    &quot;cursors&quot;: &#123;
      &quot;before&quot;: &quot;MAZDZD&quot;,
      &quot;after&quot;: &quot;MjQZD&quot;
    &#125;
  &#125;
&#125;
```

### Template analytics cost and click metrics

**Cost metrics** are returned as an array of cost objects, each with a type and value. Types can be:

* `amount_spent` — Total amount spent on conversations opened within the `start` and `end` timeframe as a result of sending the template. See [Opening Conversations](https://developers.facebook.com/documentation/business-messaging/whatsapp/pricing#opening-conversations).
* `cost_per_delivered` — The `amount_spent` value divided by the number of times the template was delivered within the `start` and `end` timeframe.
* `cost_per_url_button_click` — The `amount_spent` value divided by the number of times the template&#039;s URL button was clicked, within the `start` and `end` timeframe. Quick reply button clicks are not included. Object omitted if the template does not have a URL button.

**Click metrics** are returned as an array of JSON objects each with a type and value. Clicks are only returned for URL buttons and quick-reply buttons in templates categorized as `MARKETING` or `UTILITY`.

Types can be:

* `url_button` — The total number of clicks on the url button.
* `unique_url_button` — Unique clicks track the number of distinct WhatsApp accounts that have clicked on a button. This metric helps you understand how many individual users are engaging with your CTAs, while eliminating duplicate clicks from the same recipient and providing an accurate measurement of engagement.

### Disabling button click analytics

You can disable button click tracking on an individual template by setting its `cta_url_link_tracking_opted_out` field to `true`. Once disabled, the API will no longer return the clicked property in template analytics or display button engagement/clicks in the WhatsApp Manager when viewing the template&#039;s insights.

#### Request syntax

```html
POST /&lt;TEMPLATE_ID&gt;
  ?cta_url_link_tracking_opted_out=&lt;OPT_OUT&gt;
  &amp;category=&lt;TEMPLATE_CATEGORY&gt;
```

#### Request parameters

| Placeholder | Description | Example Value |
| --- | --- | --- |
| `&lt;WHATSAPP_TEMPLATE_ID&gt;`&lt;br&gt;&lt;br&gt;_Template ID_ | **Required.**&lt;br&gt;&lt;br&gt;Template ID. | `245435364965041` |
| `&lt;OPT_OUT&gt;`&lt;br&gt;&lt;br&gt;_Boolean_ | **Required.**&lt;br&gt;&lt;br&gt;Indicates if template button click tracking is disabled. Set to `true` to disable button click tracking on the template, or `false` to enable.&lt;br&gt;&lt;br&gt;This value is set to `false` upon template creation. | `true` |
| `&lt;TEMPLATE_CATEGORY&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Template&#039;s current category.&lt;br&gt;&lt;br&gt;If you set the template category to a value other than its current category, the template status will be set to `PENDING` and the template must undergo template review to be approved. | `marketing` |

#### Example request

```curl
curl -X POST &#039;https://graph.facebook.com/v25.0/245435364965041?cta_url_link_tracking_opted_out=true&amp;category=marketing&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039;
```

#### Example response

Upon success, the API will respond with:

```json
&#123;
    &quot;success&quot;: true
&#125;
```

## Template group analytics

The `template_group_analytics` field allows you to get the number of times templates within a [template group](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/template-groups) have been sent, delivered, and read, and the number of times their [URL buttons](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/components#url-buttons) or [Quick Reply buttons](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/components#quick-reply-buttons) have been clicked.

Data is returned with a daily granularity in the default timezone of UTC and WABA&#039;s timezone, with a lookback window of up to 90 days. To show data in the WABA&#039;s configured timezone, pass in the use_waba_timezone param with a value of true.

```json
&#123;
 &quot;data&quot;: [
   &#123;
     &quot;waba_timezone&quot;: &quot;America/Los_Angeles&quot;,
     &quot;granularity&quot;: &quot;DAILY&quot;,
     &quot;product_type&quot;: &quot;cloud_api&quot;,
     &quot;data_points&quot;: [
         ...
     ]
   &#125;
&#125;
```

### Limitations

Button click analytics are only available for templates categorized as `marketing` or `utility`.
WABAs owned by or shared with Meta Business Accounts in the European Union or Japan, or that have a business phone number with a country calling code from any of those countries or regions, are not supported.

### Enabling template analytics

You must enable template analytics on your WhatsApp Business account before you can get template group analytics. You can confirm template analytics enablement using the WhatsApp Manager or the API.

**Warning:** By confirming access via the API, you direct Meta to add insights to your WhatsApp Business account. These insights include link tracking to report website clicks. You can turn off link tracking on each message template. You also direct Meta to collect and anonymize data from your chats with customers. Meta will anonymize this data to improve services it provides you and other businesses.

To confirm enablement via API, send the following request:

`POST /&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;?is_enabled_for_insights=true`

Upon success, the API will respond with your WhatsApp Business account ID and begins capturing template group analytics for the WhatsApp Business account.

Once enabled, template analytics cannot be disabled.

### Request syntax

```html
GET /&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;/template_group_analytics
  ?granularity=daily
  &amp;start=&lt;START_TIME&gt;
  &amp;end=&lt;END_TIME&gt;
  &amp;metric_types=&lt;METRIC_TYPES&gt;
  &amp;template_group_ids=[&lt;TEMPLATE_GROUP_IDS&gt;]
```

### Template group analytics parameters

| Placeholder | Description | Example value |
| --- | --- | --- |
| `&lt;WABA_ID&gt;`&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;WhatsApp Business Account ID. | `102290129340398` |
| `&lt;START_TIME&gt;`&lt;br&gt;&lt;br&gt;_UNIX Timestamp or date string_ | **Required.**&lt;br&gt;&lt;br&gt;The start time for the date range you are retrieving analytics for. Can be represented as either a UNIX timestamp integer or a date string in the format YYYY-MM-DD.&lt;br&gt;&lt;br&gt;As template group analytics are being provided with a daily granularity in the UTC timezone, a start UNIX timestamp that does not correspond to 0:00 UTC will be adjusted back to the current day&#039;s 00:00 UTC.&lt;br&gt;&lt;br&gt;If `use_waba_timezone` param has a value of true, this value must be a date string in the format YYYY-MM-DD. | `1738465116` |
| `&lt;END_TIME&gt;`&lt;br&gt;&lt;br&gt;_UNIX Timestamp or date string_ | **Required.**&lt;br&gt;&lt;br&gt;The end time for the date range you are retrieving analytics for. Can be represented as either a UNIX timestamp integer or a date string in the format YYYY-MM-DD.&lt;br&gt;&lt;br&gt;As template group analytics are being provided with a daily granularity in the UTC timezone, an end UNIX timestamp that does not correspond to 0:00 UTC will be adjusted back to the current day&#039;s 00:00 UTC.&lt;br&gt;&lt;br&gt;If `use_waba_timezone param` has a value of true, this value must be a date string in the format YYYY-MM-DD. | `1739559516` |
| `&lt;METRIC_TYPES&gt;`&lt;br&gt;&lt;br&gt;_Array of strings_ | **Optional.**&lt;br&gt;&lt;br&gt;Array of metrics you would like to receive. If you send an empty array, the API returns results for all metric types.&lt;br&gt;&lt;br&gt;Values can be:&lt;br&gt;&lt;br&gt;- `cost`&lt;br&gt;- `clicked`&lt;br&gt;- `delivered`&lt;br&gt;- `read`&lt;br&gt;- `sent`&lt;br&gt;&lt;br&gt;Note that `COST` is not accessible to business customers who are billed through a Solution Partner.&lt;br&gt;&lt;br&gt;See [Cost and click metrics](#template-group-cost-and-click-metrics) to learn more about cost and click metrics. | ```
[
  sent,
  delivered,
  read
]
``` |
| `&lt;TEMPLATE_GROUP_IDS&gt;` | **Required.**&lt;br&gt;&lt;br&gt;An array of template group IDs for which you wish to get template group metrics.&lt;br&gt;&lt;br&gt;Maximum 10 IDs. | `102290129340398` |
| `&lt;USE_WABA_TIMEZONE&gt;`&lt;br&gt;&lt;br&gt;_Boolean_ | **Optional.**&lt;br&gt;&lt;br&gt;Whether to show metrics in the WABA&#039;s configured timezone. If false or omitted, metrics will be shown in UTC.&lt;br&gt;&lt;br&gt;If true, params start and end must be in the format YYYY-MM-DD. | `true` |

### Example request

```curl
curl -g &#039;https://graph.facebook.com/v25.0/102290129340398/template_group_analytics?granularity=daily&amp;start=1738465116&amp;end=1739559516&amp;metric_types=sent,delivered,read&amp;template_group_ids=[1044106240855852]&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039;
```

### Example response

Note that the example below has been truncated with an ellipsis (`...`) for brevity.

```json
&#123;
  &quot;data&quot;: [
    &#123;
      &quot;granularity&quot;: &quot;DAILY&quot;,
      &quot;data_points&quot;: [
        &#123;
          &quot;template_group_id&quot;: &quot;1044106240855852&quot;,
          &quot;start&quot;: 1739491200,
          &quot;end&quot;: 1739577600,
          &quot;sent&quot;: 1460,
          &quot;delivered&quot;: 1460,
          &quot;read&quot;: 1399
        &#125;,
        &#123;
          &quot;template_group_id&quot;: &quot;1044106240855852&quot;,
          &quot;start&quot;: 1739404800,
          &quot;end&quot;: 1739491200,
          &quot;sent&quot;: 673,
          &quot;delivered&quot;: 673,
          &quot;read&quot;: 645
        &#125;
        ...
      ]
    &#125;
  ],
  &quot;paging&quot;: &#123;
    &quot;cursors&quot;: &#123;
      &quot;before&quot;: &quot;MAZDZD&quot;,
      &quot;after&quot;: &quot;MjQZD&quot;
    &#125;
  &#125;
&#125;
```

### Template group cost and click metrics

**Cost metrics** are returned as an array of cost objects, each with a type and value. Types can be:

* `amount_spent` — Total amount spent on conversations opened within the `start` and `end` timeframe as a result of sending the template. See [Opening Conversations](https://developers.facebook.com/documentation/business-messaging/whatsapp/pricing#opening-conversations).
* `cost_per_delivered` — The `amount_spent` value divided by the number of times the template was delivered within the `start` and `end` timeframe.
* `cost_per_url_button_click` — The `amount_spent` value divided by the number of times the template&#039;s URL button was clicked, within the `start` and `end` timeframe. Quick reply button clicks are not included. Object omitted if the template does not have a URL button.

**Click metrics** are returned as an array of JSON objects each with a type and value. Clicks are only returned for URL buttons and quick-reply buttons in templates categorized as `marketing` or `utility`.

Types can be:

* `url_button` — The total number of clicks on the url button.
* `unique_url_button` — Unique clicks track the number of distinct WhatsApp accounts that have clicked on a button. This metric helps you understand how many individual users are engaging with your CTAs, while eliminating duplicate clicks from the same recipient and providing an accurate measurement of engagement.

## Call analytics

The `call_analytics` field provides the number and type of calls made and received by the phone numbers associated with a specific WABA. When requesting the `call_analytics` field, you can attach the following filtering parameters.

### Call analytics parameters

| Name | Description | Example value |
| --- | --- | --- |
| `start`&lt;br&gt;&lt;br&gt;type: UNIX Timestamp | **Required.**&lt;br&gt;&lt;br&gt;The start date for the date range for which you are retrieving analytics. | `1728581152` |
| `end`&lt;br&gt;&lt;br&gt;type: UNIX Timestamp | **Required.**&lt;br&gt;&lt;br&gt;The end date for the date range for which you are retrieving analytics. | `1728581152` |
| `granularity`&lt;br&gt;&lt;br&gt;type: String | **Required.**&lt;br&gt;&lt;br&gt;The granularity at which you would like to retrieve the analytics. Supported Options:&lt;br&gt;&lt;br&gt;- `HALF_HOUR`&lt;br&gt;- `DAILY`&lt;br&gt;- `MONTHLY` | `DAILY` |
| `phone_numbers`&lt;br&gt;&lt;br&gt;type: Array | **Optional.**&lt;br&gt;&lt;br&gt;An array of phone numbers for which you would like to retrieve analytics. If not provided, all phone numbers added to your WABA are included. | `[15550783881,15550783882]` |
| `country_codes`&lt;br&gt;&lt;br&gt;type: Array | **Optional.**&lt;br&gt;&lt;br&gt;The countries for which you would like to retrieve analytics. Provide an array with 2-letter country codes for the countries you would like to include. If not provided, analytics will be returned for all countries you have communicated with. | `[US,BR]` |
| `directions`&lt;br&gt;&lt;br&gt;_Array of enums_ | **Optional.**&lt;br&gt;&lt;br&gt;The call direction for which you would like to retrieve the analytics. Supported Options:&lt;br&gt;- `USER_INITIATED`&lt;br&gt;- `BUSINESS_INITIATED` | `USER_INITIATED` |
| `dimensions`&lt;br&gt;&lt;br&gt;_Array of enums_ | **Optional.**&lt;br&gt;&lt;br&gt;List of breakdowns you would like to apply to your metrics. If you send an empty list, the API returns results without any breakdowns. Supported Options:&lt;br&gt;- `phone`&lt;br&gt;- `direction`&lt;br&gt;- `country` | `direction` |
| `metric_types`&lt;br&gt;&lt;br&gt;_Array of enums_ | **Optional.**&lt;br&gt;&lt;br&gt;Array of metrics you would like to receive. If you send an empty array, the API returns results for all metric types. Supported Options:&lt;br&gt;- `COUNT`&lt;br&gt;- `COST`&lt;br&gt;- `AVERAGE_DURATION` | `AVERAGE_DURATION` |

### Example

**Scenario:** You need to get the number of user-initiated calls received by all phone numbers associated with your WABA in per day granularity.

**Suggested Solution:** Use following filtering parameters: `start`, `end`, `granularity`, `directions`.

```curl
curl -i -X GET &quot;https://graph.facebook.com/v25.0/102290129340398
  ?fields=call_analytics
  .start(1759302000)
  .end(1767168000)
  .granularity(DAILY)
  .directions(USER_INITIATED)
  &amp;access_token=BLI8lkj...&quot;
```

A successful response returns a `call_analytics` object with the data you have requested:

```json
&#123;
  &quot;call_analytics&quot;: &#123;
    &quot;granularity&quot;: &quot;DAILY&quot;,
    &quot;directions&quot;: &quot;USER_INITIATED&quot;,
    &quot;data_points&quot;: [
      &#123;
          &quot;start&quot;: 1765958400,
          &quot;end&quot;: 1766044800,
          &quot;cost&quot;: 0.47795,
          &quot;count&quot;: 35,
          &quot;average_duration&quot;: 106
      &#125;,
      &#123;
          &quot;start&quot;: 1760943600,
          &quot;end&quot;: 1761030000,
          &quot;cost&quot;: 0,
          &quot;count&quot;: 20,
          &quot;average_duration&quot;: 103
      &#125;,
      &#123;
          &quot;start&quot;: 1760857200,
          &quot;end&quot;: 1760943600,
          &quot;cost&quot;: 0,
          &quot;count&quot;: 24,
          &quot;average_duration&quot;: 103
      &#125;,
      # more data points
    ]
  &#125;,
  &quot;id&quot;: &quot;102290129340398&quot;
&#125;
```

## Group analytics

The Group Analytics API allows you to get the number of messages sent, delivered, and read in WhatsApp groups, as well as the number of participants who joined or left.

Data is returned with a daily granularity, with a lookback window of up to 90 days.

### Request syntax

```html
GET /&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;/group_analytics
  ?granularity=daily
  &amp;start=&lt;START_TIME&gt;
  &amp;end=&lt;END_TIME&gt;
  &amp;metric_types=[&lt;METRIC_TYPES&gt;]
  &amp;group_ids=[&lt;GROUP_IDS&gt;]
```

### Group analytics parameters

| Placeholder | Description | Example value |
| --- | --- | --- |
| `&lt;START_TIME&gt;`&lt;br&gt;&lt;br&gt;_UNIX Timestamp_ | **Required.**&lt;br&gt;&lt;br&gt;The start time for the date range you are retrieving analytics for. Must be no more than 90 days from the current date. | `1685548801` |
| `&lt;END_TIME&gt;`&lt;br&gt;&lt;br&gt;_UNIX Timestamp_ | **Required.**&lt;br&gt;&lt;br&gt;The end time for the date range you are retrieving analytics for. | `1685721600` |
| `&lt;GROUP_IDS&gt;`&lt;br&gt;&lt;br&gt;_Array of strings_ | **Required.**&lt;br&gt;&lt;br&gt;An array of group IDs for which you wish to get group metrics.&lt;br&gt;&lt;br&gt;Currently only supports 1 ID. | `[&quot;GROUP_ID&quot;]` |
| `&lt;GRANULARITY&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;The granularity at which you would like to retrieve the analytics.&lt;br&gt;&lt;br&gt;Values can be:&lt;br&gt;&lt;br&gt;- `DAILY`&lt;br&gt;&lt;br&gt;Default: `DAILY`. | `DAILY` |
| `&lt;METRIC_TYPES&gt;`&lt;br&gt;&lt;br&gt;_Array of strings_ | **Required.**&lt;br&gt;&lt;br&gt;Array of metrics you would like to receive.&lt;br&gt;&lt;br&gt;Values can be:&lt;br&gt;&lt;br&gt;- `SENT` — Number of messages sent by the business to the group.&lt;br&gt;- `DELIVERED` — Number of times a message was delivered to a participant in the group.&lt;br&gt;- `READ` — Number of times a message was read by a participant in the group.&lt;br&gt;- `PARTICIPANTS_JOINED` — Number of times a participant joined the group.&lt;br&gt;- `PARTICIPANTS_LEFT` — Number of times a participant left the group. | `[&quot;SENT&quot;,&quot;READ&quot;,&quot;PARTICIPANTS_JOINED&quot;]` |

### Example request

```curl
curl -g &#039;https://graph.facebook.com/v25.0/102290129340398/group_analytics?start=1764662400&amp;end=1764921600&amp;granularity=DAILY&amp;group_ids=[&#039;GROUP_ID&#039;]&amp;metric_types=[&#039;SENT&#039;,&#039;DELIVERED&#039;, &#039;READ&#039;,&#039;PARTICIPANTS_JOINED&#039;,&#039;PARTICIPANTS_LEFT&#039;]&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039;
```

### Example response

Note that the example below has been truncated with an ellipsis (`...`) for brevity.

```json
&#123;
  &quot;data&quot;: [
    &#123;
      &quot;granularity&quot;: &quot;DAILY&quot;,
      &quot;data_points&quot;: [
          &#123;
            &quot;group_id&quot;: &quot;GROUP_ID&quot;,
            &quot;start&quot;: 1685548801,
            &quot;end&quot;: 1685635200,
            &quot;sent&quot;: 100,
            &quot;delivered&quot;: 250,
            &quot;read&quot;: 200,
            &quot;joined&quot;: 3,
            &quot;left&quot;: 1
          &#125;,
          &#123;
            &quot;group_id&quot;: &quot;GROUP_ID&quot;,
            &quot;start&quot;: 1685635201,
            &quot;end&quot;: 1685721600,
            &quot;sent&quot;: 80,
            &quot;delivered&quot;: 200,
            &quot;read&quot;: 150,
            &quot;joined&quot;: 1,
            &quot;left&quot;: 0
          &#125;
          ...
        ]
    &#125;
  ],
  &quot;paging&quot;: &#123;
    &quot;cursors&quot;: &#123;
      &quot;before&quot;: &quot;MAZDZD&quot;,
      &quot;after&quot;: &quot;MjQZD&quot;
    &#125;
  &#125;
&#125;
```

## Reference

For a list of all possible values for each field, see the Graph API reference of the [WhatsApp Business account Analytics field](https://developers.facebook.com/docs/graph-api/reference/waba-analytics).
