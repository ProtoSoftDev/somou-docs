# message_template_components_update webhook reference



This reference describes trigger events and payload contents for the WhatsApp Business account `message_template_components_update` webhook.

The **message_template_components_update** webhook notifies you of changes to a template&#039;s components.


## Triggers

- A template is edited.

## Syntax

```html
&#123;
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;&quot;,
      &quot;time&quot;: &lt;WEBHOOK_TRIGGER_TIMESTAMP&gt;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;message_template_id&quot;: &lt;TEMPLATE_ID&gt;,
            &quot;message_template_name&quot;: &quot;&lt;TEMPLATE_NAME&gt;&quot;,
            &quot;message_template_language&quot;: &quot;&lt;TEMPLATE_LANGUAGE_AND_LOCALE_CODE&gt;&quot;,
            &quot;message_template_element&quot;: &quot;&lt;TEMPLATE_BODY_TEXT&gt;,

            &lt;!-- only included if template has a text header --&gt;
            &quot;message_template_title&quot;: &quot;&lt;TEMPLATE_HEADER_TEXT&gt;&quot;,

            &lt;!-- only included if template has a footer --&gt;
            &quot;message_template_footer&quot;: &quot;&lt;TEMPLATE_FOOTER_TEXT&gt;&quot;,

            &lt;!-- only included if template has a url or phone number button --&gt;
            &quot;message_template_buttons&quot;: [
              &#123;
                &quot;message_template_button_type&quot;: &quot;&lt;BUTTON_TYPE&gt;&quot;,
                &quot;message_template_button_text&quot;: &quot;&lt;BUTTON_LABEL_TEXT&gt;&quot;,

                &lt;!--only included for url buttons --&gt;
                &quot;message_template_button_url&quot;: &quot;&lt;BUTTON_URL&gt;&quot;,

                &lt;!--only included for phone number buttons --&gt;
                &quot;message_template_button_phone_number&quot;: &quot;&lt;BUTTON_PHONE_NUMBER&gt;&quot;
              &#125;
            ]
          &#125;,
          &quot;field&quot;: &quot;message_template_components_update&quot;
        &#125;
      ]
    &#125;
  ],
  &quot;object&quot;: &quot;whatsapp_business_account&quot;
&#125;
```

## Parameters

| Placeholder | Description | Example value |
| --- | --- | --- |
| `&lt;BUTTON_LABEL_TEXT&gt;`&lt;br&gt;&lt;br&gt;_String_ | Button label text. | `Email support` |
| `&lt;BUTTON_PHONE_NUMBER&gt;`&lt;br&gt;&lt;br&gt;_String_ | Button phone number. | `+15550783881` |
| `&lt;BUTTON_TYPE&gt;`&lt;br&gt;&lt;br&gt;_String_ | [Button type](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/components#buttons).&lt;br&gt;&lt;br&gt;Values can include:&lt;br&gt;&lt;br&gt;- `CATALOG`&lt;br&gt;- `COPY_CODE`&lt;br&gt;- `EXTENSION`&lt;br&gt;- `FLOW`, `MPM`&lt;br&gt;- `ORDER_DETAILS`&lt;br&gt;- `OTP`&lt;br&gt;- `PHONE_NUMBER`&lt;br&gt;- `POSTBACK`&lt;br&gt;- `REMINDER`&lt;br&gt;- `SEND_LOCATION`&lt;br&gt;- `SPM`&lt;br&gt;- `QUICK_REPLY`&lt;br&gt;- `URL`&lt;br&gt;- `VOICE_CALL` | `URL` |
| `&lt;BUTTON_URL&gt;`&lt;br&gt;&lt;br&gt;_String_ | Button URL. | `https://www.luckyshrub.com/support` |
| `&lt;TEMPLATE_BODY_TEXT&gt;`&lt;br&gt;&lt;br&gt;_String_ | Template body text. | `Thank you for your order, &#123;&#123;1&#125;&#125;! Your order number is &#123;&#123;2&#125;&#125;. If you have any questions, contact support using the buttons below. Thanks again!` |
| `&lt;TEMPLATE_FOOTER_TEXT&gt;`&lt;br&gt;&lt;br&gt;_String_ | Template footer text. | `Lucky Shrub: the Succulent Specialists!` |
| `&lt;TEMPLATE_HEADER_TEXT&gt;`&lt;br&gt;&lt;br&gt;_String_ | Template header text. | `Your order is confirmed!` |
| `&lt;TEMPLATE_ID&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | Template ID. | `1315502779341834` |
| `&lt;TEMPLATE_LANGUAGE_AND_LOCALE_CODE&gt;`&lt;br&gt;&lt;br&gt;_String_ | Template [language and locale](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/supported-languages) code. | `en_US` |
| `&lt;TEMPLATE_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | Template name. | `order_confirmation` |
| `&lt;WEBHOOK_TRIGGER_TIMESTAMP&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | Unix timestamp indicating when the webhook was triggered. | `1739321024` |
| `&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | WhatsApp Business Account ID. | `102290129340398` |

## Example

```json
&#123;
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;102290129340398&quot;,
      &quot;time&quot;: 1751250234,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;message_template_id&quot;: 1315502779341834,
            &quot;message_template_name&quot;: &quot;order_confirmation&quot;,
            &quot;message_template_language&quot;: &quot;en_US&quot;,
            &quot;message_template_title&quot;: &quot;Your order is confirmed!&quot;,
            &quot;message_template_element&quot;: &quot;Thank you for your order, &#123;&#123;1&#125;&#125;! Your order number is &#123;&#123;2&#125;&#125;. If you have any questions, contact support using the buttons below. Thanks again!&quot;,
            &quot;message_template_footer&quot;: &quot;Lucky Shrub: the Succulent Specialists!&quot;,
            &quot;message_template_buttons&quot;: [
              &#123;
                &quot;message_template_button_type&quot;: &quot;PHONE_NUMBER&quot;,
                &quot;message_template_button_text&quot;: &quot;Phone support&quot;,
                &quot;message_template_button_phone_number&quot;: &quot;+15550783881&quot;
              &#125;,
              &#123;
                &quot;message_template_button_type&quot;: &quot;URL&quot;,
                &quot;message_template_button_text&quot;: &quot;Email support&quot;,
                &quot;message_template_button_url&quot;: &quot;https://www.luckyshrub.com/support&quot;
              &#125;
            ]
          &#125;,
          &quot;field&quot;: &quot;message_template_components_update&quot;
        &#125;
      ]
    &#125;
  ],
  &quot;object&quot;: &quot;whatsapp_business_account&quot;
&#125;
```
