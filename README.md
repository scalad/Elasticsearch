# 基于elasticsearch5.6集群 #

### Required ###
* JDK1.8+

### elasticsearch 单机安装 ###
* 官网[下载](https://www.elastic.co/downloads/elasticsearch)
* curl https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.6.0.zip
* tar -zxvf elasticsearch-5.6.0.zip -C d:/
* mv elasticsearch-5.6.0 es5.6
* cd es5.6/bin
* elasticsearch.bat
* curl http://localhost:9200/

![](https://github.com/scalad/Elasticsearch/blob/master/doc/image/elasticsearch.png)

### 安装NodeJS ###
ElasticSearch5之前的版本安装Head插件只需要在`bin`目录下执行`./plugin -install mobz/elasticsearch-head`就可以完成安装，5之后改用Node安装运行，所以你需要先安装NodeJS,下面是Window环境：

* [下载你NodeJS](https://nodejs.org/en/)
* 或者 `wget https://nodejs.org/dist/v6.11.3/node-v6.11.3-x64.msi`
* 执行安装程序
* 安装完NodeJS后默认已经安装了npm，NodeJS包管理和分发工具


### elasticsearch head 插件 ###
* 项目地址[elasticsearch-head](https://github.com/mobz/elasticsearch-head)
* git clone git://github.com/mobz/elasticsearch-head.git
* cd elasticsearch-head
* npm install
* npm run start
* open http://localhost:9100/

修改elasticsearch安装目录下的config/elasticsearch.yml文件，在末尾加入以下两行解决跨域请求问题

	http.cors.enabled: true
	http.cors.allow-origin: "*"

![](https://github.com/scalad/Elasticsearch/blob/master/doc/image/head.png)

### elasticsearch 集群安装 ###
* mv es5.6 es5.6-1
* cp -r es5.6-1 es5.6-2
* cp -r es5.6-1 es5.6-3
* cp -r es5.6-1 es5.6-4
* cp -r es5.6-1 es5.6-5

> 要是部署的时候从一个节点复制elasticsearch文件夹，其他节点可能包含被复制节点的data文件数据，需要把data文件下的文件清空

es5.6-1`elasticsearch.yml`配置文件

	cluster.name: elasticsearch
	node.master: true
	node.name: node-1
	path.data: ./../data
	path.logs: ./../logs
	network.host: 172.28.3.34
	http.port: 9200
	transport.tcp.port: 9300
	discovery.zen.ping.unicast.hosts: ["172.28.3.34:9300"]
	http.cors.enabled: true
	http.cors.allow-origin: "*"

es5.6-2`elasticsearch.yml`配置文件

	cluster.name: elasticsearch
	node.master: false
	node.name: node-2
	path.data: ./../data
	path.logs: ./../logs
	network.host: 172.28.3.34
	http.port: 9201
	transport.tcp.port: 9301
	discovery.zen.ping.unicast.hosts: ["172.28.3.34:9300"]
	http.cors.enabled: true
	http.cors.allow-origin: "*"

es5.6-3`elasticsearch.yml`配置文件

	cluster.name: elasticsearch
	node.master: false
	node.name: node-3
	path.data: ./../data
	path.logs: ./../logs
	network.host: 172.28.3.34
	http.port: 9202
	transport.tcp.port: 9302
	discovery.zen.ping.unicast.hosts: ["172.28.3.34:9300"]
	http.cors.enabled: true
	http.cors.allow-origin: "*"

es5.6-4`elasticsearch.yml`配置文件

	cluster.name: elasticsearch
	node.master: false
	node.name: node-4
	path.data: ./../data
	path.logs: ./../logs
	network.host: 172.28.3.34
	http.port: 9203
	transport.tcp.port: 9303
	discovery.zen.ping.unicast.hosts: ["172.28.3.34:9300"]
	http.cors.enabled: true
	http.cors.allow-origin: "*"

es5.6-5`elasticsearch.yml`配置文件

	cluster.name: elasticsearch
	node.master: false
	node.name: node-5
	path.data: ./../data
	path.logs: ./../logs
	network.host: 172.28.3.34
	http.port: 9204
	transport.tcp.port: 9304
	discovery.zen.ping.unicast.hosts: ["172.28.3.34:9300"]
	http.cors.enabled: true
	http.cors.allow-origin: "*"

* ./es5.6-1/bin/elasticsearch.bat
* ./es5.6-2/bin/elasticsearch.bat
* ./es5.6-3/bin/elasticsearch.bat
* ./es5.6-4/bin/elasticsearch.bat
* ./es5.6-5/bin/elasticsearch.bat
* http://172.28.3.34:9100
* 创建个索引，2个副本

![](https://github.com/scalad/Elasticsearch/blob/master/doc/image/cluster.png)

### Elasticsearch-analysis-ik 中文分词器 ###
[elasticsearch-analysis-ik](https://github.com/medcl/elasticsearch-analysis-ik)
把Lucene的[IK analyzer](http://code.google.com/p/ik-analyzer/)中文分词器集成进了ElasticSearch，支持自定义的目录

#### 安装 ####
1.根据你的elasticsearch的版本在[这里](https://github.com/medcl/elasticsearch-analysis-ik/releases)下载相应版本的Elasticsearch-analysis-ik，注意版本要对应，否则安装不上，解压到你elasticsearch文件下下的/plugins/文件夹下
> 如果的elasticsearch版本大于5.5.1，则可以使用下面的命令安装，但是版本也要对应上
> ./bin/elasticsearch-plugin install https://github.com/medcl/elasticsearch-analysis-ik/releases/download/v5.6.2/elasticsearch-analysis-ik-5.6.2.zip

2.重启你的Elasticsearch

关于ElasticSearch热更新IK分词请参考[热更新 IK 分词使用方法](https://github.com/medcl/elasticsearch-analysis-ik)



### elasticsearch hq插件 ###
Elastic HQ提供一个Web应用程序来管理和监控ElasticSearch实例与集群管理和监控。具有良好体验、直观和功能强大的 ElasticSearch 的管理和监控工具。提供实时监控、全集群管理、搜索和查询，项目地址[elasticsearch-HQ](https://github.com/royrusso/elasticsearch-HQ)

### Marvel插件 ###
Marvel是Elasticsearch的管理和监控工具，对于开发使用免费的。它配备了一个叫做Sense的交互式控制台，方便通过浏览器直接与Elasticsearch交互。

### Bigdesk插件 ###
bigdesk是elasticsearch的一个集群监控工具，可以通过它来查看es集群的各种状态，如：cpu、内存使用情况，索引数据、搜索情况，http连接数等。项目git地址： [https://github.com/lukas-vlcek/bigdesk](https://github.com/lukas-vlcek/bigdesk)。和head一样，它也是个独立的网页程序，使用方式和head一样。不过可惜的是它支持的版本教老，也已经不更新了。

### About ###
* [关于ElasticSearch](https://github.com/scalad/Elasticsearch/blob/master/doc/aboutElasticSearch)
* [关于多个ElasticSearch集群的数据一致性](https://github.com/scalad/Elasticsearch/blob/master/doc/ClusterDataAccordance)
* [关于Elasticdump导出ElasticSearch索引数据](https://github.com/scalad/Elasticsearch/blob/master/doc/Elasticdump)
* [关于使用sql语句查询ElasticSearch](https://github.com/NLPchina/elasticsearch-sql)