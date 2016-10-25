2014-1-23 18:09 来自 微博 weibo.com

spray-socketio，收到和返回了第一个消息。


2014-2-10 17:30 来自 微博 weibo.com

或者我现在就在spray-socketio中先试一下用Rx来对付incoming packets。


2014-2-16 15:28 来自 微博 weibo.com

spray-socketio, 把parboiled2的JsonParser揉进来了。


2014-2-17 22:55 来自 微博 weibo.com

spray-socketio有点模样了。不过socket.io的协议写得不严谨，害我掉了几个坑


2014-2-19 22:07 来自 微博 weibo.com

在spray-socketio中暂且引入了RxScala(RxJava的wrapper)，用来以事件(数据)流的方式驱动业务逻辑，代码看上去还行。还没有写让akka调度RxScala的代码，或许会等akka决定采用哪个Rx实现。

2014-2-25 16:00 来自 微博 weibo.com

spray-socketio, WebSocker和XHR-Polling的实现差不多了。


2014-3-4 22:05 来自 微博 weibo.com

在用spray-websocket的client写个简单的性能测试工具，准备初测一下spray-websocket和spray-socketio的server. 算是自己测自己。

2014-3-6 04:04 来自 微博 weibo.com

spray-socketio.。草写了个测试程序，自己测自己。在我的笔记本上初测，client和server分别占到1.5G和2G内存时开到5000个连接。当每批消息数发到12万时开始丢信息，这时平均响应时间为1.3秒。开2000个连接时，可以抗到每批消息20万，平均响应时间3秒。总之，8万/s的响应无压力，且性能与消息量呈线性。

2014-3-7 00:31 来自 微博 weibo.com

spray-socketio. 写了个sbt dist命令和几个脚本，方便脱离sbt直接跑测试。还是在我的笔记本上同时跑client和server。开到2万个长连接，server占用内存2G。响应时间稳定在1s时，可处理9万消息。当5秒内全部消息不能返回时停机，这时100ms左右发了23万消息，平均响应时间为2.8s，server内存占用为2.7G。


2014-3-7 22:05 来自 微博 weibo.com

要大改一下spray-socketio。为了分布式，需要想办法消去那些被一批actors所依赖的actor（内面通常保存着一些查询表）。


2014-3-10 18:52 来自 微博 weibo.com

spray-socketio: 我的笔记本上跑到了5万长连接（同时跑C/S），性能仍能保持在9万消息/秒，并在20万消息/秒时才遇到5秒超时，这时服务器内存在2.7G左右。


2014-3-10 22:36 来自 微博 weibo.com

spray-socketio: 很高兴@陈兴润 把高可用分片集群的代码也调通了，spray-socketio可能成为地球上最强悍的socket.io实现，当然，spray-websocket的集群就更没难度了。


2014-3-13 04:29 来自 微博 weibo.com

spray-socketio: 逻辑想得越来越清楚，代码也更好看了。趁着天还没亮，四下无人，提交了。


2014-3-19 17:08 来自 微博 weibo.com

昨晚开始转入沉思模式，持续约一周（文档时间）。总结、发掘spray-socketio实现过程中架构上的特点及这个架构能回答和拓展哪些业务逻辑上的问题及约束。包括但不限于它顺着Rx所带来的回答和拓展这些逻辑的新的视角。


2014-4-3 15:23 来自 微博 weibo.com

在豌豆荚的基于Akka的spray-socketio分片集群的实现中，就是"actor always exists, virtually"，尤其是带状态的actor，或者直接叫entity actor可以很简单地实现分片集群和active/reactive on demand，并且以EventSource的形式更新和保持状态(包括持久化和基于Event重演的状态恢复)。


2014-4-7 01:32 来自 微博 weibo.com

用spray-socketio实现大规模有状态长连接集群的技术难点基本都验证完毕，这是实时流式计算的入口。接下来呢？作为基础架构中的MQ，虽然有各种具分布式和高可用的实现，但离我设想的多少有距离。我在考虑要不要用akka来做一个。想了几天了，也写了些原型代码，估算了一下可能一个月出原型，两个月初成。


2014-6-18 22:09 来自 微博 weibo.com

<#scaladays#>parboiled2几天前发布了正式版，用它实现的json parser比parboild1快了6、7倍，比Jackson只慢3倍。parboild是基于PEGs (Parsing Expression Grammars)的解析器。 spray-socketio的packet parser是用它实现的。


2014-10-19 00:33 来自 微博 weibo.com

重构后的 spray-socketio，Namespace 成为集群内的 sharding actor，其实，它可以看作一个通用的 MQ 的起点，供集群内外的业务逻辑订阅。经过这次重构，整个集群的架构更加合理和清晰，整个人也觉得好了。刚把重构后的 benchmark 跑起来了，在笔记本上一共起了 5 个节点和一个 Driver。正好历时一周。

2014-10-31 19:00 来自 华为Ascend Mate7

从坑里出来了。整个 spray-socketio 现在解耦成清晰的三个部分，Transport / Session / Namespace，可以在一个集群内，也可各成集群。其中 Namespace 就是一个可以 scale-out 的 MQ。而业务逻揖仍在外面，从集群收发消息流，并围绕消息实时处理业务。


2014-12-31 00:01 来自 微博 weibo.com

Adventure 2014: 1. Spray-WebSocket; 2. Spray-SocketIO; 3. AvPath; 4. Akka based distributed status service; 5. Refined time series reactor; 6. Supported SBT project on NetBeans; 7. Grasped stream.





