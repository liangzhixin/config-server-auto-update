# springcloud配置中心自动更新

实现配置中心自动更新:
1.config-server:
    添加如下依赖:
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-starter-bus-amqp</artifactId>
            </dependency>
    配置文件添加:
        spring.rabbitmq.host=10.101.90.78
        spring.rabbitmq.port=5672
        spring.rabbitmq.username=lzx
        spring.rabbitmq.password=lzx
        management.endpoints.web.exposure.include=bus-refresh
2.config-client:
    添加如下依赖:
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-starter-bus-amqp</artifactId>
            </dependency>
    配置文件添加:
        spring:
          rabbitmq:
            host: 10.101.90.78
            port: 5672
            username: lzx
            password: lzx
3.在需要自动更新的类上添加@RefreshScope注解      
4.每次修改git中的配置文件,需要通过POST方法访问http://ip:port/actuator/bus-refresh(配置中心的ip和port)去主动获取新的配置                        



注解@EnableEurekaClient,@EnableDiscoveryClient的区别:
1.注解@EnableEurekaClient上有@EnableDiscoveryClient的注解,可以说基本就是EnableEurekaClient有@EnableDiscoveryClient的功能
2.@EnableEurekaClient就是一种方便使用eureka的注解而已,推荐在服务采用eureka作为注册中心时使用
  如果使用其他的注册中心，推荐使用@EnableDiscoveryClient注解
  
  

配置中心动态刷新(需要手动刷新,项目多了不建议使用)
1.客户端添加spring-boot-starter-actuator依赖
2.在需要的类上面添加@RefreshScope注解
3.客户端配置文件添加下列属性(暴露/refresh端点):
        management:
          endpoints:
            web:
              exposure:
                include: refresh
4.每次修改git中的配置文件,需要通过POST方法访问http://ip:port/actuator/refresh(客户端的ip和port)去主动获取新的配置

注意:如果一个类添加了@RefreshScope注解,该类会被更新重建,但是其他任何类都不会被更新,除非其他类也添加了@RefreshScope注解.



config-server和config-client分别设置spring.cloud.config.label时,config-client的配置有效,
如果config-client没有配置该属性而config-server配置了该属性,则config-server配置的属性有效.



配置中心git仓库的application.properties配置能够被读取



配置中心config-server的配置文件必须是properties文件,不能是yml文件(否则读取不到git仓库的配置信息)












