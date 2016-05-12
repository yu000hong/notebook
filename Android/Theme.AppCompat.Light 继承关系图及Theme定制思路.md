# Theme.AppCompat.Light 继承关系图及Theme定制思路

Android应用程序的主题（Theme）定制真是个头疼的问题，不同的Android版本导致分支众多，主题的属性（Property）又很多并且没有文档
说明，这样的情况下如果没有一个清晰的关系图，很容易不知道从哪里下手去定制。

Android Lollipop发布后，Google更新了Support Library V7，用于兼容Pre－Lollipop的设备，使其可以使用部分Material Design的特性。
Material Design是Google力推的新一代跨平台界面设计标准，得到了设计界的广泛认可， Android应用向这个设计标准迁移已不可避免。
因此从现在开始，建议新开发的Android应用程序把父主题设为**`Theme.AppCompat.Light`**或者其子主题，再根据不同的版本进行对应的
定制化。

废话不说了，下面就是主题的关系图（用类图表示）：

![](http://www.jcodecraeer.com/uploads/141124/1-141124130230b7.jpg)

关系图中的属性主要集中在ActionBar、文字颜色、应用的基本配色等，其它的需要的属性基本上是可以根据关系找到的。
从图中可以看出，不同版本的主题属性是有一些差异的，因此对某些属性定制而言，在你的应用中也需要对版本进行区分，
具体来说就是：除了默认的values目录外，你还需要对不同版本建立不同的values目录，根据继承关系，你起码应该建立values-v11、
values-v14、values-v21这三个目录，在这些目录中都放一个styles.xml，里面是你应用程序的主题，这个主题都应该继承自
`Theme.AppComapt.Light`及其子主题（对于所有版本一样的属性可以在values的目录的styles.xml中建立1个根主题，
便于不同版本的主题进行重用），那么没有版本差异的属性设置就修改values目录中styles.xml，有版本差异的主题就修改对应版本的
values目录中的styles.xml，这样就可以很好地维持了版本的兼容性，又可以使用最新版Lollipop的主题特性。

# 链接 

[Theme.AppCompat.Light 继承关系图及Theme定制思路](http://www.jcodecraeer.com/a/anzhuokaifa/androidkaifa/2014/1124/2053.html)
