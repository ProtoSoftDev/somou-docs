# Send order details template message



## Overview

Order details message template is a template with [interactive components](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/overview#components) that extends the call-to-action button to support sending order details, which templates with only text components cannot do.

Once your Order details message templates have been created and approved, you can use the approved template to send the template message with order or bill information to prompt the WhatsApp user to make a payment.

Before sending an order details template message, businesses need to create a template with an &quot;Open order details&quot; call-to-action button. See [Create Message Templates for Your WhatsApp Business account](https://www.facebook.com/business/help/2055875911147364?id=2129163877102343) for more information on prerequisites and how to create a template.

## Creating an order details template on WhatsApp Manager

To create an order details template, business needs a business portfolio with a WhatsApp Business account.

In **WhatsApp Manager** &gt; **Account tools**:

1. Click on `create template`
2. Select `Utility` category to expand `Order details message` option
3. Enter the desired `template name` and supported `locale`
  * Depending on the number of `locales` selected there will be an equal number of template variants and businesses need to fill in the template details in respective locale.
4. Fill in template components such as `Header`, `Body`, and optional `footer` text and submit.
5. Once submitted, templates will be [categorized as per the guidelines](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/template-categorization#template-category-guidelines) and undergo the [approval process](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/template-review#approval-process) refrain from having [marketing content](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/template-categorization#marketing-templates) as part of template components.
6. The template will be approved or rejected after the template components are verified by the system.
  * If business believe the category determined is not consistent with our template category guidelines, please confirm there are no [common issues](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/template-review) that leads to rejections and if you are looking for further clarification you may [request a review](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/template-categorization#requesting-review) of the template via [Business Support](https://business.facebook.com/business-support-home/).
7. Once approved template [status](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/overview#template-status) will be changed to `ACTIVE`.
  * Note that template&#039;s status can change automatically from `ACTIVE` to `PAUSED` or `DISABLED` based on customer feedback and engagement. [Monitor status changes](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/template-review#common-rejection-reasons) and take appropriate actions whenever such change occurs.

## Creating an order details template using template creation APIs

To create a template through API and understand the general syntax, required categories and
components please refer to [templates API](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/overview#creating-templates). All the guidelines outlined above in creating templates apply through API as well.

Order details template is categorized as &quot;Utility&quot; template and apart from &#039;name&#039; and &#039;language&#039; of
choice, it has general template components such as HEADER, BODY, FOOTER, and a fixed BUTTON with &quot;ORDER_DETAILS&quot; type and &quot;Review and Pay&quot; text.

```html
POST https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;WHATSAPP_BUSINESS_BUSINESS_ID&gt;/message_templates
&#123;
  &quot;name&quot;: &quot;&lt;TEMPLATE_NAME&gt;&quot;,
  &quot;language&quot;: &quot;&lt;LANGUAGE_AND_LOCALE_CODE&gt;&quot;,
  &quot;category&quot;: &quot;UTILITY&quot;,
  /* Businesses can create the order details template under marketing category by including display_format attribute */
  /* &quot;display_format&quot;: &quot;order_details&quot;,*/
  &quot;components&quot;: [
    &#123;
      &quot;type&quot;: &quot;HEADER&quot;,
      &quot;format&quot;: &quot;TEXT&quot;,
      &quot;text&quot;: &quot;&lt;TEMPLATE_HEADER_TEXT&gt;&quot;
    &#125;,
    &#123;
      &quot;type&quot;: &quot;BODY&quot;,
      &quot;text&quot;: &quot;&lt;TEMPLATE_BODY_TEXT&gt;&quot;
    &#125;,
    &#123;
      &quot;type&quot;: &quot;FOOTER&quot;,
      &quot;text&quot;: &quot;&lt;TEMPLATE_FOOTER_TEXT&gt;&quot;
    &#125;,
    &#123;
      &quot;type&quot;: &quot;BUTTONS&quot;,
      &quot;buttons&quot;: [
        &#123;
          &quot;type&quot;: &quot;ORDER_DETAILS&quot;,
          &quot;text&quot;: &quot;Review and Pay&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

## Sending order details template message

Order details template message allows the businesses to send invoice(order_details) message as predefined `Open order details` call-to-action button component parameters. It supports businesses to send all payment integration (such as [UPI Intent](https://developers.facebook.com/documentation/business-messaging/whatsapp/payments/payments-in/upi-intent#step-2--assemble-the-interactive-object), [Payment Gateway](https://developers.facebook.com/documentation/business-messaging/whatsapp/payments/payments-in/pg#step-1) or [Payment Links](https://developers.facebook.com/documentation/business-messaging/whatsapp/payments/payments-in/payment-links#step-2--assemble-the-interactive-object)) integration as button parameters.

To send an order details template message, make a `POST` call to `/PHONE_NUMBER_ID/messages` endpoint and attach a message object with `type=template`. Then, add a template object with a predefined `Open order details` call-to-action button.

For example, the following sample describes how to send [UPI Intent](https://developers.facebook.com/documentation/business-messaging/whatsapp/payments/payments-in/upi-intent#step-2--assemble-the-interactive-object) in order details template message parameters to prompt the consumer to make a payment.

The below example shows an example payload with `upi` as the payment type. For other variants of the `payment_settings` block (for example, for the type `payment_gateway` type), refer to this [document](https://developers.facebook.com/documentation/business-messaging/whatsapp/payments/payments-in/pg#step-1).

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
        &quot;type&quot;: &quot;header&quot;,
        &quot;parameters&quot;: [
          &#123;
            &quot;type&quot;: &quot;image&quot;, // Uses header with image as an example
            &quot;image&quot;: &#123;
              &quot;link&quot;: &quot;http(s)://the-url&quot;
            &#125;
          &#125;
        ]
      &#125;,
      &#123;
        &quot;type&quot;: &quot;button&quot;,
        &quot;sub_type&quot;: &quot;order_details&quot;,
        &quot;index&quot;: 0,
        &quot;parameters&quot;: [
          &#123;
            &quot;type&quot;: &quot;action&quot;,
            &quot;action&quot;: &#123;
              &quot;order_details&quot;: &#123;
                &quot;currency&quot;: &quot;INR&quot;,
                &quot;order&quot;: &#123;
                  &quot;discount&quot;: &#123;
                    &quot;offset&quot;: 100,
                    &quot;value&quot;: 250
                  &#125;,
                  &quot;items&quot;: [
                    &#123;
                      &quot;amount&quot;: &#123;
                        &quot;offset&quot;: 100,
                        &quot;value&quot;: 400
                      &#125;,
                      &quot;name&quot;: &quot;&lt;ORDER_ITEM_NAME&gt;&quot;,
                      &quot;quantity&quot;: 1,
                      &quot;retailer_id&quot;: &quot;&lt;ORDER_ITEM_RETAILER_ID&gt;&quot;,
                      &quot;country_of_origin&quot;: &quot;&lt;ORIGIN_COUNTRY&gt;&quot;,
                      &quot;importer_name&quot;: &quot;&lt;IMPORTER_NAME&gt;&quot;,
                      &quot;importer_address&quot;: &#123;
                        &quot;address_line1&quot;: &quot;&lt;IMPORTER_ADDRESS&gt;&quot;,
                        &quot;city&quot;: &quot;&lt;CITY&gt;&quot;,
                        &quot;country_code&quot;: &quot;&lt;COUNTRY&gt;&quot;,
                        &quot;postal_code&quot;: &quot;&lt;ZIP_CODE&gt;&quot;
                      &#125;
                    &#125;
                  ],
                  &quot;shipping&quot;: &#123;
                    &quot;offset&quot;: 100,
                    &quot;value&quot;: 0
                  &#125;,
                  &quot;status&quot;: &quot;pending&quot;,
                  &quot;subtotal&quot;: &#123;
                    &quot;offset&quot;: 100,
                    &quot;value&quot;: 400
                  &#125;,
                  &quot;tax&quot;: &#123;
                    &quot;offset&quot;: 100,
                    &quot;value&quot;: 500
                  &#125;
                &#125;,
                &quot;payment_settings&quot;: [
                  &#123;
                    &quot;type&quot;: &quot;upi_intent_link&quot;,
                    &quot;upi_intent_link&quot;: &#123;
                      &quot;link&quot;: &quot;upi://pay?pa=merchant_vpa&amp;pn=merchant%20Name&amp;mc=mc_code&amp;purpose=purpose_code&amp;tr=transaction_record&quot;
                    &#125;
                  &#125;
                ],
                &quot;payment_configuration&quot;: &quot;unique_payment_config_id&quot;,
                &quot;payment_type&quot;: &quot;upi&quot;,
                &quot;reference_id&quot;: &quot;reference_id_value&quot;,
                &quot;total_amount&quot;: &#123;
                  &quot;offset&quot;: 100,
                  &quot;value&quot;: 650
                &#125;,
                &quot;type&quot;: &quot;digital-goods&quot;
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

After the order details template message delivery the rest of the payment flow is the same as &quot;Sending invoice in customer session window&quot; and depends on the chosen [payment integration](https://developers.facebook.com/documentation/business-messaging/whatsapp/payments/payments-in/overview) order details parameters. For more details refer [UPI Intent](https://developers.facebook.com/documentation/business-messaging/whatsapp/payments/payments-in/upi-intent#step-5--consumer-pays-for-the-order), [Payment Gateway](https://developers.facebook.com/documentation/business-messaging/whatsapp/payments/payments-in/pg#step-2--receive-webhook-about-transaction-status) and [Payment Links](https://developers.facebook.com/documentation/business-messaging/whatsapp/payments/payments-in/payment-links#step-5--consumer-pays-for-the-order) post payments flows.
