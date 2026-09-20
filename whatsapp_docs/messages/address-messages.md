# Address Messages



**Warning:** This feature is only available for businesses based in India and their India customers.

Address messages give your users a simpler way to share the shipping address with the business on WhatsApp.

Address messages are interactive messages that contain the four main parts: `header`, `body`, `footer`, and `action`. Inside the action component, the business specifies the name &quot;address_message&quot; and relevant parameters.

The following table outlines the fields that are supported by the address message.

| Field Name | Display Label | Input Type | Supported Countries | Limitations |
| --- | --- | --- | --- | --- |
| `name` | Name | text | India | None |
| `phone_number` | Phone Number | tel | India | Valid phone numbers only |
| `in_pin_code` | Pin Code | text | India | Max length: 6 |
| `house_number` | Flat/House Number | text | India | None |
| `floor_number` | Floor Number | text | India | None |
| `tower_number` | Tower Number | text | India | None |
| `building_name` | Building/Apartment Name | text | India | None |
| `address` | Address | text | India | None |
| `landmark_area` | Landmark/Area | text | India | None |
| `city` | City | text | India | None |
| `state` | State | text | India | None |

## Sample API call

This is a sample API call for the address message. The `country` attribute is a mandatory field in the action parameters. If the country attribute is not included, there will be a validation error.

```html
curl -X  POST \
&#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;FROM_PHONE_NUMBER_ID&gt;/messages&#039; \
-H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039; \
-H &#039;Content-Type: application/json&#039; \
-d &#039;
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;recipient_type&quot;: &quot;individual&quot;,
  &quot;to&quot;: &quot;&lt;PHONE_NUMBER&gt;&quot;,
  &quot;type&quot;: &quot;interactive&quot;,
  &quot;interactive&quot;: &#123;
    &quot;type&quot;: &quot;address_message&quot;,
    &quot;body&quot;: &#123;
      &quot;text&quot;: &quot;Thanks for your order! Tell us what address you&#039;d like this order delivered to.&quot;
    &#125;,
    &quot;action&quot;: &#123;
      &quot;name&quot;: &quot;address_message&quot;,
      &quot;parameters&quot;: &#123;
        &quot;country&quot;: &quot;&lt;COUNTRY_ISO_CODE&gt;&quot;
      &#125;
    &#125;
  &#125;
&#125;&#039;
```

## Error handling

If the area code of the phone number for the given country is not correct, businesses will be unable to request the address message from the recipient. For example, businesses will be unable to request an address message from a recipient that has the country as &quot;India&quot; but has a phone number with an area code of &quot;65&quot;.

Once the address message is sent, the business waits for the user to fill in the address and send it back. The [webhook](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/overview) registered in the [setup process](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/overview) shares the address the user entered.

## Address message steps

The steps involved in an Address Message are the following:

1. Business sends an address message with the action name `address_message` to the user.
2. User interacts with the message by clicking on the CTA, which brings up an Address Message screen. The user fills out their address and submits the form.
3. After the user submits the address message form, the partner receives a webhook notification, which contains the details of the address the user submitted.

**Sample India Address Message **

The following sequence diagram shows a typical integration flow for an address message.

## Additional action parameters

The business can pass additional attributes such as `values`, `validation_errors`, or `saved_addresses` as part of the interactive action parameters. You can find information on each of their usage below.

| Action Parameter | Usage |
| --- | --- |
| `values` | Businesses prefill this for address fields (for example, prefilling the city address field with &quot;India&quot;) |
| `saved_addresses` | Businesses can pass in saved addresses previously associated with the user.&lt;br&gt;&lt;br&gt;For users, they are presented with the option to choose the saved address instead of manually filling it in |
| `validation_errors` | Businesses can throw errors in the address fields and WhatsApp will prevent the user from submitting the address until all issues are resolved. |

### Send an address message to a user

