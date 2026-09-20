# Interactive list messages



Interactive list messages allow you to present WhatsApp users with a list of options to choose from (options are defined as rows in the request payload):

When a user taps the button in the message, WhatsApp displays a modal that lists the available options:

Users can then choose one option, and WhatsApp sends their selection as a reply:

Selecting an option triggers a webhook, which identifies the user&#039;s selected option.

Interactive list messages support up to 10 sections, with up to 10 rows for all sections combined, and can include an optional header and footer.

## Request syntax

Use the [Messages API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/message-api#post-version-phone-number-id-messages) to send an interactive list message to a WhatsApp user.

```html
curl &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;WHATSAPP_BUSINESS_PHONE_NUMBER_ID&gt;/messages&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039; \
-d &#039;
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;recipient_type&quot;: &quot;individual&quot;,
  &quot;to&quot;: &quot;&lt;WHATSAPP_USER_PHONE_NUMBER&gt;&quot;,
  &quot;type&quot;: &quot;interactive&quot;,
  &quot;interactive&quot;: &#123;
    &quot;type&quot;: &quot;list&quot;,
    &quot;header&quot;: &#123;
      &quot;type&quot;: &quot;text&quot;,
      &quot;text&quot;: &quot;&lt;MESSAGE_HEADER_TEXT&gt;&quot;
    &#125;,
    &quot;body&quot;: &#123;
      &quot;text&quot;: &quot;&lt;MESSAGE_BODY_TEXT&gt;&quot;
    &#125;,
    &quot;footer&quot;: &#123;
      &quot;text&quot;: &quot;&lt;MESSAGE_FOOTER_TEXT&gt;&quot;
    &#125;,
    &quot;action&quot;: &#123;
      &quot;button&quot;: &quot;&lt;BUTTON_TEXT&gt;&quot;,
      &quot;sections&quot;: [
        &#123;
          &quot;title&quot;: &quot;&lt;SECTION_TITLE_TEXT&gt;&quot;,
          &quot;rows&quot;: [
            &#123;
              &quot;id&quot;: &quot;&lt;ROW_ID&gt;&quot;,
              &quot;title&quot;: &quot;&lt;ROW_TITLE_TEXT&gt;&quot;,
              &quot;description&quot;: &quot;&lt;ROW_DESCRIPTION_TEXT&gt;&quot;
            &#125;
            &lt;!-- Additional rows would go here --&gt;
          ]
        &#125;
        &lt;!-- Additional sections would go here --&gt;
      ]
    &#125;
  &#125;
&#125;&#039;
```

## Request parameters

| Placeholder | Description | Sample Value |
| --- | --- | --- |
| `&lt;ACCESS_TOKEN&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;[System token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#system-user-access-tokens) or [business token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#business-integration-system-user-access-tokens). | `EAAA...` |
| `&lt;API_VERSION&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Graph API version. | v25.0 |
| `&lt;BUTTON_TEXT&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Button label text. When tapped, reveals rows (options the WhatsApp user can tap). Supports a single button.&lt;br&gt;&lt;br&gt;Maximum 20 characters. | `Shipping Options` |
| `&lt;MESSAGE_BODY_TEXT&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Message body text. Supports URLs.&lt;br&gt;&lt;br&gt;Maximum 4096 characters. | `Which shipping option do you prefer?` |
| `&lt;MESSAGE_FOOTER_TEXT&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Message footer text.&lt;br&gt;&lt;br&gt;Maximum 60 characters. | `Lucky Shrub: Your gateway to succulents™` |
| `&lt;MESSAGE_HEADER_TEXT&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;The `header` object is optional. Supports `text` header type only.&lt;br&gt;&lt;br&gt;Maximum 60 characters. | `Choose Shipping Option` |
| `&lt;ROW_DESCRIPTION_TEXT&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Row description.&lt;br&gt;&lt;br&gt;Maximum 72 characters. | `Next Day to 2 Days` |
| `&lt;ROW_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Arbitrary string identifying the row. This ID will be included in the webhook payload if the user submits the selection.&lt;br&gt;&lt;br&gt;At least one row is required. Supports up to 10 rows.&lt;br&gt;&lt;br&gt;Maximum 200 characters. | `priority_express` |
| `&lt;ROW_TITLE_TEXT&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Row title. At least 1 row is required. Supports up to 10 rows.&lt;br&gt;&lt;br&gt;Maximum 24 characters. | `Priority Mail Express` |
| `&lt;SECTION_TITLE_TEXT&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Section title text. At least 1 section is required. Supports up to 10 sections.&lt;br&gt;&lt;br&gt;Maximum 24 characters. | `I want it ASAP!` |
| `&lt;WHATSAPP_BUSINESS_PHONE_NUMBER_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;WhatsApp business phone number ID. | `106540352242922` |
| `&lt;WHATSAPP_USER_PHONE_NUMBER&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;WhatsApp user phone number. | `+16505551234` |

## Example request

Example request to send an interactive list message with a header, body, footer, and two sections containing two rows each.

```html
curl &#039;https://graph.facebook.com/v25.0/106540352242922/messages&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039; \
-d &#039;
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;recipient_type&quot;: &quot;individual&quot;,
  &quot;to&quot;: &quot;+16505551234&quot;,
  &quot;type&quot;: &quot;interactive&quot;,
  &quot;interactive&quot;: &#123;
    &quot;type&quot;: &quot;list&quot;,
    &quot;header&quot;: &#123;
      &quot;type&quot;: &quot;text&quot;,
      &quot;text&quot;: &quot;Choose Shipping Option&quot;
    &#125;,
    &quot;body&quot;: &#123;
      &quot;text&quot;: &quot;Which shipping option do you prefer?&quot;
    &#125;,
    &quot;footer&quot;: &#123;
      &quot;text&quot;: &quot;Lucky Shrub: Your gateway to succulents™&quot;
    &#125;,
    &quot;action&quot;: &#123;
      &quot;button&quot;: &quot;Shipping Options&quot;,
      &quot;sections&quot;: [
        &#123;
          &quot;title&quot;: &quot;I want it ASAP!&quot;,
          &quot;rows&quot;: [
            &#123;
              &quot;id&quot;: &quot;priority_express&quot;,
              &quot;title&quot;: &quot;Priority Mail Express&quot;,
              &quot;description&quot;: &quot;Next Day to 2 Days&quot;
            &#125;,
            &#123;
              &quot;id&quot;: &quot;priority_mail&quot;,
              &quot;title&quot;: &quot;Priority Mail&quot;,
              &quot;description&quot;: &quot;1–3 Days&quot;
            &#125;
          ]
        &#125;,
        &#123;
          &quot;title&quot;: &quot;I can wait a bit&quot;,
          &quot;rows&quot;: [
            &#123;
              &quot;id&quot;: &quot;usps_ground_advantage&quot;,
              &quot;title&quot;: &quot;USPS Ground Advantage&quot;,
              &quot;description&quot;: &quot;2–5 Days&quot;
            &#125;,
            &#123;
              &quot;id&quot;: &quot;media_mail&quot;,
              &quot;title&quot;: &quot;Media Mail&quot;,
              &quot;description&quot;: &quot;2–8 Days&quot;
            &#125;
          ]
        &#125;
      ]
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
      &quot;id&quot;: &quot;wamid.HBgLMTY0NjcwNDM1OTUVAgARGBI1RjQyNUE3NEYxMzAzMzQ5MkEA&quot;
    &#125;
  ]
&#125;
```


## Webhooks

When a WhatsApp user selects an option and sends their message, WhatsApp triggers a **messages** webhook identifying the ID (`id`) of the option they chose.

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
                  &quot;id&quot;: &quot;wamid.HBgLMTY0NjcwNDM1OTUVAgARGBIwMjg0RkMxOEMyMkNEQUFFRDgA&quot;
                &#125;,
                &quot;from&quot;: &quot;16505551234&quot;,
                &quot;id&quot;: &quot;wamid.HBgLMTY0NjcwNDM1OTUVAgASGBQzQTZDMzFGRUFBQjlDMzIzMzlEQwA=&quot;,
                &quot;timestamp&quot;: &quot;1712595443&quot;,
                &quot;type&quot;: &quot;interactive&quot;,
                &quot;interactive&quot;: &#123;
                  &quot;type&quot;: &quot;list_reply&quot;,
                  &quot;list_reply&quot;: &#123;
                    &quot;id&quot;: &quot;priority_express&quot;,
                    &quot;title&quot;: &quot;Priority Mail Express&quot;,
                    &quot;description&quot;: &quot;Next Day to 2 Days&quot;
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
