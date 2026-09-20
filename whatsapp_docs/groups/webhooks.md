# Webhooks for Groups API



In order to receive webhook notifications for metadata about your groups, please subscribe to the following webhook fields:

* `group_lifecycle_update`
* `group_participants_update`
* `group_settings_update`
* `group_status_update`

## `group_lifecycle_update` webhooks

A `group_lifecycle_update` webhook is triggered when a group is either created or deleted.

### Group create succeed

```curl
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;WHATSAPP_ACCOUNT_ID&quot;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;messaging_product&quot;: &quot;whatsapp&quot;,
            &quot;metadata&quot;: &#123;
              &quot;display_phone_number&quot;: &quot;DISPLAY_PHONE_NUMBER&quot;,
              &quot;phone_number_id&quot;: &quot;PHONE_NUMBER_ID&quot;
            &#125;,
            &quot;groups&quot;: [
              &#123;
                &quot;timestamp&quot;: &quot;TIMESTAMP&quot;,
                &quot;group_id&quot;: &quot;GROUP_ID&quot;,
                &quot;type&quot;: &quot;group_create&quot;,
                &quot;request_id&quot;: &quot;REQUEST_ID&quot;,
                &quot;subject&quot;: &quot;test invite link&quot;,
                &quot;invite_link&quot;: &quot;https://chat.whatsapp.com/LINK_ID&quot;,
                &quot;join_approval_mode&quot;: &quot;JOIN_APPROVAL_MODE&quot;
              &#125;
            ]
          &#125;,
          &quot;field&quot;: &quot;group_lifecycle_update&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

### Group create fail

```curl
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;WHATSAPP_ACCOUNT_ID&quot;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
              &quot;messaging_product&quot;: &quot;whatsapp&quot;,
              &quot;metadata&quot;: &#123;
                   &quot;display_phone_number&quot;: &quot;DISPLAY_PHONE_NUMBER&quot;,
                   &quot;phone_number_id&quot;: &quot;PHONE_NUMBER_ID&quot;,
              &#125;,
               &quot;groups&quot;: [
          &#123;
                    &quot;timestamp&quot;: &quot;TIMESTAMP&quot;,
                    &quot;type&quot;: &quot;group_create&quot;,
                    &quot;subject&quot;: &quot;GROUP_SUBJECT&quot;,
                    &quot;description&quot;: &quot;GROUP_DESCRIPTION&quot;,
                    &quot;request_id&quot;: &quot;REQUEST_ID&quot;,
                    &quot;group_id&quot;: &quot;GROUP_ID&quot;,
                    &quot;errors&quot;: [
                      &#123;
                        &quot;code&quot;: &quot;ERROR_CODE&quot;,
                        &quot;message&quot;: &quot;ERROR_MESSAGE&quot;,
                        &quot;title&quot;: &quot;ERROR_TITLE&quot;,
                        &quot;error_data&quot;: &#123;
                          &quot;details&quot;: &quot;ERROR_DETAILS&quot;
                        &#125;
                      &#125;
                    ]
          &#125;
               ]
            &#125;,
          &quot;field&quot;: &quot;group_lifecycle_update&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

### Delete group succeed

```curl
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;WHATSAPP_ACCOUNT_ID&quot;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
              &quot;messaging_product&quot;: &quot;whatsapp&quot;,
              &quot;metadata&quot;: &#123;
                   &quot;display_phone_number&quot;: &quot;DISPLAY_PHONE_NUMBER&quot;,
                   &quot;phone_number_id&quot;: &quot;PHONE_NUMBER_ID&quot;,
              &#125;,
               &quot;groups&quot;: [
                  &#123;
                    &quot;timestamp&quot;: &quot;TIMESTAMP&quot;,
                    &quot;group_id&quot;: &quot;GROUP_ID&quot;,
                    &quot;type&quot;: &quot;group_delete&quot;,
                    &quot;request_id&quot;: &quot;REQUEST_ID&quot;,
                 &#125;
               ]
          &#125;,
          &quot;field&quot;: &quot;group_lifecycle_update&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

### Delete group fails

```curl
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;WHATSAPP_ACCOUNT_ID&quot;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
              &quot;messaging_product&quot;: &quot;whatsapp&quot;,
              &quot;metadata&quot;: &#123;
                   &quot;display_phone_number&quot;: &quot;DISPLAY_PHONE_NUMBER&quot;,
                   &quot;phone_number_id&quot;: &quot;PHONE_NUMBER_ID&quot;,
              &#125;,
               &quot;groups&quot;: [
                  &#123;
                    &quot;timestamp&quot;: &quot;TIMESTAMP&quot;,
                    &quot;group_id&quot;: &quot;GROUP_ID&quot;,
                    &quot;type&quot;: &quot;group_delete&quot;,
                    &quot;request_id&quot;: &quot;REQUEST_ID&quot;,
                    &quot;errors&quot;: [
                      &#123;
                        &quot;code&quot;: &quot;ERROR_CODE&quot;,
                        &quot;message&quot;: &quot;ERROR_MESSAGE&quot;,
                        &quot;title&quot;: &quot;ERROR_TITLE&quot;,
                        &quot;error_data&quot;: &#123;
                          &quot;details&quot;: &quot;ERROR_DETAILS&quot;
                        &#125;
                      &#125;
                    ]
                 &#125;
               ]
          &#125;,
          &quot;field&quot;: &quot;group_lifecycle_update&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

