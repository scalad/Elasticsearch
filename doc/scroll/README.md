### Elasticsearch Scroll API详解 ###

#### 1. 相关数据库知识（帮助理解）####
传统数据库游标：游标（cursor）是系统为用户开设的一个数据缓冲区，存放SQL语句的执行结果。每个游标区都有一个名字,用户可以用SQL语句逐一从游标中获取记录，并赋给主变量，交由主语言进一步处理。就本质而言，游标实际上是一种能从包括多条数据记录的结果集中每次提取一条记录的机制。

游标是一段私有的SQL工作区,也就是一段内存区域,用于暂时存放受SQL语句影响到的数据。通俗理解就是将受影响的数据暂时放到了一个内存区域的虚表中，而这个虚表就是游标。

#### 2. 为什么使用Elasticsearch Scroll ####
当Elasticsearch响应请求时，它必须确定docs的顺序，排列响应结果。如果请求的页数较少（假设每页20个docs）, Elasticsearch不会有什么问题，但是如果页数较大时，比如请求第20页，Elasticsearch不得不取出第1页到第20页的所有docs，再去除第1页到第19页的docs，得到第20页的docs。

解决的方法就是使用Scroll。因为Elasticsearch要做一些操作（确定之前页数的docs）为每一次请求，所以，我们可以让Elasticsearch储存这些信息为之后的查询请求。这样做的缺点是，我们不能永远的储存这些信息，因为存储资源是有限的。所以Elasticsearch中可以设定我们需要存储这些信息的时长。

#### 3. 如何使用 Elasticsearch Scroll ####
我们只需在普通的query后加上scroll的参数例如： curl -XGET localhost:9200/bank/account/_search?pretty&scroll=2m -d {"query":{"match_all":{}}} 其中“2m” 代表2分钟，是需要保存的时长（数字+单位，具体单位表示见表1）。

Table 1. 时间单位对照表

    Time	Units
    y	Year
    M	Month
    w	Week
    d	Day
    h	Hour
    m	Minute
    s	Second

上面的查询语句返回如下结果:

    {
      "_scroll_id": : " cXVlcnlUaGVuRmV0Y2g7NTs5MTM6aDEySHRHNVpScVNiN2VUZVV6QV9xdzs5MTQ6aDEySHRHNVpScVNiN2VUZVV6QV9xdzs5MTU6aDEySHRHNVpScVNiN2VUZVV6QV9xdzs5MTc6aDEySHRHNVpScVNiN2VUZVV6QV9xdzs5MTY6aDEySHRHNVpScVNiN2VUZVV6QV9xdzswOw==",
      "took" : 3,
      "timed_out" : false,
      "_shards" : {
        "total" : 5,
        "successful" : 5,
        "failed" : 0
        }…

