## 使用RabbitMQ发送-接收对象

- 本周就是将对象转换成字符串进行传递

#### 1.对象序列化实现

> - bean实现序列化接口
> - 生产者消费者的bean包名必须一致

- 消息生产者

  ```
  @Service
  public class SendMsgService {
  
      @Autowired
      private AmqpTemplate amqpTemplate;
  
      public void sendMsg(Goods goods) {
          byte[] bytes = SerializationUtils.serialize(goods);
          amqpTemplate.convertAndSend("queue1",bytes);
      }
  }
  ```

- 消息消费者

  ```
  @Service
  @RabbitListener(queues = "queue1")
  public class ReviceMsgService {
  
      @RabbitHandler
      public void receiveMsg(byte[] bs) {
          Goods goods = (Goods) SerializationUtils.deserialize(bs);
          System.out.println(goods);
      }
  
  }
  ```

#### 2.JSON字符串实现

- 消息生产者

  ```
  @Service
  public class SendMsgService {
  
      @Autowired
      private AmqpTemplate amqpTemplate;
  
          public void sendMsg(Goods goods) throws JsonProcessingException {
  
              ObjectMapper mapper=new ObjectMapper();
              String message=mapper.writeValueAsString(goods);
              amqpTemplate.convertAndSend("queue1",message);
  
          }
  }
  ```

- 消息消费者

  ```
  @Service
  @RabbitListener(queues = "queue1")
  public class ReviceMsgService {
  
      @RabbitHandler
      public void receiveMsg(String msg) throws JsonProcessingException {
          ObjectMapper mapper=new ObjectMapper();
          Goods goods=mapper.readValue(msg,Goods.class);
          System.out.println(goods);
      }
  
  }
  ```