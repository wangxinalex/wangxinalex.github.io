---
layout: post
title:  "Spring Cache"
date:   2022-07-18 12:28:18 +0800
categories: Spring Java
---
# Spring Cache

## Enable Cache

```java
@Configuration
@EnableCaching
public class CachingConfig {

    @Bean
    public CacheManager cacheManager() {
        return new ConcurrentMapCacheManager("addresses");
    }
}
```

### Using SpringBoot

When using Spring Boot, the mere presence of the starter package on the classpath alongside the `EnableCaching` annotation would register the same `ConcurrentMapCacheManager`. So there is no need for a separate bean declaration.

## Use Caching With Annotations

### @Cacheable

```java
@Cacheable("addresses")
public String getAddress(Customer customer) {...}
```

The getAddress() call will first check the cache addresses before actually invoking the method and then caching the result.

- The default key is "", meaning **all method parameters** are considered as a key, unless a custom keyGenerator has been configured.

In this case, if any of the caches contain the required result, the result is returned and **the method is not invoked**.

### @CacheEvict

We can use the @CacheEvict annotation to indicate the **removal of one or more/all values** so that fresh values can be loaded into the cache again:

```java
@CacheEvict(value="addresses", allEntries=true)
public String getAddress(Customer customer) {...}
```

Here we're using the additional parameter allEntries in conjunction with the cache to be emptied; this will **clear all the entries** in the cache addresses and prepare it for new data.

### @CachePut

With the `@CachePut` annotation, we can update the content of the cache without interfering with the method execution. That is, the **method will always be executed and the result cached**:

```java
@CachePut(value="addresses")
public String getAddress(Customer customer) {...}
```

The difference between @Cacheable and @CachePut is that @Cacheable will skip running the method, whereas @CachePut will **actually run the method and then put its results in the cache**.

### @Caching

```java
@Caching(evict = { 
  @CacheEvict("addresses"), 
  @CacheEvict(value="directory", key="#customer.name") })
public String getAddress(Customer customer) {...}
```

### @CacheConfig

With the @CacheConfig annotation, we can streamline some of the cache configuration **into a single place at the class level**, so that we don't have to declare things multiple times:

```java
@CacheConfig(cacheNames={"addresses"})
public class CustomerDataService {

    @Cacheable
    public String getAddress(Customer customer) {...}
```

## Conditional Caching

### Conditional Parameter

```java
@CachePut(value="addresses", condition="#customer.name=='Tom'")
public String getAddress(Customer customer) {...}
```

### Unless Parameter

```java
@CachePut(value="addresses", unless="#result.length()<64")
public String getAddress(Customer customer) {...}
```

## Pitfalls

1. Not working while calling from another method of the **same bean**.

- https://stackoverflow.com/questions/12115996/spring-cache-cacheable-method-ignored-when-called-from-within-the-same-class
- https://spring.io/blog/2012/05/23/transactions-caching-and-aop-understanding-proxy-usage-in-spring
- https://stackoverflow.com/a/16899739/1501494
- https://stackoverflow.com/a/5251930/1501494

```java
@Service
@Transactional(readOnly=true)
public class SettingServiceImpl implements SettingService {

@Inject
private SettingRepository settingRepository;

@Inject
private ApplicationContext applicationContext;

@Override
@Cacheable("settingsCache")
public String findValue(String name) {
    Setting setting = settingRepository.findOne(name);
    if(setting == null){
        return null;
    }
    return setting.getValue();
}

@Override
public Boolean findBoolean(String name) {
    String value = getSpringProxy().findValue(name);
    if (value == null) {
        return null;
    }
    return Boolean.valueOf(value);
}

/**
 * Use proxy to hit cache 
 */
private SettingService getSpringProxy() {
    return applicationContext.getBean(SettingService.class); //通过这种方式获取bean
}
```