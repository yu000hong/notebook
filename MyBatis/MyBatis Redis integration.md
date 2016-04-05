# MyBatis Redis integration

**`Redis`** is an in-memory key-value store for small chunks of arbitrary data (strings, objects) from results of 
database calls, API calls, or page rendering..

The Redis integration is built on top of the **`Jedis`** client, written by Jonathan Leibiusky.

Users that want to use Redis into their applications, have to download the zip bundle, decompress it and add the jars 
in the classpath; Apache Maven users instead can simply add in the **`pom.xml`** the following dependency:

```xml
<dependencies>
  ...
  <dependency>
    <groupId>org.mybatis.caches</groupId>
    <artifactId>mybatis-redis</artifactId>
    <version>1.0.0-beta2</version>
  </dependency>
  ...
</dependencies>
```

then, just configure it in the mapper XML

```xml
<mapper namespace="org.acme.FooMapper">
  <cache type="org.mybatis.caches.redis.RedisCache" />
  ...
</mapper>
```

The Redis cache is configurable by putting the **`/redis.properties`** classpath resource; if not found, the client 
will use the default setting.

Any property found in the configuration file will be set as a property to the bean `JedisConfigPool`. There is 
an additional parameter host to set the Redis server host name; if not set "localhost" will be used.

# 链接

[MyBatis Redis integration - Reference Documentation](http://www.mybatis.org/redis-cache/)
