# chatgpt-api-worker-proxy
完整的教程，如何中转api在受限制区域连接chatgpt的api

1.注册并登录，在右上角切换为简体中文。


2.左侧域注册，注册域,输入域名查找购买。便宜的域名差不多3刀到4刀一年，够使用很久了，free计划即可。
![链接](https://github.com/SnowfallC/chatgpt-api-worker-proxy/blob/main/examples/regdomain.png)
![链接](https://github.com/SnowfallC/chatgpt-api-worker-proxy/blob/main/examples/buy.png)

3.进入workers，创建一个服务，起个名字，选左边http程序，创建服务。free计划。
![链接](https://github.com/SnowfallC/chatgpt-api-worker-proxy/blob/main/examples/http.png)

4.在workers的概述中选择你刚创建的服务，点击快速编辑。
![链接](https://github.com/SnowfallC/chatgpt-api-worker-proxy/blob/main/examples/quickedit.png)

5.复制以下代码复制到左侧文本框(代码来自https://github.com/x-dr/chatgptProxyAPI）
![链接](https://github.com/SnowfallC/chatgpt-api-worker-proxy/blob/main/examples/left.png)

```shell
const TELEGRAPH_URL = 'https://api.openai.com';

addEventListener('fetch', event => {
  event.respondWith(handleRequest(event.request))
})

async function handleRequest(request) {
  const url = new URL(request.url);
  url.host = TELEGRAPH_URL.replace(/^https?:\/\//, '');
  const modifiedRequest = new Request(url.toString(), {
    headers: request.headers,
    method: request.method,
    body: request.body,
    redirect: 'follow'
  });
  const response = await fetch(modifiedRequest);
  const modifiedResponse = new Response(response.body, response);
  modifiedResponse.headers.set('Access-Control-Allow-Origin', '*');
  return modifiedResponse;
}
```
不用点击get，404是正常的。然后点击下方的**保存并部署**

6.回到主页，点击左边最上面的网站，选择你刚买好的域名进去。点击左侧的workers路由。

![链接](https://github.com/SnowfallC/chatgpt-api-worker-proxy/blob/main/examples/workroute.png)

7.点击添加路由。上面的路由写 自定义名称.你的域名/*
不可以省略/*
例如:你的路由名称自定义为 openaiuse，你的域名是 openaiuse.bike,那你的路由填写。**以下都以该设定为例**
```shell
openaiuse.openaiuse.bike/*
```
![链接](https://github.com/SnowfallC/chatgpt-api-worker-proxy/blob/main/examples/addworkers2.png)
下面选择你刚创建好的Workers。

8.在左边选择dns，点击添加记录，然后填写你刚刚的自定义名称，Ipv4填写2.2.2.2，打开代理，保存。
![链接](https://github.com/SnowfallC/chatgpt-api-worker-proxy/blob/main/examples/dns.png)

恭喜！你已经成功代理了openai的api，代理地址就是https://openaiuse.openaiuse.bike/
使用官方api时，替换掉官方的域名，使用你的sk openai apikey即可，例如
```shell
curl --location 'https://openaiuse.openaiuse.bike/v1/chat/completions' \
--header 'Authorization: Bearer sk-xxxxxxxxxxxxxxx' \
--header 'Content-Type: application/json' \
--data '{
   "model": "gpt-3.5-turbo",
  "messages": [{"role": "user", "content": "Hello!"}]
 }'
 ```
 大功告成！
