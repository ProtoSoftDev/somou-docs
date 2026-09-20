# Zero-tap authentication templates


**Warning:** **Deprecation extension announcement:** We will extend the migration deadline until October 15, 2026. On this date, the `PendingIntent`-based handshake method for authentication templates will be deprecated. If you are currently using `PendingIntent` to initiate handshakes or verify app identity, the [OTP Android SDK](#using-the-sdk) is the preferred way to migrate.

Zero-tap authentication templates allow your users to receive one-time passwords or codes via WhatsApp without having to leave your app.

When a user in your app requests a password or code and you deliver it using a zero-tap authentication template, the WhatsApp client simply broadcasts the included password or code and your app can capture it immediately with a broadcast receiver.

From your user&#039;s perspective, they request a password or code in your app and it appears in your app automatically. If your app user happens to check the message in the WhatsApp client, they will only see a message displaying the default fixed text: _&lt; code &gt; is your verification code._

Like one-tap autofill button authentication templates, when the WhatsApp client receives the template message containing the user&#039;s password or code, we perform a series of eligibility checks. If the message fails this check and we are unable to broadcast the password or code, the message will display either a one-tap autofill button or a copy code button. For this reason, when you create a zero-tap authentication template, you must include a one-tap autofill and copy code button in your post body payload, even if the user may never see one of these buttons.

Note: The OTP Android SDK features a simplified workflow for implementing one-tap and zero-tap authentication templates. You can learn how to use it below.

## Limitations

Zero-tap is only supported on Android. If you send a zero-tap authentication template to a WhatsApp user who is using a non-Android device, the WhatsApp client will display a copy code button instead. Effective June 15, 2026, on iOS 26 and later, the push notification will also trigger [Keyboard suggestions](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/authentication-templates/keyboard-suggestions), giving the WhatsApp user a one-tap autofill prompt in the keyboard.

URLs, media, and emojis are not supported.

## Best practices

* Do not make WhatsApp your default password/code delivery method.
* Make it clear to your app users that the password or code will be automatically delivered to your app when they select WhatsApp for delivery.
* Link to our [About security codes that automatically fill on WhatsApp](https://faq.whatsapp.com/659113242716268/) help center article if your users are worried about auto-delivery of the password or code.
* After the password/code is used in your app, make it clear to your app user that it was received successfully.

Here are some examples that make it clear to an app user that their code will automatically appear in the app.

## Create a zero-tap authentication template

Use the [Message Templates API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/message-template-api#post-version-waba-id-message-templates) to create a zero-tap authentication template.

### Request syntax

```json
curl -X POST &quot;https://graph.facebook.com/v19.0/&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;/message_templates&quot; \
  -H &quot;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&quot; \
  -H &quot;Content-Type: application/json&quot; \
  -d &#039;
  &#123;
    &quot;name&quot;: &quot;&lt;TEMPLATE_NAME&gt;&quot;,
    &quot;language&quot;: &quot;&lt;TEMPLATE_LANGUAGE&gt;&quot;,
    &quot;category&quot;: &quot;authentication&quot;,
    &quot;message_send_ttl_seconds&quot;: &lt;TIME_TO_LIVE&gt;,
    &quot;components&quot;: [
      &#123;
        &quot;type&quot;: &quot;body&quot;,
        &quot;add_security_recommendation&quot;: &lt;SECURITY_RECOMMENDATION&gt;
      &#125;,
      &#123;
        &quot;type&quot;: &quot;footer&quot;,
        &quot;code_expiration_minutes&quot;: &lt;CODE_EXPIRATION&gt;
      &#125;,
      &#123;
        &quot;type&quot;: &quot;buttons&quot;,
        &quot;buttons&quot;: [
          &#123;
            &quot;type&quot;: &quot;otp&quot;,
            &quot;otp_type&quot;: &quot;zero_tap&quot;,
            &quot;text&quot;: &quot;&lt;COPY_CODE_BUTTON_TEXT&gt;&quot;,
            &quot;autofill_text&quot;: &quot;&lt;AUTOFILL_BUTTON_TEXT&gt;&quot;,
            &quot;zero_tap_terms_accepted&quot;: &lt;TERMS_ACCEPTED&gt;,
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
| `&lt;AUTOFILL_BUTTON_TEXT&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Zero-tap autofill button label text.&lt;br&gt;&lt;br&gt;If omitted, the autofill text will default to a pre-set value, localized to the template&#039;s language. For example, &quot;Autofill&quot; for English (US).&lt;br&gt;&lt;br&gt;Maximum 25 characters. | `Autofill` |
| `&lt;COPY_CODE_BUTTON_TEXT&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Copy code button label text.&lt;br&gt;&lt;br&gt;If the message fails the [eligibility check](#eligibility-check) and displays a copy code button, the button will use this text label.&lt;br&gt;&lt;br&gt;If omitted, and the message fails the eligibility check and displays a copy code button, the text will default to a pre-set value localized to the template&#039;s language. For example, `Copy Code` for English (US).&lt;br&gt;&lt;br&gt;Maximum 25 characters. | `Copy Code` |
| `&lt;CODE_EXPIRATION&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | **Optional.**&lt;br&gt;&lt;br&gt;Indicates the number of minutes the password or code is valid.&lt;br&gt;&lt;br&gt;If included, the code expiration warning and this value will be displayed in the delivered message. If the message fails the [eligibility check](#eligibility-check) and displays a one-tap autofill button, the button will be disabled in the delivered message the indicated number of minutes from when the message was sent.&lt;br&gt;&lt;br&gt;If omitted, the code expiration warning will not be displayed in the delivered message. If the message fails the eligibility check and displays a one-tap autofill button, the button will be disabled 10 minutes from when the message was sent.&lt;br&gt;&lt;br&gt;Minimum 1, maximum 90. | `5` |
| `&lt;PACKAGE_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Your Android app&#039;s package name.&lt;br&gt;&lt;br&gt;The string must have at least two segments (one or more dots), and each segment must start with a letter.&lt;br&gt;&lt;br&gt;All characters must be alphanumeric or an underscore (`a-zA-Z0-9_`).&lt;br&gt;&lt;br&gt;If using Graph API version 20.0 or older, you can define your app&#039;s package name outside of the `supported_apps` array, but this is not recommended. See [Supported Apps](#supported-apps) below.&lt;br&gt;&lt;br&gt;Maximum 224 characters. | `com.example.luckyshrub` |
| `&lt;SECURITY_RECOMMENDATION&gt;`&lt;br&gt;&lt;br&gt;_Boolean_ | **Optional.**&lt;br&gt;&lt;br&gt;Set to `true` if you want the template to include the fixed string, For your security, do not share this code. Set to `false` to exclude the string. | `true` |
| `&lt;SIGNATURE_HASH&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Your app signing key hash. See [App Signing Key Hash](#app-signing-key-hash) below.&lt;br&gt;&lt;br&gt;All characters must be either alphanumeric, `+`, `/`, or `=` (`a-zA-Z0-9+/=`).&lt;br&gt;&lt;br&gt;If using Graph API version 20.0 or older, you can define your app&#039;s signature hash outside of the `supported_apps` array, but this is not recommended. See [Supported Apps](#supported-apps) below.&lt;br&gt;&lt;br&gt;Must be exactly 11 characters. | `K8a/AINcGX7` |
| `&lt;TEMPLATE_LANGUAGE&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Template [language and locale code](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/supported-languages). | `en_US` |
| `&lt;TEMPLATE_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Template name.&lt;br&gt;&lt;br&gt;Maximum 512 characters. | `zero_tap_auth_template` |
| `&lt;TERMS_ACCEPTED&gt;`&lt;br&gt;&lt;br&gt;_Boolean_ | **Required.**&lt;br&gt;&lt;br&gt;Set to `true` to indicate that you understand that your use of zero-tap authentication is subject to the WhatsApp Business Terms of Service, and that it&#039;s your responsibility to ensure your customers expect that the code will be automatically filled in on their behalf when they choose to receive the zero-tap code through WhatsApp.&lt;br&gt;&lt;br&gt;If set to `false`, the template will **not** be created as you need to accept zero-tap terms before creating zero-tap enabled message templates. | `true` |
| `&lt;TIME_TO_LIVE&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | **Optional.**&lt;br&gt;&lt;br&gt;Authentication message time-to-live value, in seconds. See [Time-To-Live](https://developers.facebook.com/whatsapp/business-management-api/time-to-live). | `60` |

### Example request

```curl
curl &#039;https://graph.facebook.com/v25.0/102290129340398/message_templates&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039; \
-d &#039;
&#123;
  &quot;name&quot;: &quot;zero_tap_auth_template&quot;,
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
      &quot;code_expiration_minutes&quot;: 5
    &#125;,
    &#123;
      &quot;type&quot;: &quot;buttons&quot;,
      &quot;buttons&quot;: [
        &#123;
          &quot;type&quot;: &quot;otp&quot;,
          &quot;otp_type&quot;: &quot;zero_tap&quot;,
          &quot;text&quot;: &quot;Copy Code&quot;,
          &quot;autofill_text&quot;: &quot;Autofill&quot;,
          &quot;zero_tap_terms_accepted&quot;: true,
          &quot;supported_apps&quot;: [
            &#123;
              &quot;package_name&quot;: &quot;com.example.luckyshrub&quot;,
              &quot;signature_hash&quot;: &quot;K8a/AINcGX7&quot;
            &#125;
          ]
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

When a user in your app requests a password or code to be delivered to their WhatsApp number, first [initiate the handshake](#initiating-the-handshake), then call our API to send the authentication template message. When the WhatsApp client receives the message, it will perform an [eligibility check](#eligibility-check), and if there are no errors, start a broadcast.

If you do not initiate the handshake before sending the message, or the message fails an eligibility check, the broadcast will not be started. Instead, the delivered message will display a one-tap autofill button, if able to do so. If unable to do so, it will display a copy code button.

### Eligibility check

The WhatsApp client performs the following checks when it receives an authentication template message. If any check fails, it will attempt to display the one-tap autofill button in the message. If unable to do so, it will fall back to a copy code button.

* The handshake was initiated no more than 10 minutes ago (or no more than the number of minutes indicated by the template&#039;s `code_expiration_minutes` property, if present).
* The package name in the message (defined in the `package_name` property in the `components` array upon template creation) matches the package name set on the intent. The match is determined through the `getCreatorPackage` method called in the `PendingIntent` object provided by your application. See [One-Tap Autofill Button Class](#one-tap-autofill-button-activity-class).
* The app signing key hash in the message (defined in the `signature_hash` property in the components array upon template creation) matches your installed app&#039;s signing key hash.
* Your app has defined a one-tap autofill button activity and class to receive the password or code.
* Your app has defined a zero-tap broadcast receiver and class to receive the password or code.

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

### Zero-tap broadcast receiver

Declare a Receiver and intent filter that can receive the one-time password or code. The intent filter must have the action name com.whatsapp.otp.OTP_RETRIEVED.

```java
&lt;receiver
   android:name=&quot;.app.receiver.OtpCodeReceiver&quot;
   android:enabled=&quot;true&quot;
   android:exported=&quot;true&quot;&gt;
   &lt;intent-filter&gt;
       &lt;action android:name=&quot;com.whatsapp.otp.OTP_RETRIEVED&quot; /&gt;
   &lt;/intent-filter&gt;
&lt;/receiver&gt;
```

This is the receiver that the WhatsApp app or WhatsApp Business app will start once the authentication template message is received and it passes all [eligibility checks](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/authentication-templates/authentication-templates#eligibility-checks).

### Zero-tap receiver class

Using the SDK

We recommend using the SDK to declare a receiver. Define a class that extends `BroadcastReceiver`, then define the `onReceive` method, passing in your context and intent. Instantiate a `WhatsAppOtpIncomingIntentHandler` object, then run the `.processOtpCode()` method which will receive the intent, validate the handshake ID against the expected value you stored during handshake initiation, and handle errors.

```java
public class OtpCodeReceiver extends BroadcastReceiver &#123;

  &#064;Override
  public void onReceive(Context context, Intent intent) &#123;
    WhatsAppOtpIncomingIntentHandler whatsAppOtpIncomingIntentHandler = new WhatsAppOtpIncomingIntentHandler();

    // Retrieve the expected handshake ID that was stored during handshake initiation
    String expectedHandshakeId = retrieveStoredHandshakeId();

    whatsAppOtpIncomingIntentHandler.processOtpCode(intent,
      expectedHandshakeId,
      (code) -&gt; &#123;
        // The handshake ID has been validated by the SDK
        validateCode(code);
      &#125;,
      // call your function to handle errors
      (error, exception) -&gt; handleError(error, exception));
  &#125;
&#125;
```

#### Without the SDK

The broadcast receiver class should extract and validate the `request_id` (handshake ID) from the intent to ensure the OTP code is coming from a legitimate handshake initiated by your app:

```java
public class OtpCodeReceiver extends BroadcastReceiver &#123;

    &#064;Override
    public void onReceive(Context context, Intent intent) &#123;
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

### One-tap autofill button activity

**Optional.**

If you want the delivered message to be able to fall back to a one-tap autofill button if the message fails the eligibility check, implement this activity and intent filter in your app to receive the one-time password or code.

The intent filter must have the action name `com.whatsapp.otp.OTP_RETRIEVED`.

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

This is the activity that the WhatsApp client will start if the message fails the eligibility check but is still eligible to display a one-tap autofill button.

### One-tap autofill button activity class

**Optional.**

If you want the message to be able to display a one-tap autofill button if the if fails an eligibility check, define the activity public class that can accept the code once the user taps the button. The activity should validate the `request_id` (handshake ID) to ensure the OTP code is coming from a legitimate handshake initiated by your app.

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

#### Using the SDK

The preferred method for handshake initiation is via SDK. Performing a handshake via SDK can be done by instantiating a `WhatsAppOtpHandler` object and passing in your context to the `.sendOtpIntentToWhatsApp()` method. The method returns a UUID (handshake ID) that must be stored and used to validate the incoming OTP code later:

```java
WhatsAppOtpHandler whatsAppOtpHandler = new WhatsAppOtpHandler();
UUID handshakeId = whatsAppOtpHandler.sendOtpIntentToWhatsApp(context);
// Store handshakeId to validate the received OTP code later
```

#### Without the SDK

This example demonstrates one way to initiate a handshake with the WhatsApp app or WhatsApp Business app. The handshake now includes a `request_id` (UUID) that must be stored and validated when receiving the OTP code.

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

### Checking if WhatsApp is installed

You can check WhatsApp installation before offering WhatsApp as an option if you expect both WhatsApp and your app to be on the same device.

First, you need to add the following to your `AndroidManifest.xml` file:

```xml
&lt;queries&gt;
    &lt;package android:name=&quot;com.whatsapp&quot;/&gt;
    &lt;package android:name=&quot;com.whatsapp.w4b&quot;/&gt;
&lt;/queries&gt;
```

Instantiate the `WhatsAppOtpHandler` object:

```java
WhatsAppOtpHandler whatsAppOtpHandler = new WhatsAppOtpHandler();
```

Check if the WhatsApp client is installed by passing the `.isWhatsAppInstalled()` method as the clause in an `If` statement:

```java
If (whatsAppOtpHandler.isWhatsAppInstalled(context)) &#123;
    // ... do something
&#125;
```

## Error signals

See [Error Signals](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/authentication-templates/error-signals) that can help with debugging.

### Handshake ID error codes

The following error codes may be returned when using the SDK with handshake ID validation:

| Error Code | Description |
|------------|-------------|
| `HANDSHAKE_ID_MISSING` | The handshake ID was not included in the intent from WhatsApp |
| `HANDSHAKE_ID_INVALID_FORMAT` | The handshake ID is not a valid UUID format |
| `HANDSHAKE_ID_MISMATCH` | The handshake ID in the intent does not match the expected value |

## Send a zero-tap authentication template

Note that **you must first initiate a handshake** between your app and the WhatsApp client. See [Handshake](#handshake) above.

### Request

Use the [Messages API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/message-api#post-version-phone-number-id-messages) to send an [authentication template message with a one-time password button](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/authentication-templates/authentication-templates).

### Request syntax

```json
curl -X POST &quot;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;WHATSAPP_BUSINESS_PHONE_NUMBER_ID&gt;/messages&quot; \
  -H &quot;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&quot; \
  -H &quot;Content-Type: application/json&quot; \
  -d &#039;
&#123;
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
          &quot;index&quot;: &quot;0&quot;,
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
