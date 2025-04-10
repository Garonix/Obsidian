![1739502069441.png](https://img.zeges.top/OeXRVK.png)

**图中主要分为两个部分：Master (控制平面) 和 Node Pool (节点池)。**

**Master (控制平面):** 这是 Kubernetes 集群的大脑，负责集群的管理和控制。

- **etcd (key-value DB, SSOT):** 图中指出 etcd 是一个键值数据库，并且是 SSOT (Single Source of Truth，单一数据源)。 这是 **完全正确的**。 etcd 存储了 Kubernetes 集群的所有配置数据、状态数据和元数据。它是整个集群的基石，保证数据的一致性和可靠性。
    
- **API Server (REST API):** 图中指出 API Server 提供 REST API。 这也是 **完全正确的**。 API Server 是 Kubernetes 控制平面的前端，它暴露了 Kubernetes API。所有与 Kubernetes 集群的交互，无论是用户、kubectl 命令行工具，还是集群内部的组件，都必须通过 API Server。它负责接收请求、验证请求，并将请求转发给其他组件处理。
    
- **Controller Manager (Controller Loops):** 图中指出 Controller Manager 运行 “Controller Loops” (控制器循环)。 这也是 **正确的**。 Controller Manager 实际上是由多个控制器 (如 Node Controller, Replication Controller, Endpoint Controller 等) 组成的。每个控制器都负责监听 Kubernetes 集群的状态，并根据预期的状态执行相应的操作，以确保集群运行在期望的状态。例如，Node Controller 负责监控节点的状态，当节点出现故障时，会执行相应的处理。
    
- **Scheduler (Bind Pod to Node):** 图中指出 Scheduler 负责 “Bind Pod to Node” (将 Pod 绑定到节点)。 这是 **完全正确的**。 Scheduler 的作用是为新创建的 Pod 选择最合适的节点来运行。它会考虑节点的资源情况、Pod 的资源需求、亲和性、反亲和性等策略，最终将 Pod 分配到最佳节点上。
    

**Node Pool (节点池):** 这是 Kubernetes 集群中真正运行工作负载的地方，即运行容器化应用的地方。 图中展示了 Node 1, Node 2, Node 3 三个节点作为例子。

- **Kubelet:** 图中每个 Node 上都有 Kubelet 组件。 这是 **完全正确的**。 Kubelet 是运行在每个节点上的代理，负责节点与 Master 的通信。它接收来自 Master (实际上是 API Server) 的指令，如创建、删除、更新 Pod 等，并实际执行这些操作。Kubelet 还会定期向 Master 汇报节点的状态。
    
- **Container Runtime:** 图中每个 Node 上都有 Container Runtime 组件。 这也是 **完全正确的**。 Container Runtime 是实际运行容器的组件。 常见的 Container Runtime 包括 Docker, containerd, CRI-O 等。 Kubernetes 通过 Container Runtime Interface (CRI) 与 Container Runtime 进行交互，从而可以支持不同的容器运行时。
    
- **OS (操作系统) 和 Hardware (硬件):** 图中每个 Node 的底层是 OS 和 Hardware。 这是 **基础且正确的**。 Kubernetes 节点必须运行在操作系统之上，并依赖于底层的硬件资源。
    

**Legend (图例):** 图例部分解释了图中箭头代表的不同技术或标准。

- **CNI (Container Network Interface):** 图中黄色箭头标记为 CNI。 CNI 是 Kubernetes 中用于配置容器网络接口的标准接口。 它定义了容器运行时如何配置容器的网络，例如分配 IP 地址、配置路由等。 Kubernetes 使用 CNI 插件来实现各种网络方案。
    
- **CRI (Container Runtime Interface):** 图中橙色箭头标记为 CRI。 CRI 是 Kubernetes 中用于与容器运行时交互的标准接口。 它允许 Kubernetes 支持不同的容器运行时，而无需修改 Kubernetes 的核心代码。 通过 CRI，Kubernetes 可以与 Docker, containerd, CRI-O 等不同的容器运行时进行集成。
    
- **OCI (Open Container Initiative):** 图中绿色箭头标记为 OCI。 OCI 是开放容器标准组织，定义了容器镜像和运行时的标准。 Kubernetes 使用的容器镜像和容器运行时都遵循 OCI 标准。 OCI 确保了容器技术的可移植性和互操作性。
    
- **Protobuf:** 图中深蓝色箭头标记为 Protobuf。 Protobuf 是一种高效的、结构化的数据序列化协议，常用于 Kubernetes 内部组件之间的通信。 由于其高效性和结构化特性，Protobuf 可以提高 Kubernetes 内部通信的性能和可靠性。
    
- **gRPC:** 图中浅蓝色箭头标记为 gRPC。 gRPC 是一个高性能、开源的远程过程调用 (RPC) 框架。 Kubernetes 的一些组件之间也使用 gRPC 进行通信，特别是 API Server 和 Kubelet 之间的通信，以及 Controller Manager 与 API Server 的通信。 gRPC 基于 Protobuf 进行数据序列化，进一步提升了通信效率。
    
- **JSON:** 图中红色箭头标记为 JSON。 JSON 是一种轻量级的数据交换格式，常用于 REST API 的数据传输。 Kubernetes API Server 对外提供的 REST API 通常使用 JSON 格式进行数据交换，方便用户和外部系统与 Kubernetes 集群进行交互。
    

