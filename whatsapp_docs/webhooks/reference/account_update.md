# account_update webhook reference



This reference describes trigger events and payload contents for the WhatsApp Business Account **account_update** webhook.

The **account_update** webhook notifies of changes to a WhatsApp Business Account&#039;s [partner-led business verification](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/partner-led-business-verification) submission, its [authentication-international rate](https://developers.facebook.com/documentation/business-messaging/whatsapp/pricing/authentication-international-rates) eligibility, or primary business location, when it is shared with a [Solution Partner](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/overview), [policy or terms violations](https://developers.facebook.com/documentation/business-messaging/whatsapp/policy-enforcement), offboarding, reconnection, or when it is deleted.


## Triggers

- A WhatsApp Business Account&#039;s partner-led business verification submission is approved, rejected, or discarded.
- A WhatsApp Business Account is deleted.
- A WhatsApp Business Account is shared (&quot;installed&quot;) or unshared (&quot;uninstalled&quot;) with a partner.
- A WhatsApp Business Account violates Meta policies or terms.
- A WhatsApp Business Account becomes eligible for authentication-international rates.
- A WhatsApp Business Account&#039;s primary business location is set.
- A WhatsApp Business Account gives the partner access to its ad accounts.
- A WhatsApp Business Account is restricted due to policy violations or enforcement actions.
- A WhatsApp Business Account accepts the MM API for WhatsApp terms of service.
- A business customer grants or revokes app permissions for a WhatsApp Business Account.
- A WhatsApp Business Account&#039;s volume-based pricing tier is updated.
- **New:** A WhatsApp Business Account is offboarded due to a device change or phone number reregistration.
- **New:** A WhatsApp Business Account is reconnected after a device change or phone number reregistration.

## Syntax

