# springcloud配置中心自动更新

实现配置中心自动更新(通过rabbitmq实现):
1.config-server:
    添加如下依赖:
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-starter-bus-amqp</artifactId>
            </dependency>
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-config-monitor</artifactId>
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
4.自动更新: 添加Webhooks,
            Payload URL配置 http://m2221d6543.iok.la/monitor(url外网必须可以访问到)
            Content type选择 application/json                    
//4.手动更新:每次修改git中的配置文件,需要通过POST方法访问http://ip:port/actuator/bus-refresh(配置中心的ip和port)去主动获取新的配置                        












