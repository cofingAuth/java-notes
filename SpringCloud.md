#### 1. 什么是微服务架构

> 微服务是系统架构上的一种设计风格，它的主旨是将一个原本独立的系统拆分成多个小型服务，这些小型服务都在各自独立的进程中运行，服务之间通过基于HTTP的RESTful API进行通信协议

#### 2. 什么是 Spring Cloud

> Spring Cloud是一个基于Spring Boot实现的微服务架构开发工具。它为微服务架构中涉及的配置管理、服务治理、断路器、智能路由、微代理、控制总线、全局锁、决策竞选、分布式会话和集群状态管理等操作提供了一种简单的开发方式

#### 3. Spring Cloud的核心组件

> Eureka 服务注册与发现
>
> Feign 基于动态代理机制，根据注解和选择的机器，拼接请求url地址，发起请求
>
> Ribbon 实现负载均衡，从一个服务的多台机器中选择一台
>
> Hystrix 提供线程池，不同的服务走不同的线程池，实现了不同服务调用的隔离，避免了服务雪崩的问题
>
> Zuul 网关管理，由Zuul网关转发请求给对应的服务

#### 4. 服务治理 Spring Cloud Eureka

##### 4.1 服务治理的作用，为什么微服务需要服务它

> 作用：服务治理主要用来实现各个微服务实例的自动化注册与发现
>
> 为什么：当微服务应用不断增加时候，如果使用静态配置就会越来越难以维护，如集群规模、服务的位置、服务的命名等发现变化，所以出现了服务治理的框架和产品进行解决服务实例维护问题。

##### 4.2 Eureka服务治理基础机构的三个核心要素

> **服务注册中心**：Eureka提供服务端，提供服务注册与发现的功能
>
> **服务提供者**：提供服务的应用，将自己提供的服务注册到Eureka，以供其它应用发现
>
> **服务消费者**：消费者应用从服务注册中心获取服务列表，而实现服务消费有Ribbon或Feign

#### 5. 客户端负载均衡 Spring Cloud Ribbon

> Spring Cloud Ribbon是一个基于HTTP和TCP的客户端负载均衡工具，它基于Netflix Ribbon实现；
>
> Ribbon客户端组件提供一系列完善的负载均衡策略（随机选择、线性轮询、权重的线性轮询等）、重试机制；Spring Cloud Ribbon提供自动化配置，简化开发，也可以另外参数配置进行覆盖默认配置

#### 6. 服务容错保护 Spring Cloud Hystrix

> Spring Cloud Hystrix实现了断路器、线程隔离等一系列服务保护功能。
>
> Hystrix具备服务降级、服务熔断、线程和信号隔离、请求缓存、请求合并以及服务监控等强大功能

#### 7. 声明式服务调用 Spring Cloud Feign

> 它基于Netflix Feign实现，整合了Spring Cloud Ribbon与Spring Cloud Hystrix，还提供声明式的Web服务客户端定义

#### 8. API网关服务 Spring Cloud Zuul

> 网关作用：统一入口、鉴权校验、动态路由、减少客户端和服务端的耦合
>
> 过滤器类型：pre、routing、post、error
>
> 请求生命周期：Http Request → pre filters → routing filters → (error filters) → post filters → Http Response

#### 9. 分布式配置中心 Spring Cloud Config

> 