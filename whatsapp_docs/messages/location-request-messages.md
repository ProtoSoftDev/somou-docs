# Location request messages



Location request messages display **body text** and a **send location button**. When a WhatsApp user taps the button, a location sharing screen appears, which the user can then use to share their location.

Once the user shares their location, a **messages** webhook is triggered, containing the user&#039;s location details.

## Request syntax

Use the [Messages API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/message-api#post-version-phone-number-id-messages) to send a location request message to a WhatsApp user.

```html
curl &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;WHATSAPP_BUSINESS_PHONE_NUMBER_ID&gt;/messages&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039; \
-d &#039;
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;recipient_type&quot;: &quot;individual&quot;,
  &quot;type&quot;: &quot;interactive&quot;,
  &quot;to&quot;: &quot;&lt;WHATSAPP_USER_PHONE_NUMBER&gt;&quot;,
  &quot;interactive&quot;: &#123;
    &quot;type&quot;: &quot;location_request_message&quot;,
    &quot;body&quot;: &#123;
      &quot;text&quot;: &quot;&lt;BODY_TEXT&gt;&quot;
    &#125;,
    &quot;action&quot;: &#123;
      &quot;name&quot;: &quot;send_location&quot;
    &#125;
  &#125;
&#125;&#039;
```

## Request parameters

| Placeholder | Description | Example Value |
| --- | --- | --- |
| `&lt;ACCESS_TOKEN&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;[System token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#system-user-access-tokens) or [business token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#business-integration-system-user-access-tokens). | `EAAA...` |
| `&lt;API_VERSION&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Graph API version. | v25.0 |
| `&lt;BODY_TEXT&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Message body text. Supports URLs.&lt;br&gt;&lt;br&gt;Maximum 1024 characters. | `Let&#039;s start with your pickup. You can either manually *enter an address* or *share your current location*.` |
| `&lt;WHATSAPP_BUSINESS_PHONE_NUMBER_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;WhatsApp business phone number ID. | `106540352242922` |
| `&lt;WHATSAPP_USER_PHONE_NUMBER&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;WhatsApp user phone number. | `+16505551234` |

## Webhook syntax

When a WhatsApp user shares their location in response to your message, a **messages** webhook is triggered containing the user&#039;s location details.

```json
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
              &quot;display_phone_number&quot;: &quot;&lt;WHATSAPP_BUSINESS_DISPLAY_PHONE_NUMBER&gt;&quot;,
              &quot;phone_number_id&quot;: &quot;&lt;WHATSAPP_BUSINESS_PHONE_NUMBER_ID&gt;&quot;
            &#125;,
            &quot;contacts&quot;: [
              &#123;
                &quot;profile&quot;: &#123;
                  &quot;name&quot;: &quot;&lt;WHATSAPP_USER_NAME&gt;&quot;
                &#125;,
                &quot;wa_id&quot;: &quot;&lt;WHATSAPP_USER_ID&gt;&quot;
              &#125;
            ],
            &quot;messages&quot;: [
              &#123;
                &quot;context&quot;: &#123;
                  &quot;from&quot;: &quot;&lt;WHATSAPP_BUSINESS_PHONE_NUMBER&gt;&quot;,
                  &quot;id&quot;: &quot;&lt;WHATSAPP_CONTEXT_MESSAGE_ID&gt;&quot;
                &#125;,
                &quot;from&quot;: &quot;&lt;WHATSAPP_USER_ID&gt;&quot;,
                &quot;id&quot;: &quot;&lt;WHATSAPP_MESSAGE_ID&gt;&quot;,
                &quot;timestamp&quot;: &quot;&lt;TIMESTAMP&gt;&quot;,
                &quot;location&quot;: &#123;
                  &quot;address&quot;: &quot;&lt;LOCATION_ADDRESS&gt;&quot;,
                  &quot;latitude&quot;: &lt;LOCATION_LATITUDE&gt;,
                  &quot;longitude&quot;: &lt;LOCATION_LONGITUDE&gt;,
                  &quot;name&quot;: &quot;&lt;LOCATION_NAME&gt;&quot;
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

## Webhook parameters

| Placeholder | Description | Example Value |
| --- | --- | --- |
| `&lt;LOCATION_ADDRESS&gt;`&lt;br&gt;&lt;br&gt;_String_ | Location address.&lt;br&gt;&lt;br&gt;This parameter appears only if the WhatsApp user chooses to share it. | `1071 5th Ave, New York, NY 10128` |
| `&lt;LOCATION_LATITUDE&gt;`&lt;br&gt;&lt;br&gt;_Number_ | Location latitude in decimal degrees. | `40.782910059774` |
| `&lt;LOCATION_LONGITUDE&gt;`&lt;br&gt;&lt;br&gt;_Number_ | Location longitude in decimal degrees. | `-73.959075808525` |
| `&lt;LOCATION_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | Location name.&lt;br&gt;&lt;br&gt;This parameter appears only if the WhatsApp user chooses to share it. | `Solomon R. Guggenheim Museum` |
| `&lt;TIMESTAMP&gt;`&lt;br&gt;&lt;br&gt;_String_ | UNIX timestamp indicating when our servers processed the WhatsApp user&#039;s message. | `1702920965` |
| `&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | WhatsApp Business account ID. | `102290129340398` |
| `&lt;WHATSAPP_BUSINESS_DISPLAY_PHONE_NUMBER&gt;`&lt;br&gt;&lt;br&gt;_String_ | WhatsApp Business phone number&#039;s display number. | `15550783881` |
| `&lt;WHATSAPP_BUSINESS_PHONE_NUMBER&gt;`&lt;br&gt;&lt;br&gt;_String_ | WhatsApp Business phone number. | `15550783881` |
| `&lt;WHATSAPP_BUSINESS_PHONE_NUMBER_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | WhatsApp Business phone number ID. | `106540352242922` |
| `&lt;WHATSAPP_CONTEXT_MESSAGE_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | WhatsApp message ID of message that the user is responding to. | `wamid.HBgLMTY0NjcwNDM1OTUVAgARGBI1QjJGRjI1RDY0RkE4Nzg4QzcA` |
| `&lt;WHATSAPP_MESSAGE_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | WhatsApp message ID of the user&#039;s message. | `wamid.HBgLMTY0NjcwNDM1OTUVAgASGBQzQTRCRDcwNzgzMTRDNTAwRTgwRQA=` |
| `&lt;WHATSAPP_USER_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | WhatsApp user&#039;s WhatsApp ID. | `16505551234` |
| `&lt;WHATSAPP_USER_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | WhatsApp user&#039;s name. | `Pablo Morales` |

## Example request

```curl
curl &#039;https://graph.facebook.com/v25.0/106540352242922/messages&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039; \
-d &#039;
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;recipient_type&quot;: &quot;individual&quot;,
  &quot;type&quot;: &quot;interactive&quot;,
  &quot;to&quot;: &quot;+16505551234&quot;,
  &quot;interactive&quot;: &#123;
    &quot;type&quot;: &quot;location_request_message&quot;,
    &quot;body&quot;: &#123;
      &quot;text&quot;: &quot;Let&#039;\&#039;&#039;s start with your pickup. You can either manually *enter an address* or *share your current location*.&quot;
    &#125;,
    &quot;action&quot;: &#123;
      &quot;name&quot;: &quot;send_location&quot;
    &#125;
  &#125;
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
      &quot;id&quot;: &quot;wamid.HBgLMTY0NjcwNDM1OTUVAgARGBJCNUQ5RUNBNTk3OEQ2M0ZEQzgA&quot;
    &#125;
  ]
&#125;
```

## Example webhook

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
                  &quot;name&quot;: &quot;Pablo Morales&quot;
                &#125;,
                &quot;wa_id&quot;: &quot;16505551234&quot;
              &#125;
            ],
            &quot;messages&quot;: [
              &#123;
                &quot;context&quot;: &#123;
                  &quot;from&quot;: &quot;15550783881&quot;,
                  &quot;id&quot;: &quot;wamid.HBgLMTY0NjcwNDM1OTUVAgARGBI1QjJGRjI1RDY0RkE4Nzg4QzcA&quot;
                &#125;,
                &quot;from&quot;: &quot;16505551234&quot;,
                &quot;id&quot;: &quot;wamid.HBgLMTY0NjcwNDM1OTUVAgASGBQzQTRCRDcwNzgzMTRDNTAwRTgwRQA=&quot;,
                &quot;timestamp&quot;: &quot;1702920965&quot;,
                &quot;location&quot;: &#123;
                  &quot;address&quot;: &quot;1071 5th Ave, New York, NY 10128&quot;,
                  &quot;latitude&quot;: 40.782910059774,
                  &quot;longitude&quot;: -73.959075808525,
                  &quot;name&quot;: &quot;Solomon R. Guggenheim Museum&quot;
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
