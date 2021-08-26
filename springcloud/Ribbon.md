# 1、Ribbon(负载均衡器)

目前主流的负载方案分为以下两种：

- 集中式负载均衡，在消费者和服务提供方中间使用独立的代理方式进行负载，有硬件的（比如 F5），也有软件的（比如 Nginx）。
- 客户端自己做负载均衡，根据自己的请求情况做负载，Ribbon 就属于客户端自己做负载。

Spring CloudRibbon 是一个基于 HTTP 和 TCP 的客户端负载均衡工具，它基于 Netflix Ribbon 实现。通过 SpringCloud 的封装，可以让我们轻松地将面向服务的 REST 模版请求自动转换成客户端负载均衡的服务调用。

Spring Cloud Ribbon 虽然只是一个工具类框架，它不像服务注册中心、配置中心、API 网关那样需要独立部署，但是它几乎存在于每一个 Spring Cloud 构建的微服务和基础设施中。因为微服务间的调用，API 网关的请求转发等内容，实际上都是通过 Ribbon 来实现的（https://github.com/Netflix/ribbon）

![image-20200525092722360](https://gitee.com/feigeCode/picture/raw/master/img/image-20200525092722360.png)

**架构说明**

![image-20200525130201521](https://gitee.com/feigeCode/picture/raw/master/img/image-20200525130201521.png)

# 2、入门

**参考Eureka这篇博客,里面有项目的结构**

**Eureka自带了ribbon，我们只要引入Eureka，就有了ribbon**

![image-20200525130551703](https://gitee.com/feigeCode/picture/raw/master/img/image-20200525130551703.png)

> 加上@LoadBalanced就有了负载均衡的能力了

![image-20200525133923075](https://gitee.com/feigeCode/picture/raw/master/img/image-20200525133923075.png)



> getForObject和getForEntity的区别

~~~java
package com.feige.controller;

import com.feige.pojo.CommonResult;
import com.feige.pojo.Payment;
import lombok.extern.slf4j.Slf4j;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.client.RestTemplate;

import javax.annotation.Resource;

@RestController
@Slf4j
public class OrderController {

    @Resource
    private RestTemplate restTemplate;

    public static final String PAYMENT_URL = "http://CLOUD-PAYMENT-SERVICE";
    /**
     * 该方法可以理解为对getForEntity的进一步封装，
     * 它通过HttpMessageConverterExtractor对HTTP的请求响应体body内容进行对象转换，
     * 实现请求直接返回包装好的对象内容
     * @param id ID
     * @return CommonResult
     */
    @GetMapping("/consumer/payment/get/{id}")
    public CommonResult getPayment(@PathVariable("id") Long id){
        return restTemplate.getForObject(PAYMENT_URL+"/payment/get/"+id,CommonResult.class);
    }

    /**
     * 该方法返回的是ResponseEntity，该对象是Spring对HTTP请求响应的封装
     * ，其中主要存储了HTTP的几个重要元素，
     * 比如HTTP请求状态码的枚举对象HttpStatus（也就是我们常说的404、500这些错误码）、
     * 在它的父类HttpEntity中还存储着HTTP请求的头信息对象HttpHeaders以及泛型类型的请求体对象。
     * 返回的ResponseEntity对象的body内容类型会根据第二个参数转换为String类型。
     * @param id
     * @return
     */
    @GetMapping("/consumer/payment/getEntity/{id}")
    public CommonResult getPayment1(@PathVariable("id") Long id){
        ResponseEntity<CommonResult> entity = restTemplate.getForEntity(PAYMENT_URL + "/payment/getEntity/" + id, CommonResult.class);
        if (entity.getStatusCode().is2xxSuccessful()){
            return entity.getBody();
        }else {
            return new CommonResult<>(444,"失败");
        }
    }
}

~~~

> postForObject和postForEntity的区别

~~~java
 //和GetForObject类似
@GetMapping("/consumer/payment/create")
    public CommonResult create(Payment payment){
        return restTemplate.postForObject(PAYMENT_URL+"/payment/create",payment,CommonResult.class);  //写操作
    }

	//和GetForEntity类似
    @GetMapping("/consumer/payment/create1")
    public CommonResult create1(Payment payment){
        ResponseEntity<CommonResult> entity = restTemplate.postForEntity(PAYMENT_URL + "/payment/create1", payment, CommonResult.class);
        if (entity.getStatusCode().is2xxSuccessful()) {
            return entity.getBody();
        }else {
            return new CommonResult(444,"失败");
        }

    }
~~~

# 3、负载均衡规则

> 负载均衡规则

**IRule:根据特定算法从服务列表中选取一个要访问的服务**

![image-20200525130329865](https://gitee.com/feigeCode/picture/raw/master/img/image-20200525130329865.png)

![image-20200525125831113](https://gitee.com/feigeCode/picture/raw/master/img/image-20200525125831113.png)

> 替换

![image-20200525134838755](https://gitee.com/feigeCode/picture/raw/master/img/image-20200525134838755.png)

**这个自定义配置类不能放在 @ComponentScan 所扫描的当前包下以及子包下，否则自定义的配置类就会被所有的 Ribbon 客户端所共享，达不到特殊化定制的目的了。**

![image-20200525134749473](https://gitee.com/feigeCode/picture/raw/master/img/image-20200525134749473.png)

MyRule.java

~~~java
package com.rule.myRule;

import com.netflix.loadbalancer.IRule;
import com.netflix.loadbalancer.RandomRule;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class MyRule {
    @Bean
    public IRule getIRule(){
        return new RandomRule();//定义为随机
    }
}

~~~

主启动类加上`@RibbonClient(name = "CLOUD-PAYMENT-SERVICE",configuration = MyRule.class)`

测试http://localhost/consumer/payment/get/31

# 4、自定义负载均衡规则

> RoundRobinRule的原理

![image-20200525140437026](https://gitee.com/feigeCode/picture/raw/master/img/image-20200525140437026.png)

> RoundRobinRule源码

~~~java
/*
 *
 * Copyright 2013 Netflix, Inc.
 *
 * Licensed under the Apache License, Version 2.0 (the "License");
 * you may not use this file except in compliance with the License.
 * You may obtain a copy of the License at
 *
 * http://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 * See the License for the specific language governing permissions and
 * limitations under the License.
 *
 */
package com.netflix.loadbalancer;

import com.netflix.client.config.IClientConfig;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import java.util.List;
import java.util.concurrent.atomic.AtomicInteger;

/**
 * The most well known and basic load balancing strategy, i.e. Round Robin Rule.
 *
 * @author stonse
 * @author Nikos Michalakis <nikos@netflix.com>
 *
 */
public class RoundRobinRule extends AbstractLoadBalancerRule {

    private AtomicInteger nextServerCyclicCounter;
    private static final boolean AVAILABLE_ONLY_SERVERS = true;
    private static final boolean ALL_SERVERS = false;

    private static Logger log = LoggerFactory.getLogger(RoundRobinRule.class);

    public RoundRobinRule() {
        nextServerCyclicCounter = new AtomicInteger(0);
    }

    public RoundRobinRule(ILoadBalancer lb) {
        this();
        setLoadBalancer(lb);
    }

    public Server choose(ILoadBalancer lb, Object key) {
        if (lb == null) {
            log.warn("no load balancer");
            return null;
        }

        Server server = null;
        int count = 0;
        while (server == null && count++ < 10) {
            List<Server> reachableServers = lb.getReachableServers();
            List<Server> allServers = lb.getAllServers();
            int upCount = reachableServers.size();
            int serverCount = allServers.size();

            if ((upCount == 0) || (serverCount == 0)) {
                log.warn("No up servers available from load balancer: " + lb);
                return null;
            }

            int nextServerIndex = incrementAndGetModulo(serverCount);
            server = allServers.get(nextServerIndex);

            if (server == null) {
                /* Transient. */
                Thread.yield();
                continue;
            }

            if (server.isAlive() && (server.isReadyToServe())) {
                return (server);
            }

            // Next.
            server = null;
        }

        if (count >= 10) {
            log.warn("No available alive servers after 10 tries from load balancer: "
                    + lb);
        }
        return server;
    }

    /**
     * Inspired by the implementation of {@link AtomicInteger#incrementAndGet()}.
     *
     * @param modulo The modulo to bound the value of the counter.
     * @return The next value.
     */
    private int incrementAndGetModulo(int modulo) {
        for (;;) {
            int current = nextServerCyclicCounter.get();
            int next = (current + 1) % modulo;
            if (nextServerCyclicCounter.compareAndSet(current, next))
                return next;
        }
    }

    @Override
    public Server choose(Object key) {
        return choose(getLoadBalancer(), key);
    }

    @Override
    public void initWithNiwsConfig(IClientConfig clientConfig) {
    }
}
~~~

> 自己实现一个本地负载均衡器

把`@LoadBalanced`注解去了

LoadBalancer.java

~~~java
package com.feige.lb;


import org.springframework.cloud.client.ServiceInstance;

import java.util.List;

public interface LoadBalancer {
    ServiceInstance instance(List<ServiceInstance> serviceInstances);
}

~~~

MyLb.java

~~~java
package com.feige.lb.impl;

import com.feige.lb.LoadBalancer;
import org.springframework.cloud.client.ServiceInstance;
import org.springframework.stereotype.Component;

import java.util.List;
import java.util.concurrent.atomic.AtomicInteger;

@Component
public class MyLb implements LoadBalancer {

    private AtomicInteger atomicInteger = new AtomicInteger(0);

    private final int getIncrement(){
        int current;
        int next;
        do {
            current = this.atomicInteger.get();
            next = current >= 2147483647  ? 0 : current + 1;
            //compareAndSet第一个参数是期望值，第二个参数是修改值
        }while (!this.atomicInteger.compareAndSet(current,next));
        System.out.println("*******第几次访问，次数next: "+next);
        return next;
    }
    @Override
    public ServiceInstance instance(List<ServiceInstance> serviceInstances) {
        int index = getIncrement() % serviceInstances.size();
        return serviceInstances.get(index);
    }
}

~~~

​	OrderController.java

~~~java
package com.feige.controller;

import com.feige.lb.LoadBalancer;
import lombok.extern.slf4j.Slf4j;
import org.springframework.cloud.client.ServiceInstance;
import org.springframework.cloud.client.discovery.DiscoveryClient;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.client.RestTemplate;

import javax.annotation.Resource;
import java.net.URI;
import java.util.List;

@RestController
@Slf4j
public class OrderController {

    @Resource
    private RestTemplate restTemplate;

    @Resource
    private DiscoveryClient discoveryClient;

    @Resource
    private LoadBalancer loadBalancer;

    @GetMapping("/consumer/payment/get/lb")
    public String getServerPort(){
        List<ServiceInstance> instances = discoveryClient.getInstances("CLOUD-PAYMENT-SERVICE");
        if (instances == null || instances.size() == 0){
            return null;
        }
        ServiceInstance instance = loadBalancer.instance(instances);
        URI uri = instance.getUri();
        return restTemplate.getForObject(uri+"/payment/get/lb",String.class);
    }
}

~~~

项目结构图

![image-20200528164430632](https://gitee.com/feigeCode/picture/raw/master/img/image-20200528164430632.png)