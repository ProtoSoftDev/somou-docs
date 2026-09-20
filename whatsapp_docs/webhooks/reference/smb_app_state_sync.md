# smb_app_state_sync webhook reference



This reference describes trigger events and payload contents for the WhatsApp Business account **smb_app_state_sync** webhook.

The **smb_app_state_sync** webhook is used for synchronizing contacts of [WhatsApp Business app users who have been onboarded](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/onboarding-business-app-users) via a solution provider.


## Triggers

- A partner [synchronizes the WhatsApp Business app contacts](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/onboarding-business-app-users#step-1--initiate-contacts-synchronization) of a business customer with a WhatsApp Business app phone number who the provider has [onboarded](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/onboarding-business-app-users).
- A business customer with a WhatsApp Business app phone number who has been [onboarded](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/onboarding-business-app-users) by a partner adds a contact to their WhatsApp Business app [contacts](https://faq.whatsapp.com/1270784217226727/).
- A business customer with a WhatsApp Business app phone number who has been [onboarded](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/onboarding-business-app-users) by a partner removes a contact from their WhatsApp Business app [contacts](https://faq.whatsapp.com/1270784217226727/).
- A business customer with a WhatsApp Business app phone number who has been [onboarded](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/onboarding-business-app-users) by a partner edits a contact in their WhatsApp Business app [contacts](https://faq.whatsapp.com/1270784217226727/).

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
            &quot;state_sync&quot;: [
              &#123;
                &quot;type&quot;: &quot;contact&quot;,
                &quot;contact&quot;: &#123;
                  &quot;full_name&quot;: &quot;&lt;CONTACT_FULL_NAME&gt;&quot;,
                  &quot;first_name&quot;: &quot;&lt;CONTACT_FIRST_NAME&gt;&quot;,
                  &quot;phone_number&quot;: &quot;&lt;CONTACT_PHONE_NUMBER&gt;&quot;
                &#125;,
                &quot;action&quot;: &quot;&lt;ACTION&gt;&quot;,
                &quot;metadata&quot;: &#123;
                  &quot;timestamp&quot;: &quot;&lt;WEBHOOK_TRIGGER_TIMESTAMP&gt;&quot;
                &#125;
              &#125;,
              &lt;!-- Additional contacts would follow, if any --&gt;
            ]
          &#125;,
          &quot;field&quot;: &quot;smb_app_state_sync&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

## Parameters

| Placeholder | Description | Example value |
| --- | --- | --- |
| `&lt;ACTION&gt;`&lt;br&gt;&lt;br&gt;_String_ | Indicates if the business customer added, edited, or deleted a contact from their WhatsApp Business app phone address book.&lt;br&gt;&lt;br&gt;Values can be:&lt;br&gt;&lt;br&gt;`add` — Indicates the WhatsApp Business app user added or edited a contact.&lt;br&gt;&lt;br&gt;`remove` — Indicates the WhatsApp Business app user removed a contact. | `add` |
| `&lt;BUSINESS_DISPLAY_PHONE_NUMBER&gt;`&lt;br&gt;&lt;br&gt;_String_ | Business display phone number. | `15550783881` |
| `&lt;BUSINESS_PHONE_NUMBER_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | Business phone number ID. | `106540352242922` |
| `&lt;CONTACT_FIRST_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | The contact&#039;s first name, as it appears in the business customer&#039;s WhatsApp Business app phone address book.&lt;br&gt;&lt;br&gt;Not included when the business customer removes a contact from their WhatsApp Business app phone address book. | `Pablo` |
| `&lt;CONTACT_FULL_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | The contact&#039;s full name, as it appears in the business customer&#039;s WhatsApp Business app phone address book.&lt;br&gt;&lt;br&gt;Not included when the business customer removes a contact from their WhatsApp Business app phone address book. | `Pablo Morales` |
| `&lt;CONTACT_PHONE_NUMBER&gt;`&lt;br&gt;_String_ | The contact&#039;s WhatsApp phone number. | `16505551234` |
| `&lt;WEBHOOK_TRIGGER_TIMESTAMP&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | Unix timestamp indicating when the webhook was triggered. | `1739321024` |
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
            &quot;state_sync&quot;: [
              &#123;
                &quot;type&quot;: &quot;contact&quot;,
                &quot;contact&quot;: &#123;
                  &quot;full_name&quot;: &quot;Pablo Morales&quot;,
                  &quot;first_name&quot;: &quot;Pablo&quot;,
                  &quot;phone_number&quot;: &quot;16505551234&quot;
                &#125;,
                &quot;action&quot;: &quot;add&quot;,
                &quot;metadata&quot;: &#123;
                  &quot;timestamp&quot;: &quot;1739321024&quot;
                &#125;
              &#125;
            ]
          &#125;,
          &quot;field&quot;: &quot;smb_app_state_sync&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```
