
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
|skip|integer|The amount of records to be skipped. Devices are sorted by hostnames. The value must not be negative.  If the value is negative, API throws exception {"statusCode":791001,"statusDescription":"Parameter 'skip' cannot be negative"}. No upper bound for this parameter.|
|limit|integer|The up limit amount of device records to return per API call. Devices are sorted by hostnames. The value must not be negative.  If the value is negative, API throws exception {"statusCode":791001,"statusDescription":"Parameter 'limit' cannot be negative"}. No upper bound for this parameter. If the parameter is not specified in API call, it means there is not limitation setting on the call.|
|||If only provide skip value, return the rest of the full device list. If only provide limit value, return from the first device in DB. If provided both skip and limit, return as required. Error exceptions follow each parameter's description.|
|||**Note:** The skip and limit parameters are based on device, not topology result record.|

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
{
    "topology":
    [
        {   
            "hostname": "device_1",
            "neighbors":
            [
                {
                    "interface": "intf_1",
                    "connected_device":
                    {
                        "nbr_device": "device_2",
                        "nbr_intf": "intf_3"
                    },
                    "topology": "L2"
                }
				{
                    "interface": "intf_2",
                    "connected_device":
                    {
                        "nbr_device": "device_3",
                        "nbr_intf": "intf_4"
                    },
                    "topology": "L2"
                }
            ]
        }
		{   
            "hostname": "device_4",
            "neighbors":
            [
                {
                    "interface": "intf_5",
                    "connected_device":
                    {
                        "nbr_device": "device_5",
                        "nbr_intf": "intf_6"
                    },
                    "topology": "L3"
                }
        }
    ]
}
```
