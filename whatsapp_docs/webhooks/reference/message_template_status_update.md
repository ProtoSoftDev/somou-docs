# message_template_status_update webhook reference



This reference describes trigger events and payload contents for the WhatsApp Business Account `message_template_status_update` webhook.

The **message_template_status_update** webhook notifies you of changes to the status of an existing template.


## Triggers

- A template is approved.
- A template is rejected.
- A template is disabled.
- A template is archived.
- A template is unarchived.

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
            &quot;event&quot;: &quot;&lt;EVENT&gt;&quot;,
            &quot;message_template_id&quot;: &lt;TEMPLATE_ID&gt;,
            &quot;message_template_name&quot;: &quot;&lt;TEMPLATE_NAME&gt;&quot;,
            &quot;message_template_language&quot;: &quot;&lt;TEMPLATE_LANGUAGE_AND_LOCALE_CODE&gt;&quot;,
            &quot;reason&quot;: &quot;&lt;REASON&gt;&quot;,
            &quot;message_template_category&quot;: &quot;&lt;TEMPLATE_CATEGORY&gt;&quot;,

            &lt;!-- only included if template disabled --&gt;
            &quot;disable_info&quot;: &#123;
              &quot;disable_date&quot;: &quot;&lt;DISABLE_TIMESTAMP&gt;&quot;
            &#125;,

            &lt;!-- only included if template locked or unlocked --&gt;
            &quot;other_info&quot;: &#123;
              &quot;title&quot;: &quot;&lt;TITLE&gt;&quot;,
              &quot;description&quot;: &quot;&lt;DESCRIPTION&gt;&quot;
            &#125;,

            &lt;!-- only included if template rejected with INVALID_FORMAT reason --&gt;
            &quot;rejection_info&quot;: &#123;
              &quot;reason&quot;: &quot;&lt;REASON_INFO&gt;&quot;,
              &quot;recommendation&quot;: &quot;&lt;RECOMMENDATION_INFO&gt;&quot;
            &#125;
          &#125;,
          &quot;field&quot;: &quot;message_template_status_update&quot;
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
| `&lt;DESCRIPTION&gt;`&lt;br&gt;&lt;br&gt;_String_ | String describing why the template was locked or unlocked. | Your WhatsApp message template has been unpaused. |
| `&lt;DISABLE_TIMESTAMP&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | Unix timestamp indicating when the template was disabled. | `1751234563` |
| `&lt;EVENT&gt;`&lt;br&gt;&lt;br&gt;_String_ | Template status event. Values can be:&lt;br&gt;&lt;br&gt;`APPROVED` — Indicates the template has been approved and can now be sent in template messages.&lt;br&gt;&lt;br&gt;`ARCHIVED` — Indicates the template has been [archived](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/template-archival) due to inactivity. Archived templates are scheduled for deletion after 28 days unless unarchived.&lt;br&gt;&lt;br&gt;`UNARCHIVED` — Indicates the template has been [unarchived](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/template-archival) and restored to its previous status.&lt;br&gt;&lt;br&gt;`DELETED` — Indicates the template has been deleted.&lt;br&gt;&lt;br&gt;`DISABLED` — Indicates the template has been disabled due to user [feedback](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/template-quality).&lt;br&gt;&lt;br&gt;`FLAGGED` — Indicates the template has received negative feedback and is at risk of being disabled.&lt;br&gt;&lt;br&gt;`IN_APPEAL` — Indicates the template is in the [appeal](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/template-review#appeals) process.&lt;br&gt;&lt;br&gt;`LIMIT_EXCEEDED` — Indicates the WhatsApp Business Account template is at its [template limit](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/overview).&lt;br&gt;&lt;br&gt;`LOCKED` — Indicates the template has been locked and cannot be edited.&lt;br&gt;&lt;br&gt;`PAUSED` — Indicates the template has been [paused](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/template-pausing).&lt;br&gt;&lt;br&gt;`PENDING` — Indicates the template is undergoing template review.&lt;br&gt;&lt;br&gt;`REINSTATED` — Indicates the template is no longer flagged or disabled and can be sent in template messages again.&lt;br&gt;&lt;br&gt;`PENDING_DELETION` — Indicates template has been deleted via WhatsApp Manager.&lt;br&gt;&lt;br&gt;`REJECTED` — Indicates the template has been rejected. You can [edit the template](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/overview) to have it undergo template review again or [appeal](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/template-review#appeals) the rejection. | `APPROVED` |
| `&lt;TEMPLATE_ID&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | Template ID. | `1689556908129832` |
| `&lt;TEMPLATE_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | Template name. | `order_confirmation` |
| `&lt;TEMPLATE_LANGUAGE_AND_LOCALE_CODE&gt;`&lt;br&gt;&lt;br&gt;_String_ | Template [language and locale](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/supported-languages) code. | `en-US` |
| `&lt;REASON&gt;`&lt;br&gt;&lt;br&gt;_String_ | Template rejection reason, if rejected.&lt;br&gt;&lt;br&gt;If the template is scheduled for deletion, the value is `null` instead of a string. Otherwise, values can be:&lt;br&gt;&lt;br&gt;`ABUSIVE_CONTENT` — Indicates template contains content that violates our policies.&lt;br&gt;&lt;br&gt;`CATEGORY_NOT_AVAILABLE` — (Deprecated) Indicates an authentication templates for an unsupported region.&lt;br&gt;&lt;br&gt;`INCORRECT_CATEGORY` — Indicates the template&#039;s content doesn&#039;t match the category designated at the time of template creation.&lt;br&gt;&lt;br&gt;`INVALID_FORMAT` — Indicates template has an invalid format.&lt;br&gt;&lt;br&gt;`NONE` — Indicates template was paused.&lt;br&gt;&lt;br&gt;`PROMOTIONAL` — Indicates template contains content that violates our policies.&lt;br&gt;&lt;br&gt;`SCAM` — Indicates template contains content that violates our policies.&lt;br&gt;&lt;br&gt;`TAG_CONTENT_MISMATCH` — Indicates the template&#039;s content doesn&#039;t match the category designated at the time of template creation. | `INVALID_FORMAT` |
| `&lt;TITLE&gt;`&lt;br&gt;&lt;br&gt;_String_ | Title of template pause or unpause event.&lt;br&gt;&lt;br&gt;Values can be:&lt;br&gt;&lt;br&gt;`FIRST_PAUSE` — Indicates template has been paused for the first time.&lt;br&gt;&lt;br&gt;`SECOND_PAUSE` — Indicates the template has been paused a second time.&lt;br&gt;&lt;br&gt;`RATE_LIMITING_PAUSE` — Indicates template has been paused due to rate limiting.&lt;br&gt;&lt;br&gt;`UNPAUSE` — Indicates template has been unpaused.&lt;br&gt;&lt;br&gt;`DISABLED` — Indicates template has been disabled. | `FIRST_PAUSE` |
| `&lt;WEBHOOK_TRIGGER_TIMESTAMP&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | Unix timestamp indicating when the webhook was triggered. | `1739321024` |
| `&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | WhatsApp Business Account ID. | `102290129340398` |
| `&lt;MESSAGE_TEMPLATE_CATEGORY&gt;`&lt;br&gt;&lt;br&gt;_String_ | The template category.&lt;br&gt;&lt;br&gt;Values can be:&lt;br&gt;&lt;br&gt;`MARKETING` — Indicates template is categorized as MARKETING.&lt;br&gt;&lt;br&gt;`UTILITY` — Indicates the template is categorized as UTILITY.&lt;br&gt;&lt;br&gt;`AUTHENTICATION` — Indicates template is categorized as AUTHENTICATION. | `MARKETING` |
| `&lt;REASON_INFO&gt;`&lt;br&gt;&lt;br&gt;_String_ | Provides a detailed explanation for why the template was rejected. This field describes the specific issue detected in the template content. | `Your template has parameters placed next to each other (like &#123;&#123;1&#125;&#125;&#123;&#123;2&#125;&#125;) without text or punctuation between them.` |
| `&lt;RECOMMENDATION_INFO&gt;`&lt;br&gt;&lt;br&gt;_String_ | Offers actionable guidance on how to modify the template to resolve the rejection reason. This field suggests best practices for editing the template content. | `Separate parameters with descriptive text and ensure each parameter is clearly contextualized.` |

## Example

This example webhook describes a template that has been approved.

```json
&#123;
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;102290129340398&quot;,
      &quot;time&quot;: 1751247548,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;event&quot;: &quot;APPROVED&quot;,
            &quot;message_template_id&quot;: 1689556908129832,
            &quot;message_template_name&quot;: &quot;order_confirmation&quot;,
            &quot;message_template_language&quot;: &quot;en-US&quot;,
            &quot;reason&quot;: &quot;NONE&quot;,
            &quot;message_template_category&quot;: &quot;UTILITY&quot;
          &#125;,
          &quot;field&quot;: &quot;message_template_status_update&quot;
        &#125;
      ]
    &#125;
  ],
  &quot;object&quot;: &quot;whatsapp_business_account&quot;
&#125;
```

This example webhook describes a template that has been rejected with INVALID_FORMAT.

```json
&#123;
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;102290129340398&quot;,
      &quot;time&quot;: 1751247548,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;event&quot;: &quot;REJECTED&quot;,
            &quot;message_template_id&quot;: 1689556908129835,
            &quot;message_template_name&quot;: &quot;abandoned_cart&quot;,
            &quot;message_template_language&quot;: &quot;en&quot;,
            &quot;reason&quot;: &quot;INVALID_FORMAT&quot;,
            &quot;message_template_category&quot;: &quot;MARKETING&quot;,
            &quot;rejection_info&quot;: &#123;
              &quot;reason&quot;: &quot;Your template has parameters placed next to each other (like &#123;&#123;1&#125;&#125;&#123;&#123;2&#125;&#125;) without text or punctuation between them.&quot;,
              &quot;recommendation&quot;: &quot;Separate parameters with descriptive text and ensure each parameter is clearly contextualized.&quot;
            &#125;
          &#125;,
          &quot;field&quot;: &quot;message_template_status_update&quot;
        &#125;
      ]
    &#125;
  ],
  &quot;object&quot;: &quot;whatsapp_business_account&quot;
&#125;
```
