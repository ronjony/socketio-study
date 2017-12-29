# 整体架构及层次

```code
this is the test
```

### socketio的作用

```
使用websocket协议实现，使客户端与服务器端进行双向通信
```

### socketio的实现版本

> netty-socketio 为java服务器版本,可以参考  
> socket.io-client-java 为官方客户端版本，可以参考  
> 其它的版本实现也非常多，可以在后续学习其它语言中参考

### demo运行

Benchmark

We did a simple load test on laptop \(i7-2630QM 4xcore CPU @2.00GHz\), with Client/Server both run on it. It could process about 80k messages/second under 50k long-live connections.

The test code could be found at spray.contrib.socketio.examples.benchmark

To run cluster benchmark \(0.1.x\):

1. Install cassandra and start it.
2. sbt clean compile dist
3. cd examples/socketio-benchmark/target/universal/
4. unzip bench\_cluster-\*.zip
5. cd bench\_cluster-xxxx/bin
6. ./start\_cluster.sh tran 2551
7. ./start\_cluster.sh conn1
8. ./start\_cluster.sh conn2
9. ./start\_cluster.sh busi
10. ./start\_driver.sh
11. cd ../logs
12. tail -f driver\_rt.log

To run cluster benchmark \(0.2.x\):

1. Install cassandra and start it.
2. sbt clean compile dist
3. cd examples/socketio-benchmark/target/universal/
4. unzip bench\_cluster-\*.zip
5. cd bench\_cluster-xxxx/bin
6. ./start\_cluster.sh sess1 2551
7. ./start\_cluster.sh sess2
8. ./start\_cluster.sh topic1
9. ./start\_cluster.sh tran
10. ./start\_cluster.sh busi
11. ./start\_driver.sh
12. cd ../logs
13. tail -f rt\_driver.log

Since spray-socketio is under heavy developing, with the spray-socketio version changed or snapshot version, you may need to cleanup cassandra by:

```
cqlsh
cqlsh> select * from system.schema_keyspaces;
cqlsh> drop keyspace akka;
cqlsh> drop keyspace akka_snapshot;
cqlsh> quit;
```



