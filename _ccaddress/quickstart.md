## Quickstart
Click and Collect Address is a two-step process, all which is done on the client side. The first step of the process is getting the ParcelPoint locations and their details and displaying them on the website for the user to choose from. Once the user has made their choice, the second step is to create a booking with ParcelPoint.  

### Step 1: Initialise the widget
The simplest way for you to allow customers to pick a ParcelPoint location for collection is with the ParcelPoint widget. It combines HTML, JavaScript, and CSS to create an embedded list and map of all ParcelPoint store closest to the user's chosen location.

To see the widget in action, click the search bar and input one of the following:
* full or partial address (eg. 46 Kippax Street)
* town (eg. Surry Hills)
* city (eg. Sydney)
* postcode (eg. 2010)

[filename](https://teamparcelpoint.github.io/parcelpoint-docs/_media/widget.html ':include')

To get started, add the following code to your checkout page:
```html
<!-- Add this <div> to the part of the page where you want to show the widget -->
<div id="parcelpoint-stores-widget"></div> 
```

```html
<!-- Add this script to load the widget -->
<script type="text/javascript" src="https://connect.parcelpoint.com.au/widget/v3.1/stores"></script>
```

```html
<!-- Add this script to initialise the widget -->
<script type="text/javascript">
    parcelpoint.Store.init({
        apiKey: "your-parcelpoint-api-key",
        initialAddress: '2000',
        targetDiv: "parcelpoint-stores-widget",
        storeType: "full",
        initialView: "map",
        selectFirstStore: true,
        yourLocationMarker: true,
        googleAPIKey: "your-google-api-key"
    });
    parcelpoint.Store.display();
</script>
```

Parameter | Type | Required | Description
--- | --- | --- | ---
`apiKey` | String | Yes | To get an `apiKey` you can [email us](mailto:requests@parcelpoint.com.au) to request one
`initialAddress` | String | Yes | The address you want the widget to select when loaded
`targetDiv` | String | Yes | The `id` of the parent element the widget will render inside
`initialView` | String | Yes | The default view type on load. Th view types are "map" and "list"
`selectFirstStore` | String | Yes | If `true` the nearest store to the `initialAddress` will be selected
`yourLocationMarker` | String | Yes | If `true` a pin will be dropped at the user's location on the map
`googleAPIKey` | String | Yes | This is needed to show the map on the widget.

An alternative to the widget demonstrated above is to implement a [custom integration](/ccaddress?id=custom-integration). The custom approach allows you to use any HTML element or JavaScript event to get the information needed from ParcelPoint to create a booking.

#### Custom styling
If you’d prefer to have complete control over the look and feel of the ParcelPoint widget, you can overwrite our CSS with your own CSS file. 

### Step 2: Create a parcel booking
Once the user has selected a store they want their parcel to be delivered to, they will need to create a booking with ParcelPoint. To do this, the parcel must be ready for dispatch with the following information:
* `consignmentReference`
* `trackingLabel`
* `orderRef2`
* `autoConfirm: true`

**Important:** The parcel needs to have the status `AWAITING_ARRIVAL` for it to be created.

Using the endpoint below, when submitting your checkout form you will need compile a request body to be sent from the form.

**Example request**
```javascript
POST https://api.parcelpoint.com.au/api/v3/parcel/collect?apiKey=12345678
```

**Example request body**
```json
{
    "type": "CollectInitialRetailer",
    "agentId": "2015-01",
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
    "trackingLabel": "CPAP97Z0532929001",
    "autoConfirm": "false"
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
`type` | String | Yes | The type of parcel. Please use CollectInitialRetailer, CollectInitialDefault
`agentId` | String | Yes | The destination agent id. This should correspond to the store id selected by the user and exposed via the ParcelPoint widget.
`user` | Object | Yes | user data object. Detailed later.
`authorisedUsers` | Array | No | Nested list of nominated people who can collect a parcel at the agent on behalf of the original user.
`trackingLabel` | String | Yes | This is the barcode number.
`order` | Object | Yes | Details of the order
`carrier` | Object | Yes | Details of the carrier
`orderItems` | Array | No | Nested list of order item details.
`isTest` | Boolean | No | If the parcel is a test parcel. (Default: FALSE)
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