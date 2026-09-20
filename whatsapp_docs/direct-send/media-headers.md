# Media message headers


**Note:** The Direct Send API is in beta. Features and behavior described here are subject to change and may be released incrementally. Participation is subject to acceptance of the beta terms.

Interactive Direct Send messages can include a media header in addition to a text header. Supported header types are `text`, `image`, `video`, and `document`.

&gt; **Access restricted.** Image, video, and document headers are access-restricted during beta. To request access, reach out to your partner manager.

The header `type` field determines which media object is required:

| Header type | Required object | Object fields |
|-------------|-----------------|---------------|
| `text` | `text` | `text` (header string) |
| `image` | `image` | At least one of `link` or `id` |
| `video` | `video` | At least one of `link` or `id` |
| `document` | `document` | At least one of `link` or `id`; optional `filename` |

For each media header, you must provide at least one of:

- `link` — a public URL to the media file.
- `id` — a media handle (`&lt;MEDIA_ID&gt;`) for media already uploaded to WhatsApp.

## Image header

```json
&quot;header&quot;: &#123;
  &quot;type&quot;: &quot;image&quot;,
  &quot;image&quot;: &#123;
    &quot;link&quot;: &quot;&lt;IMAGE_LINK&gt;&quot;,
    &quot;id&quot;: &quot;&lt;MEDIA_ID&gt;&quot;
  &#125;
&#125;
```

## Video header

```json
&quot;header&quot;: &#123;
  &quot;type&quot;: &quot;video&quot;,
  &quot;video&quot;: &#123;
    &quot;link&quot;: &quot;&lt;VIDEO_LINK&gt;&quot;,
    &quot;id&quot;: &quot;&lt;MEDIA_ID&gt;&quot;
  &#125;
&#125;
```

## Document header

Document headers additionally accept a `filename`:

```json
&quot;header&quot;: &#123;
  &quot;type&quot;: &quot;document&quot;,
  &quot;document&quot;: &#123;
    &quot;link&quot;: &quot;&lt;DOCUMENT_LINK&gt;&quot;,
    &quot;id&quot;: &quot;&lt;MEDIA_ID&gt;&quot;,
    &quot;filename&quot;: &quot;&lt;FILENAME&gt;&quot;
  &#125;
&#125;
```

## Related

- [Supported message types](https://developers.facebook.com/documentation/business-messaging/whatsapp/direct-send/supported-message-types)
- [Send sample message payloads](https://developers.facebook.com/documentation/business-messaging/whatsapp/direct-send/send-sample-payloads) — complete interactive samples with media headers
