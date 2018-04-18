### ElasticSearch负载不均衡 ###
某天晚上，服务器告警，发现服务器接口出现很多400，初步定为是ES服务器CPU达到了98%以上，ES日志出现了很多错误日志，具体如下：

![](https://github.com/scalad/Elasticsearch/blob/master/doc/clusterLoadNotBalance/images/error.png?raw=true)

参考[https://blog.csdn.net/wwd0501/article/details/78399943](https://blog.csdn.net/wwd0501/article/details/78399943)也就是Elasticsearch在并发查询量大的情况下，访问流量超过了集群中单个Elasticsearch实例的处理能力，Elasticsearch服务端会触发保护性的机制，拒绝执行新的访问，并且抛出EsRejectedExecutionException异常

![](https://github.com/scalad/Elasticsearch/blob/master/doc/clusterLoadNotBalance/images/balance.png?raw=true)

#### (1)	primary shard主副分片分布不均 ####
primary true代表的是主数据，检索的时候从该分片拿，false是备份的副本，是做容错时的冗余数据。短信告警也是从10.13.95.228主节点开始而后10.23.28.177、10.13.28.197和10.13.130.248这几台出现的。

从上图看出两个集群中有三个节点中没有primaryshard,下图是第二个集群在傍晚时期CPU的使用率，后面两台是上面CPU都是0的node-4和node-6节点，也就是这两台完全没有计算能力，只是长期备份数据。

![](https://github.com/scalad/Elasticsearch/blob/master/doc/clusterLoadNotBalance/images/server1.png?raw=true)

![](https://github.com/scalad/Elasticsearch/blob/master/doc/clusterLoadNotBalance/images/server2.png?raw=true)

