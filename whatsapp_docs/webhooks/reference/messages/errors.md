# Errors messages webhooks reference



This reference describes trigger events and payload contents for the WhatsApp Business account **messages** webhook for errors messages.

## Triggers

- A system-level problem prevents a request from being processed.
- An app- or account-level problem prevents a request from being processed.

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
            &quot;errors&quot;: [
              &#123;
                &quot;code&quot;: &lt;ERROR_CODE&gt;,
                &quot;title&quot;: &quot;&lt;ERROR_TITLE&gt;&quot;,
                &quot;message&quot;: &quot;&lt;ERROR_MESSAGE&gt;&quot;,
                &quot;error_data&quot;: &#123;
                  &quot;details&quot;: &quot;&lt;ERROR_DETAILS&gt;&quot;
                &#125;,
                &quot;href&quot;: &quot;&lt;ERROR_CODES_URL&gt;&quot;
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
| `&lt;BUSINESS_DISPLAY_PHONE_NUMBER&gt;`&lt;br&gt;&lt;br&gt;_String_ | Business display phone number. | `15550783881` |
| `&lt;BUSINESS_PHONE_NUMBER_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | Business phone number ID. | `106540352242922` |
| `&lt;ERROR_CODE&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | [Error code](https://developers.facebook.com/documentation/business-messaging/whatsapp/support/error-codes). | `130429` |
| `&lt;ERROR_CODES_URL&gt;`&lt;br&gt;&lt;br&gt;_String_ | Link to [error code documentation](https://developers.facebook.com/documentation/business-messaging/whatsapp/support/error-codes). | `/docs/whatsapp/cloud-api/support/error-codes/` |
| `&lt;ERROR_DETAILS&gt;`&lt;br&gt;&lt;br&gt;_String_ | [Error code](https://developers.facebook.com/documentation/business-messaging/whatsapp/support/error-codes) details. | `Message failed to send because there were too many messages sent from this phone number in a short period of time` |
| `&lt;ERROR_MESSAGE&gt;`&lt;br&gt;&lt;br&gt;_String_ | [Error code](https://developers.facebook.com/documentation/business-messaging/whatsapp/support/error-codes) message. This value is the same as the `title` property value. | `Rate limit hit` |
| `&lt;ERROR_TITLE&gt;`&lt;br&gt;&lt;br&gt;_String_ | [Error code](https://developers.facebook.com/documentation/business-messaging/whatsapp/support/error-codes) title. This value is the same as the `message` property value. | `Rate limit hit` |
| `&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | WhatsApp Business Account ID. | `102290129340398` |

## Example

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
            &quot;errors&quot;: [
              &#123;
                &quot;code&quot;: 130429,
                &quot;title&quot;: &quot;Rate limit hit&quot;,
                &quot;message&quot;: &quot;Rate limit hit&quot;,
                &quot;error_data&quot;: &#123;
                  &quot;details&quot;: &quot;Message failed to send because there were too many messages sent from this phone number in a short period of time&quot;
                &#125;,
                &quot;href&quot;: &quot;/documentation/business-messaging/whatsapp/support/error-codes&quot;
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
