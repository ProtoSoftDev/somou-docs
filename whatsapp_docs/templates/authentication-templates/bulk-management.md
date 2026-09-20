# Bulk management



Use the [Upsert Message Templates API](https://developers.facebook.com/docs/graph-api/reference/whats-app-business-account/upsert_message_templates#Creating) to bulk update or create authentication templates in multiple languages that include or exclude the optional security and expiration warnings.

If a template already exists with a matching name and language, the API updates the template with the contents of the request. Otherwise, the API creates a new template.

## Request syntax

```html
POST /&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;/upsert_message_templates
```

## Post body

```html
&#123;
  &quot;name&quot;: &quot;&lt;NAME&gt;&quot;,
  &quot;languages&quot;: [&lt;LANGUAGES&gt;],
  &quot;category&quot;: &quot;AUTHENTICATION&quot;,
  &quot;components&quot;: [
    &#123;
      &quot;type&quot;: &quot;BODY&quot;,
      &quot;add_security_recommendation&quot;: &lt;ADD_SECURITY_RECOMMENDATION&gt; // Optional
    &#125;,
    &#123;
      &quot;type&quot;: &quot;FOOTER&quot;,
      &quot;code_expiration_minutes&quot;: &lt;CODE_EXPIRATION_MINUTES&gt; // Optional
    &#125;,
    &#123;
      &quot;type&quot;: &quot;BUTTONS&quot;,
      &quot;buttons&quot;: [
        &#123;
          &quot;type&quot;: &quot;OTP&quot;,
          &quot;otp_type&quot;: &quot;&lt;OTP_TYPE&gt;&quot;,
          &quot;supported_apps&quot;: [
            &#123;
              &quot;package_name&quot;: &quot;&lt;PACKAGE_NAME&gt;&quot;, // One-tap and zero-tap buttons only
              &quot;signature_hash&quot;: &quot;&lt;SIGNATURE_HASH&gt;&quot; // One-tap and zero-tap buttons only
            &#125;
          ]
        &#125;
      ]
    &#125;
  ]
&#125;
```

## Properties

All [template creation properties](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/authentication-templates/authentication-templates#properties) are supported, with these exceptions:

* The `language` property is not supported. Instead, use `languages` and set its value to an array of [language and locale code](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/supported-languages) strings. For example: `[&quot;en_US&quot;,&quot;es_ES&quot;,&quot;fr&quot;]`.
* The `text` property is not supported.
* The `autofill_text` property is not supported.

## Example copy code request

This example creates three authentication templates in English, Spanish, and French, with copy code buttons. Each template is named &quot;authentication_code_copy_code_button&quot; and includes the security recommendation and expiration time.

```curl
curl &#039;https://graph.facebook.com/v25.0/102290129340398/upsert_message_templates&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039; \
-d &#039;
&#123;
  &quot;name&quot;: &quot;authentication_code_copy_code_button&quot;,
  &quot;languages&quot;: [&quot;en_US&quot;,&quot;es_ES&quot;,&quot;fr&quot;],
  &quot;category&quot;: &quot;AUTHENTICATION&quot;,
  &quot;components&quot;: [
    &#123;
      &quot;type&quot;: &quot;BODY&quot;,
      &quot;add_security_recommendation&quot;: true
    &#125;,
    &#123;
      &quot;type&quot;: &quot;FOOTER&quot;,
      &quot;code_expiration_minutes&quot;: 10
    &#125;,
    &#123;
      &quot;type&quot;: &quot;BUTTONS&quot;,
      &quot;buttons&quot;: [
        &#123;
          &quot;type&quot;: &quot;OTP&quot;,
          &quot;otp_type&quot;: &quot;COPY_CODE&quot;
        &#125;
      ]
    &#125;
  ]
&#125;&#039;
```

## Example one-tap autofill request

This example (1) updates an existing template with the name &quot;authentication_code_autofill_button&quot; and language &quot;en_US&quot;, and (2) creates two new authentication templates in Spanish and French with one-tap autofill buttons. Both newly created templates are named &quot;authentication_code_autofill_button&quot; and include the security recommendation and expiration time.

```curl
curl &#039;https://graph.facebook.com/v25.0/102290129340398/upsert_message_templates&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039; \
-d &#039;
&#123;
  &quot;name&quot;: &quot;authentication_code_autofill_button&quot;,
  &quot;languages&quot;: [&quot;en_US&quot;,&quot;es_ES&quot;,&quot;fr&quot;],
  &quot;category&quot;: &quot;AUTHENTICATION&quot;,
  &quot;components&quot;: [
    &#123;
      &quot;type&quot;: &quot;BODY&quot;,
      &quot;add_security_recommendation&quot;: true
    &#125;,
    &#123;
      &quot;type&quot;: &quot;FOOTER&quot;,
      &quot;code_expiration_minutes&quot;: 15
    &#125;,
    &#123;
      &quot;type&quot;: &quot;BUTTONS&quot;,
      &quot;buttons&quot;: [
        &#123;
          &quot;type&quot;: &quot;OTP&quot;,
          &quot;otp_type&quot;: &quot;ONE_TAP&quot;,
          &quot;supported_apps&quot;: [
            &#123;
              &quot;package_name&quot;: &quot;com.example.luckyshrub&quot;,
              &quot;signature_hash&quot;: &quot;K8a/AINcGX7&quot;
            &#125;
          ]
        &#125;
      ]
    &#125;
  ]
&#125;&#039;
```

## Example response

```json
&#123;
  &quot;data&quot;: [
    &#123;
      &quot;id&quot;: &quot;954638012257287&quot;,
      &quot;status&quot;: &quot;APPROVED&quot;,
      &quot;language&quot;: &quot;en_US&quot;
    &#125;,
    &#123;
      &quot;id&quot;: &quot;969725527415202&quot;,
      &quot;status&quot;: &quot;APPROVED&quot;,
      &quot;language&quot;: &quot;es_ES&quot;
    &#125;,
    &#123;
      &quot;id&quot;: &quot;969725530748535&quot;,
      &quot;status&quot;: &quot;APPROVED&quot;,
      &quot;language&quot;: &quot;fr&quot;
    &#125;
  ]
&#125;
```
