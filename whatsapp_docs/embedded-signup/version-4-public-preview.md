# Version 4 Public Preview



**Warning:** **Embedded signup v2 will be deprecated on October 15, 2026.** Migrate your integration to [v4](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/version-4) before that date to avoid disruption. See [Versions](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/versions) for the full upgrade path.

Embedded Signup is being updated with a new Phone Number First flow. You can preview it by enabling v4-public-preview. In this flow, business customers enter and verify their phone number at the start of signup, before they create or select business assets. This surfaces phone number eligibility and verification issues earlier.

Functionality between v4-public-preview and v4 is identical. The key differences are the simplified UI and the Phone Number First flow.

## Enterprise number flow

### Authentication screen

This screen authenticates business customers using their Facebook or Meta Business Suite credentials.

### Authorization screen

This screen describes the data the business customer will be permitting your app to access.

### Phone number entry screen

This screen lets the business customer enter a new business phone number, or select an existing phone number from the dropdown, to associate with their WhatsApp Business Account. In v4-public-preview, the phone number is the first piece of information collected, before any asset selection.

### Phone number verification screen

This screen lets the business customer verify ownership of the phone number entered in the previous step. The customer can choose to receive the verification code via SMS or voice call from this screen, then submit the code to complete verification.

If you are providing phone numbers to your customers, you will have to deliver these codes, or provide pre-verified numbers instead.

### Business asset selection screen

This screen lets the business customer select existing business assets, such as a Meta business portfolio and WhatsApp Business Account, to use with the new phone number.

Customers can also create new assets if they have not reached their portfolio limit.

### New business creation screen

This screen lets the business customer create a new business portfolio or WhatsApp Business Account if no suitable existing asset is available.

### Permissions review screen

This screen provides a summary of the permissions the business customer is granting to your app.

### Success screen

This screen indicates that all of the business customer&#039;s assets (business portfolio, WhatsApp Business Account, phone number display profile, and business phone number) were successfully created and associated.

When the customer clicks Finish, a message event is triggered containing the customer&#039;s WhatsApp Business Account ID and business phone number ID, which you must use to onboard the customer to the platform.

## Coexistence flow

The Coexistence flow is automatically triggered when the business customer enters a phone number that is already in use with the WhatsApp Business app. Coexistence lets the business keep using the app on their phone while also enabling Cloud API access on the same number.

### Authentication screen

This screen authenticates business customers using their Facebook or Meta Business Suite credentials.

### Authorization screen

This screen describes the data the business customer will be permitting your app to access.

### Phone number entry screen

This screen lets the business customer enter the phone number they want to onboard. To trigger the Coexistence flow, the customer must enter a WhatsApp Business app phone number.

### Business profile screen

This screen displays the WhatsApp Business app account details associated with the entered phone number — the profile picture, name, phone number, and website that the business has set in the WhatsApp Business app. The business customer reviews these details and confirms this is the WhatsApp Business app account they want to onboard.

### Business portfolio selection screen

This screen lets the business customer select an existing Meta business portfolio to associate with the Coexistence onboarding, or create a new one.

### Business creation screen

This screen lets the business customer create a new business portfolio by entering business information: name, country, and website.

### QR code verification screen

This screen displays a QR code that the business customer scans from inside the WhatsApp Business app on their phone. To make scanning easier, a WhatsApp message containing a link to the QR code is also delivered to the same number. Once the customer scans the QR code, the flow advances automatically.

### Business information confirmation screen

This screen lets the business customer confirm their WhatsApp Business Account name (read-only) and select a time zone (required) for the account.

### Terms and conditions review screen

This screen presents the terms and conditions that the business customer must accept to complete Coexistence onboarding.

### Success screen

This screen indicates that the Coexistence onboarding completed successfully. When the customer clicks Finish, a message event is triggered containing the customer&#039;s WhatsApp Business Account ID and business phone number ID, which you must use to onboard the customer to the platform.

