# Location messages webhook reference



This reference describes trigger events and payload contents for the WhatsApp Business Account **messages** webhook for messages containing location information.

## Triggers

- A WhatsApp user sends a location message to a business.
- A WhatsApp user sends a location to a business via a Click to WhatsApp ad.

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
                &quot;location&quot;: &#123;
                  &quot;address&quot;: &quot;&lt;LOCATION_ADDRESS&gt;&quot;,
                  &quot;latitude&quot;: &lt;LOCATION_LATITUDE&gt;,
                  &quot;longitude&quot;: &lt;LOCATION_LONGITUDE&gt;,
                  &quot;name&quot;: &quot;&lt;LOCATION_NAME&gt;&quot;,
                  &quot;url&quot;: &quot;&lt;LOCATION_URL&gt;&quot;
                &#125;,
                &quot;type&quot;: &quot;location&quot;,

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
| `&lt;IDENTITY_KEY_HASH&gt;`&lt;br&gt;&lt;br&gt;_String_ | Identity key hash. Only included if you have enabled the [identity change check](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-phone-numbers/phone-numbers) feature. | `DF2lS5v2W6x=` |
| `&lt;LOCATION_ADDRESS&gt;`&lt;br&gt;&lt;br&gt;_String_ | Location address. | `101 Forest Ave, Palo Alto, CA 94301` |
| `&lt;LOCATION_LATITUDE&gt;`&lt;br&gt;&lt;br&gt;_Float_ | Location latitude in decimal degrees. | `37.44221496582` |
| `&lt;LOCATION_LONGITUDE&gt;`&lt;br&gt;&lt;br&gt;_Float_ | Location longitude in decimal degrees. | `-122.16165924072` |
| `&lt;LOCATION_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | Location name. | `Philz Coffee` |
| `&lt;LOCATION_URL&gt;`&lt;br&gt;&lt;br&gt;_String_ | Location URL. Usually only included for business locations. | `https://philzcoffee.com/` |
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
                &quot;id&quot;: &quot;wamid.HBgLMTY1MDM4Nzk0MzkVAgASGBQzQUFERjg0NDEzNDdFODU3MUMxMAA=&quot;,
                &quot;timestamp&quot;: &quot;1744344496&quot;,
                &quot;location&quot;: &#123;
                  &quot;address&quot;: &quot;101 Forest Ave, Palo Alto, CA 94301&quot;,
                  &quot;latitude&quot;: 37.44221496582,
                  &quot;longitude&quot;: -122.16165924072,
                  &quot;name&quot;: &quot;Philz Coffee&quot;,
                  &quot;url&quot;: &quot;https://philzcoffee.com/&quot;
                &#125;,
                &quot;type&quot;: &quot;location&quot;
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
