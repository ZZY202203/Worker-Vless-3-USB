# 优选订阅生成器 WorkerVless3sub

### 全新的通过 Cloudflare Workers 搭建VLESS 节点，自动生成优选线路订阅内容生成器

**Telegram交流群：[@科技猎奇](https://t.me/AYANGKJDS)    Telegram全能王搜索群：[@搜索一下，你就知道](https://t.me/AYANGQNW)**

### 免责声明

本声明适用于 GitHub 上的 “Edgetunnel” 项目（以下简称“该项目”），项目链接为：
[https://edgetunnel.github.io/edgetunnel/](https://github.com/cmliu/edgetunnel)

https://github.com/cmliu/WorkerVless2sub/tree/main

该项目为Edgetunnel项目的克隆整理及备份，原作者享有本项目内容的所有权；
该项目被设计和开发仅供学习、研究和安全测试目的。
它旨在为安全研究者、学术界人士和技术爱好者提供一个了解和实践网络通信技术的工具。

# Workers 部署方法 [关注博主](https://www.youtube.com/@kejiLQ)
### 1. 部署 Cloudflare Worker：

   - 在 Cloudflare Worker 控制台中创建一个新的 Worker。
   - 将 [worker.js](https://github.com/ZZY202203/Worker-Vless-3-USB/blob/main/_worker.js)  的内容粘贴到 Worker 编辑器中。


### 2. 添加你的专属优选线路：

**2.1 修改 addresses 参数示例**

 - 修改 `addresses` 参数添加本地静态的优选线路，若不带端口号默认8443，不支持生成非TLS订阅，#号后为备注别名，例如：
	```js
	let addresses = [
		'icook.tw:2053#优选域名',
		'cloudflare.cfgo.cc#优选官方线路',
		'185.221.160.203:443#电信优选IP',
	];
	```
	该方式仅推荐添加优选域名的部分，频繁变更的优选推荐通过 `addressesapi` 来实现。


 **2.2 修改 addressesapi 参数示例**
 
 - 修改 `addressesapi` 参数，在脚本中设置 `addressesapi` 变量为 **优选IP地址txt文件** 的 URL。例如：
	```js
	let addressesapi = [
		'https://raw.githubusercontent.com/onezyhcn/WorkerVless2sub/main/addressesapi.txt',
 		'https://addressesapi.090227.xyz/CloudFlareYes',
	];
	```
	可参考 [addressesapi.txt](https://github.com/Onezyh/Worker-Vless-2-USB/blob/master/addressesapi.txt) 内容格式 自行搭建。


<details>
<summary><code><strong>「 我不是小白！我有IP库！我知道IPtest是什么！我也有csv测速文件！ 」</strong></code></summary>

 
  **2.3 修改 addressescsv 参数示例**
  
 - 修改 `addressescsv` 参数，在脚本中设置 `addressescsv` 变量为 **iptest测速结果csv文件地址** 的 URL。例如：
	```js
	let DLS = 4;//速度下限
	let addressescsv = [
		'https://github.com/Onezyh/Worker-Vless-2-USB/blob/master/addressescsv.csv',
 		'https://github.com/Onezyh/Worker-Vless-2-USB/blob/master/addressescsv.csv',
	];
	```
	`DLS` 为要求满足的最低速度，不满足改数值以上的IP将不会添加至优选订阅内容。注意：不考虑单位，只看数值，请按照您的测速结果而定。

 </details>


### 3. 修改 快速订阅入口 以及 默认内置 Vless 节点信息：

  例如您的workers项目域名为：`sub.onezyhcnssss.workers.dev`；
   - 添加 `TOKEN` 变量，快速订阅访问入口，默认值为: `auto` ，获取订阅器默认节点订阅地址即 `/auto` ，例如 `https://sub.onezyhcn.workers.dev/auto`
   - 添加 `HOST` 变量，例如 `edgetunnel-2z2.pages.dev`；
   - 添加 `UUID` 变量，例如 `30e9c5c8-ed28-4cd9-b008-dc67277f8b02`；
   - 添加 `PATH` 变量，例如 `/?ed=2048`；



# 订阅生成器 使用方法[关注博主](https://www.youtube.com/@kejiLQ)

  例如您的workers项目域名为：`sub.onezyhcn.workers.dev`；
  
### 1. 快速订阅

   - 添加 `TOKEN` 变量，快速订阅访问入口，默认值为: `auto` ，获取订阅器默认节点订阅地址即 `/auto` ，例如：
     ```url
     https://sub.onezyhcn.workers.dev/auto
     ```
     
### 2. 自定义订阅 

   - **自定义订阅格式** `https://[你的Workers域名]/sub?host=[你的Vless域名]&uuid=[你的UUID]&path=[你的ws路径]`
   - **host**：您的 VLESS 伪装域名，例如 `edgetunnel-2z2.pages.dev`；
   - **uuid**：您的 VLESS 客户端 UUID，例如 `30e9c5c8-ed28-4cd9-b008-dc67277f8b02`；
   - **path**（可选）：您的 VLESS 的 WS 路径（没有可留空不填），例如 `/?ed=2048`。
   - 自定义订阅地址如下：
     ```url
     https://sub.onezyhcn.workers.dev/sub?host=edgetunnel-2z2.pages.dev&uuid=30e9c5c8-ed28-4cd9-b008-dc67277f8b02&path=/?ed=2048
     ```
   - 注意路径必须包含 "/sub"。

### 3. 指定 clash、singbox 配置文件

   - 添加 `format=clash` 键值，获取 clash 订阅配置，例如：
     ```url
     https://sub.onezyhcn.workers.dev/auto?format=clash
     https://sub.onezyhcn.workers.dev/sub?format=clash&host=edgetunnel-2z2.pages.dev&uuid=30e9c5c8-ed28-4cd9-b008-dc67277f8b02&path=/?ed=2048
     ```
     
   - 添加 `format=singbox` 键值，获取 singbox 订阅配置，例如：
     ```url
     https://sub.onezyhcn.workers.dev/auto?format=singbox
     https://sub.onezyhcn.workers.dev/sub?format=singbox&host=edgetunnel-2z2.pages.dev&uuid=30e9c5c8-ed28-4cd9-b008-dc67277f8b02&path=/?ed=2048
     ```
     
### 变量说明
| 变量名 | 示例 | 备注 | 
|--------|---------|-----|
| TOKEN | auto | 快速订阅内置节点的订阅路径地址 /auto | 
| HOST | edgetunnel-2z2.pages.dev | 快速订阅内置节点的伪装域名 | 
| UUID | 30e9c5c8-ed28-4cd9-b008-dc67277f8b02 | 快速订阅内置节点的UUID | 
| PATH | /?ed=2048 | 快速订阅内置节点的路径信息 | 
| TGTOKEN | 6894123456:XXXXXXXXXX0qExVsBPUhHDAbXXXXXqWXgBA | 发送TG通知的机器人token | 
| TGID | 6946912345 | 接收TG通知的账户数字ID | 
| SUBAPI | api.v1.mk | clash、singbox等 订阅转换后端 | 
