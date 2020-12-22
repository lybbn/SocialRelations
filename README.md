一:系统背景

本系统适合海量数据全文索引,你需要安装mysql等其他数据库事先存储要查询的数据(新输入先录入该数据库),然后由es同步数据库中的数据(有新数据可手动或自动更新)

服务器存储容量需要为数据本身至少2.5倍(es建立索引),需要python版本3.6+

二:使用方法

1:安装elasticsearch5.5.3(需要java版本11及以下,另外还需要下载es_ik中文分词插件)

elasticsearch官网地址:https://www.elastic.co/cn/downloads/elasticsearch
推荐国内加速下载地址:https://mirrors.huaweicloud.com/elasticsearch/5.5.3/

java加速下载推荐地址:https://repo.huaweicloud.com/java/jdk/  ,也可使用yum安装指定版本的java,yum -y install java-11-openjdk*

(1)优化内存:config/jvm.options 中修改占用内存大小(根据实际服务器内存容量调整)

-Xms2g
-Xmx2g

(2)有集群需求需配置config/elasticsearch.yml

2:使用virtualenv venv

3:使用pip安装python环境包 pip install -r requirements.txt

4:数据库相关

(1)生成数据库
python manage.py makemigrations
python manage.py migrate

说明:如果已存在数据库和表结构及数据,可反向生成model.py文件,使用这个models.py文件覆盖app中的models文件。如果完成了这个操作，生成的是一个不可修改/删除的models，修改meta class中的managed = True则可以去告诉django可以对数据库进行操作

python manage.py inspectdb 数据库中你想用的表格的名字 > models.py 这样会生成一个新的文件和manage.py同级目录

(2)重建es索引
python manage.py rebuild_index

说明:数据有更新的时候 python manage.py update_index更新索引

5:运行网站

python manage.py runserver 0.0.0.0:8000

5:根据上一步的提示网页访问web端口