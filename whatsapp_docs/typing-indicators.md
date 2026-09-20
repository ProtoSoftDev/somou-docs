# Typing indicators



When you get a **messages** webhook indicating a [received message](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/messages), you can use the `message.id` value to mark the message as read and display a typing indicator so the WhatsApp user knows you are preparing a response. This is good practice if it will take you a few seconds to respond.

The typing indicator will be dismissed once you respond, or after 25 seconds, whichever comes first. To prevent a poor user experience, only display a typing indicator if you are going to respond.

## Request syntax

```html
curl -X POST \
&#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;WHATSAPP_BUSINESS_PHONE_NUMBER_ID&gt;/messages&#039;
-H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039; \
-H &#039;Content-Type: application/json&#039; \
-d &#039;
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;status&quot;: &quot;read&quot;,
  &quot;message_id&quot;: &quot;&lt;WHATSAPP_MESSAGE_ID&gt;&quot;,
  &quot;typing_indicator&quot;: &#123;
    &quot;type&quot;: &quot;text&quot;
  &#125;
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

Upon success:

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
  &quot;message_id&quot;: &quot;wamid.HBgLMTY1MDM4Nzk0MzkVAgARGBJDQjZCMzlEQUE4OTJBMTE4RTUA&quot;,
  &quot;typing_indicator&quot;: &#123;
    &quot;type&quot;: &quot;text&quot;
  &#125;
&#125;&#039;
```

## Example response

Upon success:

```json
&#123;
  &quot;success&quot;: true
&#125;
```
