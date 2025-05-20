## Dify 1.0 Plugin Database Query Tools


**Author:** [Junjie.M](https://github.com/junjiem)   
**Type:** tool  
**Github Repo:** [https://github.com/junjiem/dify-plugin-tools-dbquery](https://github.com/junjiem/dify-plugin-tools-dbquery)  
**Github Issues:** [issues](https://github.com/junjiem/dify-plugin-tools-dbquery/issues)


---


### Demonstration

Database Query Tools

数据库查询工具

Currently supported database types: mysql, oracle, [oracle11g](#2-how-to-connect-to-oracle-11g--如何连接oracle-11g), postgresql, or mssql.

目前支持的数据库类型：mysql、oracle、[oracle11g](#2-how-to-connect-to-oracle-11g--如何连接oracle-11g)、postgresql、mssql。

![db_query](_assets/db_query.png)

![db_query_sql_query](_assets/db_query_sql_query.png)

![db_query_chatflow](_assets/db_query_chatflow.png)



---



### Examples 示例

- [完蛋！我被LLM包围了！（Dify1.0战绩排行版）](https://github.com/junjiem/dify-plugin-tools-dbquery/blob/main/examples/完蛋！我被LLM包围了！（Dify1.0战绩排行版）.yml)

- [完蛋！我被LLM包围了！（Dify1.0战绩排行+留言版）](https://github.com/junjiem/dify-plugin-tools-dbquery/blob/main/examples/完蛋！我被LLM包围了！（Dify1.0战绩排行+留言版）.yml)


![](_assets/llm_riddles1.png)

![](_assets/llm_riddles2.png)

![](_assets/llm_riddles3.png)



---



### FAQ

#### 1. How to Handle Errors When Installing Plugins? 安装插件时遇到异常应如何处理？

**Issue**: If you encounter the error message: plugin verification has been enabled, and the plugin you want to install has a bad signature, how to handle the issue?

**Solution**: Add the following line to the end of your .env configuration file: FORCE_VERIFYING_SIGNATURE=false
Once this field is added, the Dify platform will allow the installation of all plugins that are not listed (and thus not verified) in the Dify Marketplace.

**问题描述**：安装插件时遇到异常信息：plugin verification has been enabled, and the plugin you want to install has a bad signature，应该如何处理？

**解决办法**：在 .env 配置文件的末尾添加 FORCE_VERIFYING_SIGNATURE=false 字段即可解决该问题。
添加该字段后，Dify 平台将允许安装所有未在 Dify Marketplace 上架（审核）的插件，可能存在安全隐患。


#### 2. How to connect to oracle 11g  如何连接Oracle 11g

2.1、下载 oracle11g 的 client，这里下的是 11.2.0.4.0 版本

https://www.oracle.com/database/technologies/instant-client/downloads.html

比如：instantclient-basic-linux.x64-11.2.0.4.0.zip


2.2、上传 oracle 的 client 到宿主机的 dify 的挂载目录

将 instantclient-basic-linux.x64-11.2.0.4.0.zip 解压后的 instantclient_11_2 目录放到 docker/volumes 下面
> 注：需要将 `instantclient_11_2` 中`libclntsh.so.11.1` 改成 `libclntsh.so`、 `libocci.so.11.1` 改成 `libocci.so`

2.3、在 `docker/docker-compose.yml` 中进行配置
```
# 在 plugin_daemon 下，添加一个变量
environment:
  ......
  LD_LIBRARY_PATH: "/root/instantclient_11_2:$LD_LIBRARY_PATH"
```

Red Hat 系（如 CentOS、RHEL）64 位：
```
# 在 plugin_daemon 下，添加三个挂载
volumes:
  ......
  - ./volumes/instantclient_11_2:/root/instantclient_11_2
  - /usr/lib64/libaio.so.1.0.1:/usr/lib/x86_64-linux-gnu/libaio.so.1.0.1
  - /usr/lib64/libaio.so.1:/usr/lib/x86_64-linux-gnu/libaio.so.1
```

Debian 系（如 Ubuntu）64 位：
```
# 在 plugin_daemon 下，添加三个挂载
volumes:
  ......
  - ./volumes/instantclient_11_2:/root/instantclient_11_2
  - /usr/lib/x86_64-linux-gnu/libaio.so.1.0.1:/usr/lib/x86_64-linux-gnu/libaio.so.1.0.1
  - /usr/lib/x86_64-linux-gnu/libaio.so.1:/usr/lib/x86_64-linux-gnu/libaio.so.1
```

2.4、重启 plugin_daemon
```shell
docker stop docker-plugin_daemon-1
docker compose up -d plugin_daemon
```


#### 3. How to install the offline version 如何安装离线版本

Scripting tool for downloading Dify plugin package from Dify Marketplace and Github and repackaging [true] offline package (contains dependencies, no need to be connected to the Internet).

从Dify市场和Github下载Dify插件包并重新打【真】离线包（包含依赖，不需要再联网）的脚本工具。

Github Repo: https://github.com/junjiem/dify-plugin-repackaging

