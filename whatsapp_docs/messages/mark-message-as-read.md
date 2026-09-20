# Mark messages as read



When you receive a **message** webhook indicating an [incoming message](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/messages#incoming-messages), you can use the `message.id` value to mark the message as read.

Mark incoming messages as read within 30 days of receipt. When you mark a message as read, the API also marks earlier messages in the conversation as read.

If you mark a message as read with an invalid message ID, the API returns error code `131009` (&quot;Parameter value is not valid&quot;). Provide a valid `wamid` from a received message as the `message_id`.

## Request syntax

Use the [Messages API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/message-api#post-version-phone-number-id-messages) to mark a message as read.

```html
curl -X POST \
&#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;WHATSAPP_BUSINESS_PHONE_NUMBER_ID&gt;/messages&#039;
-H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039; \
-H &#039;Content-Type: application/json&#039; \
-d &#039;
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;status&quot;: &quot;read&quot;,
  &quot;message_id&quot;: &quot;&lt;WHATSAPP_MESSAGE_ID&gt;&quot;
&#125;&#039;
```

## Request parameters

| Placeholder | Description | Example value |
| --- | --- | --- |
| `&lt;ACCESS_TOKEN&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;[System token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#system-user-access-tokens) or [business token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#business-integration-system-user-access-tokens). | `EAAA...` |
| `&lt;API_VERSION&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Graph API version. | v25.0 |
| `&lt;WHATSAPP_BUSINESS_PHONE_NUMBER_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;WhatsApp business phone number ID. | `106540352242922` |
| `&lt;WHATSAPP_MESSAGE_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;WhatsApp message ID. This ID is assigned to the `messages.id` property in **received message** [messages](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/messages) webhooks. | `wamid.HBgLMTY1MDM4Nzk0MzkVAgARGBJDQjZCMzlEQUE4OTJBMTE4RTUA` |

## Response

A successful mark-as-read request returns the following response:

```json
&#123;
  &quot;success&quot;: true
&#125;
```

## Example request

```curl
curl &#039;https://graph.facebook.com/v25.0/106540352242922/messages&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039; \
-d &#039;
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;status&quot;: &quot;read&quot;,
  &quot;message_id&quot;: &quot;wamid.HBgLMTY1MDM4Nzk0MzkVAgARGBJDQjZCMzlEQUE4OTJBMTE4RTUA&quot;
&#125;&#039;
```

## Example response

A successful mark-as-read request returns:

```json
&#123;
  &quot;success&quot;: true
&#125;
```

