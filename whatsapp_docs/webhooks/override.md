# Webhook overrides



Meta sends webhooks to the callback URL set on your app, but you can override this by designating an alternate callback URL for the WhatsApp Business account (WABA) or business phone number.

When a [supported field](#supported-webhook-fields) triggers a webhook, the system first checks if your app has designated an alternate callback URL for the business phone number associated with the event. If the business phone number has an alternate callback URL, the system sends the webhook there. If the phone number has no alternate, the system checks if the WABA associated with the number has an alternate callback URL. If the WABA has an alternate callback URL, the system sends the webhook there. If the WABA also has no alternate, the webhook falls back to your app&#039;s callback URL.

## Supported webhook fields

The override applies only to the following webhook field types. For field types not listed here, Meta always sends webhooks to your app&#039;s default callback URL.

- `messages`
- `message_echoes`
- `calls`
- `consumer_profile`
- `messaging_handovers`
- `group_lifecycle_update`
- `group_participants_update`
- `group_settings_update`
- `group_status_update`
- `smb_message_echoes`
- `smb_app_state_sync`
- `history`
- `account_settings_update`

&gt; **Note:** Template webhooks (`message_template_status_update`, `message_template_quality_update`, `message_template_components_update`, `template_category_update`) and account-level webhooks (`account_update`, `account_review_update`, `account_alerts`) do not support callback overrides. Meta always delivers these webhooks to your app&#039;s default callback URL.

## Requirements

Before setting an alternate callback URL, make sure your app is [subscribed to webhooks on the WABA](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/manage-webhooks#subscribe-to-a-whatsapp-business-account) and verify that your alternate callback endpoint can receive and process webhooks correctly.

## Set WABA alternate callback

Use the [Subscribed Apps API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/subscribed-apps-api#post-version-waba-id-subscribed-apps) to set an alternate callback URL on a WABA.

### Request syntax

```html
POST /&lt;WABA_ID&gt;/subscribed_apps
```

### Post body

```json
&#123;
  &quot;override_callback_uri&quot;:&quot;&lt;WABA_ALT_CALLBACK_URL&gt;&quot;,
  &quot;verify_token&quot;:&quot;&lt;WABA_ALT_CALLBACK_URL_TOKEN&gt;&quot;
&#125;
```

### Body parameters

| Placeholder | Description | Example value |
| --- | --- | --- |
| `&lt;WABA_ALT_CALLBACK_URL&gt;` | **Required.**&lt;br&gt;&lt;br&gt;Alternate callback URL where [supported webhooks](#supported-webhook-fields) should be sent.&lt;br&gt;&lt;br&gt;Maximum 200 characters. | `https://my-waba-alternate-callback.com/webhook` |
| `&lt;WABA_ALT_CALLBACK_URL_TOKEN&gt;` | **Required.**&lt;br&gt;&lt;br&gt;Alternate callback URL [verification token](https://developers.facebook.com/docs/graph-api/webhooks/getting-started#verification-requests).&lt;br&gt;&lt;br&gt;No maximum length. | `myvoiceismypassport?` |

### Response

Upon success:

```json
&#123;
  &quot;success&quot;: true
&#125;
```

### Example request

```html
curl -X POST \
&#039;https://graph.facebook.com/v25.0/102290129340398/subscribed_apps&#039; \
-H &#039;Authorization: Bearer EAAJi...&#039; \
-H &#039;Content-Type: application/json&#039; \
-d &#039;
&#123;
  &quot;override_callback_uri&quot;:&quot;https://my-waba-alternate-callback.com/webhook&quot;,
  &quot;verify_token&quot;:&quot;myvoiceismypassport?&quot;
&#125;&#039;
```

### Example response

```json
&#123;
  &quot;success&quot;: true
&#125;
```

## Get WABA alternate callback

Use the [Subscribed Apps API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/subscribed-apps-api#get-version-waba-id-subscribed-apps) to get a list of all apps subscribed to webhooks on the WABA. The response should include an `override_callback_uri` property and value.

### Example response

```json
&#123;
  &quot;data&quot; : [
    &#123;
      &quot;whatsapp_business_api_data&quot; : &#123;
        &quot;id&quot; : &quot;670843887433847&quot;,
        &quot;link&quot; : &quot;https://www.facebook.com/games/?app_id=67084...&quot;,
        &quot;name&quot; : &quot;Lucky Shrub&quot;
      &#125;,
      &quot;override_callback_uri&quot; : &quot;https://my-waba-alternate-callback.com/webhook&quot;
    &#125;
  ]
&#125;
```

## Delete WABA alternate callback

Use the [Subscribed Apps API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/subscribed-apps-api#post-version-waba-id-subscribed-apps) to subscribe your app to webhooks on the WABA [as you normally would](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/manage-webhooks#subscribe) (that is, without any post body parameters). Subscribing without post body parameters removes the alternate callback URL from the WABA, and webhooks for the WABA will once again be sent to the callback URL set in the App Dashboard.

## Set phone number alternate callback

Use the [WhatsApp Business Phone Number API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/whatsapp-business-account-phone-number-api#post-version-phone-number-id) to set an alternate callback URL on the business phone number.

### Request syntax

```html
POST /&lt;BUSINESS_PHONE_NUMBER_ID&gt;
```

### Post body

```json
&#123;
  &quot;webhook_configuration&quot;: &#123;
    &quot;override_callback_uri&quot;: &quot;&lt;PHONE_ALT_CALLBACK_URL&gt;&quot;,
    &quot;verify_token&quot;: &quot;&lt;PHONE_ALT_CALLBACK_URL_TOKEN&gt;&quot;
  &#125;
&#125;
```

### Body parameters

| Placeholder | Description | Example value |
| --- | --- | --- |
| `&lt;PHONE_ALT_CALLBACK_URL&gt;` | **Required.**&lt;br&gt;&lt;br&gt;Alternate callback URL where [supported webhooks](#supported-webhook-fields) should be sent.&lt;br&gt;&lt;br&gt;Maximum 200 characters. | `https://my-phone-alternate-callback.com/webhook` |
| `&lt;PHONE_ALT_CALLBACK_URL_TOKEN&gt;` | **Required.**&lt;br&gt;&lt;br&gt;Alternate callback URL [verification token](https://developers.facebook.com/docs/graph-api/webhooks/getting-started#verification-requests).&lt;br&gt;&lt;br&gt;No maximum length. | `myvoiceismypassport?` |

### Response

Upon success:

```json
&#123;
  &quot;success&quot;: true
&#125;
```

### Example request

```html
curl -X POST &#039;https://graph.facebook.com/v25.0/106540352242922&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039; \
-d &#039;
&#123;
  &quot;webhook_configuration&quot;: &#123;
    &quot;override_callback_uri&quot;: &quot;https://my-phone-alternate-callback.com/webhook&quot;,
    &quot;verify_token&quot;: &quot;myvoiceismypassport?&quot;
  &#125;
&#125;&#039;
```

### Example response

```json
&#123;
  &quot;success&quot;: true
&#125;
```

## Get phone number alternate callback

Use the [WhatsApp Business Phone Number API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/whatsapp-business-account-phone-number-api#get-version-phone-number-id) and request the `webhook_configuration` field to verify that the business phone number has an alternate callback URL.

### Request syntax

```html
GET /&lt;BUSINESS_PHONE_NUMBER_ID&gt;
  ?fields=webhook_configuration
```

### Response

Upon success:

```json
&#123;
  &quot;webhook_configuration&quot;: &#123;
    &quot;phone_number&quot;: &quot;&lt;PHONE_ALT_CALLBACK_URL&gt;&quot;,
    &quot;whatsapp_business_account&quot;: &quot;&lt;WABA_ALT_CALLBACK_URL&gt;&quot;,
    &quot;application&quot;: &quot;&lt;APP_CALLBACK_URL&gt;&quot;
  &#125;,
  &quot;id&quot;: &quot;106540352242922&quot;
&#125;
```

The `whatsapp_business_account` property is only included if the WABA associated with the business phone number also has an alternate callback URL set.

### Example request

```html
curl &#039;https://graph.facebook.com/v25.0/106540352242922?fields=webhook_configuration&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039;
```

### Example response

```json
&#123;
  &quot;webhook_configuration&quot;: &#123;
    &quot;phone_number&quot;: &quot;https://my-phone-alternate-callback.com/webhook&quot;,
    &quot;whatsapp_business_account&quot;: &quot;https://my-waba-alternate-callback.com/webhook&quot;,
    &quot;application&quot;: &quot;https://my-production-callback.com/webhook&quot;
  &#125;,
  &quot;id&quot;: &quot;106540352242922&quot;
&#125;
```

## Delete phone number alternate callback

To delete a business phone number&#039;s alternate callback URL, use the [WhatsApp Business Phone Number API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/whatsapp-business-account-phone-number-api#post-version-phone-number-id) with the `override_callback_uri` property set to an empty string:

```json
&#123;
  &quot;webhook_configuration&quot;: &#123;
    &quot;override_callback_uri&quot;: &quot;&quot;
  &#125;
&#125;
```
