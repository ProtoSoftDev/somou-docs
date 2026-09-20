# Group management



## Overview

The Groups API gives you simple functions to control groups through their lifecycle.

When you create a new group, an invite link is created for inviting participants to the group.

Since you cannot manually add participants to the group, simply send a message with your invite link to WhatsApp users who you would like to join the group.

## Group management features

* [Create and delete group](#create-group)
* [Groups with join requests enabled](#groups-with-join-requests)
* [Get and reset group invite link](#get-and-reset-group-invite-link)
* [Send group invite link template message](#send-group-invite-link-template-message)
* [Remove group participants](#remove-group-participants)
* [Get group info](#get-group-info)
* [Get active groups](#get-active-groups)
* [Update group settings](#update-group-settings)

To learn how to message groups, view the [Group Messaging reference](https://developers.facebook.com/documentation/business-messaging/whatsapp/groups/groups-messaging).

## Subscribe to groups metadata webhooks

In order to receive webhook notifications for metadata about your groups, please subscribe to the following webhook fields:

* `group_lifecycle_update`
* `group_participants_update`
* `group_settings_update`
* `group_status_update`

**Warning:** For a full reference of webhooks for the Groups API, please visit our [Webhooks for Groups API reference](https://developers.facebook.com/documentation/business-messaging/whatsapp/groups/webhooks).

## Create group

Use this endpoint to create a new group and generate a group invite link.

Once the group is created, you will receive a webhook with an `invite_link` parameter that contains an invite link for the group. You can send this invite link to WhatsApp users interested in joining the group.

Optionally, you can create a group that requires join approval. This means that if a WhatsApp user wants to join your group, you can approve or reject their request.

[Learn more about groups with join requests enabled](#groups-with-join-requests).

### Request syntax

Create a group with an initial group invite link:

`POST /&lt;BUSINESS_PHONE_NUMBER_ID&gt;/groups`

### Request body

```curl
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;subject&quot;: &quot;&lt;GROUP_SUBJECT&gt;&quot;,
  &quot;description&quot;: &quot;&lt;GROUP_DESCRIPTION&gt;&quot;,
  &quot;join_approval_mode&quot;: &quot;&lt;JOIN_APPROVAL_MODE&gt;&quot;
&#125;
```

### Request parameters

| Placeholder | Description | Sample Value |
| --- | --- | --- |
| `&lt;BUSINESS_PHONE_NUMBER_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required**&lt;br&gt;&lt;br&gt;Business phone number ID. | `12784358810` |
| `&lt;GROUP_SUBJECT&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required**&lt;br&gt;&lt;br&gt;Group subject.&lt;br&gt;&lt;br&gt;Maximum 128 characters. Whitespace is trimmed. | `New Purchase Inquiry` |
| `&lt;GROUP_DESCRIPTION&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional**&lt;br&gt;&lt;br&gt;Group description.&lt;br&gt;&lt;br&gt;Maximum 2048 characters. | `Jim, an existing client, would like to learn about new car purchase options for current year models.` |
| `&lt;JOIN_APPROVAL_MODE&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional**&lt;br&gt;&lt;br&gt;Indicates if WhatsApp users who click the invitation link can join the group with or without being approved first.&lt;br&gt;&lt;br&gt;Values can be:&lt;br&gt;&lt;br&gt;- `approval_required` — Indicates WhatsApp users must be approved via [join request](#groups-with-join-requests) before they can access the group.&lt;br&gt;- `auto_approve` — Indicates WhatsApp users can join the group without approval.&lt;br&gt;&lt;br&gt;If omitted, `join_approval_mode` is set to `auto_approve` by default. | `auto_approve` |

### Webhooks

A `group_lifecycle_update` webhook is triggered.

#### Group create succeed

[View the &quot;Group create succeed&quot; sample webhook](https://developers.facebook.com/documentation/business-messaging/whatsapp/groups/webhooks#group-create-succeed)

#### Group create fail

[View the &quot;Group create fail&quot; sample webhook](https://developers.facebook.com/documentation/business-messaging/whatsapp/groups/webhooks#group-create-fail)

#### User joins group using invite link

[View the &quot;User joins group using invite link&quot; sample webhook](https://developers.facebook.com/documentation/business-messaging/whatsapp/groups/webhooks#user-joined-group-using-invite-link-succeed)

## Groups with join requests

You can create groups that require join request approval. Once enabled, WhatsApp users who click the group invitation link can submit a request to join the group, or cancel a prior request:

When a WhatsApp user joins the group using a join request, a [`group_participants_update` webhook for a user accepting the join request](https://developers.facebook.com/documentation/business-messaging/whatsapp/groups/webhooks#user-accepts-or-cancels-join-request) is triggered. You can also [get a list of open join requests via API](#get-join-requests). Use the contents of the webhook or API response to approve or reject requests.

### Get join requests

#### Request syntax

`GET /&lt;GROUP_ID&gt;/join_requests`

#### Request parameters

| Placeholder | Description | Sample Value |
| --- | --- | --- |
| `&lt;GROUP_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Group ID. | `Y2FwaV9ncm91cDoxNzA1NTU1MDEzOToxMjAzNjM0MDQ2OTQyMzM4MjAZD` |

#### Response syntax

Upon success:

```curl
&#123;
  &quot;data&quot;: [
    &#123;
      &quot;join_request_id&quot;: &quot;&lt;JOIN_REQUEST_ID&gt;&quot;,
      &quot;wa_id&quot;: &quot;&lt;WHATSAPP_USER_ID&gt;&quot;,
      &quot;creation_timestamp&quot;: &quot;&lt;JOIN_REQUEST_CREATION_TIMESTAMP&quot;&gt;
    &#125;,
    //Additional join request objects would follow, if any
  ],
  &quot;paging&quot;: &#123;
    &quot;cursors&quot;: &#123;
      &quot;before&quot;: &quot;&lt;BEFORE_CURSOR&gt;&quot;,
      &quot;after&quot;: &quot;&lt;AFTER_CURSOR&gt;&quot;
    &#125;
  &#125;
&#125;
```

#### Response parameters

| Placeholder | Description | Sample Value |
| --- | --- | --- |
| `&lt;JOIN_REQUEST_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | Join request ID. | `MTY0NjcwNDM1OTU6MTIwMzYzNDA0Njk0MjMzODIw` |
| `&lt;WHATSAPP_USER_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | WhatsApp user ID. | `16505551234` |
| `&lt;JOIN_REQUEST_CREATION_TIMESTAMP&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | Unix timestamp indicating when the join request was created. | `1755548877` |
| `&lt;BEFORE_CURSOR&gt;`&lt;br&gt;&lt;br&gt;_String_ | Before cursor. See [Paginated Results](https://developers.facebook.com/docs/graph-api/results). | `eyJvZAmZAzZAXQiOjAsInZAlcnNpb25JZACI6IjE3NTU1NTM3MDUxNzUwNTQ1MTAifQZDZD` |
| `&lt;AFTER_CURSOR&gt;`&lt;br&gt;&lt;br&gt;_String_ | After cursor. See [Paginated Results](https://developers.facebook.com/docs/graph-api/results). | `eyJvZAmZAzZAXQiOjAsInZAlcnNpb25JZACI6IjE3NTU1NTM3MDUxNzUwNTQ1MTAifQZDZD` |

### Approve join requests

#### Request syntax

`POST /&lt;GROUP_ID&gt;/join_requests`

#### Request body

```curl
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;join_requests&quot;: [
    &quot;&lt;JOIN_REQUEST_ID&gt;&quot;,
    // Additional join request IDs would go here, if approving in bulk
  ]
&#125;
```

#### Request parameters

| Placeholder | Description | Sample Value |
| --- | --- | --- |
| `&lt;GROUP_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Group ID. | `Y2FwaV9ncm91cDoxNzA1NTU1MDEzOToxMjAzNjM0MDQ2OTQyMzM4MjAZD` |

#### Response syntax

Upon success, the API will respond with the following JSON payload, and WhatsApp users whose join requests were approved will be able to access the group when tapping the invite link.

```curl
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;approved_join_requests&quot;: [
    &quot;&lt;JOIN_REQUEST_ID&gt;&quot;,
    // Additional join request IDs would go here, it approved in bulk
  ],

  //Only included if unable to approve one or more join requests

  &quot;failed_join_requests&quot;: [
    &#123;
      &quot;join_request_id&quot;: &quot;&lt;JOIN_REQUEST_ID&gt;&quot;,
      &quot;errors&quot;: [
        &#123;
          &quot;code&quot;: &quot;&lt;ERROR_CODE&gt;&quot;,
          &quot;message&quot;: &quot;&lt;ERROR_MESSAGE&gt;&quot;,
          &quot;title&quot;: &quot;&lt;ERROR_TITLE&gt;&quot;,
          &quot;error_data&quot;: &#123;
            &quot;details&quot;: &quot;&lt;ERROR_DETAILS&gt;&quot;
          &#125;
        &#125;
      ]
    &#125;
  ],
  &quot;errors&quot;: [
    &#123;
      &quot;code&quot;: &quot;&lt;ERROR_CODE&gt;&quot;,
      &quot;message&quot;: &quot;&lt;ERROR_MESSAGE&gt;&quot;,
      &quot;title&quot;: &quot;&lt;ERROR_TITLE&gt;&quot;,
      &quot;error_data&quot;: &#123;
        &quot;details&quot;: &quot;&lt;ERROR_DETAILS&gt;&quot;
      &#125;
    &#125;
  ]
&#125;
```

#### Response parameters

| Placeholder | Description | Sample Value |
| --- | --- | --- |
| `&lt;JOIN_REQUEST_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | ID of approved join request, or ID of failed join request, if the request could not be approved. | `MTY0NjcwNDM1OTU6MTIwMzYzNDA0Njk0MjMzODIw` |
| `&lt;ERROR_CODE&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | Error code, if unable to approve. | `131203` |
| `&lt;ERROR_MESSAGE&gt;`&lt;br&gt;&lt;br&gt;_String_ | Error message, if unable to approve. | `(#131203) Recipient has not accepted our new Terms of Service and Privacy Policy.` |
| `&lt;ERROR_TITLE&gt;`&lt;br&gt;&lt;br&gt;_String_ | Error title, if unable to approve. | `Unable to add participant to group` |
| `&lt;ERROR_DETAILS&gt;`&lt;br&gt;&lt;br&gt;_String_ | Error details, if unable to approve. | `Recipient has not accepted our new Terms of Service and Privacy Policy.` |

#### Webhook

A `group_participants_update` webhook is triggered.

[View the &quot;User accepts join request&quot; sample webhook](https://developers.facebook.com/documentation/business-messaging/whatsapp/groups/webhooks#user-accepts-or-cancels-join-request)

### Reject join requests

#### Request syntax

`DELETE /&lt;GROUP_ID&gt;/join_requests`

#### Request body

```curl
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;join_requests&quot;: [
    &quot;&lt;JOIN_REQUEST_ID&gt;&quot;,
    //Additional join request IDs would go here, it rejecting in bulk
  ]
&#125;
```

#### Request parameters

| Placeholder | Description | Sample Value |
| --- | --- | --- |
| `&lt;GROUP_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Group ID. | `Y2FwaV9ncm91cDoxNzA1NTU1MDEzOToxMjAzNjM0MDQ2OTQyMzM4MjAZD` |
| `&lt;JOIN_REQUEST_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;ID of join request to reject. | `MTY0NjcwNDM1OTU6MTIwMzYzNDA0Njk0MjMzODIw` |

#### Response syntax

Upon success, the API will respond with the following JSON payload, and the WhatsApp user will see the **Request to join** button again when accessing the group invite link.

```curl
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;rejected_join_requests&quot;: [
    &quot;&lt;JOIN_REQUEST_ID&gt;&quot;,
    //Additional join request IDs would go here, it rejecting in bulk
  ],

  //Only included if unable to reject one or more join requests
  &quot;failed_join_requests&quot;: [
    &#123;
      &quot;join_request_id&quot;: &quot;&lt;JOIN_REQUEST_ID&gt;&quot;,
      &quot;errors&quot;: [
        &#123;
          &quot;code&quot;: &quot;&lt;ERROR_CODE&gt;&quot;,
          &quot;message&quot;: &quot;&lt;ERROR_MESSAGE&gt;&quot;,
          &quot;title&quot;: &quot;&lt;ERROR_TITLE&gt;&quot;,
          &quot;error_data&quot;: &#123;
            &quot;details&quot;: &quot;&lt;ERROR_DETAILS&gt;&quot;
          &#125;
        &#125;
      ]
    &#125;
  ],
  &quot;errors&quot;: [
    &#123;
      &quot;code&quot;: &quot;&lt;ERROR_CODE&gt;&quot;,
      &quot;message&quot;: &quot;&lt;ERROR_MESSAGE&gt;&quot;,
      &quot;title&quot;: &quot;&lt;ERROR_TITLE&gt;&quot;,
      &quot;error_data&quot;: &#123;
        &quot;details&quot;: &quot;&lt;ERROR_DETAILS&gt;&quot;
      &#125;
    &#125;
  ]
&#125;
```

#### Response parameters

| Placeholder | Description | Sample Value |
| --- | --- | --- |
| `&lt;JOIN_REQUEST_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | ID of rejected join request, or ID of failed join request, if the request could not be rejected. | `MTY0NjcwNDM1OTU6MTIwMzYzNDA0Njk0MjMzODIw` |
| `&lt;ERROR_CODE&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | Error code, if unable to reject. | `131203` |
| `&lt;ERROR_MESSAGE&gt;`&lt;br&gt;&lt;br&gt;_String_ | Error message, if unable to reject. | `(#131203) Recipient has not accepted our new Terms of Service and Privacy Policy.` |
| `&lt;ERROR_TITLE&gt;`&lt;br&gt;&lt;br&gt;_String_ | Error title, if unable to reject. | `Unable to add participant to group` |
| `&lt;ERROR_DETAILS&gt;`&lt;br&gt;&lt;br&gt;_String_ | Error details, if unable to reject. | `Recipient has not accepted our new Terms of Service and Privacy Policy.` |

### Webhook

None.

## Get and reset group invite link

**Warning:** Once an invite link is reset, all previous invite links will become invalid.

An invite link for the group is generated when the group is created. Use these endpoints to get and reset group invite links.

For each endpoint, you will need your group ID in order to get or reset a link for the correct group as follows:

| Placeholder | Description | Sample Value |
| --- | --- | --- |
| `&lt;GROUP_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required**&lt;br&gt;&lt;br&gt;The ID of the group you want to get or reset an invite link for. | `Y2FwaV9ncm91cDoxOTUwNTU1MDA3OToxMjAzNjMzOTQzMjAdOTY0MTUZD` |

### Get group invite link

#### Request syntax

`GET /&lt;GROUP_ID&gt;/invite_link`

#### Response body

```curl
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;invite_link&quot;: &quot;https://chat.whatsapp.com/&lt;LINK_ID&gt;&quot;
&#125;
```

Note that `invite_link` always begins with the prefix `https://chat.whatsapp.com/`. The only variable portion is `&lt;LINK_ID&gt;`.

### Reset group invite link

#### Request syntax

`POST /&lt;GROUP_ID&gt;/invite_link`

#### Request body

```curl
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
&#125;
```

#### Response body

```curl
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;invite_link&quot;: &quot;https://chat.whatsapp.com/&lt;LINK_ID&gt;&quot;
&#125;
```

## Send group invite link template message

[Template Library](https://business.facebook.com/wa/manage/template-library) contains a utility message template for sending group invite links to WhatsApp users. Use these pre-defined templates to send group invitations as utility messages.

**Warning:** **In order to keep the template priced as `utility`, you cannot modify it when you copy it from template library to your WABA.**

To send the template message:

#### Step 1. Add a group invite link template in Template Library to your account templates:

_In WhatsApp Manager_

1. Navigate to [Template Library](https://business.facebook.com/wa/manage/template-library)
1. On the left, click the **Group invite link** dropdown, then click the **Group invite upon request** checkbox.
1. Select the template you want to use, give it a name, and click **Submit**.

_Via the API_

You can query template libraries applicable to group invite links using the request below:

`GET /message_template_library?category=utility&amp;topic=group_invite_link&amp;language=en`

[Read more about finding and adding the template to your WABA via the API](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/template-library#using-the-api)

**Note:** Template approval may require up to 24 hours. You&#039;ll be able to send messages with this template after its approval.

#### Step 2. Send the template message
1. Send the template using the request syntax and body below, substituting your group ID, the name you gave your template, and other applicable values.

When you provide the group ID in the API request, it will be automatically translated into the corresponding group invite link upon message delivery.

### Request syntax

`POST /&lt;BUSINESS_PHONE_NUMBER_ID&gt;/messages`

### Endpoint parameters
| Placeholder | Description | Sample Value |
| --- | --- | --- |
| `&lt;BUSINESS_PHONE_NUMBER_ID&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | **Required**&lt;br&gt;&lt;br&gt;Business phone number ID. | `13057863445` |

### Request body

```curl
curl --location &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;BUSINESS_PHONE_NUMBER_ID&gt;/messages?access_token=&#039; \
      --header &#039;Content-Type: application/json&#039; \
      --data &#039;&#123;
        &quot;messaging_product&quot;: &quot;whatsapp&quot;,
        &quot;to&quot;: &quot;&lt;WHATSAPP_USER_PHONE_NUMBER&gt;&quot;,
        &quot;type&quot;: &quot;template&quot;,
        &quot;template&quot;: &#123;
          &quot;name&quot;: &quot;&lt;TEMPLATE_NAME&gt;&quot;,
          &quot;language&quot;: &#123;
            &quot;code&quot;: &quot;&lt;TEMPLATE_LANGUAGE&gt;&quot;
          &#125;,
          &quot;components&quot;: [
            &#123;
              &quot;type&quot;: &quot;body&quot;,
              &quot;parameters&quot;: [
                &#123;
                  &quot;type&quot;: &quot;group_id&quot;,
                  &quot;group_id&quot;: &quot;&lt;GROUP_ID&gt;&quot;
                &#125;,
                &#123;
                  ...additional parameters
                &#125;
              ]
            &#125;
          ]
        &#125;
      &#125;&#039;
```

[Learn more about Template Library](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/template-library)

### Webhooks

#### User joins group using invite link

[View the &quot;User joins group using invite link&quot; sample webhook](https://developers.facebook.com/documentation/business-messaging/whatsapp/groups/webhooks#user-joined-group-using-invite-link-succeed)

## Delete group

This endpoint deletes the group and removes all participants, including the business. No request body is required.

### Request syntax

`DELETE /&lt;GROUP_ID&gt;`

### Request properties
| Placeholder | Description | Sample Value |
| --- | --- | --- |
| `&lt;GROUP_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required**&lt;br&gt;&lt;br&gt;The ID of the group you want to delete. | `Y2FwaV9ncm91cDoxOTUwNTU1MDA3OToxMjAzNjMzOTQzMjAdOTY0MTUZD` |

### Webhooks

A `group_lifecycle_update` webhook is triggered.

#### Delete group succeed

[View the &quot;Delete group succeed&quot; sample webhook](https://developers.facebook.com/documentation/business-messaging/whatsapp/groups/webhooks#delete-group-succeed)

#### Delete group fails

[View the &quot;Delete group fails&quot; sample webhook](https://developers.facebook.com/documentation/business-messaging/whatsapp/groups/webhooks#delete-group-fails)

## Remove group participants

Use this endpoint to remove participants from the group.

**Note: If a participant is removed from a group, they can no longer join the group via an invite link.**

### Request syntax

`DELETE /&lt;GROUP_ID&gt;/participants`

### Request body

```curl
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;participants&quot;: [
    &#123; &quot;user&quot;: &quot;&lt;WHATSAPP_USER_PHONE_NUMBER&gt; or &lt;WHATSAPP_USER_ID&gt;&quot; &#125;,
    &#123; &quot;user&quot;: &quot;&lt;WHATSAPP_USER_PHONE_NUMBER&gt; or &lt;WHATSAPP_USER_ID&gt;&quot;&quot; &#125;,
    ...
  ]
&#125;
```

### Request properties

| Placeholder | Description | Sample Value |
| --- | --- | --- |
| `&quot;participants&quot;: []`&lt;br&gt;&lt;br&gt;_Array_ | **Optional**&lt;br&gt;&lt;br&gt;Specifies an array of phone numbers or WhatsApp IDs of WhatsApp accounts. The business phone number used to create the group is always added to the group as the creator and admin.&lt;br&gt;&lt;br&gt;- Maximum 8 participants.&lt;br&gt;- The array cannot be empty. | ```
&#123; &quot;user&quot;: &quot;+17865347866&quot; &#125;,
&#123; &quot;user&quot;: &quot;+7669992245&quot; &#125;,
...
``` |

### Webhooks

A `group_participants_update` webhook is triggered.

#### Group participant leaves

[View the &quot;Group participant leaves&quot; sample webhook](https://developers.facebook.com/documentation/business-messaging/whatsapp/groups/webhooks#delete-group-succeed)

## Get group info

Use this endpoint to retrieve metadata about a single group.

**Note:** Specifying no fields in the query parameters will just return the group ID and messaging product.

### Request syntax

`GET /&lt;GROUP_ID&gt;?fields=&lt;FIELDS&gt;`

### Endpoint parameters

| Placeholder | Description | Sample Value |
| --- | --- | --- |
| `&lt;GROUP_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required**&lt;br&gt;&lt;br&gt;The ID of the group you are querying info from. | `Y2FwaV9ncm91cDoxOTUwNTU1MDA3OToxMjAzNjMzOTQzMjAdOTY0MTUZD` |
| `&lt;FIELDS&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional**&lt;br&gt;&lt;br&gt;A comma-separated list of fields to return. If no fields are passed in, only the group ID is returned. | `&quot;subject,description,participants,join_approval_mode&quot;`&lt;br&gt;&lt;br&gt;[Learn more about Graph API fields here](https://developers.facebook.com/docs/graph-api/overview#fields) |

### Available fields

| Field | Description | Sample Return Value |
| --- | --- | --- |
| `join_approval_mode`&lt;br&gt;&lt;br&gt;_String_ | Indicates if WhatsApp users who click the invitation link can join the group with or without being approved first.&lt;br&gt;&lt;br&gt;Values can be:&lt;br&gt;&lt;br&gt;- `approval_required` — Indicates WhatsApp users must be approved via [join request](#groups-with-join-requests) before they can access the group.&lt;br&gt;- `auto_approve` — Indicates WhatsApp users can join the group without approval. | `auto_approve` |
| `subject`&lt;br&gt;&lt;br&gt;_String_ | The subject for the group. | `&quot;Artificial Intelligence Insights&quot;` |
| `description`&lt;br&gt;&lt;br&gt;_String_ | The group description, if set during creation time. | `&quot;Explore AI developments, share knowledge, and discuss the future of artificial intelligence with fellow enthusiasts and experts.&quot;` |
| `suspended`&lt;br&gt;&lt;br&gt;_Boolean_ | Returns `true` if the group has been suspended by WhatsApp. | `false` |
| `creation_timestamp`&lt;br&gt;&lt;br&gt;_Integer_ | UNIX timestamp in seconds at which the group was created. | `683731200` |
| `participants`&lt;br&gt;&lt;br&gt;_List_ | A list of objects `&#123;&quot;wa_id&quot;: &quot;&lt;WA_ID&gt;&quot;&#125;`, where `&lt;WA_ID&gt;` is a participant in the group being queried. | `[&#123;&quot;wa_id&quot;: &quot;2228675309&quot;&#125;, &#123;&quot;wa_id&quot;: &quot;7693349922&quot;&#125;]` |
| `total_participant_count`&lt;br&gt;&lt;br&gt;_Integer_ | The total number of participants in the group, excluding your business. | `6` |

### Sample response

```curl
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;id&quot;: &quot;&lt;GROUP_ID&gt;&quot;,
  &quot;subject&quot;: &quot;&lt;SUBJECT&gt;&quot;,
  &quot;creation_timestamp&quot;: &quot;&lt;TIMESTAMP&gt;&quot;,
  &quot;suspended&quot;: &quot;&lt;SUSPENDED&gt;&quot;,
  &quot;description&quot;: &quot;&lt;DESCRIPTION&gt;&quot;,
  &quot;total_participant_count&quot;: &quot;&lt;TOTAL_PARTICIPANT_COUNT&gt;&quot;,
  &quot;participants&quot;: [
    &#123;
      &quot;wa_id&quot;: &quot;&lt;WA_ID&gt;&quot;
    &#125;,
    &#123;
      &quot;wa_id&quot;: &quot;&lt;WA_ID&gt;&quot;
    &#125;
  ],
  &quot;join_approval_mode&quot;: &quot;&lt;JOIN_APPROVAL_MODE&gt;&quot;
&#125;
```

## Get active groups

Use this endpoint to retrieve a list of active groups for a given business phone number.

### Request syntax

`GET /&lt;BUSINESS_PHONE_NUMBER_ID&gt;/groups`

### Query parameters

```curl
?limit=&lt;LIMIT&gt;, // Optional
&amp;after=&lt;AFTER_CURSOR&gt;, // Optional
&amp;before=&lt;BEFORE_CURSOR&gt; // Optional
```

| Parameter | Description |
| --- | --- |
| `&lt;LIMIT&gt;`&lt;br&gt;&lt;br&gt;_Optional_ | Number of groups to fetch in the request.&lt;br&gt;&lt;br&gt;Min: 1 \| Default: 25 \| Max: 1024 |
| `&lt;BEFORE_CURSOR&gt;`&lt;br&gt;&lt;br&gt;_Optional_ | Cursor that points to the beginning of a page of data. Learn more about [Paginated Results in Graph API here](https://developers.facebook.com/docs/graph-api/results) |
| `&lt;AFTER_CURSOR&gt;`&lt;br&gt;&lt;br&gt;_Optional_ | Cursor that points to the end of a page of data. Learn more about [Paginated Results in Graph API here](https://developers.facebook.com/docs/graph-api/results) |

### Response object

```curl
&#123;
  &quot;data&quot;: &#123;
    &quot;groups&quot;: [
      &#123;&quot;id&quot;: &quot;GROUP_ID&quot;, &quot;subject&quot;: SUBJECT, &quot;created_at&quot;: &quot;TIMESTAMP&quot;&#125;,
      &#123;&quot;id&quot;: &quot;GROUP_ID&quot;, &quot;subject&quot;: SUBJECT, &quot;created_at&quot;: &quot;TIMESTAMP&quot;&#125;
      …
    ]
  &#125;,
  &quot;paging&quot;: &#123;
    &quot;cursors&quot;: &#123;
      &quot;after&quot;: &quot;MTAxNTExOTQ1MjAwNzI5NDE=&quot;,
      &quot;before&quot;: &quot;NDMyNzQyODI3OTQw&quot;
    &#125;,
    &quot;previous&quot;: &quot;https://graph.facebook.com/VERSION/PHONE_NUMBER_ID/groups?limit=10&amp;before=NDMyNzQyODI3OTQw&quot;,
    &quot;next&quot;: &quot;https://graph.facebook.com/VERSION/PHONE_NUMBER_ID/groups?limit=25&amp;after=MTAxNTExOTQ1MjAwNzI5NDE=&quot;
  &#125;
&#125;
```

### Response parameters

| Parameter | Description |
| --- | --- |
| `data[groups]`&lt;br&gt;&lt;br&gt;_List_ | A list of groups, each containing the group ID, group subject, and UNIX timestamp for group creation. |
| `paging`&lt;br&gt;&lt;br&gt;_Object_ | A pagination object.&lt;br&gt;&lt;br&gt;Learn more about [Paginated Results in Graph API here](https://developers.facebook.com/docs/graph-api/results) |

## Update group settings

Use this webhook to update your group&#039;s subject, description, and photo.

### Request syntax

`POST /&lt;GROUP_ID&gt;`

### Request body

```curl
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;subject&quot;: &quot;&lt;GROUP_SUBJECT&gt;&quot;,
  &quot;profile_picture_file&quot;: &quot;&lt;FILE_PATH&gt;&quot;,
  &quot;description&quot;: &quot;&lt;GROUP_DESCRIPTION&gt;&quot;
&#125;
```

### Request properties

**Note**

| Placeholder | Description | Sample Value |
| --- | --- | --- |
| `&lt;FILE_PATH&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional**&lt;br&gt;&lt;br&gt;A path to an image file stored in your local directory.&lt;br&gt;&lt;br&gt;**To upload a file**: Follow the same request structure as the [Upload Media](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-phone-numbers/media#upload-media) endpoint.&lt;br&gt;&lt;br&gt;Sample file upload cURL:&lt;br&gt;&lt;br&gt;```
curl &#039;https://graph.facebook.com/v23.0/&lt;GROUP_ID&gt; \
          -X POST \
          -H &#039;Authorization: Bearer ...&#039; \
          -F &#039;messaging_product=whatsapp&#039; \
          -F &#039;file=&#064;/media/pictures/square_pic.png&#039;
```&lt;br&gt;&lt;br&gt;Group profile picture requirement:&lt;br&gt;&lt;br&gt;* Only support mime type image/jpeg&lt;br&gt;* Maximum size: 5MB&lt;br&gt;* Image should be in square, that is, height = width.&lt;br&gt;* Minimum size: 192 x 192 | `/local/path/file.jpg` |
| `&lt;GROUP_SUBJECT&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional**&lt;br&gt;&lt;br&gt;The new subject for the group.&lt;br&gt;&lt;br&gt;- Maximum length: 128 characters.&lt;br&gt;- Must not be empty if provided. | `&quot;Watch Enthusiasts&quot;` |
| `&lt;GROUP_DESCRIPTION&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional**&lt;br&gt;&lt;br&gt;The new description for the group.&lt;br&gt;&lt;br&gt;- Max length: 2048 characters | `&quot;Join our community to discuss the latest timepieces, share watch reviews, and connect with fellow horology enthusiasts.&quot;` |

### Webhooks

A `group_settings_update` webhook is triggered.

#### Group settings update succeed

[View the &quot;Group settings update succeed&quot; sample webhook](https://developers.facebook.com/documentation/business-messaging/whatsapp/groups/webhooks#group-settings-update-succeed).

#### Group settings update partial fail

[View the &quot;Group settings update partial fail&quot; sample webhook](https://developers.facebook.com/documentation/business-messaging/whatsapp/groups/webhooks#group-settings-update-partial-fail).

#### Group settings update total fail

[View the &quot;Group settings update total fail&quot; sample webhook](https://developers.facebook.com/documentation/business-messaging/whatsapp/groups/webhooks#group-settings-update-total-fail).

## Group message status webhooks

When you send a message to a group, you receive a status **messages** webhook when the message is delivered or read by group participants.

Status webhooks for individual group participants may be aggregated into a single webhook containing multiple `status` objects in the `statuses` array. However, aggregation is not guaranteed. If multiple participants&#039; statuses are generated at approximately the same time, they may be combined into a single webhook. If statuses are generated at different times, you may receive separate webhooks for each participant.

Each webhook only ever references a single message sent to a single group and a single status type (for example, `delivered`). Statuses for different messages, groups, or status types are never combined into a single webhook.

For the full webhook payload reference, see the [status messages webhook reference](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/messages/status).

### Pricing information

Status **messages** webhooks that contain pricing information will have `&lt;CONVERSATION_CATEGORY&gt;` set to one of:

- `group_marketing` — Indicates a group marketing conversation.
- `group_utility` — Indicates a group utility conversation.
- `group_service` — Indicates a group service conversation.
