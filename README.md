# chatgpt-api-worker-proxy
完整的教程，如何中转api在受限制区域连接chatgpt的api
第一步， 注册并登录，在右上角切换为简体中文。
第二步，左侧域注册，注册域,输入域名查找购买。便宜的域名差不多3刀到4刀一年，够使用很久了，free计划即可。
第三步，进入workers，创建一个服务，起个名字，选左边http程序，创建服务。free计划。
第四步，在workers的概述中选择你刚创建的服务，点击快速编辑。
第五步，复制以下代码复制到左侧文本框(代码来自https://github.com/x-dr/chatgptProxyAPI）
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
不用点击get，404是正常的。然后点击下方的#保存并部署
第六步，回到主页，点击左边最上面的网站，选择你刚买好的域名进去。点击左侧的workers路由。
第七步，点击添加路由。上面的路由写 自定义名称.你的域名/*
不可以省略/*