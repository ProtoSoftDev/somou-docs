# Payment Request CTA Templates (Brazil)


**Warning:** This feature is rolling out and might not be available yet for your WhatsApp Business account. If you do not see this option, your account gains access as the rollout completes.

## Overview

**Payment request CTA buttons** enable merchants in Brazil to request payments from customers through template messages on WhatsApp, without requiring order details API integration. Merchants simply embed Pix, Boleto, or Payment Link information in their message templates, allowing customers to pay directly from the conversation.

---

## Key benefits

- **Direct payment code embedding**
  Payment codes (Pix, Boleto, Payment Link) can be embedded directly in templates without order details API integration.
- **Flexible Payment Requests**
  Choose any supported payment method and customize payment details.
- **Flexible Button Combinations**
  Payment request buttons can be combined with other button types within a message template, allowing businesses to offer a variety of actions—not limited to payments—in a single message.
- **Multiple Payment Options**
  Businesses can include up to three payment request buttons in a message, each supporting a different payment method. This enables customers to choose their preferred payment option directly from the conversation.

---

## Supported payment methods

- **Pix Dynamic Code**
- **Boleto**
- **Payment Link**

---

## Creating a payment request template on WhatsApp Manager

To create a template with payment request CTA, business needs a business portfolio with a WhatsApp Business account.

In **WhatsApp Manager** &gt; **Account tools**:

1. Click on `create template`
2. Select `Utility` or `Marketing` category to see the `Default` template format option.
3. Select `Default` template format, and click **Next**
4. Enter the desired `template name` and supported `locale`
  * Depending on the number of `locales` selected there will be an equal number of template variants and businesses need to fill in the template details in respective locale.
5. Fill in template components such as `Header`, `Body`, and optional `footer` text.
  * For the `Header`, you can choose one of three media types: `Text`, `Image` or `Document`. Choose `Document` if you want to send **PDF files** in the header of this template.

6. Add a button of type &#039;Request Payment&#039; from the Buttons menu.
7. Select &#039;Pix&#039;, &#039;Boleto&#039;, or &#039;Payment Link&#039; from the Payment Type dropdown menu.
Enter a sample value in the provided field.
&gt; Note: The value entered is for demonstration purposes only. The actual code or URI will be unique to each customer interaction and will be included in the send payload.

