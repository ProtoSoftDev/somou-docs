# Pre-filling screens



**Warning:** **Embedded signup v2 will be deprecated on October 15, 2026.** Migrate your integration to [v4](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/version-4) before that date to avoid disruption. See [Versions](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/versions) for the full upgrade path.

If you know details about your customer&#039;s business, such as its name and address, you can inject this data into Embedded Signup. Injecting this data can pre-fill screens or bypass them altogether, reducing the amount of input and interaction required by your customers.

For example, here is the business portfolio screen, pre-filled with business&#039;s name, email address, website, country, and a pre-verified business phone number:

For the best experience, inject [business portfolio data](#business-portfolio-data), a [pre-verified number](#pre-verified-phone-numbers), and [phone profile data](#phone-profile-data). Injecting this data provides the best experience for your customer, as it:

* entirely pre-fills the business portfolio screen
* bypasses the WhatsApp Business account (WABA) selection and creation screens
* bypasses the business phone number selection and verification screens
* automatically sets the business phone number&#039;s profile information in the WhatsApp client

## Embedded Signup integration helper

The Embedded Signup Integration Helper provides a convenient way for you to create pre-filled data payloads and test their impact on the flow. To access the payload tool:

* Navigate to **App Dashboard** &gt; **WhatsApp** &gt; **Embedded Signup Builder**.
* Locate the **Embedded Signup Setup** section.
* Locate the **Embedded Signup Pre-fill** row.
* Click the **Edit pre-fill data** button.

## Injecting data

The `FB.login` function, which gets called when a business customer launches Embedded Signup, accepts an object as an argument. Use this object&#039;s `extras.setup` property to inject data:

```js
// Launch method and callback registration
const launchWhatsAppSignup = () =&gt; &#123;
  FB.login(fbLoginCallback, &#123;
    config_id: &#039;&lt;CONFIGURATION_ID&gt;&#039;, // your configuration ID goes here
    response_type: &#039;code&#039;,
    override_default_response_type: true,
    extras: &#123;
      setup: &#123;
        business: &#123;
          // Business portfolio data goes here
        &#125;,
        preVerifiedPhone: &#123;
          // Pre-verified phone number IDs go here
        &#125;,
        phone: &#123;
          // Phone number profile data goes here
        &#125;,
        whatsAppBusinessAccount: &#123;
          // WABA IDs go here
        &#125;
      &#125;,
      featureType: &#039;&#039;,
      sessionInfoVersion: &#039;3&#039;,
    &#125;
  &#125;);
&#125;
```

### Business portfolio data

You can inject the following business portfolio details into the business portfolio screen:

* business portfolio name
* business portfolio email address
* business portfolio website
* business portfolio country (as well as additional address details)
* business phone number

Alternatively, you can inject _just an existing business portfolio ID_, and Embedded Signup automatically injects the portfolio&#039;s existing details into the screen. Injecting just the portfolio ID can be useful if you want a pre-verified phone number to be associated with the customer&#039;s existing business portfolio.

Injecting business portfolio data will pre-fill the business portfolio screen and also cause Embedded Signup to skip the WABA selection and WABA creation screens.

Injecting business phone number data will pre-fill the [phone number addition screen](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/default-flow#phone-number-addition-screen):

Note that even if you inject data, the business customer can still edit this data using the **Edit** button, if they wish.

When a business customer completes the flow, Embedded Signup uses the business portfolio information you injected to create the business customer&#039;s business portfolio and WABA.

#### Business object syntax

The `business` object pre-fills the business portfolio screen. Use the following syntax:

```html
setup: &#123;
  business: &#123;
    id: &lt;BUSINESS_PORTFOLIO_ID&gt;,
    name: &#039;&lt;BUSINESS_PORTFOLIO_NAME&gt;&#039;,
    email: &#039;&lt;BUSINESS_PORTFOLIO_EMAIL_ADDRESS&gt;&#039;,
    website: &#039;&lt;BUSINESS_PORTFOLIO_WEBSITE&gt;&#039;,
    address: &#123;
      streetAddress1: &#039;&lt;BUSINESS_PORTFOLIO_STREET_ADDRESS_LINE_1&gt;&#039;,
      streetAddress2: &#039;&lt;BUSINESS_PORTFOLIO_STREET_ADDRESS_LINE_2&gt;&#039;,
      city: &#039;&lt;BUSINESS_PORTFOLIO_CITY&gt;&#039;,
      state: &#039;&lt;BUSINESS_PORTFOLIO_STATE&gt;&#039;,
      zipPostal: &#039;&lt;BUSINESS_PORTFOLIO_ZIP_CODE&gt;&#039;,
      country: &#039;&lt;BUSINESS_PORTFOLIO_COUNTRY&gt;&#039;
    &#125;,
    phone: &#123;
      code: &lt;BUSINESS_PORTFOLIO_COUNTRY_CALLING_CODE&gt;,
      number: &#039;&lt;BUSINESS_PORTFOLIO_PHONE_NUMBER&gt;&#039;
    &#125;,
    timezone: &#039;&lt;BUSINESS_PORTFOLIO_TIME_ZONE&gt;&#039;
  &#125;
&#125;
```

#### Business object parameters

| Placeholder | Description | Example value |
| --- | --- | --- |
| `&lt;BUSINESS_PORTFOLIO_ID&gt;`&lt;br&gt;&lt;br&gt;_Integer or null_ | **Required if using an existing business portfolio, otherwise set to null or omit to create a new portfolio.**&lt;br&gt;&lt;br&gt;Set to the business customer&#039;s existing business portfolio ID if you want to pre-fill the screen with data already set on the business portfolio, or if you want to associate a pre-verified phone number with this portfolio.&lt;br&gt;&lt;br&gt;If set to a portfolio ID, Embedded Signup checks whether the business customer owns the portfolio.&lt;br&gt;&lt;br&gt;If they own it, Embedded Signup injects its existing data into the flow and ignores all other business object properties.&lt;br&gt;&lt;br&gt;If they do not own it, Embedded Signup injects the `business.name`, `business.email`, `business.website`, and `address.country` values, if they are **all** set. If **any** are not set, the flow will display the default business portfolio screen instead.&lt;br&gt;&lt;br&gt;Set to `null` (or omit the `id` property entirely) if you want to create a new business portfolio based on injected `business.name`, `business.email`, `business.website`, and `address.country` values. | `2729063490586005` |
| `&lt;BUSINESS_PORTFOLIO_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required if creating a new business portfolio.**&lt;br&gt;&lt;br&gt;Business portfolio name.&lt;br&gt;&lt;br&gt;If this name matches the name of an existing business portfolio owned by the business customer, the existing portfolio will be used instead (it will be treated as if you assigned the existing portfolio&#039;s ID to the `id` property).&lt;br&gt;&lt;br&gt;This name will also be used as the WhatsApp Business account name, which is only visible in the WhatsApp Manager.&lt;br&gt;&lt;br&gt;Maximum 100 characters. | `Wind &amp; Wool` |
| `&lt;BUSINESS_PORTFOLIO_EMAIL_ADDRESS&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required if creating a new business portfolio.**&lt;br&gt;&lt;br&gt;The business&#039;s email address.&lt;br&gt;&lt;br&gt;This information will appear in the **Meta Business Suite** &gt; **Business portfolio** &gt; **Settings** &gt; **Business info** panel. | `support&#064;windandwool.com` |
| `&lt;BUSINESS_PORTFOLIO_COUNTRY_CALLING_CODE&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | **Required if injecting a business phone number.**&lt;br&gt;&lt;br&gt;Business phone number country calling code. | `1` |
| `&lt;BUSINESS_PORTFOLIO_PHONE_NUMBER&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required if injecting a business phone number.**&lt;br&gt;&lt;br&gt;Business phone number, without country calling code. | `6505559999` |
| `&lt;BUSINESS_PORTFOLIO_WEBSITE&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required if creating a new business portfolio.**&lt;br&gt;&lt;br&gt;The business&#039;s website URL.&lt;br&gt;&lt;br&gt;This information will appear in the **Meta Business Suite** &gt; **Business portfolio** &gt; **Settings** &gt; **Business info** panel. | `https://windandwool.com/` |
| `&lt;BUSINESS_PORTFOLIO_STREET_ADDRESS_LINE_1&gt;`&lt;br&gt;&lt;br&gt;_String_ | The business&#039;s street address, line 1.&lt;br&gt;&lt;br&gt;This information will appear in the **Meta Business Suite** &gt; **Business portfolio** &gt; **Settings** &gt; **Business info** panel. | `1 Hacker Way` |
| `&lt;BUSINESS_PORTFOLIO_STREET_ADDRESS_LINE_2&gt;`&lt;br&gt;&lt;br&gt;_String_ | The business&#039;s street address, line 2.&lt;br&gt;&lt;br&gt;This information will appear in the **Meta Business Suite** &gt; **Business portfolio** &gt; **Settings** &gt; **Business info** panel. | `Suite 1` |
| `&lt;BUSINESS_PORTFOLIO_CITY&gt;`&lt;br&gt;&lt;br&gt;_String_ | The business&#039;s city address.&lt;br&gt;&lt;br&gt;This information will appear in the **Meta Business Suite** &gt; **Business portfolio** &gt; **Settings** &gt; **Business info** panel. | `Menlo Park` |
| `&lt;BUSINESS_PORTFOLIO_STATE&gt;`&lt;br&gt;&lt;br&gt;_String_ | The business&#039;s state address.&lt;br&gt;&lt;br&gt;This information will appear in the **Meta Business Suite** &gt; **Business portfolio** &gt; **Settings** &gt; **Business info** panel. | `California` |
| `&lt;BUSINESS_PORTFOLIO_ZIP_CODE&gt;`&lt;br&gt;&lt;br&gt;_String_ | The business&#039;s zip code address.&lt;br&gt;&lt;br&gt;This information will appear in the **Meta Business Suite** &gt; **Business portfolio** &gt; **Settings** &gt; **Business info** panel. | `94025` |
| `&lt;BUSINESS_PORTFOLIO_COUNTRY&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required if creating a new business portfolio.**&lt;br&gt;&lt;br&gt;Business address [ISO 3166-1 alpha-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) country code. | `US` |
| `&lt;BUSINESS_PORTFOLIO_TIME_ZONE&gt;`&lt;br&gt;&lt;br&gt;_String_ | The business&#039;s time zone in&lt;br&gt;[UTC offset](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones) format. | `UTC-07:00` |

#### Example business object

```js
setup: &#123;
  business: &#123;
    name: &#039;Wind &amp; Wool&#039;,
    email: &#039;support&#064;windandwool.com&#039;,
    website: &#039;https://windandwool.com/&#039;,
    address: &#123;
      streetAddress1: &#039;1 Hacker Way&#039;,
      streetAddress2: &#039;Suite 1&#039;,
      city: &#039;Menlo Park&#039;,
      state: &#039;California&#039;,
      zipPostal: &#039;94025&#039;,
      country: &#039;US&#039;
    &#125;,
    phone: &#123;
      code: 1,
      number: &#039;6505559999&#039;
    &#125;,
    timezone: &#039;UTC-07:00&#039;
  &#125;
&#125;
```

### Pre-verified phone numbers

You can inject a [pre-verified business phone number](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/pre-verified-numbers) ID into Embedded Signup, which will cause Embedded Signup to skip the [phone number addition](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/default-flow#phone-number-addition-screen) and [phone number verification](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/default-flow#phone-number-verification-screen) screens.

If you are injecting a pre-verified phone number along with business portfolio data (either creating a new portfolio or using an existing one), Embedded Signup pre-fills the [business portfolio screen](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/default-flow#business-portfolio-screen) with the pre-verified number:

If you are not injecting business portfolio data along with a pre-verified number ID, the number will appear in the [WABA selection screen](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/default-flow#business-asset-selection-screen):

#### PreVerifiedPhone object syntax

The `preVerifiedPhone` object injects a pre-verified phone number ID, which causes Embedded Signup to skip the phone number addition and verification screens. Use the following syntax:

```html
setup: &#123;
  preVerifiedPhone: &#123;
    ids: [
      &#039;&lt;PRE-VERIFIED_PHONE_NUMBER_ID&gt;&#039;
    ]
  &#125;
&#125;
```

Replace `&lt;PRE-VERIFIED_PHONE_NUMBER_ID&gt;` with a unique, pre-verified business phone number ID.

Note that although the `ids` value accepts an array of strings, if you include more than one pre-verified business phone number ID, only the first ID in the array will appear in the WABA selection screen.

#### Example preVerifiedPhone object

```js
setup: &#123;
  preVerifiedPhone: &#123;
    ids: [
      &#039;106540352242922&#039;
    ]
  &#125;
&#125;
```

### Phone profile data

You can inject the following phone number profile data. This data does not pre-fill any Embedded Signup screens, but Embedded Signup populates the business phone number&#039;s profile in the WhatsApp client with it, which is visible to WhatsApp users.

* Phone number profile display name
* Phone number category
* Phone number description

If you do not include this data, Embedded Signup sets the category to **Other**, and the business customer must set or edit their profile data on their own.

Your customers can do this in the [**WhatsApp Manager** &gt; **Account tools** &gt; **Phone numbers** panel](https://business.facebook.com/latest/whatsapp_manager/phone_numbers/) by selecting their business phone number and accessing the **Profile** tab. You can also provide a way for them to update it programmatically by using the [WhatsApp Business Profile API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/whatsapp-business-profile-api#post-version-phone-number-id-whatsapp-business-profile).

#### Phone object syntax

The `phone` object populates the business phone number&#039;s profile in the WhatsApp client. Use the following syntax:

```html
setup: &#123;
  phone: &#123;
    displayName: &#039;&lt;PHONE_PROFILE_DISPLAY_NAME&gt;&#039;,
    category: &#039;&lt;PHONE_PROFILE_DISPLAY_CATEGORY&gt;&#039;,
    description: &#039;&lt;PHONE_PROFILE_DISPLAY_DESCRIPTION&gt;&#039;
  &#125;
&#125;
```

#### Phone object parameters

| Placeholder | Description | Example value |
| --- | --- | --- |
| `&lt;PHONE_PROFILE_DISPLAY_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Business profile display name, visible to WhatsApp users in the WhatsApp client (see screenshot above). | `Wind &amp; Wool` |
| `&lt;PHONE_PROFILE_DISPLAY_CATEGORY&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Business profile display category.&lt;br&gt;&lt;br&gt;See the vertical field in the [GET /&lt;WHATSAPP_BUSINESS_PHONE_ID&gt;/`whatsapp_business_profile`](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/whatsapp-business-profile-api#fields) endpoint reference for a list of possible values. | `APPAREL` |
| `&lt;PHONE_PROFILE_DISPLAY_DESCRIPTION&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Business phone number profile description.&lt;br&gt;&lt;br&gt;* Maximum 512 characters.&lt;br&gt;* Rendered emojis are supported; however, their Unicode values are not. Emoji Unicode values must be Java- or JavaScript-escape encoded.&lt;br&gt;* Hyperlinks can be included but will not render as clickable links.&lt;br&gt;* Markdown is not supported. | `Bespoke artisan apparel and lifestyle goods from upcoming designers.` |

#### Example phone object

```js
setup: &#123;
  phone: &#123;
    displayName: &#039;Wind &amp; Wool&#039;,
    category: &#039;APPAREL&#039;,
    description: &#039;Bespoke artisan apparel and lifestyle goods from upcoming designers.&#039;
  &#125;
&#125;
```

### WhatsApp Business accounts

If you are injecting a pre-verified phone number, you can also include a WABA ID. This will associate the pre-verified number with the existing WABA instead of with a new one that the business customer would be prompted to create as part of the flow.

#### WhatsAppBusinessAccount object syntax

The `whatsAppBusinessAccount` object associates a pre-verified phone number with an existing WABA, so it does not appear in any pre-filled screen. Use the following syntax:

```html
setup: &#123;
  whatsAppBusinessAccount: &#123;
    ids: &#039;&lt;WABA_ID&gt;&#039;
  &#125;
&#125;
```

Replace `&lt;WABA_ID&gt;` with a unique WABA ID.

#### Example whatsAppBusinessAccount object

This example associates a pre-verified phone number with an existing WABA.

```js
setup: &#123;
  preVerifiedPhone: &#123;
    ids: [
      &#039;106540352242922&#039;
    ]
  &#125;,
  whatsAppBusinessAccount: &#123;
    id: [
      &#039;432428883295692&#039;
    ]
  &#125;
&#125;
```

## Examples

### New business portfolio, pre-verified number, and display profile

```js
// Launch method and callback registration
const launchWhatsAppSignup = () =&gt; &#123;
  FB.login(fbLoginCallback, &#123;
    config_id: &#039;31602279155865&#039;,
    response_type: &#039;code&#039;,
    override_default_response_type: true,
    extras: &#123;
      setup: &#123;
        business: &#123;
          name: &#039;Wind &amp; Wool&#039;,
          email: &#039;support&#064;windandwool.com&#039;,
          website: &#039;https://windandwool.com/&#039;,
          address: &#123;
            streetAddress1: &#039;1 Hacker Way&#039;,
            streetAddress2: &#039;Suite 1&#039;,
            city: &#039;Menlo Park&#039;,
            state: &#039;California&#039;,
            zipPostal: &#039;94025&#039;,
            country: &#039;US&#039;
          &#125;,
          phone: &#123;
            code: 1,
            number: &#039;6505559999&#039;
          &#125;,
          timezone: &#039;UTC-07:00&#039;
        &#125;,
        preVerifiedPhone: &#123;
          ids: [
            &#039;106540352242922&#039;
          ]
        &#125;,
        phone: &#123;
          displayName: &#039;Wind &amp; Wool&#039;,
          category: &#039;APPAREL&#039;,
          description: &#039;Bespoke artisan apparel and lifestyle goods from upcoming designers.&#039;
        &#125;
      &#125;,
      featureType: &#039;&#039;,
      sessionInfoVersion: &#039;3&#039;,
    &#125;
  &#125;);
&#125;
```

### Existing business portfolio, pre-verified number, and display profile

```js
// Launch method and callback registration
const launchWhatsAppSignup = () =&gt; &#123;
  FB.login(fbLoginCallback, &#123;
    config_id: &#039;31602279155865&#039;,
    response_type: &#039;code&#039;,
    override_default_response_type: true,
    extras: &#123;
      setup: &#123;
        business: &#123;
          id: &#039;2729063490586005&#039;
        &#125;,
        preVerifiedPhone: &#123;
          ids: [
            &#039;106540352242922&#039;
          ]
        &#125;,
        phone: &#123;
          displayName: &#039;Wind &amp; Wool&#039;,
          category: &#039;APPAREL&#039;,
          description: &#039;Bespoke artisan apparel and lifestyle goods from upcoming designers.&#039;
        &#125;
      &#125;,
      featureType: &#039;&#039;,
      sessionInfoVersion: &#039;3&#039;,
    &#125;
  &#125;);
&#125;
```
