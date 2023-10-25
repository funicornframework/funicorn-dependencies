# 火麟框架
基于 spring-boot-starter2.7.9 和 spring cloud alibaba全家桶快速搭建微服务项目群，提供丰富的开源框架工具，开箱即用

## funicorn-common-base
1、DateTimeUtil.java -- 日期工具类 <br>
2、EasyExcelFactory.getInstance() -- 快速导出excel，支持自定义表头 <br>
3、HttpRequestFactory.getInstance() -- Http工具类，支持Get、Post请求 <br>
4、JsonUtil -- Json转换工具类

## funicorn-common-datasource
mysql + mybatis plus + Generator

CodeGenerator(代码自动生成器) <br>
根据数据库表自动生成Controller、Mapper、Service、ServiceImpl、Mapper.xml <br>

BaseEntity(基础公共字段)<br>
created_by、created_time、updated_by、updated_time、deleted <br>
created_time、updated_time: 自动设置服务器时间 <br>
deleted: 0未删除 1已删除 自动逻辑删除和查询自动过滤已删除数据 <br>
created_by、updated_by: 继承ContextUserProvider并重写getContextUserDetail()即可实现自动插入当前用户 <br>

## funicorn-common-redisson
基于redisson-spring-boot-starter自动注入RedisTemplate<String, Object> redisTemplate <br>

## funicorn-common-mqtt
支持配置多mqtt服务器、多主题 <br>
yml格式如下: <br>
* mqtt:
    + enable: false
    + server-configs:
        - server-addr: tcp://mqtt.demo.com:1883
        - username: Aimee
        - password: Aa123456
        - clientId: 99999
        - topic: topic:qos,topic:qos

食用方式:  
1、实现 MqttClientService  
2、实现类上添加 @MqttClient(value ="")  value可以是serverAddr或者clientId    
3、实现类创建method(String topic,MqttMessage message)  
4、method上添加 @MqttTopic("/up/IOT/+/SerialNet") 设置你想监听的哪个主题，即可监听主题消息  
5、发送 mqttClientServiceImpl.publish()

## funicorn-common-ampq
基于spring-boot-starter-amqp自动注册RabbitTemplate rabbitTemplate  
同时实现根据用户配置实现自动绑定exchange+queue+routing  
yml格式如下:
* spring:
    + rabbitmq:
        + exchange-config:
            - enable: false
            - exchanges:
                - type: Topic
                - name: exchange.test.water.data
                - bind:
                    - queue: queue.test.water.data
                    - routing-key: routing.water.*

## funicorn-common-aws
OSS对象存储，基于aws-java-sdk-s3自动注册OssTemplate ossTemplate
支持Amazon S3、Minio、阿里云OSS  
yml格式如下:
* amazon:
    + enable: true
    + aws:
        - access-key-id: accessKeyId
        - access-key-secret: accessKeySecret
    + s3:
        - default-bucket: bucketName
        - endpoint: http://endpoint.com
        - region: cn-bj

## funicorn-common-xxljob
基于xxl-job实现任务调度中心自动注册，IJobHandler Bean类模式自动注册  
yml格式如下:
* xxl:
    + job
        + enable: true
        + aws:
            - server-addr: http://opt.funicorn.cn:18080/xxl-job-admin
            - access-token: default_token
            + executor
                - app-name: funicorn-boot-example
                - address:
                - ip:
                - port: 9999

## funicorn-common-netty
支持创建服务端和客户端  
NettyFactory.getInstance().createServer(int port,ChannelHandler initializer)  
NettyFactory.getInstance().createClient(String host,int port, ChannelHandler initializer)  