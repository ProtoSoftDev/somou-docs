# Partner-led Business Verification


**Warning:** This feature is currently only available to approved **Select Solution** and **Premier** Solution Partners. See our [Sign up for partner-led business verification](https://www.facebook.com/business/help/1091073752691122) Help Center article to learn how to request approval.

This document describes how to create business verification submissions for clients who have been onboarded via Embedded Signup.

If you are an approved Solution Partner, you can gather required business verification documentation from your onboarded clients and submit their business for verification on their behalf. Decisions on submissions created in this way can be made in minutes instead of days.

## Requirements

* You must already be an approved **Select Solution** or **Premier** Solution Partner, and [approved for access](https://www.facebook.com/business/help/1091073752691122)
* Your [system user access token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#system-user-access-tokens)
* The system user whose system token you are using must be an admin on your business portfolio (see our [About business portfolio access](https://www.facebook.com/business/help/442345745885606) Help Center article)
* The system user whose system token you are using must have granted your app the **business_management** permission
* The client&#039;s business portfolio ID ([provided by the client](https://www.facebook.com/business/help/1181250022022158?id=180505742745347) or returned via API by requesting the `owner_business_info` field on the client&#039;s WABA ID, using their [business token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#business-integration-system-user-access-tokens))

## Limitations

You are limited to three submissions for a given client. If all three submissions are rejected, the client must complete business verification on their own. If your submission is rejected three times, share the following Help Center article with the client:

[How to Verify Your Business on Meta](https://www.facebook.com/business/help/2058515294227817?id=180505742745347)

## Support

If you need help with partner-led business verification, open a Direct Support ticket:

1. Go to [Direct Support](https://business.facebook.com/direct-support/).
1. Click **Ask a Question**.
1. Under **Topic** select **WABiz: Onboarding**.
1. Click **Request type** and select **Partner-led Business Verification for WhatsApp**.

## Supported documents

See the following Help Center article for a list of business documents that Meta accepts:

[Upload official documents to verify your business](https://www.facebook.com/business/help/159334372093366)

## Turnaround time

The average turnaround time for a submission is 5 minutes, but can take several hours. If you do not receive a webhook notifying of the outcome of a submission after 24 hours, please open a Direct Support ticket.

## Webhooks

Submission decisions are communicated via **account_update** webhook, so make sure your app is subscribed to the **account_update** webhook field, and your app is [subscribed to webhooks on the client&#039;s WhatsApp Business account](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/manage-webhooks#subscribe-to-a-whatsapp-business-account).

### Example webhook

```html
&#123;
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;&quot;,
      &quot;time&quot;: &lt;WEBHOOK_SENT_TIMESTAMP&gt;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;event&quot;: &quot;PARTNER_CLIENT_CERTIFICATION_STATUS_UPDATE&quot;,
            &quot;partner_client_certification_info&quot;: &#123;
              &quot;client_business_id&quot;: &quot;&lt;CUSTOMER_BUSINESS_PORTFOLIO_ID&gt;&quot;,
              &quot;status&quot;: &quot;&lt;STATUS&gt;&quot;,
              &quot;rejection_reasons&quot;: [
                &quot;&lt;REJECTION_REASONS&gt;&quot;
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

### Webhook parameters

| Placeholder | Description | Example value |
| --- | --- | --- |
| `&lt;CUSTOMER_BUSINESS_PORTFOLIO_ID&gt;` | Client&#039;s business portfolio ID. | `2729063490586005` |
| `&lt;REJECTION_REASONS&gt;` | Description of rejection reasons. Note that this parameter will be present even if the submission was rejected, but its value will be set to `NONE`.&lt;br&gt;&lt;br&gt;See the `rejection_reasons` field on the [WhatsApp Business Partner Client Verification Submission](https://developers.facebook.com/docs/graph-api/reference/whats-app-business-partner-client-verification-submission#Reading) node reference for a list of possible values and their descriptions. | `LEGAL_NAME_NOT_FOUND_IN_DOCUMENTS` |
| `&lt;STATUS&gt;` | Business verification status. Values can be:&lt;br&gt;&lt;br&gt;`APPROVED` — Indicates the client&#039;s business has been verified.&lt;br&gt;&lt;br&gt;`FAILED` — Indicates Meta is unable to verify the client&#039;s business based on the submitted business information. | `APPROVED` |
| `&lt;WABA&gt;` | Client&#039;s WABA ID. | `486585971195941` |
| `&lt;WEBHOOK_SENT_TIMESTAMP&gt;` | Unix timestamp indicating when the webhook was sent. | `1730752761` |

## Submitting a business for verification

Use the [POST /&lt;BUSINESS_PORTFOLIO_ID&gt;/self_certify_whatsapp_business](https://developers.facebook.com/documentation/ads-commerce/marketing-api/reference/business/self_certify_whatsapp_business#Creating) endpoint to initiate business verification for a client who has onboarded via your implementation of Embedded Signup.

### Request

```html
curl &#039;https://graph.facebook.com/v21.0/&lt;BUSINESS_PORTFOLIO_ID&gt;/self_certify_whatsapp_business&#039; \
-H &#039;Authorization: Bearer &lt;SYSTEM_TOKEN&gt;&#039; \
-F &#039;end_business_id=&quot;&lt;CUSTOMER_BUSINESS_PORTFOLIO_ID&gt;&quot;&#039; \
-F &#039;business_documents[]=&#064;&quot;&lt;DOCUMENT_PATH&gt;&quot;&#039; \
-F &#039;business_documents[]=&#064;&quot;&lt;DOCUMENT_PATH&gt;&quot;&#039; \
-F &#039;business_documents[]=&#064;&quot;&lt;DOCUMENT_PATH&gt;&quot;&#039;
```

### Request parameters

| Placeholder | Description | Example value |
| --- | --- | --- |
| `&lt;BUSINESS_PORTFOLIO_ID&gt;` | **Required.**&lt;br&gt;&lt;br&gt;Your business portfolio ID. | `506914307656634` |
| `&lt;CUSTOMER_BUSINESS_PORTFOLIO_ID&gt;` | **Required.**&lt;br&gt;&lt;br&gt;The client&#039;s business portfolio ID. | `2729063490586005` |
| `&lt;DOCUMENT_PATH&gt;` | **Required.**&lt;br&gt;&lt;br&gt;Path to the client&#039;s business document that you are submitting on their behalf.&lt;br&gt;&lt;br&gt;You can submit a maximum of 3 documents (the example request above submits 3). Use one parameter per document.&lt;br&gt;&lt;br&gt;The maximum size of each document is 5 MB.&lt;br&gt;&lt;br&gt;Supported file types:&lt;br&gt;&lt;br&gt;* PDF&lt;br&gt;* JPEG&lt;br&gt;* JPG&lt;br&gt;* PNG&lt;br&gt;&lt;br&gt;See our [Upload official documents to verify your business](https://www.facebook.com/business/help/159334372093366) Help Center article for documents Meta accepts. | `NP7sEWs3x/wind_and_wool_bank_statement_04302024.txt` |
| `&lt;SYSTEM_TOKEN&gt;` | **Required.**&lt;br&gt;&lt;br&gt;Your system user access token. | `EAAAN6tcBzAUBOZC82CW7iR2LiaZBwUHS4Y7FDtQxRUPy1PHZClDGZBZCgWdrTisgMjpFKiZAi1FBBQNO2IqZBAzdZAA16lmUs0XgRcCf6z1LLxQCgLXDEpg80d41UZBt1FKJZCqJFcTYXJvSMeHLvOdZwFyZBrV9ZPHZASSqxDZBUZASyFdzjiy2A1sippEsF4DVV5W2IlkOSr2LrMLuYoNMYBy8xQczzOKDOMccqHEZD` |

### Response

Upon success:

```html
&#123;
  &quot;success&quot;: true,
  &quot;message&quot;: &quot;Your request has been received and will be reviewed shortly.&quot;,
  &quot;verification_attempts&quot;: &lt;VERIFICATION_ATTEMPTS&gt;
&#125;
```

### Response parameters

| Placeholder | Description | Example value |
| --- | --- | --- |
| `&lt;VERIFICATION_ATTEMPTS&gt;` | Count of business verification submissions you have initiated on behalf of the client. | `1` |

## Getting submission status

Use the [GET /&lt;BUSINESS_PORTFOLIO_ID&gt;/self_certified_whatsapp_business_submissions](https://developers.facebook.com/documentation/ads-commerce/marketing-api/reference/business/self_certified_whatsapp_business_submissions#Reading) endpoint to get the verification status of submissions you have created for a single client, or for all of your clients.

### Request

```html
curl &#039;https://graph.facebook.com/v17.0/&lt;BUSINESS_PORTFOLIO_ID&gt;/self_certified_whatsapp_business_submissions?fields=end_business_id=&lt;CUSTOMER_BUSINESS_PORTFOLIO_ID&gt;&#039; \
-H &#039;Authorization: Bearer &lt;SYSTEM_TOKEN&gt;&#039;
```

### Request parameters

| Placeholder | Description | Example value |
| --- | --- | --- |
| `&lt;CUSTOMER_BUSINESS_PORTFOLIO_ID&gt;` | **Optional.**&lt;br&gt;&lt;br&gt;The client&#039;s business portfolio ID.&lt;br&gt;&lt;br&gt;Include this parameter if you only want to get data on submissions you have created for the business identified by the client&#039;s business portfolio ID. | `2729063490586005` |
| `&lt;SYSTEM_TOKEN&gt;` | **Required.**&lt;br&gt;&lt;br&gt;Your system user access token. | `EAAAN6tcBzAUBOZC82CW7iR2LiaZBwUHS4Y7FDtQxRUPy1PHZClDGZBZCgWdrTisgMjpFKiZAi1FBBQNO2IqZBAzdZAA16lmUs0XgRcCf6z1LLxQCgLXDEpg80d41UZBt1FKJZCqJFcTYXJvSMeHLvOdZwFyZBrV9ZPHZASSqxDZBUZASyFdzjiy2A1sippEsF4DVV5W2IlkOSr2LrMLuYoNMYBy8xQczzOKDOMccqHEZD` |

### Response

Upon success, the endpoint returns an array of [WhatsApp Business Partner Client Verification Submission](https://developers.facebook.com/docs/graph-api/reference/whats-app-business-partner-client-verification-submission) nodes, with default fields on each node.

```html
&#123;
  &quot;data&quot;: [

    // Structure for pending or approved submissions
    &#123;
      &quot;verification_status&quot;: &quot;&lt;VERIFICATION_STATUS&gt;&quot;,
      &quot;submitted_time&quot;: &quot;&lt;SUBMISSION_TIMESTAMP&gt;&quot;,
      &quot;update_time&quot;: &quot;&lt;UPDATE_TIMESTAMP&gt;&quot;,
      &quot;client_business_id&quot;: &quot;&lt;CUSTOMER_BUSINESS_PORTFOLIO_ID&gt;&quot;,
      &quot;submitted_info&quot;: &#123;
        &quot;business_vertical&quot;: &quot;&lt;CUSTOMER_BUSINESS_VERTICAL&gt;&quot;
      &#125;,
      &quot;id&quot;: &quot;&lt;SUBMISSION_ID&gt;&quot;
    &#125;,

    // Structure for rejected submissions
    &#123;
      &quot;verification_status&quot;: &quot;&lt;VERIFICATION_STATUS&gt;&quot;,
      &quot;rejection_reasons&quot;: [
        &quot;&lt;REJECTION_REASON&gt;&quot;,
        &quot;&lt;REJECTION_REASON&gt;&quot;
      ],
      &quot;submitted_time&quot;: &quot;&lt;SUBMISSION_TIMESTAMP&gt;&quot;,
      &quot;update_time&quot;: &quot;&lt;UPDATE_TIMESTAMP&gt;&quot;,
      &quot;client_business_id&quot;: &quot;&lt;CUSTOMER_BUSINESS_PORTFOLIO_ID&gt;&quot;,
      &quot;submitted_info&quot;: &#123;&#125;,
      &quot;id&quot;: &quot;&lt;SUBMISSION_ID&gt;&quot;
    &#125;,

    // Additional objects describing each submission would follow

  ],
  &quot;paging&quot;: &#123;
    &quot;cursors&quot;: &#123;
      &quot;before&quot;: &quot;&lt;BEFORE_CURSOR&gt;&quot;,
      &quot;after&quot;: &quot;&lt;AFTER_CURSOR&gt;&quot;
    &#125;,
    &quot;next&quot;: &quot;&lt;URL_TO_FETCH_NEXT_RESULT_SET&gt;&quot;
  &#125;
&#125;
```

### Response parameters

See the [WhatsApp Business Partner Client Verification Submission](https://developers.facebook.com/docs/graph-api/reference/whats-app-business-partner-client-verification-submission) node reference for descriptions of returned fields and parameter values.

## Get business verification status

If you wish, you can use the [GET /&lt;BUSINESS_PORTFOLIO_ID&gt;](https://developers.facebook.com/documentation/ads-commerce/marketing-api/reference/business#Reading) endpoint and request the `verification_status` field on the client&#039;s business portfolio ID to see its verification status (alternatively, you can request the `business_verification_status` field on the client&#039;s WABA ID using their business token).

### Request

```html
curl &#039;https://graph.facebook.com/v21.0/&lt;BUSINESS_PORTFOLIO_ID&gt;?fields=verification_status&#039; \
-H &#039;Authorization: Bearer &lt;BUSINESS_TOKEN&gt;&#039;
```

### Request parameters

| Placeholder | Description | Example value |
| --- | --- | --- |
| `&lt;BUSINESS_PORTFOLIO_ID&gt;` | **Required.**&lt;br&gt;&lt;br&gt;The client&#039;s business portfolio ID. | `2729063490586005` |
| `&lt;BUSINESS_TOKEN&gt;` | **Required.**&lt;br&gt;&lt;br&gt;The client&#039;s business token. | `EAAAN6tcBzAUBOwtDtTfmZCJ9n3FHpSDcDTH86ekf89XnnMZAtaitMUysPDE7LES3CXkA4MmbKCghdQeU1boHr0QZA05SShiILcoUy7ZAb2GE7hrUEpYHKLDuP2sYZCURkZCHGEvEGjScGLHzC4KDm8tq2slt4BsOQE1HHX8DzHahdT51MRDqBw0YaeZByrVFZkVAoVTxXUtuKgDDdrmJQXMnI4jqJYetsZCP1efj5ygGscZBm4OvvuCYB039ZAFlyNn` |

### Response

```html
&#123;
  &quot;verification_status&quot;: &quot;&lt;VERIFICATION_STATUS&gt;&quot;,
  &quot;id&quot;: &quot;&lt;BUSINESS_PORTFOLIO_ID&gt;&quot;
&#125;
```

### Response parameters

| Placeholder | Description | Example value |
| --- | --- | --- |
| `&lt;BUSINESS_PORTFOLIO_ID&gt;` | The client&#039;s business portfolio ID. | `2729063490586005` |
| `&lt;VERIFICATION_STATUS&gt;` | The business portfolio&#039;s verification status.&lt;br&gt;&lt;br&gt;See the `verification_status` field on the [Business](https://developers.facebook.com/documentation/ads-commerce/marketing-api/reference/business#Reading) node reference for a list of possible values. | `verified` |