Use the [Messages API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/message-api#post-version-phone-number-id-messages) to send an end-to-end encrypted address message to the user:

```html
curl -X  POST \
&#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;FROM_PHONE_NUMBER_ID&gt;/messages&#039; \
-H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039; \
-H &#039;Content-Type: application/json&#039; \
-d &#039;
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;recipient_type&quot;: &quot;individual&quot;,
  &quot;to&quot;: &quot;&lt;PHONE_NUMBER&gt;&quot;,
  &quot;type&quot;: &quot;interactive&quot;,
  &quot;interactive&quot;: &#123;
    &quot;type&quot;: &quot;address_message&quot;,
    &quot;body&quot;: &#123;
      &quot;text&quot;: &quot;Thanks for your order! Tell us what address you&#039;d like this order delivered to.&quot;
    &#125;,
    &quot;action&quot;: &#123;
      &quot;name&quot;: &quot;address_message&quot;,
      &quot;parameters&quot;: &quot;JSON Payload&quot;
    &#125;
  &#125;
&#125;&#039;
```

To send an address message without any saved addresses, WhatsApp will prompt the user or business with an address form to enter a new address.

```html
curl -X  POST \
&#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;FROM_PHONE_NUMBER_ID&gt;/messages&#039; \
-H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039; \
-H &#039;Content-Type: application/json&#039; \
-d &#039;
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;recipient_type&quot;: &quot;individual&quot;,
  &quot;to&quot;: &quot;+91xxxxxxxxxx&quot;,
  &quot;type&quot;: &quot;interactive&quot;,
  &quot;interactive&quot;: &#123;
    &quot;type&quot;: &quot;address_message&quot;,
    &quot;body&quot;: &#123;
      &quot;text&quot;: &quot;Thanks for your order! Tell us what address you&#039;d like this order delivered to.&quot;
    &#125;,
    &quot;action&quot;: &#123;
      &quot;name&quot;: &quot;address_message&quot;,
      &quot;parameters&quot;: &#123;
        &quot;country&quot;: &quot;IN&quot;,
        &quot;values&quot;: &#123;
          &quot;name&quot;: &quot;&lt;CUSTOMER_NAME&gt;&quot;,
          &quot;phone_number&quot;: &quot;+91xxxxxxxxxx&quot;
        &#125;
      &#125;
    &#125;
  &#125;
&#125;&#039;
```

To send an address message with saved addresses, WhatsApp will prompt the user or business with an option to select among the saved addresses or add an address option. Users can ignore the saved address and enter a new address.

```html
curl -X  POST \
&#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;FROM_PHONE_NUMBER_ID&gt;/messages&#039; \
-H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039; \
-H &#039;Content-Type: application/json&#039; \
-d &#039;
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;recipient_type&quot;: &quot;individual&quot;,
  &quot;to&quot;: &quot;91xxxxxxxxxx&quot;,
  &quot;type&quot;: &quot;interactive&quot;,
  &quot;interactive&quot;: &#123;
    &quot;type&quot;: &quot;address_message&quot;,
    &quot;body&quot;: &#123;
      &quot;text&quot;: &quot;Thanks for your order! Tell us what address you&#039;d like this order delivered to.&quot;
    &#125;,
    &quot;action&quot;: &#123;
      &quot;name&quot;: &quot;address_message&quot;,
      &quot;parameters&quot;: &#123;
        &quot;country&quot;: &quot;IN&quot;,
        &quot;saved_addresses&quot;: [
          &#123;
            &quot;id&quot;: &quot;address1&quot;,
            &quot;value&quot;: &#123;
              &quot;name&quot;: &quot;&lt;CUSTOMER_NAME&gt;&quot;,
              &quot;phone_number&quot;: &quot;+91xxxxxxxxxx&quot;,
              &quot;in_pin_code&quot;: &quot;400063&quot;,
              &quot;floor_number&quot;: &quot;8&quot;,
              &quot;building_name&quot;: &quot;&quot;,
              &quot;address&quot;: &quot;Wing A, Cello Triumph,IB Patel Rd&quot;,
              &quot;landmark_area&quot;: &quot;Goregaon&quot;,
              &quot;city&quot;: &quot;Mumbai&quot;
            &#125;
          &#125;
        ]
      &#125;
    &#125;
  &#125;
&#125;&#039;
```

## Check your response &#123;#response&#125;

A successful response includes a `messages` object with an ID for the newly created message.

```json
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;contacts&quot;: [
    &#123;
      &quot;input&quot;: &quot;&lt;PHONE_NUMBER&gt;&quot;,
      &quot;wa_id&quot;: &quot;&lt;WHATSAPP_ID&gt;&quot;
    &#125;
  ],
  &quot;messages&quot;: [
    &#123;
      &quot;id&quot;: &quot;wamid.ID&quot;
    &#125;
  ]
&#125;
```

An unsuccessful response contains an error message. See [Error and Status Codes](https://developers.facebook.com/documentation/business-messaging/whatsapp/support/error-codes) for more information.

## Send an address message with validation errors

Re-send the address message to the user in the case of a validation error on the business server. The business should send back the set of values previously entered by the user, along with the respective validation errors for each invalid field, as shown in the sample payloads below.

```html
curl -X  POST \
&#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;FROM_PHONE_NUMBER_ID&gt;/messages&#039; \
-H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039; \
-H &#039;Content-Type: application/json&#039; \
-d
&#039;&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;recipient_type&quot;: &quot;individual&quot;,
  &quot;to&quot;: &quot;91xxxxxxxxxx&quot;,
  &quot;type&quot;: &quot;interactive&quot;,
  &quot;interactive&quot;: &#123;
    &quot;type&quot;: &quot;address_message&quot;,
    &quot;body&quot;: &#123;
      &quot;text&quot;: &quot;Thanks for your order! Tell us what address you&#039;d like this order delivered to.&quot;
    &#125;,
    &quot;action&quot;: &#123;
      &quot;name&quot;: &quot;address_message&quot;,
      &quot;parameters&quot;: &#123;
          &quot;country&quot;: &quot;IN&quot;,
          &quot;values&quot;: &#123;
             &quot;name&quot;: &quot;CUSTOMER_NAME&quot;,
             &quot;phone_number&quot;: &quot;+91xxxxxxxxxx&quot;,
             &quot;in_pin_code&quot;: &quot;666666&quot;,
             &quot;address&quot;: &quot;Some other location&quot;,
             &quot;city&quot;: &quot;Delhi&quot;
          &#125;,
          &quot;validation_errors&quot;: &#123;
             &quot;in_pin_code&quot;: &quot;We could not locate this pin code.&quot;
          &#125;
       &#125;
    &#125;
  &#125;
&#125;&#039;
```

## Receive notifications for address submissions

Businesses will receive address submission notifications through [webhooks](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/overview), such as the one shown below.

```json
&#123;
  &quot;messages&quot;: [
    &#123;
      &quot;id&quot;: &quot;gBGGFlAwCWFvAgmrzrKijase8yA&quot;,
      &quot;from&quot;: &quot;&lt;PHONE_NUMBER&gt;&quot;,
      &quot;interactive&quot;: &#123;
        &quot;type&quot;: &quot;interactive&quot;,
        &quot;action&quot;: &quot;address_message&quot;,
        &quot;nfm_reply&quot;: &#123;
          &quot;name&quot;: &quot;address_message&quot;,
          &quot;response_json&quot;: &quot;&lt;response_json from client&gt;&quot;,
          &quot;body&quot;: &quot;&lt;body text from client&gt;&quot;
        &#125;,
        &quot;timestamp&quot;: &quot;1670394125&quot;
      &#125;
    &#125;
  ]
&#125;
```

The webhook notification has the following values.
| Field Name | Type | Description |
| --- | --- | --- |
| `interactive` | Object | Holds the response from the client |
| `type` | String | Would be `nfm_reply` indicating it is a Native Flow Response (NFM) from the client |
| `nfm_reply` | Object | Holds the data received from the client |
| `response_json` | String | The values of the address fields filled by the user in JSON format that are always present |
| `body` (Optional) | String | Body text from client, what the user sees |
| `name` (Optional) | String | Would be `address_message` indicating the type of NFM action response from the client |

An address message reply as an NFM response type for an India address message request is shown below.

```json
&#123;
  &quot;messages&quot;: [
    &#123;
      &quot;context&quot;: &#123;
        &quot;from&quot;: &quot;FROM_PHONE_NUMBER_ID&quot;,
        &quot;id&quot;: &quot;wamid.HBgLMTIwNjU1NTAxMDcVAgARGBI3NjNFN0U5QzMzNDlCQjY0M0QA&quot;
      &#125;,
      &quot;from&quot;: &quot;&lt;PHONE_NUMBER&gt;&quot;,
      &quot;id&quot;: &quot;wamid.HBgLMTIwNjU1NTAxMDcVAgASGCA5RDhBNENEMEQ3RENEOEEzMEI0RUExRDczN0I1NThFQwA=&quot;,
      &quot;timestamp&quot;: &quot;1671498855&quot;,
      &quot;type&quot;: &quot;interactive&quot;,
      &quot;interactive&quot;: &#123;
        &quot;type&quot;: &quot;nfm_reply&quot;,
        &quot;nfm_reply&quot;: &#123;
          &quot;response_json&quot;: &quot;&#123;\&quot;saved_address_id\&quot;:\&quot;address1\&quot;,\&quot;values\&quot;:&#123;\&quot;in_pin_code\&quot;:\&quot;400063\&quot;,\&quot;building_name\&quot;:\&quot;\&quot;,\&quot;landmark_area\&quot;:\&quot;Goregaon\&quot;,\&quot;address\&quot;:\&quot;Wing A, Cello Triumph, IB Patel Rd\&quot;,\&quot;city\&quot;:\&quot;Mumbai\&quot;,\&quot;name\&quot;:\&quot;CUSTOMER_NAME\&quot;,\&quot;phone_number\&quot;:\&quot;+91xxxxxxxxxx\&quot;,\&quot;floor_number\&quot;:\&quot;8\&quot;&#125;&#125;&quot;,
          &quot;body&quot;: &quot;CUSTOMER_NAME\n +91xxxxxxxxxx\n 400063, Goregaon, Wing A, Cello Triumph,IB Patel Rd, Mumbai, 8&quot;,
          &quot;name&quot;: &quot;address_message&quot;
        &#125;
      &#125;
    &#125;
  ]
&#125;
```

## Feature not supported

In the case where the client does not support `address_message`, WhatsApp silently drops the messages and sends an error message back to the business in a webhook. The webhook notification that would be sent back is shown below:

```json
&#123;
  &quot;statuses&quot;: [
    &#123;
      &quot;errors&quot;: [
        &#123;
          &quot;code&quot;: 1026,
          &quot;href&quot;: &quot;/docs/whatsapp/api/errors&quot;,
          &quot;title&quot;: &quot;Receiver Incapable&quot;
        &#125;
      ],
      &quot;id&quot;: &quot;gBGGFlAwCWFvAgkyHMGKnRu4JeA&quot;,
      &quot;message&quot;: &#123;
        &quot;recipient_id&quot;: &quot;+91xxxxxxxxxx&quot;
      &#125;,
      &quot;recipient_id&quot;: &quot;91xxxxxxxxxx&quot;,
      &quot;status&quot;: &quot;failed&quot;,
      &quot;timestamp&quot;: &quot;1670394125&quot;,
      &quot;type&quot;: &quot;message&quot;
    &#125;
  ]
&#125;
```
