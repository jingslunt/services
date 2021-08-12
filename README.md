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
- java后端服务批量关闭，启动，重启脚本

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
