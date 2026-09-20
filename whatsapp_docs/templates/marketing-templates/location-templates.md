# Location templates



You can send marketing location template messages that include a map header that displays a specific location. When a WhatsApp user taps the map, their default map app opens to those coordinates. Location templates are useful for promoting store openings, event invitations, pop-up shops, and other location-based promotions.

Categorize a location template as either `MARKETING` or `UTILITY`. This page demonstrates creating and sending a location template with the `MARKETING` category. See [utility location templates](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/utility-templates/location-templates/) for a utility example.

WhatsApp does not support real-time locations. Specify the location at send time, not at template creation time.

## Limitations

- Only templates categorized as `UTILITY` or `MARKETING` can include a location header
- Real-time locations are not supported
- The location (latitude, longitude, name, address) is specified at send time, not at template creation time

## Create a location template

Use the [Message Templates API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/message-template-api) to [create a location template](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/message-template-api#post-version-waba-id-message-templates).

### Supported components

Location templates support the following components:

- 1 location header (**required**)
- 1 body (**required**; supports named parameters)
- 1 footer (optional)
- Buttons (optional)

### Request syntax

```bash
curl -X POST \
  &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;/message_templates&#039; \
  -H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039; \
  -H &#039;Content-Type: application/json&#039; \
  -d &#039;&#123;
    &quot;name&quot;: &quot;&lt;TEMPLATE_NAME&gt;&quot;,
    &quot;language&quot;: &quot;&lt;TEMPLATE_LANGUAGE&gt;&quot;,
    &quot;category&quot;: &quot;MARKETING&quot;,
    &quot;parameter_format&quot;: &quot;named&quot;,
    &quot;components&quot;: [
      &#123;
        &quot;type&quot;: &quot;header&quot;,
        &quot;format&quot;: &quot;location&quot;
      &#125;,
      &#123;
        &quot;type&quot;: &quot;body&quot;,
        &quot;text&quot;: &quot;&lt;BODY_TEXT&gt;&quot;,
        &quot;example&quot;: &#123;
          &quot;body_text_named_params&quot;: [
            &#123;
              &quot;param_name&quot;: &quot;&lt;BODY_PARAM_NAME&gt;&quot;,
              &quot;example&quot;: &quot;&lt;BODY_PARAM_EXAMPLE&gt;&quot;
            &#125;
          ]
        &#125;
      &#125;,
      &#123;
        &quot;type&quot;: &quot;footer&quot;,
        &quot;text&quot;: &quot;&lt;FOOTER_TEXT&gt;&quot;
      &#125;
    ]
&#125;&#039;
```

### Request parameters

| Placeholder | Description | Example Value |
| --- | --- | --- |
| `&lt;ACCESS_TOKEN&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;[System token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#system-user-access-tokens) or [business token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#business-integration-system-user-access-tokens). | `EAAA...` |
| `&lt;API_VERSION&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Graph API version. | v25.0 |
| `&lt;BODY_PARAM_EXAMPLE&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required if body text contains named parameters.**&lt;br&gt;&lt;br&gt;Example value for the named parameter. You must supply one example for each parameter in your body text. | `Lisa` |
| `&lt;BODY_PARAM_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required if body text contains named parameters.**&lt;br&gt;&lt;br&gt;Name of the parameter, matching the placeholder in the body text. | `customer_name` |
| `&lt;BODY_TEXT&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Body text string. Supports named parameters in `&#123;&#123;parameter_name&#125;&#125;` format.&lt;br&gt;&lt;br&gt;Maximum 1024 characters. | `Hi &#123;&#123;customer_name&#125;&#125;! We are opening a new store near you. Visit us on opening day for &#123;&#123;discount&#125;&#125; off your first purchase!` |
| `&lt;FOOTER_TEXT&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Footer text. Maximum 60 characters. | `Reply STOP to unsubscribe.` |
| `&lt;TEMPLATE_LANGUAGE&gt;`&lt;br&gt;&lt;br&gt;_Enum_ | **Required.**&lt;br&gt;&lt;br&gt;Template [language and locale code](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/supported-languages). | `en_US` |
| `&lt;TEMPLATE_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Template name.&lt;br&gt;&lt;br&gt;Maximum 512 characters. | `store_grand_opening` |
| `&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;WhatsApp Business account ID. | `106540352242922` |

### Example request

Create a marketing template with a location header, body with named parameters, footer, and a quick reply button:


```bash
curl -X POST \
  &#039;https://graph.facebook.com/v25.0/106540352242922/message_templates&#039; \
  -H &#039;Authorization: Bearer EAAJB...&#039; \
  -H &#039;Content-Type: application/json&#039; \
  -d &#039;&#123;
    &quot;name&quot;: &quot;store_grand_opening&quot;,
    &quot;language&quot;: &quot;en_US&quot;,
    &quot;category&quot;: &quot;MARKETING&quot;,
    &quot;parameter_format&quot;: &quot;named&quot;,
    &quot;components&quot;: [
      &#123;
        &quot;type&quot;: &quot;HEADER&quot;,
        &quot;format&quot;: &quot;LOCATION&quot;
      &#125;,
      &#123;
        &quot;type&quot;: &quot;BODY&quot;,
        &quot;text&quot;: &quot;Hi &#123;&#123;customer_name&#125;&#125;! We are opening a new store near you. Visit us on opening day for &#123;&#123;discount&#125;&#125; off your first purchase!&quot;,
        &quot;example&quot;: &#123;
          &quot;body_text_named_params&quot;: [
            &#123;
              &quot;param_name&quot;: &quot;customer_name&quot;,
              &quot;example&quot;: &quot;Lisa&quot;
            &#125;,
            &#123;
              &quot;param_name&quot;: &quot;discount&quot;,
              &quot;example&quot;: &quot;20%&quot;
            &#125;
          ]
        &#125;
      &#125;,
      &#123;
        &quot;type&quot;: &quot;FOOTER&quot;,
        &quot;text&quot;: &quot;Reply STOP to unsubscribe.&quot;
      &#125;,
      &#123;
        &quot;type&quot;: &quot;BUTTONS&quot;,
        &quot;buttons&quot;: [
          &#123;
            &quot;type&quot;: &quot;QUICK_REPLY&quot;,
            &quot;text&quot;: &quot;Unsubscribe from Promos&quot;
          &#125;
        ]
      &#125;
    ]
&#125;&#039;
```


### Example response

```json
&#123;
  &quot;id&quot;: &quot;546151681022936&quot;,
  &quot;status&quot;: &quot;PENDING&quot;,
  &quot;category&quot;: &quot;MARKETING&quot;
&#125;
```

## Send a location template

Call the [Messages API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/message-api) to [send an approved location template](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/message-api#post-version-phone-number-id-messages) in a template message. You must specify the location coordinates at send time in the header component.

### Request syntax

```bash
curl -X POST \
  &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;WHATSAPP_BUSINESS_PHONE_NUMBER_ID&gt;/messages&#039; \
  -H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039; \
  -H &#039;Content-Type: application/json&#039; \
  -d &#039;&#123;
    &quot;messaging_product&quot;: &quot;whatsapp&quot;,
    &quot;recipient_type&quot;: &quot;individual&quot;,
    &quot;to&quot;: &quot;&lt;WHATSAPP_USER_PHONE_NUMBER&gt;&quot;,
    &quot;type&quot;: &quot;template&quot;,
    &quot;template&quot;: &#123;
      &quot;name&quot;: &quot;&lt;TEMPLATE_NAME&gt;&quot;,
      &quot;language&quot;: &#123;
        &quot;policy&quot;: &quot;deterministic&quot;,
        &quot;code&quot;: &quot;&lt;TEMPLATE_LANGUAGE_CODE&gt;&quot;
      &#125;,
      &quot;components&quot;: [
        &#123;
          &quot;type&quot;: &quot;header&quot;,
          &quot;parameters&quot;: [
            &#123;
              &quot;type&quot;: &quot;location&quot;,
              &quot;location&quot;: &#123;
                &quot;latitude&quot;: &quot;&lt;LOCATION_LATITUDE&gt;&quot;,
                &quot;longitude&quot;: &quot;&lt;LOCATION_LONGITUDE&gt;&quot;,
                &quot;name&quot;: &quot;&lt;LOCATION_NAME&gt;&quot;,
                &quot;address&quot;: &quot;&lt;LOCATION_ADDRESS&gt;&quot;
              &#125;
            &#125;
          ]
        &#125;,
        &#123;
          &quot;type&quot;: &quot;body&quot;,
          &quot;parameters&quot;: [
            &#123;
              &quot;type&quot;: &quot;text&quot;,
              &quot;parameter_name&quot;: &quot;&lt;BODY_PARAM_NAME&gt;&quot;,
              &quot;text&quot;: &quot;&lt;BODY_PARAM_VALUE&gt;&quot;
            &#125;
          ]
        &#125;
      ]
    &#125;
&#125;&#039;
```

### Request parameters

| Placeholder | Description | Example Value |
| --- | --- | --- |
| `&lt;ACCESS_TOKEN&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;[System token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#system-user-access-tokens) or [business token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#business-integration-system-user-access-tokens). | `EAAA...` |
| `&lt;API_VERSION&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Graph API version. | v25.0 |
| `&lt;BODY_PARAM_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required if the template body uses named parameters.**&lt;br&gt;&lt;br&gt;Name of the parameter to replace in the template body. | `customer_name` |
| `&lt;BODY_PARAM_VALUE&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required if the template body uses named parameters.**&lt;br&gt;&lt;br&gt;Value to substitute for the named parameter. | `Maria` |
| `&lt;LOCATION_ADDRESS&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Location address. | `3250 Ocean Park Blvd, Santa Monica, CA 90405` |
| `&lt;LOCATION_LATITUDE&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Location latitude in decimal degrees. | `34.01881798498779` |
| `&lt;LOCATION_LONGITUDE&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Location longitude in decimal degrees. | `-118.46708679200001` |
| `&lt;LOCATION_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Location name. | `Lucky Shrub - Santa Monica` |
| `&lt;TEMPLATE_LANGUAGE_CODE&gt;`&lt;br&gt;&lt;br&gt;_Enum_ | **Required.**&lt;br&gt;&lt;br&gt;Template [language and locale code](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/supported-languages). | `en_US` |
| `&lt;TEMPLATE_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Name of the template to send. | `store_grand_opening` |
| `&lt;WHATSAPP_BUSINESS_PHONE_NUMBER_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;WhatsApp business phone number ID. | `106540352242922` |
| `&lt;WHATSAPP_USER_PHONE_NUMBER&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;WhatsApp user phone number. | `+16505551234` |

### Example request

Send the template [created in the example request above](#example-request). You provide the location coordinates and body parameter values at send time. Note that the send-time values differ from the creation-time example values to demonstrate that they are independent.

```bash
curl -X POST \
  &#039;https://graph.facebook.com/v25.0/106540352242922/messages&#039; \
  -H &#039;Authorization: Bearer EAAJB...&#039; \
  -H &#039;Content-Type: application/json&#039; \
  -d &#039;&#123;
    &quot;messaging_product&quot;: &quot;whatsapp&quot;,
    &quot;recipient_type&quot;: &quot;individual&quot;,
    &quot;to&quot;: &quot;+16505551234&quot;,
    &quot;type&quot;: &quot;template&quot;,
    &quot;template&quot;: &#123;
      &quot;name&quot;: &quot;store_grand_opening&quot;,
      &quot;language&quot;: &#123;
        &quot;policy&quot;: &quot;deterministic&quot;,
        &quot;code&quot;: &quot;en_US&quot;
      &#125;,
      &quot;components&quot;: [
        &#123;
          &quot;type&quot;: &quot;header&quot;,
          &quot;parameters&quot;: [
            &#123;
              &quot;type&quot;: &quot;location&quot;,
              &quot;location&quot;: &#123;
                &quot;latitude&quot;: &quot;34.01881798498779&quot;,
                &quot;longitude&quot;: &quot;-118.46708679200001&quot;,
                &quot;name&quot;: &quot;Lucky Shrub - Santa Monica&quot;,
                &quot;address&quot;: &quot;3250 Ocean Park Blvd, Santa Monica, CA 90405&quot;
              &#125;
            &#125;
          ]
        &#125;,
        &#123;
          &quot;type&quot;: &quot;body&quot;,
          &quot;parameters&quot;: [
            &#123;
              &quot;type&quot;: &quot;text&quot;,
              &quot;parameter_name&quot;: &quot;customer_name&quot;,
              &quot;text&quot;: &quot;Maria&quot;
            &#125;,
            &#123;
              &quot;type&quot;: &quot;text&quot;,
              &quot;parameter_name&quot;: &quot;discount&quot;,
              &quot;text&quot;: &quot;15%&quot;
            &#125;
          ]
        &#125;
      ]
    &#125;
&#125;&#039;
```

### Example response

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

