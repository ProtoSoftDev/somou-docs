# phone_number_name_update webhook reference



This reference describes trigger events and payload contents for the WhatsApp Business account **phone_number_name_update** webhook.

The **phone_number_name_update** webhook notifies you of business phone number [display name verification](https://developers.facebook.com/documentation/business-messaging/whatsapp/display-names#display-name-verificationn) outcomes.


## Triggers

- A newly created business phone number&#039;s display name is reviewed.
- A business phone number&#039;s already approved display name is edited and reviewed.

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
            &quot;display_phone_number&quot;: &quot;&lt;BUSINESS_DISPLAY_PHONE_NUMBER&gt;&quot;,
            &quot;decision&quot;: &quot;&lt;DECISION&gt;&quot;,
            &quot;requested_verified_name&quot;: &quot;&lt;REQUESTED_DISPLAY_NAME&gt;&quot;,
            &quot;rejection_reason&quot;: &quot;&lt;REJECTION_REASON&gt;&quot;
          &#125;,
          &quot;field&quot;: &quot;phone_number_name_update&quot;
        &#125;
      ]
    &#125;
  ],
  &quot;object&quot;: &quot;whatsapp_business_account&quot;
&#125;
```

## Parameters

The following parameters can appear in a **phone_number_name_update** webhook notification payload.

| Placeholder | Description | Example value |
| --- | --- | --- |
| `&lt;BUSINESS_DISPLAY_PHONE_NUMBER&gt;`&lt;br&gt;&lt;br&gt;_String_ | Business display phone number. | `15550783881` |
| `&lt;DECISION&gt;`&lt;br&gt;&lt;br&gt;_String_ | Indicates the outcome of the business phone number [display name verification](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-phone-numbers/phone-numbers#display-name-verification) process.&lt;br&gt;&lt;br&gt;`APPROVED` — Indicates the display name has been approved and will now appear at the top of the business phone number&#039;s profile in the WhatsApp client.&lt;br&gt;&lt;br&gt;`DEFERRED` — Indicates a decision has been deferred.&lt;br&gt;&lt;br&gt;`PENDING` — Indicates a decision is still pending further review.&lt;br&gt;&lt;br&gt;`REJECTED` — Indicates the display name has been rejected. You can edit the name using [WhatsApp Manager](https://business.facebook.com/latest/whatsapp_manager/overview/). Review our [display name guidelines](https://www.facebook.com/business/help/757569725593362) before editing. | `APPROVED` |
| `&lt;REJECTION_REASON&gt;`&lt;br&gt;&lt;br&gt;_String_ | The reason why the business phone number display name was rejected, if it was rejected. Review our display name guidelines for common rejection reasons.&lt;br&gt;&lt;br&gt;Values can be:&lt;br&gt;&lt;br&gt;`NAME_EMPLOYEE_ISSUE` — Rejected because the display name included a person&#039;s name or employee identifier.&lt;br&gt;&lt;br&gt;`NAME_ENDCLIENT_NOTRELATED` — Rejected because the display name included an unrelated business&#039;s name.&lt;br&gt;&lt;br&gt;`NAME_FORMAT_UNACCEPTABLE` — Rejected because the display name used an unacceptable format.&lt;br&gt;&lt;br&gt;`NAME_INDIVIDUAL_ISSUE` — Rejected because the display name included a person&#039;s name or employee identifier.&lt;br&gt;&lt;br&gt;`NAME_NOT_CONSISTENT` — Rejected because the display name was not consistent with the business&#039;s branding.&lt;br&gt;&lt;br&gt;`null` — Indicates name was accepted.&lt;br&gt;&lt;br&gt;`UNKNOWN` — Rejected for an unknown reason. Contact support. | `APPROVED` |
| `&lt;REQUESTED_DISPLAY_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | The business phone number display name collected when the number was created, or name submitted when editing an already approved display name. | `Lucky Shrub` |
| `&lt;WEBHOOK_TRIGGER_TIMESTAMP&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | Unix timestamp indicating when the webhook was triggered. | `1739321024` |
| `&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | WhatsApp Business Account ID. | `102290129340398` |

## Example

```json
&#123;
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;102290129340398&quot;,
      &quot;time&quot;: 1739321024,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;display_phone_number&quot;: &quot;15550783881&quot;,
            &quot;decision&quot;: &quot;APPROVED&quot;,
            &quot;requested_verified_name&quot;: &quot;Lucky Shrub&quot;,
            &quot;rejection_reason&quot;: null
          &#125;,
          &quot;field&quot;: &quot;phone_number_name_update&quot;
        &#125;
      ]
    &#125;
  ],
  &quot;object&quot;: &quot;whatsapp_business_account&quot;
&#125;
```
