# Venom流量转发 - 自动化捡洞/打点必备神器

**郑重声明：文中所涉及的技术、思路和工具仅供以安全为目的的学习交流使用，<u>任何人不得将其用于非法用途以及盈利等目的，否则后果自行承担</u>** 。

<p align="center"><a href="https://opensource.org/licenses/MIT"><img src="https://img.shields.io/badge/license-MIT-_red.svg"></a><a href="https://github.com/z-bool/Venom"><img  src="https://goreportcard.com/badge/github.com/projectdiscovery/httpx"></a></p>

<p align="center"><a href="#install">工具介绍</a> · <a href="#tall">使用说明</a> · <a href="#notice">注意事项</a> · <a href="#communicate">技术交流</a></p>

<div id="install"></div>

<h3>工具介绍</h3>

该流量转发器诞生背景：

- 鉴于平时挖洞打点时用到被动扫描器，在挖洞时又喜欢在多台服务器上部署不同的代理扫描器，总会有捡洞的那一天，在使用Burp做流量转发的时候发现流量只能转发到置于首个的扫描器，于是又使用了https://github.com/c0ny1/passive-scan-client该工具进行流量转发，但是还是不得我意，就花了一天上手搓了一个流量转发器出来，又花了一天调试和优化。

该工具的应用场景是什么：

- 联动被动扫描器：将流量转发至被动扫描器进行漏扫，不限制扫描器数量，只需在启动命令行处设置转发地址即可
- 联动爬虫工具：将爬虫工具的流量转发到Venom监听的端口上，由Venom给多个扫描器进行分发，工具：Rad、Crawlergo、Katana、URLFinder等

为什么不开源出来：

- 这里是因为后续我要出款被动扫描器，设置下悬念。

<div id= "tall"></div>

<h3>使用说明</h3>

![image-20230730224644910](https://cdn.jsdelivr.net/gh/z-bool/images@master/img/image-20230730224644910.png)

参数说明：

```bash
-blackdomain     使用根域名进行禁用（如果公有域名时请使用-mustblackdomain），如：aa.gov.cn(根域名)/gov.cn(公有域名)
-mustblackdomain 禁用公有域名（只要带该内容的域名都会被无条件禁用，为了防止扫到不该扫的），不能为空
-blackfiletype   文件后缀黑名单，那种静态文件就直接拦截了，没必要进行转发混淆流量增加漏扫发包，但是不拦截有参数的情况，比如a.css?index=0此时是不拦截的
-port            监听的本地端口号，默认9090
-proxy           如果访问的站点需要连接外网才能加载的，使用该参数配置代理，默认直连，tcp上的代理只支持socks5
-turnproxy       转发的地址集合，以,分割，以下面的联动端口为例：http://127.0.0.1:1234,http://127.0.0.1:65530,http://127.0.0.1:65531
-workgroup       线程数，默认10，根据自己电脑配置往上加即可 
```

**证书安装：**

正常配置好参数启动时，当前目录下会出现`cert.key`和`cert.crt`文件。

![image-20230730225718581](https://cdn.jsdelivr.net/gh/z-bool/images@master/img/image-20230730225718581.png)

然后按正常装Burp证书一样装到受信任的凭证里的**根证书**里就可以了，然后把浏览器代理设置到监听的端口即可（默认9090）。

**操作示范：**

这里仅示范联动Burp、Xray、Yakit的使用教程（以顺丰为例）：

Burp（65530端口）：

![image-20230730224204767](https://cdn.jsdelivr.net/gh/z-bool/images@master/img/image-20230730224204767.png)

Yakit（65531端口）：

![image-20230730224236034](https://cdn.jsdelivr.net/gh/z-bool/images@master/img/image-20230730224236034.png)

Xray（12345端口）：

![image-20230730224350820](https://cdn.jsdelivr.net/gh/z-bool/images@master/img/image-20230730224350820.png)

Venom联动展示：

```bash
.\Venom -turnproxy "http://127.0.0.1:12345,http://127.0.0.1:65530,http://127.0.0.1:65531"
```

![image-20230730230143496](https://cdn.jsdelivr.net/gh/z-bool/images@master/img/image-20230730230143496.png)

![image-20230730230957971](https://cdn.jsdelivr.net/gh/z-bool/images@master/img/image-20230730230957971.png)

关于为什么没配置其他参数，因为这里面黑名单里默认加了我喜欢屏蔽的一些接口，有其他的新增，请复制全之后加入即可，然后线程别开太大，容易吹风扇。关于效果上，上图已经全部包含。

这里仅示范联动Crawlergo：

![image-20230730233328793](https://cdn.jsdelivr.net/gh/z-bool/images@master/img/image-20230730233328793.png)



**支持系统：**

全支持，支持mac/windows/linux，但mac在家，后续到家打包好再上传更新。

<div id="notice"></div>

<h3>注意事项</h3>

黑名单不能为空，基本除`-turnproxy`外基本默认即可，黑名单是命令行输入的，如果替换了还需保留原有的内容请复制后往后加入。

<div id="communicate"></div>

<h3>技术交流</h3>

<img src="https://cdn.jsdelivr.net/gh/z-bool/images@master/img/qrcode_for_gh_c90beef1e2e7_258.jpg" alt="阿呆攻防公众号" style="zoom:100%;" />
