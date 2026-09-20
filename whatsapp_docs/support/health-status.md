# Messaging and Calling Health Status



This document describes how to determine whether you can do messaging and calling successfully using a given API resource.

The following nodes have a `health_status` field:

* [WhatsApp Business account](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/whatsapp-business-account-api)
* [WhatsApp Business Phone Number](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/whatsapp-business-account-phone-number-api)
* [WhatsApp Message Template](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/message-template-api)

If you request the `health_status` field on any of these nodes, the API will return a summary of the messaging and calling health of all the nodes involved in messaging and calling requests if using the targeted node. This summary indicates if you will be able to use the API for messaging and calling successfully, or if you will have limited success due to some limitation on one or more nodes, or if you will be prevented from messaging and calling entirely.

## Request syntax

```html
GET /&lt;NODE_ID&gt;?fields=health_status
```

## Response

```json
&#123;
  &quot;health_status&quot;: &#123;
    &quot;can_send_message&quot;: &quot;&lt;OVERALL_MESSAGING_STATUS&gt;&quot;,
    &quot;entities&quot;: [

      /* Only included if targeting a business phone number */
      &#123;
        &quot;entity_type&quot;: &quot;PHONE_NUMBER&quot;,
        &quot;id&quot;: &quot;&lt;BUSINESS_PHONE_NUMBER_ID&gt;&quot;,
        &quot;can_send_message&quot;: &quot;&lt;BUSINESS_PHONE_NUMBER_MESSAGING_STATUS&gt;&quot;,
        &quot;can_receive_call_sip&quot;: &quot;&lt;BUSINESS_PHONE_NUMBER_RECEIVE_CALL_SIP_STATUS&gt;&quot;
      &#125;,

      /* Only included if targeting a template */
      &#123;
        &quot;entity_type&quot;: &quot;MESSAGE_TEMPLATE&quot;,
        &quot;id&quot;: &quot;&lt;TEMPLATE_ID&gt;&quot;,
        &quot;can_send_message&quot;: &quot;&lt;TEMPLATE_MESSAGING_STATUS&gt;&quot;
      &#125;,

      /* WABA, business, and app always included */
      &#123;
        &quot;entity_type&quot;: &quot;WABA&quot;,
        &quot;id&quot;: &quot;&lt;WABA_ID&gt;&quot;,
        &quot;can_send_message&quot;: &quot;&lt;WABA_MESSAGING_STATUS&gt;&quot;
      &#125;,
      &#123;
        &quot;entity_type&quot;: &quot;BUSINESS&quot;,
        &quot;id&quot;: &quot;&lt;BUSINESS_PORTFOLIO_ID&gt;&quot;,
        &quot;can_send_message&quot;: &quot;&lt;BUSINESS_PORTFOLIO_MESSAGING_STATUS&gt;&quot;
      &#125;,
      &#123;
        &quot;entity_type&quot;: &quot;APP&quot;,
        &quot;id&quot;: &quot;&lt;APP_ID&gt;&quot;,
        &quot;can_send_message&quot;: &quot;&lt;APP_MESSAGING_STATUS&gt;&quot;,
        &quot;can_receive_call_sip&quot;: &quot;&lt;APP_RECEIVE_CALL_SIP_STATUS&gt;&quot;
      &#125;
    ]
  &#125;,
  &quot;id&quot;: &quot;&lt;NODE_ID&gt;&quot;
&#125;
```

## Response contents

