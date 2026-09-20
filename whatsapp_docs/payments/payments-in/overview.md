# Payments API — India



The Payments API enables you to accept payments from your customers through all UPI apps installed on their devices and other payment methods such as cards, NetBanking, and wallets via WhatsApp.

You can send invoice (`order_details`) messages to your customers, then get notified about payment status updates through webhook notifications from the payment gateway.

## Compare the integration models

The integration model you use depends on your payment gateway. The two models differ in the following ways:

1. **[UPI Intent Mode](https://developers.facebook.com/documentation/business-messaging/whatsapp/payments/payments-in/upi-intent)**: You can use this mode with any payment gateway that supports UPI Intent generation.
1. **[Payment Gateway Deep Integration Mode](https://developers.facebook.com/documentation/business-messaging/whatsapp/payments/payments-in/pg)**: Currently supported for Razorpay, PayU, Billdesk, and Zaakpay only.

| User experience | UPI Intent Mode | Payment Gateway Deep Integration Mode |
| --- | --- | --- |
| **Native support for &quot;Other payment methods&quot;**&lt;br&gt;For example: NetBanking, cards, wallets&lt;br&gt; | ❌&lt;br&gt;&lt;br&gt;Alternative: Send payment links&lt;br&gt; | ✅&lt;br&gt; |
| **Native support for UPI Intent** | ✅&lt;br&gt; | ✅&lt;br&gt; |
| **Native Payment Status Notification** | ❌&lt;br&gt; | ✅&lt;br&gt; |

| Integration features | UPI Intent Mode | Payment Gateway Deep Integration Mode |
| --- | --- | --- |
| **Refunds from WhatsApp APIs** | ❌&lt;br&gt; | ✅&lt;br&gt; |
| **Payment Status from WhatsApp webhooks** | ❌&lt;br&gt; | ✅&lt;br&gt; |

## Prerequisites for integration
1. **Essential Payments APIs are available at SP/TP**
1. **Access to merchant order trigger APIs / CSVs** needed to trigger an order. (for example, amount, goods, or service details)
1. **Access to payment posting APIs** needed to close an order (for example, ticket generation APIs to create tickets once payment is received)

### Full payment gateway deep integration mode

1. **Find out payment gateway account owner**: This authorizes linking the account to Meta Business Suite.

### UPI Intent mode
1. **Find out VPA IDs, MCC, and PC** for your business from the merchant&#039;s payment gateway.
1. **Access to payment gateway API docs**:
1. UPI Intent S2S calls
1. Webhook configuration for payment status

## Example use cases and features needed

| Use case | Essential feature set |
| --- | --- |
| **Buying Tickets**&lt;br&gt;For example: Metro, bus, event tickets&lt;br&gt; | * Order Details Message&lt;br&gt;* Payment Status Webhook/API&lt;br&gt;* Order Status Message&lt;br&gt;* Refund |
| **Payment Reminders**&lt;br&gt;For example: Bill payments, subscription renewals, insurance renewals&lt;br&gt; | * Order Details Template&lt;br&gt;* Payment Status Webhook/API&lt;br&gt;* Order Status Message&lt;br&gt;* Refund |

## Support

- In case you run into an issue, reach out to [direct support](https://business.facebook.com/direct-support/). Make sure to choose the correct case type: **&quot;WaBiz: Business Payments API&quot;** so you get a faster resolution.
- [Sign up for office hours](https://outlook.office365.com/owa/calendar/WhatsappBusinessPaymentsIndiaOfficeHourse&#064;meta.com/bookings/). *Make sure to write down your issues in the form provided.*
