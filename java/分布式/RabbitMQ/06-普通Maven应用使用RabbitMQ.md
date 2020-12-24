## 普通Maven应用使用RabbitMQ

- 创建Maven项目

- 添加依赖

  ```
  <dependency>
      <groupId>com.rabbitmq</groupId>
      <artifactId>amqp-client</artifactId>
      <version>4.5.0</version>
  </dependency>
  <dependency>
      <groupId>org.slf4j</groupId>
      <artifactId>slf4j-log4j12</artifactId>
      <version>1.7.25</version>
  </dependency>
  <dependency>
      <groupId>org.apache.commons</groupId>
      <artifactId>commons-lang3</artifactId>
      <version>3.3.2</version>
  </dependency>
  
  <!--整合到spring项目需要导入此依赖-->
  <dependency>
      <groupId>org.springframework.amqp</groupId>
      <artifactId>spring-rabbit</artifactId>
      <version>1.7.6.RELEASE</version>
  </dependency>
  ```

- 创建日志配置文件 log4j.properties

  ```
  log4j.rootLogger=DEBUG,A1 log4j.logger.com.taotao = DEBUG 
  log4j.logger.org.mybatis = DEBUG
  log4j.appender.A1=org.apache.log4j.ConsoleAppender
  log4j.appender.A1.layout=org.apache.log4j.PatternLayout
  log4j.appender.A1.layout.ConversionPattern=%-d{yyyy-MM-dd HH:mm:ss,SSS} [%t] [%c]-[%p] %m%n
  ```

- 创建帮助类

  ```java
  package com.qfedu.utils;
  
  import com.rabbitmq.client.Connection;
  import com.rabbitmq.client.ConnectionFactory;
  
  public class ConnectionUtil {
       public static Connection getConnection() throws Exception {
            //定义连接工厂
            ConnectionFactory factory = new ConnectionFactory();
            // 设置服务地址
            factory.setHost("47.96.11.185");
            //端口
            factory.setPort(5672);
            // 设置账号信息，用户名、密码、vhost
            factory.setVirtualHost("host1");
            factory.setUsername("ytao");
            factory.setPassword("admin123");
            // 通过工程获取连接
            Connection connection = factory.newConnection();
            return connection;
       }
  
  }
  ```

#### 发送消息

```java
package com.qfedu.send;

import com.qfedu.utils.ConnectionUtil;
import com.rabbitmq.client.Channel;
import com.rabbitmq.client.Connection;

public class Send {

    public static void main(String[] argv) throws Exception {
        // 获取到连接以及mq通道
        Connection connection = ConnectionUtil.getConnection();
        // 相当于数据库中的创建连接
        // 从连接中创建通道
        Channel channel = connection.createChannel();
        // 相当于数据库中的 statement
        // 声明（创建）队列,如果存在就不创建,不存在就创建
        // 参数1 队列名,
        // 参数2 durable： 是否持久化, 队列的声明默认是存放到内存中的，如果rabbitmq重启会丢失，如果想重启之后还存在就要使队列持久化，保存到Erlang自带的Mnesia数据库中，当rabbitmq重启之后会读取该数据库
        // 参数3 exclusive：是否排外的，有两个作用，一：当连接关闭时connection.close()该队列是否会自动删除； 二：该队列是否是私有的private，如果不是排外的，可以使用两个消费者都访问同一个队列，没有任何问题，如果是排外 的，会对当前队列加锁，其他通道channel是不能访问的，如果强制访问会报异常： com.rabbitmq.client.ShutdownSignalException: channel error; protocol method: #method<channel.close>(reply-code=405, reply-text=RESOURCE_LOCKED - cannot obtain exclusive access to locked queue 'queue_name' in vhost '/', class-id=50, method-id=20)一般等于true的话 用于一个队列只能有一个消费者来消费的场景
        // 参数4 autoDelete：是否自动删除，当最后一个消费者断开连接之后队列是否自动被删除，可以通过RabbitMQ Management，查看某个队列的消费者数量，当consumers = 0时队列就会自动删除
        // 参数5 arguments：  参数
        //channel.queueDeclare("queue1", false, false, true, null);
        // 消息内容
         String message = "Hello World!";
        // 参数1 交换机,此处无
        // 参数2 发送到哪个队列
        // 参数3 属性
        // 参数4 内容
         channel.basicPublish("", "queue1", null, message.getBytes());
        // 将消息发动到数据库
         System.out.println(" 发送数据 '" + message + "'");
        //关闭通道和连接
         channel.close();
         connection.close();
    }
}
```

#### 消费消息

```java
package com.qfedu.receive;

import com.qfedu.utils.ConnectionUtil;
import com.rabbitmq.client.*;

import java.io.IOException;

public class Receive {

    public static void main(String[] argv) throws Exception {
        //创建一个新的连接
        Connection connection = ConnectionUtil.getConnection();
        //创建一个通道
        Channel channel = connection.createChannel();

        //声明要关注的队列
        //channel.queueDeclare("queue1", false, false, false, null);
        //DefaultConsumer类实现了Consumer接口，通过传入一个频道，
        // 告诉服务器我们需要那个频道的消息，如果频道中有消息，就会执行回调函数handleDelivery
        Consumer consumer = new DefaultConsumer(channel) {
            public void handleDelivery(String consumerTag, Envelope envelope,
                                       AMQP.BasicProperties properties, byte[] body)
                    throws IOException {
                String message = new String(body, "UTF-8");
                System.out.println("Customer Received '" + message + "'");
            }
        };
        //自动回复队列应答 -- RabbitMQ中的消息确认机制
        channel.basicConsume("queue1", true, consumer);
    }
}
```