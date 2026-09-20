# template_category_update webhook reference



This reference describes trigger events and payload contents for the WhatsApp Business account **template_category_update** webhook.

The **template_category_update** webhook notifies you of changes to template&#039;s [category](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/template-categorization).


## Triggers

- The existing category of a WhatsApp template is going to be changed by an [automated process](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/template-categorization#how-we-update-a-template-s-category-after-initial-approval).
- The existing category of a WhatsApp template is changed manually or by an [automated process](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/template-categorization#how-we-update-a-template-s-category-after-initial-approval).

## Syntax

```html
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;&quot;,
      &quot;time&quot;: &lt;WEBHOOK_TRIGGER_TIMESTAMP&gt;,
      &quot;changes&quot;: [
        &#123;
          &quot;field&quot;: &quot;template_category_update&quot;,
          &quot;value&quot;: &#123;
            &quot;message_template_id&quot;: &lt;TEMPLATE_ID&gt;,
            &quot;message_template_name&quot;: &quot;&lt;TEMPLATE_NAME&gt;&quot;,
            &quot;message_template_language&quot;: &quot;&lt;TEMPLATE_LANGUAGE&gt;&quot;,

            &lt;!-- impending category change notifications only --&gt;
            &quot;correct_category&quot;: &quot;&lt;CORRECT_CATEGORY&gt;&quot;,
            &quot;new_category&quot;: &quot;&lt;CURRENT_CATEGORY&gt;&quot;,
            &quot;category_update_timestamp&quot;: &lt;CATEGORY_UPDATE_TIMESTAMP&gt;

            &lt;!-- completed category change notifications only --&gt;
            &quot;previous_category&quot;: &quot;&lt;PREVIOUS_CATEGORY&gt;&quot;,
            &quot;new_category&quot;: &quot;&lt;NEW_CATEGORY&gt;&quot;

          &#125;
        &#125;
      ]
    &#125;
  ]
&#125;
```

## Parameters

| Placeholder | Description | Example value |
| --- | --- | --- |
| `&lt;CORRECT_CATEGORY&gt;`&lt;br&gt;&lt;br&gt;_String_ | The category that the template will be [recategorized](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/template-categorization#how-we-update-a-template-s-category-after-initial-approval) as in 24 hours. | `MARKETING` |
| `&lt;CURRENT_CATEGORY&gt;`&lt;br&gt;&lt;br&gt;_String_ | The template&#039;s current [category](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/overview#template-categories). | `MARKETING` |
| `&lt;NEW_CATEGORY&gt;`&lt;br&gt;&lt;br&gt;_String_ | The template&#039;s new [category](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/overview#template-categories). | `MARKETING` |
| `&lt;CATEGORY_UPDATE_TIMESTAMP&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | The Unix timestamp (in seconds) indicating when the template&#039;s category will be updated to the `&lt;CORRECT_CATEGORY&gt;` specified in the webhook. This value represents the moment the update is scheduled to occur. | `1760711433` |
| `&lt;PREVIOUS_CATEGORY&gt;`&lt;br&gt;&lt;br&gt;_String_ | The template&#039;s previous [category](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/overview#template-categories). | `UTILITY` |
| `&lt;TEMPLATE_ID&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | Template ID. | `278077987957091` |
| `&lt;TEMPLATE_LANGUAGE&gt;`&lt;br&gt;&lt;br&gt;_String_ | Template [language and locale code](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/supported-languages). | `en-US` |
| `&lt;TEMPLATE_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | Template name. | `welcome_template` |
| `&lt;WEBHOOK_TRIGGER_TIMESTAMP&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | Unix timestamp indicating when the webhook was triggered. | `1739321024` |
| `&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | WhatsApp Business Account ID. | `102290129340398` |

## Examples

This example webhook describes a template that will be recategorized as `MARKETING` in 24 hours. Note that `new_category` indicates its _current_ category:

```json
&#123;
 &quot;entry&quot;: [
   &#123;
     &quot;id&quot;: &quot;102290129340398&quot;,
     &quot;time&quot;: 1746082800,
     &quot;changes&quot;: [
       &#123;
         &quot;field&quot;: &quot;template_category_update&quot;,
         &quot;value&quot;: &#123;
           &quot;message_template_id&quot;: 278077987957091,
           &quot;message_template_name&quot;: &quot;welcome_template&quot;,
           &quot;message_template_language&quot;: &quot;en-US&quot;,
           &quot;new_category&quot;: &quot;UTILITY&quot;,
           &quot;correct_category&quot;: &quot;MARKETING&quot;,
           &quot;category_update_timestamp&quot;: 1746169200
         &#125;
       &#125;
     ]
   &#125;
 ],
 &quot;object&quot;: &quot;whatsapp_business_account&quot;
&#125;
```

This example webhook describes a template that has been recategorized as `MARKETING`:

```json
&#123;
 &quot;entry&quot;: [
   &#123;
     &quot;id&quot;: &quot;102290129340398&quot;,
     &quot;time&quot;: 1746169200,
     &quot;changes&quot;: [
       &#123;
         &quot;field&quot;: &quot;template_category_update&quot;,
         &quot;value&quot;: &#123;
           &quot;message_template_id&quot;: 278077987957091,
           &quot;message_template_name&quot;: &quot;welcome_template&quot;,
           &quot;message_template_language&quot;: &quot;en-US&quot;,
           &quot;previous_category&quot;: &quot;UTILITY&quot;,
           &quot;new_category&quot;: &quot;MARKETING&quot;
         &#125;
       &#125;
     ]
   &#125;
 ],
 &quot;object&quot;: &quot;whatsapp_business_account&quot;
&#125;
```
