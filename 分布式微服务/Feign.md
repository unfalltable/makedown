# Feign

## 简介

- 是一个声明式Http客户端
- 传统的Http远程调用是使用 RestTemplate ，存在一些问题
  - 代码可读性差，编程不统一
  - 参数复杂Url难以维护
- 自定义Fegin的配置
  - Fegin运行自定义的配置来覆盖默认配置
  - 配置：
    - fegin.Longger.Level：修改日志等级
      - NONE：没任何日志
      - BASIC：一次请求的请求时间，结束时间，耗时时间等基本信息
      - HEADERS：基本信息 + 请求头信息
      - FULL：基本信息 + 请求头信息 + 请求体 + 响应体

    - fegin.codec.Decoder：响应结果的解析器，将Json 转换为 Java对象
    - fegin.codec.Encoder：请求参数编码
    - fegin.Contract：支持的注解格式，规定支持的注解，默认是SpringMVC的注解
    - fegin.Retryer：失败重试机制，默认是不重试

- 使用
  - 配置文件
  - 加入Spring容器，创建对应的配置类
    - 全局配置：@EnableFeignClient(defaultConfiguration = 配置类)
    - 局部配置：@FeignClient(value = “作用的服务”, configuration = 配置类)


## 性能优化

- 底层的客户端，修改为支持连接池的客户端（导包 - 配置）
  - 默认是 URLConnection，不支持连接池
  - Apache HttpClient：支持连接池
  - OKHttp：支持连接池
- 日志级别最好设置为Basic

## 最佳实践

- 继承
  - 创建一个接口写这个方法声明，再让Client和Controller去继承和实现
  - 缺点
    - 对SpringMVC不起作用，这个接口的参数列表中的映射不会被继承
    - 服务紧耦合
- 抽取
  - 将方法声明，使用到的Pojo，默认配置等抽取为一个模块
  - 缺点
    - 会将所有方法引入，多余了一些

# OpenFeign

## 区别

- 

## 使用

- ```xml
  <dependency>
      <groupId>org.springframework.cloud</groupId>
      <artifactId>spring-cloud-starter-openfeign</artifactId>
  </dependency>
  ```

- ```yml
  eureka: 
  	client:
  		#是否注册进eureka
  		register-with-eureka: true
  		fetchRegistry: true
  		service-url: 
  			#集群注册
  			defaultZone: ...
  ```

- 启动类加@EnableFeignClients

- 创建一个独立的项目专门用于对接第三方接口

- 创建远程调用的service接口

  ![image-20221123154748155](C:\Users\BDA\Documents\Note\pic\image-20221123154748155.png)

- 配置OpenFeign的配置文件，也可以配置降级熔断的安全配置

  - 需要编写一个处理的类。实现远程调用的service接口