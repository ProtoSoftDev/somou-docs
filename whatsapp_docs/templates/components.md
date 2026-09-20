# Template components



Templates are made up of four primary components which you define when you create a template: header, body, footer, and buttons. The components you choose for each of your templates should be based on your business needs. The only required component is the body component.

Some components support variables, whose values you can supply when using the Cloud API to send the template in a template message. If your templates use variables, you must include sample variable values upon template creation.

## Text header

Text headers are optional elements that can be added to the top of template messages. Each template may include only one text header. Do not use Markdown special characters in this component.

Text headers support 1 [parameter](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/overview#parameter-formats).

### Creation syntax

```html
&lt;!-- No parameter syntax --&gt;
&#123;
  &quot;type&quot;: &quot;header&quot;,
  &quot;format&quot;: &quot;text&quot;,
  &quot;text&quot;: &quot;&lt;HEADER_TEXT&gt;&quot;
&#125;

&lt;!-- Named parameter syntax --&gt;
&#123;
  &quot;type&quot;: &quot;header&quot;,
  &quot;format&quot;: &quot;text&quot;,
  &quot;text&quot;: &quot;&lt;HEADER_TEXT&gt;&quot;,
  &quot;example&quot;: &#123;
    &quot;header_text_named_params&quot;: [
      &#123;
        &quot;param_name&quot;: &quot;&lt;NAMED_PARAMETER_NAME&gt;&quot;,
        &quot;example&quot;: &quot;&lt;PARAMETER_EXAMPLE_VALUE&gt;&quot;
      &#125;
    ]
  &#125;
&#125;

&lt;!-- Positional parameter syntax --&gt;
&#123;
  &quot;type&quot;: &quot;header&quot;,
  &quot;format&quot;: &quot;text&quot;,
  &quot;text&quot;: &quot;&lt;HEADER_TEXT&gt;&quot;,
  &quot;example&quot;: &#123;
    &quot;header_text&quot;: [
      &quot;&lt;PARAMETER_EXAMPLE_VALUE&gt;&quot;
    ]
  &#125;
&#125;
```

### Creation parameters

| Placeholder | Description | Example Value |
| --- | --- | --- |
| `&lt;HEADER_TEXT&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Header body text string. Supports 1 [parameter](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/overview#parameter-formats).&lt;br&gt;&lt;br&gt;If this string contains a parameter, you must include the `example` property and example parameter value.&lt;br&gt;&lt;br&gt;Maximum 60 characters. | `Our new sale starts &#123;&#123;sale_start_date&#125;&#125;!` |
| `&lt;NAMED_PARAMETER_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required if using a named parameter.**&lt;br&gt;&lt;br&gt;[Named parameter](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/overview#named-parameters) name. | `&#123;&#123;sale_start_date&#125;&#125;` |
| `&lt;PARAMETER_EXAMPLE_VALUE&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required if using a parameter.**&lt;br&gt;&lt;br&gt;[Parameter](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/overview#parameter-formats) example value. | `December 1st` |

### Creation example

This example uses 1 named parameter.

```json
&#123;
  &quot;type&quot;: &quot;HEADER&quot;,
  &quot;format&quot;: &quot;TEXT&quot;,
  &quot;text&quot;: &quot;Our new sale starts &#123;&#123;sale_start_date&#125;&#125;!&quot;,
  &quot;example&quot;: &#123;
    &quot;header_text_named_params&quot;: [
      &#123;
        &quot;param_name&quot;: &quot;sale_start_date&quot;,
        &quot;example&quot;: &quot;December 1st&quot;
      &#125;
    ]
  &#125;
&#125;
```

## Media header

Media headers can be an image, video, gif, or a document such as a PDF. You must upload all media with the [Resumable Upload API](https://developers.facebook.com/docs/graph-api/guides/upload). The syntax for defining a media header is the same for all media types.

Note: Gifs are only available for [Marketing Messages API for WhatsApp](https://developers.facebook.com/documentation/business-messaging/whatsapp/marketing-messages/features). Gifs are mp4 files with a max size of 3.5MB, and WhatsApp displays larger files as video messages.

### Creation syntax

```html
&#123;
  &quot;type&quot;: &quot;HEADER&quot;,
  &quot;format&quot;: &quot;&lt;FORMAT&gt;&quot;,
  &quot;example&quot;: &#123;
    &quot;header_handle&quot;: [
      &quot;&lt;HEADER_HANDLE&gt;&quot;
    ]
  &#125;
&#125;
```

### Creation parameters

| Placeholder | Description | Example Value |
| --- | --- | --- |
| `&lt;FORMAT&gt;` | Indicates media asset type. Set to `IMAGE`, `VIDEO`, `GIF`, or `DOCUMENT`. | `IMAGE` |
| `&lt;HEADER_HANDLE&gt;` | Uploaded media asset handle. Use the [Resumable Upload API](https://developers.facebook.com/docs/graph-api/guides/upload) to generate an asset handle. | `4::aW...` |

### Creation example

```json
&#123;
  &quot;type&quot;: &quot;HEADER&quot;,
  &quot;format&quot;: &quot;IMAGE&quot;,
  &quot;example&quot;: &#123;
    &quot;header_handle&quot;: [
      &quot;4::aW...&quot;
    ]
  &#125;
&#125;
```

## Location header

Location headers appear as generic maps at the top of the template and are useful for use cases such as order tracking, delivery updates, ride-hailing pickup/dropoff, and locating physical stores. When tapped, the app user&#039;s default map app opens and loads the specified location. You specify locations when you send the template.

Location headers can only be used in templates categorized as `UTILITY` or `MARKETING`. Real-time locations are not supported.

### Creation syntax

```json
&#123;
  &quot;type&quot;: &quot;header&quot;,
  &quot;format&quot;: &quot;location&quot;
&#125;
```

### Creation parameters

None.

### Creation example

```json
&#123;
  &quot;type&quot;: &quot;header&quot;,
  &quot;format&quot;: &quot;location&quot;
&#125;
```

### Send syntax

```html
&#123;
  &quot;type&quot;: &quot;header&quot;,
  &quot;parameters&quot;: [
    &#123;
      &quot;type&quot;: &quot;location&quot;,
      &quot;location&quot;: &#123;
        &quot;latitude&quot;: &quot;&lt;LOCATION_LATITUDE&gt;&quot;,
        &quot;longitude&quot;: &quot;&lt;LOCATION_LONGITUDE&gt;&quot;,
        &quot;name&quot;: &quot;&lt;LOCATION_NAME&gt;&quot;,
        &quot;address&quot;: &quot;&lt;LOCATION_ADDRESS&gt;&quot;
      &#125;
    &#125;
  ]
&#125;
```

### Send parameters

| Placeholder | Description | Sample Value |
| --- | --- | --- |
| `&lt;LOCATION_ADDRESS&gt;` | Location address. | `101 Forest Ave, Palo Alto, CA 94301` |
| `&lt;LOCATION_LATITUDE&gt;` | Location latitude in decimal degrees. | `37.44211676562361` |
| `&lt;LOCATION_LONGITUDE&gt;` | Location longitude in decimal degrees. | `122.16155960083124` |
| `&lt;LOCATION_NAME&gt;` | Location name. | `Philz Coffee` |

### Send example

```json
&#123;
  &quot;type&quot;: &quot;header&quot;,
  &quot;parameters&quot;: [
    &#123;
      &quot;type&quot;: &quot;location&quot;,
      &quot;location&quot;: &#123;
        &quot;latitude&quot;: &quot;37.44211676562361&quot;,
        &quot;longitude&quot;: &quot;-122.16155960083124&quot;,
        &quot;name&quot;: &quot;Philz Coffee&quot;,
        &quot;address&quot;: &quot;101 Forest Ave, Palo Alto, CA 94301&quot;
      &#125;
    &#125;
  ]
&#125;
```

## Body

The body component represents the core text of your message template and is a text-only template component. Templates are limited to one body component.

The message text in the body component accepts multiple [parameters](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/overview#parameter-formats).

### Creation syntax

```html
&lt;!-- No parameters syntax --&gt;
&#123;
  &quot;type&quot;: &quot;body&quot;,
  &quot;text&quot;: &quot;&lt;BODY_TEXT&gt;&quot;
&#125;

&lt;!-- Named parameters syntax --&gt;
&#123;
  &quot;type&quot;: &quot;body&quot;,
  &quot;text&quot;: &quot;&lt;BODY_TEXT&gt;&quot;,
  &quot;example&quot;: &#123;
    &quot;body_text_named_params&quot;: [
      &#123;
        &quot;param_name&quot;: &quot;&lt;NAMED_PARAMETER_NAME&gt;&quot;,
        &quot;example&quot;: &quot;&lt;PARAMETER_EXAMPLE_VALUE&gt;&quot;
      &#125;
      &lt;!-- Additional named parameters go here, if using --&gt;
    ]
  &#125;
&#125;

&lt;!-- Positional parameters syntax --&gt;
&#123;
  &quot;type&quot;: &quot;body&quot;,
  &quot;text&quot;: &quot;&lt;BODY_TEXT&gt;&quot;,
  &quot;example&quot;: &#123;
    &quot;body_text&quot;: [
      &quot;&lt;PARAMETER_EXAMPLE_VALUE&gt;&quot;
      &lt;!-- Additional positional parameters go here, if using --&gt;
    ]
  &#125;
&#125;
```

### Creation parameters

| Placeholder | Description | Example Value |
| --- | --- | --- |
| `&lt;BODY_TEXT&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Body text string. Supports multiple [parameters](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/overview#parameter-formats).&lt;br&gt;&lt;br&gt;Maximum of 1024 characters. | `Thank you, &#123;&#123;first_name&#125;&#125;! Your order number is &#123;&#123;order_number&#125;&#125;.` |
| `&lt;NAMED_PARAMETER_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required if using a named parameter.**&lt;br&gt;&lt;br&gt;[Named parameter](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/overview#named-parameters) name. | `&#123;&#123;order_number&#125;&#125;` |
| `&lt;PARAMETER_EXAMPLE_VALUE&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required if using a parameter.**&lt;br&gt;&lt;br&gt;[Parameter](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/overview#parameter-formats) example value. | `December 1st` |

### Creation example

```json
&#123;
  &quot;type&quot;: &quot;body&quot;,
  &quot;text&quot;: &quot;Thank you, &#123;&#123;first_name&#125;&#125;! Your order number is &#123;&#123;order_number&#125;&#125;.&quot;,
  &quot;example&quot;: &#123;
    &quot;body_text_named_params&quot;: [
      &#123;
        &quot;param_name&quot;: &quot;first_name&quot;,
        &quot;example&quot;: &quot;Pablo&quot;
      &#125;,
      &#123;
        &quot;param_name&quot;: &quot;order_number&quot;,
        &quot;example&quot;: &quot;860198-230332&quot;
      &#125;
    ]
  &#125;
&#125;
```

## Footer

Footers are optional text-only components that appear immediately after the body component. Templates are limited to one footer component.

### Syntax

```html
&#123;
  &quot;type&quot;: &quot;FOOTER&quot;,
  &quot;text&quot;: &quot;&lt;TEXT&gt;&quot;
&#125;
```

### Properties

| Placeholder | Description | Example Value |
| --- | --- | --- |
| `&lt;TEXT&gt;` | Text to appear in template footer when sent.&lt;br&gt;&lt;br&gt;60 characters maximum. | `Use the buttons below to manage your marketing subscriptions` |

### Example

```json
&#123;
  &quot;type&quot;: &quot;FOOTER&quot;,
  &quot;text&quot;: &quot;Use the buttons below to manage your marketing subscriptions&quot;
&#125;
```

## Buttons

Buttons are optional interactive components that perform specific actions when tapped.

Templates can have a combination of up to 10 button components in total, although there are limits to individual buttons of the same type as well as combination limits, which are described below. In addition, templates composed of 4 or more buttons, or a quick reply button and one or more buttons of another type, cannot be viewed on WhatsApp desktop clients. WhatsApp users who receive one of these template messages will be prompted to view the message on a phone instead.

Buttons are defined within a single buttons component object, packed into a single `buttons` array. For example, this template uses a voice call button and a URL button:

```json
&#123;
  &quot;type&quot;: &quot;BUTTONS&quot;,
  &quot;buttons&quot;: [
    &#123;
      &quot;type&quot;: &quot;VOICE_CALL&quot;,
      &quot;text&quot;: &quot;Call&quot;
    &#125;,
    &#123;
      &quot;type&quot;: &quot;URL&quot;,
      &quot;text&quot;: &quot;Shop Now&quot;,
      &quot;url&quot;: &quot;https://www.luckyshrub.com/shop/&quot;
    &#125;
  ]
&#125;
```

If a template has more than three buttons, two buttons appear in the delivered message, and WhatsApp replaces the remaining buttons with a **See all options** button. Tapping the **See all options** button reveals the remaining buttons.

### Copy code buttons

Copy code buttons copy a text string (defined when the template is sent in a template message) to the device&#039;s clipboard when tapped by the app user. Templates are limited to one copy code button.

#### Syntax

```html
&#123;
  &quot;type&quot;: &quot;COPY_CODE&quot;,
  &quot;example&quot;: &quot;&lt;EXAMPLE&gt;&quot;
&#125;
```

#### Properties

| Placeholder | Description | Example Value |
| --- | --- | --- |
| `&lt;EXAMPLE&gt;` | String to be copied to the device&#039;s clipboard when tapped by the app user.&lt;br&gt;&lt;br&gt;Maximum 20 characters. | `250FF` |

#### Example

```json
&#123;
  &quot;type&quot;: &quot;COPY_CODE&quot;,
  &quot;example&quot;: &quot;250FF&quot;
&#125;
```

### Multi-product message buttons

Multi-product message buttons are special, non-customizable buttons that, when tapped, display up to 30 products from your ecommerce catalog, organized in up to 10 sections, in a single message. See [Multi-Product Message Templates](https://developers.facebook.com/documentation/business-messaging/whatsapp/catalogs/mpm-template-messages).

### One-time password buttons

One-time password buttons are a special type of [URL button](#url-buttons) component used with authentication templates. See [Authentication Templates](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/authentication-templates/authentication-templates).

### Voice call buttons

Voice call buttons make a WhatsApp call to the business when tapped by the app user. See [Create and send WhatsApp call button template message](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/call-button-messages-deep-links/#create-and-send-whatsapp-call-button-template-message) to learn more.

### Phone number buttons

Phone number buttons call the specified business phone number when tapped by the app user. Templates are limited to one phone number button.

#### Syntax

```html
&#123;
  &quot;type&quot;: &quot;PHONE_NUMBER&quot;,
  &quot;text&quot;: &quot;&lt;TEXT&gt;&quot;,
  &quot;phone_number&quot;: &quot;&lt;PHONE_NUMBER&gt;&quot;
&#125;
```

#### Properties

| Placeholder | Description | Example Value |
| --- | --- | --- |
| `&lt;PHONE_NUMBER&gt;` | Alphanumeric string. Business phone number to be called when the user taps the button.&lt;br&gt;&lt;br&gt;Note that some countries have special phone numbers that have leading zeros after the country calling code (for example, +55-0-955-585-95436). If you assign one of these numbers to the button, the leading zero will be stripped from the number. If your number will not work without the leading zero, assign an alternate number to the button, or add the number as message body text.&lt;br&gt;&lt;br&gt;20 characters maximum. | `15550051310` |
| `&lt;TEXT&gt;` | Button label text.&lt;br&gt;&lt;br&gt;25 characters maximum. | `Call` |

#### Example

```json
&#123;
  &quot;type&quot;: &quot;PHONE_NUMBER&quot;,
  &quot;text&quot;: &quot;Call&quot;,
  &quot;phone_number&quot;: &quot;15550051310&quot;
&#125;
```

### Quick reply buttons

Quick reply buttons are custom text-only buttons that immediately message you with the specified text string when tapped by the app user. A common use case is a button that allows your customer to easily opt-out of any marketing messages.

Templates are limited to 10 quick reply buttons. If using quick reply buttons with other buttons, buttons must be organized into two groups: quick reply buttons and non-quick reply buttons. If grouped incorrectly, the API will return an error indicating an invalid combination.

Examples of valid groupings:

* Quick Reply, Quick Reply
* Quick Reply, Quick Reply, URL, Phone
* URL, Phone, Quick Reply, Quick Reply

Examples of invalid groupings:

* Quick Reply, URL, Quick Reply
* URL, Quick Reply, URL

When using the API to send a template that has multiple quick reply buttons, you can use the index property to designate the order in which buttons appear in the template message.

#### Syntax

```json
&#123;
  &quot;type&quot;: &quot;QUICK_REPLY&quot;,
  &quot;text&quot;: &quot;&lt;TEXT&gt;&quot;
&#125;
```

#### Properties

| Placeholder | Description | Example Value |
| --- | --- | --- |
| `&lt;TEXT&gt;` | Button label text.&lt;br&gt;&lt;br&gt;25 characters maximum. | `Unsubscribe` |

#### Example

```html
&#123;
  &quot;type&quot;: &quot;QUICK_REPLY&quot;,
  &quot;text&quot;: &quot;Unsubscribe from Promos&quot;
&#125;
```

### SPM buttons

Single-product message (SPM) buttons are special, non-customizable buttons that can be mapped to a product in your product catalog. When tapped, they load details about the product, which the button pulls from your catalog. Users can then add the product to their cart and place an order. See [Single-Product Message Templates](https://developers.facebook.com/documentation/business-messaging/whatsapp/catalogs/spm-template-messages) and [Product Card Carousel Templates](https://developers.facebook.com/documentation/business-messaging/whatsapp/catalogs/product-card-carousel-template-messages).

### URL buttons

URL buttons load the specified URL in the device&#039;s default web browser when tapped by the app user. Templates are limited to two URL buttons.

#### Syntax

```html
&#123;
  &quot;type&quot;: &quot;URL&quot;,
  &quot;text&quot;: &quot;&lt;TEXT&gt;&quot;,
  &quot;url&quot;: &quot;&lt;URL&gt;&quot;,

  # Required if &lt;URL&gt; contains a variable
  &quot;example&quot;: [
    &quot;&lt;EXAMPLE&gt;&quot;
  ]
&#125;
```

#### Properties

| Placeholder | Description | Example Value |
| --- | --- | --- |
| `&lt;EXAMPLE&gt;` | URL of website. Supports 1 variable.&lt;br&gt;&lt;br&gt;If using a variable, add sample variable property to the end of the URL string. The URL loads in the device&#039;s default mobile web browser when the customer taps the button.&lt;br&gt;&lt;br&gt;2000 characters maximum. | `https://www.luckyshrub.com/shop?promo=summer2023` |
| `&lt;TEXT&gt;` | Button label text. 25 characters maximum. | `Shop Now` |
| `&lt;URL&gt;` | URL of website that loads in the device&#039;s default mobile web browser when the button is tapped by the app user.&lt;br&gt;&lt;br&gt;Supports 1 variable, appended to the end of the URL string.&lt;br&gt;&lt;br&gt;2000 characters maximum. | `https://www.luckyshrub.com/shop?promo=&#123;&#123;1&#125;&#125;` |

#### Example

```json
&#123;
  &quot;type&quot;: &quot;URL&quot;,
  &quot;text&quot;: &quot;Shop Now&quot;,
  &quot;url&quot;: &quot;https://www.luckyshrub.com/shop?promo=&#123;&#123;1&#125;&#125;&quot;,
  &quot;example&quot;: [
    &quot;summer2023&quot;
  ]
&#125;
```

#### URL encoding

If your URL button parameter values contain special characters, you must percent-encode them before including them in your send template message request. Unencoded special characters can cause the generated URL to fail validation, resulting in a message send error.

The following characters are common sources of encoding issues:

| Character | Encoded value | Example |
| --- | --- | --- |
| Space | `%20` | `New York` → `New%20York` |
| `:` | `%3A` | `x:key` → `x%3Akey` |
| `\|` | `%7C` | `9\|DL` → `9%7CDL` |
| `ç` | `%C3%A7` | `Gonçalves` → `Gon%C3%A7alves` |
| `ñ` | `%C3%B1` | `Peña` → `Pe%C3%B1a` |

For example, if your template URL is `https://example.com/order?name=&#123;&#123;customer_name&#125;&#125;` and the parameter value is `Gonçalves`, you must send the value as `Gon%C3%A7alves`:

```json
&#123;
  &quot;type&quot;: &quot;button&quot;,
  &quot;sub_type&quot;: &quot;url&quot;,
  &quot;index&quot;: &quot;0&quot;,
  &quot;parameters&quot;: [
    &#123;
      &quot;type&quot;: &quot;text&quot;,
      &quot;parameter_name&quot;: &quot;customer_name&quot;,
      &quot;text&quot;: &quot;Gon%C3%A7alves&quot;
    &#125;
  ]
&#125;
```

## Limited-time offer

Limited-Time Offer components are special components used to create [limited-time offer templates](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/marketing-templates/limited-time-offer-templates).

## Example requests

### Seasonal promotion

An example request to create a marketing template with the following components:

* a text header with a variable and sample value
* a text body with variables and sample values
* a text footer
* two quick-reply buttons

```curl
curl -L &#039;https://graph.facebook.com/v25.0/102290129340398/message_templates&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039; \
-H &#039;Content-Type: application/json&#039; \
-d &#039;
&#123;
  &quot;name&quot;: &quot;seasonal_promotion&quot;,
  &quot;language&quot;: &quot;en_US&quot;,
  &quot;category&quot;: &quot;MARKETING&quot;,
  &quot;components&quot;: [
    &#123;
      &quot;type&quot;: &quot;HEADER&quot;,
      &quot;format&quot;: &quot;TEXT&quot;,
      &quot;text&quot;: &quot;Our &#123;&#123;1&#125;&#125; is on!&quot;,
      &quot;example&quot;: &#123;
        &quot;header_text&quot;: [
          &quot;Summer Sale&quot;
        ]
      &#125;
    &#125;,
    &#123;
      &quot;type&quot;: &quot;BODY&quot;,
      &quot;text&quot;: &quot;Shop now through &#123;&#123;1&#125;&#125; and use code &#123;&#123;2&#125;&#125; to get &#123;&#123;3&#125;&#125; off of all merchandise.&quot;,
      &quot;example&quot;: &#123;
        &quot;body_text&quot;: [
          [
            &quot;the end of August&quot;,&quot;25OFF&quot;,&quot;25%&quot;
          ]
        ]
      &#125;
    &#125;,
    &#123;
      &quot;type&quot;: &quot;FOOTER&quot;,
      &quot;text&quot;: &quot;Use the buttons below to manage your marketing subscriptions&quot;
    &#125;,
    &#123;
      &quot;type&quot;:&quot;BUTTONS&quot;,
      &quot;buttons&quot;: [
        &#123;
          &quot;type&quot;: &quot;QUICK_REPLY&quot;,
          &quot;text&quot;: &quot;Unsubscribe from Promos&quot;
        &#125;,
        &#123;
          &quot;type&quot;:&quot;QUICK_REPLY&quot;,
          &quot;text&quot;: &quot;Unsubscribe from All&quot;
        &#125;
      ]
    &#125;
  ]
&#125;&#039;
```

### Order confirmation

An example request to create a utility template with the following components:

* a document header with a sample value
* a text body with variables and sample values
* a phone number button
* a URL button

```curl
curl -L &#039;https://graph.facebook.com/v16.0/102290129340398/message_templates&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039; \
-H &#039;Content-Type: application/json&#039; \
-d &#039;
&#123;
  &quot;name&quot;: &quot;order_confirmation&quot;,
  &quot;language&quot;: &quot;en_US&quot;,
  &quot;category&quot;: &quot;UTILITY&quot;,
  &quot;components&quot;: [
    &#123;
      &quot;type&quot;: &quot;HEADER&quot;,
      &quot;format&quot;: &quot;DOCUMENT&quot;,
      &quot;example&quot;: &#123;
        &quot;header_handle&quot;: [
          &quot;4::YX...&quot;
        ]
      &#125;
    &#125;,
    &#123;
      &quot;type&quot;: &quot;BODY&quot;,
      &quot;text&quot;: &quot;Thank you for your order, &#123;&#123;1&#125;&#125;! Your order number is &#123;&#123;2&#125;&#125;. Tap the PDF linked above to view your receipt. If you have any questions, please use the buttons below to contact support. Thank you for being a customer!&quot;,
      &quot;example&quot;: &#123;
        &quot;body_text&quot;: [
          [
            &quot;Pablo&quot;,&quot;860198-230332&quot;
          ]
        ]
      &#125;
    &#125;,
    &#123;
      &quot;type&quot;: &quot;BUTTONS&quot;,
      &quot;buttons&quot;: [
        &#123;
          &quot;type&quot;: &quot;PHONE_NUMBER&quot;,
          &quot;text&quot;: &quot;Call&quot;,
          &quot;phone_number&quot;: &quot;15550051310&quot;
        &#125;,
        &#123;
          &quot;type&quot;: &quot;URL&quot;,
          &quot;text&quot;: &quot;Contact Support&quot;,
          &quot;url&quot;: &quot;https://www.luckyshrub.com/support&quot;
        &#125;
      ]
    &#125;
  ]
&#125;&#039;
```

### Order delivery update

An example request to create a utility template with the following components:

* a location header
* a text body with variables and sample values
* a footer
* a quick reply button

```curl
curl &#039;https://graph.facebook.com/v25.0/102290129340398/message_templates&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039; \
-d &#039;
&#123;
  &quot;name&quot;: &quot;order_delivery_update&quot;,
  &quot;language&quot;: &quot;en_US&quot;,
  &quot;category&quot;: &quot;UTILITY&quot;,
  &quot;components&quot;: [
    &#123;
      &quot;type&quot;: &quot;HEADER&quot;,
      &quot;format&quot;: &quot;LOCATION&quot;
    &#125;,
    &#123;
      &quot;type&quot;: &quot;BODY&quot;,
      &quot;text&quot;: &quot;Good news &#123;&#123;1&#125;&#125;! Your order #&#123;&#123;2&#125;&#125; is on its way to the location above. Thank you for your order!&quot;,
      &quot;example&quot;: &#123;
        &quot;body_text&quot;: [
          [
            &quot;Mark&quot;,
            &quot;566701&quot;
          ]
        ]
      &#125;
    &#125;,
    &#123;
      &quot;type&quot;: &quot;FOOTER&quot;,
      &quot;text&quot;: &quot;To stop receiving delivery updates, tap the button below.&quot;
    &#125;,
    &#123;
      &quot;type&quot;:&quot;BUTTONS&quot;,
      &quot;buttons&quot;: [
        &#123;
          &quot;type&quot;: &quot;QUICK_REPLY&quot;,
          &quot;text&quot;: &quot;Stop Delivery Updates&quot;
        &#125;
      ]
    &#125;
  ]
&#125;&#039;
```

## Webhooks

Subscribe to the [message_template_components_update](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/message_template_components_update) webhook field to be notified of changes to a template&#039;s components.
