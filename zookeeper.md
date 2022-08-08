# Windows安装ZooKeeper教程

1. 官网地址

> 下载地址[https://zookeeper.apache.org/index.html](https://zookeeper.apache.org/index.html)

2. 安装包下载

   > 带源码包(Source)和不带源码包

3. 解压压缩包

> 可以看到我们下载的是 .tar.gz 后缀的文件，这种一般是放在 Linux 上安装，现在我们在 Windows 安装，如果我们用正常的方式来解压时可能会出现 “压缩文件损坏等错误”
>
> 解决办法:
>
> 文件夹按住shift，鼠标右键 用powershell打开
>
> 运行 `tar -zxvf .\apache-zookeeper-xxx.tar.gz`

4. 更改配置

> 安装目录下新增两个文件夹，一个命名为 data ，一个命名为 log
>
> 找到解压目录下的 conf 目录，将目录中的 zoo_sample.cfg 文件，复制一份，重命名为 zoo.cfg
>
> 修改 zoo.cfg 配置文件，将默认的 dataDir=/tmp/zookeeper 修改成 zookeeper 安装目录所在的data 文件夹，再增加数据日志的配置
>
> dataDir = D:\\\\zookeeper\\\xxx\\\\data
>
> dataLogDir = D:\\\\zookeeper\\\xxx\\\\log
>
> **注意：**
> 这里配置的路径使用双斜杠，如果是单斜杠，在我们启动服务端的时候就会把我们配置的内容输出到 bin 目录下面
>
> **更改配置的操作原因：**
> 安装目录下， bin 目录的 zkEnv.cmd 文件中指定了配置文件的名称和路径，而 zkServer.cmd 服务器启动脚本中引用了这个 zkEnv.cmd 文件，所以我们网上冲浪学习的时候会经常看到需要修改配置文件名称，其实也可以更改指定的配置文件名称，原理就是如此。

5. 配置相关说明

   > tickTime：这个时间是作为 Zookeeper 服务器之间或客户端与服务器之间维持心跳的时间间隔，也就是每个 tickTime 时间就会发送一个心跳。
   >
   > initLimit：这个配置项是用来配置 Zookeeper 接受客户端（这里所说的客户端不是用户连接 Zookeeper 服务器的客户端，而是 Zookeeper 服务器集群中连接到 Leader 的 Follower 服务器）初始化连接时最长能忍受多少个心跳时间间隔数。当已经超过 10个心跳的时间（也就是 tickTime）长度后 Zookeeper 服务器还没有收到客户端的返回信息，那么表明这个客户端连接失败。总的时间长度就是 10*2000=20 秒
   >
   > syncLimit：这个配置项标识 Leader 与 Follower 之间发送消息，请求和应答时间长度，最长不能超过多少个 tickTime 的时间长度，总的时间长度就是 5*2000=10秒
   >
   > dataDir：顾名思义就是 Zookeeper 保存数据的目录，默认情况下，Zookeeper 将写数据的日志文件也保存在这个目录里。
   >
   > clientPort：这个端口就是客户端连接 Zookeeper 服务器的端口，Zookeeper 会监听这个端口，接受客户端的访问请求。
   >
   > server.A=B：C：D：其中 A 是一个数字，表示这个是第几号服务器；B 是这个服务器的 ip 地址；C 表示的是这个服务器与集群中的 Leader 服务器交换信息的端口；D 表示的是万一集群中的 Leader 服务器挂了，需要一个端口来重新进行选举，选出一个新的 Leader，而这个端口就是用来执行选举时服务器相互通信的端口。如果是伪集群的配置方式，由于 B 都是一样，所以不同的 Zookeeper 实例通信端口号不能一样，所以要给它们分配不同的端口号。

6. 启动服务端

> 进入安装目录下的 bin 目录
> 双击 zkServer.cmd 启动
>
> 如果出现闪退 可能是java没有装JAVA_HOME 没有配置

7. 启动客户端

> 同上，bin 目录下
> 双击 zkCli.cmd 启动
>
> 可以看看我们配置的 data 和 log 目录，配置已生效