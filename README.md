# 基于elasticsearch5.6 #

### elasticsearch 安装 ###
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

### elasticsearch hq插件 ###
* 项目地址[elasticsearch-HQ](https://github.com/royrusso/elasticsearch-HQ)
