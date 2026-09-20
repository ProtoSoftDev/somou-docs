# Contacts messages



Contacts messages allow you to send rich contact information directly to WhatsApp users, such as names, phone numbers, physical addresses, and email addresses.

When a WhatsApp user taps the message&#039;s profile arrow, it displays the contact&#039;s information in a profile view:

Each message can include information for up to 257 contacts, although it is recommended to send fewer for usability and negative feedback reasons.

A contact&#039;s metadata (for example, addresses, birthdays, emails) may not be supported by the recipient, especially on their primary device. Refer to this [documentation](https://faq.whatsapp.com/378279804439436/?cms_platform=android) for the definitions of primary and linked devices.

## Request syntax

Use the [Messages API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/message-api#post-version-phone-number-id-messages) to send a contacts message to a WhatsApp user.

```html
curl &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;WHATSAPP_BUSINESS_PHONE_NUMBER_ID&gt;/messages&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039; \
-d &#039;
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;to&quot;: &quot;&lt;WHATSAPP_USER_PHONE_NUMBER&gt;&quot;,
  &quot;type&quot;: &quot;contacts&quot;,
  &quot;contacts&quot;: [
    &#123;
      &quot;addresses&quot;: [
        &#123;
          &quot;street&quot;: &quot;&lt;STREET_NUMBER_AND_NAME&gt;&quot;,
          &quot;city&quot;: &quot;&lt;CITY&gt;&quot;,
          &quot;state&quot;: &quot;&lt;STATE_CODE&gt;&quot;,
          &quot;zip&quot;: &quot;&lt;ZIP_CODE&gt;&quot;,
          &quot;country&quot;: &quot;&lt;COUNTRY_NAME&gt;&quot;,
          &quot;country_code&quot;: &quot;&lt;COUNTRY_CODE&gt;&quot;,
          &quot;type&quot;: &quot;&lt;ADDRESS_TYPE&gt;&quot;
        &#125;
        &lt;!-- Additional addresses objects go here, if using --&gt;
      ],
      &quot;birthday&quot;: &quot;&lt;BIRTHDAY&gt;&quot;,
      &quot;emails&quot;: [
        &#123;
          &quot;email&quot;: &quot;&lt;EMAIL_ADDRESS&gt;&quot;,
          &quot;type&quot;: &quot;&lt;EMAIL_TYPE&gt;&quot;
        &#125;
        &lt;!-- Additional emails objects go here, if using --&gt;
      ],
      &quot;name&quot;: &#123;
        &quot;formatted_name&quot;: &quot;&lt;FULL_NAME&gt;&quot;,
        &quot;first_name&quot;: &quot;&lt;FIRST_NAME&gt;&quot;,
        &quot;last_name&quot;: &quot;&lt;LAST_NAME&gt;&quot;,
        &quot;middle_name&quot;: &quot;&lt;MIDDLE_NAME&gt;&quot;,
        &quot;suffix&quot;: &quot;&lt;SUFFIX&gt;&quot;,
        &quot;prefix&quot;: &quot;&lt;PREFIX&gt;&quot;
      &#125;,
      &quot;org&quot;: &#123;
        &quot;company&quot;: &quot;&lt;COMPANY_OR_ORG_NAME&gt;&quot;,
        &quot;department&quot;: &quot;&lt;DEPARTMENT_NAME&gt;&quot;,
        &quot;title&quot;: &quot;&lt;JOB_TITLE&gt;&quot;
      &#125;,
      &quot;phones&quot;: [
        &#123;
          &quot;phone&quot;: &quot;&lt;PHONE_NUMBER&gt;&quot;,
          &quot;type&quot;: &quot;&lt;PHONE_NUMBER_TYPE&gt;&quot;,
          &quot;wa_id&quot;: &quot;&lt;WHATSAPP_USER_ID&gt;&quot;
        &#125;
        &lt;!-- Additional phones objects go here, if using --&gt;
      ],
      &quot;urls&quot;: [
        &#123;
          &quot;url&quot;: &quot;&lt;WEBSITE_URL&gt;&quot;,
          &quot;type&quot;: &quot;&lt;WEBSITE_TYPE&gt;&quot;
        &#125;
        &lt;!-- Additional URLs go here, if using --&gt;
      ]
    &#125;
  ]
&#125;&#039;
```

## Request parameters

| Placeholder | Description | Example Value |
| --- | --- | --- |
| `&lt;ACCESS_TOKEN&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;[System token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#system-user-access-tokens) or [business token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#business-integration-system-user-access-tokens). | `EAAA...` |
| `&lt;ADDRESS_TYPE&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Type of address, such as home or work. | `Home` |
| `&lt;API_VERSION&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Graph API version. | v25.0 |
| `&lt;BIRTHDAY&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Contact&#039;s birthday. Must be in `YYYY-MM-DD` format. | `1999-01-23` |
| `&lt;CITY&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;City where the contact resides. | `Menlo Park` |
| `&lt;COMPANY_OR_ORG_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Name of the company where the contact works. | `Lucky Shrub` |
| `&lt;COUNTRY_CODE&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;ISO two-letter country code. | `US` |
| `&lt;COUNTRY_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Country name. | `United States` |
| `&lt;DEPARTMENT_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Department within the company. | `Legal` |
| `&lt;EMAIL_ADDRESS&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Email address of the contact. | `bjohnson&#064;luckyshrub.com` |
| `&lt;EMAIL_TYPE&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Type of email, such as personal or work. | `Work` |
| `&lt;FIRST_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Contact&#039;s first name. | `Barbara` |
| `&lt;FORMATTED_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Contact&#039;s formatted name. This will appear in the message alongside the profile arrow button. | `Barbara J. Johnson` |
| `&lt;JOB_TITLE&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Contact&#039;s job title. | `Lead Counsel` |
| `&lt;LAST_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Contact&#039;s last name. | `Johnson` |
| `&lt;MIDDLE_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Contact&#039;s middle name. | `Joana` |
| `&lt;PHONE_NUMBER&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;WhatsApp user phone number. | `+16505559999` |
| `&lt;PHONE_NUMBER_TYPE&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Type of phone number, such as cell, mobile, main, iPhone, home, or work. | `Home` |
| `&lt;PREFIX&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Prefix for the contact&#039;s name, such as Mr., Ms., Dr., etc. | `Dr.` |
| `&lt;STATE_CODE&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Two-letter state code. | `CA` |
| `&lt;STREET_NUMBER_AND_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Street address of the contact. | `1 Lucky Shrub Way` |
| `&lt;SUFFIX&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Suffix for the contact&#039;s name, if applicable. | `Esq.` |
| `&lt;WEBSITE_TYPE&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Type of website, such as company, work, personal, Facebook Page, or Instagram. | `Company` |
| `&lt;WEBSITE_URL&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Website URL associated with the contact or their company. | `https://www.luckyshrub.com` |
| `&lt;WHATSAPP_USER_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;WhatsApp user ID. If omitted, the message will display an Invite to WhatsApp button instead of the standard buttons.&lt;br&gt;&lt;br&gt;See [Button Behavior](#button-behavior) below. | `19175559999` |
| `&lt;WHATSAPP_BUSINESS_PHONE_NUMBER_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;WhatsApp business phone number ID. | `106540352242922` |
| `&lt;WHATSAPP_USER_PHONE_NUMBER&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;WhatsApp user phone number. | `+16505551234` |
| `&lt;ZIP_CODE&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Postal or ZIP code. | `94025` |

## Button behavior

If you include the contact&#039;s WhatsApp ID in the message (via the `wa_id` property), the message will include a **Message** and a **Save contact** button:

If the WhatsApp user taps the **Message** button, it will open a new message with the contact. If the user taps the **Save contact** button, they will be given the option to save the contact as a new contact, or to update an existing contact.

If you omit the `wa_id` property, both buttons will be replaced with an **Invite to WhatsApp** button:

## Example request

Example request to send a contacts message with two physical addresses, two email addresses, two phone numbers, and two website URLs.

```curl
curl &#039;https://graph.facebook.com/v25.0/106540352242922/messages&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039; \
-d &#039;
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;to&quot;: &quot;+16505551234&quot;,
  &quot;type&quot;: &quot;contacts&quot;,
  &quot;contacts&quot;: [
    &#123;
      &quot;addresses&quot;: [
        &#123;
          &quot;street&quot;: &quot;1 Lucky Shrub Way&quot;,
          &quot;city&quot;: &quot;Menlo Park&quot;,
          &quot;state&quot;: &quot;CA&quot;,
          &quot;zip&quot;: &quot;94025&quot;,
          &quot;country&quot;: &quot;United States&quot;,
          &quot;country_code&quot;: &quot;US&quot;,
          &quot;type&quot;: &quot;Office&quot;
        &#125;,
        &#123;
          &quot;street&quot;: &quot;1 Hacker Way&quot;,
          &quot;city&quot;: &quot;Menlo Park&quot;,
          &quot;state&quot;: &quot;CA&quot;,
          &quot;zip&quot;: &quot;94025&quot;,
          &quot;country&quot;: &quot;United States&quot;,
          &quot;country_code&quot;: &quot;US&quot;,
          &quot;type&quot;: &quot;Pop-Up&quot;
        &#125;
      ],
      &quot;birthday&quot;: &quot;1999-01-23&quot;,
      &quot;emails&quot;: [
        &#123;
          &quot;email&quot;: &quot;bjohnson&#064;luckyshrub.com&quot;,
          &quot;type&quot;: &quot;Work&quot;
        &#125;,
        &#123;
          &quot;email&quot;: &quot;bjohnson&#064;luckyshrubplants.com&quot;,
          &quot;type&quot;: &quot;Work (old)&quot;
        &#125;
      ],
      &quot;name&quot;: &#123;
        &quot;formatted_name&quot;: &quot;Barbara J. Johnson&quot;,
        &quot;first_name&quot;: &quot;Barbara&quot;,
        &quot;last_name&quot;: &quot;Johnson&quot;,
        &quot;middle_name&quot;: &quot;Joana&quot;,
        &quot;suffix&quot;: &quot;Esq.&quot;,
        &quot;prefix&quot;: &quot;Dr.&quot;
      &#125;,
      &quot;org&quot;: &#123;
        &quot;company&quot;: &quot;Lucky Shrub&quot;,
        &quot;department&quot;: &quot;Legal&quot;,
        &quot;title&quot;: &quot;Lead Counsel&quot;
      &#125;,
      &quot;phones&quot;: [
        &#123;
          &quot;phone&quot;: &quot;+16505559999&quot;,
          &quot;type&quot;: &quot;Landline&quot;
        &#125;,
        &#123;
          &quot;phone&quot;: &quot;+19175559999&quot;,
          &quot;type&quot;: &quot;Mobile&quot;,
          &quot;wa_id&quot;: &quot;19175559999&quot;
        &#125;
      ],
      &quot;urls&quot;: [
        &#123;
          &quot;url&quot;: &quot;https://www.luckyshrub.com&quot;,
          &quot;type&quot;: &quot;Company&quot;
        &#125;,
        &#123;
          &quot;url&quot;: &quot;https://www.facebook.com/luckyshrubplants&quot;,
          &quot;type&quot;: &quot;Company (FB)&quot;
        &#125;
      ]
    &#125;
  ]
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

