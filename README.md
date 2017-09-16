# 基于elasticsearch5.6集群 #

### elasticsearch 单机安装 ###
* 官网[下载](https://www.elastic.co/downloads/elasticsearch)
* curl https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.6.0.zip
* tar -zxvf elasticsearch-5.6.0.zip -C d:/
* mv elasticsearch-5.6.0 es5.6
* cd es5.6/bin
* elasticsearch.bat
* curl http://localhost:9200/

![](https://github.com/scalad/Elasticsearch/blob/master/doc/image/elasticsearch.png)

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
 

### elasticsearch hq插件 ###
* 项目地址[elasticsearch-HQ](https://github.com/royrusso/elasticsearch-HQ)
