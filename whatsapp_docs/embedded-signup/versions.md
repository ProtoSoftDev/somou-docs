# Versions



**Warning:** **Embedded signup v2 will be deprecated on October 15, 2026.** Migrate your integration to [v4](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/version-4) before that date to avoid disruption. See [Versions](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/versions) for the full upgrade path.

The latest Embedded Signup Version is:  `v4`

This guide provides an overview on versioning in Embedded Signup. The versioning cadence will align with Graph API. The versions are not
exclusive, partners can gradually roll out a new version of ES to reduce risk. The Embedded Signup version is determined inside of the **extras object** of the implementation code.

**Note: The refreshed UI, currently available in the Public Preview, will be rolled out to all versions of Embedded Signup in early September.**

## Available ES versions

| Version | Date Introduced | Available Until |
| --- | --- | --- |
| [v4-public-preview](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/version-4-public-preview) | May 12, 2026 | TBD |
| [v4](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/version-4) | October 8th, 2025 | TBD |
| [v3-public-preview](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/version-3-public-preview) | August 14, 2025 | October, 2026 |
| [v2-public-preview](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/version-2-public-preview) | August 14, 2025 | October, 2026 |
| [v3](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/version-3) | May 29, 2025 | October, 2026 |
| v2 | January 2023 | October, 2026 |

## Overview of feature availability

