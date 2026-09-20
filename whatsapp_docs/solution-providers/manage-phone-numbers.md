# Client phone numbers



**Warning:** **Embedded signup v2 will be deprecated on October 15, 2026.** Migrate your integration to [v4](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/version-4) before that date to avoid disruption. See [Versions](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/versions) for the full upgrade path.

This document describes client phone numbers, their requirements, and endpoints commonly used to manage business phone numbers.

## Basics

Your clients need a dedicated number to use WhatsApp. Phone numbers already in use with the WhatsApp app are not supported, but numbers in use with the WhatsApp Business app [can be registered](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/onboarding-business-app-users).

Clients can have multiple phone numbers associated with their [Meta Business Account](https://business.facebook.com/settings/), so they can [add another number for API use](#adding-more-phone-numbers) if they wish.

When completing the Embedded Signup flow, your clients should use a phone number and display name that they want to have appear in the WhatsApp app. Avoid signing up with a test or personal number, or a test display name, because these are difficult to change later.

- For more detailed information relating to phone numbers and WhatsApp for Business Platform, see [Phone Numbers](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-phone-numbers/phone-numbers).
- For information on how to migrate an existing registered WhatsApp phone number, see [Migrate Phone Number](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/support/migrating-phone-numbers-among-solution-partners-via-embedded-signup).

## Instructions for clients

This section is directed towards clients of Embedded Signup and provides guidance about actions they may perform relating to phone numbers.

### Add phone numbers to a WhatsApp Business account &#123;#adding-more-phone-numbers&#125;

There are two methods to add additional numbers to a WhatsApp Business account (WABA):

1. **[Recommended]** Go through the Embedded Signup flow again, select the existing Meta Business Suite and WABA, add the number, and verify it.
1. In the **Meta Business Suite**, go to the **Phone Numbers** tab of **WhatsApp Manager**, and select **Add Phone Number**. When using this option, the Solution Partner has to manually verify the phone number as phone verification is not available in WhatsApp Manager. For this reason, it is recommended that businesses follow the Embedded Signup flow to add additional numbers.

## Instructions for Solution Partners

This section is directed towards Solution Partners and provides instructions for managing client phone numbers.

### Getting phone numbers

Use the [Phone Numbers API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/phone-number-management-api#get-version-waba-id-phone-numbers) to get a list of business phone numbers on a client&#039;s WABA.

#### Request

```
curl &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;CUSTOMER_WABA_ID&gt;/phone_numbers&#039; \
-H &#039;Authorization: Bearer &lt;CUSTOMER_BUSINESS_TOKEN&gt;&#039;
```

#### Response

Upon success:

```
&#123;
  &quot;data&quot;: [
    &#123;
      &quot;verified_name&quot;: &quot;&lt;VERIFIED_DISPLAY_NAME&gt;&quot;,
      &quot;code_verification_status&quot;: &quot;&lt;VERIFICATION_STATUS&gt;&quot;,
      &quot;display_phone_number&quot;: &quot;&lt;DISPLAY_PHONE_NUMBER&gt;&quot;,
      &quot;quality_rating&quot;: &quot;&lt;QUALITY_RATING&gt;&quot;,
      &quot;platform_type&quot;: &quot;CLOUD_API&quot;,
      &quot;throughput&quot;: &#123;
        &quot;level&quot;: &quot;&lt;THROUGHPUT_LEVEL&gt;&quot;
      &#125;,
      &quot;webhook_configuration&quot;: &#123;
        &quot;application&quot;: &quot;&lt;WEBHOOK_CALLBACK_URL&gt;&quot;
      &#125;,
      &quot;id&quot;: &quot;&lt;BUSINESS_PHONE_NUMBER_ID&gt;&quot;
    &#125;
  ],
  &quot;paging&quot;: &#123;
    &quot;cursors&quot;: &#123;
      &quot;before&quot;: &quot;&lt;BEFORE_CURSOR&gt;&quot;,
      &quot;after&quot;: &quot;&lt;AFTER_CURSOR&gt;&quot;
    &#125;
  &#125;
&#125;
```

### Register phone numbers

After a client successfully completes the Embedded Signup flow and their phone number is verified, you must register the number for Cloud API use by calling the [Register API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/register-api#post-version-phone-number-id-register) with the `messaging_product` and `pin` parameters. See [Step 4: Register the number](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/registering-phone-numbers#step-4--register-the-number) for details.

Alternatively, **you can pre-verify phone numbers** and offer them to your clients in the new Embedded Signup flow. This prevents clients from having to contact you for a one-time password during the onboarding process. See [Pre-Verified Phone Numbers](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/pre-verified-numbers).

**Note:** A phone number **must** be registered up to 14 days after going through the Embedded Signup flow. If a number is not registered during that window, the phone must go through to the Embedded Signup flow again prior to registration.

### Get phone metadata

The [Phone Numbers API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/phone-number-management-api#get-version-waba-id-phone-numbers) allows you to see the status of a phone number&#039;s display name and other metadata.

#### Example request

In the following example, use the ID for the assigned WABA.

```
curl -i -X GET &quot;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;WABA_ID&gt;/phone_numbers
  ?fields=
    display_phone_number,
    name_status,
    new_name_status
  &amp;access_token=&lt;SYSTEM_USER_ACCESS_TOKEN&gt;&quot;
```

To find the ID of a WhatsApp Business Account, go to [**Business Manager**](https://business.facebook.com/) &gt; **Business Settings** &gt; **Accounts** &gt; **WhatsApp Business Accounts**. Find the account you want to use and click on it. A panel opens, with information about the account, including the ID.


#### Example response

```
&#123;
  &quot;data&quot;: [
    &#123;
      &quot;id&quot;: &quot;1972385232742141&quot;,
      &quot;display_phone_number&quot;: &quot;+1 631-555-1111&quot;,
      &quot;last_onboarded_time&quot;: &quot;2023-08-22T19:05:53+0000&quot;,
      &quot;name_status&quot;: &quot;APPROVED&quot;,
      &quot;new_name_status&quot;: &quot;APPROVED&quot;
    &#125;
  ]
&#125;
```

### Response parameters

| Name | Description |
| --- | --- |
| `name_status` | The review status of the current display name request. Available Options:&lt;br&gt;&lt;br&gt;- `APPROVED`: The name has been approved.&lt;br&gt;- `DECLINED`: The name has not been approved.&lt;br&gt;- `EXPIRED`: The approved name has expired.&lt;br&gt;- `PENDING_REVIEW`: Your name request is under review.&lt;br&gt;- `NONE`: No display name has been set. |
| `new_name_status` | The review status of a display name change request. This field returns data only if a display name change was requested. |

### Get phone number OTP status

To see if a phone number has been verified via OTP (one-time password), check that number&#039;s `code_verification_status` field. Use the [Phone Numbers API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/phone-number-management-api#get-version-waba-id-phone-numbers) to get phone numbers on the WABA:

```
curl -i -X GET \
&quot;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;WABA_ID&gt;/phone_numbers
  ?access_token=&lt;ACCESS_TOKEN&gt;&quot;
```

The response includes the `code_verification_status` with one of the following options: `VERIFIED` or `NOT_VERIFIED`. A sample response looks like this:

```
[
  &#123;
    &quot;code_verification_status&quot;: &quot;NOT_VERIFIED&quot;,
    &quot;id&quot;: &quot;1754951608042154&quot;
  &#125;
]
```

Alternatively, you can get the status by calling a phone number&#039;s ID:

```
curl -i -X GET \
&quot;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;PHONE_NUMBER_ID&gt;
  ?access_token=&lt;ACCESS_TOKEN&gt;&quot;
```

Use the [WhatsApp Business Account &gt; Phone Numbers](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/phone-number-management-api#Reading) endpoint to get a phone number&#039;s ID. See [Retrieve Phone Numbers](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-phone-numbers/phone-numbers#get-all-phone-numbers) for usage details.


### Filter phone numbers by account mode

You can query phone numbers and filter them based on their `account_mode`. For the request, you can use the parameters listed below.

#### Request parameters

| Name | Description |
| --- | --- |
| `field` | Contains the field being used for filtering. In this example, you should use `account_mode`. |
| `operator` | Contains how you want to filter the accounts. In this example, you should use `EQUAL`. |
| `value` | Contains the account mode you are looking for. Supported Values:&lt;br&gt;&lt;br&gt;- `SANDBOX`: The account is unverified.&lt;br&gt;&lt;br&gt;- `LIVE`: The account is not eligible for the unverified trial experience or it has upgraded to a verified account. |

#### Example request

In the following example, use the ID for the assigned WABA.

```
curl -i -X GET &quot;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;WABA_ID&gt;/phone_numbers
  ?filtering=[&#123;
    &quot;field&quot;:&quot;account_mode&quot;,
    &quot;operator&quot;:&quot;EQUAL&quot;,
    &quot;value&quot;:&quot;SANDBOX&quot;&#125;]
  &amp;access_token=&lt;SYSTEM_USER_ACCESS_TOKEN&gt;&quot;
```

#### Example response

```
&#123;
  &quot;data&quot;: [
    &#123;
      &quot;id&quot;: &quot;1972385232742141&quot;,
      &quot;display_phone_number&quot;: &quot;+1 631-555-1111&quot;,
      &quot;verified_name&quot;: &quot;John&#039;s Cake Shop&quot;,
      &quot;quality_rating&quot;: &quot;UNKNOWN&quot;
    &#125;
  ],
  &quot;paging&quot;: &#123;
  &quot;cursors&quot;: &#123;
    &quot;before&quot;: &quot;abcdefghij&quot;,
    &quot;after&quot;: &quot;klmnopqr&quot;
  &#125;
   &#125;
&#125;
```

## Learn more
* [Phone numbers: WhatsApp for Business Platform Overview](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-phone-numbers/phone-numbers)
* [Phone numbers: Migrate an existing registered number](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/support/migrating-phone-numbers-among-solution-partners-via-embedded-signup)
* Reference: [WhatsApp Business account](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/whatsapp-business-account-api)
