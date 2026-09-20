# Cashfree Payment Gateway Integration Guide



## Purpose

The purpose of this document is to lay down the payment integration with Cashfree that is required for a merchant or Solution Partner that has setup a chatbot using WhatsApp Business APIs and needs to receive payments from WhatsApp users.

This document covers the set of APIs that need to be integrated and how the integration works in tandem with the WhatsApp Business API integration. For additional details regarding Cashfree payment integration, please refer to the Cashfree documentation.

Where this fits into the entire flow in terms of integration to the WA P2M product: The following document covers the requests, responses in red in the flow diagram below.

## Cashfree payment integration
### Setup
- Obtain credentials (client id and secret) from Cashfree dashboard after setting up the account.
- Obtain the merchant vpa, mcc, and purpose code from Cashfree. Use these values (VPA, mcc, purpose code) to set up the payment configuration on WhatsApp. Multiple vpas are supported and hence multiple payment configurations can be set up. Each payment configuration should have one VPA.
### Create order
This creates the order at Cashfree end.

[Reference doc](https://docs.cashfree.com/reference/createorder)

#### Request

```curl
curl --request POST \
     --url https://sandbox.cashfree.com/pg/orders \ // Production URL : https://api.cashfree.com/pg/orders
     --header &#039;accept: application/json&#039; \
     --header &#039;content-type: application/json&#039; \
     --header &#039;x-api-version: 2022-09-01&#039; \
     --header &#039;x-client-id: 26268833355ef02b8ff299390c886262&#039; \
     --header &#039;x-client-secret: 1708cc38a3c1c3c2512d79b3530dc5cc65ad2fde&#039; \
     --data &#039;
    &#123;
     &quot;customer_details&quot;: &#123;
          &quot;customer_id&quot;: &quot;7112AAA812234&quot;,
          &quot;customer_email&quot;: &quot;john&#064;cashfree.com&quot;,
          &quot;customer_phone&quot;: &quot;9908734801&quot;,
          &quot;customer_bank_account_number&quot;: &quot;1518121112&quot;,
          &quot;customer_bank_ifsc&quot;: &quot;CITI0000001&quot;,
          &quot;customer_bank_code&quot;: 3333
     &#125;,
     &quot;order_meta&quot;: &#123;
          &quot;notify_url&quot;: &quot;https://b8af79f41056.eu.ngrok.io/webhook.php&quot;, // Notification URL where status notifications sent - can be different for different merchants
          &quot;payment_methods&quot;: &quot;upi&quot;
     &#125;,
      &quot;order_tags&quot;: &#123;
          &quot;channel&quot;: &quot;WhatsApp&quot; // Custom tag
     &#125;,
     &quot;order_id&quot;: &quot;order02&quot;,
     &quot;order_amount&quot;: 200.5,
     &quot;order_currency&quot;: &quot;INR&quot;,
     &quot;order_expiry_time&quot;: &quot;2022-12-29T00:00:00Z&quot;,
     &quot;order_note&quot;: &quot;Test order&quot;
    &#125;
```

#### Response

```json
&#123;
  &quot;cf_order_id&quot;: 3401407,
  &quot;created_at&quot;: &quot;2022-12-26T14:11:07+05:30&quot;,
  &quot;customer_details&quot;: &#123;
    &quot;customer_id&quot;: &quot;7112AAA812234&quot;,
    &quot;customer_name&quot;: null,
    &quot;customer_email&quot;: &quot;john&#064;cashfree.com&quot;,
    &quot;customer_phone&quot;: &quot;9908734801&quot;
  &#125;,
  &quot;entity&quot;: &quot;order&quot;,
  &quot;order_amount&quot;: 200.5,
  &quot;order_currency&quot;: &quot;INR&quot;,
  &quot;order_expiry_time&quot;: &quot;2022-12-29T05:30:00+05:30&quot;,
  &quot;order_id&quot;: &quot;order02&quot;,
  &quot;order_meta&quot;: &#123;
    &quot;return_url&quot;: null,
    &quot;notify_url&quot;: &quot;https://b8af79f41056.eu.ngrok.io/webhook.php&quot;,
    &quot;payment_methods&quot;: &quot;upi&quot;
  &#125;,
  &quot;order_note&quot;: &quot;Test order&quot;,
  &quot;order_splits&quot;: [],
  &quot;order_status&quot;: &quot;ACTIVE&quot;,
  &quot;order_tags&quot;: &#123;
    &quot;channel&quot;: &quot;WhatsApp&quot; // Custom tag
  &#125;,
  &quot;payment_session_id&quot;: &quot;session_364o8HjN0-gc6n_n4EBEPOXriJUJvCeVIdy9u8ihOhwvpNg9F1wMorWmkVxUR90kTe473bpbotNxyZ6Fze8M0w42_BpTxoEWsbBR21y7i0nh&quot;,
  &quot;payments&quot;: &#123;
    &quot;url&quot;: &quot;https://sandbox.cashfree.com/pg/orders/order02/payments&quot; // production URL&#039;s are different
  &#125;,
  &quot;refunds&quot;: &#123;
    &quot;url&quot;: &quot;https://sandbox.cashfree.com/pg/orders/order02/refunds&quot;
  &#125;,
  &quot;settlements&quot;: &#123;
    &quot;url&quot;: &quot;https://sandbox.cashfree.com/pg/orders/order02/settlements&quot;
  &#125;,
  &quot;terminal_data&quot;: null
&#125;
```

### Order pay
This API returns the UPI intent url that contains the parameters required for the WhatsApp APIs.
[Reference doc](https://docs.cashfree.com/reference/orderpay)
#### Request

```curl
curl --request POST \
     --url https://sandbox.cashfree.com/pg/orders/sessions \
     --header &#039;accept: application/json&#039; \
     --header &#039;content-type: application/json&#039; \
     --header &#039;x-api-version: 2022-09-01&#039; \
     --data &#039;
    &#123;
     &quot;payment_method&quot;: &#123;
          &quot;upi&quot;: &#123;
               &quot;channel&quot;: &quot;link&quot;,
               &quot;upi_id&quot;: &quot;rajnandan1&#064;okhdfcbak&quot;,
               &quot;upi_expiry_minutes&quot;: 10
          &#125;
     &#125;,
     &quot;payment_session_id&quot;: &quot;session_364o8HjN0-gc6n_n4EBEPOXriJUJvCeVIdy9u8ihOhwvpNg9F1wMorWmkVxUR90kTe473bpbotNxyZ6Fze8M0w42_BpTxoEWsbBR21y7i0nh&quot; // this is from the create order API response
    &#125;
```

#### Response

```json
&#123;
  &quot;action&quot;: &quot;custom&quot;,
  &quot;cf_payment_id&quot;: 885899755, // is the transaction ID, is also present in UPI url
  &quot;channel&quot;: &quot;link&quot;,
  &quot;data&quot;: &#123;
    &quot;url&quot;: null,
    &quot;payload&quot;: &#123;
      &quot;bhim&quot;: &quot;https://payments-test.cashfree.com/pgbillpayuiapi/simulator/885899755?txnId=885899755&amp;amount=200.50&amp;pa=cashfree&#064;testbank&amp;pn=Cashfree&amp;tr=885899755&amp;am=200.50&amp;cu=INR&amp;mode=00&amp;purpose=00&amp;mc=5732&amp;tn=Cashfree%20Simulator%20Payment&quot;,
      &quot;default&quot;: &quot;https://payments-test.cashfree.com/pgbillpayuiapi/simulator/885899755?txnId=885899755&amp;amount=200.50&amp;pa=cashfree&#064;testbank&amp;pn=Cashfree&amp;tr=885899755&amp;am=200.50&amp;cu=INR&amp;mode=00&amp;purpose=00&amp;mc=5732&amp;tn=Cashfree%20Simulator%20Payment&quot;,
      &quot;gpay&quot;: &quot;https://payments-test.cashfree.com/pgbillpayuiapi/simulator/885899755?txnId=885899755&amp;amount=200.50&amp;pa=cashfree&#064;testbank&amp;pn=Cashfree&amp;tr=885899755&amp;am=200.50&amp;cu=INR&amp;mode=00&amp;purpose=00&amp;mc=5732&amp;tn=Cashfree%20Simulator%20Payment&quot;,
      &quot;paytm&quot;: &quot;https://payments-test.cashfree.com/pgbillpayuiapi/simulator/885899755?txnId=885899755&amp;amount=200.50&amp;pa=cashfree&#064;testbank&amp;pn=Cashfree&amp;tr=885899755&amp;am=200.50&amp;cu=INR&amp;mode=00&amp;purpose=00&amp;mc=5732&amp;tn=Cashfree%20Simulator%20Payment&quot;,
      &quot;phonepe&quot;: &quot;https://payments-test.cashfree.com/pgbillpayuiapi/simulator/885899755?txnId=885899755&amp;amount=200.50&amp;pa=cashfree&#064;testbank&amp;pn=Cashfree&amp;tr=885899755&amp;am=200.50&amp;cu=INR&amp;mode=00&amp;purpose=00&amp;mc=5732&amp;tn=Cashfree%20Simulator%20Payment&quot;,
      &quot;web&quot;: &quot;https://sandbox.cashfree.com/pg/view/upi/qcrgfb.session_364o8HjN0-gc6n_n4EBEPOXriJUJvCeVIdy9u8ihOhwvpNg9F1wMorWmkVxUR90kTe473bpbotNxyZ6Fze8M0w42_BpTxoEWsbBR21y7i0nh.c252cd27-c877-4a51-8352-837d04a2f4c2&quot;
    &#125;,
    &quot;content_type&quot;: null,
    &quot;method&quot;: null
  &#125;,
  &quot;payment_amount&quot;: 200.5,
  &quot;payment_method&quot;: &quot;upi&quot;
&#125;
```

### Parsing the response
Store the `cf_payment_id` as a unique identifier of the payment at Cashfree end. As Cashfree supports multiple payments for a given `order_id` (or `cf_order_id`), storing the `cf_payment_id` is important for deduping multiple/duplicate payments (if they occur due to a bug or otherwise).

Extract the key-value pairs in data.payload.default

- Verify `am` to be the same as the amount that was set.
- Use the value in `tr` as the reference_id while setting up the Parameters object to send the payment message using WhatsApp API.
- The value of `pa` is the merchant vpa that will be used for this transaction. The payment configuration name corresponding to the vpa returned should be used as payment_configuration while setting up the Parameters object to send the payment message using WhatsApp API.
- In case the merchant vpa obtained from above does not match with any of the vpas set in the WhatsApp payment configuration, payment should be discontinued. Please reach out to Cashfree to confirm the updated vpa and update the WhatsApp payment configuration accordingly.
- Also check whether the `mode` and `purpose` values are the same as those set in the payment configuration. In case of mismatch, log the mismatch to follow up with Cashfree about the right/updated values. Do not block the payment due to this mismatch.

```curl
&quot;default&quot;: &quot;upi://pay?pa=cfsukoonaa&#064;yesbank&amp;pn=Sukoon&amp;tr=877376394&amp;am=10.00&amp;cu=INR&amp;mode=00&amp;purpose=00&amp;mc=5399&amp;tn=877376394&quot;
```

## Webhook
Once the user completes the payment on WhatsApp, Cashfree will send a webhook about payment completion. Please note that while WhatsApp also shares a payment completion signal, please rely on the signal from Cashfree for the final payment status to avoid reconciliation issues.

Based on the payment_status in the webhook, update the order status for the user using the WhatsApp API.

[Reference Guide](https://docs.cashfree.com/docs/payment-webhooks)

### Successful transaction webhook

```json
&#123;
  &quot;data&quot;: &#123;
    &quot;order&quot;: &#123;
      &quot;order_id&quot;: &quot;1633615918&quot;,
      &quot;order_amount&quot;: 1.00,
      &quot;order_currency&quot;: &quot;INR&quot;,
      &quot;order_tags&quot;: null
    &#125;,
    &quot;payment&quot;: &#123;
      &quot;cf_payment_id&quot;: 1107253,
      &quot;payment_status&quot;: &quot;SUCCESS&quot;,
      &quot;payment_amount&quot;: 1.00,
      &quot;payment_currency&quot;: &quot;INR&quot;,
      &quot;payment_message&quot;: &quot;Transaction pending&quot;,
      &quot;payment_time&quot;: &quot;2021-10-07T19:42:40+05:30&quot;,
      &quot;bank_reference&quot;: &quot;1903772466&quot;,
      &quot;auth_id&quot;: null,
      &quot;payment_method&quot;: &#123;
        &quot;upi&quot;: &#123;
          &quot;channel&quot;:null,
          &quot;upi_id&quot;:&quot;miglaniyogesh7&#064;okhdfcbank&quot; &#125;
                &#125;,
       &quot;payment_group&quot;:&quot;upi&quot;,
    &quot;customer_details&quot;: &#123;
      &quot;customer_name&quot;: &quot;Yogesh&quot;,
      &quot;customer_id&quot;: &quot;12121212&quot;,
      &quot;customer_email&quot;: &quot;yogesh.miglani&#064;gmail.com&quot;,
      &quot;customer_phone&quot;: &quot;9666699999&quot;
    &#125;
  &#125;,
  &quot;event_time&quot;: &quot;2021-10-07T19:42:44+05:30&quot;,
  &quot;type&quot;: &quot;PAYMENT_SUCCESS_WEBHOOK&quot;
&#125;
```

### Failed transaction webhook

```json
&#123;
  &quot;data&quot;: &#123;
    &quot;order&quot;: &#123;
      &quot;order_id&quot;: &quot;order_01&quot;,
      &quot;order_amount&quot;: 2,
      &quot;order_currency&quot;: &quot;INR&quot;,
      &quot;order_tags&quot;: null
    &#125;,
    &quot;payment&quot;: &#123;
      &quot;cf_payment_id&quot;: 975677709,
      &quot;payment_status&quot;: &quot;FAILED&quot;,
      &quot;payment_amount&quot;: 2,
      &quot;payment_currency&quot;: &quot;INR&quot;,
      &quot;payment_message&quot;: &quot;ZA::U19::Transaction fail&quot;,
      &quot;payment_time&quot;: &quot;2022-05-25T14:28:22+05:30&quot;,
      &quot;bank_reference&quot;: &quot;214568722700&quot;,
      &quot;auth_id&quot;: null,
      &quot;payment_method&quot;: &#123;
        &quot;upi&quot;: &#123;
          &quot;channel&quot;: null,
          &quot;upi_id&quot;: &quot;9611199227&#064;paytm&quot;
        &#125;
      &#125;,
      &quot;payment_group&quot;: &quot;upi&quot;
    &#125;,
    &quot;customer_details&quot;: &#123;
      &quot;customer_name&quot;: null,
      &quot;customer_id&quot;: &quot;7112AAA812234&quot;,
      &quot;customer_email&quot;: &quot;miglaniyogesh7&#064;gmail.com&quot;,
      &quot;customer_phone&quot;: &quot;9611199227&quot;
    &#125;,
    &quot;error_details&quot;: &#123;
      &quot;error_code&quot;: &quot;TRANSACTION_DECLINED&quot;,
      &quot;error_description&quot;: &quot;issuer bank or payment service provider declined the transaction&quot;,
      &quot;error_reason&quot;: &quot;auth_declined&quot;,
      &quot;error_source&quot;: &quot;customer&quot;
    &#125;
  &#125;,
  &quot;event_time&quot;: &quot;2022-05-25T14:28:38+05:30&quot;,
  &quot;type&quot;: &quot;PAYMENT_FAILED_WEBHOOK&quot;
&#125;
```

## Status check
Status API can be used as an alternative in case the webhook isn&#039;t received within a certain timeframe. Based on the payment_status in the response, update the order status for the user using the WhatsApp API.

[Reference Doc](https://docs.cashfree.com/reference/getpaymentbyid)

#### Request

```curl
curl --request GET \
     --url https://sandbox.cashfree.com/pg/orders/order02/payments/885899755 \
     --header &#039;accept: application/json&#039; \
     --header &#039;x-api-version: 2022-09-01&#039; \
     --header &#039;x-client-id: 26268833355ef02b8ff299390c886262&#039; \
     --header &#039;x-client-secret: 1708cc38a3c1c3c2512d79b3530dc5cc65ad2fde&#039;
```

#### Response

```json
&#123;
  &quot;auth_id&quot;: null,
  &quot;authorization&quot;: null,
  &quot;bank_reference&quot;: null,
  &quot;cf_payment_id&quot;: 885704957,
  &quot;entity&quot;: &quot;payment&quot;,
  &quot;error_details&quot;: null,
  &quot;is_captured&quot;: true,
  &quot;order_amount&quot;: 10.15,
  &quot;order_id&quot;: &quot;12345&quot;,
  &quot;payment_amount&quot;: 10.15,
  &quot;payment_completion_time&quot;: &quot;2022-10-27T08:43:05+05:30&quot;,
  &quot;payment_currency&quot;: &quot;INR&quot;,
  &quot;payment_group&quot;: &quot;upi&quot;,
  &quot;payment_message&quot;: &quot;Transaction Successful&quot;,
  &quot;payment_method&quot;: &#123;
    &quot;upi&quot;: &#123;
      &quot;channel&quot;: &quot;link&quot;
    &#125;
  &#125;,
  &quot;payment_status&quot;: &quot;SUCCESS&quot;,
  &quot;payment_time&quot;: &quot;2022-10-27T08:42:07+05:30&quot;
&#125;

OR

&#123;
  &quot;auth_id&quot;: null,
  &quot;authorization&quot;: null,
  &quot;bank_reference&quot;: null,
  &quot;cf_payment_id&quot;: 885899755,
  &quot;entity&quot;: &quot;payment&quot;,
  &quot;error_details&quot;: null,
  &quot;is_captured&quot;: false,
  &quot;order_amount&quot;: 200.5,
  &quot;order_id&quot;: &quot;order02&quot;,
  &quot;payment_amount&quot;: 200.5,
  &quot;payment_completion_time&quot;: &quot;2022-12-26T14:24:56+05:30&quot;,
  &quot;payment_currency&quot;: &quot;INR&quot;,
  &quot;payment_gateway_details&quot;: null,
  &quot;payment_group&quot;: &quot;upi&quot;,
  &quot;payment_message&quot;: &quot;User dropped and did not complete the two factor authentication&quot;,
  &quot;payment_method&quot;: &#123;
    &quot;upi&quot;: &#123;
      &quot;channel&quot;: &quot;link&quot;,
      &quot;upi_id&quot;: &quot;987836150&quot;
    &#125;
  &#125;,
  &quot;payment_status&quot;: &quot;USER_DROPPED&quot;,
  &quot;payment_time&quot;: &quot;2022-12-26T14:14:56+05:30&quot;
&#125;
```

## Refund
Refund API can be used to trigger refunds to the user.

[Reference Doc](https://docs.cashfree.com/reference/createrefund)
#### Request

```curl
curl --request POST \
     --url https://sandbox.cashfree.com/pg/orders/12345/refunds \
     --header &#039;accept: application/json&#039; \
     --header &#039;content-type: application/json&#039; \
     --header &#039;x-api-version: 2022-01-01&#039; \
     --header &#039;x-client-id: xxxxxx&#039; \
     --header &#039;x-client-secret: xxxxxx&#039; \
     --data &#039;
    &#123;
     &quot;refund_amount&quot;: 5,
     &quot;refund_id&quot;: &quot;refund12345&quot;
    &#125;
```

#### Response

```json
&#123;
  &quot;cf_payment_id&quot;: 885704957,
  &quot;cf_refund_id&quot;: &quot;refund_49234&quot;,
  &quot;created_at&quot;: &quot;2022-10-27T14:35:22+05:30&quot;,
  &quot;entity&quot;: &quot;refund&quot;,
  &quot;metadata&quot;: null,
  &quot;order_id&quot;: &quot;12345&quot;,
  &quot;processed_at&quot;: null,
  &quot;refund_amount&quot;: 5,
  &quot;refund_arn&quot;: null,
  &quot;refund_charge&quot;: 0,
  &quot;refund_currency&quot;: &quot;INR&quot;,
  &quot;refund_id&quot;: &quot;refund12345&quot;,
  &quot;refund_mode&quot;: &quot;STANDARD&quot;,
  &quot;refund_note&quot;: null,
  &quot;refund_splits&quot;: [],
  &quot;refund_status&quot;: &quot;PENDING&quot;,
  &quot;refund_type&quot;: &quot;MERCHANT_INITIATED&quot;,
  &quot;status_description&quot;: &quot;In Progress&quot;
&#125;
```

## Handling special cases
### Order expiry
- Cashfree allows setting the expiry time for an order in the Create Order API. Use that to set preferred expiry time.
- Post order expiry, if no webhook was received, do a status check to ensure that the order expired and then cancel the order at WhatsApp to update the user.

### Handling failed payments
- The Payment message sent to the user via WhatsApp allows for multiple retries upon failure (i.e. the Pay button is available until successful payment). However Cashfree requires the reference id (&quot;tr&quot; field in the url received in Order Pay response) to be unique for each payment.
- So when a failed payment response is received from Cashfree, update the status of order at WhatsApp to cancelled. Post that a new payment message can be sent to the user to retry the payment.
- In case, there is a delay in cancellation and the user ends up making a successful payment, Cashfree will not send a webhook to the merchant but does an auto-refund, without any additional action required by the merchant. In the case of a customer query in such a scenario (where they claim the transaction was successful but the payment cannot be found at Cashfree), suggest to the user that refund will be processed in a few days.

### Canceling Order for successful transaction
There may arise a scenario where Cashfree shared a successful payment signal but the order cannot be fulfilled by the merchant. In such scenario, process refund for the payment via one of the following mechanisms:

- Use Refund API.
- Use Cashfree dashboard for merchants.
