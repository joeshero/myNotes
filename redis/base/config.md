
```
# Redis 配置文件示例
#
# 注意：要使 Redis 运行时读取本配置文件的话，需要在运行时指定配置文件路径:
# ./redis-server /path/to/redis.conf
 
# 当涉及到内存大小时，可以使用常用单位例如 1k 5GB 4M 等等，单位换算如下:
# 1k => 1000 bytes
# 1kb => 1024 bytes
# 1m => 1000000 bytes
# 1mb => 1024*1024 bytes
# 1g => 1000000000 bytes
# 1gb => 1024*1024*1024 bytes
# 以上单位不区分大小写，所以 1GB 1Gb 1gB 都是同一个意思。
 
################################## 包含 ###################################
 
# include 指令的意思是包含其它配置文件，当有多个 Redis 服务需要配置，但其中少数参数有区
# 别时，则可以将公共的配置放入到一个公共的配置文件中，然后通过子配置文件引入父配置，从而
# 将配置按照模块分开。
#
# 注意：“include” 指令不受来自 admin 或 Redis Sentinel 的 “CONFIG REWRITE” 影响（不会
# 被重写）。
# 如果存在多个相同指令的不同参数设置，Redis 总是使用最后一次的设置，因此你最好把 “include” 放在
# 文件的开始处，以避免在运行时被后面的文件重写。 如果相反，你希望使用 “include” 来覆盖这些
# 配置选项，则最好把它放在配置文件末尾。
#
# include /path/to/local.conf
# include /path/to/other.conf
 
################################## 模块 #####################################
 
# 在启动时加载模块，如果加载失败则启动过程终止。可以同时加载多个模块。
#
# loadmodule /path/to/my_module.so
# loadmodule /path/to/other_module.so
 
################################## 网络 #####################################
 
# 默认如果没有 "bind" 指令设置的话, Redis 默认监听所有接口。可以使用 "bind" 监听
# 指定网络接口。可以同时指定一个或多个 IP 地址。
#
# bind 192.168.1.100 10.0.0.1
# bind 127.0.0.1 ::1
#
# ~~~ 警告 ~~~ 如果 Redis 所在服务器直接暴露在互联网，那么监听所有网络接口是一件
# 危险的事情。所以默认我们在下面设置了只监听环回地址，当然如果你明确了解了这些配置
# 的含义，你可以自行调整配置参数。
#
# ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
bind 127.0.0.1
 
# 保护模式是一种安全保护，目的在于避免 Redis 实例暴露在互联网遭到未授权访问和攻击。
# 当保护模式开启时，如果:
# 1) 服务器没有使用 "bind" 指令明确监听某个接口。
# 2) 服务器没有设置访问密码。
# 那么服务器仅接收来自 127.0.0.1（IPv4） 和 ::1（IPv6）, 以及 Unix domain sockets
# 的连接。
#
# 保护模式默认开启。
protected-mode yes
 
# 指定监听端口，默认为 6379。
# 如果指定了端口 0，则代表 Redis 不监听任何 TCP 协议。
port 6379
 
# TCP listen() backlog.
#
# 在高并发的场景你可能需要设置一个较高的 backlog 参数来避免客户端连接问题。注意 
# Linux 内核对并发连接数也有限制，通常在 /proc/sys/net/core/somaxconn。所以需
# 要同时调高 somaxconn 和 tcp_max_syn_backlog 参数。
tcp-backlog 511
 
# Unix socket.
#
# 监听 Unix socket， 用于接收请求，默认 Redis 不会监听 unix socket，因为没有指定
# socket 路径。
#
# unixsocket /tmp/redis.sock
# unixsocketperm 700
 
# 在客户端处于空闲 N 秒后断开连接。(0 为从不断开)
timeout 0
 
# TCP keepalive.
# 客户端心跳检测间隔时间，默认为 300 秒。
tcp-keepalive 300
 
################################# 通用设置 #####################################
 
# 默认情况下 Redis 已非守护进程方式运行。如果需要以守护进程方式运行，则将下面的 no 改为 yes。
# 守护进程方式运行支持后台运行。它会创建一个 pid 文件 /var/run/redis.pid
daemonize no
 
# 和操作系统相关参数可以设置通过 upstart 和 systemd 管理 Redis 守护进程, centos 7 以后都使用systemd
#   supervised no      - no supervision interaction
#   supervised upstart - signal upstart by putting Redis into SIGSTOP mode
#   supervised systemd - signal systemd by writing READY=1 to $NOTIFY_SOCKET
#   supervised auto    - detect upstart or systemd method based on
#                        UPSTART_JOB or NOTIFY_SOCKET environment variables
# Note: these supervision methods only signal "process is ready."
#       They do not enable continuous liveness pings back to your supervisor.
supervised no
 
# 指定 pid 文件路径，如果不是后台方式运行，即使指定了路径，pid 文件也不会被创建，而以后台方式运行
# 的话，没有指定也会创建 pid 文件，默认为 "/var/run/redis.pid"。
pidfile /var/run/redis_6379.pid
 
# 设置日志级别
# debug (大量信息，用于开发或测试)
# verbose (比 debug 信息少)
# notice (普通通知，可用于生产环境)
# warning (告警、严重问题)
loglevel notice
 
# 指定日志文件，如果已后台方式运行但日志文件为空的话，日志会被丢弃。
logfile ""
 
# 是否启用 syslog。
# syslog-enabled no
 
# 设置 syslog 识别字段
# syslog-ident redis
 
# 设置 syslog facility. 必须为 USER 或 在 LOCAL0-LOCAL7 之间。
# syslog-facility local0
 
# 设置数据库数量。
databases 16
 
# 是否在启动时显示 redis logo。
always-show-logo yes
 
################################ SNAPSHOTTING  ################################
# 设置数据库保存间隔
 
save 900 1
save 300 10
save 60 10000
 
# 磁盘写入存在问题是是否继续接收数据库写入
stop-writes-on-bgsave-error yes
 
# dump .rdb 数据库时是否使用 LZF 压缩。
rdbcompression yes
 
# 版本 5 及以上开始提供 rdb 存储的校验和。
rdbchecksum yes
 
# dump 的数据库存放点
dbfilename dump.rdb
 
# 数据库存放目录
dir ./
 
################################# REPLICATION #################################
 
# 主从配置，slave 复制对应的 master
# replicaof <masterip> <masterport>
 
# 如果master设置了 requirepass，那么 slave 要连上 master，需要有 master 的密码才行。
# masterauth 就是用来配置master的密码，这样可以在连上master后进行认证。
# masterauth <master-password>
 
# 当从库同主机失去连接或者复制正在进行，从机库有两种运行方式：1) 如果 slave-serve-stale-data 设置为
# yes(默认设置)，从库会继续响应客户端的请求。2) 如果 slave-serve-stale-data 设置为no，
# INFO,replicaOF, AUTH, PING, SHUTDOWN, REPLCONF, ROLE, CONFIG,SUBSCRIBE, UNSUBSCRIBE,
# PSUBSCRIBE, PUNSUBSCRIBE, PUBLISH, PUBSUB,COMMAND, POST, HOST: and LATENCY命令之外的任何请求
# 都会返回一个错误 ”SYNC with master in progress”。
replica-serve-stale-data yes
 
# 作为从服务器，默认情况下是只读的（yes），可以修改成NO，用于写（不建议）
replica-read-only yes
 
# Replication SYNC strategy: disk or socket.
# 是否使用socket方式复制数据。目前redis复制提供两种方式，disk和socket。如果新的slave连上来或者
# 重连的slave无法部分同步，就会执行全量同步，master会生成rdb文件。有2种方式：disk方式是master创建
# 一个新的进程把rdb文件保存到磁盘，再把磁盘上的rdb文件传递给slave。socket是master创建一个新的进
# 程，直接把rdb文件以socket的方式发给slave。disk方式的时候，当一个rdb保存的过程中，多个slave都能
# 共享这个rdb文件。socket的方式就的一个个slave顺序复制。在磁盘速度缓慢，网速快的情况下推荐用socket方式。
repl-diskless-sync no
 
# diskless复制的延迟时间，防止设置为0。一旦复制开始，节点不会再接收新slave的复制请求直到下一个rdb传输。
# 所以最好等待一段时间，等更多的slave连上来
repl-diskless-sync-delay 5
 
# slave根据指定的时间间隔向服务器发送ping请求。
# 时间间隔可以通过 repl_ping_slave_period 来设置，默认10秒。
# repl-ping-replica-period 10
 
# 复制连接超时时间。master和slave都有超时时间的设置。
# master检测到slave上次发送的时间超过repl-timeout，即认为slave离线，清除该slave信息。
# slave检测到上次和master交互的时间超过repl-timeout，则认为master离线。
# 需要注意的是repl-timeout需要设置一个比repl-ping-slave-period更大的值，不然会经常检测到超时
# repl-timeout 60
 
# 是否禁止复制tcp链接的tcp nodelay参数，可传递yes或者no。默认是no，即tcp nodelay。如果
# master设置了yes来禁止tcp nodelay设置，在把数据复制给slave的时候，会减少包的数量和更小的网络带
# 宽。但是这也可能带来数据的延迟。默认我们推荐更小的延迟，但是在数据量传输很大的场景下，建议选择yes
repl-disable-tcp-nodelay no
 
# 复制缓冲区大小，这是一个环形复制缓冲区，用来保存最新复制的命令。这样在slave离线的时候，不需要完
# 全复制master的数据，如果可以执行部分同步，只需要把缓冲区的部分数据复制给slave，就能恢复正常复制状
# 态。缓冲区的大小越大，slave离线的时间可以更长，复制缓冲区只有在有slave连接的时候才分配内存。没有
# slave的一段时间，内存会被释放出来，默认1m
# repl-backlog-size 1mb
 
# master没有slave一段时间会释放复制缓冲区的内存，repl-backlog-ttl用来设置该时间长度。单位为秒。
# repl-backlog-ttl 3600
 
# 当master不可用，Sentinel会根据slave的优先级选举一个master。最低的优先级的slave，当选master。
# 而配置成0，永远不会被选举
replica-priority 100
 
# redis提供了可以让master停止写入的方式，如果配置了min-replicas-to-write，
# 健康的slave的个数小于N，mater就禁止写入。master最少得有多少个健康的slave存活才能执行写命令。
# 这个配置虽然不能保证N个slave都一定能接收到master的写操作，但是能避免没有足够健康的slave的时候，
# master不能写入来避免数据丢失。设置为0是关闭该功能
# min-replicas-to-write 3
# min-replicas-max-lag 10
#
# 延迟小于min-replicas-max-lag秒的slave才认为是健康的slave
 
# 当处于NAT转换网络中时（例如docker），从机能够设置声明的IP，以便主机连接
# replica-announce-ip 5.5.5.5
# replica-announce-port 1234
 
################################## SECURITY ###################################
 
# requirepass配置可以让用户使用AUTH命令来认证密码，才能使用其他命令。这让redis可以使用在不受信任的
# 网络中。为了保持向后的兼容性，可以注释该命令，因为大部分用户也不需要认证。使用requirepass的时候需要
# 注意，因为redis太快了，每秒可以认证15w次密码，简单的密码很容易被攻破，所以最好使用一个更复杂的密码
# requirepass foobared
 
# 把危险的命令给修改成其他名称。比如CONFIG命令可以重命名为一个很难被猜到的命令，这样用户不能使用，而
# 内部工具还能接着使用:
# 例如：
# rename-command CONFIG b840fc02d524045429941cc15f59e41cb7be6c52
 
# 设置成一个空的值，可以禁止一个命令
# rename-command CONFIG ""
 
################################### CLIENTS ####################################
 
# Set the max number of connected clients at the same time. By default
# this limit is set to 10000 clients, however if the Redis server is not
# able to configure the process file limit to allow for the specified limit
# the max number of allowed clients is set to the current file limit
# minus 32 (as Redis reserves a few file descriptors for internal uses).
#
# Once the limit is reached Redis will close all the new connections sending
# an error 'max number of clients reached'.
#
# maxclients 10000
 
############################## MEMORY MANAGEMENT ################################
 
# redis配置的最大内存容量。当内存满了，需要配合maxmemory-policy策略进行处理。注意slave的输出缓冲区
# 是不计算在maxmemory内的。所以为了防止主机内存使用完，建议设置的maxmemory需要更小一些
# maxmemory <bytes>
 
# 内存容量超过maxmemory后的处理策略。
# volatile-lru：利用LRU算法移除设置过过期时间的key。
# volatile-random：随机移除设置过过期时间的key。
# volatile-ttl：移除即将过期的key，根据最近过期时间来删除（辅以TTL）
# allkeys-lru：利用LRU算法移除任何key。
# allkeys-random：随机移除任何key。
# noeviction：不移除任何key，只是返回一个写错误。
# 上面的这些驱逐策略，如果redis没有合适的key驱逐，对于写命令，还是会返回错误。redis将不再接收写请求，
# 只接收get请求。写命令包括：set setnx setex append incr decr rpush lpush rpushx lpushx linsert 
# lset rpoplpush sadd sinter sinterstore sunion sunionstore sdiff sdiffstore zadd zincrby 
# zunionstore zinterstore hset hsetnx hmset hincrby incrby decrby getset mset msetnx exec sort。
# maxmemory-policy noeviction
 
# LRU 检测的样本数。使用lru或者ttl淘汰算法，从需要淘汰的列表中随机选择sample个key，
# 选出闲置时间最长的key移除
# maxmemory-samples 5
 
# 是否开启salve的最大内存
# replica-ignore-maxmemory yes
 
############################# LAZY FREEING ####################################
 
# 以非阻塞方式释放内存
# 使用以下配置指令调用了
 
lazyfree-lazy-eviction no
lazyfree-lazy-expire no
lazyfree-lazy-server-del no
replica-lazy-flush no
 
############################## APPEND ONLY MODE ###############################
 
# Redis 默认不开启。它的出现是为了弥补RDB的不足（数据的不一致性），所以它采用日志的形式来记录每个写
# 操作，并追加到文件中。Redis 重启的会根据日志文件的内容将写指令从前到后执行一次以完成数据的恢复工作
# 默认redis使用的是rdb方式持久化，这种方式在许多应用中已经足够用了。但是redis如果中途宕机，会导致可
# 能有几分钟的数据丢失，根据save来策略进行持久化，Append Only File是另一种持久化方式，可以提供更好的
# 持久化特性。Redis会把每次写入的数据在接收后都写入 appendonly.aof 文件，每次启动时Redis都会先把这
# 个文件的数据读入内存里，先忽略RDB文件。若开启rdb则将no改为yes
 
appendonly no
 
# 指定本地数据库文件名，默认值为 appendonly.aof
 
appendfilename "appendonly.aof"
 
# aof持久化策略的配置
# no表示不执行fsync，由操作系统保证数据同步到磁盘，速度最快
# always表示每次写入都执行fsync，以保证数据同步到磁盘
# everysec表示每秒执行一次fsync，可能会导致丢失这1s数据
 
# appendfsync always
appendfsync everysec
# appendfsync no
 
# 在aof重写或者写入rdb文件的时候，会执行大量IO，此时对于everysec和always的aof模式来说，执行
# fsync会造成阻塞过长时间，no-appendfsync-on-rewrite字段设置为默认设置为no。如果对延迟要求很高的
# # 应用，这个字段可以设置为yes，否则还是设置为no，这样对持久化特性来说这是更安全的选择。设置为yes表
# 示rewrite期间对新写操作不fsync,暂时存在内存中,等rewrite完成后再写入，默认为no，建议yes。Linux的
# 默认fsync策略是30秒。可能丢失30秒数据
 
no-appendfsync-on-rewrite no
 
# aof自动重写配置。当目前aof文件大小超过上一次重写的aof文件大小的百分之多少进行重写，即当aof文件
# 增长到一定大小的时候Redis能够调用bgrewriteaof对日志文件进行重写。当前AOF文件大小是上次日志重写得
# 到AOF文件大小的二倍（设置为100）时，自动启动新的日志重写过程
 
auto-aof-rewrite-percentage 100
 
# 设置允许重写的最小aof文件大小，避免了达到约定百分比但尺寸仍然很小的情况还要重写
auto-aof-rewrite-min-size 64mb
 
# aof文件可能在尾部是不完整的，当redis启动的时候，aof文件的数据被载入内存。重启可能发生在redis所
# 在的主机操作系统宕机后，尤其在ext4文件系统没有加上data=ordered选项（redis宕机或者异常终止不会造
# 成尾部不完整现象。）出现这种现象，可以选择让redis退出，或者导入尽可能多的数据。如果选择的是yes，
# 当截断的aof文件被导入的时候，会自动发布一个log给客户端然后load。如果是no，用户必须手动redis-
# check-aof修复AOF文件才可以
aof-load-truncated yes
 
# 加载redis时，可以识别AOF文件以“redis”开头。
# 字符串并加载带前缀的RDB文件，然后继续加载AOF尾巴
aof-use-rdb-preamble yes
 
################################ LUA SCRIPTING  ###############################
 
# 如果达到最大时间限制（毫秒），redis会记个log，然后返回error。当一个脚本超过了最大时限。只有
# SCRIPT KILL和SHUTDOWN NOSAVE可以用。第一个可以杀没有调write命令的东西。要是已经调用了write，只能
# 用第二个命令杀
lua-time-limit 5000
 
################################ REDIS CLUSTER  ###############################
 
# 集群开关，默认是不开启集群模式
# cluster-enabled yes
 
# 集群配置文件的名称，每个节点都有一个集群相关的配置文件，持久化保存集群的信息。这个文件并不需要手动
# 配置，这个配置文件由Redis生成并更新，每个Redis集群节点需要一个单独的配置文件，请确保与实例运行的系
# 统中配置文件名称不冲突
# cluster-config-file nodes-6379.conf
 
# 节点互连超时的阀值。集群节点超时毫秒数
# cluster-node-timeout 15000
 
# 在进行故障转移的时候，全部slave都会请求申请为master，但是有些slave可能与master断开连接一段时间
# 了，导致数据过于陈旧，这样的slave不应该被提升为master。该参数就是用来判断slave节点与master断线的时
# 间是否过长。判断方法是：
# 比较slave断开连接的时间和(node-timeout * slave-validity-factor) + repl-ping-slave-period
# 如果节点超时时间为三十秒, 并且slave-validity-factor为10,假设默认的repl-ping-slave-period是10
# 秒，即如果超过310秒slave将不会尝试进行故障转移
# cluster-replica-validity-factor 10
 
# master的slave数量大于该值，slave才能迁移到其他孤立master上，如这个参数若被设为2，那么只有当一
# 个主节点拥有2 个可工作的从节点时，它的一个从节点会尝试迁移
# cluster-migration-barrier 1
 
# 默认情况下，集群全部的slot有节点负责，集群状态才为ok，才能提供服务。设置为no，可以在slot没有全
# 部分配的时候提供服务。不建议打开该配置，这样会造成分区的时候，小分区的master一直在接受写请求，而
# 造成很长时间数据不一致
# cluster-require-full-coverage yes
 
# 选项设置为yes时，会阻止replicas尝试对其master在主故障期间进行故障转移 
# 然而如果强制这样做的话，master仍然可以执行手动故障转移。
# cluster-replica-no-failover no
 
 
########################## CLUSTER DOCKER/NAT support  ########################
 
# 默认情况下，Redis会自动检测自己的IP和从配置中获取绑定的PORT，告诉客户端或者是其他节点。 
# 而在Docker环境中，如果使用的不是host网络模式，在容器内部的IP和PORT都是隔离的，那么客户端
# 和其他节点无法通过节点公布的IP和PORT建立连接。 
# 如果开启以下配置，Redis节点会将配置中的这些IP和PORT告知客户端或其他节点。而
# 这些IP和PORT是通过Docker转发到容器内的临时IP和PORT的。 
# cluster-announce-ip 10.1.1.5
# cluster-announce-port 6379
# cluster-announce-bus-port 6380
 
################################## SLOW LOG ###################################
 
# 记录超过多少微秒的查询命令 #1000000等于1秒，设置为0则记录所有命令 slowlog-log-slower-than 10000
# 记录大小，可通过SLOWLOG RESET命令重置
slowlog-max-len 128
 
################################ LATENCY MONITOR ##############################
 
# 记录执行时间大于或等于预定时间（毫秒）的操作,为0时不记录
latency-monitor-threshold 0
 
############################# EVENT NOTIFICATION ##############################
 
# Redis能通知 Pub/Sub 客户端关于键空间发生的事件，默认关闭
notify-keyspace-events ""
 
############################### ADVANCED CONFIG ###############################
 
# 当hash只有少量的entry时，并且最大的entry所占空间没有超过指定的限制时，会用一种节省内存的
# 数据结构来编码。可以通过下面的指令来设定限制
hash-max-ziplist-entries 512
hash-max-ziplist-value 64
 
# 当取正值的时候，表示按照数据项个数来限定每个quicklist节点上的ziplist长度。比如，
# 当这个参数配置成5的时候，表示每个quicklist节点的ziplist最多包含5个数据项。 
# 当取负值的时候，表示按照占用字节数来限定每个quicklist节点上的ziplist长度。这时，
# 它只能取-1到-5 #这五个值，每个值含义如下： 
# -5: 每个quicklist节点上的ziplist大小不能超过64 Kb。（注：1kb => 1024 bytes） 
# -4: 每个quicklist节点上的ziplist大小不能超过32 Kb。 
# -3: 每个quicklist节点上的ziplist大小不能超过16 Kb。 
# -2: 每个quicklist节点上的ziplist大小不能超过8 Kb。（-2是Redis给出的默认值） 
# -1: 每个quicklist节点上的ziplist大小不能超过4 Kb
list-max-ziplist-size -2
 
# 这个参数表示一个quicklist两端不被压缩的节点个数。
# 注：这里的节点个数是指quicklist双向链表的节点个数，而不是指ziplist里面的数据项个数。
# 实际上，一个quicklist节点上的ziplist，如果被压缩，就是整体被压缩的。
# 参数list-compress-depth的取值含义如下：
# 0: 是个特殊值，表示都不压缩。这是Redis的默认值。
# 1: 表示quicklist两端各有1个节点不压缩，中间的节点压缩。
# 2: 表示quicklist两端各有2个节点不压缩，中间的节点压缩。
# 3: 表示quicklist两端各有3个节点不压缩，中间的节点压缩。
# 依此类推
# 由于0是个特殊值，很容易看出quicklist的头节点和尾节点总是不被压缩的，以便于在表的两端进行快速存取。
list-compress-depth 0
 
# set有一种特殊编码的情况：当set数据全是十进制64位有符号整型数字构成的字符串时。 
# 下面这个配置项就是用来设置set使用这种编码来节省内存的最大长度。
set-max-intset-entries 512
 
# 与hash和list相似，有序集合也可以用一种特别的编码方式来节省大量空间。 
# 这种编码只适合长度和元素都小于下面限制的有序集合:
zset-max-ziplist-entries 128
zset-max-ziplist-value 64
 
# HyperLogLog稀疏结构表示字节的限制。该限制包括 #16个字节的头。当HyperLogLog使用稀疏结构表示 
# 这些限制，它会被转换成密度表示。 
# 值大于16000是完全没用的，因为在该点 
# 密集的表示是更多的内存效率。 
# 建议值是3000左右，以便具有的内存好处, 减少内存的消耗.
hll-sparse-max-bytes 3000
 
# Streams宏节点最大大小/项目。流数据结构是基数编码内部多个项目的大节点树。使用此配置 
# 可以配置单个节点的字节数，以及切换到新节点之前可能包含的最大项目数 
# 追加新的流条目。如果以下任何设置设置为0，忽略限制，因此例如可以设置一个 
# 大入口限制将max-bytes设置为0，将max-entries设置为所需的值
stream-node-max-bytes 4096
stream-node-max-entries 100
 
# 启用哈希刷新，每100个CPU毫秒会拿出1个毫秒来刷新Redis的主哈希表（顶级键值映射表）
activerehashing yes
 
# 客户端的输出缓冲区的限制，可用于强制断开那些因为某种原因从服务器读取数据的速度不够快的客户端
client-output-buffer-limit normal 0 0 0
client-output-buffer-limit replica 256mb 64mb 60
client-output-buffer-limit pubsub 32mb 8mb 60
 
# 客户端查询缓冲区累积新命令。它们仅限于固定的默认情况下， 
# 多数情况下为了避免协议不同步导致客户端查询缓冲区中未绑定的内存使用量的错误 
# 但是，如果你有使用的话，你可以在这里配置它，比如我们有很多执行请求或类似的。
#
# client-query-buffer-limit 1gb
 
# 在Redis协议中，批量请求，即表示单个的元素strings，通常限制为512 MB。 #但是，您可以更改此限制
# proto-max-bulk-len 512mb
 
# 默认情况下，“hz”的被设定为10。提高该值将在Redis空闲时使用更多的CPU时，但同时当有多个key
# 同时到期会使Redis的反应更灵敏，以及超时可以更精确地处理
hz 10
 
# 开启动态 hz
dynamic-hz yes
 
# 当一个子进程重写AOF文件时，如果启用下面的选项，则文件每生成32M数据会被同步
aof-rewrite-incremental-fsync yes
 
# 当redis保存RDB文件时，如果启用了以下选项，每生成32 MB数据，文件将被fsync-ed。 
# 这很有用，以便以递增方式将文件提交到磁盘并避免大延迟峰值。
rdb-save-incremental-fsync yes
 
# 可以调整计数器counter的增长速度，lfu-log-factor越大，counter增长的越慢。
# lfu-log-factor 10
# 是一个以分钟为单位的数值，可以调整counter的减少速度
# lfu-decay-time 1
 
########################### ACTIVE DEFRAGMENTATION #######################
# 启用主动碎片整理
# activedefrag yes
 
# 启动活动碎片整理的最小碎片浪费量
# active-defrag-ignore-bytes 100mb
 
# 启动碎片整理的最小碎片百分比
# active-defrag-threshold-lower 10
 
# 使用最大消耗时的最大碎片百分比
# active-defrag-threshold-upper 100
 
# 在CPU百分比中进行碎片整理的最小消耗
# active-defrag-cycle-min 5
 
# 磁盘碎片整理的最大消耗
# active-defrag-cycle-max 75
 
# 将从主字典扫描处理的最大set
# active-defrag-max-scan-fields 1000
```