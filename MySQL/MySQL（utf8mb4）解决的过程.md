# MySQL（utf8mb4）解决的过程

### 问题

自己的练习项目中涉及保存微信的nickname，之前一直正常使用，但是突然遇到一个之前没有遇到的问题。经过调试发现错误如下：

> Incorrect string value: '\xF0\x9F\x99\x88\xF0\x9F...' for column 'nickname' at row 1

经过仔细查看发现可以获得nickname的数据，但是无法保存到mysql数据库，查看用户的微信发现在nickname中使用了emoji字符。
到百度（只能用这个，其他的麻烦呀。）上查找发现主要解决方案就是MySQL的编码设置由utf8转为**`utf8mb4`**。
具体解释可见：[详细emoji表情与utf8mb4的关系](http://my.oschina.net/wingyiu/blog/153357) ，写的非常全面详细。

网上的解决办法大多是修改my.cnf参数，设置mysql的编码为utf8mb4，这种方法虽然彻底，但是通常要重启mysql，
会造成生产系统临时当机。我认为写的比较好的方法是：[mysql/Java服务端对emoji的支持](https://segmentfault.com/a/1190000000616820)，
一般可参考以上方法。文章中的关键点也说的比较清楚。

### 解决

下面是我的处理方法：

- MySQL的版本不能太低，低于5.5.3的版本不支持utf8mb4编码

- JDBC驱动版本不能太低，mysql connector版本高于5.1.13
```xml
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.38</version>
</dependency>
```

- 将表中的对应字段，比如会员表的呢称字段，其字符集修改成**`utf8mb4`**

- 最后修改数据源的配置，增加一行：
```xml
<property name="connectionInitSqls" value="set names utf8mb4;"/>
```

- 检查下jdbc连接串的设置：
jdbc:mysql://localhost:3306/dbname?useUnicode=true&characterEncoding=utf8

# 链接

[MySQL（utf8mb4）解决的过程](https://segmentfault.com/a/1190000004594385)
