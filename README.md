# 抽奖转盘游戏

一个基于 Web 的转盘抽奖游戏，支持 iOS 和 Android 原生应用集成。

## 功能特点

- 流畅的转盘动画效果
- 支持自定义奖品配置
- 防作弊机制
- 响应式设计
- 原生应用交互支持
- 加载动画和模态框提示

## 使用说明

### 1. 基础配置

游戏初始化时可配置以下参数：
```javascript
const PRIZES = [
{
title: '手气不错哟～恭喜获得',
prize: '100元红包',
},
// ... 其他奖品配置
];
```
以及修改图片资源images/game-wheel.png与自己的奖品配置一致

### 2. 与原生应用交互

#### 2.1 从原生应用接收参数

- 设置抽奖次数：

```javascript
// iOS/Android 通过 postMessage 发送
{
    type: 'luckDrawCount',
    count: number // 剩余抽奖次数
}
```

#### 2.2 向原生应用发送结果

- 抽奖结果通知：

```javascript
// 发送给原生应用的数据格式
{
    type: 'drawResult',
    index: number,    // 中奖索引（0-4）
    prize: {
        title: string,  // 奖品标题
        prize: string   // 奖品内容
    }
}
```

### 3. 原生应用集成方法

#### iOS 集成

```swift
// WKScriptMessageHandler 实现
func userContentController(_ userContentController: WKUserContentController, 
                         didReceive message: WKScriptMessage) {
    if let dict = message.body as? [String: Any] {
        if dict["type"] as? String == "drawResult" {
            let index = dict["index"] as? Int ?? 0
            // 处理抽奖结果
        }
    }
}

// 设置抽奖次数
webView.evaluateJavaScript("setLuckDrawCount(\(count))", 
                         completionHandler: nil)
```

#### Android 集成

```kotlin
// WebView 接口类
class WebAppInterface(private val context: Context) {
    @JavascriptInterface
    fun sendMessage(message: String) {
        // 解析 JSON 字符串
        val jsonObject = JSONObject(message)
        if (jsonObject.getString("type") == "drawResult") {
            val index = jsonObject.getInt("index")
            // 处理抽奖结果
        }
    }
}

// 注册 JavaScript 接口
webView.addJavascriptInterface(WebAppInterface(this), "Android")

// 设置抽奖次数
webView.evaluateJavascript("setLuckDrawCount($count)", null)
```

### 4. 奖品位置说明

转盘奖品位置对应关系：
- index 0: 100元红包 (22.5°)
- index 1: 优惠券礼包 (157.5°)
- index 2: 5元代金券 (112.5°)
- index 3: 1元红包 (67.5°)
- index 4: 优惠券礼包 (202.5°)

## 注意事项

1. 确保网络环境良好，所有资源能够正常加载
2. 建议使用 HTTPS 协议部署
3. 建议使用现代浏览器访问，确保 CSS3 动画效果正常

## 浏览器兼容性

- Chrome 60+
- Safari 10+
- Firefox 54+
- iOS Safari 10+
- Android Browser 4.4+
- Chrome for Android 88+

## 文件结构

```
├── css/
│   └── style.css          # 样式文件
├── js/
│   └── game.js           # 游戏逻辑
├── images/               # 图片资源
│   ├── game-wheel.png
│   ├── game-arrow.png
│   └── game-bg.png
│   └── game-title.png
│   └── screenshot.png
└── game.html            # 游戏主页面
```

