### elasticsearch，logstash，mysql同步

### 安装前的注意：
```
1. 版本需要都一样；
2. 使用非root进行安装启动；
3. 系统可用内存 >2G；
4. java 版本 大于 1.8；
```


1. 部署elasticsearch

```
1). 官网下载：https://www.elastic.co/downloads/elasticsearch；
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-8.1.2-linux-x86_64.tar.gz；
2). tar -zxvf elasticsearch-8.1.3-linux-x86_64.tar.gz；
3). 修改config/elasticsearch.yml中xpack.security.enabled，由true，改为false；
4). 运行./bin/elasticsearch，启动服务；
5). 使用curl 127.0.0.1:9200，确定elasticsearch是否安装成功；
6). 在plugins/下创建ik目录，下载https://github.com/medcl/elasticsearch-analysis-ik/releases；
wget https://github.com/medcl/elasticsearch-analysis-ik/releases/download/v8.1.2/elasticsearch-analysis-ik-8.1.2.zip；
7). unzip ik分词器包，即可；
```

2. 部署logstash

```
1). 官网下载：https://www.elastic.co/downloads/logstash；
wget https://artifacts.elastic.co/downloads/logstash/logstash-8.1.2-linux-x86_64.tar.gz；
2). 为了实现 MySQL 数据的同步，我们还需要下载 mysql-connector：
wget https://repo1.maven.org/maven2/mysql/mysql-connector-java/8.0.18/mysql-connector-java-8.0.18.jar；
3). 在解压目录中创建job目录，添加logstash-mysql.conf文件，添加内容；
4). 执行./bin/logstash -f job/logstash-mysql.conf；
5). 使用：curl 'localhost:9200/cloud_platform/_search'，确定数据是否同步；
```

```
#mysql.conf内容：

input {
    stdin {
    }
    jdbc {
      # 数据库  数据库名称为elk，表名为book_table
      jdbc_connection_string => "jdbc:mysql://127.0.0.1:3306/cloud_platform"
      # 用户名密码
      jdbc_user => "root"
      jdbc_password => "123456"
      # jar包的位置
      jdbc_driver_library => "/www/tool/logstash-8.1.3/logstash-core/lib/jars/mysql-connector-java-8.0.18.jar"
      # mysql的Driver
      jdbc_driver_class => "com.mysql.jdbc.Driver"
      jdbc_paging_enabled => "true"
      jdbc_page_size => "50000"
      #statement_filepath => "config-mysql/book.sql"
      statement => "select * from cp_user"
      schedule => "* * * * *"
	  #索引的类型
      type => "cp_user"
    }
}

#filter {
#    json {
#        source => "message"
#        remove_field => ["message"]
#    }
#}

output {
    elasticsearch {
        hosts => "127.0.0.1:9200"
        # index名
		index => "cloud_platform"
		# 需要关联的数据库中有有一个id字段，对应索引的id号
	document_type =>"cp_user"
        document_id => "%{id}"
    }
    stdout {
        codec => json_lines
    }
}
```

3. 查看es服务器健康状况
```
curl 'localhost:9200/_cat/health?v'
status：
green：每个索引的primary shard和replica shard都是active状态的
yellow：每个索引的primary shard都是active状态的，但是部分replica shard不是active状态，处于不可用的状态
red：不是所有索引的primary shard都是active状态的，部分索引有数据丢失了
```

4. 查看es服务器有哪些索引
```
curl 'localhost:9200/_cat/indices?v'
```

5. laravel，mysql数据同步es
```
参考：https://www.jianshu.com/p/4e0f4ed06af4
```

6. 名次类比mysql：
```
索引 (index) = 数据库（Database）；
类型 (type) = 表（Table）；
文档(document) = 记录（Row）；
字段 (field) = 字段（Column）；
```
