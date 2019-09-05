# JSBridge
JSBridge原理
转自https://juejin.im/post/5abca877f265da238155b6bc


JSBrige
	简单讲，主要是给JavaScript提供Native功能的接口，让混合开发中的前段部分可以方便的使用地理位置、摄像头甚至支付等native功能
	实际是natvie与非native之前的桥梁，核心是构建native和非native间消息通信的通道，而且是双向通信的通道

JS 向 Native 发送消息 : 调用相关功能、通知 Native 当前 JS 的相关状态等。

Native 向 JS 发送消息 : 回溯调用结果、消息推送、通知 JS 当前 Native 的状态等。


H5 调用Native 
原生将WebViewJavascriptBridge绑定在window上
H5 直接调用这个对象中的原生接收方法

Native调用H5
H5将JSBridge绑定在window上
Native通过原生方法调用这个对象上的H5接收方法

JSBridge实现原理

JavaScript 是运行在一个单独的 JS Context 中（例如，WebView 的 Webkit 引擎、JSCore）。由于这些 Context 与原生运行环境的天然隔离，我们可以将这种情况与 RPC（Remote Procedure Call，远程过程调用）通信进行类比，将 Native 与 JavaScript 的每次互相调用看做一次 RPC 调用。
在 JSBridge 的设计中，可以把前端看做 RPC 的客户端，把 Native 端看做 RPC 的服务器端，从而 JSBridge 要实现的主要逻辑就出现了：通信调用（Native 与 JS 通信） 和 句柄解析调用。（如果你是个前端，而且并不熟悉 RPC 的话，你也可以把这个流程类比成 JSONP 的流程）对于 JSBridge 的 Callback ，其实就是 RPC 框架的回调机制
