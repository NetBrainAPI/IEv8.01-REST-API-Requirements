
# Devices REST API Design

## GET /V1/CMDB/Devices

This API is used to get devices and their attributes data in batch. The response of this API will return a list in JSON format.

## Detail Information

>**Title:** Devices API

>**Version:** 08/22/2019

>**API Server URL:** http(s)://IP Address of NetBrain Web API Server/ServicesAPI/API/V1/CMDB/Devices

>**Authentication:**

|**Type**|**In**|**Name**|
|------|------|------|
|Bearer Authentication|Headers|Authentication token|

## Request body (*required)

>No request body.

## Query Parameters (*required)

|**Name**|**Type**|**Description**|
|------|------|------|
|hostname|string OR list of string|A list of device hostnames|
|ip|string OR list of string|A list of device management IPs|
|||If provided both of hostname and ip, hostname had higher priority. If any of the devices are not found from the provided query parameter, return the found devices as a list in response and add another json key "deviceNotFound", the value is a mixed list of hostnames and IPs that are not found.|
|fullattr|boolean|Default is false.<br>False: return basic device attributes (device id, management IP, hostname, device type, first discover time, last discover time).<br>True: return all device attributes, including customized attributes|
|skip|integer|The amount of records to be skipped. Devices are sorted by hostnames. The value should be a positive number.  If the value is negative, return error "Please provide a positive value." If the value provided is greater than the total amount of device records, return error message "Skip value is greater than total amount of device records in DB."|
|limit|integer|The up limit amount of device records to return per API call. The value should be a positive number. If the value is negative, return error "Please provide a positive value." If the value is greater than the total amount of device records, return full device list.|
|||If only provide skip value, return the rest of the full device list. If only provide limit value, return from the first device in DB. If provided both skip and limit, return as required. Error exceptions follow each parameter's description.|
|filter|json|If specified, return the matched device list with device attributes. Supported filtering attributes: name, assetTag, contact, descr, layer, loc, mgmtIP, model, site, sn, vendor, ver, mpls_asNum, modules.fwrev, modules.hwrev, modules.ports, modules.sn, modules.swrev, modules.type, mpls_accessPnt.ip, mpls_accessPnt.ceNbrs.ceIntfIP, mpls_accessPnt.ceNbrs.ceIntfName, mpls_accessPnt.ceNbrs.ceName, nbPathSchema, nbPathValue|
|||If provide invalid data format, return error "invalid filter input".|

## Headers

>**Data Format Headers**

|**Name**|**Type**|**Description**|
|------|------|------|
|Content-Type|string|support "application/json"|
|Accept|string|support "application/json"|

>**Authorization Headers**

|**Name**|**Type**|**Description**|
|------|------|------|
|token|string|Authentication token, get from login API.|

## Response

|**Name**|**Type**|**Description**|
|------|------|------|
|statusCode|integer|Code issued by NetBrain server indicating the execution result.|
|statusDescription|string|The explanation of the status code.|
|devices|string[]|A list of devices.|
|devices.devicesID|string|The device ID.|
|devices.deviceTypeName|string|The type of the returned device, such as Cisco Router.|
|devices.mgmtIP|string|The management IP address of the returned device.|
|devices.hostname|string|The hostname of returned device.|
|devices.customAttribute1|Refer to GDR data type|Customized Attribute 1.|
|devices.customAttribute2|Refer to GDR data type|Customized Attribute 2.|
|...|...|...|

>***Example***


```python
{
    "devices": [
        {
            "id": "ad53a0f6-644a-400b-9216-8df746baed3b",
            "name": "R20",
            "mgmtIP": "123.20.20.20",
            "mgmtIntf": "Loopback0",
            "subTypeName": "Cisco Router",
            "vendor": "Cisco",
            "model": "DEVELOPMENT TEST SOFTWARE",
            "ver": "15.4(2)T4",
            "sn": "71372834",
            "site": "My Network\\Unassigned",
            "loc": "",
            "contact": "",
            "mem": "356640420",
            "assetTag": "",
            "layer": "",
            "descr": "",
            "oid": "1.3.6.1.4.1.9.1.1",
            "driverName": "Cisco Router",
            "fDiscoveryTime": {
                "$date": 1547572719000
            },
            "lDiscoveryTime": {
                "$date": 1547572719000
            },
            "assignTags": "",
            "hasBGPConfig": true,
            "hasOSPFConfig": false,
            "hasEIGRPConfig": false,
            "hasISISConfig": false,
            "hasMulticastConfig": false,
            "customAttribute1": "string",
            "customAttribute2": true
        },
        {
            "id": "ad53a0f6-644a-400b-9216-8df746baed3c",
            "name": "R21",
            "mgmtIP": "123.20.20.21",
            "mgmtIntf": "Loopback0",
            "subTypeName": "Cisco Router",
            "vendor": "Cisco",
            "model": "DEVELOPMENT TEST SOFTWARE",
            "ver": "15.4(2)T4",
            "sn": "71372835",
            "site": "My Network\\Unassigned",
            "loc": "",
            "contact": "",
            "mem": "356640420",
            "assetTag": "",
            "layer": "",
            "descr": "",
            "oid": "1.3.6.1.4.1.9.1.1",
            "driverName": "Cisco Router",
            "fDiscoveryTime": {
                "$date": 1547572719000
            },
            "lDiscoveryTime": {
                "$date": 1547572719000
            },
            "assignTags": "",
            "hasBGPConfig": true,
            "hasOSPFConfig": false,
            "hasEIGRPConfig": false,
            "hasISISConfig": false,
            "hasMulticastConfig": false,
            "customAttribute1": "string",
            "customAttribute2": true
        }
    ],
    "statusCode": 790200,
    "statusDescription": "Success."
}
```
