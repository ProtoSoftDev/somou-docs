# Welcome Message Sequences - API Guide



When creating Click-to-WhatsApp ads, you can connect a Welcome Message Sequence from your messaging app. A sequence can include text, prefilled message, and FAQs.

This guide explains how to manage Welcome Message Sequences via the API endpoint.

## Requirements

Your app must be granted the **whatsapp_business_management** permission.

## Endpoints

```html
// Create a new sequence / Change an existing sequence
POST /&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;/welcome_message_sequences
```

```html
// Get a list of sequences / Get a specific sequence
GET /&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;/welcome_message_sequences
```

```html
// Delete a sequence
DELETE /&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;/welcome_message_sequences
```

## Create a sequence

To upload a new welcome message sequence, send a `POST` request to the `WHATSAPP_BUSINESS_ACCOUNT_ID/welcome_message_sequences` endpoint.

### Endpoint

```html
// Create a new sequence
POST /&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;/welcome_message_sequences
```

### Sample request

```curl
curl -X POST\
-F &#039;welcome_message_sequence=
      &#123;
       &quot;text&quot;:&quot;This is a welcome message authored in a 3P tool&quot;,
&quot;autofill_message&quot;: &#123;&quot;content&quot;: &quot;Hello! Can I get more info on this!&quot;&#125;,
&quot;ice_breakers&quot;:[
    &#123;&quot;title&quot;:&quot;Quick reply 1&quot;&#125;,
           &#123;&quot;title&quot;:&quot;Quick reply 2&quot;&#125;,
           &#123;&quot;title&quot;:&quot;Quick reply 3&quot;&#125;
        ]
      &#125;&#039; \
-F &#039;name=&quot;Driver sign-up&quot;&#039; \
&quot;https://graph.facebook.com/v14.0/WhatsappBusinessAccount/welcome_message_sequences&quot;
-H &#039;Authorization: Bearer &lt;ACCESS_TOKEN&gt;&#039;
```

### Sample response

The response includes a welcome message sequence ID.

```json
&#123;&quot;sequence_id&quot;:&quot;186473890&quot;&#125;
```

### Parameters

| Parameter | Description | Sample Value |
| --- | --- | --- |
| `sequence_id`&lt;br&gt;&lt;br&gt;_String_ | **Required**&lt;br&gt;&lt;br&gt;Identifier of the sequence. | `186473890` |
| `name`&lt;br&gt;&lt;br&gt;_String_ | **Required**&lt;br&gt;&lt;br&gt;Name of the sequence. | `Driver sign-up` |
| `welcome_message_sequence`&lt;br&gt;&lt;br&gt;_JSON Object_ | **Required**&lt;br&gt;&lt;br&gt;The welcome message JSON that will be sent upon clicking the ad. | ```curl
&#123;
  &quot;text&quot;:&quot;This is a welcome message authored in a 3P tool&quot;,
  &quot;autofill_message&quot;: &#123;&quot;content&quot;: &quot;Hello! Can I get more info on this!&quot;&#125;,
  &quot;ice_breakers&quot;:[
    &#123;&quot;title&quot;:&quot;Quick reply 1&quot;&#125;,
    &#123;&quot;title&quot;:&quot;Quick reply 2&quot;&#125;,
    &#123;&quot;title&quot;:&quot;Quick reply 3&quot;&#125;
  ]
&#125;
``` |

## Change an existing sequence

**Warning:** A sequence linked to an active ad cannot be deleted.

To update an existing sequence, send a `POST` request to the `WHATSAPP_BUSINESS_ACCOUNT_ID/welcome_message_sequences` endpoint with:

* The `sequence_id` parameter set to the ID of the sequence being updated
* Other parameters, like `name` or `welcome_message_sequence`, that need to be updated.

### Endpoint

```html
// Change an existing sequence
POST /&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;/welcome_message_sequences
```

### Sample request

```curl
curl -X POST\
-F &#039;sequence_id=&quot;186473890&quot;&#039;\
-F &#039;name=&quot;Driver sign-up updated name&quot;&#039; \
&quot;https://graph.facebook.com/v14.0/395394933592466/welcome_message_sequences&quot;
-H &#039;Authorization: Bearer BEAiil...&#039;
```

### Sample response

The response includes a success or error message.

```json
&#123;&quot;success&quot;: true&#125;
```

### Parameters

| Placeholder | Description | Sample Value |
| --- | --- | --- |
| `sequence_id`&lt;br&gt;&lt;br&gt;_String_ | **Required**&lt;br&gt;&lt;br&gt;Identifier of the sequence. | `186473890` |
| `name`&lt;br&gt;&lt;br&gt;_String_ | **Optional**&lt;br&gt;&lt;br&gt;Name of the sequence. | `Driver sign-up` |
| `welcome_message_sequence`&lt;br&gt;&lt;br&gt;_JSON Object_ | **Optional**&lt;br&gt;&lt;br&gt;The welcome message JSON that will be sent upon clicking the ad. | ```curl
&#123;
  &quot;text&quot;:&quot;This is a welcome message authored in a 3P tool&quot;,
  &quot;autofill_message&quot;: &#123;&quot;content&quot;: &quot;Hello! Can I get more info on this!&quot;&#125;,
  &quot;ice_breakers&quot;:[
    &#123;&quot;title&quot;:&quot;Quick reply 1&quot;&#125;,
    &#123;&quot;title&quot;:&quot;Quick reply 2&quot;&#125;,
    &#123;&quot;title&quot;:&quot;Quick reply 3&quot;&#125;
  ]
&#125;
``` |

