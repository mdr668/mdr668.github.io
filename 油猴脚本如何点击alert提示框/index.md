# 油猴脚本如何点击alert提示框

有的时候，网页会弹出alert提示框，用脚本的话，通常就会卡到这里了，那么Tampermonkey如何点击alert提示框呢？

搜索了解了下，其实点击做不到的话，换个角度问题就迎刃而解了。比如劫持alert事件。

示例代码如下：

```
// ==UserScript==
// @name        Block javascript alerts
// @match       https://example.com/*
// @run-at      document-start
// ==/UserScript==

addJS_Node (null, null, overrideSelectNativeJS_Functions);

function overrideSelectNativeJS_Functions () {
    window.alert = function alert (message) {
        console.log (message);
    }
}

function addJS_Node (text, s_URL, funcToRun) {
    var D                                   = document;
    var scriptNode                          = D.createElement ('script');
    scriptNode.type                         = "text/javascript";
    if (text)       scriptNode.textContent  = text;
    if (s_URL)      scriptNode.src          = s_URL;
    if (funcToRun)  scriptNode.textContent  = '(' + funcToRun.toString() + ')()';

    var targ = D.getElementsByTagName ('head')[0] || D.body || D.documentElement;
    targ.appendChild (scriptNode);
}
```

---

> 作者: 猫大人  
> URL: /%E6%B2%B9%E7%8C%B4%E8%84%9A%E6%9C%AC%E5%A6%82%E4%BD%95%E7%82%B9%E5%87%BBalert%E6%8F%90%E7%A4%BA%E6%A1%86/  

