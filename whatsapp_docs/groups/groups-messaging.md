# Group messaging



**Warning:** **This page now only discusses how to send and receive messages in groups.**

**To learn how to manage groups, see the [Group Management Reference page](https://developers.facebook.com/documentation/business-messaging/whatsapp/groups/reference)**

## Overview

This document describes the APIs and webhooks for sending and receiving messages within groups. It details support for various message types, including:

* Text messages
* Media messages
* Text-based templates
* Media-based templates

## Subscribe to groups metadata webhooks

To receive webhook notifications for metadata about your groups, subscribe to the following webhook fields:

* `group_lifecycle_update`
* `group_participants_update`
* `group_settings_update`
* `group_status_update`

**Warning:** For a full reference of webhooks for the Groups API, see the [Webhooks for Groups API reference](https://developers.facebook.com/documentation/business-messaging/whatsapp/groups/webhooks).

## Send group message

To send a group message, use the [Messages API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/message-api#post-version-phone-number-id-messages).

This endpoint has been extended to support group messages in the following way:

* The `recipient_type` field now supports `group` as well as `individual`.
* The `to` field now supports the `group ID` that is obtained when using the Groups API.

### Example group message send

```html
curl &#039;https://graph.facebook.com/v25.0/756079150920219/messages&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer EAAAu...&#039; \
-d &#039;
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;recipient_type&quot;: &quot;group&quot;,
  &quot;to&quot;: &quot;Y2FwaV9ncm91cDoxNzA1NTU1MDEzOToxMjAzNjM0MDQ2OTQyMzM4MjAZD&quot;,
  &quot;type&quot;: &quot;text&quot;,
  &quot;text&quot;: &#123;
      &quot;preview_url&quot;: true,
      &quot;body&quot;: &quot;This is another destination option: https://www.luckytravel.com/DDLmU5F1Pw&quot;
  &#125;
&#125;&#039;
```

### Webhooks

#### Group message sent example

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
               &quot;statuses&quot;: [
                 &#123;
                   &quot;id&quot;: &quot;&lt;WHATSAPP_MESSAGE_ID&gt;&quot;,
                   &quot;recipient_id&quot;: &quot;&lt;GROUP_ID&gt;&quot;,
                   &quot;recipient_type&quot;: &quot;group&quot;,
                   &quot;status&quot;: &quot;sent&quot;,
                   &quot;timestamp&quot;: &quot;&lt;WEBHOOK_TRIGGER_TIMESTAMP&gt;&quot;,
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

#### Group message failed example

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
               &quot;statuses&quot;: [
                 &#123;
                   &quot;id&quot;: &quot;&lt;WHATSAPP_MESSAGE_ID&gt;&quot;,
                   &quot;recipient_id&quot;: &quot;&lt;GROUP_ID&gt;&quot;,
                   &quot;recipient_type&quot;: &quot;group&quot;,
                   &quot;status&quot;: &quot;failed&quot;,
                   &quot;timestamp&quot;: &quot;&lt;WEBHOOK_TRIGGER_TIMESTAMP&gt;&quot;,
                   &quot;errors&quot;: [
                     &#123;
                       &quot;code&quot;: &quot;&lt;ERROR_CODE&gt;&quot;,
                       &quot;title&quot;: &quot;&lt;ERROR_TITLE&gt;&quot;,
                       &quot;message&quot;: &quot;&lt;ERROR_MESSAGE&gt;&quot;,
                       &quot;error_data&quot;: &#123;
                         &quot;details&quot;: &quot;&lt;ERROR_DETAILS&gt;&quot;,
                       &#125;,
                       &quot;href&quot;: &quot;/documentation/business-messaging/whatsapp/support/error-codes&quot;
                    &#125;
                  ]
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

## Receive group messages

You can use the following webhooks to receive statuses on messages received in the group.

The `message` object includes a `group_id` field to indicate this is a group message. The `from` field in the `message` object and the contact object point to the same participant who sends this message.

### Webhooks

#### Receive group message webhook sample

```html
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [&#123;
      &quot;id&quot;: &quot;&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;&quot;,
      &quot;changes&quot;: [&#123;
          &quot;value&quot;: &#123;
              &quot;messaging_product&quot;: &quot;whatsapp&quot;,
              &quot;metadata&quot;: &#123;
                  &quot;display_phone_number&quot;: &quot;&lt;BUSINESS_DISPLAY_PHONE_NUMBER&gt;&quot;,
                  &quot;phone_number_id&quot;: &quot;&lt;BUSINESS_PHONE_NUMBER_ID&gt;&quot;
              &#125;,
              &quot;contacts&quot;: [&#123;
                  &quot;profile&quot;: &#123;
                    &quot;name&quot;: &quot;&lt;WHATSAPP_USER_NAME&gt;&quot;
                  &#125;,
                  &quot;wa_id&quot;: &quot;&lt;WHATSAPP_USER_PHONE_NUMBER&gt;&quot;
                &#125;],
              &quot;messages&quot;: [&#123;
                  &quot;from&quot;: &quot;&lt;GROUP_PARTICIPANT_PHONE_NUMBER&gt;&quot;,
                  &quot;group_id&quot;: &quot;&lt;GROUP_ID&gt;&quot;,
                  &quot;id&quot;: &quot;&lt;WHATSAPP_MESSAGE_ID&gt;&quot;,
                  &quot;timestamp&quot;: &quot;&lt;WEBHOOK_TRIGGER_TIMESTAMP&gt;&quot;,
                  &quot;text&quot;: &#123;
                    &quot;body&quot;: &quot;&lt;MESSAGE_BODY&gt;&quot;
                  &#125;,
                  &quot;type&quot;: &quot;text&quot;
                &#125;]
          &#125;,
          &quot;field&quot;: &quot;messages&quot;
        &#125;]
  &#125;]
&#125;
```

#### Receive unsupported group message webhook sample

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
                   &quot;phone_number_id&quot;: &quot;&lt;BUSINESS_PHONE_NUMBER_ID&gt;&quot;,
              &#125;,
              &quot;contacts&quot;: [
                &#123;
                  &quot;profile&quot;: &#123;
                    &quot;name&quot;: &quot;&lt;WHATSAPP_USER_NAME&gt;&quot;
                  &#125;,
                  &quot;wa_id&quot;: &quot;&lt;WHATSAPP_USER_PHONE_NUMBER&gt;&quot;
                &#125;
              ],
              &quot;messages&quot;: [
                &#123;
                  &quot;from&quot;: &quot;&lt;GROUP_PARTICIPANT_PHONE_NUMBER&gt;&quot;,
                  &quot;group_id&quot;: &quot;&lt;GROUP_ID&gt;&quot;,
                  &quot;id&quot;: &quot;&lt;WHATSAPP_MESSAGE_ID&gt;&quot;,
                  &quot;timestamp&quot;: &quot;&lt;WEBHOOK_TRIGGER_TIMESTAMP&gt;&quot;,
                  &quot;errors&quot;: [
                    &#123;
                      &quot;code&quot;: 130501,
                      &quot;message&quot;: &quot;Message type is not currently supported&quot;,
                      &quot;title&quot;: &quot;Unsupported message type&quot;,
                      &quot;error_data&quot;: &#123;
                        &quot;details&quot;: &quot;&lt;ERROR_DETAILS&gt;&quot;
                      &#125;
                    &#125;
                  ],
                  &quot;type&quot;: &quot;unsupported&quot;
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

## Pin and unpin group message

Pinning a message highlights its relevance.

The display order of the pinned messages is based on the chronological order of parent messages, newest first. If three messages are already pinned when a new pin request is made, the oldest pinned message will be automatically unpinned.

### Limits

1. When calling the API, only one message can be pinned at a time.
1. Only the group admin can pin or unpin messages.
1. A maximum of 3 pinned messages can exist at any time.

### Request syntax
`POST /&lt;BUSINESS_PHONE_NUMBER_ID&gt;/messages`

**Note: You will receive an error in the sync response if the `recipient_type` and `to` type do not match.**

### Request body

```html
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;recipient_type&quot;: &quot;group&quot;,
  &quot;to&quot;: &quot;&lt;GROUP_ID&gt;&quot;,
  &quot;type&quot;: &quot;pin&quot;,
  &quot;pin&quot;: &#123;
    &quot;type&quot;: &quot;&lt;PIN_OPERATION&gt;&quot;,
    &quot;message_id&quot;: &quot;&lt;MESSAGE_ID&gt;&quot;,
    &quot;expiration_days&quot;: &quot;&lt;EXPIRATION&gt;&quot;
  &#125;
&#125;
```

### Body parameters

| Placeholder | Description | Sample Value |
| --- | --- | --- |
| `&lt;GROUP_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required**&lt;br&gt;&lt;br&gt;The group in which you are pinning a message. | `Y2FwaV9ncm91cDoxOTUwNTU1MDA3OToxMjAzNjMzOTQzMjAdOTY0MTUZD` |
| `&lt;PIN_OPERATION&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required**&lt;br&gt;&lt;br&gt;The pinning operation you are performing on the group.&lt;br&gt;&lt;br&gt;Can either be `&quot;pin&quot;` or `&quot;unpin&quot;` | `pin` |
| `&lt;MESSAGE_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required**&lt;br&gt;&lt;br&gt;A unique identifier for the message you are pinning or unpinning in the group. | `wamid.HBgLM...` |
| `&lt;EXPIRATION&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | **Required when `PIN_OPERATION` is `pin`**&lt;br&gt;&lt;br&gt;Pin duration in days. Can be 1 to 30 days. | `4` |

### Response body

```html
    &#123;
      &quot;messaging_product&quot;: &quot;whatsapp&quot;,
      &quot;contacts&quot;: [
        &#123;
          &quot;input&quot;: &quot;Y2FwaV9ncm91cDo....&quot;,
          &quot;wa_id&quot;: &quot;Y2FwaV9ncm91cDo....&quot;
        &#125;
      ],
      &quot;messages&quot;: [
        &#123;
          &quot;id&quot;: &quot;wamid.HBgLM...&quot;
        &#125;
      ]
&#125;
```

### Webhooks

Subscribe to the `messages` webhook topic to receive message status notifications. Standard sent and delivered statuses webhooks will be received for the `message_id` in the response.

[Learn more about the messages `status` webhook object here](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/messages/status)

## Group message status webhooks

When you send messages to a group, you will receive a webhook when the message is delivered or read.

You receive a single aggregated webhook instead of multiple webhooks.

This means that if you send a message and are set to receive several `read` or `delivered` statuses, you receive a single aggregated webhook containing multiple `status` objects.

Each webhook you receive is only ever in reference to a single message sent to a single group and a single status type.

[Learn more about the Group Message Status webhook](https://developers.facebook.com/documentation/business-messaging/whatsapp/groups/webhooks#group-message-status-webhooks)