## `group_participants_update` webhooks

A `group_participants_update` webhook is triggered when a WhatsApp user joins a group with an invite link, requests to join a group, cancels their request, or when one or more join requests are approved.

### User joined group using invite link succeed

```curl
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;WHATSAPP_ACCOUNT_ID&quot;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
              &quot;messaging_product&quot;: &quot;whatsapp&quot;,
              &quot;metadata&quot;: &#123;
                   &quot;display_phone_number&quot;: &quot;DISPLAY_PHONE_NUMBER&quot;,
                   &quot;phone_number_id&quot;: &quot;PHONE_NUMBER_ID&quot;,
              &#125;,
               &quot;groups&quot;: [
                  &#123;
                    &quot;timestamp&quot;: &quot;TIMESTAMP&quot;,
                    &quot;group_id&quot;: &quot;GROUP_ID&quot;,
                    &quot;type&quot;: &quot;group_participants_add&quot;,
                    &quot;reason&quot;: &quot;invite_link&quot;,
                    &quot;added_participants&quot;: [
                        &#123;
                          &quot;wa_id&quot;: &quot;WHATSAPP_ID&quot;,
                        &#125;,
                    ]
                 &#125;
              ]
          &#125;,
          &quot;field&quot;: &quot;group_participants_update&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

### User accepts or cancels join request

* **For join requests:** `GROUP_REQUEST_TYPE` is set to `group_join_request_created`.
* **For cancel requests:** `GROUP_REQUEST_TYPE` is set to `group_join_request_revoked`.

```curl
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;WHATSAPP_BUSINESS_ACCOUNT_ID&quot;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;messaging_product&quot;: &quot;whatsapp&quot;,
            &quot;metadata&quot;: &#123;
              &quot;display_phone_number&quot;: &quot;BUSINESS_DISPLAY_PHONE_NUMBER&quot;,
              &quot;phone_number_id&quot;: &quot;BUSINESS_PHONE_NUMBER_ID&quot;
            &#125;,
            &quot;groups&quot;: [
              &#123;
                &quot;timestamp&quot;: &quot;WEBHOOK_TRIGGER_TIMESTAMP&quot;,
                &quot;group_id&quot;: &quot;GROUP_ID&quot;,
                &quot;type&quot;: &quot;GROUP_REQUEST_TYPE&quot;,
                &quot;reason&quot;: &quot;REASON_FOR_REQUEST_OUTCOME&quot;,
                &quot;join_request_id&quot;: &quot;JOIN_REQUEST_ID&quot;,
                &quot;wa_id&quot;: &quot;WHATSAPP_USER_ID&quot;
              &#125;
            ]
          &#125;,
          &quot;field&quot;: &quot;group_participants_update&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

### Join request approved

```curl
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;WHATSAPP_BUSINESS_ACCOUNT_ID&quot;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;messaging_product&quot;: &quot;whatsapp&quot;,
            &quot;metadata&quot;: &#123;
              &quot;display_phone_number&quot;: &quot;BUSINESS_DISPLAY_PHONE_NUMBER&quot;,
              &quot;phone_number_id&quot;: &quot;BUSINESS_PHONE_NUMBER_ID&quot;
            &#125;,
            &quot;groups&quot;: [
              &#123;
                &quot;timestamp&quot;: WEBHOOK_TRIGGER_TIMESTAMP,
                &quot;group_id&quot;: &quot;GROUP_ID&quot;,
                &quot;type&quot;: &quot;group_participants_add&quot;,
                &quot;reason&quot;: &quot;invite_link&quot;,
                &quot;added_participants&quot;: [
                  &#123;
                    &quot;input&quot;: &quot;WHATSAPP_USER_PHONE_NUMBER&quot;,
                    &quot;wa_id&quot;: &quot;WHATSAPP_USER_ID&quot;
                  &#125;,
                  //Additional added participants here, if approved in bulk.
                ]
              &#125;
            ]
          &#125;,
          &quot;field&quot;: &quot;group_participants_update&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

### Group participant remove succeed

```curl
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;WHATSAPP_ACCOUNT_ID&quot;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
              &quot;messaging_product&quot;: &quot;whatsapp&quot;,
              &quot;metadata&quot;: &#123;
                   &quot;display_phone_number&quot;: &quot;DISPLAY_PHONE_NUMBER&quot;,
                   &quot;phone_number_id&quot;: &quot;PHONE_NUMBER_ID&quot;,
              &#125;,
               &quot;groups&quot;: [
                  &#123;
                    &quot;timestamp&quot;: &quot;TIMESTAMP&quot;,
                    &quot;group_id&quot;: &quot;GROUP_ID&quot;,
                    &quot;type&quot;: &quot;group_participants_remove&quot;,
                    &quot;request_id&quot;: &quot;REQUEST_ID&quot;,
                    &quot;removed_participants&quot;: [
                        // User 1 removed successfully
                        &#123;
                          &quot;input&quot;: &quot;PHONE_NUMBER or WHATSAPP_ID&quot;
                        &#125;,
                        &#123;
                          &quot;input&quot;: &quot;PHONE_NUMBER or WHATSAPP_ID&quot;
                        &#125;,
                        ...
                    ]
                 &#125;
                 &quot;initiated_by&quot;: &quot;business&quot;
              ]
          &#125;,
          &quot;field&quot;: &quot;group_participants_update&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

