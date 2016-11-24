# 整体架构及层次

```code
this is the test
```


### socketio的作用
```
使用websocket协议实现，使客户端与服务器端进行双向通信

```

### socketio的实现版本

>netty-socketio 为java服务器版本,可以参考
>socket.io-client-java 为官方客户端版本，可以参考
>其它的版本实现也非常多，可以在后续学习其它语言中参考

### demo运行

Benchmark

We did a simple load test on laptop (i7-2630QM 4xcore CPU @2.00GHz), with Client/Server both run on it. It could process about 80k messages/second under 50k long-live connections.

The test code could be found at spray.contrib.socketio.examples.benchmark

To run cluster benchmark (0.1.x):

1. Install cassandra and start it.
1. sbt clean compile dist
1. cd examples/socketio-benchmark/target/universal/
1. unzip bench_cluster-*.zip
1. cd bench_cluster-xxxx/bin
1. ./start_cluster.sh tran 2551
1. ./start_cluster.sh conn1
1. ./start_cluster.sh conn2
1. ./start_cluster.sh busi
1. ./start_driver.sh
1. cd ../logs
1. tail -f driver_rt.log


To run cluster benchmark (0.2.x):

1. Install cassandra and start it.
1. sbt clean compile dist
1. cd examples/socketio-benchmark/target/universal/
1. unzip bench_cluster-*.zip
1. cd bench_cluster-xxxx/bin
1. ./start_cluster.sh sess1 2551
1. ./start_cluster.sh sess2
1. ./start_cluster.sh topic1
1. ./start_cluster.sh tran
1. ./start_cluster.sh busi
1. ./start_driver.sh
1. cd ../logs
1. tail -f rt_driver.log

Since spray-socketio is under heavy developing, with the spray-socketio version changed or snapshot version, you may need to cleanup cassandra by:

```
cqlsh
cqlsh> select * from system.schema_keyspaces;
cqlsh> drop keyspace akka;
cqlsh> drop keyspace akka_snapshot;
cqlsh> quit;
```
