# spray-socketio学习笔记

scala的生态环境与java的生态有许多不同之处，通过对一些项目的理解及学习，理解akka的通用技术及应用技巧，以及对于akka库的一些常用场景。对于搭建 scala的通用分布式平台相关技术进行深入理解。从而可以在实时运算、通用计算平台、分布式相关组件及技术等方面有全方位的理解。

相对而言，scala的分布式生态环境因为Actor模型的存在，而简化了大量的中间组件及运行模型。通过使用同一种运算模型来取代java中的诸多模式及相关中间件，可以起到很好的简化作用。相对而言，效率更高，稳定性更好、程序实现也更快更简单。同时，因为减少了相关中间件技术，也减少了出错的机率。

spray-socketio是邓草原的相关作品，分析大师的作品对于自己的代码能力及架构思维都有非常好的帮助。

###分析方法

按照包的层次来进行分析,同时，对于包中所涉及到的akka的相关知识点会详细分析，对于其中使用到的不懂的技术会专门提出来，以待后期解答。 重点是相关流程及处理技巧。


