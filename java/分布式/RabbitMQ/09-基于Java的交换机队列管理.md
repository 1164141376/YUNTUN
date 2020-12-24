#### Java代码管理

- 交换机、队列创建及绑定

  ```
  //1. 声明队列-HelloWorld
  //参数1：queue - 指定队列的名称
  //参数2：durable - 当前队列是否需要持久化（true）
  //参数3：exclusive - 是否排外（conn.close() - 当前队列会被自动删除，当前队列只能被一个消费者消费）
  //参数4：autoDelete - 如果这个队列没有消费者在消费，队列自动删除
  //参数5：arguments - 指定当前队列的其他信息
  channel.queueDeclare("HelloWorld",true,false,false,null);
  
  //2. 创建exchange - 绑定某一个队列
  //参数1： exchange的名称
  //参数2： 指定exchange的类型  FANOUT - pubsub ,   DIRECT - Routing , TOPIC - Topics
  channel.exchangeDeclare("pubsub-exchange", BuiltinExchangeType.FANOUT);
  channel.queueBind("pubsub-queue1","pubsub-exchange","");
  channel.queueBind("pubsub-queue2","pubsub-exchange","");
  ```

#### SpringBoot Java配置管理

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