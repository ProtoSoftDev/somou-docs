# No Storage



&quot;No Storage&quot; is a custom configuration of Cloud API [local storage](https://developers.facebook.com/documentation/business-messaging/whatsapp/local-storage), where the data in-transit is kept for up to an hour in Meta data centers and the data is not persisted at rest (that is to say, not in Meta data centers nor in AWS In-Country stores).

- Outgoing/incoming messages are stored for a maximum of 1 hour in Meta data centers.
- Outgoing/incoming media blobs are stored for a maximum of 1 hour in Meta data centers.
- You can pass a custom time-to-live (TTL) — from 1 hour to 30 days — when uploading media to override the 1 hour expiration (particularly useful for marketing campaigns which reuse the same media).

## Limitations

When the No Storage feature is enabled, message content is not stored at rest for 30 days as is typical with Cloud API. This introduces the following limitations, which can put a small fraction of your total messaging volume at risk of non-delivery.

- **Message decryption failures** — If a message fails to decrypt on the consumer side, Cloud API can only retry sending the message within a 1-hour TTL window. After this 1-hour window, Cloud API cannot retry the message. You will receive an error webhook indicating the failure. See: [Retry Receipt Failures](#retry-receipt-failures).
- **Webhook delivery failures** — Normally, Cloud API retries undelivered webhooks (such as incoming messages or receipts) for up to 7 days. With No Storage enabled, webhook retries are limited to 1 hour. If your webhook server is unavailable beyond this window, the webhook (including incoming messages, receipts, etc.) will be permanently lost. See: [Failure to Deliver Webhooks](#failure-to-deliver-webhooks).
- **Incoming media messages** — Media attached to incoming messages will be available for download for up to 1 hour. After 1 hour, the media is permanently deleted and cannot be retrieved.

## Enable No Storage

### Request syntax (v21.0 or newer)

Enable the feature before registration using the [Settings API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/settings-api#post-version-phone-number-id-settings).

```html
curl &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;BUSINESS_PHONE_NUMBER_ID&gt;/settings&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039; \
-d &#039;
&#123;
  &quot;storage_configuration&quot;: &#123;
    &quot;status&quot;: &quot;NO_STORAGE_ENABLED&quot;,
    &quot;retention_minutes&quot;: 60
  &#125;
&#125;&#039;
```

Currently, only the `60` value is allowed for the `retention_minutes` parameter, as it is the only retention duration we are supporting.

### Request syntax (v20.0 or older)

Enable the feature within the [Register API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/register-api#post-version-phone-number-id-register) registration request.

```html
curl &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;BUSINESS_PHONE_NUMBER_ID&gt;/settings&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039; \
-d &#039;
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;pin&quot;: &quot;123456&quot;,
  &quot;tier&quot;: &quot;test&quot;,
  &quot;meta_store_retention_minutes&quot;: 60
&#125;&#039;
```

- Currently only the 60 value is allowed for the meta_store_retention_minutes parameter as it is the only retention duration we are supporting.
- `meta_store_retention_minutes` cannot be used alongside with `data_localization_region`.

## Disable No Storage

To disable No Storage, you must de-register the business phone number using the [Deregister API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/phone-number-deregister-api#post-version-phone-number-id-deregister), then register the number again without the `meta_store_retention_minutes` parameter.

### Example deregister syntax

```html
curl -X POST &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;BUSINESS_PHONE_NUMBER_ID&gt;/deregister&#039; \
-H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039;
```

## Override outgoing media TTL

The default 1-hour TTL for No Storage-enabled business phone numbers also applies to [media uploaded on the number](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-phone-numbers/media#upload-media). If you want to override the default 1-hour TTL, you can include the new `ttl_minutes` parameter when uploading media.

### Example syntax

```html
curl &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;BUSINESS_PHONE_NUMBER_ID&gt;/media&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039; \
-d &#039;
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;file&quot;: &quot;file=&lt;FILE_NAME&gt;;type=&lt;FILE_MIME_TYPE&gt;&quot;,
  &quot;ttl_minutes&quot;: &quot;120&quot;
&#125;&#039;
```

- `ttl_minutes` range is from 1 hour (`60`) to 30 days (`43200`).
- The API currently does not return the expiration date of the media in the response API.

## Error webhooks

### Retry receipt failures

In the case of WhatsApp client decryption failures, we will stop attempting to deliver an undelivered message from a No Storage-enabled number once the TTL is reached. In these cases, a [status messages](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/messages/status) webhook is triggered with error code `131036`:

#### Example payload

```json
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [&#123;
    &quot;id&quot;: &quot;102290129340398&quot;,
    &quot;changes&quot;: [&#123;
      &quot;field&quot;: &quot;messages&quot;,
      &quot;value&quot;: &#123;
        &quot;messaging_product&quot;: &quot;whatsapp&quot;,
        &quot;metadata&quot;: &#123;
          &quot;display_phone_number&quot;: &quot;15550783881&quot;,
          &quot;phone_number_id&quot;: &quot;106540352242922&quot;
        &#125;,
        &quot;statuses&quot;: [&#123;
          &quot;id&quot;: &quot;wamid.HBgMNDQ3ODI1MDYzOTQxFQIAERgSN0MzMTg0Nzk2RkMwOEQ5NTQ2AA==&quot;,
          &quot;status&quot;: &quot;failed&quot;,
          &quot;timestamp&quot;: &quot;1712597457&quot;,
          &quot;recipient_id&quot;: &quot;16505551234&quot;,
          &quot;errors&quot;: [&#123;
              &quot;code&quot;: 131036,
              &quot;title&quot;: &quot;Message failed to be delivered on at least one of the user&#039;s device&quot;,
              &quot;message&quot;: &quot;Message failed to be delivered on at least one of the user&#039;s device&quot;,
              &quot;error_data&quot;: &#123;
                &quot;details&quot;: &quot;Message payload not found&quot;
              &#125;
            &#125;]
        &#125;]
      &#125;
    &#125;]
  &#125;]
&#125;
```

Notes:

- This error is sent only if we fail to honor a retry receipt sent by the primary device. If the retry fails for a secondary device, we ignore it, as the message will be delivered when syncing with the primary device.
- It is possible that the message has been successfully delivered to secondary devices but not the primary device. In this case, the webhook will be sent.

### Failure to deliver webhooks

By default, Cloud API retries for up to 7 days to deliver incoming [messages](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/messages#incoming-messages) webhooks. For No Storage-enabled business phone numbers, if we fail to deliver an incoming message webhook, we will drop it and instead send an [errors messages](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/messages/errors) webhook with error code `131035`:

```json
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [&#123;
    &quot;id&quot;: &quot;102290129340398&quot;,
    &quot;changes&quot;: [&#123;
      &quot;field&quot;: &quot;messages&quot;,
      &quot;value&quot;: &#123;
        &quot;messaging_product&quot;: &quot;whatsapp&quot;,
        &quot;metadata&quot;: &#123;
          &quot;display_phone_number&quot;: &quot;15550783881&quot;,
          &quot;phone_number_id&quot;: &quot;106540352242922&quot;
        &#125;,
        &quot;errors&quot;: [&#123;
            &quot;code&quot;: 131035,
            &quot;title&quot;: &quot;Webhook could not be delivered within data retention limit&quot;,
            &quot;message&quot;: &quot;Webhook could not be delivered within data retention limit&quot;,
            &quot;error_data&quot;: &#123;
              &quot;details&quot;: &quot;Webhook could not be delivered within data retention limit&quot;
            &#125;
          &#125;]
      &#125;
    &#125;]
  &#125;]
&#125;
```
