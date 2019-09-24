# spec-event-data-server
REST API For accessing Cameo Event Data generated by SPEC Framework.
Details can be found here at UT Dallas Event Data [website](http://eventdata.utdallas.edu/data.html).

## How to access:
The webserver is running on a machine hosted in Big Data Management and Analytics Lab @ UTD. So currently it is accessible from within the UTD network. You can use VPN if you are away. Instructions for that can be found [here](http://www.utdallas.edu/oit/vpn/) 

To access the data use the follwoing url format.

`http://eventdata.utdallas.edu/api/data?api_key=<api-key-here>&query=<query-for-data>&select=<comma-separated-list-of-attributes>&unique=<name-of-attribute>`

The `select` and `unique` parts are optional. `select` is used when you need only specific attributes for a record.`unique` is used for selecting distinct values for a particular attribute.

To get the API Key, please visit the url for [signup](http://eventdata.utdallas.edu/signup)

## What you get:

The server returns the resulted data and status of operation (i.e. "success" or "error") in JSON format. The structure is as follows
```
{
     "status": <success-or-error>, 
     "data": [ ... event data array ...]
}
```
Each entry of the `data` array has the following structure (when nothing specified using `select`)
```
{
	"code": "",
	"target": "",
	"headline": "",
	"country_code": "",
	"coordinates": [X, Y],
	"content": "",
	"source": "",
	"link": "",
	"location": "",
	"date": {
		"$date": 
	},
	"_id": {
		"$oid": ""
	}
}
```
If you use select like below

`http://...&select=source,target,code`

Then the structure will be as follows

```
{
	"code": "",
	"target": "",
	"source": "",
	"_id": {
		"$oid": ""
	}
}
```

If unique is used like the following

`http://...&unique=source`

The ourput will be a JSON-Array like the following.
```
{"status": "success", "data": ["AFGCOPGOV", "JPN", "SAUGOV", "GBR", "USRTKMCOPGOVENV", "IND", "PRTBUS", "ISRGOV", "RUS", "AUT", "CHN", "PRKGOV", "KWT", "IGOUNO", "JOR", "PSEREB", "USACVL", "ITA", "CHNGOV", "FRA", .......]
```

The intial database contains 2368 cameo event records.

## How to query for that data:

We are using Mongo DB query syntax. The query is expressed in JSON Format. Here is some examples - 

Find all the events with cameo code 033

`{"code": "033"}`

Find all the events with camoe code 030 and has "Seoul" as location

`{"code":"030", "location":"Seoul"}`

Find all the events that has "United States of America" as source or target.

`{"$or":[{"source":"United States of America"}, {"target":"United States of America"}]}`

Find all the events that have occurred after June 15, 2016

`{"date" : { "$gt" : "$date(06-15-2016)" }}`

You need to specify the date using the `$date()` directive. The date format is now flexible, but in future it may change. 

To find out more, follow the documentation [here](https://docs.mongodb.com/getting-started/python/query/)

# Version 2: Working with Phoenix Dataset (Larger Dataset)

## How to access:

Similar as above, use the follwoing kind of url (port number is 5002, instead of 5000)
`http://10.176.148.60:5002/api/data?api_key=<api-key-here>&query=<query-for-data>&select=<comma-separated-list-of-attributes>&unique=<name-of-attribute>`

## Query Format:
Similar to above, except for dates. Dates are represented as String in `YYYYMMDD` format. So to query for events which happened on June 23, 2015, use the following query,

`{'date8':'20150623'}`

## Attributes contained in each record:
The following is the representation for the structure each record has - 
```
 {'event_id': , 'date8': , 'year': , 'month': , 'day': ,
'source': , 'src_actor': , 'src_agent': , 'src_other_agent': ,
'target': , 'tgt_actor': , 'tgt_agent': , 'tgt_other_agent': ,
'code': , 'root_code': , 'quad_class': , 'goldstein': ,
'geoname': , 'country_code': , 'admin_info': , 'id': ,  'url': ,
'source_text': , 'longitude': , 'latitude': }
```

Selection of specific attributes works as before.







