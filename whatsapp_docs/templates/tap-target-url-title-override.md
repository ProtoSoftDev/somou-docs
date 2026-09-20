# Tap target title URL override



This document explains how to send approved message templates using the `tap_target_configuration` component within a template message. Tap target override enables image-based, text-based, and header-less message templates to function as interactive Call-to-Action URL buttons. These buttons display a custom title and open the destination linked to the first URL button.

WhatsApp Business Accounts (WABAs) must be fully verified and consistently maintain high-quality standards to ensure compliance and access to this component.

## Request syntax

Use the [Messages API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/message-api#post-version-phone-number-id-messages) to send a text message template to a WhatsApp user.

```html
curl &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;WHATSAPP_BUSINESS_PHONE_NUMBER_ID&gt;/messages&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039; \
-d &#039;
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;recipient_type&quot;: &quot;individual&quot;,
  &quot;to&quot;: &quot;&lt;WHATSAPP_USER_PHONE_NUMBER&gt;&quot;,
  &quot;type&quot;: &quot;template&quot;,
  &quot;template&quot;: &#123;
    &quot;name&quot;: &quot;&lt;TEMPLATE_NAME&gt;&quot;,
    &quot;language&quot;: &#123;
      &quot;code&quot;: &quot;&lt;LANGUAGE_AND_LOCALE_CODE&gt;&quot;
    &#125;,
    &quot;components&quot;: [
      &#123;
        &quot;type&quot;: &quot;tap_target_configuration&quot;,
        &quot;parameters&quot;: [
          &#123;
            &quot;type&quot;: &quot;tap_target_configuration&quot;,
            &quot;tap_target_configuration&quot;: [
              &#123;
                &quot;url&quot;: &quot;&lt;URL&gt;&quot;,
                &quot;title&quot;: &quot;&lt;TITLE&gt;&quot;
              &#125;
            ]
          &#125;
        ]
      &#125;,
          &lt;!-- Add additional components --&gt;
    ]
  &#125;
&#125;
```

### Request parameters

| Placeholder | Description | Example Value |
| --- | --- | --- |
| `&lt;ACCESS_TOKEN&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;[System token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#system-user-access-tokens) or [business token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#business-integration-system-user-access-tokens). | `EAAA...` |
| `&lt;API_VERSION&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Graph API version. | v25.0 |
| `&lt;LANGUAGE_AND_LOCAL_CODE&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Template [language and locale code](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/supported-languages). | `en` |
| `&lt;TEMPLATE_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Name of template. | `august_promotion` |
| `&lt;TITLE&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;URL Title. | `Offer Details!` |
| `&lt;URL&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;URL. | `https://www.luckyshrubs.com` |
| `&lt;WHATSAPP_BUSINESS_PHONE_NUMBER_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;WhatsApp business phone number ID. | `106540352242922` |
| `&lt;WHATSAPP_USER_PHONE_NUMBER&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;WhatsApp user phone number. | `+16505551234` |

## Example request

Example request to send a template message with the `tap_target_configuration` type.

```curl
curl &#039;https://graph.facebook.com/v25.0/106540352242922/messages&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039; \
-d &#039;
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;recipient_type&quot;: &quot;individual&quot;,
  &quot;to&quot;: &quot;+1233214532&quot;,
  &quot;type&quot;: &quot;template&quot;,
  &quot;template&quot;: &#123;
    &quot;name&quot;: &quot;august_promotion&quot;,
    &quot;language&quot;: &#123;
      &quot;code&quot;: &quot;en&quot;
    &#125;,
    &quot;components&quot;: [
      &#123;
        &quot;type&quot;: &quot;header&quot;,
        &quot;parameters&quot;: [
          &#123;
            &quot;type&quot;: &quot;image&quot;,
            &quot;image&quot;: &#123;
              &quot;link&quot;: &quot;https://www.luckyshrubs.com&quot;
            &#125;
          &#125;
        ]
      &#125;,
      &#123;
        &quot;type&quot;: &quot;body&quot;,
        &quot;parameters&quot;: [
          &#123;
            &quot;type&quot;: &quot;text&quot;,
            &quot;text&quot;: &quot;Hello Andy...&quot;
          &#125;
        ]
      &#125;,
      &#123;
        &quot;type&quot;: &quot;tap_target_configuration&quot;,
        &quot;parameters&quot;: [
          &#123;
            &quot;type&quot;: &quot;tap_target_configuration&quot;,
            &quot;tap_target_configuration&quot;: [
              &#123;
                &quot;url&quot;: &quot;https://www.luckyshrubs.com/&quot;,
                &quot;title&quot;: &quot;Offer Details&quot;
              &#125;
            ]
          &#125;
        ]
      &#125;
    ]
  &#125;
&#125;&#039;
```

## Example response

```curl
&#123;
   &quot;messaging_product&quot;: &quot;whatsapp&quot;,
   &quot;contacts&quot;: [
       &#123;
           &quot;input&quot;: &quot;+1233214532&quot;,
           &quot;wa_id&quot;: &quot;1233214532&quot;
       &#125;
   ],
   &quot;messages&quot;: [
       &#123;
           &quot;id&quot;: &quot;wamid.HBgLMTMyMzI4NjU2NzgVAgARGBJBQzRBRDBEMDEwQzVBM0M0QkIA&quot;,
           &quot;message_status&quot;: &quot;accepted&quot;
       &#125;
   ]
&#125;
```
