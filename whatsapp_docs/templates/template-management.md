# Template management



Learn about common endpoints used to manage templates, including getting, editing, deleting, archiving, and unarchiving templates.

## Get templates

Use the [Message Templates API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/message-template-api#get-version-waba-id-message-templates) to get a list of templates in a WhatsApp Business account.

### Get all templates

Example request to get all templates (default fields):

```shell
curl &#039;https://graph.facebook.com/v23.0/102290129340398/message_templates&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039;
```

Example response, truncated (`...`) for brevity:

```json
&#123;
  &quot;data&quot;: [
    &#123;
      &quot;name&quot;: &quot;reservation_confirmation&quot;,
      &quot;parameter_format&quot;: &quot;NAMED&quot;,
      &quot;components&quot;: [
        &#123;
          &quot;type&quot;: &quot;HEADER&quot;,
          &quot;format&quot;: &quot;IMAGE&quot;,
          &quot;example&quot;: &#123;
            &quot;header_handle&quot;: [
              &quot;https://scontent.whatsapp.net/v/t61...&quot;
            ]
          &#125;
        &#125;,
        &#123;
          &quot;type&quot;: &quot;BODY&quot;,
          &quot;text&quot;: &quot;*You&#039;re all set!*\n\nYour reservation for &#123;&#123;number_of_guests&#125;&#125; at Lucky Shrub Eatery on &#123;&#123;day&#125;&#125;, &#123;&#123;date&#125;&#125;, at &#123;&#123;time&#125;&#125;, is confirmed. See you then!&quot;,
          &quot;example&quot;: &#123;
            &quot;body_text_named_params&quot;: [
              &#123;
                &quot;param_name&quot;: &quot;number_of_guests&quot;,
                &quot;example&quot;: &quot;4&quot;
              &#125;,
              &#123;
                &quot;param_name&quot;: &quot;day&quot;,
                &quot;example&quot;: &quot;Saturday&quot;
              &#125;,
              &#123;
                &quot;param_name&quot;: &quot;date&quot;,
                &quot;example&quot;: &quot;August 30th, 2025&quot;
              &#125;,
              &#123;
                &quot;param_name&quot;: &quot;time&quot;,
                &quot;example&quot;: &quot;7:30 pm&quot;
              &#125;
            ]
          &#125;
        &#125;,
        &#123;
          &quot;type&quot;: &quot;FOOTER&quot;,
          &quot;text&quot;: &quot;Lucky Shrub Eatery: The Luckiest Eatery in Town!&quot;
        &#125;,
        &#123;
          &quot;type&quot;: &quot;BUTTONS&quot;,
          &quot;buttons&quot;: [
            &#123;
              &quot;type&quot;: &quot;URL&quot;,
              &quot;text&quot;: &quot;Change reservation&quot;,
              &quot;url&quot;: &quot;https://www.luckyshrubeater.com/reservations&quot;
            &#125;,
            &#123;
              &quot;type&quot;: &quot;PHONE_NUMBER&quot;,
              &quot;text&quot;: &quot;Call us&quot;,
              &quot;phone_number&quot;: &quot;+16467043595&quot;
            &#125;,
            &#123;
              &quot;type&quot;: &quot;QUICK_REPLY&quot;,
              &quot;text&quot;: &quot;Cancel reservation&quot;
            &#125;
          ]
        &#125;
      ],
      &quot;language&quot;: &quot;en_US&quot;,
      &quot;status&quot;: &quot;APPROVED&quot;,
      &quot;category&quot;: &quot;UTILITY&quot;,
      &quot;id&quot;: &quot;1387372356726668&quot;
    &#125;,
    &#123;
      &quot;name&quot;: &quot;coupon_expiration_reminder_number_vars&quot;,
      &quot;parameter_format&quot;: &quot;POSITIONAL&quot;,
      &quot;components&quot;: [
        &#123;
          &quot;type&quot;: &quot;HEADER&quot;,
          &quot;format&quot;: &quot;TEXT&quot;,
          &quot;text&quot;: &quot;Act fast, &#123;&#123;1&#125;&#125;!&quot;,
          &quot;example&quot;: &#123;
            &quot;header_text&quot;: [
              &quot;Pablo&quot;
            ]
          &#125;
        &#125;,
        &#123;
          &quot;type&quot;: &quot;BODY&quot;,
          &quot;text&quot;: &quot;Just a quick reminder—your exclusive coupon code, &#123;&#123;1&#125;&#125;, *expires in only &#123;&#123;2&#125;&#125; days!* Don&#039;t miss out on our special deals. Use your code at checkout before it&#039;s too late.\n\nHappy shopping! 😃&quot;,
          &quot;example&quot;: &#123;
            &quot;body_text&quot;: [
              [
                &quot;SUMMER20&quot;,
                &quot;10&quot;
              ]
            ]
          &#125;
        &#125;,
        &#123;
          &quot;type&quot;: &quot;FOOTER&quot;,
          &quot;text&quot;: &quot;Lucky Shrub Succulents&quot;
        &#125;,
        &#123;
          &quot;type&quot;: &quot;BUTTONS&quot;,
          &quot;buttons&quot;: [
            &#123;
              &quot;type&quot;: &quot;URL&quot;,
              &quot;text&quot;: &quot;See deals&quot;,
              &quot;url&quot;: &quot;https://www.luckyshrub.com/deals&quot;
            &#125;,
            &#123;
              &quot;type&quot;: &quot;QUICK_REPLY&quot;,
              &quot;text&quot;: &quot;Unsubscribe&quot;
            &#125;
          ]
        &#125;
      ],
      &quot;language&quot;: &quot;en&quot;,
      &quot;status&quot;: &quot;APPROVED&quot;,
      &quot;category&quot;: &quot;MARKETING&quot;,
      &quot;sub_category&quot;: &quot;CUSTOM&quot;,
      &quot;id&quot;: &quot;1304694804498707&quot;
    &#125;

    ...

  ],
  &quot;paging&quot;: &#123;
    &quot;cursors&quot;: &#123;
      &quot;before&quot;: &quot;QVFIU...&quot;,
      &quot;after&quot;: &quot;QVFIU...&quot;
    &#125;,
    &quot;next&quot;: &quot;https://graph.facebook.com/v23.0/10229...&quot;
  &#125;
&#125;
```

### Get all templates and specific fields

Example request to get the name, category, and status of all templates in a WhatsApp Business account, limiting the response to 5 templates per result set:

```shell
curl &#039;https://graph.facebook.com/v23.0/102290129340398/message_templates?fields=name,category,status&amp;limit=5&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039;
```

Example response:

```json
&#123;
  &quot;data&quot;: [
    &#123;
      &quot;name&quot;: &quot;reservation_confirmation&quot;,
      &quot;category&quot;: &quot;UTILITY&quot;,
      &quot;status&quot;: &quot;APPROVED&quot;,
      &quot;id&quot;: &quot;1387372356726668&quot;
    &#125;,
    &#123;
      &quot;name&quot;: &quot;coupon_expiration_reminder_number_vars&quot;,
      &quot;category&quot;: &quot;MARKETING&quot;,
      &quot;status&quot;: &quot;APPROVED&quot;,
      &quot;id&quot;: &quot;1304694804498707&quot;
    &#125;,
    &#123;
      &quot;name&quot;: &quot;coupon_expiration_reminder_named_vars&quot;,
      &quot;category&quot;: &quot;MARKETING&quot;,
      &quot;status&quot;: &quot;APPROVED&quot;,
      &quot;id&quot;: &quot;1625063511800527&quot;
    &#125;,
    &#123;
      &quot;name&quot;: &quot;address_update&quot;,
      &quot;category&quot;: &quot;UTILITY&quot;,
      &quot;status&quot;: &quot;PENDING&quot;,
      &quot;id&quot;: &quot;1137051647947973&quot;
    &#125;,
    &#123;
      &quot;name&quot;: &quot;reservation_confirmation_short_banner&quot;,
      &quot;category&quot;: &quot;UTILITY&quot;,
      &quot;status&quot;: &quot;REJECTED&quot;,
      &quot;id&quot;: &quot;1166414785519855&quot;
    &#125;
  ],
  &quot;paging&quot;: &#123;
    &quot;cursors&quot;: &#123;
      &quot;before&quot;: &quot;QVFIU...&quot;,
      &quot;after&quot;: &quot;QVFIU...&quot;
    &#125;,
    &quot;next&quot;: &quot;https://graph.facebook.com/v23.0/10229...&quot;
  &#125;
&#125;
```

### Get all approved and rejected templates

Example request to get all approved templates and their name, category, and status (swap `status=approved` with `status=rejected` to get rejected templates instead):

```shell
curl &#039;https://graph.facebook.com/v23.0/102290129340398/message_templates?fields=name,category,status&amp;status=approved&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039;
```

Example response:

```json
&#123;
  &quot;data&quot;: [
    &#123;
      &quot;name&quot;: &quot;reservation_confirmation&quot;,
      &quot;category&quot;: &quot;UTILITY&quot;,
      &quot;status&quot;: &quot;APPROVED&quot;,
      &quot;id&quot;: &quot;1387372356726668&quot;
    &#125;,
    &#123;
      &quot;name&quot;: &quot;coupon_expiration_reminder_number_vars&quot;,
      &quot;category&quot;: &quot;MARKETING&quot;,
      &quot;status&quot;: &quot;APPROVED&quot;,
      &quot;id&quot;: &quot;1304694804498707&quot;
    &#125;,
    &#123;
      &quot;name&quot;: &quot;coupon_expiration_reminder_named_vars&quot;,
      &quot;category&quot;: &quot;MARKETING&quot;,
      &quot;status&quot;: &quot;APPROVED&quot;,
      &quot;id&quot;: &quot;1625063511800527&quot;
    &#125;,
    &#123;
      &quot;name&quot;: &quot;calling_permission_request&quot;,
      &quot;category&quot;: &quot;MARKETING&quot;,
      &quot;status&quot;: &quot;APPROVED&quot;,
      &quot;id&quot;: &quot;1092999222892024&quot;
    &#125;,
    &#123;
      &quot;name&quot;: &quot;location_request_v1&quot;,
      &quot;category&quot;: &quot;MARKETING&quot;,
      &quot;status&quot;: &quot;APPROVED&quot;,
      &quot;id&quot;: &quot;3373761659571648&quot;
    &#125;,
    &#123;
      &quot;name&quot;: &quot;order_confirmation&quot;,
      &quot;category&quot;: &quot;UTILITY&quot;,
      &quot;status&quot;: &quot;APPROVED&quot;,
      &quot;id&quot;: &quot;1667696820637468&quot;
    &#125;
  ],
  &quot;paging&quot;: &#123;
    &quot;cursors&quot;: &#123;
      &quot;before&quot;: &quot;QVFIU...&quot;,
      &quot;after&quot;: &quot;QVFIU...&quot;
    &#125;,
    &quot;next&quot;: &quot;https://graph.facebook.com/v23.0/10229...&quot;
  &#125;
&#125;
```

## Create templates

Use the [Message Templates API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/message-template-api#post-version-waba-id-message-templates) to [create a template](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/overview#creation). See also [Create templates](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/overview#creation) for detailed component and parameter guidance.

### Template name validation

Template names can only contain lowercase alphanumeric characters and underscores (regex: `^[a-z0-9_]+$`). The maximum length is 512 characters. If a name contains uppercase letters, spaces, or special characters, the API returns error code `100`.

Template names must be unique within a WhatsApp Business account for each language. Creating a template with a name that already exists for the same language returns error code `100`, subcode `2388024`, with message &quot;Content in This Language Already Exists&quot;.

### Category and language validation

The API validates template parameters at creation time. Invalid categories or unsupported [language codes](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/supported-languages) return error code `100`. Starting with v23.0, template component parameter issues at send time return error code `132018`.

## Edit templates

Use the [Message Template API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/message-template-api#post-version-template-id) to edit a template. You can also use the [Message templates](https://business.facebook.com/latest/whatsapp_manager/message_templates) panel in WhatsApp Manager to edit templates.

### Edit template limitations

- Only templates with an `APPROVED`, `REJECTED`, or `PAUSED` status can be edited.
- You can only edit a template&#039;s category, components, or time-to-live.
- You cannot edit individual template components; the API replaces all components with those in the edit request payload.
- You cannot edit the category of an approved template.
- Approved templates can be edited up to 10 times in a 30-day window, or 1 time in a 24-hour window. Rejected or paused templates can be edited an unlimited number of times.
- After you edit an approved or paused template, the API automatically re-approves the template unless it fails template review.

### Edit template category

Example request:

```shell
curl &#039;https://graph.facebook.com/v23.0/1252715608684590&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039; \
-d &#039;
&#123;
  &quot;category&quot;: &quot;MARKETING&quot;
&#125;&#039;
```

Example response:

```json
&#123;
  &quot;success&quot;: true
&#125;
```

### Edit template components

Example request to overwrite a template&#039;s existing components with new components.

```shell
curl &#039;https://graph.facebook.com/v23.0/564750795574598&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039; \
-d &#039;
&#123;
  &quot;components&quot;: [
    &#123;
      &quot;type&quot;: &quot;HEADER&quot;,
      &quot;format&quot;: &quot;TEXT&quot;,
      &quot;text&quot;: &quot;Our &#123;&#123;1&#125;&#125; is on!&quot;,
      &quot;example&quot;: &#123;
        &quot;header_text&quot;: [
          &quot;Spring Sale&quot;
        ]
      &#125;
    &#125;,
    &#123;
      &quot;type&quot;: &quot;BODY&quot;,
      &quot;text&quot;: &quot;Shop now through &#123;&#123;1&#125;&#125; and use code &#123;&#123;2&#125;&#125; to get &#123;&#123;3&#125;&#125; off of all merchandise.&quot;,
      &quot;example&quot;: &#123;
        &quot;body_text&quot;: [
          [
            &quot;the end of April&quot;,
            &quot;25OFF&quot;,
            &quot;25%&quot;
          ]
        ]
      &#125;
    &#125;,
    &#123;
      &quot;type&quot;: &quot;FOOTER&quot;,
      &quot;text&quot;: &quot;Use the buttons below to manage your marketing subscriptions&quot;
    &#125;,
    &#123;
      &quot;type&quot;: &quot;BUTTONS&quot;,
      &quot;buttons&quot;: [
        &#123;
          &quot;type&quot;: &quot;QUICK_REPLY&quot;,
          &quot;text&quot;: &quot;Unsubscribe from Promos&quot;
        &#125;,
        &#123;
          &quot;type&quot;: &quot;QUICK_REPLY&quot;,
          &quot;text&quot;: &quot;Unsubscribe from All&quot;
        &#125;
      ]
    &#125;
  ]
&#125;&#039;
```

## Delete templates

Use the [Message Templates API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/message-template-api#delete-version-waba-id-message-templates) to delete a template by name or ID, or delete multiple templates by their IDs.

This endpoint requires the `whatsapp_business_management` permission. If you only have the `whatsapp_business_messaging` permission, the API returns error code `200`.

If a template with the specified name does not exist, the API returns an error.

### Delete template limitations

- If you delete a template that has been sent in a template message but has yet to be delivered (for example, because the WhatsApp user&#039;s phone is turned off), the template&#039;s status is set to `PENDING_DELETION` and WhatsApp attempts delivery for 30 days.
- If you delete an approved template, you cannot create a new template with the same name for 30 days.
- Templates that are in a disabled status cannot be deleted.

### Delete template by name

Deleting a template by name deletes all templates that match that name (meaning templates with the same name but different languages will also be deleted).

Example request:

```shell
curl -X DELETE &#039;https://graph.facebook.com/v23.0/102290129340398/message_templates?name=order_confirmation&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039;
```

Example response:

```json
&#123;
  &quot;success&quot;: true
&#125;
```

### Delete template by ID

To delete a template by ID, include the template&#039;s ID along with its name in your request; only the template with the matching template ID will be deleted.

Example request:

```shell
curl -X DELETE &#039;https://graph.facebook.com/v23.0/102290129340398/message_templates?hsm_id=1407680676729941&amp;name=order_confirmation&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039;
```

Example response:

```json
&#123;
  &quot;success&quot;: true
&#125;
```

### Delete templates by IDs

To delete multiple templates at once, include an array of template IDs in the `hsm_ids` query parameter. You can include up to 100 template IDs per request.

The `hsm_ids` parameter cannot be combined with the `name` or `hsm_id` parameters. If any of the template IDs are invalid, the entire request fails and no templates are deleted.

Example request:

```shell
curl -X DELETE &#039;https://graph.facebook.com/v23.0/102290129340398/message_templates?hsm_ids=[1387372356726668,1304694804498707]&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039;
```

Example response:

```json
&#123;
  &quot;success&quot;: true
&#125;
```

## Archive and unarchive templates

When templates have been inactive for 12 months or more, the platform automatically archives them and schedules them for deletion after 28 days. You can also manually archive or unarchive templates in bulk using the API.

See [template archival](https://developers.facebook.com/documentation/business-messaging/whatsapp/templates/template-archival) for more information about auto-archival, the archive and unarchive endpoints, and notifications.
