# Calling API App Review Guidelines



## Overview

Reviewers reference [the permissions reference](https://developers.facebook.com/docs/permissions#w) as the official page. Use this guide as complementary to the permissions reference page, but treat the permissions reference page as the official source when in doubt.

This page provides details to improve your chances of a successful App Review specifically for WhatsApp Business Calling API features.

## Guidelines

### For the WhatsApp Business management permission

You should clearly show that your app can enable and disable Calling API features by displaying whether the Call Button icon is visible.

Share a video of you enabling and disabling the Call Button icon for the WhatsApp Business account either via cURL request, or via settings within your application UI.

Demonstrate this by enabling and disabling Calling API features, not simply toggling Call Button icon visibility.

- [Learn how to enable and disable Calling API features via API](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/call-settings#configure-update-business-phone-number-calling-settings)
- [Learn how to enable and disable Calling API features in WhatsApp Manager](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/call-settings#call-settings-in-whatsapp-manager)

#### Example

1. Display a chat thread between your business and a WhatsApp user that does not have the Call Button icon.
1. Use your app to enable Calling API features on the business phone number, which displays the Call Button icon.
1. Return to the same chat thread and display that the Call Button icon is visible.

### For the WhatsApp Business messaging permission

You should clearly demonstrate your application can support **either of the following use cases**:

#### Use case 1: Place a business-initiated call

Share a video that shows your application placing a business-initiated call. Then display a user accepting the call on a WhatsApp mobile client.

#### Use case 2: Receive a user-initiated call

Share a video that shows a user placing a call to your business phone number. Then show your application receiving the incoming call.

Show either:

1. The incoming call in the WhatsApp client application UI.
1. The calling webhook that the WhatsApp platform delivers to your app.
