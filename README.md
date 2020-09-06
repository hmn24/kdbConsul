# kdbConsul
Experiment with consul as a discovery for kdb

## Relevant Commands:

Start a consul-agent in dev mode as per 
https://learn.hashicorp.com/tutorials/consul/get-started-service-discovery

Config files can be found from provided consul.d directory

```
consul agent -dev -config-dir=./consul.d -enable-local-script-checks -node=machine

or

consul agent -dev -config-dir=./consul.d -node=machine
```

If one starts a kdb on port 5050, it would automatically check for the process on port 5050. For multiple checks or other definitions, one can look at

https://www.consul.io/docs/agent/checks
https://www.consul.io/api-docs/agent/service
https://www.consul.io/docs/commands
https://learn.hashicorp.com/tutorials/consul/service-registration-health-checks
https://medium.com/velotio-perspectives/a-practical-guide-to-hashicorp-consul-part-1-5ee778a7fcf4
https://howtodoinjava.com/spring-cloud/consul-service-registration-discovery/
https://www.consul.io/api-docs/agent/service

From within kdb, one can use HTTP GET request to check for healthy services. Examples are:

```
q)flip .j.k .Q.hg "http://localhost:8500/v1/catalog/service/kdb"
ID                      | "38661b0d-b596-03c0-c2a8-554cb6371c88"                                      
Node                    | "machine"                                                                   
Address                 | "127.0.0.1"                                                                 
Datacenter              | "dc1"                                                                       
TaggedAddresses         | `lan`lan_ipv4`wan`wan_ipv4!("127.0.0.1";"127.0.0.1";"127.0.0.1";"127.0.0.1")
NodeMeta                | (,`consul-network-segment)!,""                                              
ServiceKind             | ""                                                                          
ServiceID               | "kdb"                                                                       
ServiceName             | "kdb"                                                                       
ServiceTags             | "kdb1"                                                                      
ServiceAddress          | ""                                                                          
ServiceWeights          | `Passing`Warning!1 1f                                                       
ServiceMeta             | (`symbol$())!()                                                             
ServicePort             | 5050                                                                        
ServiceEnableTagOverride| 0                                                                           
ServiceProxy            | `MeshGateway`Expose!((`symbol$())!();(`symbol$())!())                       
ServiceConnect          | (`symbol$())!()                                                             
CreateIndex             | 13                                                                          
ModifyIndex             | 13
```

The UI for consul-server can be accessed from 
http://localhost:8500/ui/
