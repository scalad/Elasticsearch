### ElasticSearch负载不均衡 ###
某天晚上，服务器告警，发现服务器接口出现很多400，初步定为是ES服务器CPU达到了98%以上，ES日志出现了很多错误日志，具体如下：

![](https://github.com/scalad/Elasticsearch/blob/master/doc/clusterLoadNotBalance/images/error.png?raw=true)

参考[https://blog.csdn.net/wwd0501/article/details/78399943](https://blog.csdn.net/wwd0501/article/details/78399943)也就是Elasticsearch在并发查询量大的情况下，访问流量超过了集群中单个Elasticsearch实例的处理能力，Elasticsearch服务端会触发保护性的机制，拒绝执行新的访问，并且抛出EsRejectedExecutionException异常，通过观察ES集群发现了下面的几个问题:

![](https://github.com/scalad/Elasticsearch/blob/master/doc/clusterLoadNotBalance/images/balance.png?raw=true)

[为什么会出现这个问题呢？](http://grokbase.com/t/gg/elasticsearch/14121arzf8/load-balancing-and-a-node-with-no-primary-shard)通过这里面的步骤可以很容易的复现出这个问题。

#### (1)	primary shard主副分片分布不均 ####
primary true代表的是主数据，检索的时候从该分片拿，false是备份的副本，是做容错时的冗余数据。短信告警也是从10.13.95.228主节点开始而后10.23.28.177、10.13.28.197和10.13.130.248这几台出现的。

从上图看出两个集群中有三个节点中没有primaryshard,下图是第二个集群在傍晚时期CPU的使用率，后面两台是上面CPU都是0的node-4和node-6节点，也就是这两台完全没有计算能力，只是长期备份数据。

![](https://github.com/scalad/Elasticsearch/blob/master/doc/clusterLoadNotBalance/images/server1.png?raw=true)

![](https://github.com/scalad/Elasticsearch/blob/master/doc/clusterLoadNotBalance/images/server2.png?raw=true)

#### (2)	master node既是master node又是data node ####
master node既要做数据检索，也要做集群的负载均衡转发器，导致每个集群的master node的CPU都很高，因此每次告警首先都是master node

### 解决方案 ###
#### (1)	方案一：手动移动分片 ####
![](https://github.com/scalad/Elasticsearch/blob/master/doc/clusterLoadNotBalance/images/move.png)

例如移动node-1的分片0到node-4

```shell
curl -XPOST 'http://localhost:9200/_cluster/reroute' -d '{
  "commands":[{
  "move":{
    "index":"indexName",
    "shard":0,
    "from_node":"node-1",
    "to_node":"node-4"
}}]}'
```
优点：操作简单，恢复时间短；不必修改master node的配置，master node长期负载后高

缺点：索引大，移动时有很高的IO，索引容易损坏，需要做备份，不能解决master node既是数据节点又是负载均衡转发器的问题

注意：分片和副本无法移动到同一个节点

#### (2)	方案二：重建索引，从另外一个集群导入 ####
删除原来的索引，重新建立索引，；利用[elasticsearch dump](https://github.com/taskrabbit/elasticsearch-dump)等工具从另一个集群中把数据导入到新的索引中

优点：可以重新配置master node和data node，主从负载均匀

缺点：费时间，容易数据丢失，需要验证数据的一致性

#### 开始操作 ####

经过讨论，我们选了趁着月黑风高，我们选择了第一个方案，因为第一个方案操作起来比较简单，有经过本地的测试，风险也比较小。

[Too many shards on node for attribute: [rack], required per attribute: [1], node count: [2], leftover: [0]](https://stackoverflow.com/questions/44799045/too-many-shards-on-node-for-attribute-rack-required-per-attribute-1-node)
