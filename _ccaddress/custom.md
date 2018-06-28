## Custom Integration
Some retailers prefer to build their own look and feel for customers who want to search ParcelPoint locations.
To do this, we have an API that will send you all ParcelPoint locations and details of each location including:
* ParcelPoint location name (Store name)
* Geographic location (Google Maps)
* Full address
* Operating hours

It is important to know that this location data is subject to frequent changes due to opening hours changing and locations being added and closed.

Some examples of retailers that have built their own integration:

![](_media/cca-custom-ebay.png)

![](_media/cca-custom-iconic.png)

Using the endpoint below, you will be able fetch all ParcelPoint locations.

**Example request**
```javascript
GET https://api.parcelpoint.com.au/api/v1/agent/geo?max=1&query=2010
```

**Example response body**
```json
{
  "results": [
    {
      "id": "2010-03",
      "name": "Foveaux Mini-Market ParcelPoint",
      "status": "ACTIVE",
      "address": "Shop 2A, 17-51 Foveaux Street",
      "city": "Surry Hills",
      "postcode": "2010",
      "state": "NSW",
      "open": "Mon-Fri 7am-11:30pm, Sat 9am-11:30pm, Sun 10am-11pm",
      "lat": "-33.8842435",
      "lng": "151.2096471",
      "distance": "0.37433270896127807",
      "alternateId": "P734"
    }
  ],
  "size": 1,
  "total": 71
}
```

#### Request Parameters

Name | Description
--- | ---
`max=n` | `n` is maximum number of stores you want to get from the API. This number only applies to stores within `high` and `medium` coverage.
`query=location` | `location` is the suburb, postcode or address you want to send to the API to get all the stores within `high` and `medium` coverage.