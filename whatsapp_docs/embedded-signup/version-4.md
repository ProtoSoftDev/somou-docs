# Version 4



**Warning:** **Embedded signup v2 will be deprecated on October 15, 2026.** Migrate your integration to [v4](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/version-4) before that date to avoid disruption. See [Versions](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/versions) for the full upgrade path.

**Warning:** Release date: October 8, 2025. This page is updated as additional products become supported.

To upgrade to the v4 experience, you need to create a new [Facebook Login for Business Configuration](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/implementation/#step-2-create-a-facebook-login-for-business-configuration), and select your desired products. Selecting the products will automatically set you to v4.

See [screenshots](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/version-4#using-the-facebook-login-for-business-configuration-to-get-started-with-v4) below.

## Overview of v4 changes
- Simplified onboarding experience for businesses:
  - You can onboard businesses to more business messaging in a single flow ([see supported products](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/version-4#supported-products)).
  - Asset selection, business information, and permissions are each consolidated onto a single page.
  - Asset admins can share assets from other business portfolios.
  - The flow auto-links phone numbers to Facebook Pages when onboarding to ads that click to WhatsApp via the Marketing API.
  - Value proposition and Terms of Service are clearly presented.
- The [Facebook Login for Business Configuration](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/version-4#using-the-facebook-login-for-business-configuration-to-get-started-with-v4) is used to define which products to add into your onboarding flow.

## Learn more

- [v4 - Cloud API flow](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/default-flow)

## Supported products

v4 supports additional business messaging products, enabling businesses to set up and manage multiple communication channels from a single platform:

- **Conversions API (WhatsApp, Instagram, Messenger)**: Track and optimize messaging interactions by selecting the messaging platform you want to monitor, enabling enhanced measurement and optimization.
- **Click to WhatsApp Ads (CTWA)**: Create ads that direct users to initiate WhatsApp conversations with your business.
- **Click to Messenger Ads (CTM)**: Run advertising campaigns that start conversations with users on Facebook Messenger.
- **Click to Direct Ads (CTD)**: Launch Instagram ad campaigns that drive users to direct messaging conversations on Instagram Direct.

## All other supported products

v4 continues to support existing business messaging products, allowing businesses to manage their established communication channels.

 - **Cloud API**: Integrate and manage WhatsApp messaging at scale, enabling businesses to send and receive messages, automate workflows, and access advanced messaging features.
 - **Marketing Messages API for WhatsApp**: Use this API to manage optimized marketing messaging, providing tools for message analytics, and enhanced customer engagement.
 - **WhatsApp Business app user onboarding**: Onboarding WhatsApp Business app users continues to be supported through the [`feature_type` parameter](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/onboarding-business-app-users#step-2--customize-embedded-signup).
 - **Partner-led Business Verification (PLBV) support**: [PLBV](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/partner-led-business-verification) enables partners to verify businesses after onboarding via Embedded Signup. If you are considering this option, ensure you are an approved Select Solution or Premier Solution Partner, and [approved for access](https://www.facebook.com/business/help/1091073752691122).
- **Automatic Events API**: [Automatic Events API](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/automatic-events-api) notifies your application about key events that occur through Click-to-WhatsApp ads.

## Use the Facebook Login for Business Configuration to get started with v4

v4 enables you to easily set up and change which products you want to include in your onboarding flow:

Step 1: Navigate to [App Dashboard](https://developers.facebook.com/apps) &gt; **Facebook Login for Business** &gt; **Configurations** to create a new configuration.

Step 2: Select **Embedded Signup** as the login variation.

Step 3: Select which products you want to include in your onboarding flow. Selecting more than one product is optional.

Step 4: Copy the configuration id to use inside the Facebook Login SDK.

## Required assets and permissions

When you select products for v4, the flow automatically selects all necessary permissions and assets. You will need advanced access for all permissions automatically selected in the flow. If needed, you can select additional assets and permissions. The table below is a reference on what assets and permissions you need depending on what product you would like to offer.

| Product | Required assets | Required permissions (Advanced Access) |
| --- | --- | --- |
| [Cloud API](https://developers.facebook.com/documentation/business-messaging/whatsapp/about-the-platform#whatsapp-cloud-api) | WhatsApp Business accounts | whatsapp_business_management&lt;br&gt;&lt;br&gt;whatsapp_business_messaging |
| [Click to WhatsApp (CTWA on Marketing API)](https://developers.facebook.com/documentation/ads-commerce/marketing-api/ad-creative/messaging-ads/click-to-whatsapp) | WhatsApp Business accounts&lt;br&gt;&lt;br&gt;Facebook Pages&lt;br&gt;&lt;br&gt;Ad accounts | ads_read&lt;br&gt;&lt;br&gt;ads_management&lt;br&gt;&lt;br&gt;pages_manage_ads&lt;br&gt;&lt;br&gt;pages_read_engagement&lt;br&gt;&lt;br&gt;pages_show_list |
| [Click to Messenger (CTM on MAPI)](https://developers.facebook.com/documentation/ads-commerce/marketing-api/ad-creative/messaging-ads/click-to-messenger) | Facebook Pages&lt;br&gt;&lt;br&gt;Ad accounts | ads_management&lt;br&gt;&lt;br&gt;pages_manage_ads&lt;br&gt;&lt;br&gt;pages_read_engagement&lt;br&gt;&lt;br&gt;pages_show_list |
| [Click to Instagram (CTD on MAPI)](https://developers.facebook.com/documentation/ads-commerce/marketing-api/ad-creative/messaging-ads/click-to-instagram) | Facebook Pages&lt;br&gt;&lt;br&gt;Ad accounts&lt;br&gt;&lt;br&gt;Instagram accounts | ads_management&lt;br&gt;&lt;br&gt;pages_manage_ads&lt;br&gt;&lt;br&gt;pages_read_engagement&lt;br&gt;&lt;br&gt;pages_show_list |
| [Marketing Messages API for WhatsApp](https://developers.facebook.com/documentation/business-messaging/whatsapp/marketing-messages/overview) | WhatsApp Business accounts | whatsapp_business_management&lt;br&gt;&lt;br&gt;whatsapp_business_messaging |
| [Conversions API for CTWA](https://developers.facebook.com/documentation/ads-commerce/conversions-api/business-messaging#ads-that-click-to-whatsapp) | WhatsApp Business accounts&lt;br&gt;&lt;br&gt;Pixels | whatsapp_business_manage_events |
| [Conversions API for CTM](https://developers.facebook.com/documentation/ads-commerce/conversions-api/business-messaging#ads-that-click-to-messenger) | Facebook Pages&lt;br&gt;&lt;br&gt;Ad accounts&lt;br&gt;&lt;br&gt;Pixels | page_events |
| [Conversions API for CTD](https://developers.facebook.com/documentation/ads-commerce/conversions-api/business-messaging#ads-that-click-to-instagram-direct) | Facebook Pages&lt;br&gt;&lt;br&gt;Ad accounts&lt;br&gt;&lt;br&gt;Instagram accounts&lt;br&gt;&lt;br&gt;Pixels | instagram_manage_events |

