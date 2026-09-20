# Supported message types


**Note:** The Direct Send API is in beta. Features and behavior described here are subject to change and may be released incrementally. Participation is subject to acceptance of the beta terms.

Direct Send supports text and several interactive message types. For every type, remember to add `&quot;category&quot;: &quot;utility&quot;` (or `&quot;authentication&quot;` where supported) to the request body to invoke the Direct Send API.

Interactive message headers can be `text`, `image`, `video`, or `document`. Media headers are access-restricted during beta — see [Media message headers](https://developers.facebook.com/documentation/business-messaging/whatsapp/direct-send/media-headers).

## Text messages

Text messages contain a text body and an optional link preview.

&gt; **Note.** The `preview_url` field is not currently supported. Messages sent with Direct Send do not render a URL preview.

Add `&quot;category&quot;: &quot;utility&quot;` or `&quot;category&quot;: &quot;authentication&quot;` to the bottom of your request body to invoke Direct Send.

[Learn more about formatting text messages](https://developers.facebook.com/docs/whatsapp/cloud-api/messages/text-messages)

## Interactive call-to-action URL button messages

Call-to-action (CTA) URL button messages map a URL to a button, so you don&#039;t include the raw URL in the message body. We recommend the combined CTA URL and reply button format instead — see [Interactive button messages with CTA URL or reply buttons](#interactive-button-messages-with-cta-url-or-reply-buttons).

&gt; **Note.** The header must be `type: &quot;text&quot;`, `&quot;image&quot;`, `&quot;video&quot;`, or `&quot;document&quot;`. See [Media message headers](https://developers.facebook.com/documentation/business-messaging/whatsapp/direct-send/media-headers) for access requirements.

[Learn more about formatting CTA URL button messages](https://developers.facebook.com/docs/whatsapp/cloud-api/messages/interactive-cta-url-messages)

## Interactive reply button messages

Reply button messages let you send up to three predefined replies for the user to choose from. Selecting a button triggers a messages webhook describing the user&#039;s choice.

[Learn more about formatting interactive reply button messages](https://developers.facebook.com/docs/whatsapp/cloud-api/messages/interactive-reply-buttons-messages)

## Interactive button messages with CTA URL or reply buttons

This is the recommended way to send interactive buttons. You can mix call-to-action URL and reply buttons in the same message:

- Up to **10** buttons total.
- A maximum of **2** CTA buttons.
- CTA buttons are always listed **before** reply buttons.

See [Multiple call-to-action URL button samples](https://developers.facebook.com/documentation/business-messaging/whatsapp/direct-send/send-sample-payloads#multiple-call-to-action-url-button-samples) for the request format.

## Message success and pricing

The message-success webhook payload is at parity with the current Cloud API message-status webhook and includes pricing details. See the [webhook statuses object](https://developers.facebook.com/docs/whatsapp/cloud-api/webhooks/components#statuses-object) and [per-message pricing webhooks](https://developers.facebook.com/docs/whatsapp/pricing/updates-to-pricing#per-message-pricing-cloud-api-webhooks) for the latest structure.

Direct Send adds one field to the status webhook: **`template_id`**, the template used to send the Direct Send message. It appears in the `statuses` section:

```json
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;&lt;ID&gt;&quot;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;messaging_product&quot;: &quot;whatsapp&quot;,
            &quot;statuses&quot;: [
              &#123;
                &quot;id&quot;: &quot;&lt;ID&gt;&quot;,
                &quot;status&quot;: &quot;&lt;read/delivered/sent&gt;&quot;,
                &quot;timestamp&quot;: &quot;&lt;EPOCH_TIME&gt;&quot;,
                &quot;recipient_id&quot;: &quot;&lt;RECIPIENT_PHONE_NUMBER&gt;&quot;,
                &quot;template_id&quot;: &quot;&lt;TEMPLATE_ID&gt;&quot;,
                &quot;conversation&quot;: &#123; &#125;,
                &quot;pricing&quot;: &#123; &#125;
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

&gt; **Note.** `recipient_id` is the message recipient&#039;s identifier — a phone number, or a business-scoped user ID (BSUID) if the message was addressed to one. See [Business-scoped user IDs](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-scoped-user-ids/).

## Related

- [Media message headers](https://developers.facebook.com/documentation/business-messaging/whatsapp/direct-send/media-headers)
- [Send sample message payloads](https://developers.facebook.com/documentation/business-messaging/whatsapp/direct-send/send-sample-payloads) — full JSON samples for each type
