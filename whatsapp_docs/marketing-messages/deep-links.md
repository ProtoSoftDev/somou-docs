# Deep links



You can map an [Android deep link](https://developer.android.com/training/app-links/deep-linking) to a marketing template URL button that, when tapped, loads a particular location or content within your app.

If you have not onboarded to the Marketing Messages API for WhatsApp (MM API for WhatsApp), your marketing templates will not display any conversion metrics. Learn more about how to [measure conversion](https://developers.facebook.com/documentation/business-messaging/whatsapp/marketing-messages/measure-conversion).

## Template creation via WhatsApp Manager

To create a template with a button mapped to an Android deep link:

1. Access [WhatsApp Manager](https://business.facebook.com/latest/whatsapp_manager/).
1. Navigate to the **Message templates** &gt; **Manage templates** panel and click the **Create template** button.
1. Select **Marketing** (tab) &gt; **Custom** (radio button) and click the **Next** button.
1. In the **Buttons** section, click the **+ Add buttons** dropdown menu and select **Visit website**.
1. Check the **Track app conversions** checkbox to reveal the deep link fields (pictured below).
1. Complete each field using their tooltips or [form field](#form-fields) descriptions below as guidance.
1. Add any additional components you&#039;d like your template to use, name your template, and submit it for approval.

Note that you can also use the **Manage templates** panel to edit an existing template and add a deep link-mapped button, but the template will have to undergo template review again.

### Form fields

| Field label | Description | Example value |
| --- | --- | --- |
| Android deep link | **Required.**&lt;br&gt;Android deep link URI. | luckyshrub://deals/`summer_solstice` |
| Android fallback URL | **Optional.**&lt;br&gt;Fallback URL.&lt;br&gt;If the WhatsApp client cannot load the deep link URI, the WhatsApp client loads this URL in the device&#039;s default web browser.&lt;br&gt;&lt;br&gt;If omitted, the WhatsApp client loads the URL specified in the Website URL field instead. | https://www.luckyshrub.com/deals/`summer_solstice` |
| Button Text | **Required.**&lt;br&gt;Button label text.&lt;br&gt;Maximum 25 characters. | View deal |
| Meta app ID | **Required.**&lt;br&gt;This is a list of the Meta app(s) associated with your business portfolio. Select the app whose access token you will use to send the template. | Lucky Shrub (634974688087057) |
| Type of Action | **Required.**&lt;br&gt;&lt;br&gt;Must be set to **Visit website**. | Visit website |
| URL Type | **Required.**&lt;br&gt;Set to **Static** if your Android deep link or Android fallback URL has no dynamic values, otherwise set to **Dynamic**. | Static |
| Website URL | **Required.**&lt;br&gt;&lt;br&gt;URL of a website to load if the WhatsApp user views the message on a non-Android device, or if the WhatsApp client cannot load your Android deep link URI and no Android fallback URL is specified. | https://www.luckyshrub.com/ |

## Template creation via API

Use the [Message Templates API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/message-template-api#post-version-waba-id-message-templates) to create the template and include a URL button component mapped to your Android deep link.

Note that you can also use the [Template API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/message-template-api#post-version-template-id) to edit an existing template and add a URL button component, but the template will have to undergo template review again.

### Request syntax

Template components can vary based on your needs. This example syntax creates a marketing template with the following components:

- **text header**, without parameters
- **body**, without parameters
- **URL button**, mapped to a deep link URI and fallback URL

```html
&#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;/message_templates&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039; \
-d &#039;
&#123;
  &quot;name&quot;: &quot;&lt;TEMPLATE_NAME&gt;&quot;,
  &quot;language&quot;: &quot;&lt;TEMPLATE_LANGUAGE&gt;&quot;,
  &quot;category&quot;: &quot;marketing&quot;,
  &quot;components&quot;: [
    &#123;
      &quot;type&quot;: &quot;header&quot;,
      &quot;format&quot;: &quot;text&quot;,
      &quot;text&quot;: &quot;&lt;HEADER_TEXT&gt;&quot;
    &#125;,
    &#123;
      &quot;type&quot;: &quot;body&quot;,
      &quot;text&quot;: &quot;&lt;BODY_TEXT&gt;&quot;
    &#125;,
    &#123;
      &quot;type&quot;: &quot;buttons&quot;,
      &quot;buttons&quot;: [
        &#123;
          &quot;type&quot;: &quot;url&quot;,
          &quot;text&quot;: &quot;&lt;BUTTON_LABEL_TEXT&gt;&quot;,
          &quot;url&quot;: &quot;&lt;BUTTON_URL&gt;&quot;,
          &quot;app_deep_link&quot;: &#123;
            &quot;meta_app_id&quot;: &lt;META_APP_ID&gt;,
            &quot;android_deep_link&quot;: &quot;&lt;ANDROID_DEEP_LINK&gt;&quot;,
            &quot;android_fallback_playstore_url&quot;: &quot;&lt;FALLBACK_URL&gt;&quot;
          &#125;
        &#125;
      ]
    &#125;
  ]
&#125;&#039;
```

### Request parameters

| Placeholder | Description | Example value |
| --- | --- | --- |
| `&lt;ACCESS_TOKEN&gt;`&lt;br&gt;_String_ | **Required.**&lt;br&gt;[System token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#system-user-access-tokens) or [business token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#business-integration-system-user-access-tokens). | `EAAJB...` |
| `&lt;ANDROID_DEEP_LINK&gt;`&lt;br&gt;_String_ | **Required if using a URL button component mapped to a deep link.**&lt;br&gt;[Android deep link](https://developer.android.com/training/app-links/deep-linking) URI. The WhatsApp client loads this URI when the WhatsApp user taps the button on an Android device. | `luckyshrub://deals/summer/` |
| `&lt;API_VERSION&gt;`&lt;br&gt;_String_ | **Optional.**&lt;br&gt;Graph API [version](https://developers.facebook.com/docs/graph-api/guides/versioning). | `v22.0` |
| `&lt;BODY_TEXT&gt;`&lt;br&gt;_String_ | **Required if using a body component.**&lt;br&gt;Template body text. Variables are supported.&lt;br&gt;Maximum 1024 characters. | `Beat the heat with our sizzling summer deals on succulents! At Lucky Shrub, we...` |
| `&lt;BUTTON_LABEL_TEXT&gt;`&lt;br&gt;_String_ | **Required if using a URL button component.**&lt;br&gt;Button label text.&lt;br&gt;Maximum 25 characters. | `View Deals` |
| `&lt;BUTTON_URL&gt;`&lt;br&gt;_String_ | **Required if using a URL button component.**&lt;br&gt;URL of a website that the WhatsApp client loads in the device&#039;s default web browser when the user taps the button.&lt;br&gt;For deep links, the WhatsApp client uses this website URL only if the WhatsApp user taps the button on a non-Android device. | `https://www.luckyshrub.com/deals/summer/` |
| `&lt;FALLBACK_URL&gt;`&lt;br&gt;_String_ | **Required if using a URL button mapped to a deep link.**&lt;br&gt;URL of a website that the WhatsApp client loads in the device&#039;s default web browser when the user taps the button but the client cannot load the Android deep link URI. | `https://www.luckyshrub.com/deals/summer/` |
| `&lt;HEADER_TEXT&gt;`&lt;br&gt;_String_ | **Required if using a text header component.**&lt;br&gt;Template header text string.&lt;br&gt;Supports up to 1 parameter. If this string contains a parameter, you must include an `example` property.&lt;br&gt;Maximum 60 characters. | `Sizzling Summer Deals at Lucky Shrub` |
| `&lt;META_APP_ID&gt;`&lt;br&gt;_Integer_ | **Required if using a URL button mapped to a deep link.**&lt;br&gt;Your Meta app ID. | `634974688087057` |
| `&lt;TEMPLATE_LANGUAGE&gt;`&lt;br&gt;_String_ | **Required.**&lt;br&gt;Template [language and locale code](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/supported-languages). | `en_US` |
| `&lt;TEMPLATE_NAME&gt;`&lt;br&gt;_String_ | **Required.**&lt;br&gt;Template name.&lt;br&gt;Maximum 512 characters. | `summer_deals_deep_link_v1` |
| `&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;`&lt;br&gt;_String_ | **Required.**&lt;br&gt;WhatsApp Business account ID. | `102290129340398` |

## Example request

```curl
curl &#039;https://graph.facebook.com/v22.0/102290129340398/message_templates&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039; \
-d &#039;
&#123;
  &quot;name&quot;: &quot;summer_deals_deep_link_v1&quot;,
  &quot;language&quot;: &quot;en_US&quot;,
  &quot;category&quot;: &quot;marketing&quot;,
  &quot;components&quot;: [
    &#123;
      &quot;type&quot;: &quot;header&quot;,
      &quot;format&quot;: &quot;text&quot;,
      &quot;text&quot;: &quot;Sizzling Summer Deals at Lucky Shrub&quot;
    &#125;,
    &#123;
      &quot;type&quot;: &quot;body&quot;,
      &quot;text&quot;: &quot;Beat the heat with our sizzling summer deals on succulents! At Lucky Shrub, we&#039;re passionate about bringing a touch of greenery to your life. Our succulents are not only low-maintenance and easy to care for, but they also add a pop of color and style to any room. Use the button below to see our Summer Steals!&quot;
    &#125;,
    &#123;
      &quot;type&quot;: &quot;buttons&quot;,
      &quot;buttons&quot;: [
        &#123;
          &quot;type&quot;: &quot;url&quot;,
          &quot;text&quot;: &quot;View Deals&quot;,
          &quot;url&quot;: &quot;https://www.luckyshrub.com/deals/summer/&quot;,
          &quot;app_deep_link&quot;: &#123;
            &quot;meta_app_id&quot;: 634974688087057,
            &quot;android_deep_link&quot;: &quot;luckyshrub://deals/summer/&quot;,
            &quot;android_fallback_playstore_url&quot;: &quot;https://www.luckyshrub.com/deals/summer/&quot;
          &#125;
        &#125;
      ]
    &#125;
  ]
&#125;&#039;
```

## Viewing metrics

See our [Viewing metrics](https://developers.facebook.com/documentation/business-messaging/whatsapp/marketing-messages/view-metrics) document.
