# Configure message time-to-live


If a message cannot be delivered to a WhatsApp user, WhatsApp retries delivery for a period of time known as *time-to-live* (&quot;TTL&quot;), or the message validity period.

You can customize the default TTL for authentication and utility templates sent via Cloud API, and for marketing templates sent via Marketing Messages API for WhatsApp.

Set a TTL for all authentication templates, preferably equal to or less than your code expiration time, to ensure WhatsApp users only get a message when a code is still usable.

## Defaults, min/max values, and compatibility table &#123;#defaults-minmax-values-and-compatibility-table&#125;

|  | Authentication | Utility | Marketing |
| --- | --- | --- | --- |
| **Default TTL** | 10 minutes&lt;br&gt;&lt;br&gt;30 days for authentication templates created before October 23, 2024 | 30 days | 30 days |
| **Compatibility** | Cloud API | Cloud API only | Marketing Messages API for WhatsApp |
| **Customizable range** | 30 seconds to 15 minutes | 30 seconds to 12 hours | 12 hours to 30 days |

## Customize the TTL

To set a custom TTL on an authentication, utility, or marketing template, include the `message_send_ttl_seconds` property in the `POST /&lt;PHONE_NUMBER_ID&gt;/message_templates` call.

You can change the TTL on a previously configured template using the same `POST /&lt;PHONE_NUMBER_ID&gt;/message_templates` call, as well.

TTL can be customized in 1-second increments.

### Valid `message_send_ttl_seconds` property values

* Authentication templates: `30` to `900` seconds (30 seconds to 15 minutes)
* Utility templates: `30` to `43200` seconds (30 seconds to 12 hours)
* Marketing templates: `43200` to `2592000` (12 hours to 30 days)

For authentication and utility templates, you can set the `message_send_ttl_seconds` property value to `-1`, which will set a custom TTL of 30 days.

### Example request

```html
curl &#039;https://graph.facebook.com/v21.0/102290129340398/message_templates&#039; \
      -H &#039;Authorization: Bearer EAAJB...&#039; \
      -H &#039;Content-Type: application/json&#039; \
      -d &#039;
      &#123;
        &quot;name&quot;: &quot;test_template&quot;,
        &quot;language&quot;: &quot;en_US&quot;,
        &quot;category&quot;: &quot;MARKETING&quot;,
        &quot;message_send_ttl_seconds&quot;: 120,
        &quot;components&quot;: [
          &#123;
            &quot;type&quot;: &quot;BODY&quot;,
            &quot;text&quot;: &quot;Shop now through &#123;&#123;1&#125;&#125; and use code &#123;&#123;2&#125;&#125; to get &#123;&#123;3&#125;&#125; off of all merchandise.&quot;,
            &quot;example&quot;: &#123;
              &quot;body_text&quot;: [
                [
                  &quot;the end of August&quot;,&quot;25OFF&quot;,&quot;25%&quot;
                ]
              ]
            &#125;
          &#125;,
          &#123;
            &quot;type&quot;: &quot;FOOTER&quot;,
            &quot;text&quot;: &quot;Use the buttons below to manage your marketing subscriptions&quot;
          &#125;
        ]
      &#125;&#039;
```

### Sample response

```json
&#123;
  &quot;id&quot;: &quot;572279198452421&quot;,
  &quot;status&quot;: &quot;PENDING&quot;,
  &quot;category&quot;: &quot;MARKETING&quot;
&#125;
```

### When TTL is exceeded

The system drops messages that cannot be delivered within the default or customized TTL.

If you do not receive a [delivered message webhook](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/messages/status) before the TTL is exceeded, assume the message was dropped.

If you send a message that [fails to deliver](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/messages/status), there could be a minor delay before you receive the webhook, so you may wish to build in a small buffer before assuming the message was dropped.

### TTL reset on automatic category updates

If a template is reclassified by an [automatic category update](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/template-categorization/#automatic-category-updates), the system clears the TTL value by setting it to a _null value_.
You can reset the TTL to any value within the customizable range for the new template category, as listed in the [Defaults, min/max values, and compatibility table](#defaults-minmax-values-and-compatibility-table).

For example, if a utility template with a custom TTL of 12 hours is reclassified as a marketing template, the system clears the custom TTL (`message_send_ttl_seconds` = `null`).
At this point, you can set the TTL to any value between 12 hours and 30 days, which is the customizable range for marketing templates, as listed in the table above.

See [Template categorization](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/template-categorization/) for more details about WABA message template categories and associated workflows.
