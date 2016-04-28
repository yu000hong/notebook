# bundle & gem install fails with SSL certificate verification error

### Question

When I run bundle install for my Rails 3 project on Centos 5.5 it fails with an error:

```
Gem::RemoteFetcher::FetchError: SSL_connect returned=1 errno=0 state=SSLv3 
read server certificate B: certificate verify failed 
(https://bb-m.rubygems.org/gems/multi_json-1.3.2.gem)
An error occured while installing multi_json (1.3.2), and Bundler cannot continue.
Make sure that `gem install multi_json -v '1.3.2'` succeeds before bundling.
```

When I try to install the gem manually (by gem install multi_json -v '1.3.2') it works. 
The same problem occurs with several other gems. I use RVM (1.12.3), ruby 1.9.2, bundler 1.1.3.

How to fix it?

### Answer

> **for gem**

The reason is old rubygems. So we need to remove ssl source to be able to update gem --system which includes 
rubygems and so on. after this we can feel free to get back to ssl source.

`gem sources -r https://rubygems.org/` - to temporarily remove secure connection

`gem sources -a http://rubygems.org/` - add insecure connection

`gem update --system` - now we're able to update rubygems without SSL

after updating rubygems do vice versa

`gem sources -r http://rubygems.org/` - to remove insecure connection

`gem sources -a https://rubygems.org/` - add secure connection

Now you're able to update gems using secure connection.

`gem update`

> **for bundle**

Honestly the best temporary solution is to use the non-ssl version of rubygems in your `gemfile` as a temporary workaround.
what they mean is at the top of the Gemfile in your rails application directory change:

`source 'https://rubygems.org'`

to

`source 'http://rubygems.org'`

note that the second version is http instead of https.

# 链接

[bundle & gem install fails with SSL certificate verification error](http://stackoverflow.com/questions/10246023/bundle-install-fails-with-ssl-certificate-verification-error)
