---
title: 浏览器知道你的哪些信息
date: 2016-04-10 11:11:11
tags: [html5, JavaScript, 浏览器, 隐私, 位置]
categories: [科普]
banner: https://img.c7sky.com/2016/04/10/webkay-banner.png
thumbnail: https://img.c7sky.com/2016/04/10/webkay-banner.png
---
# 浏览器知道你的哪些信息

> 作者：小影 博客：[小影志](https://c7sky.com/ "小影志")

> 在不弹出权限询问的情况下，浏览器可以获得你的哪些信息？除了常见的 IP、地理位置、系统和浏览器版本，其实还能获取本地 IP、CPU 平台、显卡型号、登录过的社交网站等等信息。

> What every Browser knows about you 展示了浏览器知道的所有关于你的信息。本文就来一一解释下所使用的技术。

### 地理位置（Location）

#### 经纬坐标和位置信息

Webkay 使用了 [Google Maps Geolocation API](https://developers.google.com/maps/documentation/geolocation/intro "Google Maps Geolocation API") 来获取地理位置。

类似的地理位置服务都是通过服务器获取客户端 IP，然后在 IP 地址库中查找对应的真实坐标。

这种方法依赖于浏览器上报的 IP，精确度远不如 GPS。比如我挂了代理，Google 就把我定位到了日本。

以下是 PHP 获取客户端 IP 的代码（[source](http://stackoverflow.com/a/15699240/837684 "source")）：

    function get_client_ip() {
        $ipaddress = '';
        if (getenv('HTTP_CLIENT_IP'))
            $ipaddress = getenv('HTTP_CLIENT_IP');
        else if(getenv('HTTP_X_FORWARDED_FOR'))
            $ipaddress = getenv('HTTP_X_FORWARDED_FOR');
        else if(getenv('HTTP_X_FORWARDED'))
            $ipaddress = getenv('HTTP_X_FORWARDED');
        else if(getenv('HTTP_FORWARDED_FOR'))
            $ipaddress = getenv('HTTP_FORWARDED_FOR');
        else if(getenv('HTTP_FORWARDED'))
           $ipaddress = getenv('HTTP_FORWARDED');
        else if(getenv('REMOTE_ADDR'))
            $ipaddress = getenv('REMOTE_ADDR');
        else
            $ipaddress = 'UNKNOWN';
        return $ipaddress;
    }

如果想获取精准的地理坐标，可以使用 HTML5 中的 [Geolocation API](https://developer.mozilla.org/en-US/docs/Web/API/Geolocation/Using_geolocation "Geolocation API")，不过使用该 API 会向用户弹出权限询问框。

#### 语言
`window.navigator` 保存了不少客户端信息，接下来你还会看到很多次：

    var languages = navigator.languages     // 非 IE 浏览器，语言列表
                 || navigator.language      // 非 IE 浏览器，首选语言
                 || navigator.userLanguage; // IE 浏览器，首选语言

#### 本地时间

最容易获取的信息之一：

    new Date();

### 软件（Software）

#### 系统和浏览器

Webkay 使用了 [UAParser.js](https://github.com/faisalman/ua-parser-js "UAParser.js") 对 navigator.userAgent 进行了解析。UserAgent 中文叫用户代理字符，这个字符串包含了浏览器和系统的表示信息。

比如这几个 UserAgent：

- Chrome: `Mozilla/5.0 (Macintosh; Intel Mac OS X 10_11_4) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/49.0.2623.75 Safari/537.36`
- iPad: `Mozilla/5.0 (iPad; CPU OS 9_3 like Mac OS X) AppleWebKit/601.1.46 (KHTML, like Gecko) Version/9.0 Mobile/13E237 Safari/601.1`
- MEIZU MX4: `Mozilla/5.0 (Linux; Android 5.1; MZ-MX4 Build/LMY47I) AppleWebKit/537.36 (KHTML, like Gecko) Version/4.0 Chrome/45.0.2454.94 Mobile Safari/537.36`

#### 插件

浏览器插件则是通过遍历 `navigator.plugins` 获得。

    for (var i = 0; i < navigator.plugins.length; i++) {
        var plugin = navigator.plugins[i];
        plugin.name        // 名称
        plugin.filename    // 文件名
        plugin.description // 描述
        plugin.version     // 版本号
    }

### 硬件（Hardware）

#### CPU

Webkay 结合使用了 `navigator.platform` 和 UAParser.js 来获取 CPU 平台，核心数则通过 `navigator.hardwareConcurrency` 获取。

#### GPU

通过 [WebGL API](https://developer.mozilla.org/en-US/docs/Web/API/WebGL_API "WebGL API") 中的 [WEBGL_debug_renderer_info](https://developer.mozilla.org/en-US/docs/Web/API/WEBGL_debug_renderer_info "WEBGL_debug_renderer_info") 来获取显卡信息。

分辨率和颜色深度则是通过 `window.screen `获取。

    var canvas = document.createElement("canvas");
    var gl = canvas.getContext("experimental-webgl");
    var debugInfo = gl.getExtension("WEBGL_debug_renderer_info");
    
    gl.getParameter(debugInfo.UNMASKED_VENDOR_WEBGL);   // 显卡供应商
    gl.getParameter(debugInfo.UNMASKED_RENDERER_WEBGL); // 显卡渲染器
    
    window.screen.width      // 屏幕宽度
    window.screen.height     // 屏幕高度
    window.screen.colorDepth // 颜色深度

#### 电量

通过新的 [Battery Status API](https://developer.mozilla.org/en-US/docs/Web/API/Battery_Status_API "Battery Status API") 获取电量相关信息；

    navigator.getBattery().then(function(battery) {
        battery.charging;               // 是否在充电
        battery.level;                  // 电量比例
        battery.chargingTime;           // 充电时长
        battery.dischargingTime;        // 放电时长
    
        /* 事件 */
        battery.onchargingchange        // 充电状态放生变化
        battery.onlevelchange           // 电量发生变化
        battery.onchargingtimechange    // 充电时长发生变化
        battery.ondischargingtimechange // 放电时长发生变化
    });

### 连接（Connection）

#### 本地 IP

使用 [WebRTC](https://developer.mozilla.org/en-US/docs/Web/Guide/API/WebRTC "WebRTC") 中的 [RTCPeerConnection](https://developer.mozilla.org/en-US/docs/Web/API/RTCPeerConnection "RTCPeerConnection") 接口建立 WebRTC 连接，给其他客户端发送请求时会带上本地 IP。我们只需要对信息中的 IP 进行提取即可。

    var PeerConnection = window.RTCPeerConnection || window.mozRTCPeerConnection || window.webkitRTCPeerConnection;
    var pc = new PeerConnection({
        iceServers: [{
            urls: "stun:stun.services.mozilla.com"
        }]
    });
    
    pc.onicecandidate = function(event) {
        if (event.candidate) {
            var candidate = event.candidate.candidate;
            // candidate:2795255774 1 udp 2122260223 192.168.1.7 64974 typ host generation 0 ufrag W1Ah3U1F3qZXQ8aM
            var ip = candidate.match(/([0-9]{1,3}(\.[0-9]{1,3}){3}|[a-f0-9]{1,4}(:[a-f0-9]{1,4}){7})/)[1];
        }
    };
    
    pc.createDataChannel("c7sky.com");
    
    pc.createOffer(function(offer) {
        pc.setLocalDescription(offer);
    });

#### 公网 IP 和服务器运营商

分别使用了在线服务 https://api.ipify.org?format=json 和 http://ip-api.com/json。

原理和之前获取经纬坐标和位置信息一样，服务器根据 IP 查地址库。

#### 来源页

    document.referrer

#### 下载速度

Webkay 通过 `new Image()` 下载一张图片，然后用文件大小除以用时得出，不过这个方法需要事先在 JS 里写明文件大小。如果用 `XMLHttpRequest` 会更加方便：

    var startTime = Date.now();
    var xhr = new XMLHttpRequest();
    xhr.open("GET", "https://upload.wikimedia.org/wikipedia/commons/2/2d/Snake_River_%285mb%29.jpg");
    
    xhr.onload = function() {
        var duration = (Date.now() - startTime) / 1000;
        var size = xhr.getResponseHeader("Content-Length") / 1024;
        var speed = (size / duration).toFixed(2);
        console.log(speed);
    };
    
    xhr.send();

#### 网络类型

使用了目前还没有任何浏览器支持的 [Network Information API](https://developer.mozilla.org/en-US/docs/Web/API/Network_Information_API "Network Information API") ...

    var type = navigator.connection.type;
    // bluetooth || cellular || ethernet || none || wifi || wimax || other || unknown
    
### 社交媒体（Social Media）

如果你已经登录了某个网站，这时你再访问该网站的登录页面，网站会自动跳转到其他页面。

Webkay 就是利用了这一点，通过一个地址为登录页面（可能会跳转到 Favicon）的图像元素，如果接收到的是图片，就说明用户已经登录了该网站，并触发 `onload` 事件，反之则不会触发。

以 Twitter 为例：在你已经登录的情况下访问 https://twitter.com/login?redirect_after_login=%2Ffavicon.ico，页面就会自动跳转到 https://twitter.com/favicon.ico

    var img = new Image();
    
    img.onload = function() {
        // 你登录过 Twitter 啦
    };
    
    img.src = "https://twitter.com/login?redirect_after_login=%2Ffavicon.ico";

### 点击劫持（Click Jacking）

这个我感觉和 What every Browser knows about you 的主题无关，具体介绍可以看 [Clickjacking 简单介绍 | WooYun 知识库](http://drops.wooyun.org/papers/104 "Clickjacking 简单介绍 | WooYun 知识库")。

### 陀螺仪（Gyroscope）

这个只有在带有陀螺仪的设备上才有效，通过注册 [deviceorientation](https://developer.mozilla.org/en-US/docs/Web/API/Window/ondeviceorientation "deviceorientation") 事件实时获取设备的陀螺仪信息。

    function handleOrientation(event) {
        event.alpha; // Z 轴旋转角度
        event.beta;  // X 轴旋转角度
        event.gamma; // Y 轴旋转角度
    }
    window.addEventListener("deviceorientation", handleOrientation);

![](https://img.c7sky.com/2016/04/10/device-alpha-beta-gamma.png)

### 网络扫描（Network Scan）

用 [WebSocket API ](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API "WebSocket API ")遍历之前获得的本地 IP 所在的 IP 段。这个方法只有在对方搭建了 WebSocket 服务器的情况下才有效，对于普通用户来说，可能性基本为零。

    var ws_scan = new WebSocket("ws://" + hostname);
    var start_time_ws = Date.now();
    var closetimeout = 1200;
    
    setInterval(function() {
        var interval = Date.now() - start_time_ws;
        if (ws_scan.readyState === 3) {
            if (closetimeout < interval) {
                // 存在该 IP
            }
        }
        return;
    }, 10);

### 图像（Image）

使用 [Exif.js](https://github.com/exif-js/exif-js "Exif.js") 读取图像 EXIF 信息。

### 总结

这些信息并不像 Cookie 存在关于个人隐私的安全隐患，如果能够善用这些信息，其实可以给用户带来更好的用户体验～

#### 许可：
- 分享请注明原作者和出处，本项目不得用于商业用途。
- <a rel="license" href="http://creativecommons.org/licenses/by-nc-nd/4.0/"><img alt="知识共享许可协议" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-nd/4.0/80x15.png" /></a>
