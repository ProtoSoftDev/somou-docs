# One-tap autofill authentication templates


**Warning:** **Deprecation extension announcement:** We will extend the migration deadline until October 15, 2026. On this date, the `PendingIntent`-based handshake method for authentication templates will be deprecated. If you are currently using `PendingIntent` to initiate handshakes or verify app identity, the [OTP Android SDK](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/authentication-templates/zero-tap-authentication-templates#using-the-sdk) is the preferred way to migrate.

One-tap autofill authentication templates allow you to send a one-time password or code along with a one-tap autofill button to your users. When a WhatsApp user taps the autofill button, the WhatsApp client triggers an activity which opens your app and delivers it the password or code.

**Note:** **Android only:** One-tap autofill buttons use an Android handshake mechanism. Effective June 15, 2026, on iOS 26 and later, [Keyboard suggestions](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/authentication-templates/keyboard-suggestions) will provide native OTP autofill from the push notification with no integration required.

One-tap autofill button authentication templates consist of:

* Preset text:  _&lt;VERIFICATION_CODE&gt; is your verification code._
* An optional security disclaimer: _For your security, do not share this code._
* An optional expiration warning: _This code expires in &lt;NUM_MINUTES&gt; minutes._
* A one-tap autofill button.

**Note**: The OTP Android SDK features a simplified workflow for implementing one-tap and zero-tap authentication templates. You can learn how to use it below.

## Limitations

One-tap autofill buttons are only supported on Android. If you send an authentication template to a WhatsApp user who is using a non-Android device, the WhatsApp client will display a copy code button instead.

URLs, media, and emojis are not supported.

## Creating authentication templates

Use the [Message Templates API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/message-template-api#post-version-waba-id-message-templates) to create authentication templates.

### Request syntax

```json
curl &#039;https://graph.facebook.com/v25.0/&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;/message_templates&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039; \
-d
&#039;&#123;
  &quot;name&quot;: &quot;&lt;TEMPLATE_NAME&gt;&quot;,
  &quot;language&quot;: &quot;&lt;TEMPLATE_LANGUAGE&gt;&quot;,
  &quot;category&quot;: &quot;authentication&quot;,
  &quot;message_send_ttl_seconds&quot;: &lt;TIME_TO_LIVE&gt;, // Optional
  &quot;components&quot;: [
    &#123;
      &quot;type&quot;: &quot;body&quot;,
      &quot;add_security_recommendation&quot;: &lt;SECURITY_RECOMMENDATION&gt; // Optional
    &#125;,
    &#123;
      &quot;type&quot;: &quot;footer&quot;,
      &quot;code_expiration_minutes&quot;: &lt;CODE_EXPIRATION&gt; // Optional
    &#125;,
    &#123;
      &quot;type&quot;: &quot;buttons&quot;,
      &quot;buttons&quot;: [
        &#123;
          &quot;type&quot;: &quot;otp&quot;,
          &quot;otp_type&quot;: &quot;one_tap&quot;,
          &quot;text&quot;: &quot;&lt;COPY_CODE_BUTTON_TEXT&gt;&quot;,  // Optional
          &quot;autofill_text&quot;: &quot;&lt;AUTOFILL_BUTTON_TEXT&gt;&quot;, // Optional
          &quot;supported_apps&quot;: [
            &#123;
              &quot;package_name&quot;: &quot;&lt;PACKAGE_NAME&gt;&quot;,
              &quot;signature_hash&quot;: &quot;&lt;SIGNATURE_HASH&gt;&quot;
            &#125;
          ]
        &#125;
      ]
    &#125;
  ]
&#125;&#039;
```

Note that in your template creation request the button type is designated as `otp`, but upon creation the button type will be set to `url`. You can confirm this by performing a GET request on a newly created authentication template and analyzing its components.

### Request parameters

| Placeholder | Description | Example Value |
| --- | --- | --- |
| `&lt;AUTOFILL_BUTTON_TEXT&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;One-tap autofill button label text.&lt;br&gt;&lt;br&gt;If omitted, the autofill text will default to a pre-set value, localized to the template&#039;s language. For example, `Autofill` for English (US).&lt;br&gt;&lt;br&gt;Maximum 25 characters. | `Autofill` |
| `&lt;CODE_EXPIRATION&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | **Optional.**&lt;br&gt;&lt;br&gt;Indicates the number of minutes the password or code is valid.&lt;br&gt;&lt;br&gt;If included, the code expiration warning and this value will be displayed in the delivered message. The button will be disabled in the delivered message the indicated number of minutes from when the message was sent.&lt;br&gt;&lt;br&gt;If omitted, the code expiration warning will not be displayed in the delivered message. In addition, the button will be disabled 10 minutes from when the message was sent.&lt;br&gt;&lt;br&gt;Minimum 1, maximum 90. | `5` |
| `&lt;COPY_CODE_BUTTON_TEXT&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Copy code button label text.&lt;br&gt;&lt;br&gt;If omitted, the text will default to a pre-set value localized to the template&#039;s language. For example, `Copy Code` for English (US).&lt;br&gt;&lt;br&gt;If included, the authentication template message will display a copy code button with this text if the message fails the [eligibility check](#eligibility-check).&lt;br&gt;&lt;br&gt;Maximum 25 characters. | `Copy Code` |
| `&lt;PACKAGE_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Your Android app&#039;s package name.&lt;br&gt;&lt;br&gt;The string must have at least two segments (one or more dots), and each segment must start with a letter.&lt;br&gt;&lt;br&gt;All characters must be alphanumeric or an underscore [`a-zA-Z0-9_`].&lt;br&gt;&lt;br&gt;If using Graph API version 20.0 or older, you can define your app&#039;s package name outside of the `supported_apps` array, but this is not recommended. See [Supported Apps](#supported-apps) below.&lt;br&gt;&lt;br&gt;Maximum 224 characters. | `com.example.luckyshrub` |
| `&lt;SECURITY_RECOMMENDATION&gt;`&lt;br&gt;&lt;br&gt;_Boolean_ | **Optional.**&lt;br&gt;&lt;br&gt;Set to `true` if you want the template to include the string, _For your security, do not share this code._ Set to `false` to exclude the string. | `true` |
| `&lt;SIGNATURE_HASH&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Your app signing key hash. See [App Signing Key Hash](#app-signing-key-hash) below.&lt;br&gt;&lt;br&gt;All characters must be either alphanumeric, `+`, `/`, or `=` (`a-zA-Z0-9+/=`).&lt;br&gt;&lt;br&gt;If using Graph API version 20.0 or older, you can define your app&#039;s signature hash outside of the `supported_apps` array, but this is not recommended. See [Supported Apps](#supported-apps) below.&lt;br&gt;&lt;br&gt;Must be exactly 11 characters. | `K8a/AINcGX7` |
| `&lt;TEMPLATE_LANGUAGE&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Template [language and locale code](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/supported-languages). | `en_US` |
| `&lt;TEMPLATE_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Template name.&lt;br&gt;&lt;br&gt;Maximum 512 characters. | `verification_code` |
| `&lt;TIME_TO_LIVE&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | **Optional.**&lt;br&gt;&lt;br&gt;Authentication message time-to-live value, in seconds. See [Customizing Time-To-Live](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/overview#customizing-time-to-live). | `60` |

### Example request

This example creates a template named &quot;authentication_code_autofill_button&quot; categorized as `authentication` with all optional text strings enabled and a one-tap autofill button.

```json
curl &#039;https://graph.facebook.com/v25.0/102290129340398/message_templates&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039; \
-d &#039;
&#123;
  &quot;name&quot;: &quot;authentication_code_autofill_button&quot;,
  &quot;language&quot;: &quot;en_US&quot;,
  &quot;category&quot;: &quot;authentication&quot;,
  &quot;message_send_ttl_seconds&quot;: 60,
  &quot;components&quot;: [
    &#123;
      &quot;type&quot;: &quot;body&quot;,
      &quot;add_security_recommendation&quot;: true
    &#125;,
    &#123;
      &quot;type&quot;: &quot;footer&quot;,
      &quot;code_expiration_minutes&quot;: 10
    &#125;,
    &#123;
      &quot;type&quot;: &quot;buttons&quot;,
      &quot;buttons&quot;: [
        &#123;
          &quot;type&quot;: &quot;otp&quot;,
          &quot;otp_type&quot;: &quot;one_tap&quot;,
          &quot;text&quot;: &quot;Copy Code&quot;,
          &quot;autofill_text&quot;: &quot;Autofill&quot;,
          &quot;package_name&quot;: &quot;com.example.luckyshrub&quot;,
          &quot;signature_hash&quot;: &quot;K8a/AINcGX7&quot;
        &#125;
      ]
    &#125;
  ]
&#125;&#039;
```

### Example response

```json
&#123;
  &quot;id&quot;: &quot;594425479261596&quot;,
  &quot;status&quot;: &quot;PENDING&quot;,
  &quot;category&quot;: &quot;AUTHENTICATION&quot;
&#125;
```

## Webhooks

The [button messages webhook](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/messages/button) is triggered whenever a user taps the &quot;I didn&#039;t request a code&quot; button within the message.

### Example webhook

```html
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;320580347795883&quot;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;messaging_product&quot;: &quot;whatsapp&quot;,
            &quot;metadata&quot;: &#123;
              &quot;display_phone_number&quot;: &quot;12345678&quot;,
              &quot;phone_number_id&quot;: &quot;1234567890&quot;
            &#125;,
            &quot;contacts&quot;: [
              &#123;
                &quot;profile&quot;: &#123;
                  &quot;name&quot;: &quot;John&quot;
                &#125;,
                &quot;wa_id&quot;: &quot;12345678&quot;
              &#125;
            ],
            &quot;messages&quot;: [
              &#123;
                &quot;context&quot;: &#123;
                  &quot;from&quot;: &quot;12345678&quot;,
                  &quot;id&quot;: &quot;wamid.HBgLMTIxMTU1NTE0NTYVAgARGBJDMDEyMTFDNTE5NkFCOUU3QTEA&quot;
                &#125;,
                &quot;from&quot;: &quot;12345678&quot;,
                &quot;id&quot;: &quot;wamid.HBgLMTIxMTU1NTE0NTYVAgASGCBBQ0I3MjdCNUUzMTE0QjhFQkM4RkQ4MEU3QkE0MUNEMgA=&quot;,
                &quot;timestamp&quot;: &quot;1753919111&quot;,
                &quot;from_logical_id&quot;: &quot;131063108133020&quot;,
                &quot;type&quot;: &quot;button&quot;,
                &quot;button&quot;: &#123;
                  &quot;payload&quot;: &quot;DID_NOT_REQUEST_CODE&quot;,
                  &quot;text&quot;: &quot;I didn&#039;t request a code&quot;
                &#125;
              &#125;
            ]
          &#125;,
          &quot;field&quot;: &quot;messages&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```


## App signing key hash

You must include your app signing key hash in your post body.

To calculate your hash, follow Google&#039;s instructions for [computing your app&#039;s hash string](https://developers.google.com/identity/sms-retriever/verify#computing_your_apps_hash_string).

Alternatively, if you follow Google&#039;s instructions and download your app signing key certificate (step 1), you can use your certificate with the [sms_retriever_hash_v9.sh](http://tinyurl.com/43bkdrdt) shell script to compute the hash. For example:

```sh
./sms_retriever_hash_v9.sh --package &quot;com.example.myapplication&quot; --keystore ~/.android/debug.keystore
```


## Supported apps

The `supported_apps` array allows you define pairs of app package names and signing key hashes for up to 5 apps. This can be useful if you have different app builds and want each of them to be able to initiate the handshake:

```json
&quot;buttons&quot;: [
  &#123;
    &quot;type&quot;: &quot;otp&quot;,
    ...
    &quot;supported_apps&quot;: [
      &#123;
        &quot;package_name&quot;: &quot;&lt;PACKAGE_NAME_1&gt;&quot;,
        &quot;signature_hash&quot;: &quot;&lt;SIGNATURE_HASH_1&gt;&quot;
      &#125;,
      &#123;
        &quot;package_name&quot;: &quot;&lt;PACKAGE_NAME_2&gt;&quot;,
        &quot;signature_hash&quot;: &quot;&lt;SIGNATURE_HASH_2&gt;&quot;
      &#125;,
      ...
    ]
  &#125;
]
```

Alternatively, if you are using Graph API version 20.0 or older and have only a single app, you can define the app&#039;s package name and signing key hash as `buttons` object properties, but this is not recommended as we will stop supporting this method starting with version 21.0:

```json
&quot;buttons&quot;: [
  &#123;
    &quot;type&quot;: &quot;otp&quot;,
    ...
    &quot;package_name&quot;: &quot;&lt;PACKAGE_NAME&gt;&quot;,
    &quot;signature_hash&quot;: &quot;&lt;SIGNATURE_HASH&gt;&quot;
  &#125;
]
```


## Handshake

You must signal to the WhatsApp client to expect imminent delivery of a password or code. You can do this by initiating a &quot;handshake&quot;.

A handshake is an Android intent and public class that you implement but that the WhatsApp client can start.

When a user in your app requests a one-time password or verification code and chooses for it to be delivered to their WhatsApp number, first perform the handshake, then call our API to send the authentication template message. When the WhatsApp client receives the message, it will perform an eligibility check, and if there are no errors, start the intent and display the message to the user. Finally, when the user taps the message&#039;s one-tap autofill button, we automatically load your app and pass it the password or code.

If you do not perform a handshake before sending the message, or the message fails an eligibility check, the delivered message will display a copy code button instead of a one-tap button.

### Eligibility check

The WhatsApp client performs the following checks when it receives an authentication template message. If any check fails, the one-tap autofill button will be replaced with a copy code button.

* The handshake was initiated no more than 10 minutes ago (or no more than the number of minutes indicated by the template&#039;s `code_expiration_minutes` property, if present).
* The package name in the message (defined in the `package_name` property in the `components` array upon template creation) matches the package name set on the intent. The match is determined through the `getCreatorPackage` method called in the `PendingIntent` object provided by your application.
* None of the other apps that you included in the template&#039;s list of `supported_apps` initiated a handshake in the last 10 minutes (or the number of minutes indicated by the template&#039;s `code_expiration_minutes` property, if present).
* The app signing key hash in the message (defined in the `signature_hash` property in the components array upon template creation) matches your installed app&#039;s signing key hash.
* The message includes the one-tap autofill button text.
* Your app has defined an activity to receive the password or code.

### Android notifications

Android notifications indicating receipt of a WhatsApp authentication template message will only appear on the user&#039;s Android device if:

* The user is logged into the WhatsApp app or WhatsApp Business app with the phone number (account) that the message was sent to.
* The user is logged into your app.
* Android OS is KitKat (4.4, API 19) or above.
* **Show notifications** is enabled (**Settings** &gt; **Notifications**) in the WhatsApp app or WhatsApp Business app.
* Device level notification is enabled for the WhatsApp app or WhatsApp Business app.
* Prior message threads in the WhatsApp app or WhatsApp Business app between the user and your business are not muted.

### Using the SDK

The OTP Android SDK can be used to perform handshakes, as well as other functions in both one-tap and zero-tap authentication templates.

To access SDK functionality, add the following configuration to your Gradle file:

```java
dependencies &#123;
…
    implementation &#039;com.whatsapp.otp:whatsapp-otp-android-sdk:1.0.0&#039;
…
&#125;
```

To your repositories, add `mavenCentral()`:

```java
repositories &#123;
…
    mavenCentral()
…
&#125;
```

### Activity

Declare an activity and intent filter that can receive the one-time password or code. The intent filter must have the action name `com.whatsapp.otp.OTP_RETRIEVED`.

```java
&lt;activity
   android:name=&quot;.ReceiveCodeActivity&quot;
   android:enabled=&quot;true&quot;
   android:exported=&quot;true&quot;
   android:launchMode=&quot;standard&quot;&gt;
   &lt;intent-filter&gt;
       &lt;action android:name=&quot;com.whatsapp.otp.OTP_RETRIEVED&quot; /&gt;
   &lt;/intent-filter&gt;
&lt;/activity&gt;
```

This is the activity that the WhatsApp app or WhatsApp Business app will start once the authentication template message is received and it passes all [eligibility checks](#eligibility-check).

### Activity class

#### Using the SDK (preferred)
Define the activity public class and instantiate a `WhatsAppOtpIncomingIntentHandler` object to handle the intent. The `.processOtpCode()` method validates the handshake ID against the expected value you stored during handshake initiation and handles errors.

```java
public class ReceiveCodeActivity extends AppCompatActivity &#123;

      &#064;Override
      protected void onCreate(Bundle savedInstanceState) &#123;
          super.onCreate(savedInstanceState);
          WhatsAppOtpIncomingIntentHandler incomingIntentHandler = new WhatsAppOtpIncomingIntentHandler();

          // Retrieve the expected handshake ID that was stored during handshake initiation
          String expectedHandshakeId = retrieveStoredHandshakeId();

          incomingIntentHandler.processOtpCode(
                                 getIntent(),
                                 expectedHandshakeId,
                                 (code) -&gt; &#123;
                                   // The handshake ID has been validated by the SDK
                                   validateCode(code);
                                 &#125;,
                                 // call your function to handle errors
                                 (error, exception) -&gt; handleError(error, exception));
    &#125;
```

#### Without the SDK

Define the activity public class that can accept the code once it has been passed to your app. The activity should validate the `request_id` (handshake ID) to ensure the OTP code is coming from a legitimate handshake initiated by your app.

```java
public class ReceiveCodeActivity extends AppCompatActivity &#123;

   &#064;Override
   protected void onCreate(Bundle savedInstanceState) &#123;
       super.onCreate(savedInstanceState);
       Intent intent = getIntent();

       // Extract the handshake ID from the intent
       String incomingRequestId = intent.getStringExtra(&quot;request_id&quot;);

       // Retrieve the previously stored handshake ID
       String storedRequestId = retrieveStoredRequestId();

       // Validate the handshake ID matches
       if (storedRequestId != null &amp;&amp; storedRequestId.equals(incomingRequestId)) &#123;
         // use OTP code
         String otpCode = intent.getStringExtra(&quot;code&quot;);
         // ...
       &#125;
   &#125;
&#125;
```

### Initiating the handshake

#### Using the SDK (preferred)

Performing a handshake can be done by instantiating the `WhatsAppOtpHandler` object and passing in your context to the `.sendOtpIntentToWhatsApp()` method. The method returns a UUID (handshake ID) that must be stored and used to validate the incoming OTP code later:

```java
WhatsAppOtpHandler whatsAppOtpHandler = new WhatsAppOtpHandler();
UUID handshakeId = whatsAppOtpHandler.sendOtpIntentToWhatsApp(context);
// Store handshakeId to validate the received OTP code later
```

#### Without the SDK

This example demonstrates one way to initiate a handshake with the WhatsApp client. The handshake includes a `request_id` (UUID) that must be stored and validated when receiving the OTP code.

```java
private String currentRequestId;

public void sendOtpIntentToWhatsApp() &#123;
   // Generate a unique handshake ID
   currentRequestId = UUID.randomUUID().toString();
   // Store this ID for later validation when receiving the OTP
   storeRequestId(currentRequestId);

   // Send OTP_REQUESTED intent to both WA and WA Business App
   sendOtpIntentToWhatsApp(&quot;com.whatsapp&quot;, currentRequestId);
   sendOtpIntentToWhatsApp(&quot;com.whatsapp.w4b&quot;, currentRequestId);
&#125;

private void sendOtpIntentToWhatsApp(String packageName, String requestId) &#123;

  /**
  * Starting with Build.VERSION_CODES.S, it will be required to explicitly
  * specify the mutability of  PendingIntents on creation with either
  * (&#064;link #FLAG_IMMUTABLE&#125; or FLAG_MUTABLE
  */
  int flags = Build.VERSION.SDK_INT &gt;= Build.VERSION_CODES.S ? FLAG_IMMUTABLE : 0;
  PendingIntent pi = PendingIntent.getActivity(
      getApplicationContext(),
      0,
      new Intent(),
      flags);

  // Send OTP_REQUESTED intent to WhatsApp
  Intent intentToWhatsApp = new Intent();
  intentToWhatsApp.setPackage(packageName);
  intentToWhatsApp.setAction(&quot;com.whatsapp.otp.OTP_REQUESTED&quot;);
  // WA will use this to verify the identity of the caller app.
  Bundle extras = intentToWhatsApp.getExtras();
  if (extras == null) &#123;
     extras = new Bundle();
  &#125;
  extras.putParcelable(&quot;_ci_&quot;, pi);
  // Add the handshake ID for secure validation
  intentToWhatsApp.putExtra(&quot;request_id&quot;, requestId);
  intentToWhatsApp.putExtras(extras);
  getApplicationContext().sendBroadcast(intentToWhatsApp);
&#125;
```

### Checking if WhatsApp is installed on Android

You can check WhatsApp installation before offering WhatsApp as an option if you expect both WhatsApp and your app to be on the same device.

First, you need to add the following to your `AndroidManifest.xml` file:

```xml
&lt;queries&gt;
    &lt;package android:name=&quot;com.whatsapp&quot;/&gt;
    &lt;package android:name=&quot;com.whatsapp.w4b&quot;/&gt;
&lt;/queries&gt;
```

#### Using the SDK (preferred)

Instantiate the `WhatsAppOtpHandler` object:

```java
WhatsAppOtpHandler whatsAppOtpHandler = new WhatsAppOtpHandler();
```

Check if the WhatsApp client is installed by passing the `isWhatsAppInstalled` method as the clause in an `If` statement:

```java
If (whatsAppOtpHandler.isWhatsAppInstalled(context)) &#123;
    // ... do something
&#125;
```

#### Without the SDK

```java
if (this.isWhatsAppInstalled(context)) &#123;
    // ... do something
&#125;

public boolean isWhatsAppInstalled(final &#064;NonNull Context context)&#123;
    return isWhatsAppInstalled(context, &quot;com.whatsapp&quot;) ||
           isWhatsAppInstalled(context, &quot;com.whatsapp.w4b&quot;);
  &#125;

  public boolean isWhatsAppInstalled(final &#064;NonNull Context context,
      final &#064;NonNull String type)&#123;
    final Intent intent = new Intent();
    intent.setPackage(type);
    intent.setAction(&quot;com.whatsapp.otp.OTP_REQUESTED&quot;);
    PackageManager packageManager = context.getPackageManager();
    List&lt;ResolveInfo&gt; receivers = packageManager.queryBroadcastReceivers(intent, 0);
    return !receivers.isEmpty();
  &#125;
&#125;
```

### Checking if WhatsApp is installed on iOS

Use the following code in your iOS application to check if WhatsApp is installed.

```swift
let schemeURL = URL(string: &quot;whatsapp://otp&quot;)!
let isWhatsAppInstalled = UIApplication.shared.canOpenURL(schemeURL)
```

### Error signals

See [Error Signals](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/authentication-templates/error-signals) that can help with debugging.

### Handshake ID error codes

The following error codes may be returned when using the SDK with handshake ID validation:

| Error Code | Description |
|------------|-------------|
| `HANDSHAKE_ID_MISSING` | The handshake ID was not included in the intent from WhatsApp |
| `HANDSHAKE_ID_INVALID_FORMAT` | The handshake ID is not a valid UUID format |
| `HANDSHAKE_ID_MISMATCH` | The handshake ID in the intent does not match the expected value |

### Sample app

See our [WhatsApp One-Time Password (OTP) Sample App](https://github.com/WhatsApp/WhatsApp-OTP-Sample-App) for Android on GitHub. The sample app demonstrates how to send and receive OTP passwords and codes via the API, how to integrate the one-tap autofill and copy code buttons, how to create a template, and how to spin up a sample server.

## Sending authentication template

This document explains how to send approved [authentication templates with one-time password buttons](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/authentication-templates/authentication-templates).

Note that **you must first initiate a handshake** between your app and the WhatsApp client. See [Handshake](#handshake) above.

### Request

Use the [Messages API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/message-api#post-version-phone-number-id-messages) to send an [authentication template message with a one-time password button](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/authentication-templates/authentication-templates).

### Request syntax

```json
curl -X POST &quot;https://graph.facebook.com/v23.0/&lt;WHATSAPP_BUSINESS_PHONE_NUMBER_ID&gt;/messages&quot; \
  -H &quot;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&quot; \
  -H &quot;Content-Type: application/json&quot; \
  -d &#039;&#123;
    &quot;messaging_product&quot;: &quot;whatsapp&quot;,
    &quot;recipient_type&quot;: &quot;individual&quot;,
    &quot;to&quot;: &quot;&lt;CUSTOMER_PHONE_NUMBER&gt;&quot;,
    &quot;type&quot;: &quot;template&quot;,
    &quot;template&quot;: &#123;
      &quot;name&quot;: &quot;&lt;TEMPLATE_NAME&gt;&quot;,
      &quot;language&quot;: &#123;
        &quot;code&quot;: &quot;&lt;TEMPLATE_LANGUAGE_CODE&gt;&quot;
      &#125;,
      &quot;components&quot;: [
        &#123;
          &quot;type&quot;: &quot;body&quot;,
          &quot;parameters&quot;: [
            &#123;
              &quot;type&quot;: &quot;text&quot;,
              &quot;text&quot;: &quot;&lt;ONE-TIME PASSWORD&gt;&quot;
            &#125;
          ]
        &#125;,
        &#123;
          &quot;type&quot;: &quot;button&quot;,
          &quot;sub_type&quot;: &quot;url&quot;,
          &quot;index&quot;: 0,
          &quot;parameters&quot;: [
            &#123;
              &quot;type&quot;: &quot;text&quot;,
              &quot;text&quot;: &quot;&lt;ONE-TIME PASSWORD&gt;&quot;
            &#125;
          ]
        &#125;
      ]
    &#125;
  &#125;&#039;
```

### Request parameters

| Placeholder | Description | Sample Value |
| --- | --- | --- |
| `&lt;CUSTOMER_PHONE_NUMBER&gt;` | The customer&#039;s WhatsApp phone number. | `12015553931` |
| `&lt;ONE-TIME PASSWORD&gt;` | The one-time password or verification code to be delivered to the customer.&lt;br&gt;&lt;br&gt;Note that this value must appear twice in the payload.&lt;br&gt;&lt;br&gt;Maximum 15 characters. | `J$FpnYnP` |
| `&lt;TEMPLATE_LANGUAGE_CODE&gt;` | The template&#039;s [language and locale code](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/supported-languages). | `en_US` |
| `&lt;TEMPLATE_NAME&gt;` | The template&#039;s name. | `verification_code` |

### Response

Upon success, the API will respond with:

```json
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;contacts&quot;: [
    &#123;
      &quot;input&quot;: &quot;&lt;INPUT&gt;&quot;,
      &quot;wa_id&quot;: &quot;&lt;WA_ID&gt;&quot;
    &#125;
  ],
  &quot;messages&quot;: [
    &#123;
      &quot;id&quot;: &quot;&lt;ID&gt;&quot;
    &#125;
  ]
&#125;
```

### Response parameters

| Placeholder | Description | Sample Value |
| --- | --- | --- |
| `&lt;INPUT&gt;`&lt;br&gt;&lt;br&gt;_String_ | The customer phone number that the message was sent to. This may not match `wa_id`. | `+16315551234` |
| `&lt;WA_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | WhatsApp ID of the customer who the message was sent to. This may not match `input`. | `+16315551234` |
| `&lt;ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | WhatsApp message ID. You can use the ID listed after &quot;wamid.&quot; to track your message status. | `wamid.HBgLMTY1MDM4Nzk0MzkVAgARGBI3N0EyQUJDMjFEQzZCQUMzODMA` |

### Example request

```curl
curl -L &#039;https://graph.facebook.com/v25.0/105954558954427/messages&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039; \
-d &#039;&#123;
      &quot;messaging_product&quot;: &quot;whatsapp&quot;,
      &quot;recipient_type&quot;: &quot;individual&quot;,
      &quot;to&quot;: &quot;12015553931&quot;,
      &quot;type&quot;: &quot;template&quot;,
      &quot;template&quot;: &#123;
        &quot;name&quot;: &quot;verification_code&quot;,
        &quot;language&quot;: &#123;
          &quot;code&quot;: &quot;en_US&quot;
      &#125;,
      &quot;components&quot;: [
        &#123;
          &quot;type&quot;: &quot;body&quot;,
          &quot;parameters&quot;: [
            &#123;
              &quot;type&quot;: &quot;text&quot;,
              &quot;text&quot;: &quot;J$FpnYnP&quot;
            &#125;
          ]
        &#125;,
        &#123;
          &quot;type&quot;: &quot;button&quot;,
          &quot;sub_type&quot;: &quot;url&quot;,
          &quot;index&quot;: &quot;0&quot;,
          &quot;parameters&quot;: [
            &#123;
              &quot;type&quot;: &quot;text&quot;,
              &quot;text&quot;: &quot;J$FpnYnP&quot;
            &#125;
          ]
        &#125;
      ]
    &#125;
  &#125;&#039;
```

### Example response

```json
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;contacts&quot;: [
    &#123;
      &quot;input&quot;: &quot;12015553931&quot;,
      &quot;wa_id&quot;: &quot;12015553931&quot;
    &#125;
  ],
  &quot;messages&quot;: [
    &#123;
      &quot;id&quot;: &quot;wamid.HBgLMTY1MDM4Nzk0MzkVAgARGBI4Qzc5QkNGNTc5NTMyMDU5QzEA&quot;
    &#125;
  ]
&#125;
```