| Placeholder | Description | Example Value |
| --- | --- | --- |
| `&lt;APP_ID&gt;` | App ID. | `634974688087057` |
| `&lt;APP_MESSAGING_STATUS&gt;` | The app&#039;s messaging health status. See [Messaging Health Status](#health-status-field). | `AVAILABLE` |
| `&lt;APP_RECEIVE_CALL_SIP_STATUS&gt;` | The app&#039;s ability to receive a call over [SIP](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/sip). See [Health Status](#health-status-field). Other calling-related fields are planned for the future. | `AVAILABLE` |
| `&lt;BUSINESS_PORTFOLIO_ID&gt;` | Business portfolio ID. | `506914307656634` |
| `&lt;BUSINESS_PORTFOLIO_MESSAGING_STATUS&gt;` | The business portfolio&#039;s messaging health status. See [Messaging Health Status](#health-status-field). | `AVAILABLE` |
| `&lt;BUSINESS_PHONE_NUMBER_ID&gt;` | Business phone number ID. | `106540352242922` |
| `&lt;BUSINESS_PHONE_NUMBER_MESSAGING_STATUS&gt;` | The business phone number&#039;s messaging health status. See [Messaging Health Status](#health-status-field). | `AVAILABLE` |
| `&lt;BUSINESS_PHONE_NUMBER_RECEIVE_CALL_SIP_STATUS&gt;` | The business phone number&#039;s ability to receive a call over [SIP](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/sip). See [Health Status](#health-status-field). Other calling-related fields are planned for the future. | `AVAILABLE` |
| `&lt;NODE_ID&gt;` | The targeted node&#039;s ID. | `161311403722088` |
| `&lt;OVERALL_MESSAGING_STATUS&gt;` | The overall messaging health status, given all of the nodes involved in a messaging request, if using the targeted node. See [Messaging Health Status](#health-status-field). | `AVAILABLE` |
| `&lt;TEMPLATE_ID&gt;` | Template ID. | `1421988012088524` |
| `&lt;TEMPLATE_MESSAGING_STATUS&gt;` | The template&#039;s messaging health status. See [Messaging Health Status](#health-status-field). | `AVAILABLE` |
| `&lt;WABA_ID&gt;` | WABA ID. | `102290129340398` |
| `&lt;WABA_MESSAGING_STATUS&gt;` | The WABA&#039;s messaging health status. See [Messaging Health Status](#health-status-field). | `AVAILABLE` |

## Health status field

When you attempt to do messaging or calling, multiple nodes are involved, including the app, the business portfolio that owns or has claimed it, a WABA, a business phone number, and a template (if sending a template message).

Each of these nodes can have one of the following health statuses assigned to the `can_send_message` or `can_receive_call_sip` property:

* `AVAILABLE`: Indicates that the node meets all messaging or calling requirements.
* `LIMITED`: Indicates that the node meets messaging or calling requirements, but has some limitations. If a given node has this value, [additional info](#additional-info-property) will be included.
* `BLOCKED`: Indicates that the node does not meet one or more messaging or calling requirements. If a given node has this value, the [errors property](#errors-property) will be included which describes the error and a possible solution.

### Overall status

The overall health status property (`health_status.can_send_message`) will be set as follows:

* If one or more nodes are blocked, it will be set to `BLOCKED`.
* If no nodes are blocked, but one or more nodes are limited, it will be set to `LIMITED`.
* If all nodes are available, it will be set to `AVAILABLE`.

Note: This aggregate status is not available for `can_receive_call_sip`.

## Example request

```html
curl &#039;https://graph.facebook.com/v25.0/106540352242922?fields=health_status&#039; \
-H &#039;Authorization: Bearer EAAJB&#039;
```

## Example response

```json
&#123;
  &quot;health_status&quot;: &#123;
    &quot;can_send_message&quot;: &quot;AVAILABLE&quot;,
    &quot;entities&quot;: [
      &#123;
        &quot;entity_type&quot;: &quot;PHONE_NUMBER&quot;,
        &quot;id&quot;: &quot;106540352242922&quot;,
        &quot;can_send_message&quot;: &quot;AVAILABLE&quot;,
        &quot;can_receive_call_sip&quot;:&quot;AVAILABLE&quot;
      &#125;,
      &#123;
        &quot;entity_type&quot;: &quot;WABA&quot;,
        &quot;id&quot;: &quot;102290129340398&quot;,
        &quot;can_send_message&quot;: &quot;AVAILABLE&quot;
      &#125;,
      &#123;
        &quot;entity_type&quot;: &quot;BUSINESS&quot;,
        &quot;id&quot;: &quot;506914307656634&quot;,
        &quot;can_send_message&quot;: &quot;AVAILABLE&quot;
      &#125;,
      &#123;
        &quot;entity_type&quot;: &quot;APP&quot;,
        &quot;id&quot;: &quot;634974688087057&quot;,
        &quot;can_send_message&quot;: &quot;AVAILABLE&quot;,
        &quot;can_receive_call_sip&quot;:&quot;AVAILABLE&quot;
      &#125;
    ]
  &#125;,
  &quot;id&quot;: &quot;106540352242922&quot;
&#125;
```

## Additional info property

If a given node&#039;s `can_send_message` or `can_receive_call_sip` property is set to `LIMITED`, the `additional_info` property will be included, which provides additional context for the limitation.

### Example limited response

This is an example response to a request on a business phone number that can be used to send messages, but has a limit on the number it can send because its display name has not been approved.

```json
&#123;
  &quot;health_status&quot;: &#123;
    &quot;can_send_message&quot;: &quot;LIMITED&quot;,
    &quot;entities&quot;: [
      &#123;
        &quot;entity_type&quot;: &quot;PHONE_NUMBER&quot;,
        &quot;id&quot;: &quot;106540352242922&quot;,
        &quot;can_send_message&quot;: &quot;LIMITED&quot;,
        &quot;can_receive_call_sip&quot;:&quot;AVAILABLE&quot;,
        &quot;additional_info&quot;: [
          &quot;Your display name has not been approved yet. Your message limit will increase after the display name is approved.&quot;
        ]
      &#125;,
      &#123;
        &quot;entity_type&quot;: &quot;WABA&quot;,
        &quot;id&quot;: &quot;102290129340398&quot;,
        &quot;can_send_message&quot;: &quot;AVAILABLE&quot;
      &#125;,
      &#123;
        &quot;entity_type&quot;: &quot;BUSINESS&quot;,
        &quot;id&quot;: &quot;506914307656634&quot;,
        &quot;can_send_message&quot;: &quot;AVAILABLE&quot;
      &#125;,
      &#123;
        &quot;entity_type&quot;: &quot;APP&quot;,
        &quot;id&quot;: &quot;634974688087057&quot;,
        &quot;can_send_message&quot;: &quot;AVAILABLE&quot;,
        &quot;can_receive_call_sip&quot;:&quot;AVAILABLE&quot;
      &#125;
    ]
  &#125;,
  &quot;id&quot;: &quot;105154286024403&quot;
&#125;
```

## Errors property

If a given node&#039;s `can_send_message` or `can_receive_call_sip` property is set to `BLOCKED`, the `errors` property will be included, which describes the reason for the status and a possible solution.

### Example blocked response

This is an example response to a request on a template that can&#039;t be sent in a template message because it is still in a pending state.

```json
&#123;
  &quot;health_status&quot;: &#123;
    &quot;can_send_message&quot;: &quot;BLOCKED&quot;,
    &quot;entities&quot;: [
      &#123;
        &quot;entity_type&quot;: &quot;MESSAGE_TEMPLATE&quot;,
        &quot;id&quot;: &quot;2632273056924580&quot;,
        &quot;can_send_message&quot;: &quot;BLOCKED&quot;,
        &quot;can_receive_call_sip&quot;:&quot;AVAILABLE&quot;,
        &quot;errors&quot;: [
          &#123;
            &quot;error_code&quot;: 141002,
            &quot;error_description&quot;: &quot;Message templates can only be sent out if they are approved.&quot;,
            &quot;possible_solution&quot;: &quot;Edit or appeal the message template review decision.&quot;
          &#125;
        ]
      &#125;,
      &#123;
        &quot;entity_type&quot;: &quot;WABA&quot;,
        &quot;id&quot;: &quot;102290129340398&quot;,
        &quot;can_send_message&quot;: &quot;AVAILABLE&quot;
      &#125;,
      &#123;
        &quot;entity_type&quot;: &quot;BUSINESS&quot;,
        &quot;id&quot;: &quot;506914307656634&quot;,
        &quot;can_send_message&quot;: &quot;AVAILABLE&quot;
      &#125;,
      &#123;
        &quot;entity_type&quot;: &quot;APP&quot;,
        &quot;id&quot;: &quot;634974688087057&quot;,
        &quot;can_send_message&quot;: &quot;AVAILABLE&quot;,
        &quot;can_receive_call_sip&quot;:&quot;AVAILABLE&quot;
      &#125;
    ]
  &#125;,
  &quot;id&quot;: &quot;2632273056924580&quot;
&#125;
```

### Example blocked response for receiving calls over SIP

This is an example response to a request on a phone number that has not configured [SIP](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/sip).

```json
&#123;
  &quot;health_status&quot;: &#123;
    &quot;can_send_message&quot;: &quot;BLOCKED&quot;,
    &quot;entities&quot;: [
      &#123;
        &quot;entity_type&quot;: &quot;PHONE_NUMBER&quot;,
        &quot;id&quot;: &quot;597727103418254&quot;,
        &quot;can_send_message&quot;: &quot;AVAILABLE&quot;,
        &quot;can_receive_call_sip&quot;: &quot;BLOCKED&quot;,
        &quot;errors&quot;: [
          &#123;
            &quot;error_code&quot;: 138024,
            &quot;error_description&quot;: &quot;WhatsApp Business calling cannot use SIP because it is not enabled&quot;,
            &quot;possible_solution&quot;: &quot;Configure SIP using &#123;PHONE_NUMBER_ID&#125;/settings API&quot;
          &#125;
        ]
      &#125;,
      &#123;
        &quot;entity_type&quot;: &quot;WABA&quot;,
        &quot;id&quot;: &quot;102290129340398&quot;,
        &quot;can_send_message&quot;: &quot;AVAILABLE&quot;
      &#125;,
      &#123;
        &quot;entity_type&quot;: &quot;BUSINESS&quot;,
        &quot;id&quot;: &quot;506914307656634&quot;,
        &quot;can_send_message&quot;: &quot;AVAILABLE&quot;
      &#125;,
      &#123;
        &quot;entity_type&quot;: &quot;APP&quot;,
        &quot;id&quot;: &quot;634974688087057&quot;,
        &quot;can_send_message&quot;: &quot;AVAILABLE&quot;,
        &quot;can_receive_call_sip&quot;: &quot;BLOCKED&quot;,
        &quot;errors&quot;: [
          &#123;
            &quot;error_code&quot;: 138025,
            &quot;error_description&quot;: &quot;This app cannot use SIP for WhatsApp Business calling because it has not configured a SIP server for this business phone number&quot;,
            &quot;possible_solution&quot;: &quot;Configure SIP server using &#123;PHONE_NUMBER_ID&#125;/settings API&quot;
          &#125;
        ]
      &#125;
    ]
  &#125;,
  &quot;id&quot;: &quot;105154286024403&quot;
&#125;
```