### Group participant remove with participants partially fails

```curl
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;WHATSAPP_ACCOUNT_ID&quot;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
              &quot;messaging_product&quot;: &quot;whatsapp&quot;,
              &quot;metadata&quot;: &#123;
                   &quot;display_phone_number&quot;: &quot;DISPLAY_PHONE_NUMBER&quot;,
                   &quot;phone_number_id&quot;: &quot;PHONE_NUMBER_ID&quot;,
              &#125;,
               &quot;groups&quot;: [
                  &#123;
                    &quot;timestamp&quot;: &quot;TIMESTAMP&quot;,
                    &quot;group_id&quot;: &quot;GROUP_ID&quot;,
                    &quot;type&quot;: &quot;group_participants_remove&quot;,
                    &quot;request_id&quot;: &quot;REQUEST_ID&quot;,
                    &quot;initiated_by&quot;: &quot;business&quot;,
                    &quot;removed_participants&quot;: [
                      // User 1 removed successfully
                      &#123;
                        &quot;input&quot;: &quot;PHONE_NUMBER or WHATSAPP_ID&quot;
                      &#125;,
                      // Additional users removed successfully
                      ...
                    ],
                    &quot;failed_participants&quot;: [
                      // User 2 not removed due to errors
                      &#123;
                        &quot;input&quot;: &quot;PHONE_NUMBER or WHATSAPP_ID&quot;,
                        &quot;errors&quot;: [
                          &#123;
                            &quot;code&quot;: &quot;ERROR_CODE&quot;,
                            &quot;message&quot;: &quot;ERROR_MESSAGE&quot;,
                            &quot;title&quot;: &quot;ERROR_TITLE&quot;,
                            &quot;error_data&quot;: &#123;
                              &quot;details&quot;: &quot;ERROR_DETAILS&quot;
                            &#125;
                          &#125;
                        ]
                      &#125;
                    ],
                    &quot;errors&quot;: [
                      &#123;
                        &quot;code&quot;: &quot;ERROR_CODE&quot;,
                        &quot;message&quot;: &quot;Failed to remove some participants from the group&quot;,
                        &quot;title&quot;: &quot;Not All Participants Remove Succeeded&quot;,
                        &quot;error_data&quot;: &#123;
                          &quot;details&quot;: &quot;ERROR_DETAILS&quot;
                        &#125;
                      &#125;
                    ]
                 &#125;
                 &quot;initiated_by&quot;: &quot;business&quot;
              ]
          &#125;,
          &quot;field&quot;: &quot;group_participants_update&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

### Group participant remove fails

```curl
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;WHATSAPP_ACCOUNT_ID&quot;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
              &quot;messaging_product&quot;: &quot;whatsapp&quot;,
              &quot;metadata&quot;: &#123;
                   &quot;display_phone_number&quot;: &quot;DISPLAY_PHONE_NUMBER&quot;,
                   &quot;phone_number_id&quot;: &quot;PHONE_NUMBER_ID&quot;,
              &#125;,
               &quot;groups&quot;: [
                  &#123;
                    &quot;timestamp&quot;: &quot;TIMESTAMP&quot;,
                    &quot;group_id&quot;: &quot;GROUP_ID&quot;,
                    &quot;type&quot;: &quot;group_participants_remove&quot;,
                    &quot;request_id&quot;: &quot;REQUEST_ID&quot;,
                    &quot;failed_participants&quot;: [
                      &#123;
                        &quot;input&quot;: &quot;PHONE_NUMBER or WHATSAPP_ID&quot;
                      &#125;,
                      &#123;
                        &quot;input&quot;: &quot;PHONE_NUMBER or WHATSAPP_ID&quot;
                      &#125;,
                      // Additional users failed to be removed
                      ...
                    ],
                    &quot;errors&quot;: [
                      &#123;
                        &quot;code&quot;: &quot;ERROR_CODE&quot;,
                        &quot;message&quot;: &quot;ERROR_MESSAGE&quot;,
                        &quot;title&quot;: &quot;ERROR_TITLE&quot;,
                        &quot;error_data&quot;: &#123;
                          &quot;details&quot;: &quot;ERROR_DETAILS&quot;
                        &#125;
                      &#125;
                    ]
                 &#125;
                 &quot;initiated_by&quot;: &quot;business&quot;
              ]
          &#125;,
          &quot;field&quot;: &quot;group_participants_update&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

