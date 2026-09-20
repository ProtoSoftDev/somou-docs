# Contacts messages webhook reference



This reference describes trigger events and payload contents for the WhatsApp Business account **messages** webhook for messages containing one or more contacts.

## Triggers

- A WhatsApp user sends one or more contacts to a business.
- A WhatsApp user sends one or more contacts to a business via a Click to WhatsApp ad.

## Syntax

Many contact properties may be omitted if the WhatsApp user chooses not to share them or if their device prevents sharing.

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
                &quot;wa_id&quot;: &quot;&lt;WHATSAPP_USER_ID&gt;&quot;,
                &quot;identity_key_hash&quot;: &quot;&lt;IDENTITY_KEY_HASH&gt;&quot; &lt;!-- only included if identity change check enabled --&gt;
              &#125;
            ],
            &quot;messages&quot;: [
              &#123;
                &quot;from&quot;: &quot;&lt;WHATSAPP_USER_PHONE_NUMBER&gt;&quot;,
                &quot;id&quot;: &quot;&lt;WHATSAPP_MESSAGE_ID&gt;&quot;,
                &quot;timestamp&quot;: &quot;&lt;WEBHOOK_TRIGGER_TIMESTAMP&gt;&quot;,
                &quot;type&quot;: &quot;contacts&quot;,
                &quot;contacts&quot;: [
                  &#123;
                    &quot;addresses&quot;: [
                      &#123;
                        &quot;city&quot;: &quot;&lt;CONTACT_CITY&gt;&quot;,
                        &quot;country&quot;: &quot;&lt;CONTACT_COUNTRY&gt;&quot;,
                        &quot;country_code&quot;: &quot;&lt;CONTACT_COUNTRY_CODE&gt;&quot;,
                        &quot;state&quot;: &quot;&lt;CONTACT_STATE&gt;&quot;,
                        &quot;street&quot;: &quot;&lt;CONTACT_STREET&gt;&quot;,
                        &quot;type&quot;: &quot;&lt;CONTACT_ADDRESS_TYPE&gt;&quot;,
                        &quot;zip&quot;: &quot;&lt;CONTACT_ZIP&gt;&quot;
                      &#125;
                    ],
                    &quot;birthday&quot;: &quot;&lt;CONTACT_BIRTHDAY&gt;&quot;,
                    &quot;emails&quot;: [
                      &#123;
                        &quot;email&quot;: &quot;&lt;CONTACT_EMAIL&gt;&quot;,
                        &quot;type&quot;: &quot;&lt;CONTACT_EMAIL_TYPE&gt;&quot;
                      &#125;
                    ],
                    &quot;name&quot;: &#123;
                      &quot;formatted_name&quot;: &quot;&lt;CONTACT_FORMATTED_NAME&gt;&quot;,
                      &quot;first_name&quot;: &quot;&lt;CONTACT_FIRST_NAME&gt;&quot;,
                      &quot;last_name&quot;: &quot;&lt;CONTACT_LAST_NAME&gt;&quot;,
                      &quot;middle_name&quot;: &quot;&lt;CONTACT_MIDDLE_NAME&gt;&quot;,
                      &quot;suffix&quot;: &quot;&lt;CONTACT_NAME_SUFFIX&gt;&quot;,
                      &quot;prefix&quot;: &quot;&lt;CONTACT_NAME_PREFIX&gt;&quot;
                    &#125;,
                    &quot;org&quot;: &#123;
                      &quot;company&quot;: &quot;&lt;CONTACT_ORG_COMPANY&gt;&quot;,
                      &quot;department&quot;: &quot;&lt;CONTACT_ORG_DEPARTMENT&gt;&quot;,
                      &quot;title&quot;: &quot;&lt;CONTACT_ORG_TITLE&gt;&quot;
                    &#125;,
                    &quot;phones&quot;: [
                      &#123;
                        &quot;phone&quot;: &quot;&lt;CONTACT_PHONE&gt;&quot;,
                        &quot;wa_id&quot;: &quot;&lt;CONTACT_WHATSAPP_PHONE_NUMBER&gt;&quot;,
                        &quot;type&quot;: &quot;&lt;CONTACT_PHONE_TYPE&gt;&quot;
                      &#125;
                    ],
                    &quot;urls&quot;: [
                      &#123;
                        &quot;url&quot;: &quot;&lt;CONTACT_URL&gt;&quot;,
                        &quot;type&quot;: &quot;&lt;CONTACT_URL_TYPE&gt;&quot;
                      &#125;
                    ]
                  &#125;
                ],

                &lt;!-- only included if message sent via a Click to WhatsApp ad --&gt;
                &quot;referral&quot;: &#123;
                  &quot;source_url&quot;: &quot;&lt;AD_URL&gt;&quot;,
                  &quot;source_id&quot;: &quot;&lt;AD_ID&gt;&quot;,
                  &quot;source_type&quot;: &quot;ad&quot;,
                  &quot;body&quot;: &quot;&lt;AD_PRIMARY_TEXT&gt;&quot;,
                  &quot;headline&quot;: &quot;&lt;AD_HEADLINE&gt;&quot;,
                  &quot;media_type&quot;: &quot;&lt;AD_MEDIA_TYPE&gt;&quot;,
                  &quot;image_url&quot;: &quot;&lt;AD_IMAGE_URL&gt;&quot;,
                  &quot;video_url&quot;: &quot;&lt;AD_VIDEO_URL&gt;&quot;,
                  &quot;thumbnail_url&quot;: &quot;&lt;AD_VIDEO_THUMBNAIL&gt;&quot;,
                  &quot;ctwa_clid&quot;: &quot;&lt;AD_CLICK_ID&gt;&quot;,
                  &quot;welcome_message&quot;: &#123;
                    &quot;text&quot;: &quot;&lt;AD_GREETING_TEXT&gt;&quot;
                  &#125;
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
| `&lt;AD_CLICK_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | Click to WhatsApp ad click ID.&lt;br&gt;&lt;br&gt;The `ctwa_clid` property is omitted entirely for messages originating from an ad in WhatsApp Status ([WhatsApp Status ad placements](https://www.facebook.com/business/help/1074444721456755)). | `Aff-n8ZTODiE79d22KtAwQKj9e_mIEOOj27vDVwFjN80dp4_0NiNhEgpGo0AHemvuSoifXaytfTzcchptiErTKCqTrJ5nW1h7IHYeYymGb5K5J5iTROpBhWAGaIAeUzHL50` |
| `&lt;AD_GREETING_TEXT&gt;`&lt;br&gt;&lt;br&gt;_String_ | Click to WhatsApp ad greeting text. | `Hi there! Let us know how we can help!` |
| `&lt;AD_HEADLINE&gt;`&lt;br&gt;&lt;br&gt;_String_ | Click to WhatsApp ad headline. | `Chat with us` |
| `&lt;AD_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | Click to WhatsApp ad ID. | `120226305854810726` |
| `&lt;AD_IMAGE_URL&gt;`&lt;br&gt;&lt;br&gt;_String_ | Click to WhatsApp ad image URL. Only included if the ad is an image ad. | `https://scontent.xx.fbcdn.net/v/t45.1...` |
| `&lt;AD_MEDIA_TYPE&gt;`&lt;br&gt;&lt;br&gt;_String_ | Click to WhatsApp ad media type. Values can be:&lt;br&gt;&lt;br&gt;`image` — Indicates an image ad.&lt;br&gt;&lt;br&gt;`video` — Indicates a video ad. | `image` |
| `&lt;AD_PRIMARY_TEXT&gt;`&lt;br&gt;&lt;br&gt;_String_ | Click to WhatsApp ad primary text. | `Summer succulents are here!` |
| `&lt;AD_URL&gt;`&lt;br&gt;&lt;br&gt;_String_ | Click to WhatsApp ad URL. | `https://fb.me/3cr4Wqqkv` |
| `&lt;AD_VIDEO_THUMBNAIL&gt;`&lt;br&gt;&lt;br&gt;_String_ | Click to WhatsApp ad video thumbnail URL. Only included if ad is a video ad. | `https://scontent.xx.fbcdn.net/v/t45.3...` |
| `&lt;AD_VIDEO_URL&gt;`&lt;br&gt;&lt;br&gt;_String_ | Click to WhatsApp ad video URL. Only included if ad is a video ad. | `https://scontent.xx.fbcdn.net/v/t45.2...` |
| `&lt;BUSINESS_DISPLAY_PHONE_NUMBER&gt;`&lt;br&gt;&lt;br&gt;_String_ | Business display phone number. | `15550783881` |
| `&lt;BUSINESS_PHONE_NUMBER_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | Business phone number ID. | `106540352242922` |
| `&lt;CONTACT_ADDRESS_TYPE&gt;`&lt;br&gt;&lt;br&gt;_String_ | The type of address, such as home or work. | `Home` |
| `&lt;CONTACT_BIRTHDAY&gt;`&lt;br&gt;&lt;br&gt;_String_ | The contact&#039;s birthday. | `1999-01-23` |
| `&lt;CONTACT_CITY&gt;`&lt;br&gt;&lt;br&gt;_String_ | City mentioned in the contact address. | `Menlo Park` |
| `&lt;CONTACT_COUNTRY_CODE&gt;`&lt;br&gt;&lt;br&gt;_String_ | ISO country code on the contact address. | `US` |
| `&lt;CONTACT_COUNTRY&gt;`&lt;br&gt;&lt;br&gt;_String_ | Country mentioned in the contact address. | `United States` |
| `&lt;CONTACT_EMAIL_TYPE&gt;`&lt;br&gt;&lt;br&gt;_String_ | Type of email, such as personal or work. | `Personal` |
| `&lt;CONTACT_EMAIL&gt;`&lt;br&gt;&lt;br&gt;_String_ | Email address of the contact. | `bjohson&#064;socialtsunami.com` |
| `&lt;CONTACT_FIRST_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | Contact&#039;s first name. | `Barbara` |
| `&lt;CONTACT_FORMATTED_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | Contact&#039;s formatted name. | `Barbara J. Johnson` |
| `&lt;CONTACT_LAST_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | Contact&#039;s last name. | `Johnson` |
| `&lt;CONTACT_MIDDLE_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | Contact&#039;s middle name. | `Joana` |
| `&lt;CONTACT_NAME_PREFIX&gt;`&lt;br&gt;&lt;br&gt;_String_ | Contact&#039;s name prefix. | `Dr.` |
| `&lt;CONTACT_NAME_SUFFIX&gt;`&lt;br&gt;&lt;br&gt;_String_ | Contact&#039;s name suffix. | `Esq.` |
| `&lt;CONTACT_ORG_COMPANY&gt;`&lt;br&gt;&lt;br&gt;_String_ | Name of the company where the contact works. | `Social Tsunami` |
| `&lt;CONTACT_ORG_DEPARTMENT&gt;`&lt;br&gt;&lt;br&gt;_String_ | Name of the department where the contact works. | `Engineering` |
| `&lt;CONTACT_ORG_TITLE&gt;`&lt;br&gt;&lt;br&gt;_String_ | Contact&#039;s job title. | `Software Engineer` |
| `&lt;CONTACT_PHONE_TYPE&gt;`&lt;br&gt;&lt;br&gt;_String_ | Type of phone number. For example, cell, mobile, main, iPhone, home, or work. | `CELL` |
| `&lt;CONTACT_PHONE&gt;`&lt;br&gt;&lt;br&gt;_String_ | Contact&#039;s phone number. | `+14125550829` |
| `&lt;CONTACT_STATE&gt;`&lt;br&gt;&lt;br&gt;_String_ | State mentioned in the contact address. | `CA` |
| `&lt;CONTACT_STREET&gt;`&lt;br&gt;&lt;br&gt;_String_ | Street mentioned in the contact address. | `1 Hacker Way` |
| `&lt;CONTACT_URL_TYPE&gt;`&lt;br&gt;&lt;br&gt;_String_ | Type of website. For example, company, work, personal, Facebook Page, or Instagram. | `Company` |
| `&lt;CONTACT_URL&gt;`&lt;br&gt;&lt;br&gt;_String_ | Website URL associated with the contact or their company. | `socialtsunami.com` |
| `&lt;CONTACT_WHATSAPP_PHONE_NUMBER&gt;`&lt;br&gt;&lt;br&gt;_String_ | Contact&#039;s WhatsApp number. | `14125550829` |
| `&lt;CONTACT_ZIP&gt;`&lt;br&gt;&lt;br&gt;_String_ | Zip code in the contact address. | `94025` |
| `&lt;IDENTITY_KEY_HASH&gt;`&lt;br&gt;&lt;br&gt;_String_ | Identity key hash. Only included if you have enabled the [identity change check](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-phone-numbers/phone-numbers) feature. | `DF2lS5v2W6x=` |
| `&lt;WEBHOOK_TRIGGER_TIMESTAMP&gt;`&lt;br&gt;&lt;br&gt;_String_ | Unix timestamp indicating when the webhook was triggered. | `1739321024` |
| `&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | WhatsApp Business Account ID. | `102290129340398` |
| `&lt;WHATSAPP_MESSAGE_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | WhatsApp message ID. | `wamid.HBgLMTY1MDM4Nzk0MzkVAgASGBQzQUFERjg0NDEzNDdFODU3MUMxMAA=` |
| `&lt;WHATSAPP_USER_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | WhatsApp user ID. Note that a WhatsApp user&#039;s ID and phone number may not always match. | `16505551234` |
| `&lt;WHATSAPP_USER_PHONE_NUMBER&gt;`&lt;br&gt;&lt;br&gt;_String_ | WhatsApp user phone number. This is the same value returned by the API as the `input` value when sending a message to a WhatsApp user. Note that a WhatsApp user&#039;s phone number and ID may not always match. | `+16505551234` |
| `&lt;WHATSAPP_USER_PROFILE_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | WhatsApp user&#039;s name as it appears in their profile in the WhatsApp client. | `Sheena Nelson` |

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
                &quot;id&quot;: &quot;wamid.HBgLMTY1MDM4Nzk0MzkVAgASGBQzQTRBNjU5OUFFRTAzODEwMTQ0RgA=&quot;,
                &quot;timestamp&quot;: &quot;1744344496&quot;,
                &quot;type&quot;: &quot;contacts&quot;,
                &quot;contacts&quot;: [
                  &#123;
                    &quot;name&quot;: &#123;
                      &quot;first_name&quot;: &quot;Barbara&quot;,
                      &quot;last_name&quot;: &quot;Johnson&quot;,
                      &quot;formatted_name&quot;: &quot;Barbara J. Johnson&quot;
                    &#125;,
                    &quot;org&quot;: &#123;
                      &quot;company&quot;: &quot;Social Tsunami&quot;
                    &#125;,
                    &quot;phones&quot;: [
                      &#123;
                        &quot;phone&quot;: &quot;+1 (415) 555-0829&quot;,
                        &quot;wa_id&quot;: &quot;14125550829&quot;,
                        &quot;type&quot;: &quot;MOBILE&quot;
                      &#125;
                    ]
                  &#125;
                ]
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
