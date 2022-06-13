# SolrCloud

​	Solr可以搭建具备容错能力和高可用的Solr集群。集群中集群配置、自动负载均衡和查询故障转移、			Zookeeper集群实现集群协调管理，这些全部功能统称为SolrCloud。

​	SolrCloud是基于Zookeeper进行管理的。在Solr中已经内置了Zookeeper相关内容，当执行集群创建命令会自动创建Zookeeper相关内容。这个使用的是Zookeeper的集群管理功能实现的。

## 1.搭建

### 1.1创建

​	SolrCloud已经包含在了Solr中，可以直接启动Solr集群。

```
 ./solr -e cloud -noprompt -force
```

​	此命令等同于# ./solr -e cloud -force全部参数为默认值。

​	运行成功后会在example文件夹多出cloud文件夹。

### 1.2停止

```
 ./solr stop -all
```



### 1.3重新运行

```
 ./solr start -c -p 8983 -s ../example/cloud/node1/solr/ -force
 ./solr start -c -p 7574 -z localhost:9983 -s ../example/cloud/node2/solr/ -force
```



