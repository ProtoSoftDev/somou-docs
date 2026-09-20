# partner_solutions webhook reference



This reference describes trigger events and payload contents for the WhatsApp Business account **partner_solutions** webhook.

The **partner_solutions webhook** describes changes to the status of a [Multi-Partner Solution](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/multi-partner-solutions).


## Triggers

- A multi-partner solution is saved as a draft (solution_status: `DRAFT`).
- A multi-partner solution request is sent to a partner (solution_status: `INITIATED`).
- A multi-partner solution partner accepts a solution request (solution_status: `ACTIVE`).
- A multi-partner solution partner rejects a solution request (solution_status: `REJECTED`).
- A multi-partner solution partner requests deactivation of a solution.
- A multi-partner solution is deactivated (solution_status: `DEACTIVATED`).

## Syntax

```html
&#123;
  &quot;entry&quot;: [
    &#123;
      &quot;changes&quot;: [
        &#123;
          &quot;field&quot;: &quot;partner_solutions&quot;,
          &quot;value&quot;: &#123;
            &quot;event&quot;: &quot;&lt;EVENT&gt;&quot;,
            &quot;solution_id&quot;: &quot;&lt;SOLUTION_ID&gt;&quot;,
            &quot;solution_status&quot;: &quot;&lt;SOLUTION_STATUS&gt;&quot;
          &#125;
        &#125;
      ],
      &quot;id&quot;: &quot;&lt;BUSINESS_PORTFOLIO_ID&gt;&quot;,
      &quot;time&quot;: &lt;WEBHOOK_TRIGGER_TIMESTAMP&gt;
    &#125;
  ],
  &quot;object&quot;: &quot;whatsapp_business_account&quot;
&#125;
```

## Parameters

| Placeholder | Description | Example value |
| --- | --- | --- |
| `&lt;BUSINESS_PORTFOLIO_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | Business portfolio ID. | `506914307656634` |
| `&lt;EVENT&gt;`&lt;br&gt;&lt;br&gt;_String_ | Change event. Values can be:&lt;br&gt;&lt;br&gt;`SOLUTION_CREATED` - Indicates a new solution was saved as a draft or sent as a request to a partner.&lt;br&gt;&lt;br&gt;`SOLUTION_UPDATED` - Indicates an existing solution has been updated. | `SOLUTION_CREATED` |
| `&lt;SOLUTION_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | Solution ID. | `774485461512159` |
| `&lt;SOLUTION_STATUS&gt;`&lt;br&gt;&lt;br&gt;_String_ | Solution status. Values can be:&lt;br&gt;&lt;br&gt;`ACTIVE` - The solution partner accepted the solution request and the solution can now be used.&lt;br&gt;&lt;br&gt;`DEACTIVATED` - The solution is deactivated.&lt;br&gt;&lt;br&gt;`DRAFT` - The solution is drafted but an invitation request has not been sent to a partner.&lt;br&gt;&lt;br&gt;`INITIATED` - The solution is created and the invitation request sent, but it has not been accepted or rejected yet.&lt;br&gt;&lt;br&gt;`PENDING_DEACTIVATION` - The solution owner requested deactivation of the solution but the solution partner has yet to accept or decline the deactivation request.&lt;br&gt;&lt;br&gt;`REJECTED` - The solution partner has rejected the solution request. | `INITIATED` |
| `&lt;WEBHOOK_TRIGGER_TIMESTAMP&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | Unix timestamp indicating when the webhook was triggered. | `1739321024` |

## Example

```json
&#123;
  &quot;entry&quot;: [
    &#123;
      &quot;changes&quot;: [
        &#123;
          &quot;field&quot;: &quot;partner_solutions&quot;,
          &quot;value&quot;: &#123;
            &quot;event&quot;: &quot;SOLUTION_CREATED&quot;,
            &quot;solution_id&quot;: &quot;774485461512159&quot;,
            &quot;solution_status&quot;: &quot;INITIATED&quot;
          &#125;
        &#125;
      ],
      &quot;id&quot;: &quot;506914307656634&quot;,
      &quot;time&quot;: 1739321024
    &#125;
  ],
  &quot;object&quot;: &quot;whatsapp_business_account&quot;
&#125;
```
