## OC Services
### 部署单服务应用

#### 使用
- Consul 
```shell 
{
curl -o installConsulAlone  -sL https://github.com/jingslunt/services/releases/download/1.0/installConsulAlone
chmod +x installConsulAlone
./installConsulAlone
}
```

- Rabbitmq 

```shell
{
curl -o installRabbitmqAlone -sL https://github.com/jingslunt/services/releases/download/1.0/installRabbitmqAlone
chmod +x installRabbitmqAlone
./installRabbitmqAlone
}
```

- Redis
```shell
{
curl -o installRedisAlone -sL https://github.com/jingslunt/services/releases/download/1.0/installRedisAlone
chmod +x installRedisAlone
./installRedisAlone
}
```

- 通过app停止脚本生成ansible使用的停止脚本
```
curl -o auto_transSH_strip  -sL https://github.com/jingslunt/services/releases/download/1.0/auto_transSH_strip
```
- java后端服务批量生成关闭，启动，重启脚本

> ############################################ 
>
> ##java后端服务批量关闭，启动，重启脚本 
>
> ##需要调用message，port两个文件，文件不存在不执行 
>
> ##重启关闭调用开发放置的/opt/start/关闭启动脚本 
>
> ##需保证运维服务器可以免密管理java后端服务器 
>
> ##问题联系：OC Jngs  2021-07-07 
>
> ########################################### 
 

```
curl -o webServiceRestart_strip  -sL https://github.com/jingslunt/services/releases/download/1.0/webServiceRestart_strip
```

- 生成zabbix的web模板
下载
```shell
{
curl -o createTemplateScript -sL https://github.com/jingslunt/services/releases/download/1.0/createTemplateScript
chmod +x createTemplateScript
./createTemplateScript
}
```
使用

```
[root@k8s-master-node1 zabbix]# ./createTemplateScript 
格式:./createTemplateScript [可选参数]

	可选参数为:
            -f             调用的服务配置
            -o             生成的xml文件
            -a             产品地区，比如mc-cn
            -h             使用帮助
 例如: ./createTemplateScript -a mc-cn -f port -o mc-cn-web-template.xml

```

- 生成zabbix的hosts xml导入脚本
下载
```shell
{
curl -o zbxCreateHosts -sL https://github.com/jingslunt/services/releases/download/1.0/zbxCreateHosts
chmod +x zbxCreateHosts
./zbxCreateHosts
}
```
使用

```
[root@k8s-master-node1 zabbix]# ./zbxCreateHosts
说明：
############################################
## zabbix生成主机脚本
## 需要调用多配置文件，文件不存在不执行
## 需优先在zabbix服务端配置好指定agent代理程序
## 问题联系：OC林锦华  2021-08-27
###########################################     

格式:./zbxCreateHosts [可选参数]

	可选参数为:
            -f             调用的服务配置，比如mysql=1.2.1.1 1.2.1.2
            -s             调用的IP主机名，比如1.2.1.1  test-tw-db-01-01
            -o             生成的xml文件名
            -a             产品地区，比如test-tw
            -p             指定zabbix proxy服务端名
            -h             使用帮助
        例如: ./zbxCreateHosts -a test-tw -p test-tw-proxy -s hostname -f services.conf -o test-tw-hosts.xml


```

