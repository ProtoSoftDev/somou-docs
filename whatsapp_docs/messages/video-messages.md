# Video Messages



Video messages display a thumbnail preview of a video image with an optional caption. When the WhatsApp user taps the preview, it loads the video and displays it to the user.

## Sending video messages

Use the [Messages API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/message-api#post-version-phone-number-id-messages) to send a video message to a WhatsApp user.

### Request syntax

```https
POST /&lt;WHATSAPP_BUSINESS_PHONE_NUMBER_ID&gt;/messages
```


### Post body

```json
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;recipient_type&quot;: &quot;individual&quot;,
  &quot;to&quot;: &quot;&#123;&#123;wa-user-phone-number&#125;&#125;&quot;,
  &quot;type&quot;: &quot;video&quot;,
  &quot;video&quot;: &#123;
    &quot;id&quot; : &quot;&lt;MEDIA_ID&gt;&quot;, /* Only if using uploaded media */
    &quot;link&quot;: &quot;&lt;MEDIA_URL&gt;&quot;, /* Only if linking to your media */
    &quot;caption&quot;: &quot;&lt;VIDEO_CAPTION_TEXT&gt;&quot;
  &#125;
&#125;
```

### Post body parameters

| Placeholder | Description | Example Value |
| --- | --- | --- |
| `&lt;VIDEO_CAPTION_TEXT&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Video caption text.&lt;br&gt;&lt;br&gt;Maximum 1024 characters. | `A succulent eclipse!` |
| `&lt;MEDIA_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required if using an uploaded media asset (recommended)**.&lt;br&gt;&lt;br&gt;[Uploaded media](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-phone-numbers/media#upload-media) asset ID. | `1166846181421424` |
| `&lt;MEDIA_URL&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required if linking to your media asset (not recommended)**&lt;br&gt;&lt;br&gt;URL of video asset on your public server. For better performance, [upload your media asset](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-phone-numbers/media#upload-media) instead. | `https://www.luckyshrub.com/assets/lucky-shrub-eclipse-viewing.mp4` |
| `&lt;WHATSAPP_USER_PHONE_NUMBER&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;WhatsApp user phone number. | `+16505551234` |

## Supported video formats

Only H.264 video codec and AAC audio codec supported. Single audio stream or no audio stream only.

Note that videos encoded with the H.264 &quot;High&quot; profile and B-frames are not supported by Android WhatsApp clients. We recommend that you use H.264 &quot;Main&quot; profile without B-frames, or the H.264 &quot;Baseline&quot; profile when encoding (or re-encoding with a tool like ffmpeg), and place moov boxes before mdat boxes, for broader compatibility. If you are using ffmpeg, you can use the -movflags faststart flag to place moov boxes before mdata boxes.

| Video Type | Extension | MIME Type | Max Size |
| --- | --- | --- | --- |
| 3GPP | .3gp | video/3gpp | 16 MB |
| MP4 Video | .mp4 | video/mp4 | 16 MB |

## Example request

Example request to send a video message with a caption to a WhatsApp user.

```curl
curl &#039;https://graph.facebook.com/v25.0/106540352242922/messages&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039; \
-d &#039;&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;recipient_type&quot;: &quot;individual&quot;,
  &quot;to&quot;: &quot;+16505551234&quot;,
  &quot;type&quot;: &quot;video&quot;,
  &quot;video&quot;: &#123;
    &quot;id&quot; : &quot;1166846181421424&quot;,
    &quot;caption&quot;: &quot;A succulent eclipse!&quot;
  &#125;
&#125;&#039;
```

## Example response

```json
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;contacts&quot;: [
    &#123;
      &quot;input&quot;: &quot;+16505551234&quot;,
      &quot;wa_id&quot;: &quot;16505551234&quot;
    &#125;
  ],
  &quot;messages&quot;: [
    &#123;
      &quot;id&quot;: &quot;wamid.HBgLMTY0NjcwNDM1OTUVAgARGBI1RjQyNUE3NEYxMzAzMzQ5MkEA&quot;
    &#125;
  ]
&#125;
```

