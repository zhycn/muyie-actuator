# Setting up Spring Boot Admin Server

本文基本 Spring Boot 来快速搭建 Admin Server。首先，在 Spring Boot 项目中添加 Maven 依赖：

```
<!-- Spring Cloud Netflix Eureka 注册中心客户端 -->
<!-- 基于 Spring Cloud 注册中心的好处是：在该体系架构中，Admin Clients 可以实现零配置 -->
<dependency>
  <groupId>org.springframework.cloud</groupId>
  <artifactId>spring-cloud-dependencies</artifactId>
  <version>${spring-cloud.version}</version>
  <type>pom</type>
  <scope>import</scope>
</dependency>
<dependency>
	<groupId>org.springframework.cloud</groupId>
	<artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>

<!-- Admin Server -->
<dependency>
  <groupId>de.codecentric</groupId>
  <artifactId>spring-boot-admin-starter-server</artifactId>
  <version>2.2.2</version>
</dependency>
<dependency>
  <groupId>de.codecentric</groupId>
  <artifactId>spring-boot-admin-server-ui</artifactId>
  <version>2.2.2</version>
</dependency>
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

然后，在启动类上添加 @EnableAdminServer 注解类：

```
@SpringBootApplication
@EnableDiscoveryClient
@EnableAdminServer
public class SpringBootAdminApplication {

    public static void main(String[] args) {
        SpringApplication.run(SpringBootAdminApplication.class, args);
    }

}
```

接下来，添加 Spring Cloud Netflix Eureka 的公共配置参数：

```
# bootstrap.yml
eureka:
  instance:
    prefer-ip-address: true
    lease-renewal-interval-in-seconds: 10
    lease-expiration-duration-in-seconds: 30
    metadataMap:
      instanceId: ${spring.cloud.client.ipAddress}:${server.port}
```

再配置注册中心的服务器地址：

> 支持单机模式、集群模式和多节点高可用模式，配置参考：[Service Discovery: Eureka Clients](https://github.com/zhycn/muyie-registry/blob/feature/docs/Eureka-Client.md)

```
# application.yml
# 示例为多节点高可用模式
eureka:
  client:
    region: cn-shanghai-1
    serviceUrl:
      zone1: http://zone1:8761/eureka/
      zone2: http://zone2:8761/eureka/
    availability-zones:
      cn-shanghai-1: zone1,zone2
```