```html
&#123;
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;&quot;,
      &quot;time&quot;: &lt;WEBHOOK_TRIGGER_TIMESTAMP&gt;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;country&quot;: &quot;&lt;COUNTRY_CODE&gt;&quot;, &lt;!--only included for BUSINESS_PRIMARY_LOCATION_COUNTRY_UPDATE event --&gt;
            &quot;event&quot;: &quot;&lt;EVENT&gt;&quot;,

            &lt;!-- only included for AD_ACCOUNT_LINKED event --&gt;
            &quot;waba_info&quot;: &#123;
              &quot;waba_id&quot;: &quot;&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;&quot;,
              &quot;ad_account_linked&quot;: &quot;&lt;AD_ACCOUNT_ID&gt;&quot;,
              &quot;owner_business_id&quot;: &quot;&lt;BUSINESS_PORTFOLIO_ID&gt;&quot;
            &#125;,

            &lt;!-- only included for ACCOUNT_VIOLATION event --&gt;
            &quot;violation_info&quot;: &#123;
              &quot;violation_type&quot;: &quot;&lt;VIOLATION_TYPE&gt;&quot;
            &#125;,

            &lt;!-- only included for AUTH_INTL_PRICE_ELIGIBILITY_UPDATE event --&gt;
            &quot;auth_international_rate_eligibility&quot;: &#123;
              &quot;exception_countries&quot;: [
                &#123;
                  &quot;country_code&quot;: &quot;&lt;EXCEPTION_COUNTRY_CODE&gt;&quot;,
                  &quot;start_time&quot;: &lt;EXCEPTION_START_TIME&gt;
                &#125;
              ],
              &quot;start_time&quot;: &lt;START_TIME&gt;
            &#125;,

            &lt;!-- only included for DISABLED_UPDATE event --&gt;
            &quot;ban_info&quot;: &#123;
              &quot;waba_ban_state&quot;: &quot;&lt;WABA_BAN_STATE&gt;&quot;,
              &quot;waba_ban_date&quot;: &quot;&lt;WABA_BAN_DATE&gt;&quot;
            &#125;,

            &lt;!-- only included for VOLUME_BASED_PRICING_TIER_UPDATE event --&gt;
            &quot;volume_tier_info&quot;: &#123;
              &quot;tier_update_time&quot;: &lt;TIER_UPDATE_TIME&gt;,
              &quot;pricing_category&quot;: &quot;&lt;PRICING_CATEGORY&gt;&quot;,
              &quot;tier&quot;: &quot;&lt;TIER&gt;&quot;,
              &quot;effective_month&quot;: &quot;&lt;EFFECTIVE_MONTH&gt;&quot;,
              &quot;region&quot;: &quot;&lt;REGION&gt;&quot;
            &#125;,

            &lt;!-- only included for MM_LITE_TERMS_SIGNED event --&gt;
            &quot;waba_info&quot;: &#123;
              &quot;waba_id&quot;: &quot;&lt;WABA_ID&gt;&quot;,
              &quot;owner_business_id&quot;: &quot;&lt;BUSINESS_PORTFOLIO_ID&gt;&quot;
            &#125;,

            &lt;!-- only included for PARTNER_* events --&gt;
            &quot;waba_info&quot;: &#123;
              &quot;waba_id&quot;: &quot;&lt;CUSTOMER_WABA_ID&gt;&quot;,
              &quot;owner_business_id&quot;: &quot;&lt;CUSTOMER_BUSINESS_PORTFOLIO_ID&gt;&quot;,

              &lt;!-- only included for PARTNER_APP_INSTALLED, PARTNER_APP_UNINSTALLED events --&gt;
              &quot;partner_app_id&quot;: &quot;&lt;PARTNER_APP_ID&gt;&quot;,

              &lt;!-- only included if customer onboarded via a multi-partner solution,
                   omitted from PARTNER_APP_UNINSTALLED events --&gt;
              &quot;solution_id&quot;: &quot;&lt;SOLUTION_ID&gt;&quot;,
              &quot;solution_partner_business_ids&quot;: [
                &quot;&lt;PARTNER_IDS&gt;&quot;
              ]
            &#125;,

            &lt;!-- only included for PARTNER_REMOVED events where the business
                 was using both the WhatsApp Business app and Cloud API. --&gt;
            &quot;disconnection_info&quot;: &#123;
              &quot;reason&quot;: &quot;&lt;DISCONNECTION_REASON&gt;&quot;,
              &quot;initiated_by&quot;: &quot;&lt;DISCONNECTION_INITIATED_BY&gt;&quot;
            &#125;,

            &lt;!-- only included for PARTNER_CLIENT_CERTIFICATION_STATUS_UPDATE event --&gt;
            &quot;partner_client_certification_info&quot;: &#123;
              &quot;client_business_id&quot;: &quot;&lt;CUSTOMER_BUSINESS_PORTFOLIO_ID&gt;&quot;,
              &quot;status&quot;: &quot;&lt;STATUS&gt;&quot;,
              &quot;rejection_reasons&quot;: [
                &quot;&lt;REJECTION_REASONS&gt;&quot;
              ]
            &#125;,

            &lt;!-- only included for ACCOUNT_RESTRICTION event --&gt;
            &quot;restriction_info&quot;: [
              &#123;
                &quot;restriction_type&quot;: &quot;&lt;RESTRICTION_TYPE&gt;&quot;,
                &quot;expiration&quot;: &lt;RESTRICTION_EXPIRATION&gt;,
                &quot;remediation&quot;: &quot;&lt;REMEDIATION_STEPS&gt;&quot;
              &#125;
            ]
          &#125;,
          &quot;field&quot;: &quot;account_update&quot;
        &#125;
      ]
    &#125;
  ],
  &quot;object&quot;: &quot;whatsapp_business_account&quot;
&#125;
```

## Parameters

