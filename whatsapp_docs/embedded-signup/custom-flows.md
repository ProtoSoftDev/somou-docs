# Customizing the default flow



**Warning:** **Embedded signup v2 will be deprecated on October 15, 2026.** Migrate your integration to [v4](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/version-4) before that date to avoid disruption. See [Versions](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/versions) for the full upgrade path.

This document provides an overview of the ways you can customize Embedded Signup&#039;s default flow. You can use these options to present different versions of the flow to your business customers.

## Onboard WhatsApp Business app users

You can configure Embedded Signup to [allow business customers to onboard using their existing WhatsApp Business app account and phone number](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/onboarding-business-app-users):

After a business customer completes onboarding with this option, they can use your app to message WhatsApp users at scale. They can also continue to send messages on a one-to-one basis using the WhatsApp Business app.

## Pre-filling screens

You can pre-fill many of Embedded Signup&#039;s default flow screens with a business customer&#039;s business data. [Pre-filling screens](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/pre-filled-data) can reduce the amount of input and interaction your business customers need, and shorten the flow.

## Bypassing phone number addition and verification

You can customize Embedded Signup to entirely [skip the phone number addition and verification screens](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/bypass-phone-addition). Skipping these screens can be useful if you don&#039;t want business customers to have to enter a phone number, retrieve the verification code, and verify it.

## App-only install

By default, Embedded Signup returns tokens that your app can use to access assets owned by business customers onboarded through the flow. [App-only install](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/app-only-install) is a way to configure Embedded Signup so that only business tokens can access those assets. This does not affect the flow itself, only which tokens your app must use.
