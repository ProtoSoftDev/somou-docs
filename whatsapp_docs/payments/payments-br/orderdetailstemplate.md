# Send order details template message



## Overview

Order details message template is a template with [interactive components](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/overview#components) that extends the call-to-action button to support sending order details, letting you display itemized orders, totals, and payment options that templates with only text components cannot.

Once your Order details message templates have been created and approved, you can use the approved template to send the template message with order or bill information to prompt customers to make a payment.

Before sending an order details template message, businesses need to create a template with an &quot;order details&quot; call-to-action button. See [Create Message Templates for Your WhatsApp Business account](https://www.facebook.com/business/help/2055875911147364?id=2129163877102343) for more information on prerequisites and how to create a template.

## Creating an order details template on WhatsApp Manager

To create an order details template, business needs a business portfolio with a WhatsApp Business account.

In **WhatsApp Manager** &gt; **Account tools**:

1. Click on `create template`
2. Select `Utility` or `Marketing` category to see the `Order details` template format option.
3. Select `Order details` template format, and click **Next**
4. Enter the desired `template name` and supported `locale`
  * Depending on the number of `locales` selected there will be an equal number of template variants and businesses need to fill in the template details in respective locale.
5. Fill in template components such as `Header`, `Body`, and optional `footer` text.
  * For the `Header`, you can choose one of three media types: `Text`, `Image` or `Document`. Choose `Document` if you want to send **PDF files** in the header of this template.
6. Click submit.
7. Once submitted, templates will be [categorized as per the guidelines](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/template-categorization#template-category-guidelines) and undergo the [approval process](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/template-review#approval-process).
8. The template will be approved or rejected after the template components are verified by the system.
  * If business believe the category determined is not consistent with our template category guidelines, please confirm there are no [common issues](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/template-review) that leads to rejections and if you are looking for further clarification you may [request a review](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/template-categorization#requesting-review) of the template via [Business Support](https://business.facebook.com/business-support-home/)
9. Once approved template [status](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/overview#template-status) will be changed to `ACTIVE`
  * A template&#039;s status can change automatically from `ACTIVE` to `PAUSED` or `DISABLED` based on customer feedback and engagement. [Monitor status changes](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/template-review#common-rejection-reasons) and take appropriate action whenever such a change occurs.

## Creating an order details template using template creation APIs

To create a template through API and understand the general syntax, required categories and
components please refer to [templates API](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/overview#creating-templates). All the guidelines outlined above in creating templates apply through API as well.

Order details template can be categorized as &quot;Utility&quot; or &quot;Marketing&quot; template and apart from &#039;name&#039; and &#039;language&#039; of
choice, the template has general template components such as HEADER, BODY, FOOTER, and a fixed BUTTON with &quot;ORDER_DETAILS&quot; type.

The `category` must be `UTILITY` or `MARKETING`, and the header `format` must be `TEXT`, `IMAGE`, or `DOCUMENT`.

### Endpoint

```bash
POST /&#123;WHATSAPP_BUSINESS_ACCOUNT_ID&#125;/message_templates
```

### Request body

```json
&#123;
  &quot;name&quot;: &quot;&lt;TEMPLATE_NAME&gt;&quot;,
  &quot;language&quot;: &quot;&lt;LANGUAGE_CODE&gt;&quot;,
  &quot;category&quot;: &quot;&lt;CATEGORY&gt;&quot;,
  &quot;display_format&quot;: &quot;ORDER_DETAILS&quot;,
  &quot;components&quot;: [
    &#123;
      &quot;type&quot;: &quot;HEADER&quot;,
      &quot;format&quot;: &quot;&lt;FORMAT&gt;&quot;,
      &quot;text&quot;: &quot;&lt;HEADER_TEXT&gt;&quot;
    &#125;,
    &#123;
      &quot;type&quot;: &quot;BODY&quot;,
      &quot;text&quot;: &quot;&lt;BODY_TEXT&gt;&quot;
    &#125;,
    &#123;
      &quot;type&quot;: &quot;FOOTER&quot;,
      &quot;text&quot;: &quot;&lt;FOOTER_TEXT&gt;&quot;
    &#125;,
    &#123;
      &quot;type&quot;: &quot;BUTTONS&quot;,
      &quot;buttons&quot;: [
        &#123;
          &quot;type&quot;: &quot;ORDER_DETAILS&quot;,
          &quot;text&quot;: &quot;&lt;COPY_PIX_CODE&gt;&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

## Sending order details template message

Order details template message allows the businesses to send invoice (order_details) message as predefined `order details` call-to-action button component parameters. The order details template message supports businesses to send all payment integration (such as [Dynamic Pix code](https://developers.facebook.com/documentation/business-messaging/whatsapp/payments/payments-br/offsite-pix), [Payment Links](https://developers.facebook.com/documentation/business-messaging/whatsapp/payments/payments-br/payment-links), [Boleto](https://developers.facebook.com/documentation/business-messaging/whatsapp/payments/payments-br/boleto), and so on) integration as button parameters.

To send an order details template message, make a `POST` call to `/PHONE_NUMBER_ID/messages` endpoint and attach a message object with `type=template`. Then, add a template object with a predefined `order details` call-to-action button.

The following image shows an example of an order details template message rendered in WhatsApp, with itemized products, payment options, and call-to-action buttons.

You can optionally include a **PDF file** as attachment in the `header` component of the template message. To do so, you use `type=document` in the `parameter` object of the `header` component object, as described in our [Components](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/components#media-header) document.

For example, the following sample describes how to send [Copy Pix code](https://developers.facebook.com/documentation/business-messaging/whatsapp/payments/payments-br/offsite-pix) in order details template message parameters to prompt the consumer to make a payment.

### Endpoint

```bash
POST /&#123;PHONE_NUMBER_ID&#125;/messages
```

### Request body

```json
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;recipient_type&quot;: &quot;individual&quot;,
  &quot;to&quot;: &quot;&lt;PHONE_NUMBER&gt;&quot;,
  &quot;type&quot;: &quot;template&quot;,
  &quot;template&quot;: &#123;
    &quot;name&quot;: &quot;&lt;TEMPLATE_NAME&gt;&quot;,
    &quot;language&quot;: &#123;
      &quot;policy&quot;: &quot;deterministic&quot;,
      &quot;code&quot;: &quot;&lt;LANGUAGE_AND_LOCALE_CODE&gt;&quot;
    &#125;,
    &quot;components&quot;: [
      &#123;
        &quot;type&quot;: &quot;button&quot;,
        &quot;sub_type&quot;: &quot;order_details&quot;,
        &quot;index&quot;: 0,
        &quot;parameters&quot;: [
          &#123;
            &quot;type&quot;: &quot;action&quot;,
            &quot;action&quot;: &#123;
              &quot;order_details&quot;: &#123;
                &quot;reference_id&quot;: &quot;&lt;UNIQUE_REFERENCE_ID&gt;&quot;,
                &quot;type&quot;: &quot;digital-goods&quot;,
                &quot;payment_type&quot;: &quot;br&quot;,
                &quot;payment_settings&quot;: [
                  &#123;
                    &quot;type&quot;: &quot;pix_dynamic_code&quot;,
                    &quot;pix_dynamic_code&quot;: &#123;
                      &quot;code&quot;: &quot;00020101021226700014br.gov.bcb.pix2548pix.example.com...&quot;,
                      &quot;merchant_name&quot;: &quot;Account holder name&quot;,
                      &quot;key&quot;: &quot;39580525000189&quot;,
                      &quot;key_type&quot;: &quot;CNPJ&quot;
                    &#125;
                  &#125;
                ],
                &quot;currency&quot;: &quot;BRL&quot;,
                &quot;total_amount&quot;: &#123;
                  &quot;value&quot;: 50000,
                  &quot;offset&quot;: 100
                &#125;,
                &quot;order&quot;: &#123;
                  &quot;status&quot;: &quot;pending&quot;,
                  &quot;tax&quot;: &#123;
                    &quot;value&quot;: 0,
                    &quot;offset&quot;: 100,
                    &quot;description&quot;: &quot;optional text&quot;
                  &#125;,
                  &quot;items&quot;: [
                    &#123;
                      &quot;retailer_id&quot;: &quot;1234567&quot;,
                      &quot;name&quot;: &quot;Cake&quot;,
                      &quot;amount&quot;: &#123;
                        &quot;value&quot;: 50000,
                        &quot;offset&quot;: 100
                      &#125;,
                      &quot;quantity&quot;: 1
                    &#125;
                  ],
                  &quot;subtotal&quot;: &#123;
                    &quot;value&quot;: 50000,
                    &quot;offset&quot;: 100
                  &#125;
                &#125;
              &#125;
            &#125;
          &#125;
        ]
      &#125;
    ]
  &#125;
&#125;
```

Once the order details template message is delivered, a successful response will include an object with an identifier prefixed with wamid. Use the ID listed after wamid to track your message status.

```json
&#123;
    &quot;messaging_product&quot;: &quot;whatsapp&quot;,
    &quot;contacts&quot;: [
        &#123;
            &quot;input&quot;: &quot;&lt;PHONE_NUMBER&gt;&quot;,
            &quot;wa_id&quot;: &quot;&lt;WHATSAPP_ID&gt;&quot;
        &#125;
    ],
    &quot;messages&quot;: [
        &#123;
            &quot;id&quot;: &quot;wamid.ID&quot;
        &#125;
    ]
&#125;
```

## Post order details template message flow

After the order details template message delivery the rest of the payment flow is the same as &quot;Sending invoice in customer session window&quot; and depends on the chosen [payment integration](https://developers.facebook.com/documentation/business-messaging/whatsapp/payments/payments-br/overview) order details parameters.