| Placeholder | Description | Example value |
| --- | --- | --- |
| `&lt;AD_ACCOUNT_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | Ad account ID. | `633456882212545` |
| `&lt;BUSINESS_PORTFOLIO_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | Business portfolio ID. | `131426832456945` |
| `&lt;COUNTRY_CODE&gt;`&lt;br&gt;&lt;br&gt;_String_ | ISO 3166-1 alpha-2 country code of the country where the [business to be based](https://developers.facebook.com/documentation/business-messaging/whatsapp/pricing/authentication-international-rates#primary-business-location). | `IN` |
| `&lt;CUSTOMER_BUSINESS_PORTFOLIO_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | Business customer&#039;s business portfolio ID. | `2729063490586005` |
| `&lt;CUSTOMER_WABA_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | Onboarded business customer&#039;s WABA ID. | `365694316623787` |
| `&lt;EVENT&gt;`&lt;br&gt;&lt;br&gt;_String_ | WhatsApp Business Account (&quot;WABA&quot;) event.&lt;br&gt;&lt;br&gt;Values can be:&lt;br&gt;&lt;br&gt;`ACCOUNT_DELETED` — Indicates WABA was deleted.&lt;br&gt;&lt;br&gt;`ACCOUNT_RESTRICTION` — Indicates WABA has been restricted due to [policy violations](https://developers.facebook.com/documentation/business-messaging/whatsapp/policy-enforcement). See `restriction_info` for restriction details.&lt;br&gt;&lt;br&gt;`ACCOUNT_VIOLATION` — Indicates WABA violated Meta [policies or terms](https://developers.facebook.com/documentation/business-messaging/whatsapp/policy-enforcement).&lt;br&gt;&lt;br&gt;`AD_ACCOUNT_LINKED` — Indicates WABA has been onboarded onto Marketing Messages API for WhatsApp through Embedded Signup or Intent API and gives the partner access to its ad accounts.&lt;br&gt;&lt;br&gt;`AUTH_INTL_PRICE_ELIGIBILITY_UPDATE` — Indicates WABA is eligible for [authentication-international rates](https://developers.facebook.com/documentation/business-messaging/whatsapp/pricing/authentication-international-rates).&lt;br&gt;&lt;br&gt;`BUSINESS_PRIMARY_LOCATION_COUNTRY_UPDATE` — Indicates WABA&#039;s [primary business location](https://developers.facebook.com/documentation/business-messaging/whatsapp/pricing/authentication-international-rates#primary-business-location) has been set.&lt;br&gt;&lt;br&gt;`DISABLED_UPDATE` — Indicates WABA violated Meta [policies or terms](https://developers.facebook.com/documentation/business-messaging/whatsapp/policy-enforcement).&lt;br&gt;&lt;br&gt;`MM_LITE_TERMS_SIGNED` — Indicates that the WABA has successfully accepted the MM API for WhatsApp terms of service.&lt;br&gt;&lt;br&gt;`PARTNER_ADDED` — Indicates WABA has been shared with a [Solution Partner](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/overview).&lt;br&gt;&lt;br&gt;`PARTNER_APP_INSTALLED` — Indicates a business customer granted the app one or more permissions.&lt;br&gt;&lt;br&gt;`PARTNER_APP_UNINSTALLED` — Indicates a business customer deauthenticated or uninstalled the app.&lt;br&gt;&lt;br&gt;`PARTNER_CLIENT_CERTIFICATION_STATUS_UPDATE` — Indicates the WABA&#039;s [partner-led business verification](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/partner-led-business-verification) submission is approved, rejected, or discarded.&lt;br&gt;&lt;br&gt;`PARTNER_REMOVED` — Indicates WABA has been unshared with a [Solution Partner](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/overview).&lt;br&gt;&lt;br&gt;`VOLUME_BASED_PRICING_TIER_UPDATE` — Indicates WABA&#039;s volume-based pricing tier has been updated.&lt;br&gt;&lt;br&gt;**New:** `ACCOUNT_OFFBOARDED` — Indicates WABA has been offboarded due to a device change or phone number reregistration.&lt;br&gt;&lt;br&gt;**New:** `ACCOUNT_RECONNECTED` — Indicates WABA has been reconnected after a device change or phone number reregistration. | `PARTNER_ADDED` |
| `&lt;DISCONNECTION_INITIATED_BY&gt;`&lt;br&gt;&lt;br&gt;_String_ | Indicates whether the disconnection was initiated by your client or the system. Only included for `PARTNER_REMOVED` events when `disconnection_info` is present.&lt;br&gt;&lt;br&gt;Values can be:&lt;br&gt;&lt;br&gt;`SYSTEM` — The disconnection was system-initiated (for example, due to device inactivity or [enforcement](https://developers.facebook.com/documentation/business-messaging/whatsapp/policy-enforcement)).&lt;br&gt;&lt;br&gt;`USER` — The disconnection was client-initiated (for example, your client changed their phone number, re-registered on a new device, deleted their WhatsApp account, or registered their business phone number with the consumer WhatsApp app). | `USER` |
| `&lt;DISCONNECTION_REASON&gt;`&lt;br&gt;&lt;br&gt;_String_ | Reason for the disconnection. Only included for `PARTNER_REMOVED` events when the business was using both the [WhatsApp Business app and Cloud API](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/onboarding-business-app-users).&lt;br&gt;&lt;br&gt;Values can be:&lt;br&gt;&lt;br&gt;`ACCOUNT_DISCONNECTED` — Your client&#039;s account was disconnected due to [enforcement](https://developers.facebook.com/documentation/business-messaging/whatsapp/policy-enforcement) or because your client explicitly deleted their WhatsApp account. Can be initiated by either `USER` or `SYSTEM`.&lt;br&gt;&lt;br&gt;`BUSINESS_DOWNGRADE` — Your client registered their business phone number with the consumer WhatsApp app.&lt;br&gt;&lt;br&gt;`CHANGE_NUMBER` — Your client changed their phone number.&lt;br&gt;&lt;br&gt;`COMPANION_INACTIVITY` — A companion device was inactive for approximately 30 days.&lt;br&gt;&lt;br&gt;`PRIMARY_INACTIVITY` — The primary device was inactive for approximately 14 days.&lt;br&gt;&lt;br&gt;`USER_RE_REGISTERED` — Your client re-registered on a new device. | `PRIMARY_INACTIVITY` |
| `&lt;EFFECTIVE_MONTH&gt;`&lt;br&gt;&lt;br&gt;_String_ | Effective month for the volume-based pricing tier update, in `YYYY-MM` format. | `2025-11` |
| `&lt;EXCEPTION_COUNTRY_CODE&gt;`&lt;br&gt;&lt;br&gt;_String_ | ISO 3166-1 alpha-2 country code of the country with a [start time exception](https://developers.facebook.com/documentation/business-messaging/whatsapp/pricing/authentication-international-rates#exception-countries). | `ID` |
| `&lt;EXCEPTION_START_TIME&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | Unix timestamp indicating authentication-international rate start time for the [exception country](https://developers.facebook.com/documentation/business-messaging/whatsapp/pricing/authentication-international-rates#exception-countries). | `1751347424` |
| `&lt;PARTNER_IDS&gt;`&lt;br&gt;&lt;br&gt;_Array_ | Strings of business portfolio IDs of the Tech Provider (or Tech Partner) and Solution Partner associated with the [Multi-Partner Solution](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/multi-partner-solutions). | `&quot;506914307656634&quot;,&quot;116133292427920&quot;` |
| `&lt;PRICING_CATEGORY&gt;`&lt;br&gt;&lt;br&gt;_String_ | Pricing category for the volume-based pricing tier update. | `UTILITY` |
| `&lt;REGION&gt;`&lt;br&gt;&lt;br&gt;_String_ | Region for the volume-based pricing tier update. | `India` |
| `&lt;REMEDIATION_STEPS&gt;`&lt;br&gt;&lt;br&gt;_String_ | Steps the business can take to remediate the restriction. Only included in `ACCOUNT_RESTRICTION` events, and only when remediation steps are available. See the [account restriction example](#account-restriction) for a payload without this field. | `Review your messaging practices and ensure compliance with WhatsApp policies.` |
| `&lt;REJECTION_REASONS&gt;`&lt;br&gt;&lt;br&gt;_Array_ | Rejection reason of the partner-led business verification submission.&lt;br&gt;&lt;br&gt;Values can be:&lt;br&gt;&lt;br&gt;`ADDRESS NOT MATCHING` — The country in the submitted address does not match the country on the client&#039;s business profile. Edit the submission or have your client update their profile and try again.&lt;br&gt;&lt;br&gt;`BUSINESS NOT ELIGIBLE` — Your client is not eligible for verification via partner-provided information. The client can still apply for [Meta business verification](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/partner-led-business-verification) directly.&lt;br&gt;&lt;br&gt;`LEGAL NAME NOT MATCHING` — The legal name in the submission does not match the legal name or business name on the client&#039;s business profile. The system checks for exact, fuzzy, and normalized matches. Edit the submission or have your client update their profile and try again.&lt;br&gt;&lt;br&gt;`LEGAL NAME NOT FOUND IN DOCUMENTS` — The automated document review could not locate the business legal name in the uploaded documents. Common causes include:&lt;br&gt;&lt;br&gt;- The business legal name is not mentioned in the documents&lt;br&gt;- The text in the document is unclear or hard to read&lt;br&gt;&lt;br&gt;`MALFORMED DOCUMENTS` — The uploaded documents could not be processed. The files may be corrupted, password protected, or in an unsupported format.&lt;br&gt;&lt;br&gt;`NONE` — Indicates the submission was not rejected.&lt;br&gt;&lt;br&gt;`WEBSITE NOT MATCHING` — The website domain in the submission does not match the website domain on the client&#039;s business profile. Edit the submission or have your client update their profile and try again. | `LEGAL NAME NOT FOUND IN DOCUMENTS` |
| `&lt;RESTRICTION_EXPIRATION&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | Unix timestamp indicating when the restriction expires. Only included for `ACCOUNT_RESTRICTION` events. | `1641330498` |
| `&lt;RESTRICTION_TYPE&gt;`&lt;br&gt;&lt;br&gt;_String_ | Type of restriction applied to the account. Only included for `ACCOUNT_RESTRICTION` events.&lt;br&gt;&lt;br&gt;Values can be:&lt;br&gt;&lt;br&gt;`RESTRICTED_ADD_PHONE_NUMBER_ACTION` — Business cannot add new phone numbers to the account.&lt;br&gt;&lt;br&gt;`RESTRICTED_BIZ_INITIATED_AND_USER_INITIATED_CALLING` — Business cannot make or receive calls.&lt;br&gt;&lt;br&gt;`RESTRICTED_BIZ_INITIATED_MESSAGING` — Business cannot initiate conversations with customers.&lt;br&gt;&lt;br&gt;`RESTRICTED_BUSINESS_INITIATED_CALLING` — Business cannot initiate outbound calls.&lt;br&gt;&lt;br&gt;`RESTRICTED_CUSTOMER_INITIATED_MESSAGING` — Business cannot respond to customer-initiated messages.&lt;br&gt;&lt;br&gt;`RESTRICTED_DIRECT_SEND_UTILITY_TEMPLATES` — Business cannot send utility templates via Direct Send.&lt;br&gt;&lt;br&gt;`RESTRICTED_USER_INITIATED_CALLING` — Business cannot receive inbound calls from users.&lt;br&gt;&lt;br&gt;`RESTRICTED_USER_INITIATED_CALLING_CALL_BUTTON_HIDDEN` — Call button is hidden from users due to low pickup rates.&lt;br&gt;&lt;br&gt;`RESTRICTED_UTILITY_TEMPLATES` — Business cannot create utility templates. | `RESTRICTED_BIZ_INITIATED_MESSAGING` |
| `&lt;SOLUTION_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | [Multi-Partner Solution](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/multi-partner-solutions) solution ID. | `303610109049230` |
| `&lt;START_TIME&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | Unix timestamp indicating start time for all countries with authentication-international pricing for which you do not have an [exception](https://developers.facebook.com/documentation/business-messaging/whatsapp/pricing/authentication-international-rates#exception-countries). | `1748780624` |
| `&lt;STATUS&gt;`&lt;br&gt;&lt;br&gt;_String_ | Status of the [partner-led business verification](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/partner-led-business-verification) submission.&lt;br&gt;&lt;br&gt;Values can be:&lt;br&gt;&lt;br&gt;`APPROVED` — Submission has been reviewed and approved.&lt;br&gt;&lt;br&gt;`DISCARDED` — Submission has been discarded due to technical issues or has not made progress for a while.&lt;br&gt;&lt;br&gt;`FAILED` — Submission has been reviewed and rejected. See `&lt;REJECTION_REASONS&gt;` for details.&lt;br&gt;&lt;br&gt;`PENDING` — Submission is pending review.&lt;br&gt;&lt;br&gt;`REVOKED` — Submission has been revoked. | `APPROVED` |
| `&lt;TIER&gt;`&lt;br&gt;&lt;br&gt;_String_ | Volume range for the pricing tier, in `min:max` format. | `25000001:50000000` |
| `&lt;TIER_UPDATE_TIME&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | Unix timestamp indicating when the pricing tier was updated. | `1743451903` |
| `&lt;VIOLATION_TYPE&gt;`&lt;br&gt;&lt;br&gt;_String_ | Violation type.&lt;br&gt;&lt;br&gt;See [Violations](https://developers.facebook.com/documentation/business-messaging/whatsapp/policy-enforcement-violations) for a list of possible values. | `ADULT` |
| `&lt;WABA_BAN_STATE&gt;`&lt;br&gt;&lt;br&gt;_String_ | WABA ban state.&lt;br&gt;&lt;br&gt;Values can be:&lt;br&gt;&lt;br&gt;`DISABLE` — Indicates WABA is disabled.&lt;br&gt;&lt;br&gt;`REINSTATE` — Indicates the WABA has been reinstated.&lt;br&gt;&lt;br&gt;`SCHEDULE_FOR_DISABLE` — Indicates the WABA has been scheduled to be disabled. | `REINSTATE` |
| `&lt;WABA_BAN_DATE&gt;`&lt;br&gt;&lt;br&gt;_String_ | Indicates when the WABA was banned. | `April 17, 2025` |
| `&lt;WEBHOOK_TRIGGER_TIMESTAMP&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | Unix timestamp indicating when the webhook was triggered. | `1739321024` |
| `&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | WhatsApp Business Account ID. | `102290129340398` |

## Examples

### Account deleted

```json
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;102290129340398&quot;,
      &quot;time&quot;: 1743451903,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;event&quot;: &quot;ACCOUNT_DELETED&quot;
          &#125;,
          &quot;field&quot;: &quot;account_update&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

### Account restriction

```json
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;102290129340398&quot;,
      &quot;time&quot;: 1641330498,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;event&quot;: &quot;ACCOUNT_RESTRICTION&quot;,
            &quot;restriction_info&quot;: [
              &#123;
                &quot;restriction_type&quot;: &quot;RESTRICTED_BIZ_INITIATED_MESSAGING&quot;,
                &quot;expiration&quot;: 1641330498
              &#125;,
              &#123;
                &quot;restriction_type&quot;: &quot;RESTRICTED_ADD_PHONE_NUMBER_ACTION&quot;,
                &quot;expiration&quot;: 1641330498
              &#125;
            ]
          &#125;,
          &quot;field&quot;: &quot;account_update&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

### Account violation

```json
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;102290129340398&quot;,
      &quot;time&quot;: 1743451903,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;event&quot;: &quot;ACCOUNT_VIOLATION&quot;,
            &quot;violation_info&quot;: &#123;
              &quot;violation_type&quot;: &quot;ADULT&quot;
            &#125;
          &#125;,
          &quot;field&quot;: &quot;account_update&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

### Ad account linked

```json
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;2949482758682047&quot;,
      &quot;time&quot;: 1744823932,
      &quot;changes&quot;: [
        &#123;
          &quot;field&quot;: &quot;account_update&quot;,
          &quot;value&quot;: &#123;
            &quot;event&quot;: &quot;AD_ACCOUNT_LINKED&quot;,
            &quot;waba_info&quot;: &#123;
              &quot;owner_business_id&quot;: &quot;2329417887457253&quot;,
              &quot;ad_account_linked&quot;: &quot;980198427534243&quot;,
              &quot;waba_id&quot;: &quot;980198427658004&quot;
            &#125;
          &#125;
        &#125;
      ]
    &#125;
  ]
&#125;
```

### Authentication-international eligibility

```json
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;102290129340398&quot;,
      &quot;time&quot;: 1743451903,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;auth_international_rate_eligibility&quot;: &#123;
              &quot;exception_countries&quot;: [
                &#123;
                  &quot;country_code&quot;: &quot;ID&quot;,
                  &quot;start_time&quot;: 1751347424
                &#125;
              ],
              &quot;start_time&quot;: 1748780624
            &#125;,
            &quot;event&quot;: &quot;AUTH_INTL_PRICE_ELIGIBILITY_UPDATE&quot;
          &#125;,
          &quot;field&quot;: &quot;account_update&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

### Disabled update

```json
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;102290129340398&quot;,
      &quot;time&quot;: 1743451903,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;event&quot;: &quot;DISABLED_UPDATE&quot;,
            &quot;ban_info&quot;: &#123;
              &quot;waba_ban_state&quot;: &quot;REINSTATE&quot;,
              &quot;waba_ban_date&quot;: &quot;April 17, 2025&quot;
            &#125;
          &#125;,
          &quot;field&quot;: &quot;account_update&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

### MM API for WhatsApp terms of service

```json
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;2949482758682047&quot;,
      &quot;time&quot;: 1744823932,
      &quot;changes&quot;: [
        &#123;
          &quot;field&quot;: &quot;account_update&quot;,
          &quot;value&quot;: &#123;
            &quot;event&quot;: &quot;MM_LITE_TERMS_SIGNED&quot;,
            &quot;waba_info&quot;: &#123;
              &quot;owner_business_id&quot;: &quot;2329417887457253&quot;,
              &quot;waba_id&quot;: &quot;980198427658004&quot;
            &#125;
          &#125;
        &#125;
      ]
    &#125;
  ]
&#125;
```

### Partner added

```json
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;2949482758682047&quot;,
      &quot;time&quot;: 1744823932,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;event&quot;: &quot;PARTNER_ADDED&quot;,
            &quot;waba_info&quot;: &#123;
              &quot;waba_id&quot;: &quot;980198427658004&quot;,
              &quot;owner_business_id&quot;: &quot;2329417887457253&quot;,
              &quot;solution_id&quot;: &quot;1715120619246906&quot;,
              &quot;solution_partner_business_ids&quot;: [
                &quot;2949482758682047&quot;,
                &quot;520744086200222&quot;
              ]
            &#125;
          &#125;,
          &quot;field&quot;: &quot;account_update&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

### Partner app installed

```json
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;2949482758682047&quot;,
      &quot;time&quot;: 1745337174,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;event&quot;: &quot;PARTNER_APP_INSTALLED&quot;,
            &quot;waba_info&quot;: &#123;
              &quot;waba_id&quot;: &quot;1191624265890717&quot;,
              &quot;owner_business_id&quot;: &quot;2329417887457253&quot;,
              &quot;partner_app_id&quot;: &quot;5731794616896507&quot;,
              &quot;solution_id&quot;: &quot;1715120619246906&quot;,
              &quot;solution_partner_business_ids&quot;: [
                &quot;2949482758682047&quot;,
                &quot;520744086200222&quot;
              ]
            &#125;
          &#125;,
          &quot;field&quot;: &quot;account_update&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

### Partner app uninstalled

```json
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;2949482758682047&quot;,
      &quot;time&quot;: 1748477359,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;event&quot;: &quot;PARTNER_APP_UNINSTALLED&quot;,
            &quot;waba_info&quot;: &#123;
              &quot;waba_id&quot;: &quot;184943124712545&quot;,
              &quot;owner_business_id&quot;: &quot;1284923862322270&quot;,
              &quot;partner_app_id&quot;: &quot;869361281603019&quot;
            &#125;
          &#125;,
          &quot;field&quot;: &quot;account_update&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

### Partner-led business verification status

```json
&#123;
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;102290129340398&quot;,
      &quot;time&quot;: 1743138982,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;event&quot;: &quot;PARTNER_CLIENT_CERTIFICATION_STATUS_UPDATE&quot;,
            &quot;partner_client_certification_info&quot;: &#123;
              &quot;client_business_id&quot;: &quot;2729063490586005&quot;,
              &quot;status&quot;: &quot;APPROVED&quot;,
              &quot;rejection_reasons&quot;: [
                &quot;NONE&quot;
              ]
            &#125;
          &#125;,
          &quot;field&quot;: &quot;account_update&quot;
        &#125;
      ]
    &#125;
  ],
  &quot;object&quot;: &quot;whatsapp_business_account&quot;
&#125;
```

### Partner removed

```json
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;2949482758682047&quot;,
      &quot;time&quot;: 1748477359,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;event&quot;: &quot;PARTNER_REMOVED&quot;,
            &quot;waba_info&quot;: &#123;
              &quot;waba_id&quot;: &quot;980198427658004&quot;,
              &quot;owner_business_id&quot;: &quot;2329417887457253&quot;
            &#125;
          &#125;,
          &quot;field&quot;: &quot;account_update&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

### Partner removed (WhatsApp Business app disconnection) &#123;#partner-removed-disconnection&#125;

```json
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;2949482758682047&quot;,
      &quot;time&quot;: 1748477359,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;event&quot;: &quot;PARTNER_REMOVED&quot;,
            &quot;waba_info&quot;: &#123;
              &quot;waba_id&quot;: &quot;980198427658004&quot;,
              &quot;owner_business_id&quot;: &quot;2329417887457253&quot;
            &#125;,
            &quot;disconnection_info&quot;: &#123;
              &quot;reason&quot;: &quot;PRIMARY_INACTIVITY&quot;,
              &quot;initiated_by&quot;: &quot;SYSTEM&quot;
            &#125;
          &#125;,
          &quot;field&quot;: &quot;account_update&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

### Primary business location set

```json
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;102290129340398&quot;,
      &quot;time&quot;: 1743138982,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;country&quot;: &quot;IN&quot;,
            &quot;event&quot;: &quot;BUSINESS_PRIMARY_LOCATION_COUNTRY_UPDATE&quot;
          &#125;,
          &quot;field&quot;: &quot;account_update&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

### Pricing tiering update

```json
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;102290129340398&quot;,
      &quot;time&quot;: 1743451903,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;volume_tier_info&quot;: &#123;
                &quot;tier_update_time&quot;: 1743451903,
                &quot;pricing_category&quot;: &quot;UTILITY&quot;,
                &quot;tier&quot;: &quot;25000001:50000000&quot;,
                &quot;effective_month&quot;: &quot;2025-11&quot;,
                &quot;region&quot;: &quot;India&quot;
            &#125;,
            &quot;event&quot;: &quot;VOLUME_BASED_PRICING_TIER_UPDATE&quot;
          &#125;,
          &quot;field&quot;: &quot;account_update&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

### Account offboarded &#123;#account-offboarded&#125;

Sent when a WhatsApp Business Account is offboarded due to a device change or phone number reregistration.

```json
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;102290129340398&quot;,
      &quot;time&quot;: 1743451903,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;event&quot;: &quot;ACCOUNT_OFFBOARDED&quot;
          &#125;,
          &quot;field&quot;: &quot;account_update&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

### Account reconnected &#123;#account-reconnected&#125;

Sent when a WhatsApp Business Account is reconnected after a device change or phone number reregistration.

```json
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;102290129340398&quot;,
      &quot;time&quot;: 1743451903,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;event&quot;: &quot;ACCOUNT_RECONNECTED&quot;
          &#125;,
          &quot;field&quot;: &quot;account_update&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```
