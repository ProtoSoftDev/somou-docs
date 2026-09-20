# Embedded Signup default flow



**Warning:** **Embedded signup v2 will be deprecated on October 15, 2026.** Migrate your integration to [v4](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/version-4) before that date to avoid disruption. See [Versions](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/versions) for the full upgrade path.

This document describes the default screens that the Embedded Signup Cloud API flow presents to your business customers as they navigate the flow. If you inject [pre-filled data](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/pre-filled-data), you can pre-fill some of these screens. You can also bypass many of them entirely. Pre-filling data reduces the likelihood of errors and makes it much easier for your business customers to onboard onto the platform. This document describes the UI flow for the latest version, v4.

## Screens

### Authentication screen

This screen authenticates business customers using their Facebook or Meta Business Suite credentials.

### Authorization screen

This screen describes the data the business customer permits your app to access.

### Business asset selection screen

This screen gives business customers the option to select existing business assets such as a Meta business portfolio and WhatsApp Business account.

Business customers also have the option to create new assets if they have not reached their portfolio limit.

### Business asset creation screen

This screen gives business customers the option to select existing business assets such as a Meta business portfolio and WhatsApp Business account.

Business customers also have the option to create new assets if they have not reached their portfolio limit.

### Phone number addition screen

This screen allows the business customer to enter a new business phone number to associate with their WhatsApp Business account.

The phone number addition screen also allows the business customer to choose how they wish to receive their verification code, which they will need to provide on the phone number verification screen.

If you are providing phone numbers to your business customers, you will have to deliver these codes to them, or provide pre-verified numbers instead.

### Phone number verification screen

This screen allows the business customer to verify ownership of the business phone number they entered on the phone number addition screen.

### Permissions review screen

This screen provides a summary of the permissions the business customer grants to your app.

### Success screen

This screen indicates that Meta successfully created and associated all of the business customer&#039;s assets (business portfolio, WhatsApp Business account (WABA), phone number display profile, and business phone number).

When the business customer clicks **Finish**, Embedded Signup triggers a message event containing the business customer&#039;s WABA ID and business phone number ID, which you must then use to onboard the business customer to the platform.

