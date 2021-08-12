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