### Group participant leaves webhook

This webhook is sent when a group participant leaves the group. The `initiated_by` field and only the `wa_id` in the `removed_participants` list will point to the participant who left the group.

```curl
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;WHATSAPP_ACCOUNT_ID&quot;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
              &quot;messaging_product&quot;: &quot;whatsapp&quot;,
              &quot;metadata&quot;: &#123;
                   &quot;display_phone_number&quot;: &quot;DISPLAY_PHONE_NUMBER&quot;,
                   &quot;phone_number_id&quot;: &quot;PHONE_NUMBER_ID&quot;,
              &#125;,
               &quot;groups&quot;: [
                  &#123;
                    &quot;timestamp&quot;: &quot;TIMESTAMP&quot;,
                    &quot;group_id&quot;: &quot;GROUP_ID&quot;,
                    &quot;type&quot;: &quot;group_participants_remove&quot;,
                    &quot;removed_participants&quot;: [
                      &#123;
                        &quot;wa_id&quot;: &quot;WHATSAPP_ID&quot;,
                      &#125;
                    ]
                 &#125;
                 &quot;initiated_by&quot;: &quot;participant&quot;
               ]
          &#125;,
          &quot;field&quot;: &quot;group_participants_update&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

## `group_settings_update` webhooks

### Group settings update succeed

```curl
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;&lt;ID&gt;&quot;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;messaging_product&quot;: &quot;whatsapp&quot;,
            &quot;metadata&quot;: &#123;
              &quot;display_phone_number&quot;: &quot;DISPLAY_NUMBER&quot;,
              &quot;phone_number_id&quot;: &quot;PHONE_NUMBER_ID&quot;
            &#125;,
            &quot;groups&quot;: [
              &#123;
                &quot;timestamp&quot;: &quot;TIMESTAMP&quot;,
                &quot;group_id&quot;: &quot;GROUP_ID&quot;,
                &quot;type&quot;: &quot;group_settings_update&quot;,
                &quot;request_id&quot;: &quot;REQUEST_ID&quot;,
                &quot;profile_picture&quot;: &#123;
                  &quot;mime_type&quot;: &quot;image/jpeg&quot;,
                  &quot;update_successful&quot;: true,
                  &quot;sha256&quot;: &quot;PHOTO_HASH&quot;,
                &#125;,
                &quot;group_subject&quot;: &#123;
                  &quot;text&quot;: &quot;Test Subject&quot;,
                  &quot;update_successful&quot;: true,
                &#125;,
                &quot;group_description&quot;: &#123;
                  &quot;text&quot;: &quot;Test Description&quot;,
                  &quot;update_successful&quot;: true,
                &#125;
              &#125;
            ]
          &#125;,
          &quot;field&quot;: &quot;group_settings_update&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

### Group settings update partial fail

