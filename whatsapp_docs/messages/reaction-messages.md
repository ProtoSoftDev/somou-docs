# Reaction messages



Reaction messages are emoji-reactions that you can apply to a WhatsApp user message you received.

## Limitations

When you send a reaction message, WhatsApp triggers only a [sent message webhook](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/messages/status) (`status` set to `sent`); it does not trigger delivered or read message webhooks.

## Request syntax

Use the [Messages API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/message-api#post-version-phone-number-id-messages) to apply an emoji reaction on a message you have received from a WhatsApp user.

```html
curl &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;WHATSAPP_BUSINESS_PHONE_NUMBER_ID&gt;/messages&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039; \
-d &#039;
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;recipient_type&quot;: &quot;individual&quot;,
  &quot;to&quot;: &quot;&lt;WHATSAPP_USER_PHONE_NUMBER&gt;&quot;,
  &quot;type&quot;: &quot;reaction&quot;,
  &quot;reaction&quot;: &#123;
    &quot;message_id&quot;: &quot;&lt;WHATSAPP_MESSAGE_ID&gt;&quot;,
    &quot;emoji&quot;: &quot;&lt;EMOJI&gt;&quot;
  &#125;
&#125;&#039;
```

## Request parameters

| Placeholder | Description | Example Value |
| --- | --- | --- |
| `&lt;ACCESS_TOKEN&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;[System token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#system-user-access-tokens) or [business token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#business-integration-system-user-access-tokens). | `EAAA...` |
| `&lt;API_VERSION&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Graph API version. | v25.0 |
| `&lt;EMOJI&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Unicode escape sequence of the emoji, or the emoji itself, to apply to the user message. | Unicode escape sequence example:&lt;br&gt;&lt;br&gt;`\uD83D\uDE00`&lt;br&gt;&lt;br&gt;Emoji example:&lt;br&gt;&lt;br&gt;😀 |
| `&lt;WHATSAPP_MESSAGE_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;WhatsApp message ID of message you want to apply the emoji to.&lt;br&gt;&lt;br&gt;If the message you are reacting to is more than 30 days old, doesn&#039;t correspond to any message in the chat thread, has been deleted, or is itself a reaction message, the reaction message will not be delivered and you will receive a **messages** webhook with error code `131009`. | `wamid.HBgLMTY0NjcwNDM1OTUVAgASGBQzQUZCMTY0MDc2MUYwNzBDNTY5MAA=` |
| `&lt;WHATSAPP_BUSINESS_PHONE_NUMBER_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;WhatsApp business phone number ID. | `106540352242922` |
| `&lt;WHATSAPP_USER_PHONE_NUMBER&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;WhatsApp user phone number. | `+16505551234` |

## Example request

Example request to apply the grinning face emoji (😀) to a previously received user message.

```curl
curl &#039;https://graph.facebook.com/v25.0/106540352242922/messages&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039; \
-d &#039;
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;recipient_type&quot;: &quot;individual&quot;,
  &quot;to&quot;: &quot;+16505551234&quot;,
  &quot;type&quot;: &quot;reaction&quot;,
  &quot;reaction&quot;: &#123;
    &quot;message_id&quot;: &quot;wamid.HBgLMTY0NjcwNDM1OTUVAgASGBQzQUZCMTY0MDc2MUYwNzBDNTY5MAA=&quot;,
    &quot;emoji&quot;: &quot;\uD83D\uDE00&quot;
  &#125;
&#125;&#039;
```

## Example response

```json
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;contacts&quot;: [
    &#123;
      &quot;input&quot;: &quot;+16505551234&quot;,
      &quot;wa_id&quot;: &quot;16505551234&quot;
    &#125;
  ],
  &quot;messages&quot;: [
    &#123;
      &quot;id&quot;: &quot;wamid.HBgLMTY0NjcwNDM1OTUVAgARGBI1RjQyNUE3NEYxMzAzMzQ5MkEA&quot;
    &#125;
  ]
&#125;
```

