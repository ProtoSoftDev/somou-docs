# Template migration



This document describes how to migrate templates from one WhatsApp Business account (WABA) to another. Migration doesn&#039;t move templates; it recreates them in the destination WABA.

## Limitations

* Templates can only be migrated between WABAs owned by the same Meta business.
* Only templates with a status of `APPROVED` and a `quality_score` of either `GREEN` or `UNKNOWN` are eligible for migration.

## Request syntax

Use the [Migrate Message Templates API](https://developers.facebook.com/docs/graph-api/reference/whats-app-business-account/migrate_message_templates) to migrate templates from one WABA to another.

```html
curl -X POST &quot;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;DESTINATION_WABA_ID&gt;/migrate_message_templates&quot; \
-H &quot;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&quot; \
-H &quot;Content-Type: application/json&quot; \
-d &#039;
&#123;
  &quot;source_waba_id&quot;: &quot;&lt;SOURCE_WABA_ID&gt;&quot;,
  &quot;page_number&quot;: &lt;PAGE_NUMBER&gt;,
  &quot;count&quot;: &lt;COUNT&gt;

  &lt;!-- only if migrating specific templates that failed to migrate --&gt;
  &quot;template_ids&quot;: [&lt;TEMPLATE_IDS&gt;]
&#125;&#039;
```

### Parameters

| Placeholder | Description | Example value |
| --- | --- | --- |
| `&lt;ACCESS_TOKEN&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;[System token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#system-user-access-tokens) or [business token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#business-integration-system-user-access-tokens). | `EAAA...` |
| `&lt;API_VERSION&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Graph API version. | v25.0 |
| `&lt;COUNT&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | **Optional.**&lt;br&gt;&lt;br&gt;Overrides the default batch size with a maximum count of 500.&lt;br&gt;&lt;br&gt;If the request takes longer than 30 seconds to execute and times out, reduce the count number. | `200` |
| `&lt;DESTINATION_WABA_ID&gt;`&lt;br&gt;&lt;br&gt;_WhatsApp Business account ID_ | **Required.**&lt;br&gt;&lt;br&gt;Destination WhatsApp Business account ID. | `104996122399160` |
| `&lt;PAGE_NUMBER&gt;`&lt;br&gt;&lt;br&gt;_Integer_ | **Optional.**&lt;br&gt;&lt;br&gt;Indicates the number of templates to migrate as sets of 500. Zero-indexed. For example, to migrate 1000 templates, send one request with this value set to `0` and another request with this value set to `1`, in parallel. | `0` |
| `&lt;TEMPLATE_IDS&gt;`&lt;br&gt;&lt;br&gt;_Array of strings_ | **Optional.**&lt;br&gt;&lt;br&gt;Only use to migrate specific template IDs with a max array length of 500. For example, to migrate failed template IDs, add the specific template ID to the array. | `[&quot;35002248699842&quot;,&quot;351234565148&quot;,&quot;54382248699842&quot;]` |
| `&lt;SOURCE_WABA_ID&gt;`&lt;br&gt;&lt;br&gt;_WhatsApp Business account ID_ | **Required.**&lt;br&gt;&lt;br&gt;Source WhatsApp Business account ID. | `102290129340398` |

## Response

```json
&#123;
  &quot;migrated_templates&quot;: [&lt;MIGRATED_TEMPLATES&gt;],
  &quot;failed_templates&quot;: [&lt;FAILED_TEMPLATES&gt;]
&#125;
```

### Response properties

| Placeholder | Description | Example value |
| --- | --- | --- |
| `&lt;MIGRATED_TEMPLATES&gt;`&lt;br&gt;&lt;br&gt;_List_ | List of template IDs that were successfully duplicated in the destination WhatsApp Business account. | `&quot;1473688840035974&quot;,&quot;6162904357082268&quot;,&quot;6147830171896170&quot;` |
| `&lt;FAILED_TEMPLATES&gt;`&lt;br&gt;&lt;br&gt;_Map_ | Map identifying templates that were not duplicated in the destination WhatsApp Business account.&lt;br&gt;&lt;br&gt;Keys are template IDs and values are failure reasons. | `&quot;1019496902803242&quot;:&quot;Incorrect category&quot;,`&lt;br&gt;`&quot;259672276895259&quot;:&quot;Formatting error - dangling parameter&quot;,`&lt;br&gt;`&quot;572279198452421&quot;:&quot;Incorrect category&quot;` |

## Example request

```html
curl -X POST &#039;https://graph.facebook.com/v25.0/104996122399160/migrate_message_templates?source_waba_id=102290129340398&amp;page_number=0&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039;
```

## Example response

```json
&#123;
  &quot;migrated_templates&quot;: [
    &quot;1473688840035974&quot;,
    &quot;6162904357082268&quot;,
    &quot;6147830171896170&quot;
  ],
  &quot;failed_templates&quot;: &#123;
    &quot;1019496902803242&quot;: &quot;Incorrect category&quot;,
    &quot;259672276895259&quot;: &quot;Formatting error - dangling parameter&quot;,
    &quot;572279198452421&quot;: &quot;Incorrect category&quot;
  &#125;
&#125;
```
