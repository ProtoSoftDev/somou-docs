# Authentication-international rates



Specific countries have an **authentication-international** rate in our [rate cards](https://developers.facebook.com/documentation/business-messaging/whatsapp/pricing#rate-cards). If you send an authentication template message to a WhatsApp user whose country calling code is for a country that has an authentication-international rate, the delivered message will be billed the country&#039;s authentication–international rate if:

* your business is [eligible](#eligibility) for authentication-international rates
* your business is based in another country (see [Primary Business Location](#primary-business-location))
* the message was delivered on or after your [start time](#start-times) for that country

For example, if your business is based in Indonesia and you send an authentication template message to a WhatsApp user who has a +62 (Indonesia) country calling code, and the message is delivered, you will not be billed the authentication-international rate since you are based in the same country as the user. If your business is based in India, however, you will be billed the authentication-international rate, if you meet all of the criteria above.

See [Examples](#examples) for additional example scenarios.

Status [messages](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/messages/status) webhooks that include pricing details and [pricing analytics](https://developers.facebook.com/documentation/business-messaging/whatsapp/analytics#pricing-analytics) will indicate if a message or set of messages were billed the authentication-international rate.

## Eligibility

If your business sends more than 750K messages outside of customer service windows in a moving 30-day period, across all of your WhatsApp Business Accounts, with unique WhatsApp users whose country calling codes are for a country that has an authentication-international rate, it will be deemed eligible for authentication-international rates.

Once deemed eligible, we will set your [start times](#start-times) 30 days out for each country that has an authentication-international rate. In addition, we will attempt to determine your [primary business location](#primary-business-location) using publicly-available information.

We will then send you an [eligibility email](#eligibility-email) that includes these start times and the country that we set as your primary business location (if we were able to determine the country). This provides you with 30 days notice before authentication-international rates apply. [Webhooks](#webhooks) will also be triggered that include your start times and your primary business location (if we set it).

Note that eligibility is permanent. Once your business is deemed eligible, all authentication template messages sent on or after your start time will be charged the authentication-international rate in markets where authentication-international rates apply.

## Countries with authentication-international rates

The following countries have authentication-international rates:

* Egypt
* India
* Indonesia
* Malaysia
* Nigeria
* Pakistan
* Saudi Arabia
* South Africa
* United Arab Emirates

Please see [Rate cards](https://developers.facebook.com/documentation/business-messaging/whatsapp/pricing#rate-cards) for more details about the rates.

## Start times

Start times are business- and country-specific timestamps. They indicate when newly-delivered authentication template messages are subject to authentication-international rates. Authentication template messages sent by your business and delivered to WhatsApp users in these countries **on or after these dates** only will be charged authentication-international rates.

Start times are set when your business is first deemed eligible for authentication-international rates, and are 30 days from your eligibility date, so you will always have 30 days notice before the authentication-international rate applies.

Start times are included in your [eligibility email](#eligibility-email) and [webhooks](#webhooks). You can also get these times by requesting the `auth_international_rate_eligibility` field on any of your business&#039;s WhatsApp Business Accounts:

### Request

```html
curl &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;WABA_ID&gt;?fields=auth_international_rate_eligibility&#039; \
-H &#039;Authorization: &lt;ACCESS_TOKEN&gt;&#039;
```

### Request parameters

| Placeholder | Description | Example value |
| --- | --- | --- |
| `&lt;ACCESS_TOKEN&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;[System token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#system-user-access-tokens) or [business token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#business-integration-system-user-access-tokens). | `EAAA...` |
| `&lt;API_VERSION&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Graph API version. | v25.0 |
| `&lt;WABA_ID&gt;`&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;WhatsApp Business Account ID. | `102290129340398` |

### Response

Upon success:

```html
&#123;
  &quot;id&quot;: &quot;&lt;WABA_ID&gt;&quot;,
  &quot;auth_international_rate_eligibility&quot;: &#123;
    &quot;start_time&quot;: &lt;START_TIME&gt;,
    &quot;exception_countries&quot;: [
      &lt;EXCEPTION_COUNTRY&gt;,
      &lt;EXCEPTION_COUNTRY&gt;,
      ...
    ]
  &#125;
&#125;
```

### Response parameters

| Placeholder | Description | Example value |
| --- | --- | --- |
| `&lt;WABA_ID&gt;` | WhatsApp Business Account (WABA) ID. | `102290129340398` |
| `&lt;START_TIME&gt;` | Unix timestamp indicating start time for all countries with authentication-international pricing for which you do not have an [exception](#exception-countries). | `1732057507` |
| `&lt;EXCEPTION_COUNTRY&gt;` | A unique object describing a country that has an exception start time. See [exception country](#exception-countries).&lt;br&gt;&lt;br&gt;For most WhatsApp Business Accounts, the `exception_countries` array will be empty. | `&#123; &quot;country_code&quot;: &quot;ID&quot;, &quot;start_time&quot;: 1742450707 &#125;`&lt;br&gt; |

## Primary business location

Your primary business location is the country where your business is based. It will appear in the Business Manager under the **Primary Business Location** field starting May 1, 2024, if we are able to determine where your business is based using publicly-available information.

The following publicly-available information is used to determine where your business is based:

* Where your business may be publicly-traded and listed
* Your business&#039;s corporate structure (where a parent or may be based or publicly-traded)

We will attempt to determine where your business is based when:

* It is deemed [eligible](#eligibility) for authentication-international rates
* You [edit your primary business location](#set-or-edit-your-primary-business-location) using the Business Manager.

This process can take up to 3 business days. The outcome of this determination can be:

* **Verified** — We determined where your business is based and set your primary business location to this country (which also triggers a webhook).
* **Need more information** — We require more information in order to make a determination.
* **Rejected** — We disagreed with the country you designated in the Business Manager (if you used it to edit the **Primary Business Location** field)

You will be notified of the outcome in your initial [eligibility email](#eligibility-email), or in a separate email if you used the Business Manager to edit your location.

If rejected or if we need more information, or if you disagree with the country we determined to be the primary business location, you can use the Business Manager to edit your location.

Note that if your primary business location status is not verified but you are past your start time for a given country, any authentication messages that you send to a WhatsApp user in that country will be billed the authentication-international rate.

### Set or edit your primary business location

To set or edit your primary business location:

1. [Navigate to Business Settings by clicking here](https://business.facebook.com/settings/info?edit_pbl=true)
1. Select the country of the business&#039;s primary location of operation from the dropdown, or enter it in the text field. Note that this is the location where your business has its headquarters and maintains its bookkeeping records.
1. Click **Next**
1. Answer the questions on the screen. These answers will help Meta verify your primary business location.
1. Click **Next**
1. Click **Submit for review**

_Note: You won&#039;t be able to make any changes while your verification is under review._

### Primary business location status

The **Primary Business Location** field in the Business Manager will also display a status:

* **Verified** — We have verified your business&#039;s primary location.
* **Pending verification** — We are in the process of determining your business&#039;s primary location.
* **Rejected** — We disagreed with the country you designated, based on publicly available information and what you included when you edited your location. You can manually edit your location again and include different information as part of your submission.

### Get your location via API

You can use the API to see if your business&#039;s primary business location is set by requesting the `primary_business_location` field on your WhatsApp Business Account (WABA):

#### Request

```html
curl &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;WABA_ID&gt;?fields=primary_business_location&#039; \
-H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039;
```

### Response:

Upon success:

```html
&#123;
  &quot;id&quot;: &quot;&lt;WABA_ID&gt;&quot;,
  &quot;primary_business_location&quot;: &quot;&lt;COUNTRY_CODE&gt;&quot;
&#125;
```

* `&lt;WABA_ID&gt;` — WhatsApp Business Account ID.
* `&lt;COUNTRY_CODE&gt;` — Two-character country code indicating the country where we have determined the business to be based.

## Eligibility email

By sending authentication messages over WhatsApp, you acknowledge and agree that when your business is deemed eligible for authentication-international rates, an email will be sent to all of the email addresses associated with the admins of your accounts, and all third parties that your WhatsApp Business Accounts have been shared with (e.g. admins of Solution Partners that have access to your WhatsApp Business Accounts), to alert them that the threshold of eligibility has been reached.

The email will include:

* Your exact [start times](#start-times) for each country that has an authentication-international rate.
* The country that we set as your [primary business location](#primary-business-location).

## Exception countries

Authentication-international rates for applicable countries will begin on the same date, unless otherwise specified in your eligibility email, the `exception_countries` array in [eligibility webhooks](#eligibility-webhook), or the `exception_countries` array returned when [requesting](https://developers.facebook.com/documentation/business-messaging/whatsapp/pricing/authentication-international-rates#request) the `auth_international_rate_eligibility` field on your WhatsApp Business Account (WABA).

You will always be charged the domestic rate for your primary business location, even if it appears in the either `exception_countries` array.

### Example scenario

In the following examples, assume this scenario:

- there are three countries, identified by three fictitious country codes: A, B, and C
- countries A and B have authentication-international rates
- country C does not have an authentication-international rate
- the business portfolio has a WABA with ID 12345

Requesting the `auth_international_rate_eligibility` field on WABA 12345 returns:

```json
&#123;
  &quot;id&quot;: &quot;12345&quot;,
  &quot;auth_international_rate_eligibility&quot;: &#123;
    &quot;start_time&quot;: 1717225200, // Indicates country A start time: June 1, 2024
    &quot;exception_countries&quot;: [
      &#123;
        &quot;country_code&quot;: &quot;B&quot;,
        &quot;start_time&quot;: 1719817200 // Indicates country B start time: July 1, 2024
      &#125;
    ]
  &#125;
&#125;
```

Country C is not represented in the response because it does not have an authentication-international rate.

### Scenario 1

The business&#039;s primary business location is country C.

- The authentication-international rate applies for country A on June 1, 2024.
- The authentication-international rate applies for country B on July 1, 2024.

### Scenario 2

The business&#039;s primary business location is country B.

- The authentication-international rate applies for country A on June 1, 2024.

The authentication-international rate for country B does not apply because the business&#039;s primary business location is also country B.

## Webhooks

### Eligibility webhook

An `account_update` webhook will be triggered if your business is deemed eligible for international rates. The webhook will include start times for each country that has an authentication-international rate.

Please see [Rate cards](https://developers.facebook.com/documentation/business-messaging/whatsapp/pricing#rate-cards) for the list of countries with authentication-international rates.

```html
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;&lt;WABA_ID&gt;&quot;,
      &quot;time&quot;: &lt;WEBHOOK_SENT_TIMESTAMP&gt;,
      &quot;changes&quot;: [
        &#123;
          &quot;field&quot;: &quot;account_update&quot;,
          &quot;value&quot;: &#123;
            &quot;auth_international_rate_eligibility&quot;: &#123;
              &quot;exception_countries&quot;: [
                &#123;
                  &quot;country_code&quot;: &quot;&lt;EXCEPTION_COUNTRY_CODE&gt;&quot;,
                  &quot;start_time&quot;: &lt;EXCEPTION_START_TIME&gt;
                &#125;
              ],
              &quot;start_time&quot;: &lt;START_TIME&gt;
            &#125;,
            &quot;event&quot;: &quot;AUTH_INTL_PRICE_ELIGIBILITY_UPDATE&quot;
          &#125;
        &#125;
      ]
    &#125;
  ]
&#125;
```

* `&lt;WABA_ID&gt;` — WhatsApp Business Account ID.
* `&lt;WEBHOOK_SENT_TIMESTAMP&gt;` — Unix timestamp indicating when the webhook was sent.
* `&lt;EXCEPTION_COUNTRY_CODE&gt;` — Two-letter country code (e.g. `ID` for Indonesia) of the country with a start time exception.
* `&lt;EXCEPTION_START_TIME&gt;` — Unix timestamp indicating authentication-international rate start time for the exception country.
* `&lt;START_TIME_INDIA&gt;` — Unix timestamp indicating start time for all countries with authentication-international pricing **for which you do not have an exception**.

### Primary business location update webhook

Subscribe to the `account_update` webhook to be notified when the business&#039;s [primary business location](#primary-business-location) is set. If we are able to determine the country where your business is based, we will set your location to that country and trigger an `account_update` webhook with the country&#039;s two-character country code assigned to the `BUSINESS_PRIMARY_LOCATION_COUNTRY_UPDATE` property.

```json
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;&lt;WABA_ID&gt;&quot;,
      &quot;time&quot;: &lt;TIMESTAMP&gt;,
      &quot;changes&quot;: [
        &#123;
          &quot;field&quot;: &quot;account_update&quot;,
          &quot;value&quot;: &#123;
            &quot;country&quot;: &quot;&lt;COUNTRY_CODE&gt;&quot;,
            &quot;event&quot;: &quot;BUSINESS_PRIMARY_LOCATION_COUNTRY_UPDATE&quot;
          &#125;
        &#125;
      ]
    &#125;
  ]
&#125;
```

* `&lt;WABA_ID&gt;` — WhatsApp Business Account ID.
* `&lt;TIMESTAMP&gt;` — Unix timestamp indicating when the webhook was sent.
* `&lt;COUNTRY_CODE&gt;` — ISO 3166-1 alpha-2 country code, indicating the country where we have determined the business to be based.

### Pricing in messages webhook

If an authentication template message is billed the authentication-international rate, the `pricing` object in status [messages](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/messages/status) webhooks will have `category` set to `authentication_international`.

```json
&quot;pricing&quot;: &#123;
&quot;billable&quot;: true,
&quot;pricing_model&quot;: &quot;PMP&quot;,
&quot;type&quot;: &quot;regular&quot;,
&quot;category&quot;: &quot;authentication_international&quot;
&#125;
```

## Examples

A business with an **Indonesia** [primary business location](#primary-business-location) send an authentication template message to a WhatsApp user:

| User location | Is business eligible? | Is on/after start time? | Rate billed |
| --- | --- | --- | --- |
| Indonesia | - | - | Authentication |
| India | No | - | Authentication |
| India | Yes | No | Authentication |
| India | Yes | Yes | Authentication-International |

A business with an India primary business location sends an authentication template message to a WhatsApp user:

| User location | Is business eligible? | Is on/after start time? | Rate billed |
| --- | --- | --- | --- |
| India | - | - | Authentication |
| Indonesia | No | - | Authentication |
| Indonesia | Yes | No | Authentication |
| Indonesia | Yes | Yes | Authentication-International |

A business with a primary business location that does not have an authentication-international rate sends an authentication template message to a WhatsApp user:

| User location | Is business eligible? | Is on/after start time? | Rate billed |
| --- | --- | --- | --- |
| Indonesia | No | - | Authentication |
| Indonesia | Yes | No | Authentication |
| Indonesia | Yes | Yes | Authentication-International |
| India | No | - | Authentication |
| India | Yes | No | Authentication |
| India | Yes | Yes | Authentication-International |

