## 版本信息

Ambari：2.7.4

HDP：3.1.4

HUE：4.6.0

## 环境准备

1.hue的master节点上执行，为编译环境做准备

```shell
yum install sqlite-devel  libxslt-devel.x86_64 python-devel openldap-devel asciidoc cyrus-sasl-gssapi  libxml2-devel.x86_64 mysql-devel gcc gcc-c++ kernel-devel openssl-devel gmp-devel libffi-devel install npm
```

2.所有机器上创建用户和组

```shell
useradd -g hue hue
```

3.提前在mysql创建好hue的库并授权

```sql
CREATE DATABASE hue;
GRANT ALL PRIVILEGES ON hue.* TO hue@'%' IDENTIFIED BY '123456';
FLUSH PRIVILEGES;
```

4.提前建好hue在hdfs的HOME目录

```shell
hadoop fs -mkdir /user/hue
hadoop fs -chown hue:hue /user/hue
```

5.下载插件源码

在ambari server节点执行

```shell
VERSION=`hdp-select status hadoop-client | sed 's/hadoop-client - \([0-9]\.[0-9]\).*/\1/'`
rm -rf /var/lib/ambari-server/resources/stacks/HDP/$VERSION/services/HUE  
sudo git clone https://github.com/Gqyanxin/ambari-hue-service.git /var/lib/ambari-server/resources/stacks/HDP/$VERSION/services/HUE
```

6.hue的安装包并放到你的Apache服务器上




在ambari server节点执行

## 代码修改

1. package/files/configs.sh文件

   ```
   USERID='ambari的管理员账号'
   PASSWD='ambari的管理员密码'
   ```

2. package/scripts/params.py文件

   第32行 download_url 改成你自己的地址，可以跟hdp的本地仓库放一起

   第40行 ambari_server_hostname 改成你自己的地址

## 部署安装

重启ambari

```shell
ambari-server restart
```

ambari界面操作

界面左侧 >> services >> Add service >> Hue >> NEXT >> 选择Hue Server >> NEXT >> 配置 

数据库配置，这里选了mysql：

![1583732152511](https://github.com/lijufeng2016/ambari-hue-service/blob/master/screenshots/1583732152511.png)

![1583732214223](https://github.com/lijufeng2016/ambari-hue-service/blob/master/screenshots/1583732214223.png)



## 编译

```shell
cd /usr/hdp/3.1.5.0-152/hue/
make apps
```

**注意：在准备工作的第一步的包必须安装才能编译成功**



打开主页面，输入hue hue

![1583737992211](https://github.com/lijufeng2016/ambari-hue-service/blob/master/screenshots/1583737992211.png)
