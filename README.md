# 基于elasticsearch5.6 #

### elasticsearch 安装 ###
* 官网[下载](https://www.elastic.co/downloads/elasticsearch)
* curl https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.6.0.zip
* tar -zxvf elasticsearch-5.6.0.zip -C d:/
* mv elasticsearch-5.6.0 es5.6
* cd es5.6/bin
* elasticsearch.bat
* curl http://localhost:9200/

```JavaScript
    {
      "name" : "DGbaZ0U",
      "cluster_name" : "elasticsearch",
      "cluster_uuid" : "C2Lv4yU_T7SCH05fLgAlJQ",
      "version" : {
        "number" : "5.6.0",
        "build_hash" : "781a835",
        "build_date" : "2017-09-07T03:09:58.087Z",
        "build_snapshot" : false,
        "lucene_version" : "6.6.0"
      },
      "tagline" : "You Know, for Search"
    }
```

![](https://github.com/scalad/Elasticsearch/blob/master/doc/image/elasticsearch.png)

### elasticsearch head 插件 ###
* 项目地址[elasticsearch-head](https://github.com/mobz/elasticsearch-head)
* git clone git://github.com/mobz/elasticsearch-head.git
* cd elasticsearch-head
* npm install
* npm run start
* open http://localhost:9100/
![](https://github.com/scalad/Elasticsearch/blob/master/doc/image/head.png)

### elasticsearch hq插件 ###
* 项目地址[elasticsearch-HQ](https://github.com/royrusso/elasticsearch-HQ)
