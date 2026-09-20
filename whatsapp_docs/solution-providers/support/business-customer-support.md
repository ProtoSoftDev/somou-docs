# Support for onboarded clients



This document is intended to solve common problems encountered by clients who have been onboarded onto the WhatsApp Business Platform by a [partner](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/overview).

## Contacting support

If you were onboarded to the WhatsApp Business Platform by a partner and you have [registered](https://developers.facebook.com/async/registration/) as a Meta developer, you can get help by opening a Direct Support ticket using the **Ask a Question** button at:

[https://business.facebook.com/direct-support/](https://business.facebook.com/direct-support/)

See our [Direct Support Information](https://www.facebook.com/business/help/182669425521252) Help Center article for more information about Direct Support.

## Billing and payments

### Billing and payment support

To get support specifically related to billing, payments, and payment methods, open a [Direct Support](https://business.facebook.com/direct-support/) ticket with the following form selections:

* **Topic** — **Dev: Billing, Credit &amp; Pricing**
* **Request Type** — **Credit Card Billing**

If you do not see the **Dev: Billing, Credit &amp; Pricing** topic, please contact your Tech Provider or Tech Partner and ask them to open a ticket for you.

### Add a payment method

If you have been onboarded to the WhatsApp Business Platform by a Tech Provider or Tech Partner, you must add a payment method to your WhatsApp Business account before you can use their app to send and receive messages to WhatsApp users.

See our [Add a credit card to your WhatsApp Business Platform account](https://www.facebook.com/business/help/488291839463771) Help Center article to learn how to add a payment method to your account.

For more information about how pricing works on the WhatsApp Business Platform, see our [About billing for your WhatsApp Business account](https://www.facebook.com/business/help/2225184664363779?id=2129163877102343) Help Center article.

### Switch partners and remove previous line of credit

If your client worked with a partner in the past and still shares the previous credit line, the client may see an error when attempting to switch to a new partner.

1. Backup your client&#039;s messages.
2. Disconnect from WhatsApp Business app.
3. Reconnect in WhatsApp Business app.
4. Start onboarding process using the [WhatsApp Business app user onboarding](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/onboarding-business-app-users) flow.

### Transaction support

To get support for a specific transaction:

* access the Meta Business Suite **Billing &amp; payments** panel at [https://business.facebook.com/billing_hub/](https://business.facebook.com/billing_hub/).
* locate the transaction in either the **Accounts &gt; WhatsApp Business accounts** tab, or the **Payment activity** panel.
* copy the entire transaction ID.
* open a [Direct Support](https://business.facebook.com/direct-support/) ticket and include the transaction ID in the ticket.

### Payment errors

These are common payment errors you may encounter in the Meta Business Suite. If the proposed possible solutions do not work, please [contact support](#contacting-support).

| Error title and description | Possible solution |
| --- | --- |
| ***&lt;CARD_TYPE&gt; &lt;CARD_NUMBER&gt; hasn&#039;t been verified.***&lt;br&gt;&lt;br&gt;_We weren&#039;t able to complete verification, please try again._ | * Try again, making sure you are correctly entering the one-time-password sent to you by your bank.&lt;br&gt;* Make sure you are able to receive your bank&#039;s one-time-password code.&lt;br&gt;* Contact your card&#039;s issuing bank and ask if or why your card was blocked.&lt;br&gt;* Try again after a few days.&lt;br&gt;* Try another card. |
| **Couldn&#039;t save payment method**&lt;br&gt;&lt;br&gt;Couldn&#039;t save payment method. You&#039;ve already saved this payment method to the maximum number of ad accounts. Please use a different payment method. | Use an alternative credit card.&lt;br&gt;&lt;br&gt;Note that the maximum number of accounts sharing a given credit card cannot be increased. There are no exceptions to this limitation. |
| **Request Not Completed**&lt;br&gt;&lt;br&gt;We noticed something unusual and, for your security, this request couldn&#039;t be completed. Please try again later, or visit our Help Center. | * Ask your card&#039;s issuing bank if any restrictions have been placed on the card.&lt;br&gt;* Try another card. |
| **Something went wrong**&lt;br&gt;&lt;br&gt;We couldn&#039;t complete your request. Please try again later. | * Try again in a few days.&lt;br&gt;* Try another card. |

## Display names

Once a business phone number&#039;s display name is reviewed, clients can change their display name using WhatsApp Manager. Newly edited display names must undergo display name review again.

To edit a display name via WhatsApp Manager:

1. Access WhatsApp Manager at [https://business.facebook.com/wa/manage/home/](https://business.facebook.com/wa/manage/home/).
1. Navigate to **Account tools** &gt; **Phone numbers**.
1. Click the phone number.
1. Click the **Profile** tab.
1. Under **Display name**, use the **Edit** button to submit a new name.

Editing the display name, as well as the review outcome, triggers a [phone_number_name_update](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/phone_number_name_update) webhook.

## Unable to send template messages

If you are unable to send template messages, you likely have not added a valid payment method to your account. See our [Add a credit card to your WhatsApp Business Platform account](https://www.facebook.com/business/help/488291839463771) Help Center article to learn how to add a valid payment method.
