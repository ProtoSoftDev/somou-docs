# Enroll in the max price feature



To use the max price feature during the Limited Beta period (May 15 to October 2026), you must sign the beta agreement. Solution Partners must also allowlist the end-businesses they want to enroll.

- **Direct integrators:** Sign the beta agreement (Step 1 only).
- **Solution Partners:** Sign the beta agreement (Step 1), then allowlist end-businesses (Step 2).

Partner-enabled end-businesses do not need to sign the beta agreement.

## Before you begin

- An active Meta Business Suite with WhatsApp Business API access.
- Your app must have an access token with the `business_management` permission.
- For Solution Partners: The end-businesses you want to allowlist must own or share their WABAs with you.

## Step 1: Sign the beta agreement

Sign the max price beta agreement once per Meta Business Suite. The caller must be an admin user of the Meta Business Suite. The signer receives an email with an acceptance link.

### Request syntax

```html
POST /&lt;BUSINESS_ID&gt;/max_price_agreements
```

### Example request

```html
curl -X POST &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;BUSINESS_ID&gt;/max_price_agreements&#039; \
  -H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039; \
  -H &#039;Content-Type: application/json&#039; \
  -d &#039;&#123;
  &quot;signer_name&quot;: &quot;&lt;SIGNER_NAME&gt;&quot;,
  &quot;signer_email&quot;: &quot;&lt;SIGNER_EMAIL&gt;&quot;
&#125;&#039;
```

### Request parameters

| Placeholder | Description | Example value |
| --- | --- | --- |
| `&lt;ACCESS_TOKEN&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;[System token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#system-user-access-tokens) or [business token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#business-integration-system-user-access-tokens). | `EAAA...` |
| `&lt;API_VERSION&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Graph API version. | v25.0 |
| `&lt;BUSINESS_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;ID of your Meta Business Suite. | `529216684107530` |
| `&lt;SIGNER_NAME&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Name of the person signing the agreement. | `Jane Doe` |
| `&lt;SIGNER_EMAIL&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;Email address where Meta sends the agreement link. | `jane.doe&#064;example.com` |

### Example response

```json
&#123;
  &quot;status&quot;: &quot;pending&quot;,
  &quot;signer_name&quot;: &quot;Jane Doe&quot;,
  &quot;signer_email&quot;: &quot;jane.doe&#064;bsp.example&quot;,
  &quot;message&quot;: &quot;Agreement email sent to jane.doe&#064;bsp.example&quot;
&#125;
```

Re-posting while the agreement is pending resends the email (5-minute cooldown). You can pass an updated `signer_email` to redirect the link.

When the owner of the Meta Business Suite ID formally accepts the agreement, Meta automatically allowlists all self-owned and internally owned WABA IDs of the Meta Business Suite.

**Note:** The `signer_name` and `signer_email` you provide are collected solely to execute the Max Price Beta Agreement. `signer_name` appears on the agreement as the designated signer; `signer_email` is used only to deliver the acceptance link. Neither field is retained for marketing, associated with the signer&#039;s other Meta identities, nor shared with third parties. Once the agreement is accepted, no further use is made of this information.

## Check agreement status

### Request syntax

```html
curl -X GET &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;BUSINESS_ID&gt;/max_price_agreements&#039; \
  -H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039;
```

### Example responses

Agreement accepted:

```json
&#123;&quot;status&quot;: &quot;accepted&quot;, &quot;signer_name&quot;: &quot;Jane Doe&quot;, &quot;signer_email&quot;: &quot;jane.doe&#064;bsp.example&quot;&#125;
```

Agreement pending:

```json
&#123;&quot;status&quot;: &quot;pending&quot;, &quot;signer_name&quot;: &quot;Jane Doe&quot;, &quot;signer_email&quot;: &quot;jane.doe&#064;bsp.example&quot;&#125;
```

No agreement:

```json
&#123;&quot;status&quot;: &quot;none&quot;&#125;
```

Possible `status` values: `none`, `pending`, `accepted`.

## Step 2: Allowlist end-businesses (Solution Partner only)

Direct integrators skip this step. They are the end-business and do not need to allowlist themselves.

