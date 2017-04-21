---
date: 2017-04-21 9:25:31+00:00
layout: post
title: .NET中Web API配置、注释的几个问题 
categories: 文档
tags: 问题 Web API
---

## System.Web.Http.GlobalConfiguration 并不包含“Configure”的定义

新建WebAPI项目，代码 GlobalConfiguration.Configure(WebApiConfig.Register); 编译时会提示：System.Web.Http.GlobalConfiguration 并不包含“Configure”的定义 实际是因为vs2013默认没有安装webapi的包，解决方法如下： 通过 工具-\>库程序包管理器-\>程序包管理器控制台 打开 管理控制台 在控制台输入如下命令： Install-Package Microsoft.AspNet.WebApi.WebHost 成功执行后重新编译即可。

## 注释
1. 工程属性设置 - 生成 - 输出 - 
    - [x]  XML文档文件 - 配置 - App_Data/Haogege.API.XML
2. HelpPageConfig - 取消注释
    config.SetDocumentationProvider(new XmlDocumentationProvider(HttpContext.Current.Server.MapPath("~/App_Data/Haogege.API.XML"))