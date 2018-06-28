## Update a booking
A booking can be updated by API up until it is delivered to a ParcelPoint location. If a booking has been received by the ParcelPoint location, you will be unable to update it's details.

To update a parcel, you can call the API with the `parcelId` of the booking you want to update:
```json
POST https://api.parcelpoint.com.au/api/v3/parcel/collect/2003-0000SNW3?apiKey=123456
``` 

**Example request body**
```json
{
    "user": {
        "userId": "0004",
        "mobile": "0456 345 864"
    },
    "authorisedUsers": [
        {
            "firstName": "John",
            "lastName": "Smith",
            "mobile": "0456 345 864",
            "email": "john.smith@email.com"
        },
        {
            "firstName": "Lucy",
            "lastName": "Lui",
            "mobile": "0411 475 302",
            "email": "lucy.lui@fastmail.com"
        }
    ],
    "order": {
        "retailerName": "Oroton",
        "supplierName": "Beijing Clothing Co",
        "orderRef1": "8921852",
        "orderRef2": "SYD",
        "value": "99.95",
        "quantity": "1",
        "eta": "20181114-8.00.00"
    },
    "carrier": {
        "consignmentRef": "CPAP97Z0532929"
    },
    "isTest": "false",
}
```

**Example response**
```json
{
    "status": 200,
    "userId": "0004",
    "parcelId": "2003-0000SNW3"
}
```

#### Request Parameters
Name | Type | Required | Description
--- | --- | --- | ---
`user` | Object | Yes | user data object. Detailed later.
`authorisedUsers` | Array | No | Nested list of nominated people who can collect a parcel at the agent on behalf of the original user.
`trackingLabel` | String | No | This is the barcode number. _Required only when dispatching the parcel_
`order` | Object | Yes | Details of the order
`carrier` | Object | No | Details of the carrier
`orderItems` | Array | No | Nested list of order item details.
`isTest` | Boolean | No | If the parcel is a test parcel. (Default: `false`)
`autoConfirm` | Boolean | No | Set the value to `true` and it will check if the `consignmentReference`, `trackingLabel`, `orderRef2` has been provided. If any of the parameters are missing, a validation error will be returned.

#### User Parameters

Name | Type | Required | Description
--- | --- | --- | ---
`userId` | String | No | The ParcelPoint user id of the user from which this parcel originated. _This is required if registration details are not provided._
`firstName` | String | No | The first name of the origin user. _This is required if `userId` is not provided._
`lastName` | String | No | The last name of the origin user. _This is required if `userId` is not provided._
`email` | String | No | The email address of the origin user. _This is required if `userId` is not provided._
`mobile` | String | No | The mobile phone number of the origin user.

#### Authorised Person Parameters

Name | Type | Required | Description
--- | --- | --- | ---
`firstName` | String | Yes | First name
`lastName` | String | Yes | Last name
`mobile` | String | No | Mobile phone number. _Required if email is not provided._
`email` | String | No | E-mail address. _Required if mobile phone number is not provided._

#### Order Parameters

Name | Type | Required | Description
--- | --- | --- | ---
`retailerName` | String | No | Name of the retailer.
`supplierName` | String | Yes | Name of the supplier for the delivery. This is often the distributor of the parcel.
`orderRef1` | String | No | First order reference of the order.
`orderRef2` | String | No | ParcelPoint HUB code. Choose from `SYD`, `MEL`, `PER`, `BNE`.
`value` | String | No | Value of the order.
`quantity` | String | No | Quantity of items in the order
`eta` | String | No | Estimated date or arrival for the order

#### Carrier Parameters

Name | Type | Required | Description
--- | --- | --- | ---
`carrierName` | String | No | Name of the carrier handling the delivery (TOLL, Courier Please, TNT, etc).
`consignmentRef` | String | Yes | Carrier consignment reference

#### Order Items Parameters

Name | Type | Required | Description
--- | --- | --- | ---
`itemRef` | String | Yes | Item reference
`name` | String | No | Name of the item
`desc` | String | No | Full description of the item
`value` | String | No | Purchase value
`quantity` | Integer | No | Number of items
`height` | Integer | No | Height of the parcel in cm
`width` | Integer | No | Width of the parcel in cm
`length` | Integer | No | Length of the parcel in cm
`weight` | Integer | No | Weight of the parcel in grams
`cubicWeight`| Integer | No | Cubic weight of the parcel in grams

#### Response Parameters

Name | Type | Description
--- | --- | ---
`userId` | String | ParcelPoint `userId` of the user associated with the newly booked parcel.
`parcelId` | String | ParcelPoint `parcelId` of the newly booked parcel.