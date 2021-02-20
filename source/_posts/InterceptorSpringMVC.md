---
title: Spring MVC 拦截器规则的“例外”问题
date: 2021-01-17
tags:
	- Java
categories:
	- Programming
---

## 问题描述

在一个Spring boot 1.5.x工程（业务逻辑，URL等均已变更成不影响问题描述和解决但是和原工程毫无关系）中，我们有如下几个Controller的URL

``` Java
SALARY_IMPORT_URL = "/api/hr/salary/import"
SALARY_EXPORT_URL = "/api/hr/salary/export"
# 其他URL
```

由于一些原因，我们的项目中使用了拦截器（用于校验登录信息），原拦截逻辑如下：

1. 默认拦截所有URL
2. 所有``"/api"``开头的URL添加例外

因为业务需要，我们需要校验访问`SALARY_IMPORT_URL`和`SALARY_EXPORT_URL`URL的权限，所以新的拦截逻辑应当如下：

1. 默认拦截所有URL
2. 所有``"/api"``开头的URL添加例外
3. ***``"/api/hr/salary/import"``和``"/api/hr/salary/export"``需要拦截***

显然，逻辑3和逻辑2有干涉

原代码

``` java
interceptorRegistry.addInterceptor(loginInterceptor)
    .addPathPatterns("/**")
    .excludePathPatterns("/api/**");

interceptorRegistry.addInterceptor(loginInterceptor).addPathPatterns(whiteList);

```

一开始的想法

``` java
SALARY_IMPORT_URL = "/api/hr/salary/import"
SALARY_EXPORT_URL = "/api/hr/salary/export"

interceptorRegistry.addInterceptor(loginInterceptor)
    .addPathPatterns("/**")
    .excludePathPatterns("/api/**")
    .addPathPatterns(SALARY_IMPORT_URL)
    .addPathPatterns(SALARY_EXPORT_URL);

interceptorRegistry.addInterceptor(loginInterceptor).addPathPatterns(whiteList);
```

然而这两个URL依然没有被拦截，通过多方查证，在同事的帮助下，终于抽干了脑子里的水。

添加后的代码

``` java
SALARY_IMPORT_URL = "/api/hr/salary/import"
SALARY_EXPORT_URL = "/api/hr/salary/export"

interceptorRegistry.addInterceptor(loginInterceptor)
    .addPathPatterns("/**")
    .excludePathPatterns("/api/**");
interceptorRegistry.addInterceptor(loginInterceptor).addPathPatterns(SALARY_IMPORT_URL,SALARY_EXPORT_URL);
```

成功添加了“例外”。

一些基本原理之类的之后补，太晚了要睡觉了~

## 参考资料

1. [Spring doc](https://docs.spring.io/spring-framework/docs/current/reference/html/web.html#mvc-config-interceptors)

