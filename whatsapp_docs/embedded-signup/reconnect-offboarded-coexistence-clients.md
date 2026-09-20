# Reconnect offboarded coexistence clients



**Warning:** **Embedded signup v2 will be deprecated on October 15, 2026.** Migrate your integration to [v4](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/version-4) before that date to avoid disruption. See [Versions](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/versions) for the full upgrade path.

When a client [onboarded via coexistence](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/onboarding-business-app-users) changes devices or re-registers their WhatsApp Business app, their Cloud API companion is automatically offboarded. When your client completes registration on their new device, they see a pre-checked opt-in listing all previously connected Cloud API products. If they do not opt out, reonboarding (the process of restoring the Cloud API companion) runs automatically in the background and typically completes within a few minutes. No action is required from you or your client beyond completing registration.

## Prerequisites

- Your client must have been onboarded via [coexistence](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/onboarding-business-app-users).
- Your app must be subscribed to the [`account_update`](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/account_update) webhook field.
- No other partner or coexistence product can have onboarded the WhatsApp Business account (WABA) during the reconnection window. If a different partner onboards the WABA, WhatsApp clears reonboarding eligibility.

## How it works

1. Your client changes devices, reinstalls the WhatsApp Business app, or re-registers their phone number.
2. The Cloud API companion is automatically offboarded. You receive an `ACCOUNT_OFFBOARDED` webhook.
3. During the profile creation step of the WhatsApp Business registration flow, your client sees their previously connected Cloud API products listed with a pre-checked opt-in.
4. If your client does not opt out, reonboarding runs automatically in the background.
5. Within a few minutes, reonboarding completes. Your client receives an in-app notification, the Cloud API companion is restored, and you receive an `ACCOUNT_RECONNECTED` webhook.

After reconnection, messaging via Cloud API resumes and your client can continue using both the WhatsApp Business app and your platform.

## Detect offboarding

When a coexistence client is offboarded due to a device change or re-registration, you receive an `ACCOUNT_OFFBOARDED` webhook on the `account_update` field.

When you receive this webhook:

- Pause any pending Cloud API message sends for this client, as they fail while reonboarding is in progress.
- Monitor for the `ACCOUNT_RECONNECTED` webhook, which typically arrives once your client completes registration.

### `ACCOUNT_OFFBOARDED` webhook payload

You receive the following payload on your `account_update` webhook subscription when a coexistence client is offboarded due to a device change or re-registration:

```html
&#123;
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;&lt;WABA_ID&gt;&quot;,
      &quot;time&quot;: &lt;WEBHOOK_TIMESTAMP&gt;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;event&quot;: &quot;ACCOUNT_OFFBOARDED&quot;
          &#125;,
          &quot;field&quot;: &quot;account_update&quot;
        &#125;
      ]
    &#125;
  ],
  &quot;object&quot;: &quot;whatsapp_business_account&quot;
&#125;
```

## Capabilities during reonboarding

The following describes capability states while reonboarding is in progress.

| Capability | Status during reonboarding |
|---|---|
| Cloud API messaging | Suspended — messages cannot be sent or received via Cloud API while reonboarding is in progress. |
| Partner access to WABA | Retained — you can still query the WABA via the Graph API. |
| WABA webhook subscriptions | Retained — you continue to receive webhooks, including the `ACCOUNT_RECONNECTED` webhook when reonboarding completes. |
| WhatsApp Business app messaging | Briefly unavailable while re-registration is in progress. Messaging resumes once re-registration completes. |
| Chat history | Not synced during reonboarding. Because reonboarding completes within minutes, the gap in chat history is minimal to none. |

## Confirm reconnection

You receive an `ACCOUNT_RECONNECTED` webhook when your client&#039;s Cloud API companion is restored.

After reconnection:

- Cloud API messaging resumes automatically.
- Other companion devices (such as WhatsApp Web) aren&#039;t automatically re-linked. Your client must manually re-link any companion devices.

### `ACCOUNT_RECONNECTED` webhook payload

You receive the following payload on your `account_update` webhook subscription when your client reconnects:

```html
&#123;
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;&lt;WABA_ID&gt;&quot;,
      &quot;time&quot;: &lt;WEBHOOK_TIMESTAMP&gt;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;event&quot;: &quot;ACCOUNT_RECONNECTED&quot;
          &#125;,
          &quot;field&quot;: &quot;account_update&quot;
        &#125;
      ]
    &#125;
  ],
  &quot;object&quot;: &quot;whatsapp_business_account&quot;
&#125;
```

## Limitations

- Only the Cloud API companion is restored. Other companion devices (such as WhatsApp Web) must be manually re-linked by your client.
- Messages sent while re-registration is in progress might not be delivered via Cloud API.

## See also

- [Onboarding WhatsApp Business app users (Coexistence)](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/onboarding-business-app-users)
- [account_update webhook reference](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/account_update)
- [Embedded Signup overview](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/overview)
