# Business-scoped user IDs


**Warning:** June 29, 2026 update! See [changelog entry](#june-29-2026) for details.

Starting June 29, 2026 you can begin reserving your business username. See [reserved usernames](#reserved-usernames) for more detail.

To get started with testing, see [webhook testing](#webhook-testing).

WhatsApp will start rolling out usernames gradually in 2026. We recommend all WhatsApp Business Platform businesses and partners begin testing and building integrations with Business-scoped user IDs (BSUID) to ensure smooth transition in your workflows.

Username adoption is optional for users and businesses. If a username is adopted by a WhatsApp user, their username will be displayed instead of their phone number in the app. Business usernames are not intended for privacy, however. If you adopt a business username, it will not cause your business phone number to be hidden in the app.

To support usernames, Meta began sharing in April 2026 a new backend user identifier called business-scoped user ID, or BSUID. BSUID uniquely identifies a WhatsApp user and is tied to a specific business. Supporting business-scoped user IDs (BSUID) is required for all partners and directly-integrated businesses on the WhatsApp Business Platform — as well as CTWA advertisers. Because you cannot control whether your users adopt usernames, you must support BSUID to avoid losing the ability to process their messages.

This document describes how the addition of usernames will impact API requests, API responses, and webhook payloads. Additional changes to support usernames before the feature is made available will be recorded here.

**Any changes described in this document are subject to change.**

## User usernames

A user username is a unique, optional name that WhatsApp users can set in order to display their username instead of their phone number in the app. Usernames can be used in lieu of profile names when personalizing message content for individual users.

WhatsApp users are limited to 1 username, but are able to change them periodically. Changing a username does not affect the user&#039;s phone number or business-scoped user ID, and does not affect the user&#039;s ability to communicate with other WhatsApp users or businesses on the WhatsApp Business Platform. User usernames have the same format restrictions as [business usernames](#business-usernames).

Usernames are assigned to the `username` property in API responses and webhooks payloads. Once enabled, a WhatsApp user&#039;s username will appear in all incoming [messages](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/messages#incoming-messages) webhooks, and all **delivered** and **read** [status messages](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/messages/status) webhooks.

## Business-scoped user ID

**Warning:** BSUIDs began appearing in webhooks in early April 2026.

A BSUID is a unique user identifier that can be used to message a WhatsApp user when you don&#039;t know their phone number. BSUID will be assigned to the `user_id` parameter and appear in all [messages webhooks](#messages-webhooks), regardless of whether or not the user has enabled the username feature. In [status messages webhooks](#status-messages-webhooks), the user&#039;s BSUID is included in both the `contacts` block (`user_id`) and the `statuses` block (`recipient_user_id`), regardless of whether the original message was sent to the user&#039;s phone number or their BSUID. The exception is `failed` status messages: the `contacts` block is omitted entirely, and `recipient_user_id` will be omitted if the message was sent to the user&#039;s phone number.

BSUIDs are scoped to individual business portfolios. This means that any business phone number owned by a given portfolio can be used to message a BSUID scoped to the same portfolio, and attempts to message the BSUID using a phone number owned by a different portfolio will fail.

BSUIDs will be:

- generated automatically
- prefixed with the user&#039;s [ISO 3166 alpha-2](https://www.iso.org/iso-3166-country-codes.html) two-letter country code and a period, followed by up to 128 alphanumeric characters (for example, `US.13491208655302741918`)
- unique to each business portfolio-user pair ([business portfolios](https://www.facebook.com/business/help/486932075688253) were formerly known as Business Managers)
- regenerated if a user changes their phone number (which triggers a [system messages webhook](#system-messages-webhooks))

BSUIDs can be used to send any type of message except for one-tap, zero-tap, and copy code [authentication templates](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/authentication-templates/authentication-templates), which require user phone numbers.

When making API requests with BSUIDs, use the entire BSUID value: country code, period, and all alphanumeric characters. Omitting or changing the country code, period, or alphanumeric characters will cause your request to fail.

If you are a managed business with multiple business portfolios, and want to use BSUIDs that will work across all of them, see [Parent business-scoped user IDs](#parent-business-scoped-user-ids).

## Parent business-scoped user IDs

If you are a managed business and want to enroll your business portfolios to receive parent BSUIDs, you can ask your Meta point-of-contact to check if you are eligible. If you are eligible, and your business portfolios become enrolled, parent BSUIDs will be included in all messages webhooks, assigned to a new `parent_user_id` property.

Parent BSUIDs can be used in place of regular BSUIDS to message users. Functionally, parent BSUIDs have the same properties as regular BSUIDs, but can be used by any business phone number within the set of enrolled portfolios. Parent BSUIDs follow the same format as regular BSUIDs, but include `ENT` between the country code and the alphanumeric identifier (for example, `US.ENT.11815799212886844830`).

Note that you can still message users using their regular BSUID scoped to your business portfolio.

If you enroll to receive parent BSUIDs:

- Your business portfolio will share a parent BSUID with other business portfolios that have been enrolled in the same parent BSUID account. WhatsApp users will be identified by the same parent BSUID across all enrolled business portfolios.
- Your webhook payloads will include both a BSUID and a parent BSUID for each WhatsApp user interaction.
- No other account capabilities or permissions are affected. Your business portfolio retains its existing access controls, billing, and administrative independence.
- All business portfolios enrolled in the same parent BSUID account will be visible via the [Parent BSUID Accounts API](#get-parent-bsuid-account) to all other enrolled business portfolios.

### Get parent BSUID account

Parent BSUIDs for a given user are associated with a parent BSUID account. All business portfolios enrolled in the account are able to use its parent BSUIDs. Use the Parent BSUID Accounts API to get the parent BSUID account ID and the list of business portfolios enrolled in it.

Request syntax:

```html
curl &#039;https://api.facebook.com/&lt;BUSINESS_ID&gt;/parent-bsuid-accounts&#039; \
-H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039;
```

Response syntax:

```html
&#123;
  &quot;parent_bsuid_account_id&quot;: &quot;&lt;PARENT_BSUID_ACCOUNT_ID&gt;&quot;,
  &quot;enrolled_business_portfolios&quot;: [
    &quot;&lt;BUSINESS_PORTFOLIO_ID&gt;&quot;,
    &quot;&lt;BUSINESS_PORTFOLIO_ID&gt;&quot;
  ]
&#125;
```

- `parent_bsuid_account_id` — The ID of the parent BSUID account that is shared across your enrolled business portfolios.
- `enrolled_business_portfolios` — An array of business portfolio IDs enrolled in the parent BSUID account. Any business phone number within these portfolios can use the account&#039;s parent BSUIDs.

## Phone numbers

If a WhatsApp user enables the username feature, their phone number will not be included in webhooks, unless you have interacted with the user before, as explained below. Therefore, regardless of whether or not the user has enabled the feature, the user&#039;s BSUID will be included in any webhooks that would normally include their phone number, assigned to a new user_id property.

To reduce the chance of losing conversation context with existing users who enable the usernames feature, user phone numbers will be included in webhooks if any of the following conditions are met:

- You have messaged or called the user&#039;s phone number within the last 30 days of the webhook being triggered
- You have received a message or call from the user&#039;s phone number within the last 30 days of the webhook being triggered
- The user is in your [contact book](#contact-book)

Note that the 30-day lookback conditions above are evaluated per business phone number. If you message a user from one of your business phone numbers, webhooks associated with a different business phone number in your portfolio will not include the user&#039;s phone number unless that specific number has also sent or received a message or call to or from the user&#039;s phone number within the last 30 days.

BSUIDs began appearing in webhooks in early April 2026. However, our APIs will not support sending messages targeted to the BSUIDs until July 2026. Once our APIs support BSUIDs in July, you will be able to message users using either their BSUID, phone number, or both.

If you are a solution provider and provide WhatsApp messaging services to your business customers, your customers will be able to use your app to message users, using their portfolio&#039;s business phone numbers and any BSUIDs scoped to their portfolio. If you attempt to use one of your business customer&#039;s BSUIDs with your own business phone number, however, it will fail, since BSUIDs are scoped to portfolios (and essentially, the assets the portfolio owns).

If you are unsure of asset ownership:

- Use the [Client WhatsApp Business Accounts API](https://developers.facebook.com/documentation/ads-commerce/marketing-api/reference/business/client_whatsapp_business_accounts#get-version-business-id-client-whatsapp-business-accounts) to get a list of WABAs that you do not own, but that are shared with you.
- Use the [Owned WhatsApp Business Accounts API](https://developers.facebook.com/documentation/ads-commerce/marketing-api/reference/business/owned_whatsapp_business_accounts#get-version-business-id-owned-whatsapp-business-accounts) to get a list of WABAs that you own.
- Use the [Phone Numbers API](https://developers.facebook.com/docs/graph-api/reference/whats-app-business-account/phone_numbers/#Reading) to get a list of phone numbers owned by a given WABA.


## Requesting phone numbers from users

**Warning:** This feature will be available in early July 2026.

To make it easier to request phone numbers from WhatsApp users, a `REQUEST_CONTACT_INFO` button type is available that can be added to `utility` and `marketing` templates, or sent as an [interactive message](https://developers.facebook.com/documentation/business-messaging/whatsapp/messages/interactive-messages).

If a user taps this button, their WhatsApp phone number will be shared in the message thread, and a [contacts webhook](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/messages/contacts) will be triggered containing the user&#039;s phone number. Note that if a WhatsApp user shares a contact using the share contacts feature in the WhatsApp app instead, the webhook will also include the contact&#039;s [vCard](https://datatracker.ietf.org/doc/html/rfc6350).

If you are using the contact book feature, their phone number will also be added to your contact book automatically. For businesses that have enabled Local Storage, Meta extracts the user&#039;s phone number from the shared contact card (vCard) and stores it in your contact book on Meta data centers. Only the phone number is extracted and stored; no other vCard data is retained beyond the standard data-in-use period.

### Using templates

To add a request contact information button to a utility or marketing template, include a `REQUEST_CONTACT_INFO` button in the `components` array when creating the template:

```html
curl &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;/message_templates&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039; \
-d &#039;
&#123;
  &quot;name&quot;: &quot;&lt;TEMPLATE_NAME&gt;&quot;,
  &quot;language&quot;: &quot;&lt;TEMPLATE_LANGUAGE&gt;&quot;,
  &quot;category&quot;: &quot;utility&quot;,
  &quot;components&quot;: [
    &#123;
      &quot;type&quot;: &quot;body&quot;,
      &quot;text&quot;: &quot;&lt;BODY_TEXT&gt;&quot;
    &#125;,
    &#123;
      &quot;type&quot;: &quot;buttons&quot;,
      &quot;buttons&quot;: [
        &#123;
          &quot;type&quot;: &quot;REQUEST_CONTACT_INFO&quot;
        &#125;
      ]
    &#125;
  ]
&#125;&#039;
```

Request contact information buttons cannot be customized, so you do not need to include any parameter values when sending the template:

```html
curl &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;BUSINESS_PHONE_NUMBER_ID&gt;/messages&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039; \
-d &#039;
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;recipient_type&quot;: &quot;individual&quot;,
  &quot;recipient&quot;: &quot;&lt;BSUID&gt;&quot;,
  &quot;type&quot;: &quot;template&quot;,
  &quot;template&quot;: &#123;
    &quot;name&quot;: &quot;&lt;TEMPLATE_NAME&gt;&quot;,
    &quot;language&quot;: &#123;
      &quot;code&quot;: &quot;&lt;TEMPLATE_LANGUAGE&gt;&quot;
    &#125;
  &#125;
&#125;&#039;
```

### Using interactive messages

You can also send a request contact information button as an interactive message:

```html
curl &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;BUSINESS_PHONE_NUMBER_ID&gt;/messages&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039; \
-d &#039;
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;recipient_type&quot;: &quot;individual&quot;,
  &quot;recipient&quot;: &quot;&lt;BSUID&gt;&quot;,
  &quot;type&quot;: &quot;interactive&quot;,
  &quot;interactive&quot;: &#123;
    &quot;type&quot;: &quot;request_contact_info&quot;,
    &quot;body&quot;: &#123;
      &quot;text&quot;: &quot;&lt;BODY_TEXT&gt;&quot;
    &#125;,
    &quot;action&quot;: &#123;
      &quot;name&quot;: &quot;request_contact_info&quot;
    &#125;
  &#125;
&#125;&#039;
```

### Contacts webhook

When a user shares their contact information — either by tapping a `REQUEST_CONTACT_INFO` button or by sharing a contact directly in the chat — a [contacts webhook](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/messages/contacts) will be triggered.

```html
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;&quot;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;messaging_product&quot;: &quot;whatsapp&quot;,
            &quot;metadata&quot;: &#123;
              &quot;display_phone_number&quot;: &quot;&lt;BUSINESS_DISPLAY_PHONE_NUMBER&gt;&quot;,
              &quot;phone_number_id&quot;: &quot;&lt;BUSINESS_PHONE_NUMBER_ID&gt;&quot;
            &#125;,
            &quot;contacts&quot;: [
              &#123;
                &quot;profile&quot;: &#123;
                  &quot;name&quot;: &quot;&lt;USER_DISPLAY_NAME&gt;&quot;,
                  &quot;username&quot;: &quot;&lt;USERNAME&gt;&quot;
                &#125;,
                &quot;user_id&quot;: &quot;&lt;BSUID&gt;&quot;
              &#125;
            ],
            &quot;messages&quot;: [
              &#123;
                &quot;id&quot;: &quot;&lt;WHATSAPP_MESSAGE_ID&gt;&quot;,
                &quot;timestamp&quot;: &quot;&lt;WEBHOOK_TRIGGER_TIMESTAMP&gt;&quot;,
                &quot;type&quot;: &quot;contacts&quot;,
                &quot;from_user_id&quot;: &quot;&lt;BSUID&gt;&quot;,               &lt;!-- ADDED --&gt;
                &quot;contacts&quot;: [
                  &#123;
                    &quot;vcard&quot;: &quot;&lt;VCARD&gt;&quot;,                   &lt;!-- ADDED --&gt;
                    &quot;origin&quot;: &quot;&lt;ORIGIN&gt;&quot;,                 &lt;!-- ADDED --&gt;
                    &quot;phones&quot;: [
                      &#123;
                        &quot;phone&quot;: &quot;&lt;USER_PHONE_NUMBER&gt;&quot;,
                        &quot;wa_id&quot;: &quot;&lt;USER_WA_ID&gt;&quot;,
                        &quot;type&quot;: &quot;&lt;USER_PHONE_NUMBER_TYPE&gt;&quot;
                      &#125;
                    ]
                  &#125;
                ]
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

- `from_user_id` — New property. Will be set to the user&#039;s BSUID.
- `origin` — New property. Indicates how the contact information was shared. Values can be:
  - `contact_request` — The user shared their contact information by tapping a `REQUEST_CONTACT_INFO` button.
  - `other` — The user shared a contact directly in the chat (not via a `REQUEST_CONTACT_INFO` button).
- `vcard` — The user&#039;s virtual contact card in [vCard](https://datatracker.ietf.org/doc/html/rfc6350) format. Will be set to the user&#039;s vCard if `origin` is `other`. Will be omitted if `origin` is `contact_request`.

## Contact book

To support messaging thread continuity, a contact book feature that stores WhatsApp user contact information is available. The contact book is provided and hosted by Meta; no integration work is required.

Once the feature is available, if you send a message/call to a user&#039;s phone number, or receive a message/call from a user&#039;s phone number, the user&#039;s phone number and BSUID will be added to your contact book. Once this data has been recorded, it will be used to populate any webhook payloads to include the user&#039;s phone number, regardless of whether or not the user has enabled the usernames feature.

The contact book is scoped to the business portfolio level, so any interaction between any business phone number within the business portfolio and a user will trigger the user&#039;s phone number and BSUID to be stored in the contact book. Only interactions that occur after the contact book launches will trigger storage; prior interactions will not be retroactively captured, and contact information from those users will not be included in webhooks.

Contact book data will be retained until you disable the feature, or deactivate your account. If you wish, you can disable this feature in the **Meta Business Suite** &gt; **Business settings** &gt; [**Business info**](https://business.facebook.com/latest/settings/business_info) panel. If you disable your contact book, it will stop storing user information, and any existing user information it has already stored will be deleted. If you re-enable the contact book later, it will start storing user information again, but previously stored information cannot be restored.

Limitations:

- If you are using [Local Storage](https://developers.facebook.com/documentation/business-messaging/whatsapp/local-storage) and a user shares their phone number with you by tapping the share contact information button, Meta extracts the user&#039;s phone number from the shared contact card (vCard) and stores it in your contact book on Meta data centers. Only the phone number is extracted and stored; no other vCard data is retained beyond the [standard data-at-rest period for local storage](https://developers.facebook.com/documentation/business-messaging/whatsapp/local-storage#how-local-storage-works).
- Contact books are scoped to business portfolios. This means that if you have multiple portfolios enrolled in the same parent BSUID account, a user&#039;s phone number and BSUID would have to be recorded to each portfolio&#039;s contact book independently; user contact information is not shared or synced across enrolled portfolios.

### Delete a contact book entry

Use the Contact Book API to delete a specific user&#039;s entry from your contact book. Once deleted, the user&#039;s phone number and BSUID will no longer be included in webhook payloads for any business phone number within the business portfolio, unless the phone number is in the 30-day cache or a new interaction triggers a new contact book entry.

Request syntax:

```html
curl -X DELETE &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;BUSINESS_PHONE_NUMBER_ID&gt;/contact_book?messaging_product=whatsapp&amp;bsuid=&lt;BSUID&gt;&#039; \
-H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039;
```

Set `bsuid` to the BSUID of the contact book entry to delete. Must use the standard BSUID format (for example, `US.13491208655302741918`). The BSUID must belong to the same business portfolio as the business phone number. Parent BSUIDs are not supported.

Response syntax:

```html
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;success&quot;: &lt;SUCCESS?&gt;,
  &quot;deleted&quot;: &lt;DELETED?&gt;
&#125;
```

- `success` — Boolean. Will be set to `true` if the request was processed successfully.
- `deleted` — Boolean. Will be set to `true` if the contact book entry existed and was deleted, or `false` if no entry was found for the specified BSUID.

## Country codes

If a WhatsApp user enables the username feature, their phone number (and thus, country dialing code) may not appear in webhooks. In these cases, the user&#039;s BSUID will appear instead, prefixed with the user&#039;s [ISO 3166 alpha-2](https://www.iso.org/iso-3166-country-codes.html) two-letter country code (for example, `US.13491208655302741918`).

## Business usernames

Businesses will also be able to adopt a business username. If you adopt a business username, however, it will not cause your business phone number to be hidden in the WhatsApp or WhatsApp Business client.

A business username is mapped to a single business phone number across all of WhatsApp; that is, a phone number can have only one username at a given time, and no two WhatsApp phone numbers (consumer or business) can have the same username.

Business usernames must adhere to the following format:

- may only contain English letters (a-z), digits (0-9), period (.) and underscore (_) characters
- non-English characters (such as ñ, é, ü) are not supported and will cause the request to fail
- must be between 3-35 characters in length
- must contain at least one English letter (a-z, A-Z)
- must not start or end with a period or have 2 consecutive periods
- must not start with www
- must not end with a domain (for example, .com, .org, .net, .int, .edu, .gov, .mil, .us, .in, .html, and so on)
- case is ignored when comparing usernames, but period and underscore characters are not; for example, myID and myid are the same username but myid, my.id, and my_id are all distinct

### Reserved usernames

Starting June 29, 2026, you will have the option to claim a username that WhatsApp has reserved for you. Alternatively, you can adopt a different username that aligns with your branding requirements. A reserved username can be claimed through WhatsApp Manager, Meta Business Suite, or via the [Username API](#get-reserved-usernames). Claimed usernames that are approved will become active once the username feature is made available.

If a reserved username is already in use with your Facebook Page or Instagram account, you must link your business phone number to your Facebook Page or Instagram account before you will be able to claim the username.

You can link your phone number when claiming the username in Meta Business Suite or WhatsApp Manager, or by accessing your Facebook Page or Instagram account and [adding your phone number directly](https://www.facebook.com/business/help/4631406400243963).

To link your phone number, you must have full control of the page or account, or basic partial access with the manage_phone permission. See [About business portfolio and business asset permissions](https://www.facebook.com/business/help/442345745885606) for information about control/access and permissions.

### Chat window display priority

The following priority will be followed (in decreasing order of priority) for displaying business profile information in chat windows in the app. Your business phone numbers will always appear in your business profile.

- Saved contact name
- Verified business name or [Official Business Account](https://developers.facebook.com/documentation/business-messaging/whatsapp/official-business-accounts) name
- Username
- Phone number

### Support

- You can contact your Partner Manager with any concerns.
- You can reach out to any of the [standard support channels](https://developers.facebook.com/documentation/business-messaging/whatsapp/support); for API integrations, please raise a Direct support ticket with question type, **WA Usernames API Integration**.
- Use the **Report Abuse** channel via [Direct Support](https://business.facebook.com/direct-support/) to report impersonation.
- Use our [WhatsApp Intellectual Property Contact Form](https://www.whatsapp.com/contact/forms/5071674689613749) form to report infringement.

### Adopt or change a business username

**Warning:** This feature will be available starting June 29, 2026.

Starting June 29, 2026, you will be able to adopt or change a business username using Meta Business Suite, WhatsApp Manager, WhatsApp Business app, or via the Username API.

Request syntax:

```html
curl -X POST &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;BUSINESS_PHONE_NUMBER_ID&gt;/username&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039; \
-d &#039;
&#123;
  &quot;username&quot;: &quot;&lt;DESIRED_USERNAME&gt;&quot;,
  &quot;transfer_action&quot;: &quot;&lt;TRANSFER_ACTION&gt;&quot;
&#125;&#039;
```

The `username` parameter is required. The `transfer_action` parameter is optional.

- `username` — The desired username. Must follow the [business username format](#business-usernames).
- `transfer_action` — Controls what happens when the requested username is currently in use on another business phone number within the same business portfolio (for example, when you want to move an existing username to a different one of your phone numbers). Values can be:
  - `none` (default) — Do not transfer the username. If the username is already in use on another business phone number in the same business portfolio, the request fails with error code `147005`.
  - `force_transfer` — Transfer the username from the other business phone number to this business phone number. The username is removed from the other phone number and assigned to this one.

Response syntax, upon success:

```html
&#123;
  &quot;status&quot;: &quot;&lt;STATUS&gt;&quot;
&#125;
```

- `status` — The status of the latest requested username. Values can be:
  - `approved` — The requested username has been approved and will be visible to WhatsApp users once the usernames feature is made available.
  - `reserved` — The requested username has been reserved and approved but not yet visible to WhatsApp users. It will appear to WhatsApp users once the feature is available for everyone.

Response syntax, upon failure:

```html
&#123;
  &quot;error&quot;: &#123;
    &quot;message&quot;: &quot;&lt;MESSAGE&gt;&quot;,
    &quot;type&quot;: &quot;&lt;TYPE&gt;&quot;,
    &quot;code&quot;: &lt;CODE&gt;,
    &quot;error_data&quot;: &#123;
      &quot;messaging_product&quot;: &quot;whatsapp&quot;,
      &quot;details&quot;: &quot;&lt;DETAILS&gt;&quot;
    &#125;,
    &quot;error_subcode&quot;: &lt;ERROR_SUBCODE&gt;,
    &quot;fbtrace_id&quot;: &quot;&lt;FBTRACE_ID&gt;&quot;
  &#125;
&#125;
```


| Code | Details | Possible reason and solutions |
| :----: | :---------------------------------------------------: | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
| `10` | Application does not have permission for this action | Confirm that the system user whose token is used in the request has appropriate [business asset access](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#business-asset-access) on the WhatsApp Business Account: either **Full control** or **Partial access** for **Phone numbers**. |
| `33` | Invalid ID | (1) The business phone number ID is invalid, (2) the WhatsApp Business Account associated with the business phone number has been deleted, or (3) the user whose token was used in the request has not granted the app the **whatsapp_business_management** permission (which requires Advanced Access if you are a [solution provider](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/overview)) |
| `100` | Param Invalid | The [username format](#business-usernames) is invalid. |
| `147001` | Username not available | The username has already been claimed, doesn&#039;t pass our internal checks, or is not available for on the platform. Try requesting another username. |
| `147002` | Account not eligible to request a username | The business portfolio that owns the WhatsApp Business Account and business phone number must have a higher [messaging limit](https://developers.facebook.com/documentation/business-messaging/whatsapp/messaging-limits). |
| `147003` | FB Account not linked | You must [link](https://www.facebook.com/business/help/4631406400243963) the phone number to the Facebook Page that already uses the requested username. |
| `147004` | IG Account not linked | You must [link](https://www.facebook.com/business/help/4631406400243963) the phone number to the Instagram account that already uses the requested username |
| `147005` | Username transfer required | The requested username is currently in use on another business phone number within your business portfolio. To transfer it to this phone number, resend the request with `transfer_action` set to `force_transfer`. |
| `133010` | Account not registered | The business phone number must first be [registered for API use](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-phone-numbers/registration). |

### Get current username

Use the [Username API](#get-current-username) to get the status of the business username associated with the business phone number, or information about the username.

Request syntax:

```html
curl &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;BUSINESS_PHONE_NUMBER_ID&gt;/username&#039; \
-H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039;
```

Response syntax:

```html
&#123;
  &quot;username&quot;: &quot;&lt;USERNAME&gt;&quot;,
  &quot;status&quot;: &quot;&lt;STATUS&gt;&quot;
&#125;
```

- `username` — Current username. Will be omitted if the business phone number has no username.
- `status` — Username status. Values can be:
  - `approved` — The username is approved and visible to WhatsApp users.
  - `reserved` — The username is reserved for the business phone number but is not visible to WhatsApp users. It will become visible once the usernames feature is made available to everyone.

### Get reserved usernames

Use the [Username Suggestions API](#get-reserved-usernames) to get a list of usernames that have been reserved for your business portfolio.

Use the [Username API](#adopt-or-change-a-business-username) to claim the desired username from the list, which will then need to be approved. Once approved and usernames become available in your country, it will move to to an &quot;active&quot; status, meaning the business username will start appearing on your business profile, and users will be able to search for it using exact match search.

Request syntax:

```html
curl &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;BUSINESS_PHONE_NUMBER_ID&gt;/username_suggestions&#039; \
-H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039;
```

Response syntax:

```html
&#123;
  &quot;data&quot;: [
   &#123;
     &quot;username_suggestions&quot;: [
       &quot;&lt;RESERVED_USERNAME&gt;&quot;,
       &lt;!-- Additional usernames would follow, if any --&gt;
     ]
   &#125;
 ],
&#125;
```

- `username_suggestions` — An array of reserved usernames, if any. These usernames have a higher chance of approval.

### Delete a username

Use the [Username API](#delete-a-username) to delete the business username associated with the business phone number.

Request syntax:

```html
curl -X DELETE &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;BUSINESS_PHONE_NUMBER_ID&gt;/username&#039; \
-H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039;
```

Response syntax:

```html
&#123;
  &quot;success&quot;: &lt;SUCCESS?&gt;
&#125;
```

- `success` — Boolean. Will be set to `true` if the username is deleted successfully, otherwise it will be set to `false`.

### business_username_updates webhook

A new **business_username_updates** webhook will be added. This webhook will be triggered when a business username status changes.

Please subscribe each of your apps to this webhook field to be notified of username changes.

```html
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;&quot;,
      &quot;time&quot;: &lt;WEBHOOK_TRIGGER_TIMESTAMP&gt;,
      &quot;changes&quot;: [
        &#123;
          &quot;field&quot;: &quot;business_username_updates&quot;,
          &quot;value&quot;: &#123;
            &quot;display_phone_number&quot;: &quot;&lt;BUSINESS_DISPLAY_PHONE_NUMBER&gt;&quot;,
            &quot;username&quot;: &quot;&lt;USERNAME&gt;&quot;,
            &quot;status&quot;: &quot;&lt;STATUS&gt;&quot;
          &#125;
        &#125;
      ]
    &#125;
  ]
&#125;
```

- `id` — WhatsApp Business Account ID.
- `time` — Unix timestamp indicated when the webhook was triggered.
- `display_phone_number` — The business phone number&#039;s display number (the number displayed on your profile in the app).
- `username` — The username for which the status has changed. Omitted if `status` is set to `deleted`.
- `status` — Values can be:
  - `approved` — Indicates the username is approved and visible to WhatsApp users. Triggered when the username&#039;s status changes from `reserved` to `approved`, or the username was changed via the WhatsApp Business app.
  - `deleted` — Indicates the username has been deleted via the WhatsApp Business app.
  - `reserved` — Indicates the username is reserved for the business phone number but is not visible to WhatsApp users. It will become visible once the usernames feature is made available to everyone.

## Messages

### Send message requests

**Warning:** This feature is coming soon.

These changes will apply to [Messages API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/message-api#post-version-phone-number-id-messages) requests.

This example syntax is sending a `text` message, but the changes apply to all message types.

```html
&#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;BUSINESS_PHONE_NUMBER_ID&gt;/messages&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039; \
-d &#039;
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;recipient_type&quot;: &quot;individual&quot;,
  &quot;to&quot;: &quot;&lt;USER_PHONE_NUMBER&gt;&quot;,    &lt;!-- CHANGED --&gt;
  &quot;recipient&quot;: &quot;&lt;BSUID&gt;&quot;,         &lt;!-- ADDED --&gt;
  &quot;type&quot;: &quot;text&quot;,
  &quot;text&quot;: &#123;
    &quot;body&quot;: &quot;&lt;BODY_TEXT&gt;&quot;
  &#125;
&#125;&#039;
```

You can include both `to` (phone number) and `recipient` (BSUID or parent BSUID) in your request. If you do, `to` (phone number) will take precedence. If you prefer, you can also use one or the other:

To send a message using only the user&#039;s phone number:

- set `to` to the user&#039;s phone number
- omit the `recipient` property

To send a message using only the user&#039;s BSUID or parent BSUID:

- set `recipient` to the user&#039;s BSUID or parent BSUID
- omit the `to` property

### Send message response

These changes apply to [Messages API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/message-api#post-version-phone-number-id-messages) responses.

```html
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;contacts&quot;: [
    &#123;
      &quot;input&quot;: &quot;&lt;USER_PHONE_NUMBER_OR_BSUID&gt;&quot;,    &lt;!-- CHANGED --&gt;
      &quot;wa_id&quot;: &quot;&lt;USER_PHONE_NUMBER&gt;&quot;,             &lt;!-- CHANGED --&gt;
      &quot;user_id&quot;: &quot;&lt;BSUID&gt;&quot;                        &lt;!-- ADDED --&gt;
    &#125;
  ],
  &quot;messages&quot;: [
    &#123;
      &quot;id&quot;: &quot;&lt;WHATSAPP_MESSAGE_ID&gt;&quot;
    &#125;
  ]
&#125;
```

- `input` — New value (BSUID or parent BSUID).
  - Will return the user&#039;s phone number, if the message was sent to the user&#039;s phone number.
  - Will return the user&#039;s BSUID or parent BSUID, if it was sent to their BSUID or parent BSUID.
  - Will return the group ID, if sent to a group.
- `wa_id` — New behavior (can be omitted). Will return the user&#039;s phone number, if the message was sent to the user&#039;s phone number. Otherwise, it will be omitted.
- `user_id` — New property.
  - Will return the user&#039;s BSUID or parent BSUID, if the message was sent to the user&#039;s BSUID or parent BSUID.
  - Will be omitted if the message was sent to the user&#039;s phone number, including when both the user&#039;s phone number and BSUID or parent BSUID are included in the request (phone number takes precedence).

Example response to a send message request sent to a user&#039;s phone number (user BSUID or parent BSUID not used in request):

```json
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;contacts&quot;: [
    &#123;
      &quot;input&quot;: &quot;+16505551234&quot;,
      &quot;wa_id&quot;: &quot;16505551234&quot;
    &#125;
  ],
  &quot;messages&quot;: [
    &#123;
      &quot;id&quot;: &quot;wamid.HBgLMTY0NjcwNDM1OTUVAgARGBI1RjQyNUE3NEYxMzAzMzQ5MkEA&quot;
    &#125;
  ]
&#125;
```

Example response to a send message request sent to a user&#039;s BSUID (user phone number not used in request):

```json
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;contacts&quot;: [
    &#123;
      &quot;input&quot;: &quot;US.13491208655302741918&quot;,
      &quot;user_id&quot;: &quot;US.13491208655302741918&quot;
    &#125;
  ],
  &quot;messages&quot;: [
    &#123;
      &quot;id&quot;: &quot;wamid.HBgLMTY0NjcwNDM1OTUVAgARGBI1RjQyNUE3NEYxMzAzMzQ5MkEA&quot;
    &#125;
  ]
&#125;
```

Example response to a send message request sent to a user&#039;s phone number and BSUID (user phone number takes precedence):

```json
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;contacts&quot;: [
    &#123;
      &quot;input&quot;: &quot;+16505551234&quot;,
      &quot;wa_id&quot;: &quot;16505551234&quot;
    &#125;
  ],
  &quot;messages&quot;: [
    &#123;
      &quot;id&quot;: &quot;wamid.HBgLMTY0NjcwNDM1OTUVAgARGBI1RjQyNUE3NEYxMzAzMzQ5MkEA&quot;
    &#125;
  ]
&#125;
```

### Error codes

Adding new error code response to the [Messages API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/message-api#post-version-phone-number-id-messages).

- Error code — `131062`
- Details — `Business-scoped User ID (BSUID) recipients are not supported for this message.`

## Marketing Messages API for WhatsApp

### Send marketing message requests

Marketing Messages API for WhatsApp will support both phone numbers, BSUIDs, and parent BSUIDs. Sending messages to phone numbers is recommended, primarily so you can continue to receive phone numbers in webhooks.

These changes will apply to [Marketing Messages API](https://developers.facebook.com/documentation/business-messaging/whatsapp/marketing-messages/send-marketing-messages#send-marketing-template-messages) requests.

```html
&#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;BUSINESS_PHONE_NUMBER_ID&gt;/marketing_messages&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039; \
-d &#039;
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;recipient_type&quot;: &quot;individual&quot;,
  &quot;to&quot;: &quot;&lt;USER_PHONE_NUMBER&gt;&quot;,    &lt;!-- CHANGED --&gt;
  &quot;recipient&quot;: &quot;&lt;BSUID&gt;&quot;,         &lt;!-- ADDED --&gt;
  &quot;type&quot;: &quot;template&quot;,
  &quot;template&quot;: &#123;
    &lt;EXPECTED_TEMPLATE_PARAMETERS&gt;
  &#125;
&#125;&#039;
```

You can include both `to` (phone number) and `recipient` (BSUID or parent BSUID) in your request. If you do, `to` (phone number) will take precedence. If you prefer, you can also use one or the other:

To send a message using only the user&#039;s phone number:

- set `to` to the user&#039;s phone number
- omit the `recipient` property

To send a message using only the user&#039;s BSUID or parent BSUID:

- set `recipient` to the user&#039;s BSUID or parent BSUID
- omit the `to` property

### Send marketing message response

These changes apply to [Marketing Messages API](https://developers.facebook.com/documentation/business-messaging/whatsapp/marketing-messages/send-marketing-messages#send-marketing-template-messages) responses.

```html
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;contacts&quot;: [
    &#123;
      &quot;input&quot;: &quot;&lt;USER_PHONE_NUMBER_OR_ID&gt;&quot;,    &lt;!-- CHANGED --&gt;
      &quot;wa_id&quot;: &quot;&lt;USER_PHONE_NUMBER&gt;&quot;,          &lt;!-- CHANGED --&gt;
      &quot;user_id&quot;: &quot;&lt;BSUID&gt;&quot;                     &lt;!-- ADDED --&gt;
    &#125;
  ],
  &quot;messages&quot;: [
    &#123;
      &quot;id&quot;: &quot;&lt;WHATSAPP_MESSAGE_ID&gt;&quot;,
      &quot;message_status&quot;: &quot;&lt;PACING_STATUS&gt;&quot;
    &#125;
  ]
&#125;
```

- `input` — New value (BSUID or parent BSUID).
  - Will return the user&#039;s phone number, if the message was sent to the user&#039;s phone number.
  - Will return the user&#039;s BSUID or parent BSUID, if it was sent to their BSUID or parent BSUID.
  - Will return the group ID, if it was sent to a group.
- `wa_id` — Will return the user&#039;s phone number, if the message was sent to the user&#039;s phone number. Otherwise, it will be omitted.
- `user_id` — New property.
  - Will return the user&#039;s BSUID or parent BSUID, if the message was sent to the user&#039;s BSUID or parent BSUID.
  - Will be omitted if the message was sent to the user&#039;s phone number, including when both the user&#039;s phone number and BSUID or parent BSUID are included in the request (phone number takes precedence).

Example response to a send a template message to a user&#039;s phone number:

```json
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;contacts&quot;: [
    &#123;
      &quot;input&quot;: &quot;+16505551234&quot;,
      &quot;wa_id&quot;: &quot;16505551234&quot;
    &#125;
  ],
  &quot;messages&quot;: [
    &#123;
      &quot;id&quot;: &quot;wamid.HBgLMTY0NjcwNDM1OTUVAgARGBI1RjQyNUE3NEYxMzAzMzQ5MkEA&quot;,
      &quot;message_status&quot;: &quot;accepted&quot;
    &#125;
  ]
&#125;
```

Example response to a send a template message to a user&#039;s BSUID:

```json
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;contacts&quot;: [
    &#123;
      &quot;input&quot;: &quot;US.13491208655302741918&quot;,
      &quot;user_id&quot;: &quot;US.13491208655302741918&quot;
    &#125;
  ],
  &quot;messages&quot;: [
    &#123;
      &quot;id&quot;: &quot;wamid.HBgLMTY0NjcwNDM1OTUVAgARGBI1RjQyNUE3NEYxMzAzMzQ5MkEA&quot;,
      &quot;message_status&quot;: &quot;accepted&quot;
    &#125;
  ]
&#125;
```

## Webhook testing

You can test webhook payloads that reflect real-world username adoption scenarios using the **App Dashboard** &gt; **Use cases** (pencil icon) &gt; **Connect with customers through WhatsApp** &gt; **Customize** &gt; **Configuration** panel (**App Dashboard** &gt; **WhatsApp** &gt; **Configuration** for apps created before December, 2025). Click the **Test** link alongside the messages webhook to send a test messages webhook to your webhook endpoint.

The test tool supports incoming messages webhooks and status messages webhooks for sent messages, with the following scenarios:

- **User has not adopted a username** — Webhook payloads will include BSUID fields and phone number fields, but no username. This represents the default state for most users at launch.
- **User has adopted a username and phone number is unavailable** — Webhook payloads will include the username and BSUID fields, but phone number fields will be omitted. Your integration should handle this scenario gracefully. See [Phone numbers](#phone-numbers) for conditions under which phone numbers are included.
- **User has adopted a username and phone number is available** — Webhook payloads will include all fields: username, BSUID, and phone number.
- **Parent BSUID present** — For businesses with multiple portfolios enrolled in the same parent BSUID account, webhook payloads will include a [parent BSUID](#parent-business-scoped-user-ids) in addition to the portfolio-level BSUID.

## Webhook identifier quick reference

The following tables summarize which user identifiers will be included in messages webhooks, based on the type of webhook and whether the user has adopted a username.

### Outbound message status webhooks

These identifiers apply to sent, delivered, and read [status messages](#status-messages-webhooks) webhooks.

| Identifier | Sent to phone number | Sent to BSUID |
| :--- | :--- | :--- |
| `wa_id` | Always included | Included if phone number is available per [Phone numbers](#phone-numbers) conditions |
| `user_id` | Always included | Always included |
| `recipient_user_id` | Always included | Always included |
| `parent_user_id` | Included if parent BSUIDs enabled | Included if parent BSUIDs enabled |
| `recipient_parent_user_id` | Included if parent BSUIDs enabled | Included if parent BSUIDs enabled |
| `username` | Included in delivered/read if user has a username | Included in delivered/read if user has a username |

### Incoming messages webhooks

These identifiers apply to [incoming messages](#incoming-messages-webhooks) webhooks, including user-initiated messages and user replies.

| Identifier | User has a username | User does not have a username |
| :--- | :--- | :--- |
| `wa_id` | Included if phone number is available per [Phone numbers](#phone-numbers) conditions | Always included |
| `user_id` | Always included | Always included |
| `parent_user_id` | Included if parent BSUIDs enabled | Included if parent BSUIDs enabled |
| `username` | Always included | Not included |

## Messages webhooks

### Status messages webhooks

These changes will apply to sent, delivered, read, and failed [status messages](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/messages/status) webhooks.

```html
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;&quot;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;messaging_product&quot;: &quot;whatsapp&quot;,
            &quot;metadata&quot;: &#123;
              &quot;display_phone_number&quot;: &quot;&lt;BUSINESS_DISPLAY_PHONE_NUMBER&gt;&quot;,
              &quot;phone_number_id&quot;: &quot;&lt;BUSINESS_PHONE_NUMBER_ID&gt;&quot;
            &#125;,

            &lt;!-- Contacts will be included for sent, delivered, and read status --&gt;
            &quot;contacts&quot;: [                                      &lt;!-- ADDED --&gt;
              &#123;
                &quot;profile&quot;: &#123;
                  &quot;name&quot;: &quot;&lt;USER_DISPLAY_NAME&gt;&quot;,               &lt;!-- ADDED --&gt;

                  &lt;!-- Only included if user has enabled the username feature --&gt;
                  &quot;username&quot;: &quot;&lt;USERNAME&gt;&quot;                     &lt;!-- ADDED --&gt;

                &#125;,
                &quot;wa_id&quot;: &quot;&lt;USER_PHONE_NUMBER&gt;&quot;,                &lt;!-- ADDED --&gt;
                &quot;user_id&quot;: &quot;&lt;BSUID&gt;&quot;,                          &lt;!-- ADDED --&gt;

                &lt;!-- Only included if parent BSUIDs enabled --&gt;
                &quot;parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot;             &lt;!-- ADDED --&gt;
              &#125;
            ],

            &quot;statuses&quot;: [
              &#123;
                &quot;id&quot;: &quot;&lt;WHATSAPP_MESSAGE_ID&gt;&quot;,
                &quot;status&quot;: &quot;&lt;STATUS&gt;&quot;,
                &quot;timestamp&quot;: &quot;&lt;WEBHOOK_TRIGGER_TIMESTAMP&gt;&quot;,
                &quot;recipient_id&quot;: &quot;&lt;USER_PHONE_NUMBER&gt;&quot;,         &lt;!-- CHANGED --&gt;
                &quot;recipient_user_id&quot;: &quot;&lt;BSUID&gt;&quot;,                &lt;!-- ADDED --&gt;

                &lt;!-- Only included if parent BSUIDs enabled --&gt;
                &quot;recipient_parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot;   &lt;!-- ADDED --&gt;
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

- `contacts` — New array. Only included for sent, delivered, and read status messages. Will be omitted entirely for `failed` status messages webhooks.
  - `name` — New property. Value will be set to the WhatsApp user&#039;s display name.
  - `username` — New property.
    - Will be set to the WhatsApp user&#039;s username if the user has enabled the usernames feature.
    - Will be omitted entirely for `sent` status messages webhooks, or if the user has not enabled the usernames feature.
  - `wa_id` — New property.
    - Will be set to the user&#039;s phone number if the phone number can be included based on the conditions described in the [Phone numbers](#phone-numbers) section.
    - Will be omitted if the phone number cannot be included based on those conditions.
  - `user_id` — New property. Will be set to the WhatsApp user&#039;s BSUID.
  - `parent_user_id` — New property. Will be set to the user&#039;s [parent BSUID](#parent-business-scoped-user-ids) if you have enabled parent BSUIDs. Otherwise, the property will be omitted entirely.
- `statuses`
  - `recipient_id` — New behavior (can be omitted).
    - Will be set to the user&#039;s phone number, if you sent the message to the user&#039;s phone number.
    - Will be set to the group ID, if you sent the message to a group.
    - Will be omitted if you sent the message to the user&#039;s BSUID or parent BSUID and we are unable to include their phone number based on the conditions described in the [Phone numbers](#phone-numbers) section.
  - `recipient_user_id` — New property. Will always be set to the user&#039;s BSUID, regardless of whether the message was sent to the user&#039;s phone number or BSUID. For `failed` status messages, will be omitted if the message was sent to the user&#039;s phone number.
  - `recipient_parent_user_id` — New property. Will be set to the user&#039;s [parent BSUID](#parent-business-scoped-user-ids) if you have enabled parent BSUIDs. Otherwise, it will be omitted entirely.

Example delivered status messages webhook describing a message sent from a business that has enabled parent BSUIDS to the phone number of a WhatsApp user who has enabled the usernames feature:

```json
&#123;
 &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;102290129340398&quot;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;messaging_product&quot;: &quot;whatsapp&quot;,
            &quot;metadata&quot;: &#123;
              &quot;display_phone_number&quot;: &quot;15550783881&quot;,
              &quot;phone_number_id&quot;: &quot;106540352242922&quot;
            &#125;,
            &quot;contacts&quot;: [
              &#123;
                &quot;profile&quot;: &#123;
                  &quot;name&quot;: &quot;Pablo M.&quot;,
                  &quot;username&quot;: &quot;pablomorales&quot;
                &#125;,
                &quot;wa_id&quot;: &quot;16505551234&quot;,
                &quot;user_id&quot;: &quot;US.13491208655302741918&quot;,
                &quot;parent_user_id&quot;: &quot;US.ENT.11815799212886844830&quot;
              &#125;
            ],
            &quot;statuses&quot;: [
              &#123;
                &quot;id&quot;: &quot;wamid.HBgLMTY1MDM4Nzk0MzkVAgASGBQzQUFERjg0NDEzNDdFODU3MUMxMAA=&quot;,
                &quot;status&quot;: &quot;delivered&quot;,
                &quot;timestamp&quot;: &quot;1750030073&quot;,
                &quot;recipient_id&quot;: &quot;16505551234&quot;,
                &quot;recipient_user_id&quot;: &quot;US.13491208655302741918&quot;,
                &quot;recipient_parent_user_id&quot;: &quot;US.ENT.11815799212886844830&quot;,
                &quot;pricing&quot;: &#123;
                  &quot;billable&quot;: true,
                  &quot;pricing_model&quot;: &quot;PMP&quot;,
                  &quot;type&quot;: &quot;regular&quot;,
                  &quot;category&quot;: &quot;marketing&quot;
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

Example delivered status messages webhook describing a message sent from a business that has enabled parent BSUIDS, to the BSUID of a WhatsApp user who has enabled the username feature.
In this example, we are unable to include their phone number based on the conditions described in the [Phone numbers](#phone-numbers) section (so `wa_id` and `recipient_id` are omitted).

```json
&#123;
 &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;102290129340398&quot;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;messaging_product&quot;: &quot;whatsapp&quot;,
            &quot;metadata&quot;: &#123;
              &quot;display_phone_number&quot;: &quot;15550783881&quot;,
              &quot;phone_number_id&quot;: &quot;106540352242922&quot;
            &#125;,
            &quot;contacts&quot;: [
              &#123;
                &quot;profile&quot;: &#123;
                  &quot;name&quot;: &quot;Pablo M.&quot;,
                  &quot;username&quot;: &quot;pablomorales&quot;
                &#125;,
                &quot;user_id&quot;: &quot;US.13491208655302741918&quot;,
                &quot;parent_user_id&quot;: &quot;US.ENT.11815799212886844830&quot;
              &#125;
            ],
            &quot;statuses&quot;: [
              &#123;
                &quot;id&quot;: &quot;wamid.HBgLMTY1MDM4Nzk0MzkVAgASGBQzQUFERjg0NDEzNDdFODU3MUMxMAA=&quot;,
                &quot;status&quot;: &quot;delivered&quot;,
                &quot;timestamp&quot;: &quot;1750030073&quot;,
                &quot;recipient_user_id&quot;: &quot;US.13491208655302741918&quot;,
                &quot;recipient_parent_user_id&quot;: &quot;US.ENT.11815799212886844830&quot;,
                &quot;pricing&quot;: &#123;
                  &quot;billable&quot;: true,
                  &quot;pricing_model&quot;: &quot;PMP&quot;,
                  &quot;type&quot;: &quot;regular&quot;,
                  &quot;category&quot;: &quot;marketing&quot;
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

Example failed status messages webhook describing a message sent to a WhatsApp user&#039;s phone number. Note that the `contacts` array is omitted for failed status messages, `recipient_user_id` is omitted because the message was sent to a phone number, and an `errors` array is included in the `statuses` block:

```json
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;102290129340398&quot;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;messaging_product&quot;: &quot;whatsapp&quot;,
            &quot;metadata&quot;: &#123;
              &quot;display_phone_number&quot;: &quot;15550783881&quot;,
              &quot;phone_number_id&quot;: &quot;106540352242922&quot;
            &#125;,
            &quot;statuses&quot;: [
              &#123;
                &quot;id&quot;: &quot;wamid.HBgLMTY1MDM4Nzk0MzkVAgASGBQzQUFERjg0NDEzNDdFODU3MUMxMAA=&quot;,
                &quot;status&quot;: &quot;failed&quot;,
                &quot;timestamp&quot;: &quot;1750030073&quot;,
                &quot;recipient_id&quot;: &quot;16505551234&quot;,
                &quot;errors&quot;: [
                  &#123;
                    &quot;code&quot;: 131049,
                    &quot;title&quot;: &quot;This message was not delivered to maintain healthy ecosystem engagement.&quot;,
                    &quot;message&quot;: &quot;This message was not delivered to maintain healthy ecosystem engagement.&quot;,
                    &quot;error_data&quot;: &#123;
                      &quot;details&quot;: &quot;In order to maintain a healthy ecosystem engagement, the message failed to be delivered.&quot;
                    &#125;,
                    &quot;href&quot;: &quot;/documentation/business-messaging/whatsapp/support/error-codes&quot;
                  &#125;
                ]
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

### Incoming messages webhooks

These changes apply to incoming messages webhooks ([text](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/messages/text), [image](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/messages/image), [interactive](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/messages/interactive), and so on), including incoming messages sent by users in a Group chat.

The example syntax below is for an incoming **text** message, but the changes are the same for all incoming message types.

```html
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;&quot;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;messaging_product&quot;: &quot;whatsapp&quot;,
            &quot;metadata&quot;: &#123;
              &quot;display_phone_number&quot;: &quot;&lt;BUSINESS_DISPLAY_PHONE_NUMBER&gt;&quot;,
              &quot;phone_number_id&quot;: &quot;&lt;BUSINESS_PHONE_NUMBER_ID&gt;&quot;
            &#125;,
            &quot;contacts&quot;: [
              &#123;
                &quot;profile&quot;: &#123;
                  &quot;name&quot;: &quot;&lt;WHATSAPP_USER_PROFILE_NAME&gt;&quot;,

                  &lt;!-- Only included if user has enabled the username feature --&gt;
                  &quot;username&quot;: &quot;&lt;USERNAME&gt;&quot;                 &lt;!-- ADDED --&gt;
                &#125;,
                &quot;wa_id&quot;: &quot;&lt;WHATSAPP_USER_ID&gt;&quot;,             &lt;!-- CHANGED --&gt;
                &quot;user_id&quot;: &quot;&lt;BSUID&gt;&quot;,                      &lt;!-- ADDED --&gt;

                &lt;!-- Only included if parent BSUIDs enabled --&gt;
                &quot;parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot;         &lt;!-- ADDED --&gt;
              &#125;
            ],
            &quot;messages&quot;: [
              &#123;
                &quot;from&quot;: &quot;&lt;WHATSAPP_USER_PHONE_NUMBER&gt;&quot;,    &lt;!-- CHANGED --&gt;
                &quot;from_user_id&quot;: &quot;&lt;BSUID&gt;&quot;,                 &lt;!-- ADDED --&gt;

                &lt;!-- Only included if parent BSUIDs enabled --&gt;
                &quot;from_parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot;,   &lt;!-- ADDED --&gt;

                &lt;!-- Only included if incoming message sent in a group --&gt;
                &quot;group_id&quot;: &quot;&lt;GROUP_ID&gt;&quot;,

                &quot;id&quot;: &quot;&lt;WHATSAPP_MESSAGE_ID&gt;&quot;,
                &quot;timestamp&quot;: &quot;&lt;WEBHOOK_TRIGGER_TIMESTAMP&gt;&quot;,
                &quot;type&quot;: &quot;text&quot;,
                &quot;text&quot;: &#123;
                  &quot;body&quot;: &quot;&lt;MESSAGE_TEXT_BODY&gt;&quot;
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

- `contacts`
  - `profile`
    - `username` — New property.
      - Will be set to the user&#039;s username, if the user has enabled the username feature.
      - Will be omitted if the user has not adopted a username.
  - `wa_id` — New behavior (can be omitted).
    - Will be set to the user&#039;s phone number if the phone number can be included based on the conditions described in the [Phone numbers](#phone-numbers) section.
    - Will be omitted if the phone number cannot be included based on those conditions.
  - `user_id` — New property, set to the user&#039;s BSUID.
  - `parent_user_id` — New property. Will be set to the user&#039;s parent BSUID, if you have enabled parent BSUIDs. Otherwise, it will be omitted.
- `messages`
  - `from` — New behavior (can be omitted).
    - Will be set to the user&#039;s phone number if the phone number can be included based on the conditions described in the [Phone numbers](#phone-numbers) section.
    - Will be omitted if the phone number cannot be included based on those conditions.
  - `from_user_id` — New property, set to the user&#039;s BSUID.
  - `from_parent_user_id` — New property, set to the user&#039;s parent BSUID, if you have enabled parent BSUIDs. Otherwise, it will be omitted.

Example incoming text message from a user who has enabled the username feature, to a business that has enabled [parent BSUIDs](#parent-business-scoped-user-ids). In this scenario, we are unable to include their phone number based on the conditions described in the [Phone numbers](#phone-numbers) section.

```json
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;102290129340398&quot;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;messaging_product&quot;: &quot;whatsapp&quot;,
            &quot;metadata&quot;: &#123;
              &quot;display_phone_number&quot;: &quot;15550783881&quot;,
              &quot;phone_number_id&quot;: &quot;106540352242922&quot;
            &#125;,
            &quot;contacts&quot;: [
              &#123;
                &quot;profile&quot;: &#123;
                  &quot;name&quot;: &quot;Sheena Nelson&quot;,
                  &quot;username&quot;: &quot;realsheenanelson&quot;
                &#125;,
                &quot;user_id&quot;: &quot;US.13491208655302741918&quot;,
                &quot;parent_user_id&quot;: &quot;US.ENT.11815799212886844830&quot;
              &#125;
            ],
            &quot;messages&quot;: [
              &#123;
                &quot;from_user_id&quot;: &quot;US.13491208655302741918&quot;,
                &quot;from_parent_user_id&quot;: &quot;US.ENT.11815799212886844830&quot;,
                &quot;id&quot;: &quot;wamid.HBgLMTY1MDM4Nzk0MzkVAgASGBQzQTRBNjU5OUFFRTAzODEwMTQ0RgA=&quot;,
                &quot;timestamp&quot;: &quot;1749416383&quot;,
                &quot;type&quot;: &quot;text&quot;,
                &quot;text&quot;: &#123;
                  &quot;body&quot;: &quot;Does it come in another color?&quot;
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

### System messages webhooks

These changes apply to [system](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/messages/system) messages webhooks. A new trigger has been added: a system messages webhook will now be triggered when a WhatsApp user changes their phone number using the WhatsApp consumer app.

```html
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;&quot;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;messaging_product&quot;: &quot;whatsapp&quot;,
            &quot;metadata&quot;: &#123;
              &quot;display_phone_number&quot;: &quot;&lt;BUSINESS_DISPLAY_PHONE_NUMBER&gt;&quot;,
              &quot;phone_number_id&quot;: &quot;&lt;BUSINESS_PHONE_NUMBER_ID&gt;&quot;
            &#125;,
            &quot;messages&quot;: [
              &#123;
                &quot;from&quot;: &quot;&lt;WHATSAPP_USER_PHONE_NUMBER&gt;&quot;,
                &quot;id&quot;: &quot;&lt;WHATSAPP_MESSAGE_ID&gt;&quot;,
                &quot;timestamp&quot;: &quot;&lt;WEBHOOK_TRIGGER_TIMESTAMP&gt;&quot;,
                &quot;type&quot;: &quot;system&quot;,
                &quot;system&quot;: &#123;
                  &quot;body&quot;: &quot;User...&quot;,                       &lt;!-- CHANGED --&gt;
                  &quot;wa_id&quot;: &quot;&lt;NEW_WHATSAPP_USER_ID&gt;&quot;,       &lt;!-- CHANGED --&gt;
                  &quot;user_id&quot;: &quot;&lt;NEW_BSUID&gt;&quot;,                &lt;!-- ADDED --&gt;

                  &lt;!-- Only included if parent BSUIDs enabled --&gt;
                  &quot;parent_user_id&quot;: &quot;&lt;NEW_PARENT_BSUID&gt;&quot;,  &lt;!-- ADDED --&gt;
                  &quot;type&quot;: &quot;&lt;SYSTEM_CHANGE_TYPE&gt;&quot;           &lt;!-- CHANGED --&gt;
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
- `system`
  - `body` — New string. Will be set to `User &lt;WHATSAPP_USER_PROFILE_NAME&gt; changed from &lt;OLD_BSUID&gt; to &lt;NEW_BSUID&gt;` if the user changed their business phone number.
  - `wa_id` — New behavior (can be omitted).
    - Will be omitted if the user has enabled the username feature and we are unable to include their phone number based on the conditions described in the [Phone numbers](#phone-numbers) section.
    - Will be set to the user&#039;s phone number if the user has not enabled the usernames feature.
  - `user_id` — New property. Will be set to the user&#039;s new BSUID.
  - `parent_user_id` — New property. Will be set to the user&#039;s new [parent BSUID](#parent-business-scoped-user-ids), if you have enabled parent BSUIDs. Otherwise, it will be omitted.
  `type` — New value (`user_changed_user_id`). Will be set to `user_changed_user_id` if the WhatsApp user changed their phone number.

### user_preferences webhooks

These changes will apply to [user_preferences](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/user_preferences) webhooks.

```html
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;&quot;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;messaging_product&quot;: &quot;whatsapp&quot;,
            &quot;metadata&quot;: &#123;
              &quot;display_phone_number&quot;: &quot;&lt;BUSINESS_DISPLAY_PHONE_NUMBER&gt;&quot;,
              &quot;phone_number_id&quot;: &quot;&lt;BUSINESS_PHONE_NUMBER_ID&gt;&quot;
            &#125;,
            &quot;contacts&quot;: [
              &#123;
                &quot;profile&quot;: &#123;
                  &quot;name&quot;: &quot;&lt;WHATSAPP_USER_NAME&gt;&quot;,

                  &lt;!-- Only included if user has enabled the usernames feature --&gt;
                  &quot;username&quot;: &quot;&lt;USERNAME&gt;&quot;                 &lt;!-- ADDED --&gt;
                &#125;,
                &quot;wa_id&quot;: &quot;&lt;WHATSAPP_USER_ID&gt;&quot;,             &lt;!-- CHANGED --&gt;
                &quot;user_id&quot;: &quot;&lt;BSUID&gt;&quot;,                      &lt;!-- ADDED --&gt;

                &lt;!-- Only included if you have enabled parent BSUIDs --&gt;
                &quot;parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot;         &lt;!-- ADDED --&gt;
              &#125;
            ],
            &quot;user_preferences&quot;: [
              &#123;
                &quot;wa_id&quot;: &quot;&lt;WHATSAPP_USER_ID&gt;&quot;,             &lt;!-- CHANGED --&gt;
                &quot;user_id&quot;: &quot;&lt;BSUID&gt;&quot;,                      &lt;!-- ADDED --&gt;

                &lt;!-- Only included if you have enabled parent BSUIDs --&gt;
                &quot;parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot;,        &lt;!-- ADDED --&gt;

                &quot;detail&quot;: &quot;&lt;PREFERENCE_DESCRIPTION&gt;&quot;,
                &quot;category&quot;: &quot;marketing_messages&quot;,
                &quot;value&quot;: &quot;&lt;PREFERENCE&gt;&quot;,
                &quot;timestamp&quot;: &lt;WEBHOOK_SENT_TIMESTAMP&gt;
              &#125;
            ]
          &#125;,
          &quot;field&quot;: &quot;user_preferences&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

- `contacts`
    - `profile`
        - `username` — New property. Will be set to the user&#039;s username, if the user has enabled the username feature. Property omitted if the user has disabled the username feature.
    - `wa_id` — New behavior (can be omitted).
        - Will be omitted if the user has enabled the username feature and we are unable to include their phone number based on the conditions described in the [Phone numbers](#phone-numbers) section.
        - Will be set to the user&#039;s phone number if the user has not enabled the usernames feature.
    - `user_id` — New property. Will be set to the user&#039;s BSUID.
    - `parent_user_id` — New property. Will be set to the user&#039;s [parent BSUID](#parent-business-scoped-user-ids), if you have enabled parent BSUIDs. Otherwise, it will be omitted.
- `user_preferences`
    - `wa_id` — New behavior (can be omitted). Will be omitted if the user has enabled the username feature and we are unable to include their phone number based on the conditions described in the [Phone numbers](#phone-numbers) section. Otherwise, it will be set to the user&#039;s phone number.
    - `user_id` — New property. Will be set to the user&#039;s BSUID.
    - `parent_user_id` — New property. Will be set to the user&#039;s [parent BSUID](#parent-business-scoped-user-ids), if you have enabled parent BSUIDs. Otherwise, it will be omitted.

### user_id_update webhooks

A new **user_id_update** webhook will be triggered when a WhatsApp user&#039;s BSUID changes. Subscribe your apps to this webhook field to be notified of BSUID changes.

```html
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;&quot;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;messaging_product&quot;: &quot;whatsapp&quot;,
            &quot;metadata&quot;: &#123;
              &quot;display_phone_number&quot;: &quot;&lt;BUSINESS_DISPLAY_PHONE_NUMBER&gt;&quot;,
              &quot;phone_number_id&quot;: &quot;&lt;BUSINESS_PHONE_NUMBER_ID&gt;&quot;
            &#125;,
            &quot;contacts&quot;: [
              &#123;
                &quot;profile&quot;: &#123;
                  &quot;name&quot;: &quot;&lt;WHATSAPP_USER_NAME&gt;&quot;
                &#125;,
                &quot;wa_id&quot;: &quot;&lt;WHATSAPP_USER_ID&gt;&quot;
              &#125;
            ],
            &quot;user_id_update&quot;: [
              &#123;
                &quot;wa_id&quot;: &quot;&lt;WHATSAPP_USER_ID&gt;&quot;,
                &quot;detail&quot;: &quot;User id for &lt;WHATSAPP_USER_PROFILE_NAME&gt; has been updated.&quot;,
                &quot;user_id&quot;: &#123;
                  &quot;previous&quot;: &quot;&lt;OLD_BSUID&gt;&quot;,
                  &quot;current&quot;: &quot;&lt;NEW_BSUID&gt;&quot;
                &#125;,

                &lt;!-- Only included if parent BSUIDs enabled --&gt;
                &quot;parent_user_id&quot;: &#123;
                  &quot;previous&quot;: &quot;&lt;OLD_PARENT_BSUID&gt;&quot;,
                  &quot;current&quot;: &quot;&lt;NEW_PARENT_BSUID&gt;&quot;
                &#125;,

                &quot;timestamp&quot;: &quot;&lt;WEBHOOK_SENT_TIMESTAMP&gt;&quot;
              &#125;
            ]
          &#125;,
          &quot;field&quot;: &quot;user_id_update&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

- `contacts`
    - `wa_id` — Will be set to the user&#039;s phone number if available. Will be omitted if the user has enabled the username feature and we are unable to include their phone number based on the conditions described in the [Phone numbers](#phone-numbers) section.
- `user_id_update`
    - `wa_id` — Will be set to the user&#039;s phone number if available. Will be omitted if the user has enabled the username feature and we are unable to include their phone number based on the conditions described in the [Phone numbers](#phone-numbers) section.
    - `detail` — A human-readable description of the update.
    - `user_id` — Object containing the user&#039;s previous and current BSUID.
        - `previous` — The user&#039;s old BSUID.
        - `current` — The user&#039;s new BSUID.
    - `parent_user_id` — Object containing the user&#039;s previous and current [parent BSUID](#parent-business-scoped-user-ids), if you have enabled parent BSUIDs. Otherwise, it will be omitted.
        - `previous` — The user&#039;s old parent BSUID.
        - `current` — The user&#039;s new parent BSUID.
    - `timestamp` — Unix timestamp indicating when the webhook was sent.

## Groups API

### Get group info

These changes apply to [Group API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/groups/groups-query-api#get-version-group-id) responses.

```html
&#123;
  &quot;participants&quot;: [
    &#123;
      &quot;wa_id&quot;: &quot;&lt;USER_PHONE_NUMBER&gt;&quot;        &lt;!-- CHANGED --&gt;
      &quot;user_id&quot;: &quot;&lt;BSUID&gt;&quot;,                 &lt;!-- ADDED --&gt;

      &lt;!-- Only returned if the you have enabled parent BSUIDs --&gt;
      &quot;parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot;,   &lt;!-- ADDED --&gt;

      &lt;!-- Only returned if the user has enabled the usernames feature --&gt;
      &quot;username&quot;: &quot;&lt;USERNAME&gt;&quot;              &lt;!-- ADDED --&gt;

    &#125;
  ],
  &quot;subject&quot;: &quot;&lt;GROUP_SUBJECT&gt;&quot;,
  &quot;id&quot;: &quot;&lt;GROUP_ID&gt;&quot;,
  &quot;messaging_product&quot;: &quot;whatsapp&quot;
&#125;
```

- `wa_id` — New behavior (can be omitted).
    - Will be omitted if the user has enabled the username feature and we are unable to include their phone number based on the conditions described in the [Phone numbers](#phone-numbers) section.
    - Will be set to the user&#039;s phone number if the user has not enabled the usernames feature.
- `user_id` — New property. Will be set to the user&#039;s BSUID.
- `parent_user_id` — New property. Will be set to the user&#039;s [parent BSUID](#parent-business-scoped-user-ids), if you have enabled parent BSUIDs. Otherwise, it will be omitted.
- `username` — New property.
    - Will be set to the user&#039;s username, if the user has enabled the username feature.
    - Will be omitted if the user is not using, or has disabled, the username feature.


### Get group join requests

These changes apply to [Join Requests API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/groups/groups-join-requests-api#get-version-group-id-join-requests) responses.

```html
&#123;
  &quot;data&quot;: [
    &#123;
      &quot;join_request_id&quot;: &quot;&lt;JOIN_REQUEST_ID&gt;&quot;,
      &quot;creation_timestamp&quot;: &quot;&lt;JOIN_REQUEST_TIMESTAMP&gt;&quot;,
      &quot;wa_id&quot;: &quot;&lt;USER_PHONE_NUMBER&gt;&quot;,                    &lt;!-- CHANGED --&gt;
      &quot;user_id&quot;: &quot;&lt;BSUID&gt;&quot;,                              &lt;!-- ADDED --&gt;

      &lt;!-- Only included if parent BSUIDs enabled --&gt;
      &quot;parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot;,                &lt;!-- ADDED --&gt;

      &lt;!-- Only included if user has enabled usernames feature --&gt;
      &quot;username&quot;: &quot;&lt;USERNAME&gt;&quot;                           &lt;!-- ADDED --&gt;
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

- `wa_id` — New behavior (can be omitted).
    - Will be omitted if the user has enabled the username feature and we are unable to include their phone number based on the conditions described in the [Phone numbers](#phone-numbers) section.
    - Will be set to the user&#039;s phone number if the user has not enabled the usernames feature.
- `user_id` — New property. Will be set to the user&#039;s BSUID.
- `parent_user_id` — New property. Will be set to the user&#039;s [parent BSUID](#parent-business-scoped-user-ids), if you have enabled parent BSUIDs. Otherwise, it will be omitted.
- `username` — New property. Will be set to the user&#039;s username, if the user has enabled the username feature. Will be omitted if the user has not enabled the username feature.

### Remove group participants

These changes apply to [Participants API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/groups/groups-participants-api#delete-version-group-id-participants) requests.

```html
curl -g -X DELETE &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;GROUP_ID&gt;/participants&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039; \
-d &#039;
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;participants&quot;: [
    &#123;
      &quot;user&quot;: &quot;&lt;USER_PHONE_NUMBER&gt;&quot;,   &lt;!-- CHANGED --&gt;
      &quot;user_id&quot;: &quot;&lt;BSUID&gt;&quot;            &lt;!-- ADDED --&gt;
    &#125;
  ]
&#125;&#039;
```

- `user` — The user&#039;s phone number.
- `user_id` — New property. The user&#039;s BSUID.

Include `user` or `user_id`, but not both.

## Groups API webhooks

### Status messages webhooks for groups

These changes will apply to `delivered` and `read` [status messages](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/messages/status) webhooks for messages sent to a group.

```html
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;&quot;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;messaging_product&quot;: &quot;whatsapp&quot;,
            &quot;metadata&quot;: &#123;
              &quot;display_phone_number&quot;: &quot;&lt;BUSINESS_DISPLAY_PHONE_NUMBER&gt;&quot;,
              &quot;phone_number_id&quot;: &quot;&lt;BUSINESS_PHONE_NUMBER_ID&gt;&quot;
            &#125;,

           &lt;!-- Contacts will be included for delivered and read status --&gt;
           &quot;contacts&quot;: [                             &lt;!-- ADDED --&gt;
                &#123;
                  &quot;profile&quot;: &#123;
                    &quot;name&quot;: &quot;&lt;USER_DISPLAY_NAME&gt;&quot;,   &lt;!-- ADDED --&gt;

                    &lt;!-- Only included if user has enabled usernames feature --&gt;
                    &quot;username&quot;: &quot;&lt;USERNAME&gt;&quot;         &lt;!-- ADDED --&gt;
                  &#125;,
                  &quot;wa_id&quot;: &quot;&lt;USER_PHONE_NUMBER&gt;&quot;,    &lt;!-- ADDED --&gt;
                  &quot;user_id&quot;: &quot;&lt;BSUID&gt;&quot;,              &lt;!-- ADDED --&gt;

                  &lt;!-- Only included if parent BSUIDs enabled --&gt;
                  &quot;parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot;
                &#125;,
                # Additional contact objects would follow, if aggregated
                &#123;
                  ...
                &#125;
              ],

            &quot;statuses&quot;: [
              &#123;
                &quot;id&quot;: &quot;&lt;WHATSAPP_MESSAGE_ID&gt;&quot;,
                &quot;status&quot;: &quot;&lt;STATUS&gt;&quot;,
                &quot;timestamp&quot;: &quot;&lt;WEBHOOK_TRIGGER_TIMESTAMP&gt;&quot;,
                &quot;recipient_id&quot;: &quot;&lt;GROUP_ID&gt;&quot;,
                &quot;recipient_type&quot;: &quot;group&quot;,
                &quot;recipient_participant_id&quot;: &quot;&lt;GROUP_PARTICIPANT_USER_PHONE_NUMBER&gt;&quot;, &lt;!-- CHANGED --&gt;
                &quot;recipient_participant_user_id&quot;: &quot;&lt;BSUID&gt;&quot;,                &lt;!-- ADDED --&gt;

                &lt;!-- Only included if parent BSUIDs enabled --&gt;
                &quot;recipient_participant_parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot;,  &lt;!-- ADDED --&gt;

                &lt;!-- Omitted for v24.0+ unless webhook is for a free entry point conversation --&gt;
                &quot;conversation&quot;: &#123;
                  &quot;id&quot;: &quot;&lt;CONVERSATION_ID&gt;&quot;,
                  &quot;expiration_timestamp&quot;: &quot;&lt;CONVERSATION_EXPIRATION_TIMESTAMP&gt;&quot;,
                  &quot;origin&quot;: &#123;
                    &quot;type&quot;: &quot;&lt;CONVERSATION_CATEGORY&gt;&quot;
                  &#125;
                &#125;,

                &quot;pricing&quot;: &#123;
                  &quot;billable&quot;: &lt;IS_BILLABLE?&gt;,
                  &quot;pricing_model&quot;: &quot;&lt;PRICING_MODEL&gt;&quot;,
                  &quot;type&quot;: &quot;&lt;PRICING_TYPE&gt;&quot;,
                  &quot;category&quot;: &quot;&lt;PRICING_CATEGORY&gt;&quot;
                &#125;
              &#125;,
              # Additional status objects would follow, if aggregated
              &#123;
                ...
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

- `contacts` — New array. Only included for delivered and read status messages. Will be omitted entirely for failed status messages webhooks.
    - `name` — New property. Value will be set to the WhatsApp user&#039;s display name.
    - `username` — New property. Will be set to the WhatsApp user&#039;s username if the user has adopted a username. Will be omitted for sent status messages webhooks, or if the user has not enabled the usernames feature.
    - `wa_id` — New property.
        - Will be omitted if the user has adopted a username and we are unable to include their phone number based on the conditions described in the [Phone numbers](#phone-numbers) section.
        - Will be set to the user&#039;s phone number, if you sent the message to the user&#039;s phone number.
    - `user_id` — New property. Will be set to the WhatsApp user&#039;s BSUID.
    - `parent_user_id` — New property. Will be set to the user&#039;s [parent BSUID](#parent-business-scoped-user-ids) if you have enabled parent BSUIDs. Otherwise, it will be omitted.
- `recipient_participant_id` — Changed. Will be set to the user&#039;s phone number, if the message was sent to their phone number. Otherwise, it will be omitted.
- `recipient_participant_user_id` — New property. Will always be set to the user&#039;s BSUID, regardless of whether the message was sent to the user&#039;s phone number or BSUID. For `failed` status messages, will be omitted if the message was sent to the user&#039;s phone number.
- `recipient_participant_parent_user_id` — New property. Will be set to the user&#039;s parent BSUID if you have enabled parent BSUIDs. Otherwise, it will be omitted.

### group_participants_update webhooks

These changes apply to the [group_participants_update](https://developers.facebook.com/documentation/business-messaging/whatsapp/groups/webhooks#group-participants-update-webhooks) webhook.

```html
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;&quot;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;messaging_product&quot;: &quot;whatsapp&quot;,
            &quot;metadata&quot;: &#123;
              &quot;display_phone_number&quot;: &quot;&lt;BUSINESS_DISPLAY_PHONE_NUMBER&gt;&quot;,
              &quot;phone_number_id&quot;: &quot;&lt;BUSINESS_PHONE_NUMBER_ID&gt;&quot;
            &#125;,
            &quot;groups&quot;: [
              &#123;
                &quot;timestamp&quot;: &lt;WEBHOOK_TRIGGER_TIMESTAMP&gt;,
                &quot;group_id&quot;: &quot;&lt;GROUP_ID&gt;&quot;,

                &lt;!-- Only if business removes participant from group --&gt;
                &quot;type&quot;: &quot;group_participants_remove&quot;,
                &quot;request_id&quot;: &quot;REQUEST_ID&quot;,
                &quot;removed_participants&quot;: [
                  &#123;
                    &quot;input&quot;: &quot;&lt;USER_PHONE_NUMBER_OR_BSUID&gt;&quot;, &lt;!-- CHANGED --&gt;
                  &#125;
                ],

                &quot;initiated_by&quot;: &quot;business&quot;

                &lt;!-- Only if user removes themself from group --&gt;
                &quot;type&quot;: &quot;group_participants_remove&quot;,
                &quot;request_id&quot;: &quot;REQUEST_ID&quot;,
                &quot;removed_participants&quot;: [
                  &#123;
                    &quot;wa_id&quot;: &quot;&lt;USER_PHONE_NUMBER&gt;&quot;       &lt;!-- CHANGED --&gt;
                    &quot;user_id&quot;: &quot;&lt;BSUID&gt;&quot;,                &lt;!-- ADDED --&gt;
                    &quot;parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot;,  &lt;!-- ADDED --&gt;
                    &quot;username&quot;: &quot;&lt;USERNAME&gt;&quot;             &lt;!-- ADDED --&gt;
                  &#125;
                ],

                &quot;initiated_by&quot;: &quot;participant&quot;

                &lt;!-- Only if user joins group via invite link --&gt;
                &quot;type&quot;: &quot;group_participants_add&quot;,
                &quot;reason&quot;: &quot;invite_link&quot;,
                &quot;added_participants&quot;: [
                  &#123;
                    &quot;wa_id&quot;: &quot;&lt;USER_PHONE_NUMBER&gt;&quot;       &lt;!-- CHANGED --&gt;
                    &quot;user_id&quot;: &quot;&lt;BSUID&gt;&quot;,                &lt;!-- ADDED --&gt;
                    &quot;parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot;,  &lt;!-- ADDED --&gt;
                    &quot;username&quot;: &quot;&lt;USERNAME&gt;&quot;             &lt;!-- ADDED --&gt;
                  &#125;
                ]

                &lt;!-- Only if join request created --&gt;
                &quot;type&quot;: &quot;group_join_request_created&quot;,
                &quot;join_request_id&quot;: &quot;&lt;JOIN_REQUEST_ID&gt;&quot;,
                &quot;wa_id&quot;: &quot;&lt;USER_PHONE_NUMBER&gt;&quot;,          &lt;!-- CHANGED --&gt;
                &quot;user_id&quot;: &quot;&lt;BSUID&gt;&quot;,                    &lt;!-- ADDED --&gt;
                &quot;parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot;,      &lt;!-- ADDED --&gt;
                &quot;username&quot;: &quot;&lt;USERNAME&gt;&quot;                 &lt;!-- ADDED --&gt;

                &lt;!-- Only if join request revoked --&gt;
                &quot;type&quot;: &quot;group_join_request_revoked&quot;,
                &quot;join_request_id&quot;: &quot;&lt;JOIN_REQUEST_ID&gt;&quot;,
                &quot;wa_id&quot;: &quot;&lt;USER_PHONE_NUMBER&gt;&quot;           &lt;!-- CHANGED --&gt;
                &quot;user_id&quot;: &quot;&lt;BSUID&gt;&quot;,                    &lt;!-- ADDED --&gt;
                &quot;parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot;,      &lt;!-- ADDED --&gt;
                &quot;username&quot;: &quot;&lt;USERNAME&gt;&quot;                 &lt;!-- ADDED --&gt;
              &#125;
            ]
          &#125;,
          &quot;field&quot;: &quot;group_participants_update&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```
- `input` — New value (phone number or BSUID).
    - Will be set to the user&#039;s phone number if you removed the user from the group using their phone number.
    - Will be set to the user&#039;s BSUID if you removed the user from the group using their BSUID.
- `wa_id` — New behavior (can be omitted).
    - Will be omitted if the user has enabled the username feature and we are unable to include their phone number based on the conditions described in the [Phone numbers](#phone-numbers) section. Otherwise, it will be set to the user&#039;s phone number.
- `user_id` — New property. Will be set to the user&#039;s BSUID.
- `parent_user_id` — New property. Will be set to the user&#039;s [parent BSUID](#parent-business-scoped-user-ids) if you have enabled parent BSUIDs. Otherwise, it will be omitted.
- `username` — New property. Will be set to the user&#039;s username, if the user has enabled the username feature. Otherwise, it will be omitted.

## Block Users API

### Block or unblock user requests

These changes apply to the POST and DELETE [Block Users](https://developers.facebook.com/documentation/business-messaging/whatsapp/block-users) requests. This example is for a block user request syntax, but the changes also apply to unblock requests.

```html
curl &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;BUSINESS_PHONE_NUMBER_ID&gt;/block_users&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039; \
-d &#039;
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;block_users&quot;: [
    &#123;
      &quot;user&quot;: &quot;&lt;USER_PHONE_NUMBER&gt;&quot;
    &#125;,
    &#123;
      &quot;user_id&quot;: &quot;&lt;BSUID&gt;&quot;   &lt;!-- ADDED --&gt;
    &#125;
  ]
&#125;&#039;
```

You can include both `user` (phone number) and `user_id` (BSUID) in your request. If you do, `user` (phone number) will take precedence. If you prefer, you can also use one or the other:

To block or unblock a user using only their phone number:

- Set `user` to the user&#039;s phone number
- Omit the `user_id` object

To block or unblock a user using only their BSUID:

- Set `user_id` to the user&#039;s BSUID
- Omit the `user` object

Parent BSUIDs are not supported for blocking or unblocking users. If you attempt to use a parent BSUID, the request will fail.


### Block or unblock request responses

These changes will apply to the POST and DELETE [Block Users](https://developers.facebook.com/documentation/business-messaging/whatsapp/block-users) request responses.

```html
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;block_users&quot;: &#123;
    &quot;added_users&quot;: [
      &#123;
        &quot;input&quot;: &quot;&lt;USER_PHONE_NUMBER&gt;&quot;,  &lt;!-- CHANGED --&gt;
        &quot;wa_id&quot;: &quot;&lt;USER_PHONE_NUMBER&gt;&quot;,  &lt;!-- CHANGED --&gt;
        &quot;user_id&quot;: &quot;&lt;BSUID&gt;&quot;             &lt;!-- ADDED --&gt;
      &#125;
    ]
  &#125;
&#125;
```

- `input` — New value (BSUID).
    - Will be set to the user&#039;s BSUID if you used the user&#039;s BSUID to block or unblock the user.
    - Will be set to the user&#039;s phone number if you used the user&#039;s phone number to block or unblock the user.
- `wa_id` — New behavior (can be omitted).
    - Will be omitted if you used the user&#039;s BSUID to block or unblock the user.
    - Will be set to the user&#039;s phone number if you used their phone number to block or unblock the user.
- `user_id` — New property.
    - Will be set to the user&#039;s BSUID if you used the user&#039;s BSUID to block or unblock the user.
    - Will be omitted if you used the user&#039;s phone number to block or unblock the user.

### Get blocked users

These changes apply to GET [Block Users](https://developers.facebook.com/documentation/business-messaging/whatsapp/block-users) responses.

Response syntax:

```html
&#123;
  &quot;data&quot;: [
    &#123;
      &quot;messaging_product&quot;: &quot;whatsapp&quot;,
      &quot;wa_id&quot;: &quot;&lt;USER_PHONE_NUMBER&gt;&quot;,    &lt;!-- CHANGED --&gt;
      &quot;user_id&quot;: &quot;&lt;BSUID&gt;&quot;,             &lt;!-- ADDED --&gt;

      &lt;!-- Only included if parent BSUIDs enabled --&gt;
      &quot;parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot; &lt;!-- ADDED --&gt;
    &#125;
  ],
  &quot;paging&quot;: &#123;
    &quot;cursors&quot;: &#123;
      &quot;after&quot;: &quot;&lt;AFTER_CURSOR&gt;&quot;,
      &quot;before&quot;: &quot;&lt;BEFORE_CURSOR&gt;&quot;
    &#125;
  &#125;
&#125;
```

- `wa_id` — Will be set to the user&#039;s phone number if available. Will be omitted if the user has enabled the username feature and we are unable to include their phone number based on the conditions described in the [Phone numbers](#phone-numbers) section.
- `user_id` — New property. Will be set to the user&#039;s BSUID.
- `parent_user_id` — New property. Will be set to the user&#039;s [parent BSUID](#parent-business-scoped-user-ids) if you have enabled parent BSUIDs. Otherwise, it will be omitted.

## Calling API

### Businesses-initiated call requests

The changes apply to business-initiated Calling API requests.

```html
&#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;BUSINESS_PHONE_NUMBER_ID&gt;/calls&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039; \
-d &#039;
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;to&quot;: &quot;&lt;USER_PHONE_NUMBER&gt;&quot;,    &lt;!-- CHANGED --&gt;
  &quot;recipient&quot;: &quot;&lt;BSUID&gt;&quot;,         &lt;!-- ADDED --&gt;
  &quot;action&quot;: &quot;connect&quot;,
  &quot;session&quot;: &#123;
    &quot;sdp_type&quot;: &quot;offer&quot;,
    &quot;sdp&quot;: &quot;&lt;RFC_4566_SDP&gt;&quot;
  &#125;
&#125;&#039;
```

You can include both `to` (phone number) and `recipient` (BSUID or parent BSUID) in your request. If you do, `to` (phone number) will take precedence. If you prefer, you can also use one or the other:

To call a user using only their phone number:

- set `to` to the user&#039;s phone number
- omit the `recipient` property

To call a user using only their BSUID or parent BSUID:

- set `recipient` to the user&#039;s BSUID or parent BSUID
- omit the `to` property

### Get call permissions

The changes apply to [get call permissions](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/user-call-permissions#call-permission-request-basics) requests. There are no changes to responses.

Get call permissions using a user&#039;s phone number:

```html
curl &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;BUSINESS_PHONE_NUMBER_ID&gt;/call_permissions?user_wa_id=&lt;USER_PHONE_NUMBER&gt;&#039; \
-H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039; \
```

- `user_wa_id` — Set to the user&#039;s phone number.

Get call permissions using a user&#039;s BSUID or parent BSUID:

```html
curl &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;BUSINESS_PHONE_NUMBER_ID&gt;/call_permissions?recipient=&lt;BSUID&gt;&#039; \
-H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039; \
```

- `recipient` — Set to the user&#039;s BSUID or parent BSUID.

### Send call permission request

See [send message requests](#send-message-requests).

### Call permission request webhooks

These changes will apply to incoming call permission reply [interactive messages](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/messages/interactive) webhooks.

```html
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;&quot;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;messaging_product&quot;: &quot;whatsapp&quot;,
            &quot;metadata&quot;: &#123;
              &quot;display_phone_number&quot;: &quot;&lt;BUSINESS_DISPLAY_PHONE_NUMBER&gt;&quot;,
              &quot;phone_number_id&quot;: &quot;&lt;BUSINESS_PHONE_NUMBER_ID&gt;&quot;
            &#125;,
            &quot;contacts&quot;: [
              &#123;
                &quot;profile&quot;: &#123;
                  &quot;name&quot;: &quot;&lt;WHATSAPP_USER_PROFILE_NAME&gt;&quot;,

                  &lt;!-- Only included if user has enabled the usernames feature --&gt;
                  &quot;username&quot;: &quot;&lt;USERNAME&gt;&quot;                &lt;!-- ADDED --&gt;

                &#125;,
                &quot;wa_id&quot;: &quot;&lt;WHATSAPP_USER_ID&gt;&quot;,            &lt;!-- CHANGED --&gt;
                &quot;user_id&quot;: &quot;&lt;BSUID&gt;&quot;,                     &lt;!-- ADDED --&gt;

                &lt;!-- Only included if parent BSUIDs enabled --&gt;
                &quot;parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot;        &lt;!-- ADDED --&gt;

              &#125;
            ],
            &quot;messages&quot;: [
              &#123;
                &quot;context&quot;: &#123;
                  &quot;from&quot;: &quot;&lt;BUSINESS_DISPLAY_PHONE_NUMBER&gt;&quot;,
                  &quot;id&quot;: &quot;&lt;CONTEXTUAL_WHATSAPP_MESSAGE_ID&gt;&quot;
                &#125;,
                &quot;from&quot;: &quot;&lt;WHATSAPP_USER_PHONE_NUMBER&gt;&quot;,   &lt;!-- CHANGED --&gt;
                &quot;from_user_id&quot;: &quot;&lt;BSUID&gt;&quot;,                &lt;!-- ADDED --&gt;

                &lt;!-- Only included if parent BSUIDs enabled --&gt;
                &quot;from_parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot;   &lt;!-- ADDED --&gt;

                &quot;id&quot;: &quot;&lt;WHATSAPP_MESSAGE_ID&gt;&quot;,
                &quot;timestamp&quot;: &quot;&lt;WEBHOOK_TRIGGER_TIMESTAMP&gt;&quot;,
                &quot;type&quot;: &quot;interactive&quot;,
                &quot;interactive&quot;: &#123;
                  &quot;type&quot;:  &quot;call_permission_reply&quot;,
                  &quot;call_permission_reply&quot;: &#123;
                    &quot;response&quot;: &quot;&lt;RESPONSE&gt;&quot;,
                    &quot;expiration_timestamp&quot;: &quot;&lt;EXPIRATION_TIMTESTAMP&gt;&quot;,
                    &quot;response_source&quot;: &quot;&lt;RESPONSE_SOURCE&gt;&quot;
                  &#125;
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
- `contacts`
    - `profile`
        - `username` — New property. Will be set to the user&#039;s username if the user has adopted a username. Will be omitted if the user is not using a username.
    - `wa_id` — New property.
        - Will be omitted if the user has adopted a username and we are unable to include their phone number based on the conditions described in the [Phone numbers](#phone-numbers) section.
        - Will be set to the user&#039;s phone number if the user has not adopted a username.
    - `user_id` — New property. Will be set to the user&#039;s BSUID.
    - `parent_user_id` — New property. Will be set to the user&#039;s [parent BSUID](#parent-business-scoped-user-ids) if you have enabled parent BSUIDs. Otherwise, it will be omitted.
- `messages`
    - `from` — New behavior (can be omitted).
        - Will be set to the user&#039;s phone number if the user has not enabled the user name feature.
        - Will be omitted if the user has enabled the username feature and we are unable to include their phone number based on the conditions described in the [Phone numbers](#phone-numbers) section.
    - `from_user_id` — New property. Will be set to the user&#039;s BSUID.

### Business-initiated connected calls webhooks

These changes apply to business-initiated [connected calls](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/reference#call-connect-webhook) webhooks.

```html
&#123;
  &quot;entry&quot;: [
    &#123;
      &quot;changes&quot;: [
        &#123;
          &quot;field&quot;: &quot;calls&quot;,
          &quot;value&quot;: &#123;
            &quot;contacts&quot;: [                                  &lt;!-- ADDED --&gt;
              &#123;
                &quot;profile&quot;: &#123;
                  &lt;!-- Only included if user has enabled the usernames feature --&gt;
                  &quot;username&quot;: &quot;&lt;USERNAME&gt;&quot;                 &lt;!-- ADDED --&gt;
                &#125;,
                &quot;wa_id&quot;: &quot;&lt;USER_PHONE_NUMBER&gt;&quot;,            &lt;!-- ADDED --&gt;
                &quot;user_id&quot;: &quot;&lt;BSUID&gt;&quot;,                      &lt;!-- ADDED --&gt;

                &lt;!-- Only included if parent BSUIDs enabled --&gt;
                &quot;parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot;         &lt;!-- ADDED --&gt;
              &#125;
            ],
            &quot;calls&quot;: [
              &#123;
                &quot;biz_opaque_callback_data&quot;: &quot;&lt;DATA&gt;&quot;,
                &quot;session&quot;: &#123;
                  &quot;sdp_type&quot;: &quot;answer&quot;,
                  &quot;sdp&quot;: &quot;&lt;SDP&gt;&quot;
                &#125;,
                &quot;from&quot;: &quot;&lt;BUSINESS_PHONE_NUMBER&gt;&quot;,
                &quot;id&quot;: &quot;&lt;WHATSAPP_CALL_ID&gt;&quot;,
                &quot;to&quot;: &quot;&lt;USER_PHONE_NUMBER&gt;&quot;,               &lt;!-- CHANGED --&gt;
                &quot;to_user_id&quot;: &quot;&lt;BSUID&gt;&quot;,                   &lt;!-- ADDED --&gt;

                &lt;!-- Only included if parent BSUIDs enabled --&gt;
                &quot;to_parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot;,     &lt;!-- ADDED --&gt;

                &quot;event&quot;: &quot;connect&quot;,
                &quot;timestamp&quot;: &quot;&lt;WEBHOOK_TRIGGER_TIMESTAMP&gt;&quot;,
                &quot;direction&quot;: &quot;BUSINESS_INITIATED&quot;
              &#125;
            ],
            &quot;metadata&quot;: &#123;
              &quot;phone_number_id&quot;: &quot;&lt;BUSINESS_PHONE_NUMBER_ID&gt;&quot;,
              &quot;display_phone_number&quot;: &quot;&lt;BUSINESS_PHONE_NUMBER&gt;&quot;
            &#125;,
            &quot;messaging_product&quot;: &quot;whatsapp&quot;
          &#125;
        &#125;
      ],
      &quot;id&quot;: &quot;&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;&quot;
    &#125;
  ],
  &quot;object&quot;: &quot;whatsapp_business_account&quot;
&#125;
```

- `contacts` — New array.
    - `profile`
        - `username` — New property.
            - Will be set to the WhatsApp user&#039;s username if the user has adopted a username.
            - Will be omitted for sent status messages webhooks, or if the user is not using a username.
    - `wa_id` — New property.
        - Will be omitted if the user has adopted a username and we are unable to include their phone number based on the conditions described in the [Phone numbers](#phone-numbers) section.
        - Will be set to the user&#039;s phone number, if you sent the message to the user&#039;s phone number.
    - `user_id` — New property. Will be set to the WhatsApp user&#039;s BSUID.
    - `parent_user_id` — New property. Will be set to the user&#039;s [parent BSUID](#parent-business-scoped-user-ids) if you have enabled parent BSUIDs. Otherwise, it will be omitted.
- `calls`
    - `to` — New behavior (can be omitted). Will be set to the user&#039;s phone number if the user has adopted a username and we are able to include their phone number based on the conditions described in the [Phone numbers](#phone-numbers) section. Otherwise, it will be omitted.
    - `to_user_id` — New property. Will be set to the user&#039;s BSUID.
    - `to_parent_user_id` — New property. Will be set to the user&#039;s [parent BSUID](#parent-business-scoped-user-ids), if you have enabled parent BSUIDs. Otherwise, the property will be omitted entirely.

### User-initiated connected calls webhooks

These changes will apply to user-initiated [connected calls](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/reference#call-connect-webhook) webhooks.

```html
&#123;
  &quot;entry&quot;: [
    &#123;
      &quot;changes&quot;: [
        &#123;
          &quot;field&quot;: &quot;calls&quot;,
          &quot;value&quot;: &#123;
            &quot;metadata&quot;: &#123;
              &quot;phone_number_id&quot;: &quot;&lt;BUSINESS_PHONE_NUMBER_ID&gt;&quot;,
              &quot;display_phone_number&quot;: &quot;&lt;BUSINESS_PHONE_NUMBER&gt;&quot;
            &#125;,
            &quot;calls&quot;: [
              &#123;
                &quot;session&quot;: &#123;
                  &quot;sdp_type&quot;: &quot;offer&quot;,
                  &quot;sdp&quot;: &quot;&lt;SDP&gt;&quot;
                &#125;,
                &quot;from&quot;: &quot;&lt;USER_PHONE_NUMBER&gt;&quot;,             &lt;!-- CHANGED --&gt;
                &quot;from_user_id&quot;: &quot;&lt;BSUID&gt;&quot;,                 &lt;!-- ADDED --&gt;

                &lt;!-- Only included if parent BSUIDs enabled --&gt;
                &quot;from_parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot;,   &lt;!-- ADDED --&gt;

                &quot;id&quot;: &quot;&lt;WHATSAPP_CALL_ID&gt;&quot;,
                &quot;to&quot;: &quot;&lt;BUSINESS_PHONE_NUMBER&gt;&quot;,
                &quot;event&quot;: &quot;connect&quot;,
                &quot;timestamp&quot;: &quot;&lt;WEBHOOK_TRIGGER_TIMESTAMP&gt;&quot;,
                &quot;direction&quot;: &quot;USER_INITIATED&quot;
              &#125;
            ],
            &quot;contacts&quot;: [
              &#123;
                &quot;wa_id&quot;: &quot;&lt;USER_PHONE_NUMBER&gt;&quot;,            &lt;!-- CHANGED --&gt;
                &quot;profile&quot;: &#123;
                  &quot;name&quot;: &quot;&lt;USER_DISPLAY_NAME&gt;&quot;,           &lt;!-- ADDED --&gt;

                  &lt;!-- Only included if user has enabled usernames feature --&gt;
                  &quot;username&quot;: &quot;&lt;USERNAME&gt;&quot;                 &lt;!-- ADDED --&gt;

                &#125;,
                &quot;user_id&quot;: &quot;&lt;BSUID&gt;&quot;                       &lt;!-- ADDED --&gt;,

                &lt;!-- Only included if parent BSUIDs enabled --&gt;
                &quot;parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot;         &lt;!-- ADDED --&gt;
              &#125;
            ],
            &quot;messaging_product&quot;: &quot;whatsapp&quot;
          &#125;
        &#125;
      ],
      &quot;id&quot;: &quot;&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;&quot;
    &#125;
  ],
  &quot;object&quot;: &quot;whatsapp_business_account&quot;
&#125;
```

- `calls`
    - `from` — New behavior (can be omitted). Will be omitted if the username has enabled the usernames feature and we are unable to include their phone number based on the conditions described in the [Phone numbers](#phone-numbers) section. Otherwise, it will be set to the user&#039;s phone number.
    - `from_user_id` — New property. Will be set to the user&#039;s BSUID.
    - `from_parent_user_id` — New property. Will be set to the user&#039;s [parent BSUID](#parent-business-scoped-user-ids) if you have enabled parent BSUIDs. Otherwise, it will be omitted.
- `contacts`
    - `wa_id` — New behavior (can be omitted).
        - Will be omitted if the user has adopted a username and we are unable to include their phone number based on the conditions described in the [Phone numbers](#phone-numbers) section. Otherwise, it will be set to the user&#039;s phone number.
    - `name` — New property. Will be set to the user&#039;s profile name.
    - `username` — New property. If the user has adopted a username, it will be set to the user&#039;s username. Otherwise, it will be omitted.
    - `user_id` — New property. Will be set to the user&#039;s BSUID.
    - `parent_user_id` — New property. Will be set to the user&#039;s [parent BSUID](#parent-business-scoped-user-ids) if you have enabled parent BSUIDs. Otherwise, it will be omitted.

### Business-initiated terminated calls webhooks

These changes apply to business-initiated [terminated calls](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/reference#call-terminate-webhook) webhooks.

```html
&#123;
  &quot;entry&quot;: [
    &#123;
      &quot;changes&quot;: [
        &#123;
          &quot;field&quot;: &quot;calls&quot;,
          &quot;value&quot;: &#123;
            &quot;calls&quot;: [
              &#123;
                &quot;biz_opaque_callback_data&quot;: &quot;&lt;BUSINESS_OPAQUE_DATA&gt;&quot;,
                &quot;from&quot;: &quot;&lt;BUSINESS_PHONE_NUMBER&gt;&quot;,
                &quot;id&quot;: &quot;&lt;WHATSAPP_CALL_ID&gt;&quot;,
                &quot;to&quot;: &quot;&lt;USER_PHONE_NUMBER&gt;&quot;,              &lt;!-- CHANGED --&gt;
                &quot;to_user_id&quot;: &quot;&lt;BSUID&gt;&quot;,                  &lt;!-- ADDED --&gt;

                &lt;!-- Only included if parent BSUIDs enabled --&gt;
                &quot;to_parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot;,    &lt;!-- ADDED --&gt;

                &quot;event&quot;: &quot;terminate&quot;,
                &quot;timestamp&quot;: &quot;&lt;WEBHOOK_TRIGGER_TIMESTAMP&gt;&quot;,
                &quot;direction&quot;: &quot;BUSINESS_INITIATED&quot;,
                &quot;status&quot;: &quot;COMPLETED&quot;
              &#125;
            ],
            &quot;metadata&quot;: &#123;
              &quot;phone_number_id&quot;: &quot;&lt;BUSINESS_PHONE_NUMBER_ID&gt;&quot;,
              &quot;display_phone_number&quot;: &quot;&lt;BUSINESS_PHONE_NUMBER&gt;&quot;
            &#125;,
            &quot;contacts&quot;: [                                 &lt;!-- ADDED --&gt;
              &#123;
                &quot;profile&quot;: &#123;
                &lt;!-- Only included if user has enabled the usernames feature --&gt;
                &quot;username&quot;: &quot;&lt;USERNAME&gt;&quot;                 &lt;!-- ADDED --&gt;
                &#125;,
                &quot;wa_id&quot;: &quot;&lt;USER_PHONE_NUMBER&gt;&quot;,           &lt;!-- ADDED --&gt;
                &quot;user_id&quot;: &quot;&lt;BSUID&gt;&quot;,                     &lt;!-- ADDED --&gt;

                &lt;!-- Only included if parent BSUIDs enabled --&gt;
                &quot;parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot;        &lt;!-- ADDED --&gt;
              &#125;
            ],
            &quot;messaging_product&quot;: &quot;whatsapp&quot;
          &#125;
        &#125;
      ],
      &quot;id&quot;: &quot;&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;&quot;
    &#125;
  ],
  &quot;object&quot;: &quot;whatsapp_business_account&quot;
&#125;
```

- `calls`
    - `to` — New behavior (can be omitted). Will be set to the user&#039;s phone number if the user has adopted a username and we are able to include their phone number based on the conditions described in the [Phone numbers](#phone-numbers) section. Otherwise, it will be omitted.
    - `to_user_id` — New property. This will be set to the user&#039;s BSUID.
    - `to_parent_user_id` — New property. Will be set to the user&#039;s [parent BSUID](#parent-business-scoped-user-ids) if you have enabled parent BSUIDs. Otherwise, it will be omitted.
- `contacts` — New array.
    - `profile`
        - `username` — New property. If the user has adopted a username, it will be set to the user&#039;s username. Otherwise, it will be omitted.
    - `wa_id` — New property. Will be set to the user&#039;s phone number, if the terminated call was made to the user&#039;s phone number. Otherwise, it will be omitted.
    - `user_id` — New property. This will be set to the user&#039;s BSUID.
    - `parent_user_id` — New property. Will be set to the user&#039;s [parent BSUID](#parent-business-scoped-user-ids) if you have enabled parent BSUIDs. Otherwise, it will be omitted.

### User-initiated terminated calls webhooks

These changes will apply to user-initiated [terminated calls](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/reference#call-terminate-webhook) webhooks.

```html
&#123;
  &quot;entry&quot;: [
    &#123;
      &quot;changes&quot;: [
        &#123;
          &quot;field&quot;: &quot;calls&quot;,
          &quot;value&quot;: &#123;
            &quot;metadata&quot;: &#123;
              &quot;phone_number_id&quot;: &quot;&lt;BUSINESS_PHONE_NUMBER_ID&gt;&quot;,
              &quot;display_phone_number&quot;: &quot;&lt;BUSINESS_PHONE_NUMBER&gt;&quot;
            &#125;,
            &quot;calls&quot;: [
              &#123;
                &quot;duration&quot;: &lt;CALL_DURATION&gt;,
                &quot;start_time&quot;: &quot;&lt;CALL_START_TIMESTAMP&gt;&quot;,
                &quot;biz_opaque_callback_data&quot;: &quot;&lt;BUSINESS_OPAQUE_DATA&gt;&quot;,
                &quot;end_time&quot;: &quot;&lt;CALL_END_TIMESTAMP&gt;&quot;,
                &quot;from&quot;: &quot;&lt;USER_PHONE_NUMBER&gt;&quot;,             &lt;!-- CHANGED --&gt;
                &quot;from_user_id&quot;: &quot;&lt;BSUID&gt;&quot;,                 &lt;!-- ADDED --&gt;

                &lt;!-- Only included if you have enabled parent BSUIDs --&gt;
                &quot;from_parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot;,   &lt;!-- ADDED --&gt;

                &quot;id&quot;: &quot;&lt;WHATSAPP_CALL_ID&gt;&quot;,
                &quot;to&quot;: &quot;&lt;BUSINESS_PHONE_NUMBER&gt;&quot;,
                &quot;event&quot;: &quot;terminate&quot;,
                &quot;timestamp&quot;: &quot;&lt;WEBHOOK_TRIGGER_TIMESTAMP&gt;&quot;,
                &quot;direction&quot;: &quot;USER_INITIATED&quot;,
                &quot;status&quot;: &quot;COMPLETED&quot;
              &#125;
            ],
            &quot;contacts&quot;: [
              &#123;
                &quot;profile&quot;: &#123;
                  &quot;name&quot;: &quot;&lt;USER_PROFILE_NAME&gt;&quot;            &lt;!-- ADDED --&gt;

                  &lt;!-- Only included if user has enabled the usernames feature --&gt;
                  &quot;username&quot;: &quot;&lt;USERNAME&gt;&quot;                 &lt;!-- ADDED --&gt;
                &#125;,
                &quot;wa_id&quot;: &quot;&lt;USER_PHONE_NUMBER&gt;&quot;,            &lt;!-- CHANGED --&gt;
                &quot;user_id&quot;: &quot;&lt;BSUID&gt;&quot;,                      &lt;!-- ADDED --&gt;

                &lt;!-- Only included if you have enabled parent BSUIDs --&gt;
                &quot;parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot;         &lt;!-- ADDED --&gt;
              &#125;
            ],
            &quot;messaging_product&quot;: &quot;whatsapp&quot;
          &#125;
        &#125;
      ],
      &quot;id&quot;: &quot;&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;&quot;
    &#125;
  ],
  &quot;object&quot;: &quot;whatsapp_business_account&quot;
&#125;
```

- `calls`
    - `from` — New behavior (can be omitted). Will be omitted if the user has enabled the usernames feature and we are unable to include their phone number based on the conditions described in the [Phone numbers](#phone-numbers) section. Otherwise, it will be set to the user&#039;s phone number.
    - `from_user_id` — New property. Will be set to the user&#039;s BSUID.
    - `from_parent_user_id` — New property. Will be set to the user&#039;s parent BSUID if you have enabled [parent BSUIDs](#parent-business-scoped-user-ids). Otherwise, it will be omitted.
- `contacts`
    - `profile`
        - `name` — New property. This will be set to the user&#039;s profile name
        - `username` — New property. If the user has adopted a username, it will be set to the user&#039;s username. Otherwise, it will be omitted.
    - `wa_id` — Will be omitted if the user has enabled the usernames feature and we are unable to include their phone number based on the conditions described in the [Phone numbers](#phone-numbers) section. Otherwise, it will be set to the user&#039;s phone number.
    - `user_id` — New property. This will be set to the user&#039;s BSUID.
    - `parent_user_id` — New property. Will be set to the user&#039;s [parent BSUID](#parent-business-scoped-user-ids), if you have enabled parent BSUIDs. Otherwise, it will be omitted.

### Business-initiated calls status webhooks

These changes will apply to business-initiated [calls status](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/reference#call-status-webhook) webhooks.

```html
&#123;
  &quot;entry&quot;: [
    &#123;
      &quot;changes&quot;: [
        &#123;
          &quot;field&quot;: &quot;calls&quot;,
          &quot;value&quot;: &#123;
            &quot;statuses&quot;: [
              &#123;
                &quot;biz_opaque_callback_data&quot;: &quot;&lt;BUSINESS_OPAQUE_DATA&gt;&quot;,
                &quot;id&quot;: &quot;&lt;WHATSAPP_CALL_ID&gt;&quot;,
                &quot;type&quot;: &quot;call&quot;,
                &quot;status&quot;: &quot;&lt;STATUS&gt;&quot;,
                &quot;timestamp&quot;: &quot;&lt;WEBHOOK_TRIGGER_TIMESTAMP&gt;&quot;,
                &quot;recipient_id&quot;: &quot;&lt;USER_PHONE_NUMBER&gt;&quot;,         &lt;!-- CHANGED --&gt;
                &quot;recipient_user_id&quot;: &quot;&lt;BSUID&gt;&quot;,                &lt;!-- ADDED --&gt;

                &lt;!-- Only included if you have enabled parent BSUIDs --&gt;
                &quot;recipient_parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot;   &lt;!-- ADDED --&gt;
              &#125;
            ],
            &quot;metadata&quot;: &#123;
              &quot;phone_number_id&quot;: &quot;&lt;BUSINESS_PHONE_NUMBER_ID&gt;&quot;,
              &quot;display_phone_number&quot;: &quot;&lt;BUSINESS_PHONE_NUMBER&gt;&quot;
            &#125;,
            &quot;contacts&quot;: [                                      &lt;!-- ADDED --&gt;
              &#123;
                &quot;profile&quot;: &#123;
                  &lt;!-- Only included if user has enabled the usernames feature --&gt;
                  &quot;username&quot;: &quot;&lt;USERNAME&gt;&quot;                     &lt;!-- ADDED --&gt;
                &#125;,
                &quot;wa_id&quot;: &quot;&lt;USER_PHONE_NUMBER&gt;&quot;,                &lt;!-- ADDED --&gt;
                &quot;user_id&quot;: &quot;&lt;BSUID&gt;&quot;,                          &lt;!-- ADDED --&gt;

                &lt;!-- Only included if you have enabled parent BSUIDs --&gt;
                &quot;parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot;             &lt;!-- ADDED --&gt;
              &#125;
            ],
            &quot;messaging_product&quot;: &quot;whatsapp&quot;
          &#125;
        &#125;
      ],
      &quot;id&quot;: &quot;&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;&quot;
    &#125;
  ],
  &quot;object&quot;: &quot;whatsapp_business_account&quot;
&#125;
```

- `statuses`
    - `recipient_id` — New behavior (can be omitted).
        - Will be omitted if the user has adopted a username and we are unable to include their phone number based on the conditions described in the [Phone numbers](#phone-numbers) section. Otherwise, it will be set to the user&#039;s phone number.
    - `recipient_user_id` — New property. This will be set to the user&#039;s BSUID.
    - `recipient_parent_user_id` — New property. Will be set to the user&#039;s [parent BSUID](#parent-business-scoped-user-ids), if you have enabled parent BSUIDs. Otherwise, it will be omitted.
- `contacts` — New array.
    - `profile`
        - `username` — New property. Will be set to the user&#039;s username, if the user has enabled the usernames feature. Otherwise, it will be omitted.
    - `wa_id` — New property. Will be set to the user&#039;s phone number if the call was made to the user&#039;s phone number. Otherwise, it will be omitted.
    - `user_id` — New property. This will be set to the user&#039;s BSUID.
    - `parent_user_id` — New property. Will be set to the user&#039;s [parent BSUID](#parent-business-scoped-user-ids), if you have enabled parent BSUIDs. Otherwise, it will be omitted.

### SIP invites for business-initiated calls

These changes apply to business-initiated calls made using [SIP](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/sip).

```html
&lt;!-- BEGIN CHANGE --&gt;
INVITE sip:&lt;BSUID_OR_PHONE_NUMBER&gt;&#064;wa.meta.vc;transport=tls SIP/2.0
&lt;!-- END CHANGE --&gt;

Record-Route: &lt;sip:+159.65.244.171:5061;transport=tls;lr;ftag=Kc9QZg4496maQ;nat=yes&gt;
Via: SIP/2.0/TLS 159.65.244.171:5061;received=2803:6081:798c:93f8:5f9b:bfe8:300:0;branch=z9hG4bK0da2.36614b8977461b486ceabc004c723476.0;i=617261
Via: SIP/2.0/TLS 137.184.87.1:35181;rport=56533;received=137.184.87.1;branch=z9hG4bKQNa6meey5Dj2g
Max-Forwards: 69
From: &lt;sip:+17125550259&#064;meta-voip.example.com&gt;;tag=Kc9QZg4496maQ

&lt;!-- BEGIN CHANGE --&gt;
To: &lt;sip:&lt;BSUID_OR_PHONE_NUMBER&gt;&#064;wa.meta.vc&gt;
&lt;!-- END CHANGE --&gt;

Call-ID: dc2c5b33-1b81-43ee-9213-afb56f4e56ba
CSeq: 96743476 INVITE
Contact: &lt;sip:mod_sofia&#064;137.184.87.1:35181;transport=tls;swrad=137.184.87.1~56533~3&gt;
User-Agent: SignalWire
Allow: INVITE, ACK, BYE, CANCEL, OPTIONS, MESSAGE, INFO, UPDATE, REGISTER, REFER, NOTIFY
Supported: timer, path, replaces
Allow-Events: talk, hold, conference, refer
Session-Expires: 600;refresher=uac
Min-SE: 90
Content-Type: application/sdp
Content-Disposition: session
Content-Length: 2427
X-Relay-Call-ID: dc2c5b33-1b81-43ee-9213-afb56f4e56ba
Remote-Party-ID: &lt;sip:+17125550259&#064;meta-voip.example.com&gt;;party=calling;screen=yes;privacy=off
Content-Type: application/sdp
Content-Length:  2427

&lt;!-- SDP omitted for brevity --&gt;
```

- &lt;BSUID_OR_PHONE_NUMBER&gt; — Will be the user&#039;s BSUID or parent BSUID if the call was made to the user&#039;s BSUID or parent BSUID, or the user&#039;s phone number if sent to their phone number.

### SIP invites for user-initiated calls

These changes apply to user-initiated calls made using [SIP](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/sip).

```html
INVITE sip:+17015558857&#064;meta-voip.example.com;transport=tls SIP/2.0
Via: SIP/2.0/TLS [2803:6080:e888:51aa:d4a4:c5e0:300:0]:33819;rport=33819;received=2803:6080:e888:51aa:d4a4:c5e0:300:0;branch=z9hG4bKPjNvs.IZBnUa1W4l8oHPpk3SUMmcx3MMcE;alias
Max-Forwards: 70

&lt;!-- BEGIN CHANGE --&gt;
From: &quot;&lt;BSUID_OR_PHONE_NUMBER&gt;&quot; &lt;sip:&lt;BSUID_OR_PHONE_NUMBER&gt;&#064;wa.meta.vc&gt;;tag=bbf1ad6e-79bb-4d9c-8a2c-094168a10bea
&lt;!-- END CHANGE --&gt;

To: &lt;sip:+17015558857&#064;meta-voip.example.com&gt;

&lt;!-- BEGIN CHANGE --&gt;
Contact: &lt;sip:&lt;BSUID_OR_PHONE_NUMBER&gt;&#064;wa.meta.vc;transport=tls;ob&gt;;isfocus
&lt;!-- END CHANGE --&gt;

Call-ID: outgoing:wacid.HBgLMTIxOTU1NTA3MTQVAgASGCAzODg1NTE5NEU1NTBEMTc1RTFFQUY5NjNCQ0FCRkEzRhwYCzE3MDE1NTU4ODU3FQIAAA==
CSeq: 2824 INVITE
Route: &lt;sip:onevc-sip-proxy-dev.fbinfra.net:8191;transport=tls;lr&gt;
X-FB-External-Domain: wa.meta.vc

&lt;!-- BEGIN ADDITION --&gt;
x-wa-meta-user-id: &lt;BSUID&gt;
x-wa-meta-parent-user-id: &lt;PARENT_BSUID&gt;
x-wa-meta-username: &lt;USERNAME&gt;
&lt;!-- END ADDITION --&gt;

Allow: INVITE, ACK, BYE, CANCEL, NOTIFY, OPTIONS
User-Agent: Facebook SipGateway
Content-Type: application/sdp
Content-Length: 1028

&lt;!-- SDP omitted for brevity --&gt;
```

- `&lt;BSUID&gt;` — Will be set to the user&#039;s BSUID.
- `&lt;BSUID_OR_PHONE_NUMBER&gt;` — Will be the user&#039;s BSUID or parent BSUID if the call was made to the user&#039;s BSUID or parent BSUID, or if the user has adopted a username and we are unable to include their phone number based on the conditions described in the [Phone numbers](#phone-numbers) section. Otherwise, it will be the user&#039;s phone number.
- `&lt;PARENT_BSUID&gt;` — Will be set to the user&#039;s [parent BSUID](#parent-business-scoped-user-ids), if you enabled parent BSUIDs. Otherwise, it will be omitted.
- `&lt;USERNAME&gt;` — Will be the user&#039;s username, if the user has enabled the usernames feature. Otherwise, it will be omitted.

### SIP OK responses for business-initiated calls

```html
SIP/2.0 200 OK
Via: SIP/2.0/TLS 54.172.60.1:5061;received=2803:6080:f934:8894:7eb5:24f9:300:0;branch=z9hG4bK1e5a.0da2ace9cc912d9e5f2595ca4acb9847.0
Via: SIP/2.0/UDP 172.25.10.217:5060;rport=5060;branch=z9hG4bK5cdada8c-cbf0-4369-b02d-cc97d3c36f2b_c3356d0b_54-457463274351249162
Record-Route: &lt;sip:onevc-sip-proxy.fbinfra.net:8191;transport=tls;lr&gt;
Record-Route: &lt;sip:wa.meta.vc;transport=tls;lr&gt;
Record-Route: &lt;sip:54.172.60.1:5061;transport=tls;lr;r2=on&gt;
Record-Route: &lt;sip:54.172.60.1;lr;r2=on&gt;
Call-ID: f304a1d2cafb8139c1f9ff93a7733586&#064;0.0.0.0

&lt;!-- BEGIN CHANGE --&gt;
From: &quot;&lt;BSUID_OR_PHONE_NUMBER&gt;&quot; &lt;sip:&lt;BSUID_OR_PHONE_NUMBER&gt;&#064;meta-voip.example.com&gt;;tag=28460006_c3356d0b_5cdada8c-cbf0-4369-b02d-cc97d3c36f2b
&lt;!-- END CHANGE --&gt;

To: &lt;sip:12195550714&#064;wa.meta.vc&gt;;tag=0d185053-2615-46c7-8ff2-250bda494cf1
CSeq: 2 INVITE
Allow: INVITE, ACK, BYE, CANCEL, NOTIFY, OPTIONS
Supported: timer
X-FB-External-Domain: wa.meta.vc

&lt;!-- BEGIN CHANGE --&gt;
&lt;sip:&lt;BSUID_OR_PHONE_NUMBER&gt;&#064;wa.meta.vc;transport=tls;ob;X-FB-Sip-Smc-Tier=collaboration.sip_gateway.sip.prod&gt;;isfocus
&lt;!-- END CHANGE --&gt;

&lt;!-- BEGIN ADDITION --&gt;
x-wa-meta-user-id: &lt;BSUID&gt;
x-wa-meta-parent-user-id: &lt;PARENT_BSUID&gt;
x-wa-meta-username: &lt;USERNAME&gt;
&lt;!-- END ADDITION --&gt;

Content-Type: application/sdp
Content-Length:   645

&lt;!-- SDP omitted for brevity --&gt;
```

- `&lt;BSUID&gt;` — Will be set to the user&#039;s BSUID.
- `&lt;BSUID_OR_PHONE_NUMBER&gt;` — Will be the user&#039;s BSUID or parent BSUID if the call was made to the user&#039;s BSUID or parent BSUID, or if the user has adopted a username and we are unable to include their phone number based on the conditions described in the [Phone numbers](#phone-numbers) section. Otherwise, it will be the user&#039;s phone number.
- `&lt;PARENT_BSUID&gt;` — Will be set to the user&#039;s [parent BSUID](#parent-business-scoped-user-ids), if you enabled parent BSUIDs. Otherwise, it will be omitted.
- `&lt;USERNAME&gt;` — Will be set to the user&#039;s username, if the user has enabled the usernames feature. Otherwise, it will be omitted.

### SIP BYE responses for business- and user-initiated calls

```html
BYE sip:+12195550714&#064;103.30.244.182:5061;transport=tls SIP/2.0
Via: SIP/2.0/TLS [2803:6080:e800:6746::]:56843;rport;branch=z9hG4bKPj65946b3e6f68128d52b6a498a8fd00a5;alias
Record-Route: &lt;sip:wa.meta.vc;transport=tls;lr&gt;
Record-Route: &lt;sip:onevc-sip-proxy.fbinfra.net:8191;transport=tls;lr&gt;
Via: SIP/2.0/TLS [2803:6080:e800:6746:3347:2251:14a4:a00]:5061;branch=z9hG4bKPj65946b3e6f68128d52b6a498a8fd00a5
Via: SIP/2.0/TLS [2803:6080:e934:3f82:b543:8a4d:1414:a00]:52767;rport=52767;received=2803:6080:e934:3f82:b543:8a4d:1414:a00;branch=z9hG4bKPj-D8BXdIVMqAUT9MIJIp78LxKUZNnjYKF;alias
Max-Forwards: 69

&lt;!-- BEGIN CHANGE --&gt;
From: &lt;sip:&lt;BSUID_OR_PHONE_NUMBER&gt;&#064;wa.meta.vc&gt;;tag=0fb8b5f1-2703-49f4-a454-46b1bcb9bfac
&lt;!-- END CHANGE --&gt;

To: &lt;sip:+12195550714&#064;dev.moxcal.com&gt;;tag=2c21fad0-c581-4e54-a707-3bd52abfcc3f
Call-ID: 21e38222-6fcb-4631-8e7d-5b94cf849c90
CSeq: 31641 BYE

&lt;!-- BEGIN ADDITION --&gt;
x-wa-meta-user-id: &lt;BSUID&gt;
x-wa-meta-parent-user-id: &lt;PARENT_BSUID&gt;
x-wa-meta-username: &lt;USERNAME&gt;
&lt;!-- END ADDITION --&gt;

X-FB-External-Domain: wa.meta.vc
Allow: INVITE, ACK, BYE, CANCEL, NOTIFY, OPTIONS
User-Agent: Facebook SipGateway
Content-Length:  0
```

- `&lt;BSUID&gt;` — Will be set to the user&#039;s BSUID.
- `&lt;BSUID_OR_PHONE_NUMBER&gt;` — Will be the user&#039;s BSUID or parent BSUID if the call was made to the user&#039;s BSUID or parent BSUID, or if the user has adopted a username and we are unable to include their phone number based on the conditions described in the [Phone numbers](#phone-numbers) section. Otherwise, it will be the user&#039;s phone number.
- `&lt;PARENT_BSUID&gt;` — Will be set to the user&#039;s [parent BSUID](#parent-business-scoped-user-ids), if you enabled parent BSUIDs. Otherwise, it will be omitted.
- `&lt;USERNAME&gt;` — Will be set to the user&#039;s username, if the user has enabled the usernames feature. Otherwise, it will be omitted.

## Coexistence

### History webhooks

These changes will apply to [history](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/history) webhooks that describe an onboarded business customer&#039;s WhatsApp Business app chat history.

```html
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;&lt;CUSTOMER_WABA_ID&gt;&quot;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;messaging_product&quot;: &quot;whatsapp&quot;,
            &quot;metadata&quot;: &#123;
              &quot;display_phone_number&quot;: &quot;&lt;CUSTOMER_DISPLAY_PHONE_NUMBER&gt;&quot;,
              &quot;phone_number_id&quot;: &quot;&lt;CUSTOMER_PHONE_NUMBER_ID&gt;&quot;
            &#125;,
            &quot;history&quot;: [
              &#123;
                &quot;metadata&quot;: &#123;
                  &quot;phase&quot;: &lt;PHASE&gt;,
                  &quot;chunk_order&quot;: &lt;CHUNK_ORDER&gt;,
                  &quot;progress&quot;: &lt;PROGRESS&gt;
                &#125;,
                &quot;threads&quot;: [
                  /* First chat history thread object */
                  &#123;
                    &quot;id&quot;: &quot;&lt;WHATSAPP_USER_PHONE_NUMBER&gt;&quot;,           &lt;!-- CHANGED --&gt;
                    &quot;context&quot;: &#123;                                    &lt;!-- ADDED --&gt;
                      &quot;wa_id&quot;: &quot;&lt;WHATSAPP_USER_PHONE_NUMBER&gt;&quot;,      &lt;!-- ADDED --&gt;
                      &quot;user_id&quot;: &quot;&lt;BSUID&gt;&quot;,                         &lt;!-- ADDED --&gt;

                      &lt;!-- Only included if parent BSUIDs enabled before sync request --&gt;
                      &quot;parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot;,           &lt;!-- ADDED --&gt;

                      &lt;!-- Only included if user has enabled usernames feature before sync request --&gt;
                      &quot;username&quot;: &quot;&lt;USERNAME&gt;&quot;                      &lt;!-- ADDED --&gt;

                    &#125;,
                    &quot;messages&quot;: [
                      /* First message object in thread */
                      &#123;
                        &quot;from&quot;: &quot;&lt;BUSINESS_OR_WHATSAPP_USER_PHONE_NUMBER&gt;&quot;,  &lt;!-- CHANGED --&gt;
                        &quot;from_user_id&quot; : &quot;&lt;BSUID&gt;&quot;,                 &lt;!-- ADDED --&gt;

                        &lt;!-- Only included if parent BSUIDs enabled before sync request --&gt;
                        &quot;from_parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot;,    &lt;!-- ADDED --&gt;

                        &quot;to&quot;: &quot;&lt;WHATSAPP_USER_PHONE_NUMBER&gt;&quot;,
                        &quot;id&quot;: &quot;&lt;WHATSAPP_MESSAGE_ID&gt;&quot;,
                        &quot;timestamp&quot;: &quot;&lt;DEVICE_TIMESTAMP&gt;,
                        &quot;type&quot;: &quot;&lt;MESSAGE_TYPE&gt;&quot;,
                        &quot;&lt;MESSAGE_TYPE&gt;&quot;: &#123;
                          &lt;MESSAGE_CONTENTS&gt;
                        &#125;,
                        &quot;history_context&quot;: &#123;
                          &quot;status&quot;: &quot;&lt;MESSAGE_STATUS&gt;&quot;
                        &#125;
                      &#125;,
                      /* Additional message objects in thread would follow, if any */
                    ]
                  &#125;,
                  /* Additional chat history thread objects would follow, if any */
                ]
              &#125;
            ]
          &#125;,
          &quot;field&quot;: &quot;history&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

- `id` — New behavior (can be omitted). Will be omitted if, at the time of the history sync request, the user has already enabled usernames and we are unable to include their phone number based on the conditions described in the [Phone numbers](#phone-numbers) section. Otherwise, it will be set to the user&#039;s phone number.
- `context` — New context object.
    - `wa_id` — New property.
        - Will be omitted if, at the time of the sync request, the user has already enabled the usernames feature and we are unable to include their phone number based on the conditions described in the [Phone numbers](#phone-numbers) section. Otherwise, it will be set to the user&#039;s phone number.
    - `user_id` — New property. Will be set to the user&#039;s BSUID.
    - `parent_user_id` — Will be set to the user&#039;s [parent BSUID](#parent-business-scoped-user-ids), if you enabled parent BSUIDs. Otherwise, it will be omitted.
    - `username` — New property.
        - Will be set to the user&#039;s username, if the user has enabled the username feature. Otherwise, it will be omitted.
- `messages`
    - `from` — New behavior (can be omitted).
        - Will be omitted if, at the time of the sync request, the user has already enabled the usernames feature and we are unable to include their phone number based on the conditions described in the [Phone numbers](#phone-numbers) section. Otherwise, it will be set to the user&#039;s phone number.
    - `from_user_id` — New property. Will be set to the user&#039;s BSUID.
    - `from_parent_user_id` — Will be set to the user&#039;s [parent BSUID](#parent-business-scoped-user-ids), if you enabled parent BSUIDs. Otherwise, it will be omitted.

These changes will apply to [history](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/history) webhooks that describe a media asset sent from a WhatsApp user to a business customer, or vice-versa.

```html
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;&lt;CUSTOMER_WABA_ID&gt;&quot;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;messaging_product&quot;: &quot;whatsapp&quot;,
            &quot;metadata&quot;: &#123;
              &quot;display_phone_number&quot;: &quot;&lt;CUSTOMER_DISPLAY_PHONE_NUMBER&gt;&quot;,
              &quot;phone_number_id&quot;: &quot;&lt;CUSTOMER_PHONE_NUMBER_ID&gt;&quot;
            &#125;,
            &quot;contacts&quot;: [                                          &lt;!-- ADDED --&gt;
              &#123;

                &lt;!-- Profile only included if user has enabled the usernames feature --&gt;
                &quot;profile&quot;: &#123;
                  &quot;username&quot;: &quot;&lt;USERNAME&gt;&quot;,                        &lt;!-- ADDED --&gt;
                &#125;,
                &quot;wa_id&quot;: &quot;&lt;WHATSAPP_USER_PHONE_NUMBER&gt;&quot;,           &lt;!-- ADDED --&gt;
                &quot;user_id&quot;: &quot;&lt;BSUID&gt;&quot;,                              &lt;!-- ADDED --&gt;

                &lt;!-- Only included if parent BSUIDs enabled --&gt;
                &quot;parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot;                        &lt;!-- ADDED --&gt;
              &#125;,
            ],

            &lt;!-- Only for messages sent from a user to a business --&gt;
            &quot;messages&quot;: [
              &#123;
                &quot;from&quot;: &quot;&lt;WHATSAPP_USER_PHONE_NUMBER&gt;&quot;,            &lt;!-- CHANGED --&gt;
                &quot;from_user_id&quot;: &quot;&lt;BSUID&gt;&quot;,                         &lt;!-- ADDED --&gt;

                 &lt;!-- Only included if parent BSUIDs enabled --&gt;
                &quot;from_parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot;,           &lt;!-- ADDED --&gt;

                &quot;id&quot;: &quot;&lt;WHATSAPP_MESSAGE_ID&gt;&quot;,
                &quot;timestamp&quot;: &quot;&lt;ORIGINAL_WEBHOOK_TRIGGER_TIMESTAMP&gt;&quot;,
                &quot;type&quot;: &quot;&lt;MEDIA_TYPE&gt;&quot;,
                &quot;&lt;MEDIA_TYPE&gt;&quot;: &#123;
                  &lt;MEDIA_METADATA&gt;
                &#125;
              &#125;
            ],

            &lt;!-- Only for messages sent from a business to a user --&gt;
            &quot;message_echoes&quot;: [
              &#123;
                &quot;from&quot;: &quot;&lt;BUSINESS_DISPLAY_PHONE_NUMBER&gt;&quot;,
                &quot;to&quot;: &quot;&lt;WHATSAPP_USER_PHONE_NUMBER&gt;&quot;,              &lt;!-- CHANGED --&gt;
                &quot;to_user_id&quot;: &quot;&lt;BSUID&gt;&quot;,                           &lt;!-- ADDED --&gt;

                &lt;!-- Only included if parent BSUIDs enabled --&gt;
                &quot;to_parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot;,             &lt;!-- ADDED --&gt;

                &quot;id&quot;: &quot;&lt;WHATSAPP_MESSAGE_ID&gt;&quot;,
                &quot;timestamp&quot;: &quot;&lt;WEBHOOK_TRIGGER_TIMESTAMP&gt;&quot;,
                &quot;type&quot;: &quot;&lt;MESSAGE_TYPE&gt;&quot;,
                &quot;&lt;MESSAGE_TYPE&gt;&quot;: &#123;
                  &lt;MESSAGE_CONTENTS&gt;
                &#125;
              &#125;
            ]

          &#125;,
          &quot;field&quot;: &quot;history&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

- `contacts` — New object.
    - `profile`
        - `username` — New property. Will be set to the user&#039;s username, if the user has enabled the username feature. Otherwise, it will be omitted.
    - `wa_id` — New property.
        - Will be omitted if, at the time of the sync request, the user has already enabled the usernames feature and we are unable to include their phone number based on the conditions described in the [Phone numbers](#phone-numbers) section. Otherwise, it will be set to the user&#039;s phone number.
    - `user_id` — New property. Will be set to the user&#039;s BSUID.
    - `parent_user_id` — Will be set to the user&#039;s [parent BSUID](#parent-business-scoped-user-ids), if you enabled parent BSUIDs. Otherwise, it will be omitted.
- `messages`
    - `from` — New behavior (can be omitted).
        - Will be omitted if, at the time of the sync request, the user has already enabled the usernames feature and we are unable to include their phone number based on the conditions described in the [Phone numbers](#phone-numbers) section. Otherwise, it will be set to the user&#039;s phone number.
    - `from_user_id` — New property. Will be set to the user&#039;s BSUID.
    - `from_parent_user_id` — Will be set to the user&#039;s [parent BSUID](#parent-business-scoped-user-ids), if you enabled parent BSUIDs. Otherwise, it will be omitted.
- `message_echoes`
    - `to` — New behavior (can be omitted).
        - Will be omitted if, at the time of the sync request, the user has already enabled the usernames feature and we are unable to include their phone number based on the conditions described in the [Phone numbers](#phone-numbers) section. Otherwise, it will be set to the user&#039;s phone number.
    - `to_user_id` — New property. Will be set to the user&#039;s BSUID.
    - `to_parent_user_id` — Will be set to the user&#039;s [parent BSUID](#parent-business-scoped-user-ids), if you enabled parent BSUIDs. Otherwise, it will be omitted.

### smb_message_echoes webhooks

These changes will apply to [smb_message_echoes](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/smb_message_echoes) webhooks.

```html
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;&quot;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;messaging_product&quot;: &quot;whatsapp&quot;,
            &quot;metadata&quot;: &#123;
              &quot;display_phone_number&quot;: &quot;&lt;BUSINESS_DISPLAY_PHONE_NUMBER&gt;&quot;,
              &quot;phone_number_id&quot;: &quot;&lt;BUSINESS_PHONE_NUMBER_ID&gt;&quot;
            &#125;,
            &quot;contacts&quot;: [                                     &lt;!-- ADDED --&gt;
              &#123;

                &lt;!-- Only included if user has enabled the usernames feature --&gt;
                &quot;profile&quot;: &#123;
                  &quot;username&quot;: &quot;&lt;USERNAME&gt;&quot;                    &lt;!-- ADDED --&gt;
                &#125;,

                &quot;wa_id&quot;: &quot;&lt;WHATSAPP_USER_PHONE_NUMBER&gt;&quot;,      &lt;!-- ADDED --&gt;
                &quot;user_id&quot;: &quot;&lt;BSUID&gt;&quot;,                         &lt;!-- ADDED --&gt;

                &lt;!-- Only included if parent BSUIDs enabled --&gt;
                &quot;parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot;            &lt;!-- ADDED --&gt;
              &#125;
            ],
            &quot;message_echoes&quot;: [
              &#123;
                &quot;from&quot;: &quot;&lt;BUSINESS_DISPLAY_PHONE_NUMBER&gt;&quot;,
                &quot;to&quot;: &quot;&lt;WHATSAPP_USER_PHONE_NUMBER&gt;&quot;,         &lt;!-- CHANGED --&gt;
                &quot;to_user_id&quot;: &quot;&lt;BSUID&gt;&quot;,                      &lt;!-- ADDED --&gt;

                &lt;!-- Only included if parent BSUIDs enabled --&gt;
                &quot;to_parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot;,        &lt;!-- ADDED --&gt;

                &quot;id&quot;: &quot;&lt;WHATSAPP_MESSAGE_ID&gt;&quot;,
                &quot;timestamp&quot;: &quot;&lt;WEBHOOK_TRIGGER_TIMESTAMP&gt;&quot;,
                &quot;type&quot;: &quot;&lt;MESSAGE_TYPE&gt;&quot;,
                &quot;&lt;MESSAGE_TYPE&gt;&quot;: &#123;
                  &lt;MESSAGE_CONTENTS&gt;
                &#125;
              &#125;
            ]
          &#125;,
          &quot;field&quot;: &quot;smb_message_echoes&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

- `contacts` — New array.
    - `profile`
        - `username` — New property. Will be set to the user&#039;s username, if the user has enabled the username feature. Otherwise, it will be omitted.
    - `wa_id` — New property. Will be omitted if, at the time when the business customer used the WhatsApp Business app to send the message to the user, the user had already enabled the usernames feature and we are unable to include their phone number based on the conditions described in the [Phone numbers](#phone-numbers) section. Otherwise, it will be set to the user&#039;s phone number.
    - `user_id` — New property. Will be set to the user&#039;s BSUID.
    - `parent_user_id` — Will be set to the user&#039;s [parent BSUID](#parent-business-scoped-user-ids), if you enabled parent BSUIDs. Otherwise, it will be omitted.
- `message_echoes`
    - `to` — New behavior (can be omitted). Will be omitted if, at the time when the business customer used the WhatsApp Business app to send the message to the user, the user had already enabled the usernames feature and we are unable to include their phone number based on the conditions described in the [Phone numbers](#phone-numbers) section. Otherwise, it will be set to the user&#039;s phone number.
    - `to_user_id` — New property. Will be set to the user&#039;s BSUID.
    - `to_parent_user_id` — Will be set to the user&#039;s [parent BSUID](#parent-business-scoped-user-ids), if you enabled parent BSUIDs. Otherwise, it will be omitted.

### smb_app_state_sync webhooks

These changes will apply to [smb_app_state_sync](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/smb_app_state_sync) webhooks.

```html
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;&quot;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;messaging_product&quot;: &quot;whatsapp&quot;,
            &quot;metadata&quot;: &#123;
              &quot;display_phone_number&quot;: &quot;&lt;BUSINESS_DISPLAY_PHONE_NUMBER&gt;&quot;,
              &quot;phone_number_id&quot;: &quot;&lt;BUSINESS_PHONE_NUMBER_ID&gt;&quot;
            &#125;,
            &quot;state_sync&quot;: [
              &#123;
                &quot;type&quot;: &quot;contact&quot;,
                &quot;contact&quot;: &#123;
                  &quot;full_name&quot;: &quot;&lt;CONTACT_FULL_NAME&gt;&quot;,
                  &quot;first_name&quot;: &quot;&lt;CONTACT_FIRST_NAME&gt;&quot;,
                  &quot;phone_number&quot;: &quot;&lt;CONTACT_PHONE_NUMBER&gt;&quot;,    &lt;!-- CHANGED --&gt;
                  &quot;user_id&quot;: &quot;&lt;BSUID&gt;&quot;,                        &lt;!-- ADDED --&gt;

                  &lt;!-- Only included if parent BSUIDs enabled --&gt;
                  &quot;parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot;,          &lt;!-- ADDED --&gt;

                  &lt;!-- Only included if user has enabled the usernames feature --&gt;
                  &quot;username&quot;: &quot;&lt;USERNAME&gt;&quot;                     &lt;!-- ADDED --&gt;
                &#125;,
                &quot;action&quot;: &quot;&lt;ACTION&gt;&quot;,
                &quot;metadata&quot;: &#123;
                  &quot;timestamp&quot;: &quot;&lt;WEBHOOK_TRIGGER_TIMESTAMP&gt;&quot;
                &#125;
              &#125;,
              &lt;!-- Additional contacts would follow, if any --&gt;
            ]
          &#125;,
          &quot;field&quot;: &quot;smb_app_state_sync&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

- `phone_number` — New behavior (can be omitted). Will be omitted if, at the time of the sync request, the user has already enabled usernames and we are unable to include their phone number based on the conditions described in the [Phone numbers](#phone-numbers) section. Otherwise, it will be set to the user&#039;s phone number.
- `user_id` — New property. Will be set to the user&#039;s BSUID.
- `parent_user_id` — Will be set to the user&#039;s [parent BSUID](#parent-business-scoped-user-ids), if you enabled parent BSUIDs. Otherwise, it will be omitted.
- `username` — New property. Will be set to the user&#039;s username, if the user has enabled the username feature. Otherwise, it will be omitted.

### Revoke messages webhooks

These changes will apply to [revoke messages](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/messages/revoke) webhooks.

```html
&#123;
 &quot;object&quot;: &quot;whatsapp_business_account&quot;,
 &quot;entry&quot;: [
   &#123;
     &quot;id&quot;: &quot;&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;&quot;,
     &quot;changes&quot;: [
       &#123;
         &quot;value&quot;: &#123;
           &quot;messaging_product&quot;: &quot;whatsapp&quot;,
           &quot;metadata&quot;: &#123;
             &quot;display_phone_number&quot;: &quot;&lt;BUSINESS_DISPLAY_PHONE_NUMBER&gt;&quot;,
             &quot;phone_number_id&quot;: &quot;&lt;BUSINESS_PHONE_NUMBER_ID&gt;&quot;
           &#125;,
           &quot;contacts&quot;: [
             &#123;
               &quot;profile&quot;: &#123;
                 &quot;name&quot;: &quot;&lt;WHATSAPP_USER_PROFILE_NAME&gt;&quot;,

                 &lt;!-- Only included if user has enabled the usernames feature --&gt;
                 &quot;username&quot;: &quot;&lt;USERNAME&gt;&quot;            &lt;!-- ADDED --&gt;
               &#125;,
               &quot;wa_id&quot;: &quot;&lt;WHATSAPP_USER_ID&gt;&quot;,
               &quot;user_id&quot;: &quot;&lt;BSUID&gt;&quot;,                 &lt;!-- ADDED --&gt;

               &lt;!-- Only included if parent BSUIDs enabled --&gt;
               &quot;parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot;    &lt;!-- ADDED --&gt;
             &#125;
           ],
           &quot;messages&quot;: [
             &#123;
               &quot;from&quot;: &quot;&lt;WHATSAPP_USER_PHONE_NUMBER&gt;&quot;,    &lt;!-- CHANGED --&gt;
               &quot;from_user_id&quot;: &quot;&lt;BSUID&gt;&quot;,                 &lt;!-- ADDED --&gt;

               &lt;!-- Only included if parent BSUIDs enabled --&gt;
               &quot;from_parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot;,   &lt;!-- ADDED --&gt;

               &quot;id&quot;: &quot;&lt;WHATSAPP_MESSAGE_ID&gt;&quot;,
               &quot;timestamp&quot;: &quot;&lt;WEBHOOK_TRIGGER_TIMESTAMP&gt;&quot;,
               &quot;type&quot;: &quot;revoke&quot;,
               &quot;revoke&quot;: &#123;
                 &quot;original_message_id&quot;: &quot;&lt;ORIGINAL_WHATSAPP_MESSAGE_ID&gt;&quot;
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

- `contacts`
    - `profile`
        - `username` — New property. Will be set to the user&#039;s username, if the user has enabled the username feature. Otherwise, it will be omitted.
    - `user_id` — New property. Will be set to the user&#039;s BSUID.
    - `parent_user_id` — Will be set to the user&#039;s [parent BSUID](#parent-business-scoped-user-ids) if you enabled parent BSUIDs. Otherwise, it will be omitted.

- `messages`
    - `from` — New behavior (can be omitted). Will be set to the user&#039;s phone number if the phone number can be included based on the conditions described in the [Phone numbers](#phone-numbers) section. Otherwise, it will be omitted.
    - `from_user_id` — New property, set to the user&#039;s BSUID.
    - `from_parent_user_id` — New property, set to the user&#039;s parent BSUID, if you have enabled parent BSUIDs. Otherwise, it will be omitted.

When the business customer revokes a message using the WhatsApp Business app, the revoke is delivered as an [smb_message_echoes](#smb_message_echoes-webhooks) webhook with a `revoke` object.

```html
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;&quot;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;messaging_product&quot;: &quot;whatsapp&quot;,
            &quot;metadata&quot;: &#123;
              &quot;display_phone_number&quot;: &quot;&lt;BUSINESS_DISPLAY_PHONE_NUMBER&gt;&quot;,
              &quot;phone_number_id&quot;: &quot;&lt;BUSINESS_PHONE_NUMBER_ID&gt;&quot;
            &#125;,
            &quot;contacts&quot;: [
              &#123;
                &lt;!-- Only included if user has enabled the usernames feature --&gt;
                &quot;profile&quot;: &#123;
                  &quot;username&quot;: &quot;&lt;USERNAME&gt;&quot;
                &#125;,
                &quot;wa_id&quot;: &quot;&lt;WHATSAPP_USER_PHONE_NUMBER&gt;&quot;,
                &quot;user_id&quot;: &quot;&lt;BSUID&gt;&quot;,

                &lt;!-- Only included if parent BSUIDs enabled --&gt;
                &quot;parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot;
              &#125;
            ],
            &quot;message_echoes&quot;: [
              &#123;
                &quot;from&quot;: &quot;&lt;BUSINESS_DISPLAY_PHONE_NUMBER&gt;&quot;,
                &quot;to&quot;: &quot;&lt;WHATSAPP_USER_PHONE_NUMBER&gt;&quot;,
                &quot;to_user_id&quot;: &quot;&lt;BSUID&gt;&quot;,

                &lt;!-- Only included if parent BSUIDs enabled --&gt;
                &quot;to_parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot;,

                &quot;id&quot;: &quot;&lt;WHATSAPP_MESSAGE_ID&gt;&quot;,
                &quot;timestamp&quot;: &quot;&lt;WEBHOOK_TRIGGER_TIMESTAMP&gt;&quot;,
                &quot;type&quot;: &quot;revoke&quot;,
                &quot;revoke&quot;: &#123;
                  &quot;original_message_id&quot;: &quot;&lt;ORIGINAL_WHATSAPP_MESSAGE_ID&gt;&quot;
                &#125;
              &#125;
            ]
          &#125;,
          &quot;field&quot;: &quot;smb_message_echoes&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

- The `contacts` and `message_echoes` identifier fields (`username`, `wa_id`, `user_id`, `parent_user_id`, `to`, `to_user_id`, `to_parent_user_id`) behave the same as described in the [smb_message_echoes webhooks](#smb_message_echoes-webhooks) section.
- `message_echoes`
    - `revoke` — The revoked message, in the same format as the `revoke` object in the consumer revoke messages webhook above.

### Edit messages webhooks

These changes will apply to [edit messages](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/messages/edit) webhooks.

```html
&#123;
 &quot;object&quot;: &quot;whatsapp_business_account&quot;,
 &quot;entry&quot;: [
   &#123;
     &quot;id&quot;: &quot;&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;&quot;,
     &quot;changes&quot;: [
       &#123;
         &quot;value&quot;: &#123;
           &quot;messaging_product&quot;: &quot;whatsapp&quot;,
           &quot;metadata&quot;: &#123;
             &quot;display_phone_number&quot;: &quot;&lt;BUSINESS_DISPLAY_PHONE_NUMBER&gt;&quot;,
             &quot;phone_number_id&quot;: &quot;&lt;BUSINESS_PHONE_NUMBER_ID&gt;&quot;
           &#125;,
           &quot;contacts&quot;: [
             &#123;
               &quot;profile&quot;: &#123;
                 &quot;name&quot;: &quot;&lt;WHATSAPP_USER_PROFILE_NAME&gt;&quot;,

                 &lt;!-- Only included if the user has enabled usernames --&gt;
                 &quot;username&quot;: &quot;&lt;USERNAME&gt;&quot;              &lt;!-- ADDED --&gt;
               &#125;,
               &quot;wa_id&quot;: &quot;&lt;WHATSAPP_USER_ID&gt;&quot;,          &lt;!-- CHANGED --&gt;
               &quot;user_id&quot;: &quot;&lt;BSUID&gt;&quot;,                   &lt;!-- ADDED --&gt;

               &lt;!-- Only included if parent BSUIDs enabled --&gt;
               &quot;parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot;      &lt;!-- ADDED --&gt;
             &#125;
           ],
           &quot;messages&quot;: [
             &#123;
               &quot;from&quot;: &quot;&lt;WHATSAPP_USER_PHONE_NUMBER&gt;&quot;,    &lt;!-- CHANGED --&gt;
               &quot;from_user_id&quot;: &quot;&lt;BSUID&gt;&quot;,                 &lt;!-- ADDED --&gt;

               &lt;!-- Only included if parent BSUIDs enabled --&gt;
               &quot;from_parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot;,   &lt;!-- ADDED --&gt;

               &quot;id&quot;: &quot;&lt;WHATSAPP_MESSAGE_ID&gt;&quot;,
               &quot;timestamp&quot;: &quot;&lt;WEBHOOK_TRIGGER_TIMESTAMP&gt;&quot;,
               &quot;type&quot;: &quot;edit&quot;,
               &quot;edit&quot;: &#123;
                 &quot;original_message_id&quot;: &quot;&lt;ORIGINAL_WHATSAPP_MESSAGE_ID&gt;&quot;,
                 &quot;message&quot;: &#123;
                   &quot;context&quot;: &#123;
                     &quot;id&quot;: &quot;&lt;CONTEXT_ID&gt;&quot;
                   &#125;,
                   &quot;type&quot;: &quot;image&quot;,
                   &quot;image&quot;: &#123;
                     &quot;caption&quot;: &quot;&lt;MEDIA_ASSET_CAPTION&gt;&quot;,
                     &quot;mime_type&quot;: &quot;&lt;MEDIA_ASSET_MIME_TYPE&gt;&quot;,
                     &quot;sha256&quot;: &quot;&lt;MEDIA_ASSET_SHA256_HASH&gt;&quot;,
                     &quot;id&quot;: &quot;&lt;MEDIA_ASSET_ID&gt;&quot;,
                     &quot;url&quot;: &quot;&lt;MEDIA_ASSET_URL&gt;&quot;
                   &#125;
                 &#125;
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

- `contacts`
    - `profile`
        - `username` — New property. Will be set to the user&#039;s username, if the user has enabled the username feature. Otherwise, it will be omitted.
    - `wa_id` — New property. Will be omitted if, at the time when WhatsApp user edits the message, the user has already enabled the usernames feature and the phone number cannot be included based on the conditions described in the [Phone numbers](#phone-numbers) section. Otherwise, it will be set to the user&#039;s phone number.
    - `user_id` — New property. Will be set to the user&#039;s BSUID.
    - `parent_user_id` — Will be set to the user&#039;s [parent BSUID](#parent-business-scoped-user-ids) if you enabled parent BSUIDs. Otherwise, it will be omitted.

- `messages`
    - `from` — New behavior (can be omitted). Will be set to the user&#039;s phone number if the phone number can be included based on the conditions described in the [Phone numbers](#phone-numbers) section. Otherwise, it will be omitted.
    - `from_user_id` — New property, set to the user&#039;s BSUID.
    - `from_parent_user_id` — New property, set to the user&#039;s parent BSUID, if you have enabled parent BSUIDs. Otherwise, it will be omitted.

When the business customer edits a message using the WhatsApp Business app, the edit is delivered as an [smb_message_echoes](#smb_message_echoes-webhooks) webhook with an `edit` object. The example syntax below is for an edited **image** message.

```html
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;&quot;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;messaging_product&quot;: &quot;whatsapp&quot;,
            &quot;metadata&quot;: &#123;
              &quot;display_phone_number&quot;: &quot;&lt;BUSINESS_DISPLAY_PHONE_NUMBER&gt;&quot;,
              &quot;phone_number_id&quot;: &quot;&lt;BUSINESS_PHONE_NUMBER_ID&gt;&quot;
            &#125;,
            &quot;contacts&quot;: [
              &#123;
                &lt;!-- Only included if user has enabled the usernames feature --&gt;
                &quot;profile&quot;: &#123;
                  &quot;username&quot;: &quot;&lt;USERNAME&gt;&quot;
                &#125;,
                &quot;wa_id&quot;: &quot;&lt;WHATSAPP_USER_PHONE_NUMBER&gt;&quot;,
                &quot;user_id&quot;: &quot;&lt;BSUID&gt;&quot;,

                &lt;!-- Only included if parent BSUIDs enabled --&gt;
                &quot;parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot;
              &#125;
            ],
            &quot;message_echoes&quot;: [
              &#123;
                &quot;from&quot;: &quot;&lt;BUSINESS_DISPLAY_PHONE_NUMBER&gt;&quot;,
                &quot;to&quot;: &quot;&lt;WHATSAPP_USER_PHONE_NUMBER&gt;&quot;,
                &quot;to_user_id&quot;: &quot;&lt;BSUID&gt;&quot;,

                &lt;!-- Only included if parent BSUIDs enabled --&gt;
                &quot;to_parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot;,

                &quot;id&quot;: &quot;&lt;WHATSAPP_MESSAGE_ID&gt;&quot;,
                &quot;timestamp&quot;: &quot;&lt;WEBHOOK_TRIGGER_TIMESTAMP&gt;&quot;,
                &quot;type&quot;: &quot;edit&quot;,
                &quot;edit&quot;: &#123;
                  &quot;original_message_id&quot;: &quot;&lt;ORIGINAL_WHATSAPP_MESSAGE_ID&gt;&quot;,
                  &quot;message&quot;: &#123;
                    &quot;context&quot;: &#123;
                      &quot;id&quot;: &quot;&lt;CONTEXT_ID&gt;&quot;
                    &#125;,
                    &quot;type&quot;: &quot;image&quot;,
                    &quot;image&quot;: &#123;
                      &quot;caption&quot;: &quot;&lt;MEDIA_ASSET_CAPTION&gt;&quot;,
                      &quot;mime_type&quot;: &quot;&lt;MEDIA_ASSET_MIME_TYPE&gt;&quot;,
                      &quot;sha256&quot;: &quot;&lt;MEDIA_ASSET_SHA256_HASH&gt;&quot;,
                      &quot;id&quot;: &quot;&lt;MEDIA_ASSET_ID&gt;&quot;,
                      &quot;url&quot;: &quot;&lt;MEDIA_ASSET_URL&gt;&quot;
                    &#125;
                  &#125;
                &#125;
              &#125;
            ]
          &#125;,
          &quot;field&quot;: &quot;smb_message_echoes&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

- The `contacts` and `message_echoes` identifier fields (`username`, `wa_id`, `user_id`, `parent_user_id`, `to`, `to_user_id`, `to_parent_user_id`) behave the same as described in the [smb_message_echoes webhooks](#smb_message_echoes-webhooks) section.
- `message_echoes`
    - `edit` — The edited message, in the same format as the `edit` object in the consumer edit messages webhook above.

## Analytics

No changes.

## Billing and invoicing

No changes.

## FAQs

**What do I need to do to support usernames?**

BSUIDs and parent BSUIDs began appearing in webhooks payloads in April 2026, before usernames are made available to WhatsApp users. In order to process messages from users who enable the feature once it is available, you will need to support BSUIDs (and parent BSUIDs if you enable them). To do this, you must:

- Update your webhook integrations to support BSUIDs (and [parent BSUID](#parent-business-scoped-user-ids), if using).
- Build logic to support handling multiple identifiers (phone numbers from non-username adopters; BSUIDs from username adopters if phone number is not present in webhooks), and map relevant fields back to your CRM/database.
- Update internal and external systems related to these integrations to be able to handle BSUIDs and join with previous identifiers; primarily CRM (either 3P or internal database) and any tools or workflows triggered off of CRM (for example, triggered campaign messages, campaign management, measurement, billing, and so on).
- If you still require customer phone numbers, update your messaging bots/journeys (if used) to request phone numbers, handle scenarios where users do not share phone numbers, and iterate on these new conversation journeys.
- If you have multiple business portfolios with Meta, you may want to implement a solution to enable central CRM access across multiple portfolios, to minimize the operational overhead that comes with using and storing BSUIDs (and parent BSUIDs).

**When will I receive a BSUID or parent BSUID vs. a phone number?**

When a user adopts a username, they will have phone number privacy meaning their phone number will not be displayed in the app, and their phone number will not be included in webhooks. If the user&#039;s phone number is not present (the wa_id property is missing), you can use their BSUID (or [parent BSUID](#parent-business-scoped-user-ids), if using), which will be included and assigned to a new user_id property (`parent_user_id` for parent BSUIDs).

 If a user has not adopted usernames, you will receive both their phone number and their BSUID (and parent BSUID, if enabled).

Note that will continue to share the phone number if [certain conditions are met](#phone-numbers). Phone numbers and related data are retained for a maximum of 30 days to support features like message redelivery. There may be situations where you receive messages from existing users outside of this 30-day window, which may look like a new user thread to you. Therefore, it is essential that you begin supporting BSUIDs as soon as possible, to minimize losing any conversation context.

**Why do partners and directly-integrated businesses using the WhatsApp Business Platform, and users who have directly integrated ads that click to WhatsApp advertisers, have to adopt BSUID?**

Partners and businesses must adopt BSUID to continue processing incoming messages from WhatsApp username adopters. Once BSUID is adopted and user messages from username adopters are processed, message webhooks will no longer include phone numbers in some cases as part of the webhook such as wa_id, so anyone using the WhatsApp Business Platform must ensure all connected systems can handle BSUID. They will also be able to ask for a user&#039;s phone number in-thread.

**If I have not yet adopted BSUIDs and start to receive messages from username adopters which I cannot process, recourse is there?**

If you have not yet adopted BSUID and are not able to process messages from username adopters, there will not be any recourse or corrective action that you can take.

For messages from new customers: the webhook will continue to be sent of an incoming message. Depending on the specifics of the implementation, this may impact your systems that are not equipped to handle incoming messages without user phone numbers, and BSUID assigned to the new user_id field.
For messages from your existing customers: the phone number will continue to be included if the conditions described in the [Phone numbers](#phone-numbers) section are met.

Once you support BSUIDs, request phone numbers from users by implementing a [phone number request button](#requesting-phone-numbers-from-users).

**How do business usernames differ from display names? When will a user see a business username vs a display name?**

Business usernames will provide an ability for the users to reach the business by the business&#039; username, meaning an end user can search for a business username using their exact username and reach out to the businesses. Since end users cannot search by display names, business usernames offer a clear advantage as a searchable and unique identifier for users to reliably find the correct business.

Business usernames must follow specific formatting rules on length and allowed characters, while display names have some more leeway in terms of formatting.

Business usernames are unique and are tied 1-1 to phone numbers, meaning &#064;JaspersMarket would be tied to one phone number while &#064;JaspersMarketCustomerSupport would be tied to another phone number. Display names are not tied 1-1 to phone numbers, meaning the display name Jasper&#039;s Market can have 10 phone numbers under this display name.

When a business has both a username and display name, display name will be shown first (for example, in Profile, Chat list, Messages, and so on), for the businesses to build trust with the users and for users to recognize the business when business reaches out to the user.

## Document changelog

### June 29, 2026

- Updated [reserved usernames](#reserved-usernames) and [adopt or change a business username](#adopt-or-change-a-business-username): businesses can reserve and claim usernames starting June 29, 2026 (previously &quot;later in 2026&quot;).
- Updated [business-scoped user ID](#business-scoped-user-id): sending messages to a BSUID is supported starting in July 2026 (previously listed as May 2026).
- Updated [requesting phone numbers from users](#requesting-phone-numbers-from-users): the `REQUEST_CONTACT_INFO` button is available starting in early July 2026 (previously listed as early May 2026).
- Updated [requesting phone numbers from users (using templates)](#using-templates): clarified that the text field on a `REQUEST_CONTACT_INFO` button is optional and the button label cannot be customized. If included, it must be passed exactly as &quot;Share Contact Info&quot;; the label shown to the recipient is automatically rendered in their app&#039;s language. Removed the hardcoded text field from the template creation example.

### June 12, 2026

- Added `from_user_id` and `from_parent_user_id` to the [revoke messages webhooks](#revoke-messages-webhooks) example payload (previously listed in the May 5, 2026 changelog but missing from the example), and added an [smb_message_echoes](#smb_message_echoes-webhooks) example for a message revoked by the business customer using the WhatsApp Business app.
- Added `from_user_id` and `from_parent_user_id` to the [edit messages webhooks](#edit-messages-webhooks) payload, and added an [smb_message_echoes](#smb_message_echoes-webhooks) example for a message edited by the business customer using the WhatsApp Business app.

### June 10, 2026

- Updated [adopt or change a business username](#adopt-or-change-a-business-username): added the optional `transfer_action` parameter (`none`, `force_transfer`) for transferring a username from another business phone number within the same business portfolio, and documented the new `147005` error code returned when a transfer is required but not requested.

### May 28, 2026

- Fixed [requesting phone numbers from users](#requesting-phone-numbers-from-users): renamed the contact request interactive message type from `contact_request` to `request_contact_info` to match the API.
- Fixed [get parent BSUID account](#get-parent-bsuid-account): corrected the endpoint URL from `graph.facebook.com` to `api.facebook.com`.

### May 11, 2026

- Fixed [using templates](#using-templates) code example: hardcoded the `REQUEST_CONTACT_INFO` button text to &quot;Share Contact Info&quot; instead of using the `&lt;BUTTON_LABEL_TEXT&gt;` placeholder, which was misleading since this button text cannot be customized.

### May 7, 2026

- Corrected [contact book](#contact-book) section: clarified that contact book data is used to populate webhook payloads only, not API responses.

### May 5, 2026

- Fixed [status messages webhooks](#status-messages-webhooks) and [outbound message status webhooks](#outbound-message-status-webhooks): renamed `parent_recipient_user_id` to `recipient_parent_user_id`.

### May 4, 2026

- Fixed [status messages webhooks](#status-messages-webhooks) and [group status messages webhooks](#status-messages-webhooks-for-groups) `recipient_user_id` and `recipient_participant_user_id` descriptions: these fields are always included regardless of how the message was sent. For `failed` status messages, they will be omitted if the message was sent to the user&#039;s phone number.
- Added `recipient_user_id` to the [outbound message status webhooks](#outbound-message-status-webhooks) quick reference table.
- Added [failed status messages webhook](#status-messages-webhooks) example payload.
- Expanded [requesting phone numbers from users](#requesting-phone-numbers-from-users): added full template create/send payload examples, interactive message send payload example, and updated [contacts webhook](#contacts-webhook) payload with `from_user_id`, `origin`, and `vcard` fields. Corrected intro text: vCard is only included when a user shares a contact directly, not when tapping the `REQUEST_CONTACT_INFO` button.
- Added [delete a contact book entry](#delete-a-contact-book-entry) subsection to the [contact book](#contact-book) section.
- Added [get parent BSUID account](#get-parent-bsuid-account) subsection and enrollment details to the [parent business-scoped user IDs](#parent-business-scoped-user-ids) section.
- Updated [Block Users API](#block-users-api): parent BSUIDs are not supported for blocking or unblocking users.
- Renamed &quot;System status messages webhooks&quot; to [system messages webhooks](#system-messages-webhooks) and added new trigger for when a WhatsApp user changes their phone number.
- Added `user_id` field to the [remove group participants](#remove-group-participants) request payload.
- Updated [group_participants_update webhooks](#group_participants_update-webhooks): `removed_participants.input` now reflects the identifier used when removing the participant.
- Added `from_user_id` and `from_parent_user_id` to the [revoke messages webhooks](#revoke-messages-webhooks) payload.
- Added `name` property to the `profile` object in the [smb_message_echoes webhooks](#smb_message_echoes-webhooks) payload.

### April 9, 2026

- Fixed [status messages webhooks](#status-messages-webhooks) field description and second example: corrected `parent_user_id` to `parent_recipient_user_id` in the `statuses` block to match the `recipient_id`/`recipient_user_id` naming convention. The `parent_user_id` field in the `contacts` block is unchanged.
- Added `parent_recipient_user_id` to the [outbound message status webhooks](#outbound-message-status-webhooks) quick reference table.

### March 31, 2026

- Updated BSUID webhook rollout date from March 31 to early April 2026.
- Updated [contact book](#contact-book) limitations: Local Storage businesses now automatically have phone numbers extracted from shared vCards and stored in the contact book on Meta data centers.
- Updated [requesting phone numbers from users](#requesting-phone-numbers-from-users): removed the Local Storage exception that required manually sending a message; replaced with automatic vCard phone number extraction behavior.
- Updated [webhook testing](#webhook-testing) instructions with corrected App Dashboard navigation path.
- Added availability warning to the [adopt or change a business username](#adopt-or-change-a-business-username) section.

### March 23, 2026

- Corrected [send message response](#send-message-response) and [send marketing message response](#send-marketing-message-response) `user_id` descriptions: when both a phone number and BSUID or parent BSUID are included in the request, the response will not include `user_id`, since the phone number takes precedence and the response is identical to a phone-number-only request. Updated the example response for the phone number and BSUID case accordingly.

### March 18, 2026

- Added [webhook identifier quick reference](#webhook-identifier-quick-reference) tables summarizing which identifiers appear in messages webhooks.
- Added [webhook testing](#webhook-testing) section describing test scenarios available in the App Dashboard.
- Added [user_id_update webhooks](#user_id_update-webhooks) section.
- Added [get blocked users](#get-blocked-users) response changes to the Block Users API section.
- Added [parent BSUID](#parent-business-scoped-user-ids) format description and updated example payloads to use the `ENT` prefix (e.g., `US.ENT.11815799212886844830`).
- Added availability note for the [request contact information button](#requesting-phone-numbers-from-users) (early May 2026).
- Clarified that the [contact book](#contact-book) is provided and hosted by Meta with no integration work required, is scoped to the business portfolio level, and only stores interactions that occur after launch.
- Clarified that [user usernames](#user-usernames) have the same format restrictions as [business usernames](#business-usernames).
- Clarified that non-English characters (such as ñ, é, ü) are not supported in [business usernames](#business-usernames).
- Clarified that the 30-day lookback conditions in the [Phone numbers](#phone-numbers) section are evaluated per business phone number.
- Removed &quot;You are in the user&#039;s WhatsApp contacts list&quot; from the [phone number inclusion conditions](#phone-numbers).

### February 18, 2026

- Clarified that API requests that accept both a phone number and a BSUID or parent BSUID can include both identifiers simultaneously, with the phone number taking precedence. Updated [send message requests](#send-message-requests), [send marketing message requests](#send-marketing-message-requests), [business-initiated call requests](#businesses-initiated-call-requests), and [block or unblock user requests](#block-or-unblock-user-requests).

### February 6, 2026

- Changed number of alphanumeric characters that compose [BSUIDs](#business-scoped-user-id) from 256 to 128 alphanumeric characters.
- Changed how to use a BSUID to send a message; BSUIDs must now be assigned to dedicated properties/fields in message send requests (instead of existing properties/fields supporting both BSUIDs and phone numbers).
- Changed how [country codes](#country-codes) will appear in webhooks: they will be prefixed to user BSUIDs instead of assigned to a dedicated webhook property.
- Added [parent BSUID](#parent-business-scoped-user-ids) information, which can be used across linked business portfolios.
- Added [contact book](#contact-book) information, which can automatically store user phone numbers and BSUIDs.
- Added [phone number request button](#requesting-phone-numbers-from-users) information.
- Changed syntax examples, payload examples, and descriptions for all webhooks that returned empty strings in cases where a user has enabled the usernames feature. Now, these properties will not be set to an empty string. Instead, they will be omitted (e.g, the `wa_id` property in incoming messages webhooks).
- Changed how errors are returned when attempting to [adopt or change a business username](#adopt-or-change-a-business-username).
- Changed response syntax for [getting a business&#039;s current username](#get-current-username).
- Removed ability to cancel pending business username requests.
- Changed phone_number_username_update webhook to [business_username_updates](#business_username_updates-webhook) webhook.
