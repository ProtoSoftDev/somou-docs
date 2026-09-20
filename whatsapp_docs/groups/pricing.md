# Groups API Pricing



## Per-message pricing on Groups API

Groups API uses Cloud API&#039;s [per-message pricing model](https://developers.facebook.com/documentation/business-messaging/whatsapp/pricing#per-message-pricing) to determine if a given message is billable. However, **you are charged each time a billable message is delivered to someone in the group.**

For example, if you send a (billable) marketing template message to a group with 5 WhatsApp users and it is delivered to all 5 users, you would be charged for 5 delivered messages at the going marketing message rate for each recipient&#039;s country calling code.

If the message was delivered to only 4 of the 5 users, you would only be charged for the 4 delivered messages.

## How customer service windows work with Groups API

Customer service windows work differently when using Groups API.

When any WhatsApp user in the group messages you, a [customer service window](https://developers.facebook.com/documentation/business-messaging/whatsapp/messages/send-messages#customer-service-windows) is opened between you and the entire group (or is refreshed, if one already exists). This allows you to send utility and marketing template messages, or free form messages, for free.

This is different from 1:1 messaging, where when a WhatsApp user messages you, a [customer service window](https://developers.facebook.com/documentation/business-messaging/whatsapp/messages/send-messages#customer-service-windows) is opened between you and that customer (or is refreshed, if one already exists).

Everything else about [customer service windows](https://developers.facebook.com/documentation/business-messaging/whatsapp/messages/send-messages#customer-service-windows) remains the same.

## Pricing information in message status webhook

Pricing information for messages sent using Groups API is included in [messages status webhooks](https://developers.facebook.com/documentation/business-messaging/whatsapp/groups/webhooks#pricing-information).

### How `read` and `delivered` message status webhooks are processed

In order for a message status to be considered `read`, it must have been at least `delivered`.

In some scenarios, such as when a user is present in the chat thread when a message arrives, the message is marked `delivered` and `read` nearly simultaneously. In this and other similar scenarios, the `delivered` webhook is not sent back. This is because it is implied that the message was delivered since it has been read.

### How pricing data is displayed in the Message Status webhook

Not all Message Status webhooks include pricing information.

With the introduction of [Per-message Pricing](https://developers.facebook.com/documentation/business-messaging/whatsapp/pricing#per-message-pricing), pricing data can be present in `sent`, `delivered` or `read` status webhook. If a message is **charged**, you can expect that at least one webhook (`delivered` or `read`) will contain the pricing information.

### Sent message status webhook

```https
// All versions

&quot;pricing&quot;: &#123;
  &quot;billable&quot;: &quot;&lt;IS_BILLABLE&gt;&quot;,
  &quot;pricing_model&quot;: &quot;&lt;PRICING_MODEL&gt;&quot;,  // new value, see table below
  &quot;type&quot;: &quot;&lt;PRICING_TYPE&gt;&quot;,            // new property, see table below
  &quot;category&quot;: &quot;&lt;CONVERSATION_CATEGORY&gt;&quot;
&#125;
```

### Delivered / Read message status webhook

```https
// Version 24.0 and higher

&quot;pricing&quot;: &#123;
  &quot;billable&quot;: &quot;&lt;IS_BILLABLE?&gt;&quot;,
  &quot;pricing_model&quot;: &quot;&lt;PRICING_MODEL&gt;&quot;,  // new value, see table below
  &quot;type&quot;: &quot;&lt;PRICING_TYPE&gt;&quot;,            // new property, see table below
  &quot;category&quot;: &quot;&lt;CONVERSATION_CATEGORY&gt;&quot;
&#125;
// Version 23.0 and lower
&quot;conversation&quot;: &#123;
  &quot;id&quot;: &quot;&lt;CONVERSATION_ID&gt;&quot;,           // new behavior, see table below
  &quot;expiration_timestamp&quot;: &quot;&lt;CONVERSATION_EXPIRATION_TIMESTAMP&gt;&quot;,
  &quot;origin&quot;: &#123;
    &quot;type&quot;: &quot;&lt;CONVERSATION_CATEGORY&gt;&quot;
  &#125;
&#125;,

&quot;pricing&quot;: &#123;
  &quot;billable&quot;: &quot;&lt;IS_BILLABLE?&gt;&quot;,
  &quot;pricing_model&quot;: &quot;PMP&quot;,              // Value is now &quot;PMP&quot; instead of &quot;CBP&quot;
  &quot;type&quot;: &quot;&lt;PRICING_TYPE&gt;&quot;,            // new property, see table below
  &quot;category&quot;: &quot;&lt;PRICING_CATEGORY&gt;&quot;
&#125;
```

### Parameters

| Placeholder | Description |
| --- | --- |
| `&lt;CONVERSATION_ID&gt;` | Version 24.0 and higher:&lt;br&gt;&lt;br&gt;- The `conversation` object will be omitted entirely&lt;br&gt;&lt;br&gt;Version 23.0 and lower:&lt;br&gt;&lt;br&gt;- Value will now be set to a unique ID per-message, instead of per-conversation. |
| `&lt;CONVERSATION_CATEGORY&gt;` | Not changing. |
| `&lt;CONVERSATION_EXPIRATION_TIMESTAMP&gt;` | Not changing. |
| `&lt;IS_BILLABLE?&gt;` | Not changing.&lt;br&gt;&lt;br&gt;However, the `billable` property will be deprecated in a future [versioned release](https://developers.facebook.com/docs/graph-api/guides/versioning#calling_older_versions). Start using `pricing.type` and `pricing.category` together to determine if a message is billable, and if so, its [billing rate](#identifying-billable-messages). |
| `&lt;PRICING_TYPE&gt;` | New property. Values can be:&lt;br&gt;&lt;br&gt;- `regular` — indicates the message is billable.&lt;br&gt;- `free_group_customer_service` — indicates the message is free because it was either a utility template message or non-template message sent within a [customer service window](https://developers.facebook.com/documentation/business-messaging/whatsapp/messages/send-messages#customer-service-windows). |
| `&lt;PRICING_CATEGORY&gt;` | Values are not changing, but can now be interpreted as follows:&lt;br&gt;&lt;br&gt;* `group_marketing` — indicates a marketing template message.&lt;br&gt;* `group_utility` — indicates a utility template message.&lt;br&gt;* `group_service` — indicates a non-template message. |

### Identifying billable messages

Billable messages have `pricing.type` set to `regular`. The `pricing.category` value indicates the rate (`group_marketing` or `group_utility`).

### Identifying free messages

Free messages have `pricing.type` set to `free_group_customer_service`. The `pricing.category` value tells you why it was free:

* `group_utility` — the message was sent within an open group customer service window.
* `group_service` — all non-template messages are free.

## Messaging analytics for Groups API

The `analytics` field provides the number and type of messages sent and delivered by the phone numbers associated with a specific WABA — for conversation metrics, see Conversation Analytics.

You can use the following endpoint to retrieve analytics for messages sent using Groups API:

```https
/&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;?fields=analytics.&lt;FILTER_PARAMETER&gt;.&lt;FILTER_PARAMETER&gt;...
```

### Filter parameters for messaging analytics

For a full list of messaging analytics filter parameters, view the [Messaging Analytics reference](https://developers.facebook.com/documentation/business-messaging/whatsapp/analytics#conversation-analytics).

### Changes to filter parameters for Groups API

| Name | Description |
| --- | --- |
| `product_types`&lt;br&gt;&lt;br&gt;type: Array | _Optional._&lt;br&gt;&lt;br&gt;The types of messages (notification messages and/or customer support messages) for which you want to retrieve notifications.&lt;br&gt;&lt;br&gt;Provide an array and include:&lt;br&gt;* `101` for group notification messages&lt;br&gt;* `102` for group customer support messages.&lt;br&gt;* `103` for inbound group messages&lt;br&gt;&lt;br&gt;If the above values are not provided, the API call will return analytics for all messages together.&lt;br&gt;&lt;br&gt;Inbound product type cannot be queried together with other product types, or you will see an error similar to the one below:&lt;br&gt;&lt;br&gt;```https
 &#123;
 &quot;error&quot;: &#123;
   &quot;message&quot;: &quot;Invalid parameter&quot;,
   &quot;type&quot;: &quot;OAuthException&quot;,
   &quot;code&quot;: 100,
   &quot;error_subcode&quot;: 2388077,
   &quot;is_transient&quot;: false,
   &quot;error_user_title&quot;: &quot;Insight Invalid Product Type Combination&quot;,
   &quot;error_user_msg&quot;: &quot;Unable to query this combination of product types. Please query individually and try again.&quot;,
 &#125;
&#125;
``` |

### Response value

Successful responses to the analytics API when querying Groups API message data will return an object similar to the following:

**Note: The country code filter is not supported for group sent messages.**

```https
With Country code filter
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
        &quot;delivered&quot;: 179715,
        &quot;groups_delivered&quot;: 4
      &#125;,
      &#123;
        &quot;start&quot;: 1543629600,
        &quot;end&quot;: 1543716000,
        &quot;sent&quot;: 147649,
        &quot;delivered&quot;: 139032
      &#125;
      # more data points
    ]
  &#125;,
  &quot;id&quot;: &quot;102290129340398&quot;
&#125;

Without Country code filter
&#123;
  &quot;analytics&quot;: &#123;
    &quot;phone_numbers&quot;: [
      &quot;16505550111&quot;,
      &quot;16505550112&quot;,
      &quot;16505550113&quot;
    ],
    &quot;granularity&quot;: &quot;DAY&quot;,
    &quot;data_points&quot;: [
      &#123;
        &quot;start&quot;: 1543543200,
        &quot;end&quot;: 1543629600,
        &quot;sent&quot;: 196093,
        &quot;delivered&quot;: 179715,
        &quot;groups_sent&quot;: 2,
        &quot;groups_delivered&quot;: 4
      &#125;,
      &#123;
        &quot;start&quot;: 1543629600,
        &quot;end&quot;: 1543716000,
        &quot;sent&quot;: 147649,
        &quot;delivered&quot;: 139032
      &#125;
      # more data points
    ]
  &#125;,
  &quot;id&quot;: &quot;102290129340398&quot;
&#125;
```

## Pricing analytics for Groups API

The `pricing_analytics` field allows you to get pricing breakdowns for any messages delivered within a specified date range.

```https
GET /&lt;WABA_ID&gt;
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

### Filter parameters for pricing analytics

For a full list of messaging analytics filter parameters, view the [Messaging Analytics reference](https://developers.facebook.com/documentation/business-messaging/whatsapp/analytics#pricing-analytics).

### Changes to filter parameters for Groups API

| Name | Description |
| --- | --- |
| `&lt;PRICING_CATEGORIES&gt;`&lt;br&gt;&lt;br&gt;_Array of strings_ | _Optional._&lt;br&gt;&lt;br&gt;Array of pricing categories. If you send an empty array, you receive results for all pricing categories.&lt;br&gt;&lt;br&gt;Values can be:&lt;br&gt;&lt;br&gt;* `GROUP_MARKETING`: Group messages charged the marketing rate.&lt;br&gt;* `GROUP_SERVICE`: Group messages that were not charged. Includes all non-template messages and utility messages sent inside of a customer service window.&lt;br&gt;* `GROUP_UTILITY`: Group messages charged the utility rate. |
| `&lt;PRICING_TYPES&gt;`&lt;br&gt;&lt;br&gt;_Array of strings_ | _Optional._&lt;br&gt;&lt;br&gt;Array of pricing types. If you send an empty array, you receive results for all pricing types.&lt;br&gt;&lt;br&gt;Values can be:&lt;br&gt;&lt;br&gt;* `FREE_GROUP_CUSTOMER_SERVICE`: Free group messages. These are non-template messages and utility messages sent within group customer service windows.&lt;br&gt;* `REGULAR`: Billable messages. Includes all authentication and marketing template messages, and any utility template messages sent outside of a customer service window. |

## Rate cards

**Warning:** Group utility messages are not eligible for volume tiers.

Messaging rates for Groups API are the same as per-messaging pricing rates for 1 to 1 messaging.

[View per-message pricing rate cards](https://developers.facebook.com/documentation/business-messaging/whatsapp/pricing#rate-cards-and-volume-tiers)