## Get a list of sequences

To get an existing sequence, send a `GET` request to the `WHATSAPP_BUSINESS_ACCOUNT_ID/welcome_message_sequences` endpoint.

### Endpoint

```html
// Get a list of sequences
GET /&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;/welcome_message_sequences
```

### Sample request

```curl
curl -X GET &quot;https://graph.facebook.com/v14.0/395394933592466/welcome_message_sequences&quot;
     -H &#039;Authorization: Bearer BEAiil...&#039;
```

### Sample response

On success, the API returns a list of sequences for that app.

```json
[
  &#123;
    &quot;sequence_id&quot;:&quot;8716291&quot;,
    &quot;name&quot;:&quot;Driver Sign up&quot;,
    &quot;welcome_message_sequence&quot;:&quot;&lt;JSON_OBJECT&gt;&quot;,
    &quot;is_used_in_ad&quot;: true
  &#125;,
  &#123;
    &quot;sequence_id&quot;:&quot;4362&quot;,
    &quot;name&quot;:&quot;Basic Triage&quot;,
    &quot;welcome_message_sequence&quot;:&quot;&lt;JSON_OBJECT&gt;&quot;,
    &quot;is_used_in_ad&quot;: false
  &#125;,
  &#123;
    &quot;sequence_id&quot;:&quot;0139138&quot;,
    &quot;name&quot;:&quot;Appointment Schedule&quot;,
    &quot;welcome_message_sequence&quot;:&quot;&lt;JSON_OBJECT&gt;&quot;,
    &quot;is_used_in_ad&quot;: true
  &#125;
  ...
  ...
  ...,
  &#123;
    &quot;sequence_id&quot;:&quot;6987565&quot;,
    &quot;name&quot;:&quot;Car Leads&quot;,
    &quot;welcome_message_sequence&quot;:&quot;&lt;JSON_OBJECT&gt;&quot;,
    &quot;is_used_in_ad&quot;: false
  &#125;
]
```

## Get a specific sequence

To get a specific sequence, send a `GET` request to `WHATSAPP_BUSINESS_ACCOUNT_ID/welcome_message_sequences` with the `sequence_id` parameter set to the ID of the sequence you want to query.

### Endpoint

```html
// Get a specific sequence
GET /&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;/welcome_message_sequences
```

### Sample request

```curl
curl -X GET \
-F &#039;sequence_id=&quot;6987565&quot;&#039;
&quot;https://graph.facebook.com/v14.0/395394933592466/welcome_message_sequences&quot;
-H &#039;Authorization: Bearer BEAiil...&#039;
```

### Sample response

On success, the API returns the requested sequence.

```json
[
  &#123;
    &quot;sequence_id&quot;:&quot;6987565&quot;,
    &quot;name&quot;:&quot;Driver Sign up&quot;,
    &quot;welcome_message_sequence&quot;:&quot;&lt;JSON_OBJECT&gt;&quot;,
    &quot;is_used_in_ad&quot;: false
  &#125;
]
```

| Placeholder | Description | Sample Value |
| --- | --- | --- |
| `sequence_id`&lt;br&gt;&lt;br&gt;_String_ | **Optional**&lt;br&gt;&lt;br&gt;Identifier of the sequence. | `186473890` |
| `limit`&lt;br&gt;&lt;br&gt;_int_ | **Optional**&lt;br&gt;&lt;br&gt;Number of sequences to fetch. | `5` |

## Delete a sequence

**Warning:** A sequence linked to an active ad cannot be deleted.

To delete a sequence, send a `DELETE` request to `WHATSAPP_BUSINESS_ACCOUNT_ID/welcome_message_sequences` with the `sequence_id` parameter set to the ID of the sequence you want to delete.

### Endpoint

```html
// Delete a sequence
DELETE /&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;/welcome_message_sequences
```

### Sample request

```curl
curl -X DELETE \
-F &#039;sequence_id=&quot;1234567890&quot;&#039;
&quot;https://graph.facebook.com/v14.0/395394933592466/welcome_message_sequences&quot;
-H &#039;Authorization: Bearer BEAiil...&#039;
```

### Sample response

On success, the API returns a success confirmation.

```curl
&#123;&quot;success&quot;:true&#125;
```

| Placeholder | Description | Sample Value |
| --- | --- | --- |
| `sequence_id`&lt;br&gt;&lt;br&gt;_String_ | **Optional**&lt;br&gt;&lt;br&gt;Identifier of the sequence. | `186473890` |

