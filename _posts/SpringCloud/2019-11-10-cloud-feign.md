---
layout: post
title: feign的使用
category: other
tags: [other]
---
# feign的使用

feign做cloud组件可以在同一个注册中心可以调用，也可以调用外部接口，使用起来特别方便。比HTTP请求方便许多。

配置feign打印出日志文件.在application.yml配置

feign:

    client.config.服务名（feignCilent中name对应自己定义）.loggerLevel: full
创建feign接口源码如下

```
package com.isc.claimplus.feign.foreignsystem;

import com.alibaba.fastjson.JSONObject;
import org.springframework.cloud.openfeign.FeignClient;
import org.springframework.http.HttpEntity;
import org.springframework.http.HttpMethod;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;

@FeignClient(url=" .com.cn",name="服务名")
public interface ElectronicSignFeign {
    /**
     * 
     * @param param
     * @return
     */
    @PostMapping(value = "调用具体地址",consumes = {"application/json;charset=UTF-8"})
    String callTechPlatform(@RequestBody String param);
}
```

配置请求格式参数：consumes

配置接收格式参数：product

@postMapping 请求post请求

url和value是请求地址

注意请求方式和格式。