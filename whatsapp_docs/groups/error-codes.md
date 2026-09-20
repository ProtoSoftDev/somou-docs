# Groups API Error Codes and Troubleshooting



## Error codes

| Code | Description | HTTP Status Code |
| --- | --- | --- |
| `131020`&lt;br&gt;&lt;br&gt;Bad Group | Cannot send messages to single member groups. | `400`&lt;br&gt;&lt;br&gt;Bad Request |
| `131041`&lt;br&gt;&lt;br&gt;Group unknown | The group was not found, either because it doesn&#039;t exist or you are not a member. | `400`&lt;br&gt;&lt;br&gt;Bad Request |
| `131059`&lt;br&gt;&lt;br&gt;Invalid cursor | The cursor has either expired or become corrupted. Start pagination from the beginning again. | `400`&lt;br&gt;&lt;br&gt;Bad Request |
| `131201`&lt;br&gt;&lt;br&gt;Request partially succeeded | Not all participant-level operations in the request succeeded. | `206`&lt;br&gt;&lt;br&gt;Partial Content Success |
| `131202`&lt;br&gt;&lt;br&gt;Duplicate participant | Duplicate participants in the participant array input. | `400`&lt;br&gt;&lt;br&gt;Bad Request |
| `131204`&lt;br&gt;&lt;br&gt;Participant overlimit | Group participant size exceeds limit. | `400`&lt;br&gt;&lt;br&gt;Bad Request |
| `131207`&lt;br&gt;&lt;br&gt;Group suspended | The group violates platform policies. | `403`&lt;br&gt;&lt;br&gt;Forbidden |
| `131208`&lt;br&gt;&lt;br&gt;Group Rate Limit Hit | Group operation failed because there were too many group operations from this phone number in a short period. | `429`&lt;br&gt;&lt;br&gt;Too Many Requests |
| `131209`&lt;br&gt;&lt;br&gt;Invalid Group Profile Picture Aspect Ratio | Width and height of the image must be equal. | `400`&lt;br&gt;&lt;br&gt;Bad Request |
| `131210`&lt;br&gt;&lt;br&gt;Image is Too Small to Process | Image width and height must be greater than 192px. | `400`&lt;br&gt;&lt;br&gt;Bad Request |
| `131211`&lt;br&gt;&lt;br&gt;Group create limit reached | Reached the limit for the maximum number of groups that can be created for this number. | `400`&lt;br&gt;&lt;br&gt;Bad Request |
| `131212`&lt;br&gt;&lt;br&gt;Participant is not a part of the group. | Participant is not a part of the group. | `400`&lt;br&gt;&lt;br&gt;Bad Request |
| `131213`&lt;br&gt;&lt;br&gt;Group join request does not exist. | Group join request does not exist. | `400`&lt;br&gt;&lt;br&gt;Bad Request |
| `131214`&lt;br&gt;&lt;br&gt;Group creation is temporarily disabled | Group creation is temporarily disabled due to excessive marketing messages sent by the WABA in customer service window over the past 7 days. | `400`&lt;br&gt;&lt;br&gt;Bad Request |
| `131215`&lt;br&gt;&lt;br&gt;This phone number is not eligible to access Groups APIs | Groups APIs are only available for eligible phone numbers. Check eligibility for Groups APIs in our documentation - /documentation/business-messaging/whatsapp/groups/get-started | `400`&lt;br&gt;&lt;br&gt;Bad Request |

