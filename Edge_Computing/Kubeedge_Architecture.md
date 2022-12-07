# Kubeedge架构
---
## 云端
### CloudHub
CloudHub是一个websocket服务器，负责监控云边的变化、缓存以及向EdgeHub发送消息message
> a web socket server responsible for watching changes at the cloud side, caching and sending messages to EdgeHub
### EdgeController
EdgeController 是一个扩展的K8S控制器：负责管理**边缘节点**和**pods**的**元数据**，因此数据才能被发送到指定的边缘节点
> an extended kubernetes controller which manages edge nodes and pods metadata so that the data can be targeted to a specific edge node.

### DeviceController
DeviceController 也是一个扩展性的K8S控制器，负责管理devices, 实现**device元数据/状态数据**在云端与边缘端的同步
>  an extended kubernetes controller which manages devices so that the device metadata/status data can be synced between edge and cloud

## 边缘端
### EdgeHub
EdgeHub 是一个websocket 客户端，负责与云边服务交互实现边缘计算*类似KubeEdge 架构中的Edge Controller*。其中包括将**云边资源同步**更新到边缘端以及将**边端主机、设备状态变化**广播至云端。
> a web socket client responsible for interacting with Cloud Service for the edge computing (like Edge Controller as in the KubeEdge Architecture). This includes syncing cloud-side resource updates to the edge, and reporting edge-side host and device status changes to the cloud. 

### Edged
Edged 是一个运行在边缘节点**管理容器化应用**的代理
>  an agent that runs on edge nodes and manages containerized applications.
### EventBus
EventBus 是**一个MQTT客户端**，负责与MQTT服务器mosquitto的交互，为其他组件提供发布与订阅功能
> a MQTT client to interact with MQTT servers (mosquitto), offering publish and subscribe capabilities to other components.
### DeviceTwin
DeviceTwin 负责存储**设备状态**，并将设备状态同步到云端，同时也提供了了应用的查询接口。
>  responsible for storing device status and syncing device status to the cloud. It also provides query interfaces for applications.
### MetaManager
MetaManager 是edged与edgehub之间的message 处理器，同时，也负责将元数据存储/查询 到/从 一个轻量级数据库SQLite。

[设备管理框架](https://www.cnblogs.com/huaweiyun/p/16625188.html)
