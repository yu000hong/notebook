# RubyGems 镜像 - 淘宝网

### 为什么有这个？

由于国内网络原因（你懂的），导致 **rubygems.org** 存放在 Amazon S3 上面的资源文件间歇性连接失败。所以你会与遇到 
`gem install rack` 或 `bundle install` 的时候半天没有响应，具体可以用 `gem install rails -V` 来查看执行过程。

> 这是一个完整 rubygems.org 镜像，你可以用此代替官方版本，同步频率目前为15分钟一次以保证尽量与官方服务同步。

### 如何使用？

```bash
$ gem sources --add https://ruby.taobao.org/ --remove https://rubygems.org/
$ gem sources -l
*** CURRENT SOURCES ***

https://ruby.taobao.org
# 请确保只有 ruby.taobao.org
$ gem install rails
```

### 如果你使用 Gemfile 和 Bundle (例如：Rails 项目)

你可以用 Bundler 的 [Gem 源代码镜像命令](http://bundler.io/v1.5/bundle_config.html#gem-source-mirrors)。

```bash
$ bundle config mirror.https://rubygems.org https://ruby.taobao.org
```

这样你不用改你的 Gemfile 的 source。

```bash
source 'https://rubygems.org/'
gem 'rails', '4.1.0'
...
```

### Ruby 源代码镜像

本镜像来源于 `cache.ruby-lang.org` 用于改善国内 Ruby 安装的速度。

修改 RVM ，改用本站作为下载源, 提高安装速度。

> FOR MAC

```bash
$ sed -i .bak -E 's!https?://cache.ruby-lang.org/pub/ruby!https://ruby.taobao.org/mirrors/ruby!' $rvm_path/config/db
```

> FOR LINUX

```bash
$ sed -i -E 's!https?://cache.ruby-lang.org/pub/ruby!https://ruby.taobao.org/mirrors/ruby!' $rvm_path/config/db
```

### 常见问题

Q: 为何我新发布的 Gem 在淘宝源上面无法安装？

A: 由于同步是定期执行的，新发布的 Gem 可能没有那么快同步过来，你需要稍等一段时间后才能使用。

Q: 已经换成淘宝源了，但 bundle install 或 gem install xxx 的时候卡住很久不动？

A: 这有可能是你网络问题，或者没有正确的好 gem 的源，你可以尝试 gem install xxx -V 并把执行过程的结果在 Ruby China 上面发帖求助。

Q: gem install xxx 的时候遇到错误信息包含：“Error fetching data: Errno::ETIMEDOUT: Operation timed out - connect(2)”

A: 网络问题导致请求淘宝服务器被连接重置了，在遇到此类情况的时候，你可以尝试换一台机器或网络尝试安装，看是否还有同样的问题，以确定是淘宝镜像服务器的问题还是你的环境问题，如果你换了环境仍然有问题，请上 Ruby China 发帖求助。

# 链接

[RubyGems 镜像 - 淘宝网](https://ruby.taobao.org/)
