# Two-Step Verification



Set up two-step verification for your phone number to require a 6-digit PIN when registering the phone number. Use the [WhatsApp Business Phone Number API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/whatsapp-business-account-phone-number-api#post-version-phone-number-id) to set it up with the parameters below. There is no endpoint to disable two-step verification.

| Endpoint | Authentication |
| --- | --- |
| `/PHONE_NUMBER_ID`&lt;br&gt;&lt;br&gt;(See [Get Phone Number ID](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-phone-numbers/phone-numbers#get-all-phone-numbers))&lt;br&gt; | Solution Partners must authenticate themselves with an access token with the `whatsapp_business_management`  and `whatsapp_business_messaging`  permissions.&lt;br&gt; |

### Parameters

| Name | Description |
| --- | --- |
| `pin` | **Required.**&lt;br&gt;&lt;br&gt;A 6-digit PIN you wish to use for two-step verification. |

### Example

Sample request:

```curl
curl -X  POST \
 &#039;https://graph.facebook.com/v25.0/FROM_PHONE_NUMBER_ID&#039; \
 -H &#039;Authorization: Bearer ACCESS_TOKEN&#039; \
 -H &#039;Content-Type: application/json&#039; \
 -d &#039;&#123;&quot;pin&quot; : &quot;6_DIGIT_PIN&quot;&#125;&#039;
```


Sample response:

```json
&#123;
  &quot;success&quot;: true
&#125;
```

**Note:** All API calls require authentication with access tokens.

Developers can authenticate their API calls with the access token generated in the **App Dashboard** &gt; **WhatsApp** &gt; **API Setup**.

Solution Partners must authenticate themselves with an access token with the `whatsapp_business_messaging`  and `whatsapp_business_management`  permissions. See [System User Access Tokens](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#system-user-access-tokens) for information.

## Reset your PIN

Resetting your PIN is the non-API alternative to the API setup above. If you forget or misplace your PIN, update your PIN in WhatsApp Manager by following these steps:

1. Go to [settings](https://business.facebook.com/settings/) and log in to your Facebook Business. Click the business you use to manage your WABA (WhatsApp Business account).
1. In the settings screen, click **WhatsApp Accounts**. Find the WABA you want to update. Click the WABA. A panel with its info displays.
1. In the WABA info panel, click **Settings**.
1. In the new tab, click **WhatsApp Manager**.
1. In WhatsApp Manager, find your phone number and click **Settings**.
1. Click **Two-step verification**.
1. In the Two-step verification tab, click **Change PIN**.
1. Enter a new PIN and confirm it to complete the update.
