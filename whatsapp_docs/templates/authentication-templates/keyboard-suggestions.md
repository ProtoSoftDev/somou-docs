# Keyboard suggestions


**iOS only:** Keyboard suggestions apply to iOS devices (iOS 26 and later). Android devices have their own keyboard suggestions mechanism and are not affected by these settings.

iOS 26 introduced native OTP autofill support for third-party messaging apps, including WhatsApp. When a WhatsApp user receives an authentication code on an iOS 26 device, iOS detects the OTP in the push notification and presents a one-tap autofill prompt in the keyboard, similar to how SMS OTP autofill works.

This feature works automatically with your existing authentication templates. You do not need to create new templates or update your integration.

## How it works

1. Your app sends an authentication template message containing an OTP to a WhatsApp user.
2. The WhatsApp user&#039;s iOS device receives a push notification containing the code.
3. iOS detects the numeric code in the notification and presents a one-tap autofill prompt in the keyboard.
4. The WhatsApp user taps the prompt to autofill the code into your app.

The copy code button remains available in the WhatsApp message as a fallback.

## Requirements

For Keyboard suggestions to work, the WhatsApp user&#039;s device must meet these requirements:

- The device must be running **iOS 26** or later.
- WhatsApp notifications must be enabled: **Settings** &gt; **Notifications** &gt; **WhatsApp** &gt; **Allow Notifications**.
- Autofill for security codes must be enabled: **Settings** &gt; **Passwords** &gt; **Password Options** &gt; **Autofill Security Codes**.

If any of these conditions are not met, the WhatsApp user will not see the autofill prompt. They can still use the copy code button in the WhatsApp message.

## Limitations

- iOS only detects **numeric codes of 3 to 8 digits**. Alphanumeric codes are not supported for autofill.
- Autofill does not trigger when the WhatsApp app is in the foreground, because iOS relies on the push notification to extract the code.
- This feature is only available on iOS 26 and later. WhatsApp users on earlier iOS versions are not affected.

## Opt out

Keyboard suggestions are enabled by default for all authentication templates. To disable it for a specific template, opt out at the template level:

1. Open the authentication template in the template editor.
2. Deselect the **Allow autofill on iOS** toggle.
3. Save the template.

This gives you granular control over autofill behavior. For example, you can disable autofill for high-security flows like financial transactions or account recovery while keeping it enabled for standard logins.

You can re-enable autofill at any time by selecting the **Allow autofill on iOS** toggle again.

The opt-out toggle is only available in WhatsApp Manager. There is no corresponding API parameter for this setting.

## Related

For Android OTP delivery, see [zero-tap authentication templates](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/authentication-templates/zero-tap-authentication-templates).