```curl
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;WHATSAPP_BUSINESS_ACCOUNT_ID&quot;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;messaging_product&quot;: &quot;whatsapp&quot;,
            &quot;metadata&quot;: &#123;
              &quot;display_phone_number&quot;: &quot;DISPLAY_PHONE_NUMBER&quot;,
              &quot;phone_number_id&quot;: &quot;PHONE_NUMBER_ID&quot;
            &#125;,
            &quot;groups&quot;: [
              &#123;
                &quot;timestamp&quot;: &quot;TIMESTAMP&quot;,
                &quot;group_id&quot;: &quot;GROUP_ID&quot;,
                &quot;type&quot;: &quot;group_settings_update&quot;,
                &quot;request_id&quot;: &quot;REQUEST_ID&quot;,
                &quot;profile_picture&quot;: &#123;
                  &quot;mime_type&quot;: &quot;image/jpeg&quot;,
                  &quot;update_successful&quot;: true,
                  &quot;sha256&quot;: &quot;PHOTO_HASH&quot;,
                &#125;,
                &quot;group_subject&quot;: &#123;
                  &quot;text&quot;: &quot;Test Subject&quot;,
                  &quot;update_successful&quot;: false,
                  &quot;errors&quot;: [
                    &#123;
                      &quot;code&quot;: &quot;ERROR_CODE&quot;,
                      &quot;message&quot;: &quot;ERROR_MESSAGE&quot;,
                      &quot;title&quot;: &quot;ERROR_TITLE&quot;,
                      &quot;error_data&quot;: &#123;
                        &quot;details&quot;: &quot;ERROR_DETAILS&quot;
                      &#125;
                    &#125;
                  ]
                &#125;,
                &quot;group_description&quot;: &#123;
                  &quot;text&quot;: &quot;Test Description&quot;,
                  &quot;update_successful&quot;: false,
                  &quot;errors&quot;: [
                    &#123;
                      &quot;code&quot;: &quot;ERROR_CODE&quot;,
                      &quot;message&quot;: &quot;ERROR_MESSAGE&quot;,
                      &quot;title&quot;: &quot;ERROR_TITLE&quot;,
                      &quot;error_data&quot;: &#123;
                        &quot;details&quot;: &quot;ERROR_DETAILS&quot;
                      &#125;
                    &#125;
                  ]
                &#125;,
                &quot;errors&quot;: [
                  &#123;
                    &quot;code&quot;: &quot;ERROR_CODE&quot;,
                    &quot;message&quot;: &quot;ERROR_MESSAGE&quot;,
                    &quot;title&quot;: &quot;ERROR_TITLE&quot;,
                    &quot;error_data&quot;: &#123;
                      &quot;details&quot;: &quot;ERROR_DETAILS&quot;
                    &#125;
                  &#125;
                ]
              &#125;
            ]
          &#125;,
          &quot;field&quot;: &quot;group_settings_update&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

### Group settings update total fail

```curl
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;&lt;ID&gt;&quot;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;messaging_product&quot;: &quot;whatsapp&quot;,
            &quot;metadata&quot;: &#123;
              &quot;display_phone_number&quot;: &quot;DISPLAY_PHONE_NUMBER&quot;,
              &quot;phone_number_id&quot;: &quot;PHONE_NUMBER&quot;
            &#125;,
            &quot;groups&quot;: [
              &#123;
                &quot;timestamp&quot;: &quot;TIMESTAMP&quot;,
                &quot;group_id&quot;: &quot;GROUP_ID&quot;,
                &quot;request_id&quot;: &quot;REQUEST_ID&quot;,
                &quot;type&quot;: &quot;group_settings_update&quot;,
                &quot;profile_picture&quot;: &#123;
                  &quot;mime_type&quot;: &quot;image/jpeg&quot;,
                    &quot;sha256&quot;: &quot;PHOTO_HASH&quot;,
                  &quot;update_successful&quot;: false,
                  &quot;errors&quot;: [
                    &#123;
                      &quot;code&quot;: &quot;ERROR_CODE&quot;,
                      &quot;message&quot;: &quot;ERROR_MESSAGE&quot;,
                      &quot;title&quot;: &quot;ERROR_TITLE&quot;,
                      &quot;error_data&quot;: &#123;
                        &quot;details&quot;: &quot;ERROR_DETAILS&quot;
                      &#125;
                    &#125;
                  ]
                &#125;,
                &quot;group_subject&quot;: &#123;
                  &quot;text&quot;: &quot;Test Subject&quot;,
                  &quot;update_successful&quot;: false,
                  &quot;errors&quot;: [
                    &#123;
                      &quot;code&quot;: &quot;ERROR_CODE&quot;,
                      &quot;message&quot;: &quot;ERROR_MESSAGE&quot;,
                      &quot;title&quot;: &quot;ERROR_TITLE&quot;,
                      &quot;error_data&quot;: &#123;
                        &quot;details&quot;: &quot;ERROR_DETAILS&quot;
                      &#125;
                    &#125;
                  ]
                &#125;,
                &quot;group_description&quot;: &#123;
                  &quot;text&quot;: &quot;Test Description&quot;,
                  &quot;update_successful&quot;: false,
                  &quot;errors&quot;: [
                    &#123;
                      &quot;code&quot;: &quot;ERROR_CODE&quot;,
                      &quot;message&quot;: &quot;ERROR_MESSAGE&quot;,
                      &quot;title&quot;: &quot;ERROR_TITLE&quot;,
                      &quot;error_data&quot;: &#123;
                        &quot;details&quot;: &quot;ERROR_DETAILS&quot;
                      &#125;
                    &#125;
                  ]
                &#125;,
                &quot;errors&quot;: [
                  &#123;
                    &quot;code&quot;: &quot;ERROR_CODE&quot;,
                    &quot;message&quot;: &quot;ERROR_MESSAGE&quot;,
                    &quot;title&quot;: &quot;ERROR_TITLE&quot;,
                    &quot;error_data&quot;: &#123;
                      &quot;details&quot;: &quot;ERROR_DETAILS&quot;
                    &#125;
                  &#125;
                ]
              &#125;
            ]
          &#125;,
          &quot;field&quot;: &quot;group_settings_update&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

