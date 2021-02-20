---
title: 在Spring中获取当前环境字符串
date: 2020-11-12 00:00:00
categories: Programming
tags:
	- Spring
	- Bean
---
###  Spring程序在运行时获取激活的配置
``` java
package com.demo;

import lombok.val;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.ApplicationContext;
import org.springframework.test.context.ActiveProfiles;
import org.springframework.test.context.junit4.SpringRunner;

@RunWith(SpringRunner.class)
@ActiveProfiles({"dev"})
public class ProfileTest {
    @Autowired
    ApplicationContext applicationContext;

    @Test
    public void test()
    {
        val res = applicationContext.getEnvironment().getActiveProfiles()[0];
        System.out.println(res);
    }
}
```
输出
`dev`