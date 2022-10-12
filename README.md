1.	整体感知  该MatchingService使用Golang编写，提供了http api以提供对已有表信息的匹配查询。该项目的代码设计6个文件，行数大概是600行（含注释，此外另有100行单元测试内容）。在实现过程中，由于本身没有使用过于复杂的算法，所以将重点放在了代码规范性、设计模式和错误检查上。


2.	运行方式  在合适的go环境下（开发时使用的是go 1.18），该demo可以轻松地编译以及运行。其中，服务端信息表的来源是MatchingService/dict.csv；成功运行之后，程序提供一个可以从localhost访问的API，端口号为9527；成功查询之后的返回结果是一个带有列标题的数据集的CSV文件。在使用中，可以通过修改dict.csv以及更换命令进行检验。

3.	规范以及要求
  1.	针对数据源CSV文件，要求：a)	至少有一行列标题和一行数据b)	标题列的内容仅含有字母和数字c)	每一行都是完备的（每一行有相同长度），否则，日志打印出具体的错误信息并退出程序。
  2.	针对query的内容。有效的http请求格式如：127.0.0.1:9527/?query=C == "c1" or C %26= "c"，其中Query为C == "c1" or C %26= "c"。我们要求：a)	Query的每一个元素均由空格分隔b)	value的部分必须由双引号包裹起来c)	信息需要完整，否则，停止处理该请求，并返回错误提示信息，如 near XXX。


4.待改进项
    a)	更完善的错误提示方式 b)	改进算法。可以引入倒排索引的概念，记录数据表中每个数据对应的行列坐标，从而提高某些情况下的处理效率。当然，如果是数据相似度比较高的情况下，使用倒排索引的匹配方式可能  反倒不如遍历，针对这一点，可以在初始化的时候引入相似度指标，用以评估使用何种搜索方式 c)	计算方式的修改。如果在实际使用场景中，每一次查询中包含的query数量比较多，我们可以采用分布式计算的方式，比如MapReduce。