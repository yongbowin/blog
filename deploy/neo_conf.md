## ubuntu部署neo4j（需要JDK的支持）

下载neo4j的linux文件： `neo4j-community-3.2.1-unix.tar.gz`  
对上边的文件进行解压，然后修改 `conf/neo4j-server.properties` 配置文件，将 `org.neo4j.server.webserver.address=0.0.0.0` 注释字符去掉

* 先安装python

```
sudo apt install python      安装python2.7
apt-get install python-pip      安装pip
sudo pip install neo4j-driver      安装neo4j的python插件
sudo pip install pandas      比较慢（使用下边已经配置好的pandas）
```

* 在虚拟机的同一个账号下，加载其他人的配置

```
source ~/.bashrc
source activate python2
conda install pandas
```

* 开启neo4j服务：
在neo4j的bin目录下执行

```
./neo4j console
```

* 然后将数据装填到neo4j中：（下边是python代码）

```
# coding: utf-8
import requests
import pandas as pd
import json
from neo4j.v1 import GraphDatabase, basic_auth
driver = GraphDatabase.driver("bolt://localhost:7687", auth=basic_auth("neo4j", "123456"))
session = driver.session()
insert = "MERGE(a:展示行业{0})-[rel:{1}]->(b:展示行业{2})"
data = pd.read_csv("222.csv").values
for line in data:
  sub, rel, obj = line
  session.run(insert.format("{name:'"+sub+"'}",rel+"{rel:'"+rel+"'}","{name:'"+obj+"'}"))
```

* 在浏览器中查看装填的内容：

```
http://192.168.100.251:7474
neo4j的初始账号是：neo4j   密码是：neo4j      （需要在浏览器中打开进行登录和修改）
```

* 清空neo4j中所有数据：

```
match (n)
detach delete n
```
