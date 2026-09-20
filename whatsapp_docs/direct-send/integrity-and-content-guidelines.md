# Integrity and content guidelines


**Note:** The Direct Send API is in beta. Features and behavior described here are subject to change and may be released incrementally. Participation is subject to acceptance of the beta terms.

The Direct Send API is limited to utility and authentication messages, and Meta actively monitors how messages are categorized against [Meta&#039;s template guidelines](https://developers.facebook.com/docs/whatsapp/updates-to-pricing/new-template-guidelines). The same integrity rules that apply to user-created templates also apply to Direct Send generated templates.

## Template pausing

If you send a message that matches a template paused for low quality, the message fails and you receive an error code in a webhook response. See the [template pausing guidelines](https://developers.facebook.com/docs/whatsapp/message-templates/guidelines#template-pausing) for pausing and unpausing details.

To avoid template restrictions:

1. Send messages only to users who are expecting them.
2. Design content so the message is expected — for example, a standard opt-out message or appointment reminder.

### Webhook for template pausing

Meta notifies you by email and webhook when a template&#039;s status changes to paused. Subscribe to the [template status webhook](https://developers.facebook.com/docs/whatsapp/message-templates/guidelines#monitoring-status-changes) for the fastest updates.

```json
&#123;
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;102290129340398&quot;,
      &quot;time&quot;: 1751247548,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;event&quot;: &quot;PAUSED&quot;,
            &quot;message_template_id&quot;: 1689556908129832,
            &quot;message_template_name&quot;: &quot;auto_generated_123456&quot;,
            &quot;message_template_language&quot;: &quot;en-US&quot;
          &#125;,
          &quot;field&quot;: &quot;message_template_status_update&quot;
        &#125;
      ]
    &#125;
  ],
  &quot;object&quot;: &quot;whatsapp_business_account&quot;
&#125;
```

See the [template status webhook reference](https://developers.facebook.com/docs/whatsapp/cloud-api/webhooks/reference/message_template_status_update/) for full details.

### Error when sending a paused template

When you attempt to send a message matching a paused template, the message fails and the webhook response includes a specific error code:

```json
&#123;
  &quot;statuses&quot;: [
    &#123;
      &quot;id&quot;: &quot;wamid.xxxxx&quot;,
      &quot;status&quot;: &quot;failed&quot;,
      &quot;errors&quot;: [
        &#123;
          &quot;code&quot;: 132015,
          &quot;title&quot;: &quot;Template is temporarily unavailable to use because it was paused due to low quality.&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

## Template categorization

Meta monitors how Direct Send messages are categorized. If Meta detects marketing content sent through Direct Send, you&#039;re notified. Persistent misuse results in restrictions to Direct Send access.

Submit samples for new use cases proactively to stay aligned with [Meta&#039;s template guidelines](https://developers.facebook.com/docs/whatsapp/updates-to-pricing/new-template-guidelines) and reduce enforcement risk. See [Send sample message payloads](https://developers.facebook.com/documentation/business-messaging/whatsapp/direct-send/send-sample-payloads).

### Notification of non-compliant content

Meta sends an email whenever marketing or authentication content is detected on messages sent through Direct Send. This gives you an opportunity to address the content before restrictions are enforced. Reach out to [wadirectsendapisupport&#064;meta.com](mailto:wadirectsendapisupport&#064;meta.com) to request a review.

- **While a review is in progress**, no immediate disruptions are expected.
- **Once a review confirms** the content is out of compliance (deemed marketing), Meta either offboards the use case from Direct Send or works with you to revise the content and resume sending.
- Using the samples API to submit real content — and ensuring sends match the samples — minimizes disruption.

### Webhook for category misuse

Subscribe to the `template_correct_category_detection` webhook field to be notified about messages identified as category misuse. Ingest the `message_template_id` values for the flagged templates.

```json
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;&lt;ID&gt;&quot;,
      &quot;time&quot;: &lt;TIME&gt;,
      &quot;changes&quot;: [
        &#123;
          &quot;field&quot;: &quot;template_correct_category_detection&quot;,
          &quot;value&quot;: &#123;
            &quot;message_template_id&quot;: &lt;MESSAGE_TEMPLATE_ID&gt;,
            &quot;message_template_name&quot;: &quot;&lt;MESSAGE_TEMPLATE_NAME&gt;&quot;,
            &quot;message_template_language&quot;: &quot;&lt;MESSAGE_TEMPLATE_LANGUAGE&gt;&quot;,
            &quot;category&quot;: &quot;UTILITY&quot;,
            &quot;correct_category&quot;: &quot;MARKETING&quot;
          &#125;
        &#125;
      ]
    &#125;
  ]
&#125;
```

The `correct_category` can be `MARKETING` or `AUTHENTICATION`.

### Retrieve flagged templates

To view Direct Send templates flagged for not meeting category guidelines:

```html
GET /&lt;WABA_ID&gt;/message_templates?source=AUTO_GENERATED&amp;correct_category=MARKETING
GET /&lt;WABA_ID&gt;/message_templates?source=AUTO_GENERATED&amp;correct_category=AUTHENTICATION
```

Example response:

```json
&#123;
  &quot;data&quot;: [
    &#123;
      &quot;name&quot;: &quot;direct_send_text_e22a3ec4_7c4a_4097_ae40_56ed1e89941c&quot;,
      &quot;parameter_format&quot;: &quot;POSITIONAL&quot;,
      &quot;components&quot;: [
        &#123;
          &quot;type&quot;: &quot;BODY&quot;,
          &quot;text&quot;: &quot;&lt;BODY_TEXT&gt;&quot;,
          &quot;example&quot;: &#123;
            &quot;body_text&quot;: [
              [&quot;sample 1&quot;]
            ]
          &#125;
        &#125;
      ],
      &quot;language&quot;: &quot;zh_CN&quot;,
      &quot;status&quot;: &quot;APPROVED&quot;,
      &quot;category&quot;: &quot;UTILITY&quot;,
      &quot;correct_category&quot;: &quot;MARKETING&quot;,
      &quot;source&quot;: &quot;AUTO_GENERATED&quot;,
      &quot;id&quot;: &quot;1951933648908188&quot;
    &#125;
  ],
  &quot;paging&quot;: &#123;
    &quot;cursors&quot;: &#123;
      &quot;before&quot;: &quot;MAZDZD&quot;,
      &quot;after&quot;: &quot;MjQZD&quot;
    &#125;
  &#125;
&#125;
```

## Account restrictions for persistent category misuse

**Note:** Account restrictions roll out incrementally. Warning notifications and rate-limiting apply first; the 7-day, 30-day, and permanent-revocation stages are being enabled gradually and may not yet be active for every account.

Persistent misuse of Direct Send to send non-compliant messages leads to increasingly restricted access, up to complete revocation:

1. **Stage 1 — Misuse detected (no restriction).** You&#039;re notified and can email [wadirectsendapisupport&#064;meta.com](mailto:wadirectsendapisupport&#064;meta.com) to resolve the issue.
2. **Rate-limiting restriction.** If misuse continues, a temporary rate-limit cap is applied to the WABA. The WABA can still send utility messages up to a capped volume; once the cap is reached, further utility sends are rejected with error `131064` until the volume falls back under the limit or the enforcement window ends. The limit lifts automatically — see the expiration in the `ACCOUNT_RESTRICTION` webhook.
3. **Stage 2 — Continued misuse (7-day restriction).** The WABA can&#039;t use the Direct Send API for utility (or authentication) messaging for 7 days.
4. **Stage 3 — Continued misuse (30-day restriction).** A 30-day restriction is imposed. This is the final warning before permanent removal from the beta.
5. **Stage 4 — Continued misuse (permanent revocation).** Direct Send access is permanently revoked.

### Webhook for account restriction

Subscribe to the `account_update` field to receive account-restriction notifications. Every notification has an `event` of `ACCOUNT_RESTRICTION` and uses the same payload shape; the `violation_type` value — and whether a `restriction_info` object is present — identifies the restriction stage.

#### Warning (no restriction)

A warning carries no `restriction_info`. The `violation_type` is `DIRECT_SEND_UTILITY_CATEGORY_ABUSE_WARN` or `DIRECT_SEND_UTILITY_TEMPLATE_ABUSE`.

```json
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;WHATSAPP-BUSINESS-ACCOUNT-ID&quot;,
      &quot;time&quot;: &quot;TIMESTAMP&quot;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;event&quot;: &quot;ACCOUNT_RESTRICTION&quot;,
            &quot;violation_info&quot;: &#123;
              &quot;violation_type&quot;: &quot;DIRECT_SEND_UTILITY_CATEGORY_ABUSE_WARN&quot;
            &#125;
          &#125;,
          &quot;field&quot;: &quot;account_update&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

#### Active restriction (rate-limiting, 7-day, 30-day, or permanent)

For a strike with a rate-limiting restriction, a 7-day ban, a 30-day ban, or a permanent ban, the payload adds a `restriction_info` array, with one entry per active restriction carrying the `restriction_type` and an `expiration` timestamp. The `violation_type` is one of:

- `DIRECT_SEND_UTILITY_CATEGORY_ABUSE_RATE_LIMIT`
- `DIRECT_SEND_UTILITY_CATEGORY_ABUSE_STRIKE_1`
- `DIRECT_SEND_UTILITY_CATEGORY_ABUSE_STRIKE_2`
- `DIRECT_SEND_UTILITY_CATEGORY_ABUSE_OFFBOARD`

```json
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;WHATSAPP-BUSINESS-ACCOUNT-ID&quot;,
      &quot;time&quot;: &quot;TIMESTAMP&quot;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;event&quot;: &quot;ACCOUNT_RESTRICTION&quot;,
            &quot;violation_info&quot;: &#123;
              &quot;violation_type&quot;: &quot;DIRECT_SEND_UTILITY_CATEGORY_ABUSE_RATE_LIMIT&quot;
            &#125;,
            &quot;restriction_info&quot;: [
              &#123;
                &quot;restriction_type&quot;: &quot;RESTRICTED_DIRECT_SEND_UTILITY_TEMPLATES&quot;,
                &quot;expiration&quot;: &quot;&lt;expiration_timestamp&gt;&quot;
              &#125;
            ]
          &#125;,
          &quot;field&quot;: &quot;account_update&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

#### Restriction lifted

When a restriction is lifted, the payload carries no `restriction_info`. The `violation_type` is `DIRECT_SEND_UTILITY_CATEGORY_ABUSE_UNBAN` (ban lifted) or `DIRECT_SEND_UTILITY_CATEGORY_ABUSE_RATE_LIMIT_RECOVERY` (rate limit recovered).

```json
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;WHATSAPP-BUSINESS-ACCOUNT-ID&quot;,
      &quot;time&quot;: &quot;TIMESTAMP&quot;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;event&quot;: &quot;ACCOUNT_RESTRICTION&quot;,
            &quot;violation_info&quot;: &#123;
              &quot;violation_type&quot;: &quot;DIRECT_SEND_UTILITY_CATEGORY_ABUSE_UNBAN&quot;
            &#125;
          &#125;,
          &quot;field&quot;: &quot;account_update&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

### Error when sending during a restriction

If you attempt to send during a restriction period, the Cloud API synchronously returns error `139200`:

```json
&#123;
  &quot;error&quot;: &#123;
    &quot;message&quot;: &quot;(#139200) Direct Send Utility access is blocked&quot;,
    &quot;type&quot;: &quot;OAuthException&quot;,
    &quot;code&quot;: 139200,
    &quot;error_data&quot;: &#123;
      &quot;messaging_product&quot;: &quot;whatsapp&quot;,
      &quot;details&quot;: &quot;Direct Send Access is restricted: Direct send messaging capability is not available for this WABA right now due to misclassification based enforcements.&quot;
    &#125;,
    &quot;fbtrace_id&quot;: &quot;ARTCDsilOnw0CnIEuIq4_No&quot;
  &#125;
&#125;
```

## Related

- [Error codes](https://developers.facebook.com/documentation/business-messaging/whatsapp/direct-send/api-reference)
- [Send sample message payloads](https://developers.facebook.com/documentation/business-messaging/whatsapp/direct-send/send-sample-payloads)
