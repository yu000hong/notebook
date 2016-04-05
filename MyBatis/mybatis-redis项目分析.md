# mybatis-redis项目分析

redis作为现在最优秀的key-value数据库，非常适合提供项目的缓存服务。把redis作为mybatis的查询缓存也是很常见的做法。
在网上发现N多人是自己做的Cache，其实在mybatis的git下有一个子项目mybatis-redis；这个项目提供了redis作为mybatis查询
缓存的一个实现，下面先分析一下这个项目的实现原理，再提出几个项目的问题

### 代码实现

该项目和大家普遍实现Mybatis的缓存方案大同小异，无非是实现Cache接口，并使用jedis操作缓存；不过该项目在设计细节上
有一些区别；下面简要分析一下源码：

```java
public final class RedisCache implements Cache {  
public RedisCache(final String id) {
  if (id == null) {
  throw new IllegalArgumentException("Cache instances require an ID");
}
this.id = id;
RedisConfig redisConfig = RedisConfigurationBuilder.getInstance().parseConfiguration();
pool = new JedisPool(redisConfig, redisConfig.getHost(), redisConfig.getPort(),
        redisConfig.getConnectionTimeout(), redisConfig.getSoTimeout(), redisConfig.getPassword(),
        redisConfig.getDatabase(), redisConfig.getClientName());
}
```

RedisCache在mybatis启动的时候，由MyBatis的CacheBuilder创建，创建的方式很简单，就是调用RedisCache的带有String参数的
构造方法，即RedisCache(String id)；而在RedisCache的构造方法中，调用了RedisConfigurationBuilder来创建RedisConfig对象，
并使用RedisConfig来创建JedisPool。

RedisConfig类继承了JedisPoolConfig，并提供了host,port等属性的包装，简单看一下RedisConfig的属性：

```java
public class RedisConfig extends JedisPoolConfig {

private String host = Protocol.DEFAULT_HOST;
private int port = Protocol.DEFAULT_PORT;
private int connectionTimeout = Protocol.DEFAULT_TIMEOUT;
private int soTimeout = Protocol.DEFAULT_TIMEOUT;
private String password;
private int database = Protocol.DEFAULT_DATABASE;
private String clientName;
}
```

RedisConfig对象是由RedisConfigurationBuilder创建的，简单看下这个类的主要方法：

```java
public RedisConfig parseConfiguration(ClassLoader classLoader) {
    Properties config = new Properties();

    InputStream input = classLoader.getResourceAsStream(redisPropertiesFilename);
    if (input != null) {
        try {
            config.load(input);
        } catch (IOException e) {
            throw new RuntimeException(
                    "An error occurred while reading classpath property '"
                            + redisPropertiesFilename
                            + "', see nested exceptions", e);
        } finally {
            try {
                input.close();
            } catch (IOException e) {
                // close quietly
            }
        }
    }

    RedisConfig jedisConfig = new RedisConfig();
    setConfigProperties(config, jedisConfig);
    return jedisConfig;
}
```

核心的方法就是parseConfiguration方法，该方法从classpath中读取一个**`redis.properties`**文件：

```ini
host=localhost
port=6379
connectionTimeout=5000
soTimeout=5000
password=
database=0
clientName=
```

并将该配置文件中的内容设置到RedisConfig对象中，并返回；接下来，就是RedisCache使用RedisConfig类创建完成JedisPool；
在RedisCache中实现了一个简单的模板方法，用来操作Redis：

```java
private Object execute(RedisCallback callback) {
  Jedis jedis = pool.getResource();
  try {
    return callback.doWithRedis(jedis);
  } finally {
    jedis.close();
  }
}
```

模板接口为RedisCallback，这个接口中就只需要实现了一个doWithRedis方法而已：

```java
public interface RedisCallback {
    Object doWithRedis(Jedis jedis);
}
```

接下来看看Cache中最重要的两个方法：putObject和getObject，通过这两个方法来查看mybatis-redis储存数据的格式：

```java
@Override
public void putObject(final Object key, final Object value) {
  execute(new RedisCallback() {
    @Override
    public Object doWithRedis(Jedis jedis) {
      jedis.hset(id.toString().getBytes(), key.toString().getBytes(), SerializeUtil.serialize(value));
      return null;
    }
  });
}

@Override
public Object getObject(final Object key) {
  return execute(new RedisCallback() {
    @Override
    public Object doWithRedis(Jedis jedis) {
      return SerializeUtil.unserialize(jedis.hget(id.toString().getBytes(), key.toString().getBytes()));
    }
  });
}
```

可以很清楚的看到，mybatis-redis在存储数据的时候，是使用的hash结构，把cache的id作为这个hash的key（cache的
id在mybatis中就是mapper的namespace）；这个mapper中的查询缓存数据作为hash的field，需要缓存的内容直接使用SerializeUtil
存储，SerializeUtil和其他的序列化类差不多，负责对象的序列化和反序列化；

### 使用方式

整个mybatis-redis的关键代码就这些，通过对代码的分析，我们很容易得到mybatis-redis的使用方式：
- 1. 在项目中添加一个redis.properties配置；
- 2. 直接在mapper中使用<cache type="org.mybatis.caches.redis.RedisCache" />即可；

### 分析

通过代码，我们可以看到在实际应用当中可能存在的一些问题：

- 默认情况下，mybatis会为每一个mapper创建一个RedisCache，而JedisPool是在RedisCache的构造方法中创建的，
这就意味着会为每一个mapper创建一个JedisPool，使用意图和开销上面都会有问题；

- 在很多情况下，我们的应用中也会独立使用到redis，这样也无法让RedisCache直接使用我们项目中可能已经存在的JedisPool；
并且会造成两个配置文件（除非我们应用也使用redis.properties）；

- RedisCache是使用hash来缓存一个Mapper中的查询，所以我们只能通过mybatis的cache配置来控制对象的生存时间，
空闲时间等属性；而无法独立的去配置每一个缓存区域（即每一个hash）；

# 链接
原文链接：[文／小码哥Java学院（简书作者）](http://www.jianshu.com/p/648c10420df1)
