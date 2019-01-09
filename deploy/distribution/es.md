## Elasticsearch全文搜索引擎

#### 安装过程

* 1.下载安装es的分词工具ik，并自定义字典
在github下载和es对应的插件,地址在[这里](https://github.com/medcl/elasticsearch-analysis-ik)

然后根据上边步骤进行安装（每个子节点上都需要安装！！！）

* 2.修改analysis-ik的用户和用户组为es
修改目录是： `/usr/share/elasticsearch/plugins/analysis-ik`  
将 `/etc/elasticsearch` 下的analysis-ik文件cp到 `/usr/share/elasticsearch/config` 中，然后修改用户和用户组为es

#### 关于Elasticsearch的一些理解

Elasticsearch与关系数据的类比对应关系如下：

```
Relational DB ⇒ Databases ⇒ Tables ⇒ Rows ⇒ Columns
Elasticsearch ⇒ Indices ⇒ Types ⇒ Documents ⇒ Fields
```

可以参考[知乎文章](https://www.zhihu.com/question/26446020?sort=created)
