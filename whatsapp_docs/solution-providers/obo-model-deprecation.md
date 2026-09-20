# On-Behalf-Of account ownership model deprecation



We have deprecated the On-Behalf-Of (&quot;OBO&quot;) account ownership model and replaced it with [partner-initiated WABA creation](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/partner-initiated-waba-creation). All existing WABAs created under the OBO model should have been transferred to clients by October 1, 2025. Post 1st October 2025, all the eligible OBO accounts will be auto-migrated in batches through the end of 2025.

## Deprecation timeline

- **March 24, 2025**: [partner-initiated WABA creation](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/partner-initiated-waba-creation) is made available to all Solution Partners.
- **September 29, 2025**: last day to onboard clients to the OBO model.
- **October 1, 2025**: last day to transfer ownership of OBO model WABAs to clients.

## Payment methods

Partner-initiated WABA creation does not support automatic payment setup. Instead, you must share your credit line with the client via the API. See [Partner-initiated WABA creation](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/partner-initiated-waba-creation) for details.

## Multi-Partner Solutions

Clients cannot be onboarded to a Multi-Partner Solution as part of the
partner-initiated WABA creation process, but can be added to an MPS afterwards. See [Partner-initiated WABA creation](https://developers.facebook.com/documentation/business-messaging/whatsapp/solution-providers/partner-initiated-waba-creation) for details.

## Marketing Messages API for WhatsApp

Existing OBO model WABAs need to be transferred to clients if you want to use them with the Marketing Messages API for WhatsApp, but this can be done as part of the [Marketing Messages API for WhatsApp onboarding process](https://developers.facebook.com/documentation/business-messaging/whatsapp/marketing-messages/onboarding#onboard-via-a-partner-using-whatsapp-manager).
