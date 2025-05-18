Project: Closed Loop Automation using AIML
===
# Author: Vijay Chaganti
*Data Source: <URL>*

*Python Code: <URL>*

# Simple ModifyRequest to add NH, NHG and Prefix in AFT
*ModifyRequest in JSON format*
---
```
{
  "modifyRequest": [
    {
      "operation": {
        "networkInstance": "default",
        "aft": "NEXT_HOP",
        "op": "ADD",
        "nh": {
          "index": 100,
          "ipAddress": "192.0.2.1"
        }
      }
    },
    {
      "operation": {
        "networkInstance": "default",
        "aft": "NEXT_HOP_GROUP",
        "op": "ADD",
        "nhg": {
          "id": 200,
          "nextHops": [
            {
              "index": 100,
              "weight": 1
            }
          ]
        }
      }
    },
    {
      "operation": {
        "networkInstance": "default",
        "aft": "IPV4",
        "op": "ADD",
        "ipv4": {
          "prefix": "198.51.100.0/24",
          "nextHopGroup": 200
        }
      }
    }
  ]
}
```

*ModifyRequest in textproto format*
---
```
updates: {
  network_instance: "default"
  operation: ADD
  entry {
    next_hop {
      index: 100
      ip_address: "192.0.2.1"
    }
  }
}
updates: {
  network_instance: "default"
  operation: ADD
  entry {
    next_hop_group {
      id: 200
      next_hops: {
        index: 100
        weight: 1
      }
    }
  }
}
updates: {
  network_instance: "default"
  operation: ADD
  entry {
    ipv4 {
      prefix: "198.51.100.0/24"
      next_hop_group: 200
    }
  }
}
```

*ModifyRequest in yaml format*
---
*Entries must be submitted in order: NextHop → NextHopGroup → IPv4.*
```
modifyRequest:
  - operation:
      networkInstance: "default"
      op: ADD
      aft: NEXT_HOP
      nextHop:
        index: 100
        ipAddress: "192.0.2.1"

  - operation:
      networkInstance: "default"
      op: ADD
      aft: NEXT_HOP_GROUP
      nextHopGroup:
        id: 200
        nextHops:
          - index: 100
            weight: 1

  - operation:
      networkInstance: "default"
      op: ADD
      aft: IPV4
      ipv4:
        prefix: "198.51.100.0/24"
        nextHopGroup: 200
```

*In gRIBI, the only RPC for modifying the routing table is Modify. This one RPC will do multiple operations as below*
<img width="984" src="https://github.com/user-attachments/assets/30b1902b-0438-4f3d-9406-14297946d0f0" />

*Example on how to invoke RPC call using gribic with yaml file*
---
*gribic modify --input-file modifyRequest.yaml -a <server-ip>:<server-port> -u <server-username> -p <server-password> --election-id <controller-electionid>*

*Enabling server to accept GRPC connections from client on port 9340 on Juniper Router*
```
#set system services 
http {
    servers {
        server gribi {
            port 9340;
            grpc gribi;
        }
```