## `group_status_update` webhooks

WhatsApp uses advanced machine learning technology to evaluate group information including group subjects, profile photos, and group descriptions. We also provide simple options for users to make reports to us from any chat.

We may prevent further activity in chat groups to comply with our legal obligations. We may also prevent further chat activity when a group admin is in violation of our [Terms of Service](https://www.whatsapp.com/legal/terms-of-service).

You may receive a webhook if a group you manage is suspended. You may also receive a webhook if a suspended group you manage becomes clear of suspensions.

### Group suspended

```curl
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;WHATSAPP_BUSINESS_ACCOUNT_ID&quot;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;messaging_product&quot;: &quot;whatsapp&quot;,
            &quot;metadata&quot;: &#123;
              &quot;display_phone_number&quot;: &quot;DISPLAY_PHONE_NUMBER&quot;,
              &quot;phone_number_id&quot;: &quot;PHONE_NUMBER_ID&quot;
            &#125;,
            &quot;groups&quot;: [
              &#123;
                &quot;timestamp&quot;: &quot;TIMESTAMP&quot;,
                &quot;type&quot;: &quot;group_suspend&quot;,
                &quot;group_id&quot;: &quot;GROUP_ID&quot;
              &#125;
            ]
          &#125;,
          &quot;field&quot;: &quot;group_status_update&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

### Group suspension cleared

```curl
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;WHATSAPP_BUSINESS_ACCOUNT_ID&quot;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;messaging_product&quot;: &quot;whatsapp&quot;,
            &quot;metadata&quot;: &#123;
              &quot;display_phone_number&quot;: &quot;DISPLAY_PHONE_NUMBER&quot;,
              &quot;phone_number_id&quot;: &quot;PHONE_NUMBER_ID&quot;
            &#125;,
            &quot;groups&quot;: [
              &#123;
                &quot;timestamp&quot;: &quot;TIMESTAMP&quot;,
                &quot;type&quot;: &quot;group_suspend_cleared&quot;,
                &quot;group_id&quot;: &quot;GROUP_ID&quot;
              &#125;
            ]
          &#125;,
          &quot;field&quot;: &quot;group_status_update&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

## Group message status webhooks

When you send messages to a group, you will receive a status webhook when the message is sent, delivered, and read. Instead of sending multiple webhooks for each status update, WhatsApp may send an aggregated webhook.

There are two types of aggregated message status webhooks you can receive.

### Multiple participants, single message

If you send a message and are set to receive several `read` or `delivered` statuses from participants, you receive a single, aggregated webhook that contains multiple `status` objects.

Each webhook you receive will be in reference to a single message sent to a single group and a single status type, that is, single group, single status by multiple participants for a single message.

**Aggregated group message status**

```json
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;WHATSAPP_BUSINESS_ACCOUNT_ID&quot;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;messaging_product&quot;: &quot;whatsapp&quot;,
            &quot;metadata&quot;: &#123;
              &quot;display_phone_number&quot;: &quot;BUSINESS_DISPLAY_PHONE_NUMBER&quot;,
              &quot;phone_number_id&quot;: &quot;BUSINESS_PHONE_NUMBER_ID&quot;
            &#125;,
            &quot;statuses&quot;: [
              &#123;
                &quot;id&quot;: &quot;WHATSAPP_MESSAGE_ID&quot;,
                &quot;status&quot;: &quot;read&quot;,
                &quot;timestamp&quot;: &quot;WEBHOOK_TRIGGER_TIMESTAMP&quot;,
                &quot;recipient_id&quot;: &quot;GROUP_ID&quot;,
                &quot;recipient_type&quot;: &quot;group&quot;,
                &quot;recipient_participant_id&quot;: &quot;GROUP_PARTICIPANT_PHONE_NUMBER_1&quot;,
                &quot;conversation&quot;: &#123;
                  &quot;id&quot;: &quot;CONVERSATION_ID&quot;,
                  &quot;origin&quot;: &#123;
                    &quot;type&quot;: &quot;CONVERSATION_CATEGORY&quot;
                  &#125;,
                  &quot;pricing&quot;: &#123;
                    &quot;billable&quot;: IS_BILLABLE,
                    &quot;pricing_model&quot;: &quot;PRICING_MODEL&quot;,
                    &quot;category&quot;: &quot;CONVERSATION_CATEGORY&quot;
                  &#125;
                &#125;
              &#125;,
              &#123;
                &quot;id&quot;: &quot;WHATSAPP_MESSAGE_ID&quot;,
                &quot;status&quot;: &quot;read&quot;,
                &quot;timestamp&quot;: &quot;WEBHOOK_TRIGGER_TIMESTAMP&quot;,
                &quot;recipient_id&quot;: &quot;GROUP_ID&quot;,
                &quot;recipient_type&quot;: &quot;group&quot;,
                &quot;recipient_participant_id&quot;: &quot;GROUP_PARTICIPANT_PHONE_NUMBER_2&quot;,
                &quot;conversation&quot;: &#123;
                  &quot;id&quot;: &quot;CONVERSATION_ID&quot;,
                  &quot;origin&quot;: &#123;
                    &quot;type&quot;: &quot;CONVERSATION_CATEGORY&quot;
                  &#125;,
                  &quot;pricing&quot;: &#123;
                    &quot;billable&quot;: IS_BILLABLE,
                    &quot;pricing_model&quot;: &quot;PRICING_MODEL&quot;,
                    &quot;category&quot;: &quot;CONVERSATION_CATEGORY&quot;
                  &#125;
                &#125;
              &#125;
            ]
          &#125;,
          &quot;field&quot;: &quot;messages&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

### Multiple messages, single participant

If you send multiple messages to a group and are set to receive several `read` or `delivered` statuses from a single
participant, WhatsApp may send you a single, aggregated webhook that contains multiple `status` objects.

Each webhook you receive will be in reference to multiple messages sent to a single group and a single status type, that is, single group, single status by single participant for multiple messages.

**Aggregated group message status**

```json
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;WHATSAPP_BUSINESS_ACCOUNT_ID&quot;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;messaging_product&quot;: &quot;whatsapp&quot;,
            &quot;metadata&quot;: &#123;
              &quot;display_phone_number&quot;: &quot;BUSINESS_DISPLAY_PHONE_NUMBER&quot;,
              &quot;phone_number_id&quot;: &quot;BUSINESS_PHONE_NUMBER_ID&quot;
            &#125;,
            &quot;statuses&quot;: [
              &#123;
                &quot;id&quot;: &quot;WHATSAPP_MESSAGE_ID_1&quot;,
                &quot;status&quot;: &quot;delivered&quot;,
                &quot;timestamp&quot;: &quot;WEBHOOK_TRIGGER_TIMESTAMP&quot;,
                &quot;recipient_id&quot;: &quot;GROUP_ID&quot;,
                &quot;recipient_type&quot;: &quot;group&quot;,
                &quot;recipient_participant_id&quot;: &quot;GROUP_PARTICIPANT_PHONE_NUMBER&quot;,
                &quot;conversation&quot;: &#123;
                  &quot;id&quot;: &quot;CONVERSATION_ID&quot;,
                  &quot;origin&quot;: &#123;
                    &quot;type&quot;: &quot;CONVERSATION_CATEGORY&quot;
                  &#125;,
                  &quot;pricing&quot;: &#123;
                    &quot;billable&quot;: IS_BILLABLE,
                    &quot;pricing_model&quot;: &quot;PRICING_MODEL&quot;,
                    &quot;category&quot;: &quot;CONVERSATION_CATEGORY&quot;
                  &#125;
                &#125;
              &#125;,
              &#123;
                &quot;id&quot;: &quot;WHATSAPP_MESSAGE_ID_2&quot;,
                &quot;status&quot;: &quot;delivered&quot;,
                &quot;timestamp&quot;: &quot;WEBHOOK_TRIGGER_TIMESTAMP&quot;,
                &quot;recipient_id&quot;: &quot;GROUP_ID&quot;,
                &quot;recipient_type&quot;: &quot;group&quot;,
                &quot;recipient_participant_id&quot;: &quot;GROUP_PARTICIPANT_PHONE_NUMBER&quot;,
                &quot;conversation&quot;: &#123;
                  &quot;id&quot;: &quot;CONVERSATION_ID&quot;,
                  &quot;origin&quot;: &#123;
                    &quot;type&quot;: &quot;CONVERSATION_CATEGORY&quot;
                  &#125;,
                  &quot;pricing&quot;: &#123;
                    &quot;billable&quot;: IS_BILLABLE,
                    &quot;pricing_model&quot;: &quot;PRICING_MODEL&quot;,
                    &quot;category&quot;: &quot;CONVERSATION_CATEGORY&quot;
                  &#125;
                &#125;
              &#125;
            ]
          &#125;,
          &quot;field&quot;: &quot;messages&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

### Group message delivered

```curl
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;WHATSAPP_BUSINESS_ACCOUNT_ID&quot;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;messaging_product&quot;: &quot;whatsapp&quot;,
            &quot;metadata&quot;: &#123;
              &quot;display_phone_number&quot;: &quot;BUSINESS_DISPLAY_PHONE_NUMBER&quot;,
              &quot;phone_number_id&quot;: &quot;BUSINESS_PHONE_NUMBER_ID&quot;
            &#125;,
            &quot;statuses&quot;: [
              &#123;
                &quot;id&quot;: &quot;WHATSAPP_MESSAGE_ID&quot;,
                &quot;status&quot;: &quot;delivered&quot;,
                &quot;timestamp&quot;: &quot;WEBHOOK_TRIGGER_TIMESTAMP&quot;,
                &quot;recipient_id&quot;: &quot;GROUP_ID&quot;,
                &quot;recipient_type&quot;: &quot;group&quot;,
                &quot;participant_recipient_id&quot;: &quot;GROUP_PARTICIPANT_PHONE_NUMBER&quot;,
                &quot;conversation&quot;: &#123;
                &quot;id&quot;: &quot;CONVERSATION_ID&quot;,
                &quot;origin&quot;: &#123;
                  &quot;type&quot;: &quot;CONVERSATION_CATEGORY&quot;
                &#125;
              &#125;,
                &quot;pricing&quot;: &#123;
                  &quot;billable&quot;: IS_BILLABLE,
                  &quot;pricing_model&quot;: &quot;PRICING_MODEL&quot;,
                  &quot;category&quot;: &quot;CONVERSATION_CATEGORY&quot;
                &#125;
              &#125;
            ]
          &#125;,
          &quot;field&quot;: &quot;messages&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

### Pricing information

Status messages webhooks that contain pricing information will have:

- `CONVERSATION_CATEGORY` set to one of:
  - `group_marketing` — Indicates a marketing conversation.
  - `group_utility` — Indicates a utility conversation.
  - `group_service` — Indicates a service conversation.
- `IS_BILLABLE` set to one of:
  - `true` — Indicates a billable conversation.
  - `false` — Indicates a non-billable conversation.
- `PRICING_MODEL` set to `PMP`.

[Learn more about Groups API pricing](https://developers.facebook.com/documentation/business-messaging/whatsapp/groups/pricing)

####  Group message read (_With pricing_)

```curl
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;WHATSAPP_BUSINESS_ACCOUNT_ID&quot;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;messaging_product&quot;: &quot;whatsapp&quot;,
            &quot;metadata&quot;: &#123;
              &quot;display_phone_number&quot;: &quot;BUSINESS_DISPLAY_PHONE_NUMBER&quot;,
              &quot;phone_number_id&quot;: &quot;BUSINESS_PHONE_NUMBER_ID&quot;
            &#125;,
            &quot;statuses&quot;: [
              &#123;
                &quot;id&quot;: &quot;WHATSAPP_MESSAGE_ID&quot;,
                &quot;status&quot;: &quot;read&quot;,
                &quot;timestamp&quot;: &quot;WEBHOOK_TRIGGER_TIMESTAMP&quot;,
                &quot;recipient_id&quot;: &quot;GROUP_ID&quot;,
                &quot;recipient_type&quot;: &quot;group&quot;,
                &quot;participant_recipient_id&quot;: &quot;GROUP_PARTICIPANT_PHONE_NUMBER&quot;,
                &quot;conversation&quot;: &#123;
                &quot;id&quot;: &quot;CONVERSATION_ID&quot;,
                &quot;origin&quot;: &#123;
                  &quot;type&quot;: &quot;CONVERSATION_CATEGORY&quot;
                &#125;
              &#125;,
                &quot;pricing&quot;: &#123;
                  &quot;billable&quot;: IS_BILLABLE,
                  &quot;pricing_model&quot;: &quot;PRICING_MODEL&quot;,
                  &quot;category&quot;: &quot;CONVERSATION_CATEGORY&quot;
                &#125;
              &#125;
            ]
          &#125;,
          &quot;field&quot;: &quot;messages&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```

#### Group message read (_Without pricing_)

```curl
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;WHATSAPP_BUSINESS_ACCOUNT_ID&quot;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;messaging_product&quot;: &quot;whatsapp&quot;,
            &quot;metadata&quot;: &#123;
              &quot;display_phone_number&quot;: &quot;BUSINESS_DISPLAY_PHONE_NUMBER&quot;,
              &quot;phone_number_id&quot;: &quot;BUSINESS_PHONE_NUMBER_ID&quot;
            &#125;,
            &quot;statuses&quot;: [
              &#123;
                &quot;id&quot;: &quot;WHATSAPP_MESSAGE_ID&quot;,
                &quot;status&quot;: &quot;read&quot;,
                &quot;timestamp&quot;: &quot;WEBHOOK_TRIGGER_TIMESTAMP&quot;,
                &quot;recipient_id&quot;: &quot;GROUP_ID&quot;,
                &quot;recipient_type&quot;: &quot;group&quot;,
                &quot;participant_recipient_id&quot;: &quot;GROUP_PARTICIPANT_PHONE_NUMBER&quot;
              &#125;
            ]
          &#125;,
          &quot;field&quot;: &quot;messages&quot;
        &#125;
      ]
    &#125;
  ]
&#125;
```
