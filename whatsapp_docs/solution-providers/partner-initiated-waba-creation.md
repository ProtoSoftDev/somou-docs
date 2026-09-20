# Partner-initiated WABA creation



If you are a Solution Partner and you don&#039;t want to onboard a client with [Embedded Signup](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/overview), you can use Meta Business Suite to initiate WABA creation for a client. This generates a WABA creation request which your client can review in Meta Business Suite. The client can then accept the request (or decline it) and optionally add a business phone number.

If accepted, the WABA will be created and its ownership will be assigned to the client. You will also be given access to the WABA based on the permissions you defined when you initiated the request. You can then use your [system token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#system-user-access-tokens) to add a business phone number to the client&#039;s WABA (if they opted not to create one) and share your credit line with the client, which completes the onboarding process.

Note that if you use this method to create a WABA for a client, and the client accepts it, you must use your system token when accessing the WABA (a [business token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#business-integration-system-user-access-tokens) will not work), and you must use the API to share your credit line with the client (it cannot be shared as part of the initiation or acceptance process).

## Initiating WABA creation

1. Access [Meta Business Suite](https://business.facebook.com/).
1. If you have multiple business portfolios, select the appropriate portfolio using the dropdown menu at the top-left of the page.
1. Navigate to the **Settings** (gear icon) &gt; **Accounts** &gt; **WhatsApp accounts** panel.
1. Click the blue **Add** dropdown button and select **Request a new WhatsApp Business account for a client**.
1. Complete the flow, filling out each field as appropriate.
1. Navigate to the **Settings** &gt; **Requests** &gt; **Other requests** panel and click the **Sent** tab and verify that your invitation has been sent to the client.
1. Instruct the client to accept the request. See [Client instructions](#client-instructions) below for content you can send them.

## Onboarding clients

1. Listen for an **account_update** webhook with the `event` property set to `PARTNER_ADDED` or `PARTNER_APP_INSTALLED`, or look for a developer notification or developer alert, indicating that the client has accepted your request.
1. If the client accepted your request, navigate to the **Settings** (gear icon) &gt; **Accounts** &gt; **WhatsApp accounts** panel and confirm that you see the client&#039;s WABA in the list of WABAs.
1. If the WABA doesn&#039;t have a business phone number, click the three-dot menu to the far right of the WABA&#039;s name, select **Add phone number**, and complete the flow. Alternatively, you can [add a phone number programmatically](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/registering-phone-numbers) using the API.
1. [Share your credit line](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/share-and-revoke-credit-lines#alternate-method-for-sharing-your-credit-line) with the client.

This completes the onboarding process. You can now use your system token to provide WhatsApp messaging services to the client.

## Client instructions

Once you have confirmed that the invitation has been sent, instruct the client to review and accept the request in the Meta Business Suite. Invitations that have not been accepted within 90 days will be canceled automatically.

You can send them the following instructions:

1. _Access Meta Business Suite at [https://business.facebook.com](https://business.facebook.com)._
1. _If you have multiple business portfolios, select the appropriate portfolio using the dropdown menu at the top-left of the page._
1. _Navigate to the **Settings** (gear icon) &gt; **Requests** &gt; **Other Requests** panel and click the **Received** tab._
1. _Locate the invitation and review its contents (or decline the invitation)._
1. _Add and verify a business phone number (optional)._
1. _Confirm the invitation._
1. _Navigate to the **Accounts** &gt; **WhatsApp account** panel and confirm that your WhatsApp Business Account has been created and shared with your Solution Partner._

## Adding phone numbers

Once the client has shared their WABA with you, you can register a business phone number for the client in one of two ways:

- **Via WhatsApp Manager**: Navigate to the [**WhatsApp Manager**](https://business.facebook.com/latest/whatsapp_manager/) &gt; **Overview** panel and locate the WABA in the WhatsApp account section. Click the three-dot menu to the far right of the WABA&#039;s name, click **Add phone number**, and complete the flow.
- **Via API**: See [Registering business phone numbers](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/registering-phone-numbers).

Alternatively, you can instruct the client to add a number on their own using the WhatsApp Manager.

### Canceling invitations

To cancel an invitation that has not been accepted yet, navigate to the **Settings** (gear icon) &gt; **Requests** panel and click the **Sent** tab. Locate the invitation and click its **Cancel** button.

### Payment methods

Clients cannot add their own payment method to a WABA created via the partner-initiated WABA creation process. You must use the API to [share your credit line](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/share-and-revoke-credit-lines#alternate-method-for-sharing-your-credit-line) with any client who accepts your creation request.

### Multi-Partner Solutions

If you are part of a [Multi-Partner Solution](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/multi-partner-solutions) (&quot;MPS&quot;), you can share a WABA created through the partner-initiated WABA creation process with other MPS participants after you have successfully [onboarded the WABA](#onboarding-clients).

To share the WABA with other MPS participants, you have two options:

- **Recommended**: direct the client to your (or your partner&#039;s) MPS-configured implementation of Embedded Signup, and instruct them to complete the flow using their existing WABA name, business portfolio, and business phone number.
- Use the API to [add the WABA to your MPS](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/support/adding-waba-to-mps).

## WABA Sharing model

With the WABA Sharing model, a client creates and grants access to their WABA to a partner using Embedded Signup.

When a client successfully completes a partner&#039;s Embedded Signup flow, a WABA is created under the client&#039;s business portfolio (and is thus owned by the client) and a webhook is triggered, notifying the partner. The partner can then use the contents of the webhook and the client&#039;s [business token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#business-integration-system-user-access-tokens) to onboard the client and provide message services via the API.

## Onboarding clients to the WABA Sharing model

To onboard a client to the WABA Sharing model, you must use Embedded Signup. See the [Embedded Signup](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/overview) documentation to learn how to implement Embedded Signup, and how to onboard clients as a Solution Partner.

## On-Behalf-Of model

**Note:** The On-Behalf-Of WABA ownership model has been deprecated and is no longer possible. See [On-Behalf-Of account ownership model deprecation](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/obo-model-deprecation) for details.

