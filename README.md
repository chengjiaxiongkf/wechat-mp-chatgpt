# wechat-mp-chatgpt
微信公众号自动回复接入chatgpt开放AP,由于微信官方限制回复消息接口响应必须在5秒内，但是Chatgpt是境外请求，部署的服务没有CDN的情况延时较高，有可能回复不到微信公众号内。

### 获取你的Chatgpt的开放API https://platform.openai.com/account/api-keys
### 在下面配置API_KEY并以docker启动封装了Chatgpt开放API的接口服务(请在国外服务器或者具备科学上网的服务器)
```shell
docker run -p 9001:8080 -e API_KEY=xxx -e -d -v $PWD/log:/app/log tomatocuke/openai
echo "http://127.0.0.1:9001/test?msg=你是谁"
```

## Web版
### 在本JAVA项目中根目录的index.html文件中配置上面chatgpt接口服务的接口url，把这个index.html放置到Nginx静态资源目录下。

## 微信公众号版
### 在本JAVA项目中application.yml中配置上面chatgpt接口服务的接口url，以及公众号信息
```yaml
api:
  chatgpt:
    url: https://xxx.xxx.xxx.xx/test?msg=
wx:
  mp:
    useRedis: false
    redisConfig:
      host: 127.0.0.1
      port: 6379
      password:
    configs:
      - appId: xxx # 第一个公众号的appid
        secret: xxx # 公众号的appsecret
        token: xxx # 接口配置里的Token值
        aesKey: xxx # 接口配置里的EncodingAESKey值
```

### 公众号配置相关问题
1. 登陆微信公众平台https://mp.weixin.qq.com

2. 在菜单[设置与开发]-[基本配置]的服务器配置中预先填好token跟aesKey

3. 在本项目的yml中配置好这些信息先部署到服务器上，验证本项目部署在服务器上的http://xxx.xxx.xxx.x/wx/portal/{appId} 接口能正常返回时后再填写到[服务器配置]的url输入框提交。

4. 消息加解密方式可以先选明文模式，有需要自己再改。
