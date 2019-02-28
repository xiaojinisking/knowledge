# Xcode开发遇到的问题

## Xcode如何打开项目

在react-native 项目下面的ios目录 找到里面的 .xcodeproj文件，打开就会启动xcode项目，因为他是xcode的项目文件。

![](assets/markdown-img-paste-20190226005945516.png)


## React Native出错：Application XXX has not been registered解决方案

![](assets/markdown-img-paste-20190226010207214.png)


原因是之前打开了其他的react Native项目占用了端口

解决办法是：关闭之前的项目，然后重新启动即可