Once your beta agreement is accepted, allowlist the end-business IDs whose WABAs should access the max price feature. Each Solution Partner can hold up to 15 active allowlist entries during the Limited Beta period.

### Add end-businesses

#### Request syntax

```html
curl -X POST &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;SOLUTION_PARTNER_BUSINESS_ID&gt;/max_price_end_businesses&#039; \
  -H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039; \
  -H &#039;Content-Type: application/json&#039; \
  -d &#039;&#123;&quot;end_business_ids&quot;: [&lt;END_BUSINESS_IDS&gt;]&#125;&#039;
```

#### Request parameters

| Placeholder | Description | Example value |
| --- | --- | --- |
| `&lt;ACCESS_TOKEN&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;[System token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#system-user-access-tokens) or [business token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#business-integration-system-user-access-tokens). | `EAAA...` |
| `&lt;API_VERSION&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Graph API version. | v25.0 |
| `&lt;SOLUTION_PARTNER_BUSINESS_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;ID of your Solution Partner Meta Business Suite. | `529216684107530` |
| `&lt;END_BUSINESS_IDS&gt;`&lt;br&gt;&lt;br&gt;_Array_ | **Required.**&lt;br&gt;&lt;br&gt;List of end-business IDs to allowlist. Maximum 15 entries. | `[&quot;111111111&quot;, &quot;222222222&quot;]` |

#### Example response

```json
&#123;
  &quot;results&quot;: [
    &#123;&quot;end_business_id&quot;: &quot;111111111&quot;, &quot;status&quot;: &quot;enrolled&quot;&#125;,
    &#123;&quot;end_business_id&quot;: &quot;222222222&quot;, &quot;status&quot;: &quot;rejected&quot;, &quot;reason&quot;: &quot;No shared WABA&quot;&#125;
  ]
&#125;
```

Requests are additive and idempotent. Duplicate enrollments are no-ops.

### List allowlisted end-businesses

#### Request syntax

```html
curl -X GET &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;SOLUTION_PARTNER_BUSINESS_ID&gt;/max_price_end_businesses&#039; \
  -H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039;
```

#### Example response

```json
&#123;
  &quot;data&quot;: [
    &#123;&quot;end_business_id&quot;: &quot;111111111&quot;, &quot;end_business_name&quot;: &quot;Acme Foods&quot;, &quot;enrolled_time&quot;: 1714500000&#125;
  ]
&#125;
```

Returns an empty `data` array if no allowlist exists yet. `end_business_name` is the display name of the enrolled end-business, or `null` if the business can&#039;t be resolved (for example, if it was deactivated or deleted).

### Remove end-businesses

#### Request syntax

```html
curl -X DELETE &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;SOLUTION_PARTNER_BUSINESS_ID&gt;/max_price_end_businesses&#039; \
  -H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039; \
  -H &#039;Content-Type: application/json&#039; \
  -d &#039;&#123;&quot;end_business_ids&quot;: [&lt;END_BUSINESS_IDS&gt;]&#125;&#039;
```

#### Example response

```json
&#123;
  &quot;results&quot;: [
    &#123;&quot;end_business_id&quot;: &quot;111111111&quot;, &quot;status&quot;: &quot;removed&quot;&#125;
  ]
&#125;
```

Per-ID `status` is `removed` (was present, now deleted) or `not_found` (was not in the allowlist). Both are non-errors.

## Error handling

| Condition | Behavior |
| --- | --- |
| Missing or invalid parameters | HTTP 400 with parameter validation error |
| Solution Partner does not have an accepted agreement | HTTP 403 &quot;Agreement required&quot; |
| `POST` agreement called within 5 minutes of last email send | HTTP 429 with `retry_after` |
| End-business not eligible (no shared WABA, not onboarded to MM API) | HTTP 200 with per-ID `&#123;&quot;status&quot;: &quot;rejected&quot;, &quot;reason&quot;: &quot;...&quot;&#125;` |
| Allowlist already at cap of 15 | HTTP 200 with per-ID `&#123;&quot;status&quot;: &quot;rejected&quot;, &quot;reason&quot;: &quot;The allowlist has reached the maximum of 15 end-businesses.&quot;&#125;` |

