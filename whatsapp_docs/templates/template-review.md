# Template review



Cloud API reviews templates and variable parameters using machine learning to protect the security and integrity of Cloud API services. When Cloud API reviews templates and variable text, no information is shared with WhatsApp.

When you submit a template creation request, the content undergoes validation through a combination of automated systems and manual reviews. This process ensures that the template complies with WhatsApp&#039;s policies and quality standards. Templates that contain spam, scam-like content, or violate WhatsApp policies are rejected during this review process.

## Approval process

Once you have created your template you can submit it for approval. It can take up to 24 hours for an approval decision to be made. Once a decision has been made, a notification appears in your WhatsApp Manager and Meta sends an email to your Meta Business Suite admins. Meta also sends a [message_template_status_change](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/message_template_status_update) webhook.

If your message template is approved, its status will be set to **Active - Quality pending** (`APPROVED` in the API) and you can begin sending it to customers. If it is rejected, you can edit it and resubmit for approval, or appeal the decision.

### Samples

If your template uses variables you must include sample variable values, such as media assets and text strings, with your submission. Providing a sample makes it easier for reviewers to visualize how your template will appear to customers.

To include a sample with your submission in the WhatsApp Manager, first create your template, adding any variables that it requires, then click the Add Sample button. The preview pane will render any sample media assets or sample text values you provide.

## Common rejection reasons

Submissions are commonly rejected for the following reasons, so make sure you avoid these mistakes.

### Parameter formatting

- Variable parameters are missing or have mismatched curly braces. The correct format is `&#123;&#123;1&#125;&#125;` for positional parameters.
- Variable parameters contain special characters such as a `#`, `$`, or `%`.
- Variable parameters are not sequential. For example, `&#123;&#123;1&#125;&#125;`, `&#123;&#123;2&#125;&#125;`, `&#123;&#123;5&#125;&#125;`, `&#123;&#123;4&#125;&#125;`.
- The template contains too many variable parameters relative to the message length. You need to decrease the number of variable parameters or increase the message length.
- The message template cannot start or end with a parameter (dangling parameters are not allowed).

### Content and policy violations

- The message template contains content that violates WhatsApp&#039;s Commerce Policy: When you offer goods or services for sale, Meta considers all messages and media related to your goods or services, including any descriptions, prices, fees, taxes, and/or any required legal disclosures, to constitute transactions. Transactions must comply with the WhatsApp Commerce Policy.
- The message template contains content that violates the WhatsApp Business Policy: Do not request sensitive identifiers from users. For example, do not ask people to share full length individual payment card numbers, financial account numbers, National Identification numbers, or other sensitive identifiers. This also includes not requesting documents from users that might contain sensitive identifiers. Requesting partial identifiers (ex: last 4 digits of their Social Security number) is OK.
- The content contains potentially abusive or threatening content, such as threatening a customer with legal action or threatening to publicly shame them.

### Character limits and text format

The body component will have different character limits depending on the format and tag of the template. The number of emojis allowed in the body component may also be limited.

### Duplication

The message template is a duplicate of an existing template. If a template is submitted with the same wording in the body and footer of an existing template, the duplicate template will be rejected.

### Rejection notifications

A rejection notification that includes the rejection reason will appear in Business Support Home. You can view rejections in the Business Support Home by navigating to **Account Overview** &gt; **View my accounts** (button) &gt; (your Meta Business Account) &gt; (your WABA) &gt; **Rejected message templates**.

Rejection info will also be sent via email.

You can refer to the Business Support Home notification to see the name and language of the existing template with the same content as the rejected duplicate template. You may also choose to edit the template and resubmit.

Note: This check does not apply to templates categorized as `AUTHENTICATION`.

## Appeals

If your submission is rejected you may file an appeal. Note that appeals must include a sample. If an approved template has become disabled, you may also edit it and resubmit it for approval.

### In the WhatsApp Manager:

1. Hover over the suitcase icon (Account tools) and click Message templates. If you have multiple WhatsApp Business Accounts, use the dropdown menu in the top-right corner to select the account whose templates you want to manage.
1. Find the message template that you would like to edit and click it.
1. Edit the template&#039;s contents.
1. Click the **Add Sample** button and add sample variable values and images.
1. Click **Submit**.

The appeal will be reviewed and a decision made within 24 hours.

## Template review webhooks

You can receive updates via the [message_template_status_update](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/message_template_status_update) webhook, which notifies whether a template is approved, pending, or rejected.

If your template is rejected and you disagree with the decision, you have the option to submit an appeal for reconsideration.
