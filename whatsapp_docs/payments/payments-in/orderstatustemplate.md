# Send order status template message



## Overview

An Order status template is a template with interactive [components](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/components) that extends the call-to-action button to support updating order status through a template. It allows businesses to update the order status outside of the [customer session window](https://developers.facebook.com/documentation/business-messaging/whatsapp/messages/send-messages#customer-service-windows) in use cases such as charging the card for past order and updating about a shipment on order placed in the past.

Upon receiving the payment signals, the businesses must update the order status to keep the user up to date. The WhatsApp Business Platform currently supports the following order status values.

| Value | Description |
| --- | --- |
| `pending` | User has not successfully paid yet |
| `processing` | User payment authorized, merchant/partner is fulfilling the order, performing the service, and so on |
| `partially-shipped` | A portion of the products in the order have been shipped by the merchant |
| `shipped` | All the products in the order have been shipped by the merchant |
| `completed` | The order is completed and no further action is expected from the user or the partner/merchant |
| `canceled` | The partner/merchant would like to cancel the `order_details` message for the order/invoice. The status update will fail if there is already a `successful` or `pending` payment for this `order_details` message |

## Creating an order status template

To create an order status template, the business needs a business portfolio with a WhatsApp Business account, and access to the WhatsApp Manager.

In **WhatsApp Manager** &gt; **Account tools**:

1. Click on `create template`.
2. Select `Utility` category to expand `Order details message` option.
3. Enter the desired `template name` and supported `locale`.
  * Depending on the number of `locales` selected there will be an equal number of template variants and businesses need to fill in the template details in respective locale.
4. Fill in template components such as `Body` and optional `footer` text and submit.
5. Once submitted, templates will be [categorized as per the guidelines](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/template-categorization#template-category-guidelines) and undergo the [approval process](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/template-review#approval-process) refrain from having [marketing content](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/template-categorization#marketing-templates) as part of template components.
6. The template will be approved or rejected after the template components are verified by the system.

  * If business believe the category determined is not consistent with our template category guidelines, confirm there are no [common issues](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/template-review) that leads to rejections and if you are looking for further clarification you may [request a review](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/template-categorization#requesting-review) of the template via [Business Support](https://business.facebook.com/business-support-home/).
7. Once approved template [status](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/overview#template-status) will be changed to `ACTIVE`.
  * Note that template&#039;s status can change automatically from `ACTIVE` to `PAUSED` or `DISABLED` based on customer feedback and engagement. [Monitor status changes](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/template-review#common-rejection-reasons) and take appropriate actions whenever such change occurs.

## Creating an order status template using template creation APIs

To create a template through API and understand the general syntax, required categories and
components please refer to our [Templates](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/overview) document. All the guidelines outlined above in creating templates apply through API as well.

Order status template is categorized as &quot;Utility&quot; template and apart from &#039;name&#039; and &#039;language&#039; of
choice, it has general template components such as BODY, FOOTER, and additionally sub category as &quot;ORDER_STATUS&quot;.

```html
POST https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;WHATSAPP_BUSINESS_BUSINESS_ID&gt;/message_templates
&#123;
  &quot;name&quot;: &quot;&lt;TEMPLATE_NAME&gt;&quot;,
  &quot;language&quot;: &quot;&lt;LANGUAGE_AND_LOCALE_CODE&gt;&quot;,
  &quot;category&quot;: &quot;UTILITY&quot;,
  &quot;sub_category&quot;: &quot;ORDER_STATUS&quot;,
  &quot;components&quot;: [
    &#123;
      &quot;type&quot;: &quot;BODY&quot;,
      &quot;text&quot;: &quot;&lt;TEMPLATE_BODY_TEXT&gt;&quot;
    &#125;,
    &#123;
      &quot;type&quot;: &quot;FOOTER&quot;,
      &quot;text&quot;: &quot;&lt;TEMPLATE_FOOTER_TEXT&gt;&quot;
    &#125;
  ]
&#125;
```

## Sending order status template message

Order status template message allows businesses to send update on the status of the
order as template component parameters.

To send an order status template message, make a `POST` call to `/&lt;PHONE_NUMBER_ID&gt;/messages`
endpoint and attach a message object with `type=template`. Then, add a [template object](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/message-api#template-object) with a
`order_status` component and parameters with latest status on order with order `reference-id`.

For example, the following sample describes how to send `shipped` status on the placed order.

```html
  curl -X  POST \
  &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;FROM_PHONE_NUMBER_ID&gt;/messages&#039; \
 -H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039; \
 -H &#039;Content-Type: application/json&#039; \
 -d &#039;&#123;
    &quot;messaging_product&quot;: &quot;whatsapp&quot;,
    &quot;recipient_type&quot;: &quot;individual&quot;,
    &quot;to&quot;: &quot;&lt;PHONE_NUMBER&gt;&quot;,
    &quot;type&quot;: &quot;template&quot;,
    &quot;template&quot;:
    &#123;
        &quot;name&quot;: &quot;&lt;TEMPLATE_NAME&gt;&quot;,
        &quot;language&quot;:
        &#123;
            &quot;policy&quot;: &quot;deterministic&quot;,
            &quot;code&quot;: &quot;&lt;LANGUAGE_AND_LOCALE_CODE&gt;&quot;
        &#125;,
        &quot;components&quot;:
        [
            &#123;
                &quot;type&quot;: &quot;order_status&quot;,
                &quot;parameters&quot;: [&#123;
                    &quot;type&quot;: &quot;order_status&quot;,
                    &quot;order_status&quot;:
                    &#123;
                        &quot;reference_id&quot;: &quot;reference_id_value&quot;,
                        &quot;order&quot;:
                        &#123;
                            &quot;status&quot;: &quot;processing | partially_shipped | shipped | completed | canceled&quot;,
                            &quot;description&quot;: &quot;&lt;OPTIONAL_DESCRIPTION&gt;&quot;
                        &#125;
                    &#125;
                &#125;]
            &#125;
        ]
    &#125;
&#125;
```

Upon sending an `order_status` message with an invalid transition, you will receive an error webhook with the error code `2046` and message `New order status was not correctly transitioned`.

## Canceling an order

An order can be canceled by sending an `order_status` message with the status `canceled`. The customer cannot pay for an order that is canceled. The customer receives an `order_status` message and the order details page is updated to show that the order is canceled and the `Continue` button is removed. The optional text shown below `Order canceled` on the order details page can be specified using the description field in the `order_status` message.

An order can be canceled only if the user has not already paid for the order. If the user has paid and the business sends an order_status message with canceled status will receive an error webhook with error code `2047` and message `Could not change order status to &#039;canceled&#039;`.
