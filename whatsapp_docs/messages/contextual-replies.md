# Contextual replies



Contextual replies are a special way of responding to a WhatsApp user message. Sending a message as a contextual reply makes it clearer to the user which message you are replying to by quoting the previous message in a contextual bubble:

## Limitations

- You cannot send a [reaction message](https://developers.facebook.com/documentation/business-messaging/whatsapp/messages/reaction-messages) as a contextual reply.

The contextual bubble does not appear at the top of the delivered message if:

- The previous message has been deleted or moved to long term storage (messages are typically moved to long term storage after 30 days, unless you have enabled [local storage](https://developers.facebook.com/documentation/business-messaging/whatsapp/local-storage)).
- You reply with an [audio](https://developers.facebook.com/documentation/business-messaging/whatsapp/messages/audio-messages), [image](https://developers.facebook.com/documentation/business-messaging/whatsapp/messages/image-messages), or [video message](https://developers.facebook.com/documentation/business-messaging/whatsapp/messages/video-messages) and the WhatsApp user is running KaiOS.
- You use the WhatsApp client to reply with a [push-to-talk](https://faq.whatsapp.com/657157755756612/?cms_platform=web) message and the WhatsApp user is running KaiOS.
- You reply with a [template message](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/overview).

## Request syntax

```https
POST /&lt;WHATSAPP_BUSINESS_PHONE_NUMBER_ID&gt;/messages
```


### Post body

```json
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;recipient_type&quot;: &quot;individual&quot;,
  &quot;to&quot;: &quot;&lt;WHATSAPP_USER_PHONE_NUMBER&gt;&quot;,
  &quot;context&quot;: &#123;
    &quot;message_id&quot;: &quot;WAMID_TO_REPLY_TO&quot;
  &#125;,

  /* Message type and type contents goes here */

&#125;
```

### Post body parameters

| Placeholder | Description | Example Value |
| --- | --- | --- |
| `&lt;WAMID_TO_REPLY_TO&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;WhatsApp message ID (wamid) of the previous message you want to reply to. | `wamid.HBgLMTY0NjcwNDM1OTUVAgASGBQzQTdCNTg5RjY1MEMyRjlGMjRGNgA=` |
| `&lt;WHATSAPP_USER_PHONE_NUMBER&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;WhatsApp user phone number. | `+16505551234` |

## Example request

Example of a text message sent as a reply to a previous message.

```curl
curl &#039;https://graph.facebook.com/v19.0/106540352242922/messages&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039; \
-d &#039;
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;recipient_type&quot;: &quot;individual&quot;,
  &quot;to&quot;: &quot;+16505551234&quot;,
  &quot;context&quot;: &#123;
    &quot;message_id&quot;: &quot;wamid.HBgLMTY0NjcwNDM1OTUVAgASGBQzQTdCNTg5RjY1MEMyRjlGMjRGNgA=&quot;
  &#125;,
  &quot;type&quot;: &quot;text&quot;,
  &quot;text&quot;: &#123;
    &quot;body&quot;: &quot;You&#039;re welcome, Pablo!&quot;
  &#125;
&#125;&#039;
```
