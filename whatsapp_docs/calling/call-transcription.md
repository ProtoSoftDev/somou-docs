# Call transcription


## Overview

The Calling API can transcribe the audio of business-initiated calls (BIC) and user-initiated calls (UIC) you make through the WhatsApp Business Cloud API. When you opt a call into transcription, both participants hear a short legally required announcement before transcription begins. After the call ends, you receive a webhook with a media ID you can use to download the finished transcript as a JSON document.

Transcription is opt-in on a per-call basis — you decide at the time you initiate or accept each call whether it should be transcribed.

Transcription and [call recording](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/call-recording/) are independent features. You can enable either one on its own, both together, or neither. They are configured and priced separately, each has its own request object, and each delivers its result in its own webhook event. Enabling transcription does not produce an audio recording, and enabling recording does not produce a transcript. See [Using transcription with recording](#using-transcription-with-recording) for what changes when you enable both on the same call.

## Prerequisites

Before you transcribe a call, make sure:

* Your business phone number has [Calling API enabled](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/call-settings).
* Your app is [subscribed to the `calls` webhook field](https://developers.facebook.com/documentation/business-messaging/whatsapp/webhooks/create-webhook-endpoint#configure-webhooks).
* You have obtained an open conversation or [call permission from the WhatsApp user](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/user-call-permissions) (for business-initiated calls).

## Enable transcription on a business-initiated call

Add a `transcription` object to your [business-initiated call request body](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/business-initiated-calls#part-2-your-business-initiates-a-new-call-to-the-whatsapp-user):

```html
POST /&lt;PHONE_NUMBER_ID&gt;/calls
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;to&quot;: &quot;14085551234&quot;,
  &quot;recipient&quot;: &quot;US.13491208655302741918&quot;,
  &quot;action&quot;: &quot;connect&quot;,
  &quot;session&quot;: &#123;
    &quot;sdp_type&quot;: &quot;offer&quot;,
    &quot;sdp&quot;: &quot;&lt;&lt;RFC 8866 SDP&gt;&gt;&quot;
  &#125;,
  &quot;transcription&quot;: &#123;
    &quot;status&quot;: &quot;ENABLED&quot;,
    &quot;purpose&quot;: &quot;quality assurance&quot;,
    &quot;announcement_language&quot;: &quot;en_US&quot;
  &#125;
&#125;
```

**Note:** **Usernames and business-scoped user IDs:** The `recipient` field lets you identify the WhatsApp user by their BSUID instead of, or in addition to, their phone number in `to`. For details, see [Business-scoped user IDs](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-scoped-user-ids#business-scoped-user-id).

## Enable transcription on a user-initiated call

Add the same `transcription` object when you accept an incoming call:

```html
POST /&lt;PHONE_NUMBER_ID&gt;/calls
&#123;
  &quot;messaging_product&quot;: &quot;whatsapp&quot;,
  &quot;call_id&quot;: &quot;wacid.ABGGFjFVU2AfAgo6V&quot;,
  &quot;action&quot;: &quot;accept&quot;,
  &quot;session&quot;: &#123;
    &quot;sdp_type&quot;: &quot;answer&quot;,
    &quot;sdp&quot;: &quot;&lt;&lt;RFC 8866 SDP&gt;&gt;&quot;
  &#125;,
  &quot;transcription&quot;: &#123;
    &quot;status&quot;: &quot;ENABLED&quot;,
    &quot;purpose&quot;: &quot;quality assurance&quot;,
    &quot;announcement_language&quot;: &quot;en_US&quot;
  &#125;
&#125;
```

To accept an incoming call without transcribing it, either omit the `transcription` field entirely or send it with `&quot;status&quot;: &quot;DISABLED&quot;`.

## Announcements and consent

Before any audio is transcribed, the Calling API mixes a spoken announcement into both your business and the WhatsApp user audio streams. The announcement is generated from the `purpose` string you provide and the `announcement_language` you select, for example:

&gt; _&quot;The audio of this call will be transcribed for the following purpose: &lt;your purpose string&gt;.&quot;_

Transcription starts only after the announcement has finished playing. A participant who does not consent can decline by terminating the call before or during the announcement.

The `purpose` field is mandatory whenever `status` is `ENABLED`. Calls submitted with transcription enabled but without a purpose are rejected with a request error.

## `transcription` object reference

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `status` | _String_ | Yes | `ENABLED` to transcribe the call, `DISABLED` to explicitly opt out. |
| `purpose` | _String_ | Yes, when `status` is `ENABLED` | The purpose of the transcription, spoken to both participants as part of the announcement. Maximum 250 characters. Provide the text in the language you specified in `announcement_language`. |
| `announcement_language` | _String_ | Yes, when `status` is `ENABLED` | Locale code for the language of the spoken announcement, for example `en_US` or `es`. See [Supported announcement languages](#supported-announcement-languages). |

## Supported announcement languages

The following `announcement_language` values have a localized announcement. The Calling API speaks the matching phrase, followed by your `purpose` string, to both participants before transcription begins.

| Language | `announcement_language` | Transcription announcement |
|---|---|---|
| English | `en` (also `en_US`, `en_AU`, `en_CA`, `en_GB`, `en_IN`, `en_NZ`) | The audio of this call will be transcribed for the following purpose: |
| French | `fr` | L&#039;audio de cet appel sera transcrit aux fins suivantes : |
| German | `de` | Dieser Anruf wird zu folgenden Zwecken transkribiert: |
| Hindi | `hi` | इस कॉल के ऑडियो को इस उद्देश्य के लिए ट्रांसक्राइब किया जाएगा: |
| Italian | `it` | L&#039;audio di questa chiamata verrà trascritto per il seguente scopo: |
| Kannada | `kn` | ಈ ಕರೆಯ ಆಡಿಯೋವನ್ನು ಈ ಕೆಳಗಿನ ಉದ್ದೇಶಕ್ಕಾಗಿ ಲಿಪ್ಯಂತರಿಸಲಾಗುತ್ತದೆ: |
| Portuguese (Brazil) | `pt` | O áudio desta ligação será transcrito para a seguinte finalidade: |
| Spanish | `es` | El audio de esta llamada se transcribirá con el siguiente propósito: |
| Telugu | `te` | ఈ కాల్ ఆడియో క్రింది ప్రయోజనం కోసం ట్రాన్‌స్క్రైబ్ చేయడం జరుగుతుంది: |
| Vietnamese | `vi` | Âm thanh của cuộc gọi này sẽ được chép lời cho mục đích sau: |

The `announcement_language` field also accepts `nl` and `es_ES`. These values are valid, but until a localized transcription announcement is available they play the English announcement.

## Using transcription with recording

Transcription and [recording](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/call-recording/) are fully independent. The `transcription` and `recording` objects are separate request fields, so you choose each one independently on a per-call basis:

* Send only `transcription` to receive a transcript and no audio recording.
* Send only `recording` to receive an audio recording and no transcript.
* Send both objects to receive both a transcript and an audio recording.
* Omit both (or set both to `DISABLED`) to receive neither.

When you enable both on the same call, participants hear a single combined announcement instead of two:

&gt; _&quot;The audio of this call will be recorded and transcribed for the following purpose: &lt;your purpose string&gt;.&quot;_

When both objects are present, the `announcement_language` and `purpose` from the `recording` object are used for this combined announcement, and the corresponding values in the `transcription` object are ignored. You still receive a separate webhook for each enabled feature: a [`call_recording_available`](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/call-recording/#recording-available-webhook) event for the recording and a [`call_transcription_available`](#transcription-available-webhook) event for the transcript.

## Transcription-available webhook

After the call ends and post-processing finishes (typically under one minute), the Calling API sends a `call_transcription_available` event under the existing `calls` webhook field:

```json
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;&lt;WABA_ID&gt;&quot;,
      &quot;changes&quot;: [
        &#123;
          &quot;field&quot;: &quot;calls&quot;,
          &quot;value&quot;: &#123;
            &quot;messaging_product&quot;: &quot;whatsapp&quot;,
            &quot;metadata&quot;: &#123;
              &quot;phone_number_id&quot;: &quot;&lt;BUSINESS_PHONE_NUMBER_ID&gt;&quot;,
              &quot;display_phone_number&quot;: &quot;&lt;BUSINESS_DISPLAY_PHONE_NUMBER&gt;&quot;
            &#125;,
            &quot;calls&quot;: [
              &#123;
                &quot;id&quot;: &quot;wacid.HBgLMTQxMjYxMzYyNTMVAgASGCBGO...&quot;,
                &quot;from&quot;: &quot;&lt;USER_PHONE_NUMBER&gt;&quot;,
                &quot;from_user_id&quot;: &quot;&lt;BSUID&gt;&quot;,
                &quot;from_parent_user_id&quot;: &quot;&lt;PARENT_BSUID&gt;&quot;,
                &quot;timestamp&quot;: &quot;1728932177&quot;,
                &quot;event&quot;: &quot;call_transcription_available&quot;,
                &quot;call_transcript&quot;: &#123;
                  &quot;document&quot;: &#123;
                    &quot;id&quot;: &quot;1002764438271669&quot;,
                    &quot;sha256&quot;: &quot;Y9vvGyeo3n76ptkXu3CwDBsnzbRFqpjHskQdMGSVqas=&quot;,
                    &quot;mime_type&quot;: &quot;application/json&quot;,
                    &quot;url&quot;: &quot;https://lookaside.fbsbx.com/whatsapp_business/attachments/?mid=133...&quot;
                  &#125;
                &#125;
              &#125;
            ]
          &#125;
        &#125;
      ]
    &#125;
  ]
&#125;
```

### `call_transcript` fields

| Field | Type | Description |
| --- | --- | --- |
| `document.id` | _String_ | Media asset ID. Use the [Media API](https://developers.facebook.com/docs/whatsapp/cloud-api/reference/media) to [retrieve the media URL](https://developers.facebook.com/docs/whatsapp/cloud-api/reference/media#retrieve-media-url) for download. |
| `document.sha256` | _String_ | Base64-encoded SHA-256 hash of the transcript document. Use it to verify the downloaded file&#039;s integrity. |
| `document.mime_type` | _String_ | MIME type of the transcript document. Currently always `application/json`. |
| `document.url` | _String_ | A short-lived download URL. Issue an authenticated GET request with your access token to download the asset. |

**Note:** **Usernames and business-scoped user IDs:** The `from_user_id` and `from_parent_user_id` fields identify the WhatsApp user by their BSUID; the `from` phone number may be omitted if the user has adopted a username. For details, see [Business-scoped user IDs](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-scoped-user-ids#business-scoped-user-id).

## Transcript language

You do not specify a transcription language in the request. The Calling API automatically detects the spoken language of the call, transcribes it, and reports the detected language in the `transcript.language` field of the transcript document (see [Transcript document format](#transcript-document-format)). This detected language is an ISO 639 language code such as `en` and is determined from the audio — it is independent of the `announcement_language` you set for the spoken announcement.

The set of languages that can be automatically detected and transcribed is evolving constantly as the underlying speech models improve, so this list changes over time. The languages currently supported include:

Afrikaans, Albanian, Arabic, Azerbaijani, Bengali, Bulgarian, Burmese, Cebuano, Chinese, Croatian, Czech, Danish, Dutch, English, Finnish, French, German, Greek, Guarani, Gujarati, Hebrew, Hindi, Hungarian, Indonesian, Italian, Japanese, Javanese, Kannada, Korean, Macedonian, Malay, Malayalam, Marathi, Norwegian, Persian, Polish, Portuguese, Punjabi, Romanian, Russian, Serbian, Sinhala, Slovak, Slovenian, Spanish, Swahili, Swedish, Tagalog (Filipino), Tamil, Telugu, Thai, Turkish, Urdu, and Vietnamese.

If a call is spoken in a language that isn&#039;t currently supported, you still receive the `call_transcription_available` webhook, but the returned transcript may be empty.

## Transcript document format

The downloaded transcript is a JSON document with two top-level objects: `metadata` (information about the processed audio) and `transcript` (the transcribed content, including a flat `text` rendering, the detected `language`, an overall `confidence`, and time-stamped `segments` with word-level detail).

Each segment is attributed to the speaker who produced it and the channel it was spoken on — channel `0` is your business and channel `1` is the WhatsApp user — so speaker attribution stays accurate even when participants talk over each other. The full conversation is also available as a single string in `transcript.text`, with each segment prefixed by its speaker label, for example `[Business]` or `[Customer]`.

```json
&#123;
  &quot;metadata&quot;: &#123;
    &quot;processed_at&quot;: &quot;2026-06-18T20:16:47Z&quot;,
    &quot;audio&quot;: &#123;
      &quot;duration&quot;: 21.76,
      &quot;sample_rate&quot;: 16000,
      &quot;channels&quot;: 2,
      &quot;audio_format&quot;: &quot;stereo&quot;
    &#125;
  &#125;,
  &quot;transcript&quot;: &#123;
    &quot;text&quot;: &quot;[Business] Hello, how about you? [Customer] Hey, I&#039;m good. How are you?&quot;,
    &quot;language&quot;: &quot;en&quot;,
    &quot;duration&quot;: 21.76,
    &quot;confidence&quot;: 0.83,
    &quot;segments&quot;: [
      &#123;
        &quot;id&quot;: 1,
        &quot;speaker&quot;: &quot;Business&quot;,
        &quot;channel&quot;: 0,
        &quot;start&quot;: 1.16,
        &quot;end&quot;: 2.44,
        &quot;text&quot;: &quot;Hello, how about you?&quot;,
        &quot;confidence&quot;: 0.85,
        &quot;words&quot;: [
          &#123;
            &quot;word&quot;: &quot;Hello,&quot;,
            &quot;start&quot;: 1.16,
            &quot;end&quot;: 1.64,
            &quot;confidence&quot;: 0.89,
            &quot;lang&quot;: &quot;en&quot;
          &#125;,
          &#123;
            &quot;word&quot;: &quot;how&quot;,
            &quot;start&quot;: 1.64,
            &quot;end&quot;: 1.8,
            &quot;confidence&quot;: 0.99,
            &quot;lang&quot;: &quot;en&quot;
          &#125;,
          &#123;
            &quot;word&quot;: &quot;about&quot;,
            &quot;start&quot;: 1.8,
            &quot;end&quot;: 2.04,
            &quot;confidence&quot;: 0.52,
            &quot;lang&quot;: &quot;en&quot;
          &#125;,
          &#123;
            &quot;word&quot;: &quot;you?&quot;,
            &quot;start&quot;: 2.04,
            &quot;end&quot;: 2.44,
            &quot;confidence&quot;: 0.99,
            &quot;lang&quot;: &quot;en&quot;
          &#125;
        ]
      &#125;,
      &#123;
        &quot;id&quot;: 2,
        &quot;speaker&quot;: &quot;Customer&quot;,
        &quot;channel&quot;: 1,
        &quot;start&quot;: 3.66,
        &quot;end&quot;: 5.74,
        &quot;text&quot;: &quot;Hey, I&#039;m good. How are you?&quot;,
        &quot;confidence&quot;: 0.85,
        &quot;words&quot;: [
          &#123;
            &quot;word&quot;: &quot;Hey,&quot;,
            &quot;start&quot;: 3.66,
            &quot;end&quot;: 4.46,
            &quot;confidence&quot;: 0.60,
            &quot;lang&quot;: &quot;en&quot;
          &#125;,
          &#123;
            &quot;word&quot;: &quot;I&#039;m&quot;,
            &quot;start&quot;: 4.46,
            &quot;end&quot;: 4.7,
            &quot;confidence&quot;: 0.78,
            &quot;lang&quot;: &quot;en&quot;
          &#125;,
          &#123;
            &quot;word&quot;: &quot;good.&quot;,
            &quot;start&quot;: 4.7,
            &quot;end&quot;: 5.02,
            &quot;confidence&quot;: 0.71,
            &quot;lang&quot;: &quot;en&quot;
          &#125;,
          &#123;
            &quot;word&quot;: &quot;How&quot;,
            &quot;start&quot;: 5.02,
            &quot;end&quot;: 5.18,
            &quot;confidence&quot;: 0.99,
            &quot;lang&quot;: &quot;en&quot;
          &#125;,
          &#123;
            &quot;word&quot;: &quot;are&quot;,
            &quot;start&quot;: 5.18,
            &quot;end&quot;: 5.34,
            &quot;confidence&quot;: 0.99,
            &quot;lang&quot;: &quot;en&quot;
          &#125;,
          &#123;
            &quot;word&quot;: &quot;you?&quot;,
            &quot;start&quot;: 5.34,
            &quot;end&quot;: 5.74,
            &quot;confidence&quot;: 0.99,
            &quot;lang&quot;: &quot;en&quot;
          &#125;
        ]
      &#125;
    ]
  &#125;
&#125;
```

### `metadata` fields

| Field | Type | Description |
| --- | --- | --- |
| `processed_at` | _String_ | ISO 8601 UTC timestamp of when transcription post-processing completed. |
| `audio.duration` | _Number_ | Duration of the processed call audio, in seconds. |
| `audio.sample_rate` | _Integer_ | Sample rate of the processed audio, in Hz. |
| `audio.channels` | _Integer_ | Number of audio channels. A two-party call has two channels. |
| `audio.audio_format` | _String_ | Format of the processed audio mix, for example `stereo`. |

### `transcript` fields

| Field | Type | Description |
| --- | --- | --- |
| `text` | _String_ | The full conversation as a single string. Each segment is prefixed with its speaker label in brackets, for example `[Business]` or `[Customer]`. |
| `language` | _String_ | The detected language of the call as an ISO 639 code, for example `en`. See [Transcript language](#transcript-language). |
| `duration` | _Number_ | Total duration of the transcribed audio, in seconds. |
| `confidence` | _Number_ | Overall confidence score for the transcript, from `0` to `1`. |
| `segments` | _Array_ | The ordered list of spoken segments. See [`segments` fields](#segments-fields). |

### `segments` fields

Each segment represents a continuous span of speech from one speaker.

| Field | Type | Description |
| --- | --- | --- |
| `id` | _Integer_ | Sequential identifier for the segment within the transcript. |
| `speaker` | _String_ | The speaker who produced the segment, either `Business` or `Customer`. |
| `channel` | _Integer_ | The audio channel the segment was spoken on. Channel `0` is the business; channel `1` is the WhatsApp user. |
| `start` | _Number_ | The start time of the segment, in seconds from the beginning of the call audio. |
| `end` | _Number_ | The end time of the segment, in seconds from the beginning of the call audio. |
| `text` | _String_ | The full transcribed text of the segment. |
| `confidence` | _Number_ | A confidence score from `0` to `1` for the segment transcription. |
| `words` | _Array_ | Word-level breakdown of the segment. Each entry contains `word` (_String_), `start` (_Number_), `end` (_Number_), `confidence` (_Number_), and `lang` (_String_, the ISO 639 code of the detected language for that word), where `start` and `end` are in seconds from the beginning of the call audio. |

## Download the transcript

Transcripts use the same download flow as [media messages](https://developers.facebook.com/docs/whatsapp/cloud-api/reference/media#retrieve-media-url):

1. The `url` returned in the webhook is valid for 5 minutes. Issue an authenticated GET request with your access token to download the file directly.
2. If the URL has expired, use the [Media API](https://developers.facebook.com/docs/whatsapp/cloud-api/reference/media) to [retrieve a fresh media URL](https://developers.facebook.com/docs/whatsapp/cloud-api/reference/media#retrieve-media-url) with the `document.id`.

## Retention

Transcripts remain available for download for **7 days** after the `call_transcription_available` webhook is delivered. After that period, the media ID expires and the underlying file is deleted. Download and persist the transcript to your own storage within the retention window if you need to keep it long-term.

## Errors

The following request errors are specific to call transcription. See [Cloud API error codes](https://developers.facebook.com/docs/whatsapp/cloud-api/support/error-codes/) for the full list.

| Scenario | Description |
| --- | --- |
| Missing `purpose` | `transcription.status` is `ENABLED` but `purpose` is omitted or empty. |
| `purpose` too long | `purpose` exceeds 250 characters. |
| Invalid `announcement_language` | `announcement_language` is not a supported locale code. |
| Invalid `status` | `status` is not one of `ENABLED` or `DISABLED`. |

