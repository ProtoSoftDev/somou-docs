# Template previews



You can generate previews of authentication template text in various languages that include or exclude the security recommendation string and code expiration string using the [Message Template Previews API](https://developers.facebook.com/docs/graph-api/reference/whats-app-business-account/message_template_previews#Reading).

### Request syntax

```html
GET /&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;/message_template_previews
  ?category=AUTHENTICATION,
  &amp;language=&lt;LANGUAGE&gt;, // Optional
  &amp;add_security_recommendation=&lt;ADD_SECURITY_RECOMMENDATION&gt;, // Optional
  &amp;code_expiration_minutes=&lt;CODE_EXPIRATION_MINUTES&gt;, // Optional
  &amp;button_types=&lt;BUTTON_TYPES&gt; // Optional
```

### Request parameters

| Placeholder | Description | Example value |
| --- | --- | --- |
| `&lt;LANGUAGE&gt;`&lt;br&gt;&lt;br&gt;_Comma-separated list_ | **Optional.**&lt;br&gt;&lt;br&gt;Comma-separated list of [language and locale codes](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/supported-languages) of language versions you want returned.&lt;br&gt;&lt;br&gt;If omitted, the API returns all supported language versions. | `en_US,es_ES` |
| `&lt;ADD_SECURITY_RECOMMENDATION&gt;`&lt;br&gt;&lt;br&gt;_Boolean_ | **Optional.**&lt;br&gt;&lt;br&gt;Set to `true` if you want the security recommendation body string included in the response.&lt;br&gt;&lt;br&gt;If omitted, the API omits the security recommendation string. | `true` |
| `&lt;CODE_EXPIRATION_MINUTES&gt;`&lt;br&gt;&lt;br&gt;_Int64_ | **Optional.**&lt;br&gt;&lt;br&gt;Set to an integer if you want the code expiration footer string included in the response.&lt;br&gt;&lt;br&gt;If omitted, the API omits the code expiration footer string.&lt;br&gt;&lt;br&gt;Value indicates number of minutes until code expires.&lt;br&gt;&lt;br&gt;Minimum `1`, maximum `90`. | `10` |
| `&lt;BUTTON_TYPES&gt;`&lt;br&gt;&lt;br&gt;_Comma-separated list of strings_ | **Required.**&lt;br&gt;&lt;br&gt;Comma-separated list of strings indicating button type.&lt;br&gt;&lt;br&gt;If included, the response includes the button text for each button.&lt;br&gt;&lt;br&gt;For authentication templates, this value must be `OTP`. | `OTP` |

### Example request

```curl
curl &#039;https://graph.facebook.com/v17.0/102290129340398/message_template_previews?category=AUTHENTICATION&amp;languages=en_US,es_ES&amp;add_security_recommendation=true&amp;code_expiration_minutes=10&amp;button_types=OTP&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039;
```

### Example response

```json
&#123;
  &quot;data&quot;: [
    &#123;
      &quot;body&quot;: &quot;*&#123;&#123;1&#125;&#125;* is your verification code. For your security, do not share this code.&quot;,
      &quot;buttons&quot;: [
        &#123;
          &quot;autofill_text&quot;: &quot;Autofill&quot;,
          &quot;text&quot;: &quot;Copy code&quot;
        &#125;
      ],
      &quot;footer&quot;: &quot;This code expires in 10 minutes.&quot;,
      &quot;language&quot;: &quot;en_US&quot;
    &#125;,
    &#123;
      &quot;body&quot;: &quot;Tu código de verificación es *&#123;&#123;1&#125;&#125;*. Por tu seguridad, no lo compartas.&quot;,
      &quot;buttons&quot;: [
        &#123;
          &quot;autofill_text&quot;: &quot;Autocompletar&quot;,
          &quot;text&quot;: &quot;Copiar código&quot;
        &#125;
      ],
      &quot;footer&quot;: &quot;Este código caduca en 10 minutos.&quot;,
      &quot;language&quot;: &quot;es_ES&quot;
    &#125;
  ]
&#125;
```
