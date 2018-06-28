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

### Flat File (CSV) Integration
Although we highly recommend that your use the API to get the most up-to-date ParcelPoint locations, you are also able to request a CSV file on a daily basis to use on the retailer's website. 
ParcelPoint will place a CSV file on a daily basis at a arranged time on the partner's SFTP server using the file name:

`ParcelPoint_Active_PUDO_List-YYYYMMDD.csv`

To request access to a Flat File (CSV) of all our ParcelPoint locations daily, please [contact us](mailto:requests@parcelpoint.com.au) with the following details of your SFTP server:
* Hostname
* Username
* Password
* Protocol
* Folder
* Options (optional)
* Header (optional)

**CSV File Structure**

CSV | Format | Description
--- | --- | ---
Agent Name | String (Max 100 characters) | PUDO store name always suffixed with 'ParcelPoint' e.g. Easy Mini-Market ParcelPoint
ID | String XXXX-YY	| PUDO store's Unique Identification; XXXX : Represents Australian Postal Code; YY: Represents store number in that Postal Code
Street | String (Max 100 characters) | PUDO store's Street address e.g. Shop 2A, 17-19 Foveaux Street
Suburb | String (Max 55 characters) | PUDO store's suburb eg. Surry Hills
State | String (Max 3 characters) | PUDO store's state e.g NSW, QLD, MEL, PER
Postal Code	| String (Max 4 characters) | PUDO store's postal code e.g. 2010
Latitude | Number | PUDO store's latitude e.g. -33.8842435
Longitude | Number | PUDO store's longitude e.g. 151.206471
Mon Opening Hrs | HHHH OR Closed | Monday opening hour e.g. 0800 Opens at 8AM; Closed = Closed all day
Mon Closing Hrs | HHHH OR Closed | Monday closing hour e.g. 2000 Closes at 8PM; Closed = Closed all day
Tue Opening Hrs | HHHH OR Closed | Tuesday opening hour e.g. 0800 Opens at 8AM; Closed = Closed all day
Tue Closing Hrs | HHHH OR Closed | Tuesday closing hour e.g. 2000 Closes at 8PM; Closed = Closed all day
Wed Opening Hrs | HHHH OR Closed | Wednesday opening hour e.g. 0800 Opens at 8AM; Closed = Closed all day
Wed Closing Hrs | HHHH OR Closed | Wednesday closing hour e.g. 2000 Closes at 8PM; Closed = Closed all day
Thu Opening Hrs | HHHH OR Closed | Thursday opening hour e.g. 0800 Opens at 8AM; Closed = Closed all day
Thu Closing Hrs | HHHH OR Closed | Thursday closing hour e.g. 2000 Closes at 8PM; Closed = Closed all day
Fri Opening Hrs | HHHH OR Closed | Friday opening hour e.g. 0800 Opens at 8AM; Closed = Closed all day
Fri Closing Hrs | HHHH OR Closed | Friday closing hour e.g. 2000 Closes at 8PM; Closed = Closed all day
Sat Opening Hrs | HHHH OR Closed | Saturday opening hour e.g. 0800 Opens at 8AM; Closed = Closed all day
Sat Closing Hrs | HHHH OR Closed | Saturday closing hour e.g. 2000 Closes at 8PM; Closed = Closed all day
Sun Opening Hrs | HHHH OR Closed | Sunday opening hour e.g. 0800 Opens at 8AM; Closed = Closed all day
Sun Closing Hrs | HHHH OR Closed | Sunday closing hour e.g. 2000 Closes at 8PM; Closed = Closed all day