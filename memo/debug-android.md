# Mac 调试安卓手机页面

下文所指手机均为华为手机。

## 方法一：同一 Wi-Fi 下调试手机页面

步骤：

1. 让手机和电脑处于同一 Wi-Fi 中
2. http-server 启动服务器，会给出 IP 和端口号
3. 手机可以直接通过 IP 和端口号访问电脑页面



## 方法二：Chrome Dev Tool 的远程调试

### 情况一：Mac 访问安卓手机的浏览器页面

使用场景：项目部署在其他服务器上。手机上访问页面出现问题，用电脑进行调试。

步骤：

1. 用 USB 数据线将你的安卓手机连接到您的开发机上。
    需要激活安卓的`开发人员选项`，打开 `USB 调试`，注意要打开`连接 USB 时总是弹出提示`。
    当用数据线将手机与 Mac 连接的时候，弹出“USB 连接方式”的提示，选择`传输文件`。默认是“仅充电”，不会读取到手机文件。
    注意：有些三合一 USB 数据线只能充电，无法传输文件，需更换可以传输文件的数据线。

![连接 USB 时总是弹出提示](https://user-images.githubusercontent.com/11813936/179130102-5d9947bb-d350-496d-b8b1-3850e8490b2e.png) ![image](https://user-images.githubusercontent.com/11813936/179130223-11f97bcf-3418-4794-83cd-f8bebe7e8993.png)

2. 地址栏访问：`chrome://inspect/#devices`
    勾选“Discover USB devices”，等待发现设备。

    <details>
      <summary>发现设备的界面效果</summary>
      <img alt="华为手机 HLK-AL00" src="https://wx4.sinaimg.cn/large/6cdfff77gy1h44gvs42gfj20f90eh41s.jpg"/>
    </details>

3. 打开手机 Chrome 浏览器或自带浏览器（目前无法识别夸克浏览器）。默认首页页签是无法 inpect 的，只有手机进入具体内容的页面，Mac 端再点击 inspect 才会看到内容。此时可以在电脑端对安卓手机进行 debug，手机页面和电脑页面处于实时联动状态（滑动手机页面屏幕，Mac 屏幕也跟着移动，反之亦然）。

### 情况二：安卓手机访问 Mac 本地服务的 HTML 页面

使用场景：项目可以在本地服务器运行。电脑端页面制作完成，需要在手机上测试手机端效果。

步骤：

1. 前两步与情况一相同。USB 数据线连接是必要步骤。

2. Mac 命令行用`http-server`创建本地服务。（http-server 是一个基于 node.js 的命令行工具，可以建立一个 http 服务。）此时服务的访问地址：`http://127.0.0.1:8081`

3. 定义端口转发。在`chrome://inspect/#devices`页面中点击`Port forwarding`，设置转发的端口为上个步骤创建的服务的 IP 地址和端口。

    <details>
      <summary>端口转发的界面效果</summary>
      <img alt="端口转发至 8081 端口" src="https://wx4.sinaimg.cn/large/6cdfff77gy1h44iv0s5y3j20nr0ef42n.jpg"/>
    </details>

4. 此时即使在断网的情况下，手机用任意浏览器访问`http://127.0.0.1:8081/index.html`，都可以访问到内容。（安卓手机仅仅能访问页面内容，无法做到情况一的实时联动。）

## 解惑：

- 问：什么是端口转发？

> Port forwarding enables your Android device to access content that's being hosted on your development machine's web server. Port forwarding works by creating a listening TCP port on your Android device that maps to a TCP port on your development machine. Traffic between the ports travel through the USB connection between your Android device and development machine, so the connection doesn't depend on your network configuration.

翻译：端口转发使得安卓设备可以访问本地服务器上的内容。<u>它的工作原理是在安卓设备上对某个 TCP 端口进行监听，并将这个 TCP 端口映射到本地开发机的 TCP 端口。</u>两个端口通过 USB 数据线在安卓设备和本地开发机之间进行通信，并不依赖于网络配置（即使两个设备属于不同网络，或是当前处于断网状态也不影响通信正常进行）。

*自己的理解：设置端口转发后，会把安卓的8081端口和 Mac 的8081端口之间建立通信，相当于两个服务器之间可以通信。当有 HTTP 请求访问安卓的8081端口时，会自动把请求映射到 Mac 的8081端口，相当于访问 Mac 的8081端口。因此，安卓浏览器访问安卓的8081端口，返回的是 Mac 的8081端口对应的服务器内容。(HTTP 请求是指：客户端通过发送 HTTP 请求向服务器请求对资源的访问。)*

- 问：什么是端口监听？

答：对设备的某个端口进行监听，当有请求到达本设备想访问这个端口时，就可以对请求进行自定义处理。

## 参考：

- Chrome Developers 官方文档：
    - [Remote debug Android devices](https://developer.chrome.com/docs/devtools/remote-debugging/)
    - [port-forwarding](https://developer.chrome.com/docs/devtools/remote-debugging/local-server/#port-forwarding)
- 官方文档的中文翻译：[远程调试 Android 设备](https://www.html.cn/doc/chrome-devtools/remote-debugging/)

- [express 端口监听运行原理](https://juejin.cn/post/6844903992087019533)
