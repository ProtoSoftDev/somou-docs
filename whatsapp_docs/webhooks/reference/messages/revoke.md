# Revoke messages webhook reference



**Warning:** The revoke webhook is only available to WhatsApp Business app users (aka &quot;Coexistence&quot;)

This reference describes revoke events and payload contents for the WhatsApp Business account messages webhook for replies to messages.

## Triggers

- A WhatsApp user revokes (deletes) a previously sent message.
- A WhatsApp user revokes a previously sent message within two days after being sent.

## Syntax

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
           &quot;contacts&quot;: [
             &#123;
               &quot;profile&quot;: &#123;
                 &quot;name&quot;: &quot;&lt;WHATSAPP_USER_PROFILE_NAME&gt;&quot;
               &#125;,
               &quot;wa_id&quot;: &quot;&lt;WHATSAPP_USER_ID&gt;&quot;
             &#125;
           ],
           &quot;messages&quot;: [
             &#123;
               &quot;from&quot;: &quot;&lt;WHATSAPP_USER_PHONE_NUMBER&gt;&quot;,
               &quot;id&quot;: &quot;&lt;WHATSAPP_MESSAGE_ID&gt;&quot;,
               &quot;timestamp&quot;: &quot;&lt;WEBHOOK_TRIGGER_TIMESTAMP&gt;&quot;,
               &quot;type&quot;: &quot;revoke&quot;,
               &quot;revoke&quot;: &#123;
                 &quot;original_message_id&quot;: &quot;&lt;ORIGINAL_WHATSAPP_MESSAGE_ID&gt;&quot;
               &#125;
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

## Parameters

| Placeholder | Description | Example value |
| --- | --- | --- |
| `&lt;BUSINESS_DISPLAY_PHONE_NUMBER&gt;` | Business display phone number. | 15550783881 |
| `&lt;BUSINESS_PHONE_NUMBER_ID&gt;` | Business phone number ID. | 106540352242922 |
| `&lt;WHATSAPP_USER_PROFILE_NAME&gt;` | WhatsApp user&#039;s profile name. | Sheena Nelson |
| `&lt;WHATSAPP_USER_ID&gt;` | WhatsApp user ID. | 16505551234 |
| `&lt;WHATSAPP_USER_PHONE_NUMBER&gt;` | WhatsApp user phone number. | 16505551234 |
| `&lt;WHATSAPP_MESSAGE_ID&gt;` | WhatsApp message ID for the revoke event. | wamid.HBgLMTY1MDM4Nzk0MzkV... |
| `&lt;WEBHOOK_TRIGGER_TIMESTAMP&gt;` | Unix timestamp when the webhook was triggered. | 1739321024 |
| `&lt;ORIGINAL_WHATSAPP_MESSAGE_ID&gt;` | ID of the original message being revoked (deleted). | wamid.HBgLMTQxMjU1NTA4MjkV... |

## Example

This example webhook describes a delete made by a user in a message.

```json
&#123;
 &quot;object&quot;: &quot;whatsapp_business_account&quot;,
 &quot;entry&quot;: [
   &#123;
     &quot;id&quot;: &quot;102290129340398&quot;,
     &quot;changes&quot;: [
       &#123;
         &quot;value&quot;: &#123;
           &quot;messaging_product&quot;: &quot;whatsapp&quot;,
           &quot;metadata&quot;: &#123;
             &quot;display_phone_number&quot;: &quot;15550783881&quot;,
             &quot;phone_number_id&quot;: &quot;106540352242922&quot;
           &#125;,
           &quot;contacts&quot;: [
             &#123;
               &quot;profile&quot;: &#123;
                 &quot;name&quot;: &quot;Sheena Nelson&quot;
               &#125;,
               &quot;wa_id&quot;: &quot;16505551234&quot;
             &#125;
           ],
           &quot;messages&quot;: [
             &#123;
               &quot;from&quot;: &quot;16505551234&quot;,
               &quot;id&quot;: &quot;wamid.HBgLMTY1MDM4Nzk0MzkVAgASGBQzQUFERjg0NDEzNDdFODU3MUMxMAA=&quot;,
               &quot;timestamp&quot;: &quot;1749854575&quot;,
               &quot;type&quot;: &quot;revoke&quot;,
               &quot;revoke&quot;: &#123;
                 &quot;original_message_id&quot;: &quot;wamid.HBgLMTQxMjU1NTA4MjkVAgASGBQzQUNCNjk5RDUwNUZGMUZEM0VBRAA=&quot;
               &#125;
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