## Webhook

The following webhook is triggered when a conversation is started after a user clicks an ad with a Click to WhatsApp&#039;s call-to-action.

```json
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;ID&quot;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;messaging_product&quot;: &quot;whatsapp&quot;,
            &quot;metadata&quot;: &#123;
              &quot;display_phone_number&quot;: &quot;PHONE_NUMBER&quot;,
              &quot;phone_number_id&quot;: &quot;PHONE_NUMBER_ID&quot;
            &#125;,
            &quot;contacts&quot;: [
              &#123;
                &quot;profile&quot;: &#123;
                  &quot;name&quot;: &quot;NAME&quot;
                &#125;,
                &quot;wa_id&quot;: &quot;ID&quot;
              &#125;
            ],
            &quot;messages&quot;: [
              &#123;
                &quot;referral&quot;: &#123;
                  &quot;source_url&quot;: &quot;AD_OR_POST_FB_URL&quot;,
                  &quot;source_id&quot;: &quot;ADID&quot;,
                  &quot;source_type&quot;: &quot;ad or post&quot;,
                  &quot;headline&quot;: &quot;AD_TITLE&quot;,
                  &quot;body&quot;: &quot;AD_DESCRIPTION&quot;,
                  &quot;media_type&quot;: &quot;image or video&quot;,
                  &quot;image_url&quot;: &quot;RAW_IMAGE_URL&quot;,
                  &quot;video_url&quot;: &quot;RAW_VIDEO_URL&quot;,
                  &quot;thumbnail_url&quot;: &quot;RAW_THUMBNAIL_URL&quot;,
                  &quot;ctwa_clid&quot;: &quot;CTWA_CLID&quot;,
                  &quot;ref&quot;: &quot;REF_ID&quot;,  // New field in referral

                &#125;,
                &quot;from&quot;: &quot;SENDER_PHONE_NUMBERID&quot;,
                &quot;id&quot;: &quot;wamid.ID&quot;,
                &quot;timestamp&quot;: &quot;TIMESTAMP&quot;,
                &quot;type&quot;: &quot;text&quot;,
                &quot;text&quot;: &#123;
                  &quot;body&quot;: &quot;BODY&quot;
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

## Marketing API experience

After you submit welcome message sequences through the API, use the sequence ID to configure ads through the Marketing API.

In the ad creative, the sequence ID can be set as follows:

```json
&#123;
  &quot;name&quot;: &quot;creative&quot;,
  &quot;object_story_spec&quot;: &#123;...&#125;,
  &quot;asset_feed_spec&quot;: &#123;
    &quot;additional_data&quot;: &#123;
      &quot;partner_app_welcome_message_flow_id&quot;: &quot;&lt;SEQUENCE_ID_RETURNED_FROM_POST_REQUEST&gt;&quot;
    &#125;
  &#125;
&#125;
```

For more information about messaging ads, refer to [Messaging Ads](https://developers.facebook.com/documentation/ads-commerce/marketing-api/ad-creative/messaging-ads) in the Marketing API documentation.

## Ads Manager experience walk-through

1. In the **Message Template** section of the Ad Creative, select **Partner App**

2. Click the **Partner app** dropdown and select the appropriate messaging partner app.

3. Under **Message sequence**, select the Welcome Message Sequence that you submitted via the API.

4. Preview your message sequence and click **Save**.

## Error codes

| Code | Description | Possible Solutions |
| --- | --- | --- |
| `4027001`&lt;br&gt;&lt;br&gt;Invalid input data | Some or all of the input data is not of the required format. | Check all the fields and parameters passed into the request are of the correct type and format, and that all required parameters are present. |
| `4027005`&lt;br&gt;&lt;br&gt;Unable to create a welcome message sequence | An error occurred while trying to create a new welcome message sequence. | Check that the access token has all the required permissions for the WhatsApp Business account. |
| `4027006`&lt;br&gt;&lt;br&gt;Unable to update a welcome message sequence | Unable to update the welcome message sequence. | Check all fields and the sequence ID for correctness. Check that the access token has the necessary permissions for the WhatsApp Business account. |
| `4027007`&lt;br&gt;&lt;br&gt;API unavailable | The API being accessed is not available for use yet. | Wait a day or two for the API to become available and try again. |
| `4027010`&lt;br&gt;&lt;br&gt;Missing parameter | One or more required parameters is missing. | Check all the documentation and ensure the required parameters are present. |
| `4027012`&lt;br&gt;&lt;br&gt;Sequence used in an ad | The welcome message sequence is linked to an active ad and cannot be updated or deleted. | Disconnect the sequence from the ad and try again. |
| `4027017`&lt;br&gt;&lt;br&gt;Could not load the sequence | Could not load the sequence being updated or deleted. | The welcome message sequence either does not exist, or you do not have permission to access it. Please check the access token and make sure you have the required permissions. |

