# 1、Zookeeper做服务注册中心

ZooKeeper是一个开源的**分布式协调服务**

新建一个父工程项目spingcloud（看Eureka这篇博客，上面有详细介绍）

## 1.1、cloud-provider-payment8004

**新建一个cloud-provider-payment8004模块**

pom.xml

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>springcloud</artifactId>
        <groupId>com.feige</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>cloud-provider-payment8004</artifactId>

    <dependencies>

        <dependency>
            <groupId>com.feige</groupId>
            <artifactId>cloud-api-commons</artifactId>
            <version>${project.version}</version>
        </dependency>


        <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-web -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-web -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.springframework.cloud/spring-cloud-starter-zookeeper-discovery -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-zookeeper-discovery</artifactId>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-devtools -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.projectlombok/lombok -->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-test -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>


    </dependencies>


</project>
~~~

application.yaml

~~~yaml
server:
  port: 8004

spring:
  application:
    name: cloud-provider-payment
  cloud:
    zookeeper:
      connect-string: 49.235.158.110:3344


~~~

PaymentController.java

~~~java
package com.feige.controller;

import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

import java.util.UUID;

@RestController
@Slf4j
public class PaymentController {

    @Value("${server.port}")
    private String serverPort;

    @GetMapping(value = "/payment/zk")
    public String paymentZk(){
        return "springcloud with zookeeper:"+serverPort+"\t"+ UUID.randomUUID().toString();
    }

}
~~~

**主启动类**

ZkApplication8004.java

~~~java
package com.feige;


import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;

@SpringBootApplication
@EnableDiscoveryClient
public class ZkApplication8004 {
    public static void main(String[] args) {
        SpringApplication.run(ZkApplication8004.class,args);
    }
}

~~~

## 1.2、启动zookeeper和cloud-provider-payment8004

~~~shell
[root@VM_0_2_centos ~]# docker run -d -p 3344:2181 --name zk zookeeper
97823ba7d1dc49ba1975a1758f1dfe859f2d102249ecab1b7ab4c8a0df6c41cb
[root@VM_0_2_centos ~]# docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                                                  NAMES
97823ba7d1dc        zookeeper           "/docker-entrypoint.…"   42 seconds ago      Up 40 seconds       2888/tcp, 3888/tcp, 8080/tcp, 0.0.0.0:3344->2181/tcp   zk
[root@VM_0_2_centos ~]# docker exec -it 97823ba7d1dc /bin/bash
root@97823ba7d1dc:/apache-zookeeper-3.6.1-bin# ls
LICENSE.txt  NOTICE.txt  README.md  README_packaging.md  bin  conf  docs  lib
root@97823ba7d1dc:/apache-zookeeper-3.6.1-bin# cd bin
root@97823ba7d1dc:/apache-zookeeper-3.6.1-bin/bin# ls
README.txt  zkCleanup.sh  zkCli.cmd  zkCli.sh  zkEnv.cmd  zkEnv.sh  zkServer-initialize.sh  zkServer.cmd  zkServer.sh  zkSnapShotToolkit.cmd  zkSnapShotToolkit.sh  zkTxnLogToolkit.cmd  zkTxnLogToolkit.sh
root@97823ba7d1dc:/apache-zookeeper-3.6.1-bin/bin# zkCli.sh
[zk: localhost:2181(CONNECTED) 4] ls /services
[cloud-provider-payment]
[zk: localhost:2181(CONNECTED) 5] ls /services/cloud-provider-payment
[402b3df0-b543-4046-b11e-d59a9e107e51]
[zk: localhost:2181(CONNECTED) 6] ls /services/cloud-provider-payment/402b3df0-b543-4046-b11e-d59a9e107e51
[]
[zk: localhost:2181(CONNECTED) 7] get /services/cloud-provider-payment/402b3df0-b543-4046-b11e-d59a9e107e51
{"name":"cloud-provider-payment","id":"402b3df0-b543-4046-b11e-d59a9e107e51","address":"192.168.0.106","port":8004,"sslPort":null,"payload":{"@class":"org.springframework.cloud.zookeeper.discovery.ZookeeperInstance","id":"application-1","name":"cloud-provider-payment","metadata":{}},"registrationTimeUTC":1590247962766,"serviceType":"DYNAMIC","uriSpec":{"parts":[{"value":"scheme","variable":true},{"value":"://","variable":false},{"value":"address","variable":true},{"value":":","variable":false},{"value":"port","variable":true}]}}
~~~

## 1.3、cloud-consumerzk-order80

**新建一个模块cloud-consumerzk-order80**

pom.xml

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>springcloud</artifactId>
        <groupId>com.feige</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>cloud-consumerzk-order80</artifactId>


    <dependencies>

        <dependency>
            <groupId>com.feige</groupId>
            <artifactId>cloud-api-commons</artifactId>
            <version>${project.version}</version>
        </dependency>


        <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-web -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-web -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.springframework.cloud/spring-cloud-starter-zookeeper-discovery -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-zookeeper-discovery</artifactId>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-devtools -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.projectlombok/lombok -->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-test -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>


    </dependencies>





</project>
~~~

application.yaml

~~~yaml
server:
  port: 80

spring:
  application:
    name: cloud-consumer-order
  cloud:
    zookeeper:
      connect-string: 49.235.158.110:3344

~~~

OrderZKController.java

~~~java
package com.feige.controller;

import lombok.extern.slf4j.Slf4j;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.client.RestTemplate;

import javax.annotation.Resource;

@RestController
@Slf4j
public class OrderZKController {

    public static final String INVOKE_URL = "http://cloud-provider-payment";

    @Resource
    private RestTemplate restTemplate;

    @GetMapping("/consumer/payment/zk")
    public String payment (){
        String result = restTemplate.getForObject(INVOKE_URL+"/payment/zk",String.class);
        return result;
    }
}

~~~

ApplicationContextConfig.java

~~~java
package com.feige.config;

import org.springframework.cloud.client.loadbalancer.LoadBalanced;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.client.RestTemplate;

@Configuration
public class ApplicationContextConfig {

    @LoadBalanced
    @Bean
    public RestTemplate getRestTemplate(){
        return new RestTemplate();
    }
}

~~~

**主启动类**

ZkOrderApplication80.java

~~~java
package com.feige;


import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;

@SpringBootApplication
@EnableDiscoveryClient
public class ZkOrderApplication80 {
    public static void main(String[] args) {
        SpringApplication.run(ZkOrderApplication80.class,args);
    }
}
~~~



## 1.4、启动cloud-consumerzk-order80

**进入zkCli.sh**

~~~shell
ls /services #便可查看当前注册了的服务
~~~

