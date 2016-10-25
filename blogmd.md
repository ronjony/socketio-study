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




