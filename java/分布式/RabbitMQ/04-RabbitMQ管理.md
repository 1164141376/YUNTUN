## 用户管理

#### 1.用户级别

- 超级管理员administrator,可以登录控制台,查看所有信息,可以对用户和策略进行操作
- 监控者monitoring,可以登录控制台,可以查看节点的相关信息,比如进程数,内存磁盘使用情况
- 策略制定者policymaker ,可以登录控制台,制定策略,但是无法查看节点信息
- 普通管理员 management 仅能登录控制台
- 其他, 无法登录控制台,一般指的是提供者和消费者

#### 2.添加用户（命令模式）

- 添加/配置用户

  ```
  # 插件目录
  ./rabbitmqctl add_user ytao admin123
  ```

- 设置用户权限

  ```
  #设置admin为administrator级别
  ./rabbitmqctl set_user_tags ytao administrator
  ```

  

#### 3.添加用户（web方式）

- 浏览器访问：http://47.96.11.185:15672/ (使用guest guest 登录, guest 具有最高权限,只能在本机登录；先使用命令行创建一个用户)

  ![image](04-RabbitMQ%E7%AE%A1%E7%90%86.assets/41D1E9C8E6B6494397A10DD328DD89A4)

- 添加用户

  ![image](04-RabbitMQ%E7%AE%A1%E7%90%86.assets/D8A88B1B9C1747A79C202F1AACAACE2C)

- 为用户分配可以访问的虚拟主机

  - 默认情况下没有任何可以访问的,我们可以添加一个主机(相当于添加一个数据库),然后分配权限

    ![image](04-RabbitMQ%E7%AE%A1%E7%90%86.assets/5AF6C7D6A154462B84F18ACF181393AE)

  - 创建虚拟主机

    ![image](04-RabbitMQ%E7%AE%A1%E7%90%86.assets/BCDA0C5EFB14416CBF7F0A913C06E8AA)

  - 给指定用户分配虚拟主机

    ![image](04-RabbitMQ%E7%AE%A1%E7%90%86.assets/D79EE0EB70654749A1B7E456DB866DE0)![image](04-RabbitMQ%E7%AE%A1%E7%90%86.assets/4035DC4D28CE466D93278800E52F5255)

  - 设置完成后，回到用户界面确认

    ![image](04-RabbitMQ%E7%AE%A1%E7%90%86.assets/472D0A7356A34D5EA532BEA644F5058B)