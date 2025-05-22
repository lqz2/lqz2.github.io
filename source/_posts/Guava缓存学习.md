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