| Version | Feature types | Features | Completion State | Prefilled Info | [Session Info Logging](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/implementation#session-logging-message-event-listener) | Products (via login config) |
| --- | --- | --- | --- | --- | --- | --- |
| `v4` | `whatsapp_business_app_onboarding` | `app_only_install` | Users can finish with a verified, unverified, or no phone number | Can fill business information, no screens will be skipped. | Sent back for all flows | Marketing Messages API for WhatsApp (MM API for WhatsApp)&lt;br&gt;&lt;br&gt;Click to WhatsApp Ads (CTWA)&lt;br&gt;&lt;br&gt;Conversions API (WhatsApp) |
| `v3-public-preview` | `whatsapp_business_app_onboarding` | `app_only_install`&lt;br&gt;&lt;br&gt;`marketing_messages_lite` | Users can finish with a verified, unverified, or no phone number | Can fill business information, no screens will be skipped. | Sent back for all flows | Not supported |
| `v2-public-preview` | `only_waba_sharing`&lt;br&gt;&lt;br&gt;`whatsapp_business_app_onboarding`&lt;br&gt;&lt;br&gt;`marketing_messages_lite` | `app_only_install`&lt;br&gt;&lt;br&gt;`marketing_messages_lite` | Users will finish with a verified phone number | Can fill business information, no screens will be skipped. | Sent back for all flows | Not supported |
| `v3` | `whatsapp_business_app_onboarding` | `app_only_install`&lt;br&gt;&lt;br&gt;`marketing_messages_lite` | Users can finish with a verified, unverified, or no phone number | Can fill business information, no screens will be skipped. | Sent back for all flows | Not supported |
| `v2` | `only_waba_sharing`&lt;br&gt;&lt;br&gt;`whatsapp_business_app_onboarding`&lt;br&gt;&lt;br&gt;`marketing_messages_lite` | `marketing_messages_lite` | Users will finish with a verified phone number | Can fill business information and skip screens | Partners are required to add a `sessionInfoVersion` to receive the callback | Not supported |

## Version 4 Public Preview
**Released:** May 12, 2026 | **Available until:** TBD | [Detailed changes](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/version-4-public-preview)

**Extras Configuration**

```js
extras: &#123;
    version: &quot;v4-public-preview&quot;
&#125;
```

Set `version` to `v4-public-preview` to opt into the preview. All other product configuration is handled by your Facebook Login for Business configuration.

**Login Configuration**

To use Embedded Signup V4 public preview: You need to create a new [Facebook Login for Business Configuration](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/implementation/#step-2-create-a-facebook-login-for-business-configuration), and select your desired products.


## Version 4
**Released:** October 8, 2025 | [Detailed changes](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/version-4)

**Login Configuration**

To use v4: You need to create a new [Facebook Login for Business Configuration](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/implementation/#step-2-create-a-facebook-login-for-business-configuration), and select your desired products. Selecting the products will automatically set you to v4.

**Extras Configuration**

```js
extras: &#123;&#125; // The extras object is purposely empty for v4.
```

## Version 3 Public Preview
**Released:** August 14, 2025 | **Available until:** October, 2026 | [Detailed changes](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/version-3-public-preview)

**Extras Configuration**

```js
extras: &#123;
    setup: &quot;&lt;SETUP_DATA&gt;&quot;,
    features: [
      &#123;
        name: &quot;&lt;FEATURE_NAME&gt;&quot;
      &#125;
    ],
    featureType: &quot;&lt;FEATURE_TYPE&gt;&quot;,
    version: &quot;&lt;VERSION&gt;&quot;
&#125;
```

| Placeholder | Description | Example Value |
| --- | --- | --- |
| `&lt;PREFILL_DATA&gt;` | Prefilled business data to inject data into Embedded Signup. | ```js
&#123;
  &quot;business&quot;: &#123;
  &quot;name&quot;: &quot;Jaspers Market&quot;
   &#125;
&#125;
``` |
| `&lt;FEATURE_NAME&gt;` | Indicates a flow or feature to enable. | **Possible Values:**&lt;br&gt;&lt;br&gt;`app_only_install` — Allows partners to access WABAs via API using a granular token (BISU), without creating a (SUAT)&lt;br&gt;&lt;br&gt;`marketing_messages_lite` — Enables the [Marketing Messages API for WhatsApp onboarding](https://developers.facebook.com/documentation/business-messaging/whatsapp/marketing-messages/overview) flow. |
| `&lt;FEATURE_TYPE&gt;` | Indicates a flow or feature to enable. | **Possible Values:**&lt;br&gt;&lt;br&gt;`whatsapp_business_app_onboarding` — Enables the [WhatsApp Business App phone number onboarding](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/onboarding-business-app-users) custom flow.&lt;br&gt;&lt;br&gt;Leave **blank** to enable the default onboarding flow. |
| `&lt;VERSION&gt;` | Indicates Embedded Signup version flow. | **Possible Values:**&lt;br&gt;&lt;br&gt;`v4-public-preview`,&lt;br&gt;`v3-public-preview`,&lt;br&gt;`v2-public-preview`,&lt;br&gt;`v3`,&lt;br&gt;`v2` |

## Version 2 Public Preview
**Released:** August 14, 2025 | **Available until:** October, 2026 | [Detailed changes](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/version-2-public-preview)

**Extras Configuration**

```js
extras: &#123;
    setup: &quot;&lt;SETUP_DATA&gt;&quot;,
    features: [
      &#123;
        name: &quot;&lt;FEATURE_NAME&gt;&quot;
      &#125;
    ],
    featureType: &quot;&lt;FEATURE_TYPE&gt;&quot;,
    sessionInfoVersion: &quot;&lt;SESSION_INFO_VERSION&gt;&quot;
    version: &quot;&lt;VERSION&gt;&quot;
&#125;
```

| Placeholder | Description | Example Value |
| --- | --- | --- |
| `&lt;PREFILL_DATA&gt;` | Prefilled business data to inject data into Embedded Signup. | ```js
&#123;
  &quot;business&quot;: &#123;
  &quot;name&quot;: &quot;Jaspers Market&quot;
   &#125;
&#125;
``` |
| `&lt;FEATURE_NAME&gt;` | Indicates a flow or feature to enable. | **Possible Values:**&lt;br&gt;&lt;br&gt;`app_only_install` — Allows partners to access WABAs via API using a granular token (BISU), without creating a (SUAT)&lt;br&gt;&lt;br&gt;`marketing_messages_lite` — Enables the [MM API for WhatsApp onboarding](https://developers.facebook.com/documentation/business-messaging/whatsapp/marketing-messages/overview) flow. |
| `&lt;FEATURE_TYPE&gt;` | Indicates a flow or feature to enable. | **Possible Values:**&lt;br&gt;&lt;br&gt;`whatsapp_business_app_onboarding` — Enables the [WhatsApp Business App phone number onboarding](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/onboarding-business-app-users) custom flow.&lt;br&gt;&lt;br&gt;`only_waba_sharing` - Enables the [WhatsApp Business App phone number onboarding](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/onboarding-business-app-users) custom flow. Will **NOT** show the new consolidated UI.&lt;br&gt;&lt;br&gt;`marketing_messages_lite` - Enables the [MM API for WhatsApp onboarding](https://developers.facebook.com/documentation/business-messaging/whatsapp/marketing-messages/overview) custom flow. Will **NOT** show the new consolidated UI.&lt;br&gt;&lt;br&gt;Leave **blank** to enable the default onboarding flow. |
| `&lt;SESSION_INFO_VERSION&gt;` | Indicates the returned session payload. | **Possible Values:**&lt;br&gt;&lt;br&gt;`3` |
| `&lt;VERSION&gt;` | Indicates Embedded Signup version flow. | **Possible Values:**&lt;br&gt;&lt;br&gt;`v4-public-preview`,&lt;br&gt;`v3-public-preview`,&lt;br&gt;`v2-public-preview`,&lt;br&gt;`v3`,&lt;br&gt;`v2` |

## Version 3

**Released:** May 29, 2025 | **Available until:** October, 2026 | [Blog post](https://developers.facebook.com/blog/post/2025/05/29/introducing-graph-api-v23-and-marketing-api-v23/) | [Detailed changes](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/version-3)

**Extras Configuration**

```js
extras: &#123;
    setup: &quot;&lt;SETUP_DATA&gt;&quot;,
    features: [
      &#123;
        name: &quot;&lt;FEATURE_NAME&gt;&quot;
      &#125;
    ],
    featureType: &quot;&lt;FEATURE_TYPE&gt;&quot;,
    version: &quot;v3&quot;
&#125;
```

| Placeholder | Description | Example Value |
| --- | --- | --- |
| `&lt;PREFILL_DATA&gt;` | Prefilled business data to inject data into Embedded Signup. | ```js
&#123;
  &quot;business&quot;: &#123;
  &quot;name&quot;: &quot;Jaspers Market&quot;
   &#125;
&#125;
``` |
| `&lt;FEATURE_NAME&gt;` | Indicates a flow or feature to enable. | **Possible Values:**&lt;br&gt;&lt;br&gt;`app_only_install` — Allows partners to access WABAs via API using a granular token (BISU), without creating a (SUAT)&lt;br&gt;&lt;br&gt;`marketing_messages_lite` — Enables the [MM API for WhatsApp onboarding](https://developers.facebook.com/documentation/business-messaging/whatsapp/marketing-messages/overview) flow. |
| `&lt;FEATURE_TYPE&gt;` | Indicates a flow or feature to enable. | **Possible Values:**&lt;br&gt;&lt;br&gt;`whatsapp_business_app_onboarding` — Enables the [WhatsApp Business App phone number onboarding](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/onboarding-business-app-users) custom flow.&lt;br&gt;&lt;br&gt;Leave **blank** to enable the default onboarding flow. |
| `&lt;VERSION&gt;` | Indicates Embedded Signup version flow. | **Possible Values:**&lt;br&gt;&lt;br&gt;`v4-public-preview`,&lt;br&gt;`v3-public-preview`,&lt;br&gt;`v2-public-preview`,&lt;br&gt;`v3`,&lt;br&gt;`v2` |

## Version 2

**Released:** January 2023 | **Available until:** October, 2026

**Extras Configuration**

```js
extras: &#123;
    setup: &quot;&lt;SETUP_DATA&gt;&quot;,
    features: [
      &#123;
        name: &quot;&lt;FEATURE_NAME&gt;&quot;
      &#125;
    ],
    featureType: &quot;&lt;FEATURE_TYPE&gt;&quot;,
    sessionInfoVersion: &quot;&lt;SESSION_INFO_VERSION&gt;&quot;,
&#125;
```

Head to the [pre-fill screens page](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/pre-filled-data#injecting-data) to see how to inject customer business data into Embedded Signup.

| Placeholder | Description | Example Value |
| --- | --- | --- |
| `&lt;PREFILL_DATA&gt;` | Prefilled business data to inject data into Embedded Signup. | ```js
&#123;
  &quot;business&quot;: &#123;
  &quot;name&quot;: &quot;Jaspers Market&quot;
   &#125;
&#125;
``` |
| `&lt;FEATURE_NAME&gt;` | Indicates a flow or feature to enable. | **Possible Values:**&lt;br&gt;&lt;br&gt;`app_only_install` — Allows partners to access WABAs via API using a granular token (BISU), without creating a (SUAT)&lt;br&gt;&lt;br&gt;`marketing_messages_lite` — Enables the [MM API for WhatsApp onboarding](https://developers.facebook.com/documentation/business-messaging/whatsapp/marketing-messages/overview) flow. |
| `&lt;FEATURE_TYPE&gt;` | Indicates a flow or feature to enable. | **Possible Values:**&lt;br&gt;&lt;br&gt;`whatsapp_business_app_onboarding` — Enables the [WhatsApp Business App phone number onboarding](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/onboarding-business-app-users) custom flow.&lt;br&gt;&lt;br&gt;`only_waba_sharing` - Enables the [WhatsApp Business App phone number onboarding](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/onboarding-business-app-users) custom flow.&lt;br&gt;&lt;br&gt;`marketing_messages_lite` - Enables the [MM API for WhatsApp onboarding](https://developers.facebook.com/documentation/business-messaging/whatsapp/marketing-messages/overview) custom flow.&lt;br&gt;&lt;br&gt;Leave **blank** to enable the default onboarding flow. |
| `&lt;SESSION_INFO_VERSION&gt;` | Indicates the returned session payload. | **Possible Values:**&lt;br&gt;&lt;br&gt;`3` |
| `&lt;VERSION&gt;` | Indicates Embedded Signup version flow. | **Possible Values:**&lt;br&gt;&lt;br&gt;`v4-public-preview`,&lt;br&gt;`v3-public-preview`,&lt;br&gt;`v2-public-preview`,&lt;br&gt;`v3`,&lt;br&gt;`v2` |

