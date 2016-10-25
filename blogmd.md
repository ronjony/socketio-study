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
