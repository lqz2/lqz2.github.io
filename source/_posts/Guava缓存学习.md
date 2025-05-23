---
title: Guava缓存学习
date: 2025-05-22 16:52:05
categories: 学习
math:
tags:
---

# Guava缓存学习

## Guava简介
Guava是Google封装的基础工具包，主要用于**本地缓存**，提供了以下能力：
- 封装了缓存与数据源的交互流程，使得开发更关注于业务操作
- 提供线程安全的存取操作，类比ConcurrentHashMap
- 提供了多种缓存过期策略，刷新策略
- 提供缓存命中率的监控

## 基础使用
下面是一个简单的示例：
```
private String convert(String key) {
    return key.toUpperCase();
}


@Test
public void testCache() {
    Cache<String, String> cache = CacheBuilder.newBuilder()
            .build(new CacheLoader<String, String>() {
                @Override
                public String load(String key) throws Exception {
                    return convert(key);
                }
            });
    assertEquals("HELLO", cache.getUnchecked("hello"));
    assertEquals("HELLO", cache.get("hello"));
    assertEquals(1, cache.size());
}
```
上面的load方法可以理解为从数据源加载数据的过程。

当调用`getUnchecked`方法或者`get`方法时，Guava的行为如下：
- 如果缓存命中，则直接返回
- 缓存未命中，会调用load接口，加载进缓存，返回缓存值
- 多线程下，A线程load时，会阻塞B线程的请求，直到缓存加载完毕

## 预加载缓存

预加载缓存一般用于以下场景：
- 秒杀场景，事先将热点商品加载到缓存
- 系统启动时，预加载一些数据到缓存

预加载缓存一般用put：
```
cache.put("key", "value");
```
上面的put方法会直接将数据放入缓存中，不会调用load方法。
>不要使用get方法来预加载数据，因为get方法会调用load方法，可能会导致性能问题。

## 缓存过期

### 缓存固定数量的值
通过maximumSize方法设置缓存的最大值，当缓存超过最大值时，Guava会自动进行缓存淘汰。
```
@Test
public void maxSizeTest(){
    Cache<String, String> cache = CacheBuilder.newBuilder()
            .maximumSize(2)
            .build(new CacheLoader<String, String>() {
                @Override
                public String load(String key) throws Exception {
                    return convert(key);
                }
            });
    cache.put("key1", "value1");
    cache.put("key2", "value2");
    assertEquals(2, cache.size());
    cache.getIfPresent("key1");
    cache.put("key3", "value3");
    assertEquals(2, cache.size());
}
```
这里的`getIfPresent()`与`getUnchecked()`和`get()`的区别在于，当缓存未命中时，`getIfPresent()`不会调用load方法，而是返回null。

### LRU过期策略
Guava Cache 默认采用LRU（Least Recently Used）算法来淘汰缓存中的数据。LRU算法会优先淘汰最久未使用的数据。
上面的例子中，key2会被淘汰，因为key2是最久未使用的。
### 缓存固定时间
Guava Cache还可以给缓存设置过期时间，常见的有：
- expireAfterAccess：访问后多久过期
- expireAfterWrite：写入后多久过期

## 移除缓存
Guava Cache中移除缓存主要有两种方式：
- cache.invalidate(key)：移除单个缓存
- cache.invalidateAll()：移除所有缓存

## 缓存刷新

### 手动刷新
通过refresh()方法手动刷新缓存，refresh()方法会调用load方法，重新加载数据到缓存中。

需要注意的是：refresh()方法不会阻塞get方法，所以refresh期间，get方法会返回旧的缓存值。

### 定时刷新
通过refreshAfterWrite方法设置定时刷新时间，Guava会在指定时间后自动刷新缓存。

## 缓存命中统计
Guava Cache提供了缓存命中率的统计功能，可以通过recordStats()方法来记录缓存命中率。

```
@Test
public void testCacheStats() {
    Cache<String, String> cache = CacheBuilder.newBuilder()
            .maximumSize(2)
            .recordStats()
            .build(new CacheLoader<String, String>() {
                @Override
                public String load(String key) throws Exception {
                    return convert(key);
                }
            });
    cache.put("key1", "value1");
    cache.put("key2", "value2");
    CacheStats stats = cache.stats();
    System.out.println("Cache Stats: " + stats);
}
```

## 缓存移除通知
如果希望在缓存失效或者被移除时进行一些回调处理，可以使用removalListener方法来添加移除通知。
```
@Test
public void testCacheRemovalListener() {
    Cache<String, String> cache = CacheBuilder.newBuilder()
            .maximumSize(2)
            .removalListener(cacheItem -> {
                System.out.println(cacheItem+" was removed"+"caused by "+cacheItem.getCause());
            })
            .build(new CacheLoader<String, String>() {
                @Override
                public String load(String key) throws Exception {
                    return convert(key);
                }
            });
    cache.put("key1", "value1");
    cache.put("key2", "value2");
    cache.invalidate("key1");
}
```
## 常见问题
### 为什么不用redis？
如果本地缓存能解决，没必要引入一个中间件


### 如何保证缓存和数据源一致性？
- 一般在数据敏感度不高的场景，短暂的不一致可以接受
- 另外的情况，可以设置定期刷新缓存或者手动刷新缓存的机制，比如给页面添加一个刷新按钮，当数据源更新后刷新缓存，或者定期刷新缓存
- 通过配置中心协调和同步