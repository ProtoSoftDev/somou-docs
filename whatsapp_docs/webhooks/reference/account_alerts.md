# account_alerts webhook reference



This reference describes trigger events and payload contents for the WhatsApp Business account `account_alerts` webhook.

The **account_alerts** webhook notifies you of changes to a business phone number&#039;s [messaging limit](https://developers.facebook.com/documentation/business-messaging/whatsapp/messaging-limits), [business profile](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-phone-numbers/phone-numbers#business-profiles), and [Official Business Account](https://developers.facebook.com/documentation/business-messaging/whatsapp/whatsapp-business-accounts#official-business-account) status.


## Triggers

- An increase to the messaging limit of all of a business portfolio&#039;s phone numbers is denied, a decision on the increase has been deferred, or more information is needed before a decision can be made.
- A business phone number Official Business Account status is approved or denied.
- A business phone number&#039;s business profile photo is deleted.

## Syntax

```json
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;&quot;,
      &quot;time&quot;: &lt;WEBHOOK_TRIGGER_TIMESTAMP&gt;,
      &quot;changes&quot;: [
        &#123;
          &quot;field&quot;: &quot;account_alerts&quot;,
          &quot;value&quot;: &#123;
            &quot;entity_type&quot;: &quot;&lt;ENTITY_TYPE&gt;&quot;,
            &quot;entity_id&quot;: &quot;&lt;ENTITY_ID&gt;&quot;,
            &quot;alert_info&quot;: &#123;
              &quot;alert_severity&quot;: &quot;&lt;ALERT_SEVERITY&gt;&quot;,
              &quot;alert_status&quot;: &quot;&lt;ALERT_STATUS&gt;&quot;,
              &quot;alert_type&quot;: &quot;&lt;ALERT_TYPE&gt;&quot;,
              &quot;alert_description&quot;: &quot;&lt;ALERT_DESCRIPTION&gt;&quot;
            &#125;
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
| `&lt;ALERT_DESCRIPTION&gt;`&lt;br&gt;&lt;br&gt;_String_ | Alert description.&lt;br&gt;&lt;br&gt;Values can be:&lt;br&gt;&lt;br&gt;- `Additional verification is required for your business &#123;NAME&#125;. Go to Security Center in Meta for Business to complete identity verification. To continue without completing additional verification, your business can use WhatsApp Business platform actively for several days and follow our messaging policies.`&lt;br&gt;&lt;br&gt;- `Based on your activity, limits cannot be increased for your business. Contact support for more information.`&lt;br&gt;&lt;br&gt;- `Limits cannot be increased for your business &lt;BUSINESS_PORTFOLIO_NAME&gt;. Use WhatsApp Business platform actively for several days and follow our messaging policies.`&lt;br&gt;&lt;br&gt;- `Limits cannot be increased at this time for your business &lt;BUSINESS_PORTFOLIO_NAME&gt;. Limits cannot be increased as your identity verification submission was rejected. To continue without additional verification, use WhatsApp Business platform actively for several days and follow our messaging policies.`&lt;br&gt;&lt;br&gt;- `Please reupload your profile picture`&lt;br&gt;&lt;br&gt;- `This phone number now has a green badge next to its name showing that it&#039;s an authentic and notable business account. Add more details to your business profile to increase customer trust.`&lt;br&gt;&lt;br&gt;- `We do not grant official business accounts to individuals. Your display name must be directly associated with your business. Edit the display name and submit a new request.`&lt;br&gt;&lt;br&gt;- `Your messaging quality was too low to unlock more capabilities at this time. Alternatively, your business can unlock more capabilities by submitting business documents to verify your business.` | `Limits cannot be increased for your business &lt;BUSINESS_PORTFOLIO_NAME&gt;. Use WhatsApp Business platform actively for several days and follow our messaging policies.` |
| `&lt;ALERT_SEVERITY&gt;`&lt;br&gt;&lt;br&gt;_String_ | Alert severity. Values can be:&lt;br&gt;&lt;br&gt;`CRITICAL` — Indicates a rejection or denial. The `alert_description` value may describe actions that can be taken to resolve the underlying reason for the rejection or denial.&lt;br&gt;&lt;br&gt;`INFORMATIONAL` — Indicates webhooks contain only informational data and no action is needed.&lt;br&gt;&lt;br&gt;`WARNING` — Indicates action may be needed. The `alert_description` value describes possible actions that can be taken. | `WARNING` |
| `&lt;ALERT_STATUS&gt;` | Alert status.&lt;br&gt;&lt;br&gt;Values can be:&lt;br&gt;&lt;br&gt;- `ACTIVE`&lt;br&gt;- `NONE` | `ACTIVE` |
| `&lt;ALERT_TYPE&gt;`&lt;br&gt;&lt;br&gt;_String_ | Alert type.&lt;br&gt;&lt;br&gt;Values can be:&lt;br&gt;&lt;br&gt;`INCREASED_CAPABILITIES_ELIGIBILITY_DEFERRED` — Indicates Meta doesn&#039;t have enough message signal to make a determination, identity verification was rejected, or your message quality is too low. Possible solutions are to increase your [messaging limit](https://developers.facebook.com/documentation/business-messaging/whatsapp/messaging-limits) or get your [business verified](https://www.facebook.com/business/help/2058515294227817).&lt;br&gt;&lt;br&gt;`INCREASED_CAPABILITIES_ELIGIBILITY_FAILED` — Indicates messaging limits cannot be increased due to past messaging activity. Possible solutions are to [request an increase](https://developers.facebook.com/documentation/business-messaging/whatsapp/messaging-limits#request-an-increase) or get your [business verified](https://www.facebook.com/business/help/2058515294227817).&lt;br&gt;&lt;br&gt;`INCREASED_CAPABILITIES_ELIGIBILITY_NEED_MORE_INFO` — Indicates messaging limits cannot be increased due to past messaging activity. Possible solutions are to [verify your identity](https://www.facebook.com/business/help/587323819101032) or increase your [messaging limit](https://developers.facebook.com/documentation/business-messaging/whatsapp/messaging-limits).&lt;br&gt;&lt;br&gt;`OBA_APPROVED` — Indicates Official Business Account status approved.&lt;br&gt;&lt;br&gt;`OBA_REJECTED` — Indicates Official Business Account (&quot;OBA&quot;) status denied. Review OBA [criteria](https://developers.facebook.com/documentation/business-messaging/whatsapp/whatsapp-business-accounts#criteria) and edit your [display name](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-phone-numbers/phone-numbers#display-names) after reviewing our display name guidelines.&lt;br&gt;&lt;br&gt;`PROFILE_PICTURE_LOST` — Indicates the business phone number&#039;s business profile photo has been deleted. Upload a new photo using [WhatsApp Manager](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-phone-numbers/phone-numbers#viewing-or-updating-your-profile-via-whatsapp-manager) or the [API](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-phone-numbers/phone-numbers#updating-your-profile-via-api). | `INCREASED_CAPABILITIES_ELIGIBILITY_DEFERRED` |
| `&lt;ENTITY_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | Entity ID. Value can be a business portfolio ID or business phone number ID. | `506914307656634` |
| `&lt;ENTITY_TYPE&gt;`&lt;br&gt;&lt;br&gt;_String_ | Entity type. Values can be:&lt;br&gt;&lt;br&gt;`BUSINESS` — Indicates a change associated with a business portfolio.&lt;br&gt;&lt;br&gt;`PHONE_NUMBER` — Indicates a change associated with a business phone number.&lt;br&gt;&lt;br&gt;`CURRENT_STATUS_ID` — Indicates a change associated with a business phone number&#039;s [business profile](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-phone-numbers/phone-numbers#business-profiles). | `BUSINESS` |
| `&lt;WEBHOOK_TRIGGER_TIMESTAMP&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | Unix timestamp indicating when the webhook was triggered. | `1739321024` |
| `&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | WhatsApp Business Account ID. | `102290129340398` |

## Example

```json
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;102290129340398&quot;,
      &quot;time&quot;: 1745612159,
      &quot;changes&quot;: [
        &#123;
          &quot;field&quot;: &quot;account_alerts&quot;,
          &quot;value&quot;: &#123;
            &quot;entity_type&quot;: &quot;BUSINESS&quot;,
            &quot;entity_id&quot;: &quot;506914307656634&quot;,
            &quot;alert_info&quot;: &#123;
              &quot;alert_severity&quot;: &quot;WARNING&quot;,
              &quot;alert_status&quot;: &quot;ACTIVE&quot;,
              &quot;alert_type&quot;: &quot;INCREASED_CAPABILITIES_ELIGIBILITY_DEFERRED&quot;,
              &quot;alert_description&quot;: &quot;Limits cannot be increased for your business &lt;BUSINESS_PORTFOLIO_NAME&gt;. Use WhatsApp Business platform actively for several days and follow our messaging policies.&quot;
            &#125;
          &#125;
        &#125;
      ]
    &#125;
  ]
&#125;
```
