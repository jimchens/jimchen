---

title: Spring Cloud (3) - Eureka 注册中心
date: 2018-11-28 17:32:09

tags: 
  - Spring Cloud
  - Eureka
  - 注册中心
categories:
  - SpringCloud
  
---

## 概述：
&ensp;&ensp;官方解释：Eureka is a REST (Representational State Transfer) based service that is primarily used in the AWS cloud for locating services for the purpose of load balancing and failover of middle-tier servers.

&ensp;&ensp;Eureka是一个基于REST的服务，主要用于定位运行在AWS域中的中间层服务，以达到负载均衡和中间层服务故障转移的目的。

&ensp;&ensp;Eureka是Netflix开发的服务发现框架，SpringCloud将它集成在其子项目spring-cloud-netflix中，以实现SpringCloud的服务发现功能，功能类似于Zookeeper。

## 1. 搭建Eureka服务注册中心
  
####  1.1 maven配置pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>com.jim</groupId>
  <artifactId>eureka</artifactId>
  <version>1.0-SNAPSHOT</version>
  <packaging>jar</packaging>

  <name>eureka</name>

  <dependencyManagement>
    <dependencies>
      <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-dependencies</artifactId>
        <version>Finchley.RELEASE</version>
        <type>pom</type>
        <scope>import</scope>
      </dependency>
    </dependencies>
  </dependencyManagement>

  <dependencies>
    <dependency>
      <groupId>org.springframework.cloud</groupId>
      <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
    </dependency>
  </dependencies>
</project>
```

spring-cloud-starter-netflix-eureka-server中已经包含了spring boot包，无需再添加spring boot依赖了

#### 1.2 java启动类（EurekaApplication.java）

```java
package com.jim.eureka;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.eureka.server.EnableEurekaServer;

@EnableEurekaServer
@SpringBootApplication
public class EurekaApplication {

    public static void main(String[] args) {
        SpringApplication.run(EurekaApplication.class, args);
    }
}

```

#### 1.3 服务参数配置和Eureka配置(application.yml)
使用过springboot的同学们都知道，服务启动会自动加载resources下application.yml文件作为服务配置。

```yml
server:
  port: 8088
  ompression:
    enabled: true
    min-response-size: 1024
eureka:
  instance:
    hostname: localhost
  client:
    #由于该应用为注册中心,所以设置为false,代表不向注册中心注册自己
    registerWithEureka: false
    #由于注册中心的职责就是维护服务实例,它并不需要去检索服务,所以也设置为false
    fetchRegistry: false
    serviceUrl:
      defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/
```

按以上步骤完成后，服务器就可以运行了，直接运行EurekaApplication.java, 然后在浏览器打开连接http://localhost:8088/ 即可看到服务注册中心

<img src="../img/20484fc7aaf8e6ad.png"/>


