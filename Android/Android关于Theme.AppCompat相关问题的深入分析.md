# Android关于Theme.AppCompat相关问题的深入分析

先来看这样一个错误：

> No resource found that matches the given name '@style/Theme.AppCompat.Light'

对于这个错误，相信大部分Android开发者都遇到过，可能很多朋友通过百度或者Google已经解决了这个问题，但是网上大部分都只给出了
解决方法。正所谓知其然，知其所以然，本文将从此问题出发，深入分析探讨导致此问题的原因、由其衍生出来的一系列问题及其解决方案。

### Android Support Library

> The Android Support Library package is a set of code libraries that provide backward-compatible versions 
> of Android framework APIs as well as features that are only available through the library APIs.

Android的SDK版本很多，新的SDK版本包含了很多新的特性，为此Google官方提供Android Support Library package来保证高版本SDK
的向下兼容。通过使用此包，可以让拥有最新SDK特性的应用运行在API lever 4(即Android 1.6) 及更高版本的设备之上。

- v4 Support Library

此包用在API lever 4(即Android 1.6)及更高版本之上。它包含了较多的内容，使用非常广泛，例如：Fragment，NotificationCompat，
LoadBroadcastManager，ViewPager，PageTabStrip，Loader，FileProvider 等。

- v7 Support Libraries

此包是针对API level 7(即Android 2.1)及以上版本而设计的，但是v7是要依赖v4这个包的，v7支持了Action Bar以及一些Theme的兼容。

> Note: v7 appcompat library
v7 appcompat library 是包含在 v7 Support Libraries里面的一个包，正是此包增加了Action Bar 用户界面的设计模式，并加入了对material design 的支持，是我们使用最多的一个兼容包。


