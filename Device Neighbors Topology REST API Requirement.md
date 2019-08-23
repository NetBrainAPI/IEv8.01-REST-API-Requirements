
## ***GET*** /V1/CMDB/Topology/Devices/Neighbors
Call this API to get the neighbor device hostname and connection interface names of interface pair from any devices in current working domain if without the input hostname and topotype. 

## Detail Information

> **Title** : Device Neighbors with Topology Type API<br>

> **Version** : 08/22/2019.

> **API Server URL** : http(s)://IP address of NetBrain Web API Server/ServicesAPI/API/V1/CMDB/Topology/Devices/Neighbors

> **Authentication** : 

|**Type**|**In**|**Name**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|Bearer Authentication| Headers | Authentication token | 

## Request body(****required***)

>No request body.

## Query Parameters(****required***)

|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|hostname | list of string  | The devices name, such as ["US-BOS-R1"] or ["US-BOS-R2", "US-BOS-R3", "US-BOS-R4"]<br> ***Note:*** If customer insert the wrong hostname whihc means there is no such device exist in current domain, then return a response: "Device XXXX doesn't exist in current domain" Or if customer insert a device list with 10 devices but only 7 of them exist in domain, then return "Device [XXXX, yyyy, zzzz] doesn't exist in current domain". if customer insert the value with wrong value type: [1111,2222], then return respone: "Device hostname must be insert as String"|
|topoType | list of int  | Return the neighbors in specified topology types<br> 1: L2_Topo_Type, <br>2: L3_Topo_Type, <br>3: Ipv6_L3_Topo_Type, <br>4: VPN_Topo_Type, <br>such as [1] or [2,3,4].<br> ***Note:*** if customer insert the value outside of 1-4 return respones: "Please select the exist topology type."if customer insert the value with wrong value type: ["1", "2"], then return respone: "Topology type must be insert as Integer"|
If customer insert both and there is no corresponding topology interface exist in some devices, then return "Device XXXXX don't have XXXXX interface." If customer only insert one input then only need to consider one filter. e.g. only ["US-BOS-R1"] then return all topology type of this device.|
|skip|integer|The amount of records to be skipped. Devices are sorted by XXXX. Discuss with Jia.<br> ***Note:*** Must be positive integrer, if insert number greater than record number in DB, then response: "Skip number greater than records number."|
|limit|integer|The up limit amount of device records to return per API call. Discuss with Jia.<br> ***Note:*** Must be inserted as positive integer. |
***Note:*** if customer insert both, then return value start from skip till skip + limit. If skip + limit > record count, then return value start from skip till record number with a message: "Not enough record to be retrieved." If customer only insert skip, then return value start from skip till record number. if only insert limit, then return value start from 0 till limit - 1.

## Headers

> **Data Format Headers**

|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
| Content-Type | string  | support "application/json" |
| Accept | string  | support "application/json" |

> **Authorization Headers**

|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
| token | string  | Authentication token, get from login API. |

## Response

|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|hostdevice | list of object | List of object content detail information of neighbor base on this device. |
|hostdevice.interface | string | Interface name of the interface which belongs to hostdevice. |
|hostdevice.connected_device | object | Detail information of neighbor device which connect to the hostdevice |
|hostdevice.connected_device.nbr_device| string | The host name of neighbor device which connect to the hostdevice.|
|hostdevice.connected_device.inteface_name| string | The interface name belongs to neighbor device which connect with the hostdevice interface.|
|hostdevice.topology | string | The topology name which this neighbor device belongs to, such as "L2_Topo_Type", "L3_Topo_Type", "Ipv6_L3_Topo_Type" or "VPN_Topo_Type". |
|statusCode| integer | The returned status code of executing the API.  |
|statusDescription| string | The explanation of the status code.  |

> ***Example***



```python
response = {
    "hostdevice1" : [
        {
            "interface" : "interface_name1",
            "connected_device" : {
                "nbr_device" : "device name1",
                "nbr_intf" : "inteface_name1"
            },
            "topology" : "L2"
        },
        {
            "interface" : "interface_name1",
            "connected_device" : {
                "nbr_device" : "device name1",
                "nbr_intf" : "inteface_name1"
            },
            "topology" : "L2"
        },
        {
            "interface" : "interface_name1",
            "connected_device" : {
                "nbr_device" : "device name1",
                "nbr_intf" : "inteface_name1"
            },
            "topology" : "L2"
        },
        .
        .
        .
    ],
    "hostdevice2" : [
        {
            "interface" : "interface_name1",
            "connected_device" : {
                "nbr_device" : "device name1",
                "nbr_intf" : "inteface_name1"
            },
            "topology" : "L2"
        },
        {
            "interface" : "interface_name1",
            "connected_device" : {
                "nbr_device" : "device name1",
                "nbr_intf" : "inteface_name1"
            },
            "topology" : "L2"
        },
        .
        .
        .
    ]
    .
    .
    .
    "statusCode": 790200,
    "statusDescription": "Success."
    
}
```
