# Template Library



Template Library makes it faster and easier for businesses to create utility templates for common use cases, like payment reminders, delivery updates — and authentication templates for common identity verification use cases.

These pre-written templates have already been categorized as utility or authentication. Library templates contain fixed content that cannot be edited and parameters you can adapt for business or user-specific information.

You can browse and create templates using Template Library in WhatsApp Manager, or programmatically via the API.

## Creating templates via WhatsApp Manager (WAM)

Follow the instructions below to create templates using the Template Library in [WhatsApp Manager](https://business.facebook.com/wa/manage/template-library).

1. In the sidebar of WAM, under **Message Templates**, select **Create Template**.

2. Under *Browse the WhatsApp Template Library*, select **Browse Templates**.

3. You will now see all currently available templates. Use the search bar to search by topic or use case, or use the dropdown options on the sidebar to filter the results.

Hovering over a template will show you its parameter values.

4. To create a template, **select one** by clicking on it. Then, add your template name, select the language, and fill out the button details. Once you have completed these steps, click **Submit**.

Note: If you choose **Customize template**, your template will have to go through review before you are able to send messages.

## Template parameters and restrictions

**Warning:** When a template contains the value `library_template_name` in the `GET &lt;WABAID&gt;/message_templates?name=&lt;TEMPLATE_NAME&gt;` response, it is a template created from the Template Library and is subject to type checks and restrictions.

Templates in the library contain both fixed content and parameters. The parameters represent spaces in the template where variable information can be inserted, such as names, addresses, and phone numbers.

In the example above, parameters like the name `Jim` or the business name `CS Mutual` can be modified to accept variables like your customer&#039;s name and your business&#039;s name.

Messages sent using templates from Template Library are subject to parameter checks during send time. Values used in parameters that are outside of the established ranges listed below will cause the message send to fail.

### List of parameters and sample values

**Warning:** All parameters are length restricted. If you receive an error, try again with a shorter value.

| Parameter Type | Description | Sample Value |
| --- | --- | --- |
| `ADDRESS` | A location address.&lt;br&gt;&lt;br&gt;* Must be a valid address | * `1 Hacker Way, Menlo Park, CA 94025` |
| `TEXT` | Basic text. | * `regarding your order.`&lt;br&gt;* `12 pack of paper towels`&lt;br&gt;* `your request`&lt;br&gt;* `purchase`&lt;br&gt;* `Jasper&#039;s Market` |
| `AMOUNT` | A number signifying a quantity.&lt;br&gt;&lt;br&gt;* May contain a prefix or suffix for monetary values such as USD or RS&lt;br&gt;* May contain decimals (.) and commas (,)&lt;br&gt;* May contain valid currency symbols such as $ and € | * `145`&lt;br&gt;* `USD $375.32`&lt;br&gt;* `€1,376.22 EUR`&lt;br&gt;* `RS 1200` |
| `DATE` | A standard calendar date. | * `2021-04-19`&lt;br&gt;* `13/03/2021`&lt;br&gt;* `5th January 1982`&lt;br&gt;* `08.22.1991`&lt;br&gt;* `January 1st, 2024`&lt;br&gt;* `05 12 2022` |
| `PHONE NUMBER` | A telephone number.&lt;br&gt;&lt;br&gt;* May contain numbers, spaces, dashes (-), parentheses, and plus symbols (+) | * `+1 4256789900`&lt;br&gt;* `+91-7884-789122`&lt;br&gt;* `+39 87 62232` |
| `EMAIL` | A standard email address.&lt;br&gt;&lt;br&gt;* Must be a valid email address | * `1hackerway&#064;meta.com`&lt;br&gt;* `yourcustomername&#064;gmail.com`&lt;br&gt;* `abusinessorcustomername&#064;hotmail.com` |
| `NUMBER` | A number.&lt;br&gt;&lt;br&gt;* Must be a number.&lt;br&gt;* Cannot contain spaces. | * `23444`&lt;br&gt;* `90001234921388904`&lt;br&gt;* `453638` |

## Forms

**Warning:** Forms are only available to accounts who have had their message limits increased.

Some templates in Template Library are interactive forms that are powered by WhatsApp Flows.

In WhatsApp Manager, you can identify these specific templates by the &quot;Form&quot; label they contain. The current supported use cases are Customer Feedback and Delivery Failure.

### Identifying forms in the request response

When calling the `GET /message_template_library` endpoint, the `type` key in the `buttons` array will show as `&quot;FORMS&quot;`.

```json
&#123;
      &quot;name&quot;: &quot;delivery_failed_2_form&quot;,
      &quot;language&quot;: &quot;en_US&quot;,
      &quot;category&quot;: &quot;UTILITY&quot;,
      &quot;topic&quot;: &quot;ORDER_MANAGEMENT&quot;,
      &quot;usecase&quot;: &quot;DELIVERY_FAILED&quot;,
      &quot;industry&quot;: [
        &quot;E_COMMERCE&quot;
      ],
      &quot;body&quot;: &quot;We were unable to deliver order &#123;&#123;1&#125;&#125; today.

Please &#123;&#123;2&#125;&#125; to schedule another delivery attempt.&quot;,
      &quot;body_params&quot;: [
        &quot;#12345&quot;,
        &quot;try a redelivery&quot;
      ],
      &quot;body_param_types&quot;: [
        &quot;TEXT&quot;,
        &quot;TEXT&quot;
      ],
      &quot;buttons&quot;: [
        &#123;
          &quot;type&quot;: &quot;FLOW&quot;,
          &quot;text&quot;: &quot;Reschedule&quot;
        &#125;
      ],
      &quot;id&quot;: &quot;7138055039625658&quot;
&#125;,
```

## Using the API

The Template Library API has two endpoints:

```https
// Used to browse available library templates
GET /message_template_library
```

```https
// Used when you are ready to create a template from the library.
POST /&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;/message_templates
```

### Searching and filtering available templates

**Warning:** Templates with `Header` parameter types of `Document` only support PDFs.

To browse and filter available templates, use the `message_template_library` endpoint.

Once you find the template you are interested in, note the name as you will use it when creating the template via the `POST` method.

### Request syntax

```https
// Get all available templates
GET /message_template_library

// Search for substring
GET /message_template_library?search=&lt;SEARCH_KEY&gt;

// Filter by template topic
GET/message_template_library?topic=&lt;TOPIC&gt;

// Filter by template use case
GET/message_template_library?usecase=&lt;USECASE&gt;

// Filter by template industry
GET/message_template_library?industry=&lt;INDUSTRY&gt;

// Filter by template language
GET/message_template_library?language=&lt;LANGUAGE&gt;

// Search by template name
GET /message_template_library?name=&lt;NAME&gt;
```

### Query string parameters

| Placeholder | Description | Sample Value |
| --- | --- | --- |
| `&lt;SEARCH_KEY&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;A substring you are searching for in the content, name, header, body, or footer of the template. | `payments` |
| `&lt;TOPIC&gt;`&lt;br&gt;&lt;br&gt;_Enum_ | **Optional.**&lt;br&gt;&lt;br&gt;The topic of the template.&lt;br&gt;&lt;br&gt;See Template Filters below | `ORDER_MANAGEMENT` |
| `&lt;USECASE&gt;`&lt;br&gt;&lt;br&gt;_Enum_ | **Optional.**&lt;br&gt;&lt;br&gt;The use case of the template.&lt;br&gt;&lt;br&gt;See Template Filters below | `SHIPMENT_CONFIRMATION` |
| `&lt;INDUSTRY&gt;`&lt;br&gt;&lt;br&gt;_Enum_ | **Optional.**&lt;br&gt;&lt;br&gt;The industry of the template.&lt;br&gt;&lt;br&gt;See Template Filters below | `E_COMMERCE` |
| `&lt;LANGUAGE&gt;`&lt;br&gt;&lt;br&gt;_Enum_ | **Optional.**&lt;br&gt;&lt;br&gt;The template language locale code.&lt;br&gt;&lt;br&gt;See [Supported Languages](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/supported-languages) | `en_US` |
| `&lt;NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;The name of the template you are searching for in the template library. | `verify_otp_usecase` |

### Example request

```curl
curl &#039;https://graph.facebook.com/v25.0/102290129340398/message_templates?search=&quot;payments&quot;&#039;
-H &#039;Authorization: Bearer EAAJB...&#039;
```

### Example response

```json
&#123;
      &quot;name&quot;: &quot;low_balance_warning_1&quot;,
      &quot;language&quot;: &quot;en_US&quot;,
      &quot;category&quot;: &quot;UTILITY&quot;,
      &quot;topic&quot;: &quot;PAYMENTS&quot;,
      &quot;usecase&quot;: &quot;LOW_BALANCE_WARNING&quot;,
      &quot;industry&quot;: [
        &quot;FINANCIAL_SERVICES&quot;
      ],
      &quot;header&quot;: &quot;Your account balance is low&quot;,
      &quot;body&quot;: &quot;Hi &#123;&#123;1&#125;&#125;,
This is to notify you that your &#123;&#123;2&#125;&#125; in your &#123;&#123;3&#125;&#125; account, ending in &#123;&#123;4&#125;&#125; is below your pre-set &#123;&#123;5&#125;&#125; of &#123;&#123;6&#125;&#125;.
Click the button to deposit more &#123;&#123;7&#125;&#125;.
&#123;&#123;8&#125;&#125;&quot;,
      &quot;body_params&quot;: [
        &quot;Jim&quot;,
        &quot;available funds&quot;,
        &quot;CS Mutual checking plus&quot;,
        &quot;1234&quot;,
        &quot;limit&quot;,
        &quot;$75.00&quot;,
        &quot;funds&quot;,
        &quot;CS Mutual&quot;
      ],
      &quot;buttons&quot;: [
        &#123;
          &quot;type&quot;: &quot;URL&quot;,
          &quot;text&quot;: &quot;Make a deposit&quot;,
          &quot;url&quot;: &quot;https://www.example.com/&quot;
        &#125;,
        &#123;
          &quot;type&quot;: &quot;PHONE_NUMBER&quot;,
          &quot;text&quot;: &quot;Call us&quot;,
          &quot;phone_number&quot;: &quot;+18005551234&quot;
        &#125;
      ],
      &quot;id&quot;: &quot;7147013345418927&quot;
&#125;
```

### Template filters

There are several templates to choose from in the Template Library. You can use the API to filter them based on a few factors.

**Industry**

- `E_COMMERCE`
- `FINANCIAL_SERVICES`

**Topic**

- `ACCOUNT_UPDATE`
- `CUSTOMER_FEEDBACK`
- `ORDER_MANAGEMENT`
- `PAYMENTS`

**Use case**

- `ACCOUNT_CREATION_CONFIRMATION`
- `AUTO_PAY_REMINDER`
- `DELIVERY_CONFIRMATION`
- `DELIVERY_FAILED`
- `DELIVERY_UPDATE`
- `FEEDBACK_SURVEY`
- `FRAUD_ALERT`
- `LOW_BALANCE_WARNING`
- `ORDER_ACTION_NEEDED`
- `ORDER_CONFIRMATION`
- `ORDER_DELAY`
- `ORDER_OR_TRANSACTION_CANCEL`
- `ORDER_PICK_UP`
- `PAYMENT_ACTION_REQUIRED`
- `PAYMENT_CONFIRMATION`
- `PAYMENT_DUE_REMINDER`
- `PAYMENT_OVERDUE`
- `PAYMENT_REJECT_FAIL`
- `PAYMENT_SCHEDULED`
- `RECEIPT_ATTACHMENT`
- `RETURN_CONFIRMATION`
- `SHIPMENT_CONFIRMATION`
- `STATEMENT_ATTACHMENT`
- `STATEMENT_AVAILABLE`
- `TRANSACTION_ALERT`

## Creating templates

**Warning:** **Note: The modification of rules surrounding body properties for this endpoint is for the explicit purpose of showcasing how to use the endpoint with Template Library.**

To create a new template using the Template Library, call the existing `&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;/message_templates` endpoint using the body properties below.

### Request syntax

```https
POST /&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;/message_templates
```

### Post body

```json
&#123;
  &quot;name&quot;: &quot;&lt;NAME&gt;&quot;,
  &quot;category&quot;: &quot;UTILITY&quot;,
  &quot;language&quot;: &quot;en_US&quot;,
  &quot;library_template_name&quot;: &quot;&lt;LIBRARY_TEMPLATE_NAME&gt;&quot;,
  &quot;library_template_button_inputs&quot;: &quot;[
    &#123;&#039;type&#039;: &#039;URL&#039;, &#039;url&#039;: &#123;&#039;base_url&#039; : &#039;https://www.example.com/&#123;&#123;1&#125;&#125;&#039;,
    &#039;url_suffix_example&#039; : &#039;https://www.example.com/demo&#039;&#125;&#125;,
    &#123;type: &#039;PHONE_NUMBER&#039;, &#039;phone_number&#039;: &#039;+16315551010&#039;&#125;
]&quot;
&#125;
```

### Body properties

| Placeholder | Description | Sample Value |
| --- | --- | --- |
| `&lt;NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;The name you are providing for your template.&lt;br&gt;&lt;br&gt;Maximum 512 characters. | `my_payment_template` |
| `&lt;CATEGORY&gt;`&lt;br&gt;&lt;br&gt;_Enum_ | **Required.**&lt;br&gt;&lt;br&gt;The template category.&lt;br&gt;&lt;br&gt;**Must be `UTILITY` for use with Template Library.** | `UTILITY` |
| `&lt;LANGUAGE&gt;`&lt;br&gt;&lt;br&gt;_Enum_ | **Required.**&lt;br&gt;&lt;br&gt;The template language locale code.&lt;br&gt;&lt;br&gt;See [Supported Languages](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/supported-languages) | `en_US` |
| `&lt;LIBRARY_TEMPLATE_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;The exact name of the Template Library template. | `delivery_update_1` |
| `&lt;LIBRARY_TEMPLATE_BUTTON_INPUTS&gt;`&lt;br&gt;&lt;br&gt;_Array of objects_ | **Optional.**&lt;br&gt;&lt;br&gt;The website and/or phone number of the business being used in the template.&lt;br&gt;&lt;br&gt;**Note: For utility templates that have button inputs, this property is _not_ optional.** | `&quot;[&lt;br&gt;&#123;&#039;type&#039;: &#039;URL&#039;, &#039;url&#039;: &#123;&#039;base_url&#039; : &#039;https://www.example.com/&#123;&#123;1&#125;&#125;&#039;,&lt;br&gt;&#039;url_suffix_example&#039; : &#039;https://www.example.com/demo&#039;&#125;&#125;,&lt;br&gt;&#123;type: &#039;PHONE_NUMBER&#039;, &#039;phone_number&#039;: &#039;+16315551010&#039;&#125;&lt;br&gt;]&quot;&lt;br&gt;` |

### Library template button inputs

| Placeholder | Description | Sample Value |
| --- | --- | --- |
| `type`&lt;br&gt;&lt;br&gt;_enum_ | The button type&lt;br&gt;&lt;br&gt;`QUICK_REPLY`, `URL`, `PHONE_NUMBER`, `OTP`, `MPM`, `CATALOG`, `FLOW`, `VOICE_CALL`, `APP`&lt;br&gt;&lt;br&gt;*Required* | `OTP` |
| `phone_number`&lt;br&gt;&lt;br&gt;_String_ | Phone number for the button.&lt;br&gt;&lt;br&gt;*Optional* | `&quot;+13057652345&quot;` |
| `url`&lt;br&gt;&lt;br&gt;_JSON Object_ | [View JSON object URL parameters `base_url` and `url_suffix_example` here](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/message-template-api#post-version-waba-id-message-templates)&lt;br&gt;&lt;br&gt;*Optional* |  |
| `zero_tap_terms_accepted`&lt;br&gt;&lt;br&gt;_boolean_ | Whether the zero tap terms were accepted by the user or not.&lt;br&gt;&lt;br&gt;*Optional* | `TRUE` |
| `otp_type`&lt;br&gt;&lt;br&gt;_enum_ | The OTP type.&lt;br&gt;&lt;br&gt;`COPY_CODE`, `ONE_TAP`, `ZERO_TAP`&lt;br&gt;&lt;br&gt;*Optional* | `TRUE` |
| `supported_apps`&lt;br&gt;&lt;br&gt;_Array of JSON Object_ | [View JSON object Supported App parameters `package_name` and `signature_hash` here](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/message-template-api#post-version-waba-id-message-templates)&lt;br&gt;&lt;br&gt;*Optional* |  |

### Library template body inputs

| Placeholder | Description | Sample Value |
| --- | --- | --- |
| `&lt;LIBRARY_TEMPLATE_BODY_INPUTS&gt;`&lt;br&gt;&lt;br&gt;_JSON Object_ | **Optional.**&lt;br&gt;&lt;br&gt;Optional data during creation of a template from Template Library. These are optional fields for the button component.&lt;br&gt;&lt;br&gt;[_Learn how to create templates using Template Library_](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/template-library) |  |
| `add_contact_number`&lt;br&gt;&lt;br&gt;_boolean_ | Boolean value to add information to the template about contacting business on their phone number.&lt;br&gt;&lt;br&gt;*Optional* | `TRUE` |
| `add_learn_more_link`&lt;br&gt;&lt;br&gt;_boolean_ | Boolean value to add information to the template about learning more information with a url link.&lt;br&gt;&lt;br&gt;Not widely available and will be ignored if not available.&lt;br&gt;&lt;br&gt;*Optional* | `TRUE` |
| `add_security_recommendation`&lt;br&gt;&lt;br&gt;_boolean_ | Boolean value to add information to the template about not sharing authentication codes with anyone.&lt;br&gt;&lt;br&gt;*Optional* | `TRUE` |
| `add_track_package_link`&lt;br&gt;&lt;br&gt;_boolean_ | Boolean value to add information to the template to track delivery packages.&lt;br&gt;&lt;br&gt;Not widely available and will be ignored if not available.&lt;br&gt;&lt;br&gt;*Optional* | `TRUE` |
| `code_expiration_minutes`&lt;br&gt;&lt;br&gt;_int64_ | Integer value to add information to the template on when the code will expire.&lt;br&gt;&lt;br&gt;*Optional* | `5` |

### Example request

```curl
curl &#039;https://graph.facebook.com/v19.0/102290129340398/message_templates&#039;
-H &#039;Authorization: Bearer EAAJB...&#039;
-H &#039;Content-Type: application/json&#039;
-d &#039;
&#123;
  &quot;name&quot;: &quot;my_delivery_update&quot;,
  &quot;language&quot;: &quot;en_US&quot;,
  &quot;category&quot;: &quot;UTILITY&quot;,
  &quot;library_template_name&quot;: &quot;delivery_update_1&quot;,
  &quot;library_template_button_inputs&quot;: &quot;[
    &#123;&#039;type&#039;: &#039;URL&#039;, &#039;url&#039;: &#123;&#039;base_url&#039; : &#039;https://www.example.com/&#123;&#123;1&#125;&#125;&#039;,
    &#039;url_suffix_example&#039; : &#039;https://www.example.com/order_update&#125;&#125;
  ]&quot;
&#125;
```

### Example response

```curl
&#123;
  &quot;id&quot;: &quot;&#123;hsm-id&#125;&quot;,
  &quot;status&quot;: &quot;APPROVED&quot;,
  &quot;category&quot;: &quot;UTILITY&quot;
&#125;
```

## Sending template messages

To learn how to send templated messages, view the [Template fundamentals](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/overview)
