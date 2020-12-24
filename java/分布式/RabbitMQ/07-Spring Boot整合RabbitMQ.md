## 消息发送者

- 创建SpringBoot项目"mq-send-demo"，导入以下依赖：

  - Spring Web
  - Thymeleaf
  - Spring for RabbitMQ ![image](07-Spring%20Boot%E6%95%B4%E5%90%88RabbitMQ.assets/75AA2E24E9FB460C97C136AE4A12A4E0)

- 如果是在项目中新增RabbitMQ支持导入以下依赖：

  ```
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-amqp</artifactId>
  </dependency>
  
  <dependency>
      <groupId>org.springframework.amqp</groupId>
      <artifactId>spring-rabbit-test</artifactId>
      <scope>test</scope>
  </dependency>
  ```

- 在application.yml配置RabbitMQ服务器连接属性

  ```
  spring:
    application:
      name: mq-sender-demo
    rabbitmq:
      host: 47.96.11.185
      port: 5672
      username: ytao
      password: admin123
      virtual-host: wfx_host
  
  server:
    port: 9001
  ```

- 配置RabbitMQ创建队列（Quence）

  ```
  @Configuration
  public class RabbitMQConfiguration {
  
      @Bean
      public Queue queue() {
          return new Queue("wfx-quence");
      }
  
      @Bean
      public Queue fanoutQuence() {
          return new Queue("wfx-fanout-quence");
      }
      /**
       * 声明交换机,fanout 类型
       */
      @Bean
      public FanoutExchange fanoutExchange() {
          FanoutExchange fanoutExchange = new FanoutExchange("fanoutExchange");
          return fanoutExchange;
      }
      /**
       * 将队列和交换机绑定
       */
      @Bean
      public Binding bindingFanoutExchange(Queue fanoutQuence, FanoutExchange fanoutExchange) {
          return BindingBuilder.bind(fanoutQuence).to(fanoutExchange);
      }
  
  
      @Bean
      public Queue directQuence1() {
          return new Queue("wfx-direct-quence1");
      }
      @Bean
      public Queue directQuence2() {
          return new Queue("wfx-direct-quence2");
      }
      /**
       * 声明交换机,direct 类型
       */
      @Bean
      public DirectExchange directExchange() {
          DirectExchange directExchange = new DirectExchange("directExchange");
          return directExchange;
      }
      /**
       * 将队列和交换机绑定
       */
      @Bean
      public Binding bindingDirectExchange(Queue directQuence1, DirectExchange directExchange) {
         return BindingBuilder.bind(directQuence1).to(directExchange).with("rk1");
      }
  
      @Bean
      public Binding bindingDirectExchange2(Queue directQuence2, DirectExchange directExchange) {
          return BindingBuilder.bind(directQuence2).to(directExchange).with("rk2");
      }
  
  }
  ```

- 在消息发送者中注入AmqpTemplate对象即可发送消息

  ```
  @Service
  public class SendMsgService {
  
      @Autowired
      private AmqpTemplate amqpTemplate;
  
      public void sendMsg(String message){
          amqpTemplate.convertAndSend("wfx-simple",message);
  
          //amqpTemplate.convertAndSend("fanout", "", message);
  
          //amqpTemplate.convertAndSend("direct", "rk1", message);
      }
  }
  ```

## 消息接收者

- 监听队列

  ```
  @Component
  @RabbitListener(queues = "wfx-direct-quence1")
  public class ReceiveDirectHanlder1 {
  
      @RabbitHandler//标记当前方法是用来处理消息的
      public void recevieMsg(String message) {
          System.out.println("收到wfx-direct-quence1消息: =>"+message);
      }
  
  }
  ```