8. Click submit.
9. Once submitted, templates will be [categorized as per the guidelines](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/template-categorization#template-category-guidelines) and undergo the [approval process](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/template-review#approval-process).
10. The template will be approved or rejected after the template components are verified by the system.
  * If business believe the category determined is not consistent with our template category guidelines, please confirm there are no [common issues](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/template-review) that leads to rejections and if you are looking for further clarification you may [request a review](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/template-categorization#requesting-review) of the template via [Business Support](https://business.facebook.com/business-support-home/)
. Once approved template [status](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/overview#template-status) will be changed to `ACTIVE`
  * Please be informed that template&#039;s status can change automatically from `ACTIVE` to `PAUSED` or `DISABLED` based on customer feedback and engagement. [Monitor status changes](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/template-review#common-rejection-reasons) and take appropriate actions whenever such change occurs.

---

## Creating a payment request template using APIs

#### Endpoint

```bash
POST /&#123;WHATSAPP_BUSINESS_ACCOUNT_ID&#125;/message_templates
```

#### Request body

```json
&#123;
  &quot;name&quot;: &quot;&lt;TEMPLATE_NAME&gt;&quot;,
  &quot;language&quot;: &quot;&lt;TEMPLATE_LANGUAGE_CODE&gt;&quot;,
  &quot;category&quot;: &quot;UTILITY&quot;,
  &quot;components&quot;: [
    &#123;
      &quot;type&quot;: &quot;HEADER&quot;,
      &quot;format&quot;: &quot;text&quot;,
      &quot;text&quot;: &quot;Header Text&quot;
    &#125;,
    &#123;
      &quot;type&quot;: &quot;BODY&quot;,
      &quot;text&quot;: &quot;Body Text&quot;
    &#125;,
    &#123;
      &quot;type&quot;: &quot;FOOTER&quot;,
      &quot;text&quot;: &quot;Footer text&quot;
    &#125;,
    &#123;
      &quot;type&quot;: &quot;BUTTONS&quot;,
      &quot;buttons&quot;: [
        &#123;
          &quot;type&quot;: &quot;PAYMENT_REQUEST&quot;,
          &quot;text&quot;: &quot;Copy Boleto code&quot;,
          &quot;payment_setting&quot;: &#123;
            &quot;type&quot;: &quot;boleto&quot;,
            &quot;boleto&quot;: &#123;
              &quot;digitable_line&quot;: &quot;03399026944140000002628346101018898510000008848&quot;
            &#125;
          &#125;
        &#125;,
        &#123;
          &quot;type&quot;: &quot;PAYMENT_REQUEST&quot;,
          &quot;text&quot;: &quot;Copy Pix code&quot;,
          &quot;payment_setting&quot;: &#123;
            &quot;type&quot;: &quot;pix_dynamic_code&quot;,
            &quot;pix_dynamic_code&quot;: &#123;
              &quot;code&quot;: &quot;00020101021226700014br.gov.bcb.pix2548pix.example.com...&quot;
            &#125;
          &#125;
        &#125;,
        &#123;
          &quot;type&quot;: &quot;PAYMENT_REQUEST&quot;,
          &quot;text&quot;: &quot;Open payment link&quot;,
          &quot;payment_setting&quot;: &#123;
            &quot;type&quot;: &quot;payment_link&quot;,
            &quot;payment_link&quot;: &#123;
              &quot;uri&quot;: &quot;https://my-payment-link-url&quot;
            &#125;
          &#125;
        &#125;
      ]
    &#125;
  ]
&#125;
```

#### Example response

```json
&#123;
  &quot;id&quot;: &quot;123455683495832&quot;,
  &quot;status&quot;: &quot;Approved&quot;,
  &quot;category&quot;: &quot;UTILITY&quot;
&#125;
```

---

## Sending payment request messages

The following image shows examples of payment request template messages rendered in WhatsApp, with Pix, Boleto, and Payment Link buttons respectively.

#### Endpoint

```bash
POST /&#123;PHONE_NUMBER_ID&#125;/messages
```

#### Request body

```json
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;recipient_type&quot;: &quot;individual&quot;,
  &quot;to&quot;: &quot;&lt;PHONE_NUMBER&gt;&quot;,
  &quot;type&quot;: &quot;template&quot;,
  &quot;template&quot;: &#123;
    &quot;name&quot;: &quot;&lt;TEMPLATE_NAME&gt;&quot;,
    &quot;language&quot;: &#123;
      &quot;code&quot;: &quot;&lt;LANGUAGE&gt;&quot;
    &#125;,
    &quot;components&quot;: [
      &#123;
        &quot;type&quot;: &quot;button&quot;,
        &quot;sub_type&quot;: &quot;payment_request&quot;,
        &quot;index&quot;: &quot;0&quot;,
        &quot;parameters&quot;: [
          &#123;
            &quot;type&quot;: &quot;action&quot;,
            &quot;action&quot;: &#123;
              &quot;payment_request&quot;: &#123;
                &quot;payment_setting&quot;: &#123;
                  &quot;type&quot;: &quot;boleto&quot;,
                  &quot;boleto&quot;: &#123;
                    &quot;digitable_line&quot;: &quot;03399026944140000002628346101018898510000008848&quot;
                  &#125;
                &#125;
              &#125;
            &#125;
          &#125;
        ]
      &#125;,
      &#123;
        &quot;type&quot;: &quot;button&quot;,
        &quot;sub_type&quot;: &quot;payment_request&quot;,
        &quot;index&quot;: &quot;1&quot;,
        &quot;parameters&quot;: [
          &#123;
            &quot;type&quot;: &quot;action&quot;,
            &quot;action&quot;: &#123;
              &quot;payment_request&quot;: &#123;
                &quot;payment_setting&quot;: &#123;
                  &quot;type&quot;: &quot;pix_dynamic_code&quot;,
                  &quot;pix_dynamic_code&quot;: &#123;
                    &quot;code&quot;: &quot;00020101021226700014br.gov.bcb.pix2548pix.example.com...&quot;
                  &#125;
                &#125;
              &#125;
            &#125;
          &#125;
        ]
      &#125;,
      &#123;
        &quot;type&quot;: &quot;button&quot;,
        &quot;sub_type&quot;: &quot;payment_request&quot;,
        &quot;index&quot;: &quot;2&quot;,
        &quot;parameters&quot;: [
          &#123;
            &quot;type&quot;: &quot;action&quot;,
            &quot;action&quot;: &#123;
              &quot;payment_request&quot;: &#123;
                &quot;payment_setting&quot;: &#123;
                  &quot;type&quot;: &quot;payment_link&quot;,
                  &quot;payment_link&quot;: &#123;
                    &quot;uri&quot;: &quot;https://my-payment-link-url&quot;
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

#### Example response

```json
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;contacts&quot;: [
    &#123;
      &quot;input&quot;: &quot;&lt;PHONE_NUMBER&gt;&quot;,
      &quot;wa_id&quot;: &quot;&lt;PHONE_NUMBER&gt;&quot;
    &#125;
  ],
  &quot;messages&quot;: [
    &#123;
      &quot;id&quot;: &quot;wamid.HBgNNTUxMTk4Nzg2NjI5NRUCABEYEjUxOTBGNEU0RDA2MjdCMUVBOQA=&quot;,
      &quot;message_status&quot;: &quot;accepted&quot;
    &#125;
  ]
&#125;
```

---

## Combining with limited-time offer templates

Payment request CTA buttons can be combined with limited-time offer templates to send time-sensitive payment requests that display a countdown timer alongside payment options. To add the `limited_time_offer` component to your template, include it in the `components` array of your [template creation payload](#creating-a-payment-request-template-using-apis). For the full component schema, supported header formats, and button ordering requirements, see [Limited-time offer templates](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/marketing-templates/limited-time-offer-templates).

The following images show examples of limited-time offer templates combined with payment request CTA buttons rendered in WhatsApp.

### Managing payment code and link expiration

The payment request CTA button remains active in the conversation even after the limited-time offer is displayed as expired. WhatsApp does not automatically disable the payment button when the Pix code, Boleto, or Payment Link is no longer valid. You are responsible for managing the expiration of the underlying payment code or link to match your offer&#039;s validity — whether the offer is time-bound, inventory-limited, or invalidated for any other reason.

---

## Parameters

#### Buttons object &#123;#buttonsobject&#125;
| Field Name | Optional? | Type | Description |
| --- | --- | --- | --- |
| `type` | Required | String | Must be `PAYMENT_REQUEST` for payment messages. |
| `text` | Required | String | The button label. Must be `Copy Pix code` for `pix_dynamic_code`, `Copy Boleto code` for `boleto`, or `Open payment link` for `payment_link`. |
| `payment_setting` | Required | [Payment Setting Object](#paymentsettingsobject) | Payment configuration object. |

#### Payment setting &#123;#paymentsettingsobject&#125;
| Field Name | Optional? | Type | Description |
| --- | --- | --- | --- |
| `type` | Required | String | One of `pix_dynamic_code`, `payment_link`, `boleto`. |
| One of the following objects: `pix_dynamic_code`, `payment_link`, `boleto`. | Required | Object | Payment instructions which will be displayed to buyers during the checkout process. |

#### Dynamic Pix code object &#123;#dynamicpixobject&#125;
| **Field Name** | **Optional?** | **Type** | **Description** |
| --- | --- | --- | --- |
| `pix_dynamic_code` | Required | String | The dynamic Pix code which will be copied by the buyer. |

#### Payment link object &#123;#paymentlinkobject&#125;
| **Field Name** | **Optional?** | **Type** | **Description** |
| --- | --- | --- | --- |
| `uri` | Required | String | The Payment Link&#039;s uri which will be opened in the web browser, when the user taps on the Payment Link CTA button. |

#### Boleto object &#123;#boletoobject&#125;
| **Field Name** | **Optional?** | **Type** | **Description** |
| --- | --- | --- | --- |
| `digitable_line` | Required | String | The Boleto digital line / code which will be copied to the clipboard, when the user taps on the Boleto CTA button. |

