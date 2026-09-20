# Managing credit lines



**Warning:** **Embedded signup v2 will be deprecated on October 15, 2026.** Migrate your integration to [v4](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/version-4) before that date to avoid disruption. See [Versions](https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/versions) for the full upgrade path.

This document describes how Solution Partners can share and revoke lines of credit with onboarded clients.

**Note:** **Billing Liability Disclosure**

Clients that you onboard through Embedded Signup must be granted access to your line of credit with Meta to pay for WhatsApp Business Platform access. This means that businesses pay you, and you receive an aggregated invoice to pay Meta.

You are the &quot;Bill To Party&quot; for all businesses sharing your credit line. You are liable for and will pay Meta for all WhatsApp Business Platform spend made by these businesses.

You can grant access to your line of credit using the APIs described in this document. You can revoke access to your line of credit for individual businesses within the [Meta Business Suite](https://business.facebook.com/home/accounts) or with a [series of API calls](#revoke-a-shared-credit-line).

## Authentication and authorization

Nearly all credit line related endpoints require your system user access token. In addition, the system user who the token represents must have granted your app the **business_management** permission, and must have been granted an **Admin** or **Financial Editor** role on your business portfolio.

## Get your credit line ID

Nearly all API calls related to credit lines require your credit line ID. Use the [Extended Credits API](https://developers.facebook.com/documentation/ads-commerce/marketing-api/reference/business/extendedcredits#get-version-business-id-extendedcredits) to get your business portfolio&#039;s credit line ID.

### Request syntax

```html
curl &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;BUSINESS_ID&gt;/extendedcredits&#039; \
-H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039;
```

### Request example

```curl
curl &#039;https://graph.facebook.com/v24.0/102289599326934/extendedcredits&#039; \
-H &#039;Authorization: Bearer EAAJi...&#039;
```

### Response

Upon success, the API will return the business portfolio&#039;s extended credit line ID (&quot;credit line ID&quot;).

```json
&#123;
  &quot;data&quot;: [
    &#123;
      &quot;id&quot;: &quot;1972385232742146&quot;
    &#125;
  ]
&#125;
```

## Sharing your credit line

**Warning:** We are currently testing new steps for sharing your credit line with onboarded clients. These steps will eventually replace this step, so if you wish to implement these steps now, refer to the [Alternate method for sharing your credit line](#alternate-method-for-sharing-your-credit-line) below.

Use the [Credit Sharing API](https://developers.facebook.com/docs/marketing-api/reference/extended-credit/whatsapp_credit_sharing_and_attach) to share your credit line with an onboarded client.

### Request syntax

```html
curl -X POST &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;EXTENDED_CREDIT_LINE_ID&gt;/whatsapp_credit_sharing_and_attach?waba_currency=&lt;CUSTOMER_BUSINESS_CURRENCY&gt;&amp;waba_id=&lt;CUSTOMER_WABA_ID&gt;&#039; \
-H &#039;Authorization: Bearer &lt;SYSTEM_TOKEN&gt;&#039;
```

### Request parameters

| Placeholder | Description | Example value |
| --- | --- | --- |
| `&lt;CUSTOMER_BUSINESS_CURRENCY&gt;` | **Required.**&lt;br&gt;&lt;br&gt;The business&#039;s currency, as a three-letter currency code. Supported values are:&lt;br&gt;&lt;br&gt;* `AUD`&lt;br&gt;* `EUR`&lt;br&gt;* `GBP`&lt;br&gt;* `IDR`&lt;br&gt;* `INR`&lt;br&gt;* `USD`&lt;br&gt;&lt;br&gt;This currency is used for invoicing and corresponds to [pricing](https://developers.facebook.com/documentation/business-messaging/whatsapp/pricing) rates. | `USD` |
| `&lt;CUSTOMER_WABA_ID&gt;` | **Required.**&lt;br&gt;&lt;br&gt;The customer&#039;s WABA ID. | `102290129340398` |
| `&lt;EXTENDED_CREDIT_LINE_ID&gt;` | **Required.**&lt;br&gt;&lt;br&gt;Your extended credit line ID. | `1972385232742146` |
| `&lt;SYSTEM_TOKEN&gt;` | **Required.**&lt;br&gt;&lt;br&gt;Your system token. | `EAAAN6tcBzAUBOZC82CW7iR2LiaZBwUHS4Y7FDtQxRUPy1PHZClDGZBZCgWdrTisgMjpFKiZAi1FBBQNO2IqZBAzdZAA16lmUs0XgRcCf6z1LLxQCgLXDEpg80d41UZBt1FKJZCqJFcTYXJvSMeHLvOdZwFyZBrV9ZPHZASSqxDZBUZASyFdzjiy2A1sippEsF4DVV5W2IlkOSr2LrMLuYoNMYBy8xQczzOKDOMccqHEZD` |

### Response

Upon success:

```html
&#123;
  &quot;allocation_config_id&quot;: &quot;58501441721238&quot;,
  &quot;waba_id&quot;: &quot;102290129340398&quot;
&#125;
```

### Response parameters

| Placeholder | Description | Example value |
| --- | --- | --- |
| `&lt;ALLOCATION_CONFIGURATION_ID&gt;` | The extended credit line&#039;s allocation configuration ID.&lt;br&gt;&lt;br&gt;Save this ID if you want to [verify](#verifying-shared-status) that your credit line has been shared with the customer. | `58501441721238` |
| `&lt;CUSTOMER_WABA_ID&gt;` | The customer&#039;s WABA ID. | `102290129340398` |

## Alternate method for sharing your credit line

**Warning:** We are currently testing new steps for sharing your credit line with onboarded clients. These steps are described below, and will eventually replace the [current method](#sharing-your-credit-line) for sharing your credit line with an onboarded client.

### Step 1: Get your customer&#039;s business portfolio ID

Use the [WhatsApp Business Account API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/whatsapp-business-account-api#get-version-waba-id) and request the `owner_business_info` field to get the client&#039;s business portfolio ID.

#### Request syntax

```html
curl --get &#039;https://graph.facebook.com/v21.0/&lt;WABA_ID&gt;?fields=owner_business_info&#039; \
-H &#039;Authorization: Bearer &lt;BUSINESS_TOKEN&gt;&#039;
```

#### Request parameters

| Placeholder | Description | Example value |
| --- | --- | --- |
| `&lt;BUSINESS_TOKEN&gt;` | **Required.**&lt;br&gt;&lt;br&gt;The customer&#039;s business token. | `EAAAN6tcBzAUBOwtDtTfmZCJ9n3FHpSDcDTH86ekf89XnnMZAtaitMUysPDE7LES3CXkA4MmbKCghdQeU1boHr0QZA05SShiILcoUy7ZAb2GE7hrUEpYHKLDuP2sYZCURkZCHGEvEGjScGLHzC4KDm8tq2slt4BsOQE1HHX8DzHahdT51MRDqBw0YaeZByrVFZkVAoVTxXUtuKgDDdrmJQXMnI4jqJYetsZCP1efj5ygGscZBm4OvvuCYB039ZAFlyNn` |
| `&lt;WABA_ID&gt;` | **Required.**&lt;br&gt;&lt;br&gt;The customer&#039;s WABA ID. | `102290129340398` |

#### Response syntax

Upon success:

```html
&#123;
  &quot;owner_business_info&quot;: &#123;
    &quot;name&quot;: &quot;&lt;BUSINESS_PORTFOLIO_NAME&gt;&quot;,
    &quot;id&quot;: &quot;&lt;BUSINESS_PORTFOLIO_ID&gt;&quot;
  &#125;,
  &quot;id&quot;: &quot;&lt;WABA_ID&gt;&quot;
&#125;
```

#### Response parameters

| Placeholder | Description | Example value |
| --- | --- | --- |
| `&lt;BUSINESS_PORTFOLIO_ID&gt;` | The customer&#039;s business portfolio ID. | `2729063490586005` |
| `&lt;BUSINESS_PORTFOLIO_NAME&gt;` | The customer&#039;s business portfolio name. | `Wind &amp; Wool` |
| `&lt;WABA_ID&gt;` | The customer&#039;s WABA ID. | `102290129340398` |

### Step 2: Share your credit line with the customer&#039;s business

Use the [Credit Sharing API](https://developers.facebook.com/docs/marketing-api/reference/extended-credit/whatsapp_credit_sharing) and your **system token** to indicate your intent to share your credit line with the customer&#039;s business portfolio.

#### Request syntax

```html
curl -X POST &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;EXTENDED_CREDIT_LINE_ID&gt;/whatsapp_credit_sharing?receiving_business_id=&lt;BUSINESS_PORTFOLIO_ID&gt;&#039; \
-H &#039;Authorization: Bearer &lt;SYSTEM_TOKEN&gt;&#039;
```

#### Request parameters

| Placeholder | Description | Example value |
| --- | --- | --- |
| `&lt;EXTENDED_CREDIT_LINE_ID&gt;` | **Required.**&lt;br&gt;&lt;br&gt;Your extended credit line ID. | `5985499441566032` |
| `&lt;BUSINESS_PORTFOLIO_ID&gt;` | **Required.**&lt;br&gt;&lt;br&gt;The customer&#039;s business portfolio ID. | `2729063490586005` |
| `&lt;SYSTEM_TOKEN&gt;` | **Required.**&lt;br&gt;&lt;br&gt;Your system user access token. | `EAAAN6tcBzAUBOZC82CW7iR2LiaZBwUHS4Y7FDtQxRUPy1PHZClDGZBZCgWdrTisgMjpFKiZAi1FBBQNO2IqZBAzdZAA16lmUs0XgRcCf6z1LLxQCgLXDEpg80d41UZBt1FKJZCqJFcTYXJvSMeHLvOdZwFyZBrV9ZPHZASSqxDZBUZASyFdzjiy2A1sippEsF4DVV5W2IlkOSr2LrMLuYoNMYBy8xQczzOKDOMccqHEZD` |

#### Response example

Upon success:

```html
&#123;
  &quot;success&quot;: true,
  &quot;allocation_config_id&quot;: &quot;58501441721238&quot;
&#125;
```

#### Response parameters

| Placeholder | Description | Example value |
| --- | --- | --- |
| `&lt;ALLOCATION_CONFIG_ID&gt;` | The extended credit line&#039;s allocation configuration ID. | `58501441721238` |

### Step 3: Attach your credit line to the customer&#039;s WABA

Use the [POST /&lt;EXTENDED_CREDIT_LINE_ID&gt;/whatsapp_credit_attach](https://developers.facebook.com/docs/marketing-api/reference/extended-credit/whatsapp_credit_attach) endpoint to attach your credit line to the customer&#039;s WABA.

Note: Credit lines cannot be changed after being attached to a WABA. If the WABA needs a different credit line, a new WABA must be created and the new credit line can then be attached to it.

#### Request syntax

```html
curl -X POST &#039;https://graph.facebook.com/v21.0/&lt;EXTENDED_CREDIT_LINE_ID&gt;/whatsapp_credit_attach?waba_currency=&lt;WABA_CURRENCY&gt;&amp;waba_id=&lt;WABA_ID&gt;&#039; \
-H &#039;Authorization: Bearer &lt;BUSINESS_TOKEN&gt;&#039;
```

#### Request parameters

| Placeholder | Description | Example value |
| --- | --- | --- |
| `&lt;BUSINESS_TOKEN&gt;` | **Required.**&lt;br&gt;&lt;br&gt;The customer&#039;s business token. | `EAAAN6tcBzAUBOwtDtTfmZCJ9n3FHpSDcDTH86ekf89XnnMZAtaitMUysPDE7LES3CXkA4MmbKCghdQeU1boHr0QZA05SShiILcoUy7ZAb2GE7hrUEpYHKLDuP2sYZCURkZCHGEvEGjScGLHzC4KDm8tq2slt4BsOQE1HHX8DzHahdT51MRDqBw0YaeZByrVFZkVAoVTxXUtuKgDDdrmJQXMnI4jqJYetsZCP1efj5ygGscZBm4OvvuCYB039ZAFlyNn` |
| `&lt;EXTENDED_CREDIT_LINE_ID&gt;` | **Required.**&lt;br&gt;&lt;br&gt;Your extended credit line ID. | `5985499441566032` |
| `&lt;WABA_ID&gt;` | **Required.**&lt;br&gt;&lt;br&gt;The customer&#039;s WABA ID. | `102290129340398` |
| `&lt;WABA_CURRENCY&gt;` | **Required.**&lt;br&gt;&lt;br&gt;The customer&#039;s business currency. | `USD` |

#### Response syntax

Upon success:

```html
&#123;
  &quot;success&quot;: true,
  &quot;waba_id&quot;: &quot;&lt;WABA_ID&gt;&quot;,
  &quot;allocation_config_id&quot;: &quot;&lt;ALLOCATION_CONFIG_ID&gt;&quot;
&#125;
```

#### Response parameters

| Placeholder | Description | Example value |
| --- | --- | --- |
| `&lt;ALLOCATION_CONFIG_ID&gt;` | The extended credit line&#039;s allocation configuration ID.&lt;br&gt;&lt;br&gt;Save this ID if you want to [verify](#verifying-shared-status) that your credit line has been shared with the customer. | `58501441721238` |
| `&lt;WABA_ID&gt;` | The customer&#039;s WABA ID. | `102290129340398` |

Your credit line should now be shared with the client. If you want to verify that it has in fact been shared, see [Verifying that your credit line has been shared with a client](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/share-and-revoke-credit-lines#step-3--verify-credit-line-was-shared).

## Verifying shared status

Perform the following queries if you want to make sure that your credit line has been shared with an onboarded client.

### Step 1: Get credit line&#039;s receiving credential

Use the [GET /&lt;EXTENDED_CREDIT_ALLOCATION_ID&gt;](https://developers.facebook.com/docs/graph-api/reference/extended-credit-allocation-config) endpoint to request the `receiving_credential` field on your extended credit allocation ID (returned when you [shared your credit line](#sharing-your-credit-line) with the client).

### Request syntax

```html
curl &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;EXTENDED_CREDIT_ALLOCATION_ID&gt;?fields=receiving_credential&#039; \
-H &#039;Authorization: Bearer &lt;SYSTEM_TOKEN&gt;&#039;
```

### Response syntax

Upon success:

```html
&#123;
  &quot;receiving_credential&quot;: &#123;
    &quot;id&quot;: &quot;&lt;RECEIVING_CREDENTIAL_ID&gt;&quot;
  &#125;,
  &quot;id&quot;: &quot;&lt;ALLOCATION_CONFIGURATION_ID&gt;&quot;
&#125;
```

### Step 2: Get WABA&#039;s primary funding ID

Use the [WhatsApp Business Account API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/whatsapp-business-account-api#get-version-waba-id) to request the `primary_funding_id` on the customer&#039;s WABA ID.

### Request syntax

```html
curl &#039;https://graph.facebook.com/v21.0/&lt;CUSTOMER_WABA_ID&gt;/?fields=primary_funding_id&#039; \
-H &#039;Authorization: Bearer &lt;CUSTOMER_BUSINESS_TOKEN&gt;&#039;
```

### Response syntax

Upon success:

```html
&#123;
  &quot;primary_funding_id&quot;: &quot;&lt;PRIMARY_FUNDING_ID&gt;&quot;,
  &quot;id&quot;: &quot;&lt;CUSTOMER_WABA_ID&gt;&quot;
&#125;
```

### Step 3: Compare IDs

Compare the receiving credential ID to the primary funding ID. If the values match, your credit line has been shared correctly with the client&#039;s WABA.

## Revoke a shared credit line

These are the steps needed to remove the shared line of credit if you need to revoke access for any reason. You can revoke your credit line whenever you feel the need to and/or when your client removes you as a partner from their WhatsApp Business Account.

When you revoke a credit line from a customer&#039;s account, that revocation applies to all WABAs that belong to the customer&#039;s business **and** have been shared with you, the Solution Partner.

### Step 1: Get your credit line ID

#### Example request

```curl
curl -i -X GET &quot;https://graph.facebook.com/v25.0/105954558954427/
  extendedcredits?fields=id,legal_entity_name&amp;
  access_token=EAAFl...&quot;
```

#### Example response

```json
&#123;
  &quot;data&quot;: [
    &#123;
      &quot;id&quot;: &quot;1972385232742146&quot;,   //Credit line ID
      &quot;legal_entity_name&quot;: &quot;Your Legal Entity&quot;,
    &#125;
  ]
&#125;
```


### Step 2: Get the customer&#039;s business portfolio ID

If the WhatsApp Business Account is currently shared with the Solution Partner, get the client’s business ID from the shared WhatsApp Business Account.

In the following example, use the ID for the assigned WhatsApp Business Account.

Request:

```curl
curl -i -X GET &quot;https://graph.facebook.com/v25.0/
  &lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;?fields=owner_business_info&amp;
  access_token=&lt;ACCESS_TOKEN&gt;&quot;
```

Response:

```json
&#123;
  &quot;owner_business_info&quot;: &#123;
    &quot;name&quot;: &quot;Client Business Name&quot;,
    &quot;id&quot;: &quot;1972385232742147&quot;
  &#125;,
&#125;
```

If the WhatsApp Business Account was unshared with the Solution Partner or the client business removed the Solution Partner as a partner from the WhatsApp Business Account, you cannot access the client&#039;s business ID from the above API call. See [Unshared WhatsApp Business Account](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/share-and-revoke-credit-lines#unshared-whatsapp-business-accounts) for information.


### Step 3: Get the customer&#039;s credit sharing record

In the following example, use your credit line ID as the extended credit ID.

Request:

```html
curl -i -X GET &quot;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;EXTENDED_CREDIT_ID&gt;/
  owning_credit_allocation_configs?
  receiving_business_id=&lt;CLIENT_BUSINESS_ID&gt;&amp;
  fields=id,receiving_business&amp;
  access_token=&lt;SYSTEM_USER_ACCESS_TOKEN&gt;&quot;
```

Response:

```json
&#123;
  &quot;id&quot;: &quot;1972385232742140&quot;, // Allocation config (i.e., credit sharing) id
  &quot;receiving_business&quot;: &#123;
    &quot;name&quot;: &quot;Client Business Name&quot;
    &quot;id&quot;: &quot;1972385232742147&quot;
  &#125;,
&#125;
```


### Step 4: Revoke credit sharing

Request:

```curl
curl -i -X DELETE &quot;https://graph.facebook.com/v25.0/
  &#123;allocation-config-id&#125;?
  access_token=&#123;system-user-access-token&#125;&quot;
```

Response:

```json
&#123;
  &quot;success&quot;: true
&#125;
```

### Step 5: Verify credit sharing was revoked (Optional)

Request:

```curl
curl -i -X GET &quot;https://graph.facebook.com/v25.0/
  &#123;allocation-config-id&#125;?fields=receiving_business,request_status&amp;
  access_token=&#123;system-user-access-token&#125;&quot;
```

Response:

```json
&#123;
  &quot;receiving_business&quot;: &#123;
    &quot;name&quot;: &quot;Customer Business Name&quot;,
    &quot;id&quot;: &quot;1972385232742147&quot;
  &#125;,
  &quot;request_status&quot;: &quot;DELETED&quot;
&#125;
```

## Troubleshooting

### Unshared WhatsApp Business Accounts

If a client unshares their WABA with you, or removes you as a partner from their WhatsApp Business Account, you will not be able to get their business portfolio ID via API.

Instead, you can get their portfolio ID from the email notification that was sent to admins of the business portfolio, when the client removed you as a partner, or unshared their WABA.

When WABA is unshared with you, all messaging for that WABA is blocked to protect your credit line. For complete security, we recommend that you revoke your credit line from the client&#039;s WABA as soon as it has been unshared with you.

## See Also

- Reference: [Business](https://developers.facebook.com/documentation/ads-commerce/marketing-api/reference/business)
- Reference: [WhatsApp Business Account](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/whatsapp-business-account-api)
- Reference: [Extended Credit](https://developers.facebook.com/docs/marketing-api/reference/extended-credit)
