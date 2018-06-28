## Cancel a booking
A booking can be cancelled by API up until it is delivered to a ParcelPoint location. If a booking is cancelled after it has been received by the ParcelPoint location, charges may apply.

To cancel a parcel, you can call the API with the `parcelId` of the booking you want to cancel:
```json
PUT https://api.parcelpoint.com.au/api/v3/parcel/collect/{parcelId}/cancel?apiKey=12345678
``` 

#### Request Parameters

Name | Type | Required | Description
--- | --- | --- | ---
`parcelId` | String | Yes | Id of the parcel you want to cancel