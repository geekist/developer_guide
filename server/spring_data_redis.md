
- [一、spring-boot-starter-data-redis介绍](#一spring-boot-starter-data-redis介绍)
- [二、引入Spring data redis依赖](#二引入spring-data-redis依赖)
- [三、配置redis服务器信息](#三配置redis服务器信息)
- [四、开始使用spring-data-redis](#四开始使用spring-data-redis)
- [五、自定义redisTemplate](#五自定义redistemplate)
- [六、封装redis操作类](#六封装redis操作类)
- [七、使用redis封装类](#七使用redis封装类)

# 一、spring-boot-starter-data-redis介绍

Spring Boot 提供了对 Redis 集成的组件包：spring-boot-starter-data-redis. 提供了配置连接池、连接器信息以及 key 和 value 的序列化方案。

spring-boot-starter-data-redis主要由两部分组成：spring-data-redis 和 lettuce.

Lettuce是redis数据库的java客户端，多个线程可以共享同一个 RedisConnection，它利用优秀 netty NIO 框架来高效地管理多个连接。在spring boot 2.0后，替代了jedis客户端。

spring-data-redis是Spring提供的操作redis的一个库，封装了对redis的连接、数据读取等操作，官网地址：https://spring.io/projects/spring-data-redis

# 二、引入Spring data redis依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

# 三、配置redis服务器信息

```yml
spring:  
  redis:
    database: 2
    host: r-bp16e3sx3zw4jrrbxzpd.redis.rds.aliyuncs.com
    port: 6379
    password: employ_2020
```
当开发者在项目中引入了 Spring Data Redis ，并且配置了Redis的基本信息，此时，自动化配置就会生效。spring-data-redis通过RedisProperties.class类读取spring.redis的配置，并自动实现数据库的连接。

# 四、开始使用spring-data-redis

引入依赖并配置了redis数据库后，就可以直接进行redis的操作了。

```java
@RestController
public class RedisController {

	@Autowired
	private StringRedisTemplate redisTemplate;

	@RequestMapping("/getRedis")
	public ResultJson getRedis() {
		redisTemplate.opsForValue().set("key1","value1");

		Employee employee = new Employee();
		emoloyee.setAge(30)；
		employee.setGender("male");
		employee.setName("Mask");
		employee.setPassword("123456");
		redisTemplate.opsForValue().set("employee",Json.toJsonString(employee));

		return ResultJson.data(redisTemplate.opsForValue().get("employee"));
	}
}

```

# 五、自定义redisTemplate

spring-data-redis默认情况下的模板只能支持 RedisTemplate<String,String>，只能存入字符串，很多时候，我们需要自定义 RedisTemplate ，设置序列化器，这样我们可以很方便的操作实例对象。如下所示：

```java
package com.yuya.common.config;

import com.fasterxml.jackson.annotation.JsonAutoDetect;
import com.fasterxml.jackson.annotation.PropertyAccessor;
import com.fasterxml.jackson.databind.ObjectMapper;
import org.springframework.cache.annotation.CachingConfigurerSupport;
import org.springframework.cache.annotation.EnableCaching;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.data.redis.connection.RedisConnectionFactory;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.data.redis.serializer.Jackson2JsonRedisSerializer;
import org.springframework.data.redis.serializer.StringRedisSerializer;

@Configuration
@EnableCaching//启用缓存
public class RedisConfig extends CachingConfigurerSupport{
	@Bean
	@SuppressWarnings("all")
	public RedisTemplate<String, Object> redisTemplate(RedisConnectionFactory factory) {

		RedisTemplate<String, Object> template = new RedisTemplate<String, Object>();

		template.setConnectionFactory(factory);

		Jackson2JsonRedisSerializer jackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer(Object.class);

		ObjectMapper om = new ObjectMapper();

		om.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);

//		om.enableDefaultTyping(ObjectMapper.DefaultTyping.NON_FINAL);

		jackson2JsonRedisSerializer.setObjectMapper(om);

		StringRedisSerializer stringRedisSerializer = new StringRedisSerializer();

		// key采用String的序列化方式

		template.setKeySerializer(stringRedisSerializer);

		// hash的key也采用String的序列化方式

		template.setHashKeySerializer(stringRedisSerializer);

		// value序列化方式采用jackson

		template.setValueSerializer(jackson2JsonRedisSerializer);

		// hash的value序列化方式采用jackson

		template.setHashValueSerializer(jackson2JsonRedisSerializer);

		template.afterPropertiesSet();

		return template;

	}
}
```

@Configruation标记这个是一个配置类，该类中的@Bean标注的对象会被spring boot扫描并自动管理，需要使用bean的时候直接加@Autowired注解就可以了。

@EnableCaching来开启缓存。如果定义了cacheManger，就可以用@Cacheable注解直接对缓存进行操作。

最后，提供了RedisTemplate,并设置采用string的序列化方式。

# 六、封装redis操作类

对redis的template做进一步的封装，可以更加方便使用redis。


```java

package com.yuya.common.config;

import java.util.Collection;
import java.util.List;
import java.util.Map;
import java.util.Objects;
import java.util.Set;
import java.util.concurrent.TimeUnit;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.stereotype.Service;
import org.springframework.util.CollectionUtils;

@Service
public class RedisService {
	@Autowired
	private RedisTemplate<String, Object> redisTemplate;

	// =============================common============================
	/**
	 * 指定缓存失效时间
	 * @param key 键
	 * @param time 时间(秒)
	 * @return
	 */
	public boolean expire(String key, long time) {
		try {
			if (time > 0) {
				redisTemplate.expire(key, time, TimeUnit.SECONDS);
			}
			return true;
		} catch (Exception e) {
			e.printStackTrace();
			return false;
		}
	}

	/**
	 * 根据key 获取过期时间
	 * @param key 键 不能为null
	 * @return 时间(秒) 返回0代表为永久有效
	 */
	public long getExpire(String key) {
		return redisTemplate.getExpire(key, TimeUnit.SECONDS);
	}

	/**
	 * 判断key是否存在
	 * @param key 键
	 * @return true 存在 false不存在
	 */
	public boolean hasKey(String key) {
		try {
			return redisTemplate.hasKey(key);
		} catch (Exception e) {
			e.printStackTrace();
			return false;
		}
	}

	/**
	 * 删除缓存
	 * @param key 可以传一个值 或多个
	 */
	@SuppressWarnings("unchecked")
	public void del(String... key) {
		if (key != null && key.length > 0) {
			if (key.length == 1) {
				redisTemplate.delete(key[0]);
			} else {
				redisTemplate.delete(CollectionUtils.arrayToList(key));
			}
		}
	}

	/**
	 * 删除缓存
	 * @param keys 可以传一个值 或多个
	 */
	@SuppressWarnings("unchecked")
	public void del(Collection keys) {
		if (org.apache.commons.collections4.CollectionUtils.isNotEmpty(keys)) {
			redisTemplate.delete(keys);
		}
	}
	
	/**
	 * 通过前缀删除
	 * @description: TODO
	 * @author: lzy
	 * @create: 2022年3月14日 
	 * @return: void
	 */
	public void deleteByPrex(String prex) {
        Set<String> keys = redisTemplate.keys(prex);
        if (org.apache.commons.collections4.CollectionUtils.isNotEmpty(keys)) {
            redisTemplate.delete(keys);
        }
    }

	// ============================String=============================
	/**
	 * 普通缓存获取
	 * @param key 键
	 * @return 值
	 */
	public Object get(String key) {
		return key == null ? null : redisTemplate.opsForValue().get(key);
	}

	public String getStr(String key) {
		if (key == null) {
			return null ;
		}
		Object obj = redisTemplate.opsForValue().get(key);
		return Objects.nonNull(obj)?obj.toString():null;
	}

	/**
	 * 普通缓存放入
	 * @param key 键
	 * @param value 值
	 * @return true成功 false失败
	 */
	public boolean set(String key, Object value) {
		try {
			redisTemplate.opsForValue().set(key, value);
			return true;
		} catch (Exception e) {
			e.printStackTrace();
			return false;
		}
	}

	/**
	 * 普通缓存放入并设置时间
	 * @param key 键
	 * @param value 值
	 * @param time 时间(秒) time要大于0 如果time小于等于0 将设置无限期
	 * @return true成功 false 失败
	 */
	public boolean set(String key, Object value, long time) {
		try {
			if (time > 0) {
				redisTemplate.opsForValue().set(key, value, time, TimeUnit.SECONDS);
			} else {
				set(key, value);
			}
			return true;
		} catch (Exception e) {
			e.printStackTrace();
			return false;
		}
	}

	/**
	 * 递增
	 * @param key 键
	 * @param delta 要增加几(大于0)
	 * @return
	 */
	public long incr(String key, long delta) {
		if (delta < 0) {
			throw new RuntimeException("递增因子必须大于0");
		}
		return redisTemplate.opsForValue().increment(key, delta);
	}

	/**
	 * 递减
	 * @param key 键
	 * @param delta 要减少几(小于0)
	 * @return
	 */
	public long decr(String key, long delta) {
		if (delta < 0) {
			throw new RuntimeException("递减因子必须大于0");
		}
		return redisTemplate.opsForValue().increment(key, -delta);
	}

	// ================================Map=================================
	/**
	 * HashGet
	 * @param key 键 不能为null
	 * @param item 项 不能为null
	 * @return 值
	 */
	public Object hget(String key, String item) {
		return redisTemplate.opsForHash().get(key, item);
	}

	/**
	 * 获取hashKey对应的所有键值
	 * @param key 键
	 * @return 对应的多个键值
	 */
	public Map<Object, Object> hmget(String key) {
		return redisTemplate.opsForHash().entries(key);
	}

	/**
	 * HashSet
	 * @param key 键
	 * @param map 对应多个键值
	 * @return true 成功 false 失败
	 */
	public boolean hmset(String key, Map<String, Object> map) {
		try {
			redisTemplate.opsForHash().putAll(key, map);
			return true;
		} catch (Exception e) {
			e.printStackTrace();
			return false;
		}
	}

	/**
	 * HashSet 并设置时间
	 * @param key 键
	 * @param map 对应多个键值
	 * @param time 时间(秒)
	 * @return true成功 false失败
	 */
	public boolean hmset(String key, Map<String, Object> map, long time) {
		try {
			redisTemplate.opsForHash().putAll(key, map);
			if (time > 0) {
				expire(key, time);
			}
			return true;
		} catch (Exception e) {
			e.printStackTrace();
			return false;
		}
	}

	/**
	 * 向一张hash表中放入数据,如果不存在将创建
	 * @param key 键
	 * @param item 项
	 * @param value 值
	 * @return true 成功 false失败
	 */
	public boolean hset(String key, String item, Object value) {
		try {
			redisTemplate.opsForHash().put(key, item, value);
			return true;
		} catch (Exception e) {
			e.printStackTrace();
			return false;
		}
	}

	/**
	 * 向一张hash表中放入数据,如果不存在将创建
	 * @param key 键
	 * @param item 项
	 * @param value 值
	 * @param time 时间(秒) 注意:如果已存在的hash表有时间,这里将会替换原有的时间
	 * @return true 成功 false失败
	 */
	public boolean hset(String key, String item, Object value, long time) {
		try {
			redisTemplate.opsForHash().put(key, item, value);
			if (time > 0) {
				expire(key, time);
			}
			return true;
		} catch (Exception e) {
			e.printStackTrace();
			return false;
		}
	}

	/**
	 * 删除hash表中的值
	 * @param key 键 不能为null
	 * @param item 项 可以使多个 不能为null
	 */
	public void hdel(String key, Object... item) {
		redisTemplate.opsForHash().delete(key, item);
	}

	/**
	 * 删除hash表中的值
	 * @param key 键 不能为null
	 * @param items 项 可以使多个 不能为null
	 */
	public void hdel(String key, Collection items) {
		redisTemplate.opsForHash().delete(key, items.toArray());
	}

	/**
	 * 判断hash表中是否有该项的值
	 * @param key 键 不能为null
	 * @param item 项 不能为null
	 * @return true 存在 false不存在
	 */
	public boolean hHasKey(String key, String item) {
		return redisTemplate.opsForHash().hasKey(key, item);
	}

	/**
	 * hash递增 如果不存在,就会创建一个 并把新增后的值返回
	 * @param key 键
	 * @param item 项
	 * @param delta 要增加几(大于0)
	 * @return
	 */
	public double hincr(String key, String item, double delta) {
		if (delta < 0) {
			throw new RuntimeException("递增因子必须大于0");
		}
		return redisTemplate.opsForHash().increment(key, item, delta);
	}

	/**
	 * hash递减
	 * @param key 键
	 * @param item 项
	 * @param delta 要减少记(小于0)
	 * @return
	 */
	public double hdecr(String key, String item, double delta) {
		if (delta < 0) {
			throw new RuntimeException("递减因子必须大于0");
		}
		return redisTemplate.opsForHash().increment(key, item, -delta);
	}

	// ============================set=============================
	/**
	 * 根据key获取Set中的所有值
	 * @param key 键
	 * @return
	 */
	public Set<Object> sGet(String key) {
		try {
			return redisTemplate.opsForSet().members(key);
		} catch (Exception e) {
			e.printStackTrace();
			return null;
		}
	}

	/**
	 * 根据value从一个set中查询,是否存在
	 * @param key 键
	 * @param value 值
	 * @return true 存在 false不存在
	 */
	public boolean sHasKey(String key, Object value) {
		try {
			return redisTemplate.opsForSet().isMember(key, value);
		} catch (Exception e) {
			e.printStackTrace();
			return false;
		}
	}

	/**
	 * 将数据放入set缓存
	 * @param key 键
	 * @param values 值 可以是多个
	 * @return 成功个数
	 */
	public long sSet(String key, Object... values) {
		try {
			return redisTemplate.opsForSet().add(key, values);
		} catch (Exception e) {
			e.printStackTrace();
			return 0;
		}
	}

	/**
	 * 将数据放入set缓存
	 * @param key 键
	 * @param values 值 可以是多个
	 * @return 成功个数
	 */
	public long sSet(String key, Collection values) {
		try {
			return redisTemplate.opsForSet().add(key, values.toArray());
		} catch (Exception e) {
			e.printStackTrace();
			return 0;
		}
	}

	/**
	 * 将set数据放入缓存
	 * @param key 键
	 * @param time 时间(秒)
	 * @param values 值 可以是多个
	 * @return 成功个数
	 */
	public long sSetAndTime(String key, long time, Object... values) {
		try {
			Long count = redisTemplate.opsForSet().add(key, values);
			if (time > 0)
			expire(key, time);
			return count;
		} catch (Exception e) {
			e.printStackTrace();
			return 0;
		}
	}

	/**
	 * 获取set缓存的长度
	 * @param key 键
	 * @return
	 */
	public long sGetSetSize(String key) {
		try {
			return redisTemplate.opsForSet().size(key);
		} catch (Exception e) {
			e.printStackTrace();
			return 0;
		}
	}

	/**
	 * 移除值为value的
	 * @param key 键
	 * @param values 值 可以是多个
	 * @return 移除的个数
	 */
	public long setRemove(String key, Object... values) {
		try {
			Long count = redisTemplate.opsForSet().remove(key, values);
			return count;
		} catch (Exception e) {
			e.printStackTrace();
			return 0;
		}
	}

	// ===============================list=================================
	/**
	 * 获取list缓存的内容
	 * @param key 键
	 * @param start 开始
	 * @param end 结束 0 到 -1代表所有值
	 * @return
	 */
	public List<Object> lGet(String key, long start, long end) {
		try {
			return redisTemplate.opsForList().range(key, start, end);
		} catch (Exception e) {
			e.printStackTrace();
			return null;
		}
	}

	/**
	 * 获取list缓存的长度
	 * @param key 键
	 * @return
	 */
	public long lGetListSize(String key) {
		try {
			return redisTemplate.opsForList().size(key);
		} catch (Exception e) {
			e.printStackTrace();
			return 0;
		}
	}

	/**
	 * 通过索引 获取list中的值
	 * @param key 键
	 * @param index 索引 index>=0时， 0 表头，1 第二个元素，依次类推；index<0时，-1，表尾，-2倒数第二个元素，依次类推
	 * @return
	 */
	public Object lGetIndex(String key, long index) {
		try {
			return redisTemplate.opsForList().index(key, index);
		} catch (Exception e) {
			e.printStackTrace();
			return null;
		}
	}

	/**
	 * 将list放入缓存
	 * @param key 键
	 * @param value 值
	 * @return
	 */
	public boolean lSet(String key, Object value) {
		try {
			redisTemplate.opsForList().rightPush(key, value);
			return true;
		} catch (Exception e) {
			e.printStackTrace();
			return false;
		}
	}

	/**
	 * 将list放入缓存
	 * @param key 键
	 * @param value 值
	 * @param time 时间(秒)
	 * @return
	 */
	public boolean lSet(String key, Object value, long time) {
		try {
			redisTemplate.opsForList().rightPush(key, value);
			if (time > 0)
			expire(key, time);
			return true;
		} catch (Exception e) {
			e.printStackTrace();
			return false;
		}
	}

	/**
	 * 将list放入缓存
	 * @param key 键
	 * @param value 值
	 * @return
	 */
	public boolean lSet(String key, List<Object> value) {
		try {
			redisTemplate.opsForList().rightPushAll(key, value);
			return true;
		} catch (Exception e) {
			e.printStackTrace();
			return false;
		}
	}

	/**
	 * 将list放入缓存
	 *
	 * @param key 键
	 * @param value 值
	 * @param time 时间(秒)
	 * @return
	 */
	public boolean lSet(String key, List<Object> value, long time) {
		try {
			redisTemplate.opsForList().rightPushAll(key, value);
			if (time > 0)
			expire(key, time);
			return true;
		} catch (Exception e) {
			e.printStackTrace();
			return false;
		}
	}

		/**
		 * 根据索引修改list中的某条数据
		 * @param key 键
		 * @param index 索引
		 * @param value 值
		 * @return
		 */
	public boolean lUpdateIndex(String key, long index, Object value) {
		try {
			redisTemplate.opsForList().set(key, index, value);
			return true;
		} catch (Exception e) {
			e.printStackTrace();
			return false;
		}
	}

		/**
		 * 移除N个值为value
		 * @param key 键
		 * @param count 移除多少个
		 * @param value 值
		 * @return 移除的个数
		 */
	public long lRemove(String key, long count, Object value) {
		try {
			Long remove = redisTemplate.opsForList().remove(key, count, value);
			return remove;
		} catch (Exception e) {
			e.printStackTrace();
			return 0;
		}
	}
	
	public String rpop(String key) {
		try {
			Object rpop = redisTemplate.opsForList().rightPop(key);
			redisTemplate.opsForList().remove(key, -1, rpop);
			return (String)rpop;
		} catch (Exception e) {
			e.printStackTrace();
			return null;
		}
	}
}
```

# 七、使用redis封装类


加载redis的数据推荐采用懒加载的方式，当数据库数据发生变化时，删除redis对应的数据；读取数据时，首先从redis数据库读取，如果存在则返回数据。如果不存在，从数据库读取数据，然后写入到redis数据库中。


```java

package com.yuya.console.service.organ.impl;

......

import com.yuya.common.config.RedisService;

@Service
@DS("user")
public class UAppVersionServiceImpl implements UAppVersionService  {

     @Autowired
     UAppVersionMapper appVersionMapper;
     
 	@Autowired
 	private RedisService redisService;

	@Override
	public int insert(UAppVersion appVersion) {
		appVersion.setId(null);
		//删除缓存
		redisService.del("jwt:app:version:token:_"+appVersion.getPhoneSystem()+"_"+appVersion.getClient());
		appVersion.setReleaseTime(new Date());
		return appVersionMapper.insert(appVersion);
	}

	@Override
	public int update(UAppVersion appVersion) {
		//删除缓存
		redisService.del("jwt:app:version:token:_"+appVersion.getPhoneSystem()+"_"+appVersion.getClient());
		appVersion.setReleaseTime(new Date());
		return appVersionMapper.updateByPrimaryKeySelective(appVersion);
	}

    //加载采用懒加载方式
    @Override
	public UAppVersion getAppVersion(@Valid UAppVersionInfoDto versionDto) {
		Object uAppVersion = redisService.get("jwt:app:version:token:_"+versionDto.getPhoneSystem()+"_"+versionDto.getClient());
        if(uAppVersion!=null && Objects.nonNull(uAppVersion)){
            return JSONObject.parseObject(uAppVersion+"", UAppVersion.class);
        }

        UAppVersion appVersion = appVersionMapper.findByParam(versionDto.getPhoneSystem(),versionDto.getClient());
        redisService.set("jwt:app:version:token:_"+versionDto.getPhoneSystem()+"_"+versionDto.getClient(), JSONObject.toJSON(appVersion).toString());
        //是否强制更新
        
        Integer status = appVersionMapper.findStatusByParam(versionDto.getPhoneSystem(),versionDto.getClient(),versionDto.getVersion());
        appVersion.setStatus(status);
        return appVersion;
	}
}
```
