# Obtain user call permissions



**Warning:** As of November 3, 2025, permanent permissions is now available. Users can now grant a business ongoing permission to call. Users can review and change calling permission for a business at any time in the business profile.

**Note:** Call permission related features are available only in regions where [business initiated calling is available](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling#availability).

## Overview

If you want to place a call to a WhatsApp user, your business must receive user permission first. When a WhatsApp user grants call permissions, they can be either temporary or permanent.

Your business does not have control over this permission; only the user can grant or revoke it, at any time. WhatsApp stores permanent permission data until the user revokes the permission.

You can obtain calling permission from a WhatsApp user in any of the following ways:

1. **Send a call permission request to the user** — Send a free-form or templated message requesting calling permission from the user. The user can choose between temporary or permanent.
1. **Callback permission is provided by the WhatsApp user** — The WhatsApp user automatically provides temporary call permissions by placing a call to the business. You must [enable the callback setting](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/call-settings#configure-update-business-phone-number-calling-settings) on the business phone number.
1. **WhatsApp user provides call permission via Business Profile** — The WhatsApp user provides call permissions to the business through their business profile.

### Limits (per business and WhatsApp user pair)

* Temporary permissions are **granted for 7 calendar days (168 hours)**
  * Calculated as the number of seconds in a day multiplied by 7, from the time of the user&#039;s approval.
* Permanent permissions do not expire, but they have the same connected calls limit.
* Your business can make a maximum of **100 connected calls every 24 hours**
* These limits are on the **business phone number**

These limits are in place to protect WhatsApp users from unwanted calls.

**Warning:** When you test your WhatsApp Calling integration using public test numbers (PTNs) and sandbox accounts, Calling API restrictions are relaxed.

[Learn more about testing your WhatsApp Calling API integration](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling#testing-and-sandbox-accounts)

## Call permission request basics

You can proactively request a calling permission from a WhatsApp user by sending a permission request message, either as a:

* Free form interactive message
* Template message

The WhatsApp user may approve (temporary or permanent), decline, or simply not respond to a call permission request.

**With permissions, the WhatsApp user is in control.** Even if the user provides calling permission, they can revoke the granted permission at any time. Conversely, if the user declines a permission request, they can still grant calling permission, up until the permission request expires.

**A call permission request expires** when any of the following occurs:

* The WhatsApp user interacts with a subsequent new call permission request from the business
* 7 days after the permission was accepted or declined by the WhatsApp user
* 7 days after the permission was delivered if the WhatsApp user does not respond to the request

[View client UI behavior for expired permission requests](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/user-call-permissions#call-permission-request-expiration-scenarios)

To ensure an optimal user experience around business initiated calling, the following limits are enforced:

1. **When sending a calling permission request message**
* Maximum of 1 permission request in 24 hours
* Maximum 2 permission requests within 7 days.
    * _These limits reset when any connected call (business-initiated/user-initiated) is made between the business and WhatsApp user._
    * _These limits apply toward permissions requests sent either as free form or template messages._

1. **When business-initiated calls go unanswered or are rejected**
* 2 consecutive unanswered calls result in a system message to reconsider an approved permission
* 4 consecutive unanswered calls result in an approved permission being automatically revoked. The user may again update this if they so choose.

[View client UI behavior for consecutive unanswered calls](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/user-call-permissions#consecutive-unanswered-calls)

## Free form vs template call permission request message

**Note:** Call permission request messages are subject to [messaging charges](https://developers.facebook.com/documentation/business-messaging/whatsapp/pricing)

A call permission request message can be sent to users in one of the following ways:

**Send a free form message**

* When you are within a customer service window with a WhatsApp user, you can send a free form message with a call permission request.
* The text body is optional. Include a text body to build context with the WhatsApp user. Free form calling permission request messages do not support header and footer sections.
* Since the customer service window is open, there is no need to create a conversation window.

**Create and send a template message**

* Sending a template message allows you to initiate a user conversation with a call permission request.
* Context (that is, a text body) is required when sending a template message with a call permission request.
* With template messages, you can further customize your permission request by adding a message header and footer.

## Client application UI experience

### Call permission request flow and sample messages

#### Allow calls

#### Temporarily allow calls

### Template message

With header, footer and body

With body only

With no text body

#### Free form message types

With no text body

With text body only

### Updating call permission on business profile
Users always have the option to change the permission using a new option on the business profile.

| Update call permission on business profile |
| --- |
|  |

### Consecutive unanswered calls

| Consecutive unanswered calls |
| --- |
| 2 consecutive unanswered calls — System message for user to update permission |
| 4 consecutive unanswered calls — Permissions automatically revoked |

### Call permission request expiration scenarios

Permission request expires after 7 days — User interacts with request

Permission request expires after 7 days — User does not interact

Previous permission request expires immediately — User does not interact / New call permission request is received

Previous permission request expires immediately — User allows / Interacts with the new request

## Send free form call permission request message

**Note:** Call permission request messages are subject to [messaging charges](https://developers.facebook.com/documentation/business-messaging/whatsapp/pricing)

Use this endpoint to send a free form interactive message with a call permission request during a [customer service window](https://developers.facebook.com/documentation/business-messaging/whatsapp/messages/send-messages#customer-service-windows). Cloud API sends a standard [message status webhook](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/messages/status) in response to this message send.

**Note:** The call permission request interactive object cannot be edited by the business. Only the message body can be customized.

[See how this message is rendered on the WhatsApp client](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/user-call-permissions#call-permission-request-flow-and-sample-messages)

#### Request syntax

```https
POST &lt;PHONE_NUMBER_ID&gt;/messages
```

| Parameter | Description | Sample Value |
| --- | --- | --- |
| `&lt;PHONE_NUMBER_ID&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | **Required**&lt;br&gt;&lt;br&gt;&lt;br&gt;The business phone number which you are sending messages from.&lt;br&gt;&lt;br&gt;[Learn more about formatting phone numbers in Cloud API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/whatsapp-business-account-phone-number-api) | `+18274459827` |

#### Request body

```https
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;recipient_type&quot;: &quot;individual&quot;,
  &quot;to&quot;: &quot;&lt;PHONE_NUMBER_ID&gt; or &lt;WHATSAPP_ID&gt;&quot;,
  &quot;recipient&quot;: &quot;US.13491208655302741918&quot;,
  &quot;type&quot;: &quot;interactive&quot;,
  &quot;interactive&quot;: &#123;
    &quot;type&quot;: &quot;call_permission_request&quot;,
    &quot;action&quot;: &#123;
      &quot;name&quot;: &quot;call_permission_request&quot;
    &#125;,
    &quot;body&quot;: &#123;
      &quot;text&quot;: &quot;We would like to call you to help support your query on Order No: ON-12853.&quot;
    &#125;
  &#125;
&#125;
```

#### Body parameters

| Parameter | Description | Sample Value |
| --- | --- | --- |
| `to`&lt;br&gt;&lt;br&gt;_Integer_ | **Required** (unless `recipient` is provided)&lt;br&gt;&lt;br&gt;The phone number of the WhatsApp user you are messaging&lt;br&gt;&lt;br&gt;[Learn more about formatting phone numbers in Cloud API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/whatsapp-business-account-phone-number-api) | `+17863476655` |
| `recipient`&lt;br&gt;&lt;br&gt;_String_ | **Optional**&lt;br&gt;&lt;br&gt;The WhatsApp user&#039;s business-scoped user ID (BSUID) or parent BSUID. Use this instead of, or in addition to, `to`. If you include both, `to` takes precedence.&lt;br&gt;&lt;br&gt;[Learn more about business-scoped user IDs](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-scoped-user-ids#business-scoped-user-id) | `US.13491208655302741918` |
| `type`&lt;br&gt;&lt;br&gt;_String_ | **Required**&lt;br&gt;&lt;br&gt;The type of interactive message you are sending.&lt;br&gt;&lt;br&gt;In this case, you are sending a `call_permission_request`.&lt;br&gt;&lt;br&gt;[Learn more about interactive messages](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/message-api) | `&quot;call_permission_request&quot;` |
| `action`&lt;br&gt;&lt;br&gt;_String_ | **Required**&lt;br&gt;&lt;br&gt;The action of your interactive message.&lt;br&gt;&lt;br&gt;Must be `call_permission_request`. | `&quot;call_permission_request&quot;` |
| `body`&lt;br&gt;&lt;br&gt;_String_ | **Optional**&lt;br&gt;&lt;br&gt;The body of your message.&lt;br&gt;&lt;br&gt;Although this field is optional, give the WhatsApp user context when you request permission to call them. | `&quot;Allow us to call you so we can support you with your order.&quot;` |

#### Success response

```json
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;contacts&quot;: [&#123;
      &quot;input&quot;: &quot;+1-408-555-1234&quot;,
      &quot;wa_id&quot;: &quot;14085551234&quot;,
      &quot;user_id&quot;: &quot;&lt;BSUID&gt;&quot;,
      &quot;parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot;
    &#125;],
  &quot;messages&quot;: [&#123;
      &quot;id&quot;: &quot;wamid.gBGGFlaCmZ9plHrf2Mh-o&quot;
    &#125;]
&#125;
```

[_Learn more about messaging success responses_](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/message-api)

**Note:** **Usernames and business-scoped user IDs:** When sending a call permission request message, you can use the `recipient` field to identify the user by BSUID, and the response may include `user_id` and `parent_user_id` fields; the user&#039;s phone number may be omitted. For details, see [Business-scoped user IDs](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-scoped-user-ids#business-scoped-user-id).

#### Error response

Possible errors that can occur:

* Invalid `phone-number-id`
* Permissions/Authorization errors
* Rate limit reached
* Sending this message to users on older app versions will result in error webhook with error code [131026](https://developers.facebook.com/documentation/business-messaging/whatsapp/support/error-codes)
* Calling not enabled
* Calling restriction errors

[View general Cloud API Error Codes here](https://developers.facebook.com/documentation/business-messaging/whatsapp/support/error-codes)

## Create and send call permission request template messages

**Note:** Call permission request messages are subject to [messaging charges](https://developers.facebook.com/documentation/business-messaging/whatsapp/pricing)

Use these endpoints to create and send a call permission request message template.

Once your permission request template message is created, your business can send the template message to the user as a call permission request outside of a customer service window.

[Learn more about creating and managing message templates](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/overview)

### Create message template

Use this endpoint to create a call permission request message template.

#### Request syntax

```https
POST/&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;/message_templates
```

| Parameter | Description | Sample Value |
| --- | --- | --- |
| `&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required**&lt;br&gt;&lt;br&gt;Your WhatsApp Business account ID.&lt;br&gt;&lt;br&gt;[Learn how to find your WABA ID](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/whatsapp-business-account-api) | `&quot;waba-90172398162498126&quot;` |

#### Request body

```json
&#123;
  &quot;name&quot;: &quot;sample_cpr_template&quot;,
  &quot;language&quot;: &quot;en&quot;,
  &quot;category&quot;: &quot;[MARKETING|UTILITY]&quot;,
  &quot;components&quot;: [
     &#123;
      &quot;type&quot;: &quot;HEADER&quot;,
      &quot;text&quot;: &quot;Support of Order No: &#123;&#123;1&#125;&#125;&quot;,
      &quot;example&quot;: &#123;
        &quot;body_text&quot;: [
          [
            &quot;ON-12345&quot;
          ]
        ]
      &#125;
    &#125;,
    &#123;
      &quot;type&quot;: &quot;BODY&quot;,
      &quot;text&quot;: &quot;We would like to call you to help support your query on Order No: &#123;&#123;1&#125;&#125; for the item &#123;&#123;2&#125;&#125;.&quot;,
      &quot;example&quot;: &#123;
        &quot;body_text&quot;: [
          [
            &quot;ON-12345&quot;,
            &quot;Avocados&quot;
          ]
        ]
      &#125;
    &#125;,
    &#123;
      &quot;type&quot;: &quot;FOOTER&quot;,
      &quot;text&quot;: &quot;Talk to you soon!&quot;
    &#125;,
    &#123;
      &quot;type&quot;: &quot;call_permission_request&quot;
    &#125;
  ]
&#125;
```

#### Body parameters

Creating and managing template messages can be done both through Cloud API and the Meta Business Suite interface.

When creating your call permission request template, ensure you configure `type` as `call_permission_request`.

[Learn more about creating and managing message templates](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/overview)

| Parameter | Description | Sample Value |
| --- | --- | --- |
| `type`&lt;br&gt;&lt;br&gt;_String_ | **Required**&lt;br&gt;&lt;br&gt;The type of template message you are creating.&lt;br&gt;&lt;br&gt;In this case, you are creating a `call_permission_request`. | `&quot;call_permission_request&quot;` |

#### Template status response

```https
&#123;
  &quot;id&quot;: &quot;&lt;ID&gt;&quot;,
  &quot;status&quot;: &quot;&lt;STATUS&gt;&quot;,
  &quot;category&quot;: &quot;&lt;CATEGORY&gt;&quot;
&#125;
```

[_Learn more about template status response_](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/overview#template-status)

#### Error response

Possible errors that can occur:

* Invalid WABA id
* Permissions/Authorization errors
* Template structure/component validation alerts

[View general Cloud API Error Codes here](https://developers.facebook.com/documentation/business-messaging/whatsapp/support/error-codes)

### Send message template

Use this endpoint to send a call permission request message template

The following is a simplified sample of the send template message request, however you can [learn more about how to send message templates here.](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/overview)

#### Request syntax

```https
POST/&lt;PHONE_NUMBER_ID&gt;/messages
```

| Parameter | Description | Sample Value |
| --- | --- | --- |
| `&lt;PHONE_NUMBER_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required**&lt;br&gt;&lt;br&gt;The business phone number which you are sending a message from.&lt;br&gt;&lt;br&gt;[Learn more about formatting phone numbers in Cloud API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/whatsapp-business-account-phone-number-api) | `+18762639988` |

#### Request body

```https
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;recipient_type&quot;: &quot;individual&quot;,
  &quot;to&quot;: &quot;+13287759822&quot;, // The WhatsApp user who will receive the template message
  &quot;recipient&quot;: &quot;US.13491208655302741918&quot;,
  &quot;type&quot;: &quot;template&quot;,
  &quot;template&quot;: &#123;
    &quot;name&quot;: &quot;sample_cpr_template&quot;, // The call permission request template name
    &quot;language&quot;: &#123;
      &quot;code&quot;: &quot;en&quot;
    &#125;,
    &quot;components&quot;: [ // Body text parameters such as customer name and order number
      &#123;
        &quot;type&quot;: &quot;body&quot;,
        &quot;parameters&quot;: [
          &#123;
            &quot;type&quot;: &quot;text&quot;,
            &quot;text&quot;: &quot;John Smith&quot;
          &#125;,
          &#123;
            &quot;type&quot;: &quot;text&quot;,
            &quot;text&quot;: &quot;order #1522&quot;
          &#125;
        ]
      &#125;
    ]
  &#125;
&#125;
```

[Learn more about sending template messages](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/overview)

**Note:** **Usernames and business-scoped user IDs:** When sending a call permission request template message, you can use the `recipient` field to identify the user by BSUID instead of a phone number; the user&#039;s phone number may be omitted. For details, see [Business-scoped user IDs](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-scoped-user-ids#business-scoped-user-id).

## Get current call permission state

Use this endpoint to get the call permission state for a business phone number with a single WhatsApp user. You can identify the user by their phone number (`user_wa_id`) or by their business-scoped user ID (`recipient`).

### Request syntax

```https
GET /&lt;PHONE_NUMBER_ID&gt;/call_permissions?user_wa_id=&lt;CONSUMER_WHATSAPP_ID&gt;
```

Or, using a BSUID:

```https
GET /&lt;PHONE_NUMBER_ID&gt;/call_permissions?recipient=&lt;BSUID&gt;
```

### Request parameters

| Parameter | Description | Sample Value |
| --- | --- | --- |
| `&lt;PHONE_NUMBER_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required**&lt;br&gt;&lt;br&gt;The business phone number you are fetching permissions against.&lt;br&gt;&lt;br&gt;[Learn more about formatting phone numbers in Cloud API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/whatsapp-business-account-phone-number-api) | `+18762639988` |
| `&lt;CONSUMER_WHATSAPP_ID&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | **Required** (unless `recipient` is provided)&lt;br&gt;&lt;br&gt;The phone number of the WhatsApp user who you are requesting call permissions from.&lt;br&gt;&lt;br&gt;[Learn more about formatting phone numbers in Cloud API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/whatsapp-business-account-phone-number-api) | `+13057765456` |
| `recipient`&lt;br&gt;&lt;br&gt;_String_ | **Optional**&lt;br&gt;&lt;br&gt;The business-scoped user ID (BSUID) or parent BSUID of the WhatsApp user. Use this instead of `user_wa_id`.&lt;br&gt;&lt;br&gt;[Learn more about business-scoped user IDs](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-scoped-user-ids#business-scoped-user-id) | `US.13491208655302741918` |

#### Response body

```https
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;permission&quot;: &#123;
    &quot;status&quot;: &quot;temporary&quot;,
    &quot;expiration_time&quot;: 1745343479
  &#125;,
  &quot;actions&quot;: [
    &#123;
      &quot;action_name&quot;: &quot;send_call_permission_request&quot;,
      &quot;can_perform_action&quot;: true,
      &quot;limits&quot;: [
        &#123;
          &quot;time_period&quot;: &quot;PT24H&quot;,
          &quot;max_allowed&quot;: 1,
          &quot;current_usage&quot;: 0,
        &#125;,
        &#123;
          &quot;time_period&quot;: &quot;P7D&quot;,
          &quot;max_allowed&quot;: 2,
          &quot;current_usage&quot;: 1,
        &#125;
      ]
    &#125;,
    &#123;
      &quot;action_name&quot;: &quot;start_call&quot;,
      &quot;can_perform_action&quot;: false,
      &quot;limits&quot;: [
        &#123;
          &quot;time_period&quot;: &quot;PT24H&quot;,
          &quot;max_allowed&quot;: 5,
          &quot;current_usage&quot;: 5,
          &quot;limit_expiration_time&quot;: 1745622600,
        &#125;
      ]
    &#125;
  ]
&#125;
```

#### Response parameters

| Parameter | Description |
| --- | --- |
| `permission`&lt;br&gt;&lt;br&gt;_JSON Object_ | The permission object contains two values:&lt;br&gt;&lt;br&gt;`status` _(String)_ — The current status of the permission.&lt;br&gt;&lt;br&gt;Can be either:&lt;br&gt;&lt;br&gt;* `&quot;no_permission&quot;`&lt;br&gt;* `&quot;temporary&quot;`&lt;br&gt;* `&quot;permanent&quot;`&lt;br&gt;&lt;br&gt;`expiration` _(Integer)_ — The Unix time at which the permission will expire in UTC timezone.&lt;br&gt;&lt;br&gt;If the permission is permanent, this field won&#039;t be present. |
| `actions`&lt;br&gt;&lt;br&gt;_JSON Object_ | A list of actions a business phone number may undertake to facilitate a call permission or a business initiated call.&lt;br&gt;&lt;br&gt;Current actions are:&lt;br&gt;&lt;br&gt;`send_call_permission_request`: Represents the action of sending new call permissions request messages to the WhatsApp user.&lt;br&gt;&lt;br&gt;`start_call`: Represents the action of establishing a new call with the WhatsApp user. Establishing a new call means that the call was successfully picked up by the WhatsApp user.&lt;br&gt;&lt;br&gt;For example, `send_call_permission_request` having a `can_perform_action` of `true` means that your business can send a call permission request to the WhatsApp user in question.&lt;br&gt;&lt;br&gt;`can_perform_action` (_Boolean_) —&lt;br&gt;&lt;br&gt;A flag indicating whether the action can be performed now, taking into account all limits. |
| `limits`&lt;br&gt;&lt;br&gt;_JSON Object_ | A list of time-bound restrictions for the given `action_name`.&lt;br&gt;&lt;br&gt;Each `action_name` has 1 or more restrictions depending on the timeframe.&lt;br&gt;&lt;br&gt;For example, a business can only send 2 permission requests in a 24-hour period.&lt;br&gt;&lt;br&gt;`limits` contains the following fields:&lt;br&gt;&lt;br&gt;`time_period` (_String_) — The span of time in which the limit applies, represented in the ISO 8601 format.&lt;br&gt;&lt;br&gt;`max_allowed` (_Integer_) — The maximum number of actions allowed within the specified time period.&lt;br&gt;&lt;br&gt;`current_usage` (_Integer_) — The current number of actions the business has taken within the specified time period.&lt;br&gt;&lt;br&gt;`limit_expiration_time` (_Integer_) — The Unix time at which the limit will expire in UTC timezone.&lt;br&gt;&lt;br&gt;If `current_usage` is under the max allowed for the limit, this field won&#039;t be present. |

#### Error response

Possible errors that can occur:

* Invalid `phone-number-id`
* If the WhatsApp user phone number is uncallable, the API returns `no_permission`.
* Permissions/Authorization errors.
* Rate limit reached. A maximum of 100 requests in a 1 second window can be made to the API.
* Calling is not enabled for the business phone number.

[View Calling API Error Codes and Troubleshooting for more information](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/troubleshooting)

[View general Cloud API Error Codes here](https://developers.facebook.com/documentation/business-messaging/whatsapp/support/error-codes)

**Note:** **Usernames and business-scoped user IDs:** When querying call permission state, you can use the `recipient` parameter to identify the user by BSUID instead of a phone number; the user&#039;s phone number may be omitted. For details, see [Business-scoped user IDs](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-scoped-user-ids#business-scoped-user-id).

## User call permission reply webhook

WhatsApp delivers this webhook whenever a user selects or updates their calling permissions. The webhook could be in response to a call permission request sent by the business, or the user could be proactively making a decision.

The webhook fields values change depending on the circumstances of the user permission decision:

* the user accepts or rejects the request
* the user approves permission by responding to a request or by calling the business
* the user permission is an automatic callback permission in response to a user-initiated call
* the user permission is automatically revoked in response to 4 consecutive unanswered business-initiated calls

Lastly, the user can grant permanent calling permission to the business, which is represented in the `is_permanent` parameter.

**Note:** No webhook is sent when a temporary permission expires. The `expiration_timestamp` field included in the accepted permission webhook indicates the time this permission will expire. Alternatively the current permission state can be queried from the [get current call permission state](#get-current-call-permission-state) endpoint.

#### Webhook sample

```https
&#123;
. . .

&quot;messages&quot;: [&#123;
    &quot;from&quot;: &quot;&#123;customer_phone_number&#125;&quot;,
    &quot;from_user_id&quot;: &quot;&lt;BSUID&gt;&quot;,
    &quot;from_parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot;,
    &quot;id&quot;: &quot;wamid.sH0kFlaCGg0xcvZbgmg90lHrg2dL&quot;,
    &quot;timestamp&quot;: &quot;&#123;timestamp&#125;&quot;,
    &quot;context&quot;: &#123;
          &quot;from&quot;: &quot;&#123;customer_phone_number&#125;&quot;,
          &quot;id&quot;: &quot;wamid.gBGGFlaCmZ9plHrf2Mh-o&quot;
    &#125;,
    &quot;interactive&quot;: &#123;
       &quot;type&quot;:  &quot;call_permission_reply&quot;,
        &quot;call_permission_reply&quot;: &#123;
            &quot;response&quot;:&quot;accept&quot;,
            &quot;is_permanent&quot;:false,
            &quot;expiration_timestamp&quot;: &quot;&#123;timestamp&#125;&quot;,
            &quot;response_source&quot;: &quot;user_action&quot;
       &#125;
    &#125;
 ],
. . .
&#125;
```

#### Webhook values

| Placeholder | Description |
| --- | --- |
| `customer_phone_number`&lt;br&gt;&lt;br&gt;_String_ | The phone number of the WhatsApp user. May be omitted if the user has adopted a username and the phone number cannot be included. |
| `from_user_id`&lt;br&gt;&lt;br&gt;_String_ | The [BSUID](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-scoped-user-ids#business-scoped-user-id) of the WhatsApp user. |
| `from_parent_user_id`&lt;br&gt;&lt;br&gt;_String_ | **Optional.** The [parent BSUID](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-scoped-user-ids#parent-business-scoped-user-ids) of the WhatsApp user. Only included if parent BSUIDs are enabled. |
| `context.id`&lt;br&gt;&lt;br&gt;_String_ | Can be either of two values&lt;br&gt;&lt;br&gt;* Message ID of the permission request message sent by the business to the WhatsApp user. Shows when a permission decision is made by the user in response to a call permission request.&lt;br&gt;* Call ID of the missed call placed by the business to the WhatsApp user. Shows when callback permission is enabled in settings and the user calls the business. |
| `response`&lt;br&gt;&lt;br&gt;_String_ | The WhatsApp user&#039;s response to the call permission request message&lt;br&gt;&lt;br&gt;Can be `accept` or `reject` |
| `is_permanent`&lt;br&gt;&lt;br&gt;_Boolean_ | Indicates if the permission is permanent or not. For temporary permission this will always be false. |
| `expiration_timestamp`&lt;br&gt;&lt;br&gt;_String_ | Time in seconds when this call permission expires if the WhatsApp user approved it |
| `response_source`&lt;br&gt;&lt;br&gt;_String_ | The source of this permission&lt;br&gt;&lt;br&gt;Possible values for accepted call permissions are:&lt;br&gt;&lt;br&gt;* `user_action`: User approved or rejected the permission&lt;br&gt;* `automatic`: An automatic permission approval due to the WhatsApp user initiating the call |

**Note:** **Usernames and business-scoped user IDs:** Call permission reply webhooks may include `from_user_id` and `from_parent_user_id` to identify the user by BSUID; the user&#039;s phone number may be omitted. For details, see [Business-scoped user IDs](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-scoped-user-ids#business-scoped-user-id).

#### Webhook sample scenarios

| Scenario | Webhook sample |
| --- | --- |
| The WhatsApp user approves a temporary call permission from a call permission request message | ```https
&#123;
. . .

&quot;messages&quot;: [&#123;
    &quot;from&quot;: &quot;&#123;customer_phone_number&#125;&quot;,
    &quot;from_user_id&quot;: &quot;&lt;BSUID&gt;&quot;,
    &quot;from_parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot;,
    &quot;id&quot;: &quot;wamid.sH0kFlaCGg0xcvZbgmg90lHrg2dL&quot;,
    &quot;timestamp&quot;: &quot;1767168000&quot;,
    &quot;context&quot;: &#123;
          &quot;from&quot;: &quot;&#123;customer_phone_number&#125;&quot;,
          &quot;id&quot;: &quot;wamid.gBGGFlaCmZ9plHrf2Mh-o&quot;
    &#125;,
    &quot;interactive&quot;: &#123;
       &quot;type&quot;:  &quot;call_permission_reply&quot;,
        &quot;call_permission_reply&quot;: &#123;
            &quot;response&quot;:&quot;accept&quot;,
            &quot;is_permanent&quot;:false,
            &quot;expiration_timestamp&quot;: &quot;1768550400&quot;,
            &quot;response_source&quot;: &quot;user_action&quot;
       &#125;
    &#125;
 ],
. . .
&#125;
``` |
| The WhatsApp user approves a permanent call permission from a call permission request message | ```https
&#123;
. . .

&quot;messages&quot;: [&#123;
    &quot;from&quot;: &quot;&#123;customer_phone_number&#125;&quot;,
    &quot;from_user_id&quot;: &quot;&lt;BSUID&gt;&quot;,
    &quot;from_parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot;,
    &quot;id&quot;: &quot;wamid.sH0kFlaCGg0xcvZbgmg90lHrg2dL&quot;,
    &quot;timestamp&quot;: &quot;1767168000&quot;,
    &quot;context&quot;: &#123;
          &quot;from&quot;: &quot;&#123;customer_phone_number&#125;&quot;,
          &quot;id&quot;: &quot;wamid.gBGGFlaCmZ9plHrf2Mh-o&quot;
    &#125;,
    &quot;interactive&quot;: &#123;
       &quot;type&quot;:  &quot;call_permission_reply&quot;,
        &quot;call_permission_reply&quot;: &#123;
            &quot;response&quot;:&quot;accept&quot;,
            &quot;is_permanent&quot;:true,
            &quot;response_source&quot;: &quot;user_action&quot;
       &#125;
    &#125;
 ],
. . .
&#125;
``` |
| The WhatsApp user approves a permanent call permission from the business profile | ```https
&#123;
. . .

&quot;messages&quot;: [&#123;
    &quot;from&quot;: &quot;&#123;customer_phone_number&#125;&quot;,
    &quot;from_user_id&quot;: &quot;&lt;BSUID&gt;&quot;,
    &quot;from_parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot;,
    &quot;id&quot;: &quot;wamid.sH0kFlaCGg0xcvZbgmg90lHrg2dL&quot;,
    &quot;timestamp&quot;: &quot;1767168000&quot;,
    &quot;interactive&quot;: &#123;
       &quot;type&quot;:  &quot;call_permission_reply&quot;,
        &quot;call_permission_reply&quot;: &#123;
            &quot;response&quot;:&quot;accept&quot;,
            &quot;is_permanent&quot;:true,
            &quot;response_source&quot;: &quot;user_action&quot;
       &#125;
    &#125;
 ],
. . .
&#125;
``` |
| The WhatsApp user rejects a call permission after receiving a call permission request message | ```https
&#123;
. . .

&quot;messages&quot;: [&#123;
    &quot;from&quot;: &quot;&#123;customer_phone_number&#125;&quot;,
    &quot;from_user_id&quot;: &quot;&lt;BSUID&gt;&quot;,
    &quot;from_parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot;,
    &quot;id&quot;: &quot;wamid.sH0kFlaCGg0xcvZbgmg90lHrg2dL&quot;,
    &quot;timestamp&quot;: &quot;1767168000&quot;,
    &quot;context&quot;: &#123;
          &quot;from&quot;: &quot;&#123;customer_phone_number&#125;&quot;,
          &quot;id&quot;: &quot;wamid.gBGGFlaCmZ9plHrf2Mh-o&quot;
    &#125;,
    &quot;interactive&quot;: &#123;
       &quot;type&quot;:  &quot;call_permission_reply&quot;,
        &quot;call_permission_reply&quot;: &#123;
            &quot;response&quot;:&quot;reject&quot;,
            &quot;response_source&quot;: &quot;user_action&quot;
       &#125;
    &#125;
 ],
. . .
&#125;
``` |
| An automatic temporary callback permission is granted to the business when the WhatsApp user calls the business | ```https
&#123;
. . .

&quot;messages&quot;: [&#123;
    &quot;from&quot;: &quot;&#123;customer_phone_number&#125;&quot;,
    &quot;from_user_id&quot;: &quot;&lt;BSUID&gt;&quot;,
    &quot;from_parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot;,
    &quot;id&quot;: &quot;wamid.sH0kFlaCGg0xcvZbgmg90lHrg2dL&quot;,
    &quot;timestamp&quot;: &quot;1767168000&quot;,
    &quot;context&quot;: &#123;
          &quot;from&quot;: &quot;&#123;customer_phone_number&#125;&quot;,
          &quot;id&quot;: &quot;wacid.gBGGF4lasdnlasdHrf2Mh-o&quot;
    &#125;,
    &quot;interactive&quot;: &#123;
       &quot;type&quot;:  &quot;call_permission_reply&quot;,
        &quot;call_permission_reply&quot;: &#123;
            &quot;response&quot;:&quot;accept&quot;,
            &quot;is_permanent&quot;:false,
            &quot;expiration_timestamp&quot;: &quot;1768550400&quot;,
            &quot;response_source&quot;: &quot;automatic&quot;
       &#125;
    &#125;
 ],
. . .
&#125;
``` |
| A call permission is automatically revoked when a business makes 4 consecutive unanswered calls to the WhatsApp user | ```https
&#123;
. . .

&quot;messages&quot;: [&#123;
    &quot;from&quot;: &quot;&#123;customer_phone_number&#125;&quot;,
    &quot;from_user_id&quot;: &quot;&lt;BSUID&gt;&quot;,
    &quot;from_parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot;,
    &quot;id&quot;: &quot;wamid.sH0kFlaCGg0xcvZbgmg90lHrg2dL&quot;,
    &quot;timestamp&quot;: &quot;1767168000&quot;,
    &quot;interactive&quot;: &#123;
       &quot;type&quot;:  &quot;call_permission_reply&quot;,
        &quot;call_permission_reply&quot;: &#123;
            &quot;response&quot;:&quot;reject&quot;,
            &quot;response_source&quot;: &quot;automatic&quot;
       &#125;
    &#125;
 ],
. . .
&#125;
``` |

