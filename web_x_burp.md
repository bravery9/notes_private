
### burp suite扩展 - bypassWAF

BApp安装bypassWAF后，

在burp的选项卡`Project options` - `Session Handling Rules` - `Rules Actions` - `Add`-`Invoke a Burp Extension` 选择`bypassWAF`

在`scope`里指定：工具范围 url范围 参数范围

这里只需要在`URL scope`里增加一个`Any`即可
