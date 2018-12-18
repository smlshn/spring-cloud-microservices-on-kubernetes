# project instruction
## Introduction

I often have friends from the Internet circle to ask questions about developing microservice applications with ``Spring Cloud``, and how to deploy ``Spring Cloud`` microservices on ``Kubernetes``, how to address them. The problem, therefore, has sprouted the idea of ​​refining the best practices I have accumulated in several Internet projects and k8s deployment microservices. This project is a mini microservices reference that condenses Spring Cloud Microservices on Kubernetes best practices for sharing best practices, technical demonstrations:

1. A fast-replicating framework for developing microservice applications using the ``Spring Cloud`` family bucket;
2. A set of quick-to-copy deployment scenarios for deploying ``Spring Cloud`` microservices to ``Kubernetes`` clusters and YAML deployment scripts (in production project applications, these YAML scripts should be deployed to the company's CI) & CD pipeline to automate CI & CD).


## Technology Stack

1. ``Kubernetes`` and ``Rancher Server``: as a PaaS platform for automated deployment, scaling and orchestration of microservice container clusters.

2. ``Spring Cloud`` family bucket: used to develop microservice applications. With the most stable ``Edgware RELEASE`` currently available, the best match for the whole family of bucket components is:
    - Development: ``Zuul`` as API Gateway, ``Eureka`` as service registration management center, ``Spring Cloud Config`` as configuration center, ``Hystrix`` as fuse, downgrade and current limit, `` Ribbon`` as load balancing, ``Feign`` as a declarative REST Client.
    - Monitoring: ``Spring Boot Actuator`````Spring Boor Admin`` provides introspection and monitoring capabilities for each microservice, as well as visual monitoring UI, ``Hystrix Dashboard`` for visual monitoring of Hystrix Metrics, ``Turbine ``Hystrix Metrics, ``Sleuth`` and ``Zipkin`` for aggregating individual microservices for distributed call tracking.

3. ``Swagger`` & ``Swagger UI``: for REST API documentation and for REST API debugging.

4. ``MyBatis``: for the ``MySQL`` database to access the ORM.

5. ``RabbitMQ``: for reliable messaging service middleware.

6. ``Flyway``: Inheriting ``DevOps`` ``all code`` ideas, using ``Flyway`` for database upgrade configuration management.

7. Third library: ``lombok``, ``guava``


## Online Demo

In order to facilitate the "painless" experience of the friends ``Spring Cloud`` microservices and ``Kubernetes`` cluster deployment, I purchased 3 virtual machines on the Azure cloud and built a ``Kubernetes`` cluster. The mini microservice demo project and the underlying dependencies are deployed to the cluster. Open a read only ``Kubernetes`` cluster experience account for everyone to experience.

``Kubernetes`` cluster experience account:
- Web Console URL: master1.k8s.kyletiger.com
- Login Name: spring
- Password: spring123456

The configuration of the three virtual machines is as follows:
1. Kubernetes Master node: 1 set, configured as standard F2s_v2 (2 vcpu, 4 GB memory);
2. Kubernetes Worker node: 2 units, configured as standard F1s (1 vcpu, 2 GB memory).

Please: Do not traffic to attack this cluster or vulnerability scan! This system is purely for communication and convenience for users, a small cost deployment environment, no need to buy high-performance virtual machines and WAF gateways, anti-DDoS and other security protection services.


### Service Dictionary

note:
1. All the services exposed in the Kubernetes cluster that can be accessed by the external network are purely for the convenience of users to use the off-the-shelf basic services (for example, the Eureka Service Registration Center) when learning spring cloud, especially to use ``type: NodePort`. `Deployment mode additionally exposes the service to the external network IP;
2. For the actual project, you need to use the ``type: LoadBalancer`` deployment plan to expose the service, only open to the intranet access!

Services | Intranet Service Addressing | Intranet Service Port | External Network DNS Addressing | External Network Service Port
---|---|---|---|---
Service Registry HA-0 | eureka-0.discovery.default.svc.cluster.local | 8761 | master1.k8s.kyletiger.com | 38761
Service Registry HA-1 | eureka-1.discovery.default.svc.cluster.local | 8761 | node1.k8s.kyletiger.com | 38762
Configuration Center HA | Query Addressing by Service Consumer to Eureka Server | 8888 | node1.k8s.kyletiger.com | 38888
API Gateway Zuul HA | Query addressing by service consumer to service registry | 8080 | master1.k8s.kyletiger.com | 38080
Hystrix Dashboard | Query by User Service to Service Registry | 9000 | node2.k8s.kyletiger.com | 39000
Hystrix Turbine | Query by the service consumer to the service registry | 9100 | master1.k8s.kyletiger.com | 39100
Spring Boot Admin | Querying Addressing by Service Consumer to Service Registry | 9090 | node2.k8s.kyletiger.com | 39090
Employee microservice | Query addressing by service consumer to service registry | 8000 | node2.k8s.kyletiger.com | 38000
Department Micro Service | Query Mode by Service Consumer to Service Registry | 8100 | node2.k8s.kyletiger.com | 38100


- Kubernetes cluster management:

![](images/2018-12-13-02-50-08.png)

![](images/2018-12-16-21-28-39.png)

![](images/2018-12-16-21-29-59.png)

![](images/2018-12-13-03-07-37.png)

- Microservice Registration Center:

![](images/2018-12-14-01-45-04.png)

- Spring Boot Admin:

![](images/2018-12-16-21-38-21.png)
![](images/2018-12-16-21-37-52.png)

- Hystrix Dashboard:

![](images/2018-12-14-01-49-17.png)

- Swagger:

![](images/2018-12-14-01-50-37.png)


## Update plan

1. Use ``RabbitMQ`` to let each microservice instance asynchronously spit out Hystrix Metrics and ``Turbine`` for asynchronous collection aggregation.
2. Deploy ``Zipkin`` and ``Elasticsearch``: Estimate the configuration of the virtual machine needs to be upgraded to run: (.
3. Demonstration of ``TCC`` flexible distributed transaction processing: Several microservices demonstrating ``TCC`` need to be developed.
4. ``Spring Data JPA Repositories``+``Hibernate``: Demonstrate another database ORM solution.
5. Documentation: Add & improve documentation.


## Conclusion
Thank you for your patience. If you have any questions about Spring Cloud & Kubernetes in this project or have a better idea or suggestion about your coding style, please feel free to email <lisong.zheng@gmail.com> or QQ <40000646@qq .com> Thanks to me for getting in touch.
