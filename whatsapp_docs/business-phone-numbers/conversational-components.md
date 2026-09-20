# Conversational Components



Conversational components are in-chat features that you can enable on business phone numbers. They make it easier for WhatsApp users to interact with your business. You can configure easy-to-use commands and provide pre-written ice breakers that users can tap.

## Limitations

If a WhatsApp user taps a [universal link](https://faq.whatsapp.com/425247423114725) (that is, **wa.me** link) configured with pre-filled text, the user interfaces for **ice breakers** are automatically dismissed.

## Configure using WhatsApp Manager (WAM)

You can configure all of these features in WhatsApp Manager on the specific numbers you choose:

1. Navigate to the [My Apps dashboard in the Meta for Developers site.](https://developers.facebook.com/apps/)
2. Select your app, then on the left panel select **Configuration** under **WhatsApp**.
3. Under **Phone Numbers** select **Manage Phone Numbers**.
4. On the far right of the phone number you want to configure, select the **Gear Icon** under **Settings**.
5. Select **Automations**.
6. Access and configure Conversational Components.

Solution Partners can configure these features for their customers as well if they have access to their customers&#039; WhatsApp Business account in WhatsApp Manager.

## Ice breakers

Ice breakers are customizable, tappable text strings that appear in a message thread the first time you chat with a user. For example, &quot;Plan a trip&quot; or &quot;Create a workout plan&quot;.

Ice breakers are great for service interactions, such as customer support or account servicing. For example, you can embed a WhatsApp button on your app or website. When users tap the button, they are redirected to WhatsApp, where they can choose from a set of customizable prompts, showing them how to interact with your services.

You can configure up to 4 ice breakers on a business phone number. Each ice breaker can have a maximum of 80 characters. Emojis are not supported.

When a user taps an ice breaker, it triggers a standard received message webhook. The payload assigns the ice breaker string to the `body` property. If the user attempts to message you instead of tapping an ice breaker, the keyboard appears as an overlay, but the user can dismiss it to see the ice breaker menu again.

**Warning:** If a WhatsApp user taps a [universal link](https://faq.whatsapp.com/425247423114725) (**wa.me** or **api.whatsapp.com** links) configured with pre-filled text, WhatsApp automatically dismisses the user interfaces for **ice breakers**.

### Webhook payload

```json
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;&quot;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;messaging_product&quot;: &quot;whatsapp&quot;,
            &quot;metadata&quot;: &#123;
              &quot;display_phone_number&quot;: &quot;&lt;BUSINESS_DISPLAY_PHONE_NUMBER&gt;&quot;,
              &quot;phone_number_id&quot;: &quot;&lt;BUSINESS_PHONE_NUMBER_ID&gt;&quot;
            &#125;,
            &quot;contacts&quot;: [
              &#123;
                &quot;profile&quot;: &#123;
                  &quot;name&quot;: &quot;&lt;WHATSAPP_USER_NAME&gt;&quot;
                &#125;,
                &quot;wa_id&quot;: &quot;&lt;WHATSAPP_USER_ID&gt;&quot;
              &#125;
            ],
            &quot;messages&quot;: [
              &#123;
                &quot;from&quot;: &quot;&lt;WHATSAPP_USER_PHONE_NUMBER_ID&gt;&quot;,
                &quot;id&quot;: &quot;&lt;WHATSAPP_MESSAGE_ID&gt;&quot;,
                &quot;timestamp&quot;: &quot;&lt;TIMESTAMP&gt;&quot;,
                &quot;text&quot;: &#123;
                  &quot;body&quot;: &quot;Plan a trip&quot;
                &#125;,
                &quot;type&quot;: &quot;text&quot;
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

## Commands

Commands are text strings that WhatsApp users can see by typing a forward slash in a message thread with your business.

Commands are composed of the command itself and a hint, which gives the user an idea of what can happen when they use the command. For example, you could define the command:

`/imagine - Create images using a text prompt`

When a WhatsApp user types, _/imagine cars racing on Mars_, it would trigger a received message webhook with that exact text string assigned to the `body` property. You could then generate and return an image of cars racing on the planet Mars.

You can define up to 30 commands. Each command has a maximum of 32 characters, and each hint has a maximum of 256 characters. Emojis are not supported.

### Webhook payload

```json
&#123;
  &quot;object&quot;: &quot;whatsapp_business_account&quot;,
  &quot;entry&quot;: [
    &#123;
      &quot;id&quot;: &quot;&lt;WHATSAPP_BUSINESS_ACCOUNT_ID&gt;&quot;,
      &quot;changes&quot;: [
        &#123;
          &quot;value&quot;: &#123;
            &quot;messaging_product&quot;: &quot;whatsapp&quot;,
            &quot;metadata&quot;: &#123;
              &quot;display_phone_number&quot;: &quot;&lt;BUSINESS_DISPLAY_PHONE_NUMBER&gt;&quot;,
              &quot;phone_number_id&quot;: &quot;&lt;BUSINESS_PHONE_NUMBER_ID&gt;&quot;
            &#125;,
            &quot;contacts&quot;: [
              &#123;
                &quot;profile&quot;: &#123;
                  &quot;name&quot;: &quot;&lt;WHATSAPP_USER_NAME&gt;&quot;
                &#125;,
                &quot;wa_id&quot;: &quot;&lt;WHATSAPP_USER_ID&gt;&quot;
              &#125;
            ],
            &quot;messages&quot;: [
              &#123;
                &quot;from&quot;: &quot;&lt;WHATSAPP_USER_PHONE_NUMBER_ID&gt;&quot;,
                &quot;id&quot;: &quot;&lt;WHATSAPP_MESSAGE_ID&gt;&quot;,
                &quot;timestamp&quot;: &quot;&lt;TIMESTAMP&gt;&quot;,
                &quot;text&quot;: &#123;
                  &quot;body&quot;: &quot;/imagine cars racing on Mars&quot;
                &#125;,
                &quot;type&quot;: &quot;text&quot;
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

## Configure using the API

Using the API, you can also configure conversational components and view any configured values.

Use the [Conversational Automation API](https://developers.facebook.com/documentation/business-messaging/whatsapp/reference/whatsapp-business-account/conversational-automation-api) to configure conversational components and view configured values on a given phone number.

### Configure conversational components via the API

You can configure conversational components on a given phone number by calling the POST endpoint.

#### Request syntax

```https
// Configure Commands with names and descriptions
POST /&lt;PHONE_NUMBER_ID&gt;/conversational_automation?commands=&lt;COMMAND_LIST&gt;

// Configure Prompts
POST /&lt;PHONE_NUMBER_ID&gt;/conversational_automation?prompts=&lt;PROMPT&gt;
```

#### Body properties

| Placeholder | Description | Sample Value |
| --- | --- | --- |
| `&lt;PHONE_NUMBER_ID&gt;`&lt;br&gt;&lt;br&gt;_String_ | **Required.**&lt;br&gt;&lt;br&gt;A phone number ID on a WhatsApp Business account. | `+12784358810` |
| `&lt;COMMAND_LIST&gt;`&lt;br&gt;&lt;br&gt;_JSON_ | **Optional.**&lt;br&gt;&lt;br&gt;A list of commands to be configured. | ```json
[
  &#123;
    &quot;command_name&quot;: &quot;generate&quot;,
    &quot;command_description&quot;: &quot;Create a new image&quot;
  &#125;,
  &#123;
    &quot;command_name&quot;: &quot;rethink&quot;,
    &quot;command_description&quot;: &quot;Generate new images from existing images&quot;
  &#125;
]
``` |
| `&lt;PROMPTS&gt;`&lt;br&gt;&lt;br&gt;_List of String_ | **Optional.**&lt;br&gt;&lt;br&gt;The prompt(s) to be configured. | `&quot;prompts&quot;: [&quot;Book a flight&quot;,&quot;plan a vacation&quot;]` |

#### Sample request

```curl
   curl -X POST \
 &#039;https://graph.facebook.com/v22.0/PHONE_NUMBER_ID/conversational_automation&#039; \
 -H &#039;Authorization: Bearer ACCESS_TOKEN&#039; \
 -H &#039;Content-Type: application/json&#039; \
 -d &#039;&#123;
   &quot;commands&quot;: [
     &#123;
       &quot;command_name&quot;: &quot;tickets&quot;,
       &quot;command_description&quot;: &quot;Book flight tickets&quot;
     &#125;,
     &#123;
       &quot;command_name&quot;: &quot;hotel&quot;,
       &quot;command_description&quot;: &quot;Book hotel&quot;
     &#125;
   ],
   &quot;prompts&quot;: [&quot;Book a flight&quot;, &quot;plan a vacation&quot;]
&#125;&#039;
```

#### Sample response

```json
&#123;
  &quot;success&quot;: true
&#125;
```

### View the current configuration using the API

You can view the current configuration of Conversational Components on a given phone number by calling the GET endpoint.

#### Request syntax

```https
GET  /&lt;PHONE_NUMBER_ID&gt;?fields=conversational_automation
```

#### Sample response

```json
&#123;
  &quot;conversational_automation&quot;: &#123;
    &quot;prompts&quot;: [
      &quot;Find the best hotels in the area&quot;,
      &quot;Find deals on rental cars&quot;
    ],
    &quot;commands&quot;: [
      &#123;
        &quot;command_name&quot;: &quot;tickets&quot;,
        &quot;command_description&quot;: &quot;Book flight tickets&quot;
      &#125;,
      &#123;
        &quot;command_name&quot;: &quot;hotel&quot;,
        &quot;command_description&quot;: &quot;Book hotel&quot;
      &#125;
    ]
  &#125;,
  &quot;id&quot;: &quot;123456&quot;
&#125;
```

## Testing

To test conversational components once they have been configured, open the WhatsApp client and open a chat with your business phone number.

For ice breakers, if you already have a chat thread going with the business phone number, you must first delete the chat thread:

1. Open the thread in the WhatsApp client.
1. Tap the business phone number&#039;s profile.
1. Tap **Clear Chat** &gt; **Clear All Messages**.
1. **Delete Chat**.
1. Start a new chat thread with this business.

You can then send a message to the business phone number to test your ice breakers.
