# QR Codes and Short Links



WhatsApp QR codes and short links create a digital doorstep for businesses, enabling them to stay connected with their existing customers and connect with new ones. This way, customers can simply scan QR codes with their mobile device camera or type in a short link to begin a chat thread, without needing to input a phone number.

You can view, create, edit and delete QR codes and short links in the [WhatsApp Business Management API](https://developers.facebook.com/documentation/business-messaging/whatsapp/qr-codes) or in the [Business Manager UI](https://www.facebook.com/business/help/890732351439459).


### Limitations

- A single WABA phone number cannot be associated with more than 2,000 QR codes and short links.

- A QR code scan can initiate a pre-filled message containing up to 140 characters of text.

- Analytics are not available for QR Codes and Short Links as we limit the amount of data we log to protect user privacy.

## Create QR code

Use the [QR Codes API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/whatsapp-business-qr-code-management-api#post-version-phone-number-id-message-qrdls) to create a QR code.

In your post body, include an object with a `prefilled_message` property set to your message text and a `generate_qr_image` property set to your preferred image format, either `SVG` or `PNG`.

### Example request

```curl
curl &#039;https://graph.facebook.com/v25.0/106540352242922/message_qrdls&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039; \
-d &#039;
&#123;
  &quot;prefilled_message&quot;: &quot;Cyber Monday&quot;,
  &quot;generate_qr_image&quot;: &quot;SVG&quot;
&#125;&#039;
```

### Example response

```json
&#123;
  &quot;code&quot;: &quot;4O4YGZEG3RIVE1&quot;,
  &quot;prefilled_message&quot;: &quot;Cyber Monday 1&quot;,
  &quot;deep_link_url&quot;: &quot;https://wa.me/message/4O4YGZEG3RIVE1&quot;,
  &quot;qr_image_url&quot;: &quot;https://scontent-iad3-2.xx.fbcdn.net/...&quot;
&#125;
```

## Get a list of QR codes and short links &#123;#get-list&#125;

Use the [QR Codes API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/whatsapp-business-qr-code-management-api#get-version-phone-number-id-message-qrdls) to get a list of all QR codes on a business phone number. To use Meta Business Suite to get a list of existing QR codes, see the [Help Center article](https://www.facebook.com/business/help/890732351439459).

### Example request

```curl
curl &#039;https://graph.facebook.com/v25.0/106540352242922/message_qrdls&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039;
```

### Example response

```json
&#123;
  &quot;data&quot;: [
    &#123;
      &quot;code&quot;: &quot;4O4YGZEG3RIVE1&quot;,
      &quot;prefilled_message&quot;: &quot;Cyber Monday&quot;,
      &quot;deep_link_url&quot;: &quot;https://wa.me/message/4O4YGZEG3RIVE1&quot;
    &#125;,
    &#123;
      &quot;code&quot;: &quot;WOMVT6TJ2BP7A1&quot;,
      &quot;prefilled_message&quot;: &quot;Tell me more about your production workshop&quot;,
      &quot;deep_link_url&quot;: &quot;https://wa.me/message/WOMVT6TJ2BP7A1&quot;
    &#125;
  ]
&#125;
```

## Get a QR code

Use the [QR Codes API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/whatsapp-business-qr-code-management-api#get-version-phone-number-id-message-qrdls) to get information about a specific QR code by appending the QR code ID as a path parameter.

### Example request

```curl
curl &#039;https://graph.facebook.com/v25.0/106540352242922/message_qrdls/4O4YGZEG3RIVE1&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039;
```

### Example response

```json
&#123;
  &quot;data&quot;: [
    &#123;
      &quot;code&quot;: &quot;4O4YGZEG3RIVE1&quot;,
      &quot;prefilled_message&quot;: &quot;Cyber Monday&quot;,
      &quot;deep_link_url&quot;: &quot;https://wa.me/message/4O4YGZEG3RIVE1&quot;
    &#125;
  ]
&#125;
```

## Update a QR code

Use the [QR Codes API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/whatsapp-business-qr-code-management-api#post-version-phone-number-id-message-qrdls) to update a QR code.

In the post body, include a `code` property set to the ID of the QR code you wish to update, and a `prefilled_message` property set to the new QR code text.

### Example request

```curl
curl &#039;https://graph.facebook.com/v25.0/106540352242922/message_qrdls&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039; \
-d &#039;
&#123;
    &quot;code&quot;: &quot;4O4YGZEG3RIVE1&quot;,
    &quot;prefilled_message&quot;: &quot;Cyber Tuesday&quot;
&#125;&#039;
```

### Example response

```json
&#123;
  &quot;code&quot;: &quot;4O4YGZEG3RIVE1&quot;,
  &quot;prefilled_message&quot;: &quot;Cyber Tuesday&quot;,
  &quot;deep_link_url&quot;: &quot;https://wa.me/message/4O4YGZEG3RIVE1&quot;
&#125;
```

## Delete QR code

QR codes do not expire automatically. Use the [QR Codes API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/whatsapp-business-qr-code-management-api) to delete a QR code by sending a DELETE request and appending the ID of the QR code you wish to retire as a path parameter. To use Meta Business Suite to delete QR codes, see the [Help Center article](https://www.facebook.com/business/help/890732351439459).

### Example request

```curl
curl -X DELETE &#039;https://graph.facebook.com/v25.0/106540352242922/message_qrdls/4O4YGZEG3RIVE1&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039;
```

### Example response

```json
&#123;
  &quot;success&quot;: true
&#125;
```

## Prefilled messages &#123;#prefilled-messages&#125;

QR codes and short links can be programmed to populate a prefilled message. Prefilled messages can contain up to 140 characters of text. These messages are fully customizable and can be updated or deleted at any time.

## User experience

| User Scenario | Expected Behavior |
| --- | --- |
| User tries to access a code or link that has been deleted. | User sees an error message saying &quot;This QR code [short link] has expired.&quot; |
| User scans the QR code or types in the short link of a business they previously blocked. | User gets a prompt asking if they would like to unblock the business to continue messaging them. |
| User clicks a short link on a desktop browser. | Desktop client launches with message populated in chat thread. If there is no client installed, user is prompted to install it. |

## Best practices

### Format &#123;#format&#125;

We recommend outputting the QR code as a scalable vector graphic (`.svg`) file. You can resize your QR Code to develop product packaging, signage, etc.

### Appearance

While we do not offer the capability to natively customize QR codes, you can do so by downloading and editing QR codes with the software of your choice. We recommend you do not customize the color or look and feel of the code itself in order to preserve readability.

## FAQs &#123;#faq&#125;

**How do I create a QR code or short link? **

You can view, create, edit and delete QR codes and short links in the [WhatsApp Business Management API](https://developers.facebook.com/documentation/business-messaging/whatsapp/qr-codes) or in the [Business Manager UI](https://www.facebook.com/business/help/890732351439459).

**How many QR codes and short links can I create?**

A single WABA phone number cannot be associated with more than 2,000 QR codes and short links.

**What is the best format to use to optimize print quality of a QR code?**

We recommend the `.svg` file format for the best quality in print materials.

**How do short links improve on the existing wa.me links?**

The new short links enable prefilled messages associated with a link to be edited or deleted at any time. They also reduce the syntax of the URL to a random code, which eliminates the need to embed messages in the URL and masks the phone number.

**What happens if a user clicks a short link on a desktop?**

If the user has the WhatsApp desktop client installed, it will launch a chat thread with your business. If not, the user will be prompted to install the WhatsApp desktop client.

**What happens when a user scans a QR code or clicks a short link that has been deleted? **

If a user tries to access a QR code or short link that has been deleted, they will see an error message indicating the QR code/short link has expired.

**How does this differ from QR codes I am already generating in my own development environment today?**

QR codes can now be generated and managed directly within the WhatsApp Business Management API and users can scan them with their WhatsApp, iOS, or Android camera.

In addition, with WhatsApp QR codes

* Prefilled messages are fully customizable and can be changed or deleted at any time,
* Users will always go directly to the app without any interstitial page, and
* The in-app experience for an expired code sends the user a clear message.

**How can I ensure the code is scanned by users in the right language?**

You will be responsible for using the appropriate QR code based on expected location and language of users.

**Will analytics be available to track QR code scans?**

Analytics are not available for QR Codes and Short Links as we limit the amount of data we log to protect user privacy.

