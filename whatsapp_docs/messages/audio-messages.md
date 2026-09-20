# Audio messages


**Warning:** On March 17th, 2026, voice messages will start receiving a [&quot;played&quot; status webhook](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/reference/messages/status) the first time a WhatsApp user plays a voice message shared by the business.

You can use Cloud API to send voice messages and basic audio messages.

## Voice messages

A voice message (sometimes referred to as a voice note, voice memo, or audio) is a recording of one or more persons speaking, and can include background sounds like music. Voice messages include features like automatic download, profile picture, and voice icon. These features are not available with basic audio messages. If the user sets voice message transcripts to **Automatic**, the message includes a text transcription.

- Voice messages require .ogg files encoded with the **OPUS** codec. If you send a different file type or a file encoded with a different codec, voice message transcription will fail.
- The play icon will only appear if the file is 512KB or smaller, otherwise it will be replaced with a download icon (a downward facing arrow).
- The message displays your business&#039;s profile image with a microphone icon.
- The text transcription appears if the user has enabled **Automatic** [voice message transcripts](https://faq.whatsapp.com/241617298315321/). If the user has set this to **Manual**, the text &quot;Transcribe&quot; will appear instead, which will display the transcribed text once tapped. If the user has set voice message transcripts to **Never**, no text will appear.

## Basic audio messages

Basic audio messages display a download icon and a music icon. When the WhatsApp user taps the play icon, the user manually downloads the audio message for the WhatsApp client to load and then play the audio file.

- The download icon will be replaced with a play icon if the WhatsApp user has enabled [auto-download](https://faq.whatsapp.com/366146522333492/) for audio media and conditions for auto-download are met (for example, connected to wi-fi).
- If you send a .ogg file encoded with the OPUS codec as a basic audio message, the music icon will be replaced with a microphone icon. In addition, if the user has enabled **Automatic** or **Manual** [voice message transcripts](https://faq.whatsapp.com/241617298315321/), a text transcription or the text &quot;Transcribe&quot; will accompany the message.

## Request syntax

Use the [Messages API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-phone-number/message-api#post-version-phone-number-id-messages) to send an audio message to a WhatsApp user.

```html
curl &#039;https://graph.facebook.com/&lt;API_VERSION&gt;/&lt;WHATSAPP_BUSINESS_PHONE_NUMBER_ID&gt;/messages&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039; \
-d &#039;
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;recipient_type&quot;: &quot;individual&quot;,
  &quot;to&quot;: &quot;&lt;WHATSAPP_USER_PHONE_NUMBER&gt;&quot;,
  &quot;type&quot;: &quot;audio&quot;,
  &quot;audio&quot;: &#123;
    &quot;id&quot;: &quot;&lt;MEDIA_ID&gt;&quot;, &lt;!-- Only if using uploaded media --&gt;
    &quot;link&quot;: &quot;&lt;MEDIA_URL&gt;&quot;, &lt;!-- Only if using hosted media (not recommended) --&gt;
    &quot;voice&quot;: &lt;IS_VOICE?&gt; &lt;!-- Only include if sending voice message --&gt;
  &#125;
&#125;&#039;
```

## Request parameters

| Placeholder | Description | Example Value |
| --- | --- | --- |
| `&lt;ACCESS_TOKEN&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;[System token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#system-user-access-tokens) or [business token](https://developers.facebook.com/documentation/business-messaging/whatsapp/access-tokens#business-integration-system-user-access-tokens). | `EAAA...` |
| `&lt;API_VERSION&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Optional.**&lt;br&gt;&lt;br&gt;Graph API version. | v25.0 |
| `&lt;IS_VOICE?&gt;`&lt;br&gt;&lt;br&gt;_Boolean_ | **Optional.**&lt;br&gt;&lt;br&gt;Set to `true` if sending a [voice message](#voice-messages). Voice messages must be Ogg files encoded with the **OPUS** codec.&lt;br&gt;&lt;br&gt;To send a [basic audio message](#basic-audio-messages), set to `false` or omit entirely. | `true` |
| `&lt;MEDIA_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required if using uploaded media, otherwise omit.**&lt;br&gt;&lt;br&gt;ID of the [uploaded media asset](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-phone-numbers/media#upload-media). | `1013859600285441` |
| `&lt;MEDIA_URL&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required if using hosted media, otherwise omit.**&lt;br&gt;&lt;br&gt;URL of the media asset hosted on your public server. For better performance, we recommend using `id` and an [uploaded media asset ID](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-phone-numbers/media#upload-media) instead. | `https://www.luckyshrub.com/media/ringtones/wind-chime.mp3` |
| `&lt;WHATSAPP_BUSINESS_PHONE_NUMBER_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;WhatsApp business phone number ID. | `106540352242922` |
| `&lt;WHATSAPP_USER_PHONE_NUMBER&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;WhatsApp user phone number. | `+16505551234` |

## Supported audio formats

| Audio Type | Extension | MIME Type | Max Size |
| --- | --- | --- | --- |
| AAC | .aac | audio/aac | 16 MB |
| AMR | .amr | audio/amr | 16 MB |
| MP3 | .mp3 | audio/mpeg | 16 MB |
| MP4 Audio | .m4a | audio/mp4 | 16 MB |
| OGG Audio | .ogg | audio/ogg (OPUS codecs only; base audio/ogg not supported; mono input only) | 16 MB |

The most common errors associated with audio files are mismatched MIME types (MIME type doesn&#039;t match the file type indicated by the file name) and invalid encoding for Ogg files (OPUS codec only). If you encounter an error when sending a media file, verify that your audio file&#039;s MIME type matches its extension and is a supported type. For Ogg files, use the OPUS codec for encoding.

## Example request

Example request to send an image message using an uploaded media ID and a caption.

```curl
curl &#039;https://graph.facebook.com/v25.0/106540352242922/messages&#039; \
-H &#039;Content-Type: application/json&#039; \
-H &#039;Authorization: Bearer EAAJB...&#039; \
-d &#039;
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;recipient_type&quot;: &quot;individual&quot;,
  &quot;to&quot;: &quot;+16505551234&quot;,
  &quot;type&quot;: &quot;audio&quot;,
  &quot;audio&quot;: &#123;
    &quot;id&quot; : &quot;1013859600285441&quot;,
    &quot;voice&quot;: true
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

