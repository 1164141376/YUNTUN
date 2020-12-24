## RabbitMQ介绍

- RabbitMQ是一个在AMQP基础上完成的，可复用的企业消息系统。他遵循Mozilla Public License开源协议。
- AMQP，即Advanced Message Queuing Protocol, 一个提供统一消息服务的应用层标准高级消息队列协议,是应用层协议的一个开放标准,为面向消息的中间件设计。基于此协议的客户端与消息中间件可传递消息，并不受客户端/中间件不同产品，不同的开发语言等条件的限制。Erlang中的实现有 RabbitMQ等。
- 主要特性：
  - 保证可靠性：使用一些机制来保证可靠性，如持久化、传输确认、发布确认
  - 灵活的路由功能
  - 可伸缩性：支持消息集群，多台RabbitMQ服务器可以组成一个集群
  - 高可用性：RabbitMQ集群中的某个节点出现问题时队列任然可用
  - 支持多种协议
  - 支持多语言客户端
  - 提供良好的管理界面
  - 提供跟踪机制：如果消息出现异常，可以通过跟踪机制分析异常原因
  - 提供插件机制：可通过插件进行多方面扩展

## RabbitMQ安装及配置

> 基于 CentOS 安装 RabbitMQ 3.7

说明：推荐使用本地安装，减少网络依赖

#### 1.安装前准备

- 如果之前安装过erlang,先删除

  ```
  yum remove erlang*
  ```

- 安装C++编译环境

  ```
  # yum -y install make gcc gcc-c++
  yum -y install make gcc gcc-c++ kernel-devel m4 ncurses-devel openssl-devel unixODBC unixODBC-devel httpd python-simplejson
  ```

- 下载erlang和rabbitMQ

  ```
  # 下载erlang
  wget http://www.erlang.org/download/otp_src_20.1.tar.gz
  
  # 下载rabbitMQ
  wget https://github.com/rabbitmq/rabbitmq-server/releases/download/v3.7.0/rabbitmq-server-generic-unix-3.7.0.tar.xz
  ```

#### 2.安装erlang

- 解压erlang安装包

  ```
  tar -xvf otp_src_20.1.tar.gz
  ```

- 进入解压文件夹

  ```
  cd otp_src_20.1
  ```

- 指定安装目录及安装配置（需要先安装并配置JDK）

  ```
  # erlang指定安装在/usr/local/erlang目录 
  ./configure --prefix=/usr/local/erlang --enable-smp-support --enable-threads --enable-sctp --enable-kernel-poll --enable-hipe --with-ssl --without-javac
  ```

- 编译与安装

  ```
  make && make install
  ```

- 配置erlang环境变量

  ```
  vi /etc/profile
  ```

  - 将 export PATH=$PATH:/usr/local/erlang/bin 添加到文件末尾

  ![image](03-RabbitMQ%E5%AE%89%E8%A3%85%E5%8F%8A%E9%85%8D%E7%BD%AE.assets/A24DC7227FE4411FBBE4E9F6C580B1D4)

- 重新加载profile文件

  ```
  source /etc/profile
  ```

#### 3.安装RabbitMQ

- 解压RabbitMQ安装包

  - 由于下载的安装包为xz文件，先将xz解压为tar

  ```
  xz -d rabbitmq-server-generic-unix-3.7.0.tar.xz
  ```

  

  - 再解压缩tar文件

  ```
  tar -xvf rabbitmq-server-generic-unix-3.7.0.tar
  ```

- 启动RabbitMQ

  - 进入到解压的RabbitMQ的sbin目录

  ```
  cd rabbitmq_server-3.7.0/sbin
  ```

  - 启动

  ```
  ./rabbitmq-server -detached
  ```

  - 查看进程

  ```
  ps aux|grep rabbit
  #ps a 显示现行终端机下的所有程序，包括其他用户的程序。
  #ps u 　 以用户为主的格式来显示程序状况。
  #ps x 　 显示所有程序，不以终端机来区分。
  ```

  

#### 4.启动管理界面

- 启动RabbitMQ的管理系统插件(需进入sbin目录)

  ```
  ./rabbitmq-plugins enable rabbitmq_management
  ```

  

#### 5.放行端口

> 如果没有网络指令需要先安装：yum install net-tools

- 查看并放行端口

  ```
  netstat -tlnp
  firewall-cmd --add-port=15672/tcp --permanent
  firewall-cmd --add-port=5672/tcp --permanent
  ```

  ![image](03-RabbitMQ%E5%AE%89%E8%A3%85%E5%8F%8A%E9%85%8D%E7%BD%AE.assets/F29082ABEC164474B557BADCB710151E)

- 也可以直接关闭防火墙

  - CentOS7

    ```
    #关闭防火墙 
    systemctl stop firewalld
    #开机禁用 
    systemctl disable firewalld
    #查看状态
    systemctl status firewalld
    ```

    

  - CentOS6

    ```
    #1.永久性生效，重启后不会复原
    #开启： 
    chkconfig iptables on
    #关闭： 
    chkconfig iptables off
    
    #2.即时生效，重启后复原
    #开启： 
    service iptables start
    #关闭： 
    service iptables stop
    
    #3.查询TCP连接情况：
    netstat -n | awk '/^tcp/ {++S[$NF]} END {for(a in S) print a, S[a]}'
    
    #4.查询端口占用情况：
    netstat   -anp   |   grep  portno（例如：netstat –apn | grep 80）
    ```

- 云服务器需要在控制台添加“安全组设置”

  - 阿里云服务器

    ![image](03-RabbitMQ%E5%AE%89%E8%A3%85%E5%8F%8A%E9%85%8D%E7%BD%AE.assets/8D175732B09546A8A5BE4034C355D6B7)![image](03-RabbitMQ%E5%AE%89%E8%A3%85%E5%8F%8A%E9%85%8D%E7%BD%AE.assets/E14618928A6443D2A389506753393D83)

  - 华为云服务器

    ![image](03-RabbitMQ%E5%AE%89%E8%A3%85%E5%8F%8A%E9%85%8D%E7%BD%AE.assets/BA5CF8232AAE4C50AE8834B3F4A71658)