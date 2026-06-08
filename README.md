# PySide6 CDP Web Hook Demo

一个基于 **PySide6 / Qt WebEngine / Chrome DevTools Protocol（CDP）** 的内嵌 Web 调试与请求监控示例项目。

本项目主要演示如何在 PySide6 桌面程序中嵌入 Web 页面，并通过 CDP 与内置 Chromium 内核进行通信，实现页面生命周期监听、网络请求观察、接口响应分析、页面脚本注入以及自动化调试等能力。

## 项目特点

* 使用 PySide6 构建桌面端 GUI 界面
* 通过 QWebEngineView 嵌入 Web 页面
* 基于 CDP 连接 Qt WebEngine 的调试端口
* 支持监听页面请求、响应、加载状态等事件
* 支持执行 JavaScript 脚本进行页面调试
* 支持将 Web 页面行为与本地 Python 逻辑联动
* 适用于 Web 自动化测试、内嵌页面调试、接口联调和数据观测场景

## 技术栈

* Python
* PySide6
* Qt WebEngine
* Chrome DevTools Protocol
* WebSocket
* JavaScript

## 应用场景

本项目适合用于以下场景：

1. 桌面应用内嵌 Web 页面
2. Web 页面加载状态监控
3. 网络请求与响应调试
4. 自动化测试辅助工具
5. 本地 GUI 与 Web 页面交互
6. 接口联调与页面行为分析

## 基本原理

Qt WebEngine 基于 Chromium 内核，可以通过开启远程调试端口暴露 CDP 调试接口。

程序启动后，PySide6 负责创建桌面窗口和 Web 容器，CDP 模块通过 WebSocket 连接调试端口，并订阅 Network、Page、Runtime 等 DevTools 事件，从而实现对页面请求、响应、脚本执行和页面状态的监听。

整体流程如下：

```text
PySide6 GUI
    ↓
QWebEngineView 内嵌页面
    ↓
开启 Qt WebEngine 调试端口
    ↓
CDP WebSocket 连接
    ↓
监听 Network / Page / Runtime 事件
    ↓
Python 业务逻辑处理
```

## 功能模块

```text
project/
├── main.py                 # 程序入口
├── browser_window.py       # PySide6 主窗口与 WebEngineView
├── cdp_client.py           # CDP WebSocket 客户端
├── hooks/
│   ├── network_hook.py     # 网络请求监听
│   ├── runtime_hook.py     # JS 执行与页面注入
│   └── page_hook.py        # 页面加载状态监听
├── utils/
│   └── logger.py           # 日志工具
└── README.md
```

## 示例能力

### 监听请求

```python
cdp.send("Network.enable")
```

订阅网络事件后，可以监听页面发出的请求：

```text
Network.requestWillBeSent
Network.responseReceived
Network.loadingFinished
```

### 执行 JavaScript

```python
cdp.send("Runtime.evaluate", {
    "expression": "document.title"
})
```

### 页面加载监听

```python
cdp.send("Page.enable")
```

可监听页面加载完成、跳转、刷新等事件。

## 注意事项

本项目仅用于学习、调试、自动化测试和合法的数据观测场景。
请勿将本项目用于绕过平台安全策略、未授权数据采集、账号风险控制规避或其他违反目标网站服务条款的行为。

## 运行环境

建议环境：

```text
Python 3.10+
PySide6
websocket-client / websockets
requests
```

安装依赖：

```bash
pip install PySide6 websocket-client requests
```

运行项目：

```bash
python main.py
```

## 项目定位

该项目不是传统意义上的浏览器，而是一个面向开发者的内嵌 Web 调试容器。
它将 PySide6 桌面 GUI、Qt WebEngine 页面渲染能力和 CDP 调试能力结合起来，方便开发者在本地应用中观察、调试和控制 Web 页面行为。
