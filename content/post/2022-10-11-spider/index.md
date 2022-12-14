---
title: Spider
author: 王彪
date: '2022-10-11'
slug: spider
categories:
  - R
tags:
  - plot
---
## 1.爬虫在使用场景中的分类

​	(1)通用爬虫：抓取系统的重要组成部分。抓取的是一整张页面的数据。

​	(2)聚焦爬虫：是建立在通用爬虫的基础之上。抓取的是页面中特定的局部内容。

​	(3)增量式爬虫：检测网站中数据更新的情况。只会抓取网站中最新更新出来的数据。

## 2.爬虫的矛与盾

​	(1)反爬机制：门户网站，可以通过制定相应的政策或技术手段，防止爬虫程序进行网站数据的爬取。

​	(2)反反爬策略：爬虫程序可以通过制定相关的策略或技术手段，破解门户网站中具备的反爬机制，从而可以获取门户网站的数据。

## 3.robots.txt协议（君子协议）

​	规定了网站中哪些数据可以被爬虫爬取，哪些数据不可以被爬取。

## 4.http协议

​	概念：就是服务器和客户端进行数据交互的一种形式

## 5.常用请求头信息

​	—User-Agent：请求载体的身份标识

​	—Connection:请求完毕后，是断开连接还是保持连接

## 6.常用响应头信息

—Content-Type:服务器响应回客户端的数据类型

## 7.https协议

—安全的超文本传输协议(http)

## 8.加密方式

1. **对称秘钥加密**（客户端对一个数据进行某种方式加密后，将数据与秘钥一起发送给服务器）

   缺点：可能在发送过程中密文和秘钥会被拦截

2. **非对称秘钥加密**（服务器自己制定一种加密方式（秘钥），然后将秘钥发送给客户端，客户端通过该秘钥对数据进行加密，最后只将密文发送给服务器）

   缺点：（1）效率比较低

   ​             （2）可能发送秘钥的时候被拦截后进行了篡改，客户端收到的秘钥可能不是服务器所发送的。只要是发送秘钥，就有可能有被劫持的风险

3. —**证书秘钥加密**（服务器制定一个公钥（秘钥）后，先发送给2者都信任的证书认证机构进行签名加到证书中，再将证书发送给客户端）   https主要就用这个加密方式

## 9.requests模块

  它是python中原生的一款基于网络请求的模块，功能非常强大，简单便捷，效率极高。

作用：模拟浏览器发送请求

如何使用：（requests模块的编码流程）

​		—指定url

​		—发送请求

​		—获取响应数据

​		—数据解析（聚焦爬虫中才有这步）

​		—持久化存储

## 10.UA检测

门户网站的服务器会检测对应请求载体的身份标识，如果检测到请求载体身份标识为某一款浏览器，说明该请求是一个正常的请求。但是如果检测到请求的载体身份标识不是基于某一款浏览器，则标识该请求为不正常的请求（爬虫），则服务器端可能拒绝该次请求。

**UA伪装**：让爬虫对应的请求载体的身份标识伪装成某一款浏览器

## 11.数据解析分类：

​	—正则

​	—bs4

​	—xpath

## 12.数据解析原理概述：

​	—解析的局部的文本的内容都会在标签之间或者对应的属性中进行存储

​	—1.进行指定标签的定位

​	—2.标签或者标签对应的属性中存储的数据值进行提取（解析）

## 13.常用正则表达式

单字符：

​		. ：除换行以外所有字符

​		[] : 匹配集合中任意一个字符 。 如[0-5]只能写入0到5的几个自然数，不可以写入其他的

​		\d : 数字[0-9]

​		\D : 非数字

​		\w : 数字，字母，下划线，中文

​		\W : 非\w

​		\s : 所有的空白字符包，包括空格，制表符，换行符等

​		\S : 非空白

数量修饰：

​		*: 任意多次    >=0

​		+: 至少一次    >=1

​		? : 可有可无    0次或者1次

​		{m} : 固定m次

​		{m,} : 至少m次

​		{m,n} : m-n次（m到n次）

边界：

​		$ : 以某某结尾

​		^ : 以某某开头

分组：

​		() : 分组

贪婪模式 : .*

非贪婪（惰性）模式 : .*?

re.I : 忽略大小写

re.M : `多行匹配`

re.S : 单行匹配

## 14.bs4数据解析（python专有）的原理

1. 实例化一个BeautifulSoup对象，并且将页面源码数据加载到该对象中
2. 通过调用BeautifulSoup对象中相关的属性或者方法进行标签定位和数据提取

### 14.1.如何实例化BeautifulSoup对象

1. 将本地的html文档加载到该对象中

   ```python
   fp=open('./text.html','r',encoding='utf-8')
   soup=BeautifulSoup(fp,'lxml')
   ```

2. 将互联网上获取的页面源码加载到该对象中

   ```python
   page_text=response.text
   soup=BeautifulSoup(page_text,'lxml')
   ```

   [^lxml]: 是一个解析器

   

### 14.2.用于数据解析的方法和属性

1. soup.tagName:返回的是文档中第一次出现的tagName标签

2. soup.find:

   1. soup.find('tagName')  等同于soup.tagName

   2. 属性定位：

      1. ```
         soup.find('a',href="vs.html")
         ```

3. soup.find_all('tagNmae') :返回符合要求的所有标签（结果为一个列表）

4. select:

   1. soup.select('某种选择器（id(#),class(.),标签选择器）')。返回一个列表

   2. 层级选择器：

      1. ```python
         soup.select('#web前端>p')[1]   >号表示一个层级
         
         soup.select('#web前端 p')[1]  空格表示多个层级
         ```

5. 如何获取标签之间的文本数据：

   1. soup.a.text/get_text()/string
      1. get_text()/text:可以获取某一标签中所有的文本内容
      2. string:只可以获取该标签下面直系的文本内容

6. 获取标签中的属性值

   1. soup.a['href']

      

## 15.xpath解析

​	—最常用且最便捷高效的一种解析方法。通用性。

###### 解析原理：

1. 实例化一个etree的对象，且需要将被解析的页面源码数据加载到该对象中
2. 调用etree对象中的xpath方法并结合着xpath表达式实现标签的定位和内容的捕获      (xpath('xpath表达式'))

### 15.1.如何实例化一个etree对象

​	—from lxml import etree

1. 将本地的html文档中的源码数据加载到etree对象中

   etree.parse(filePath)

2. 将互联网上获取的源码数据加载到该对象中

   etree.HTML('page_text')

### 15.2.xpath表达式（最后返回的都是列表）

1. / :表示的是从根节点开始定位。表示的是一个层级。
2. // :可以表示从任意位置开始定位。表示多个层级。
3. 属性定位：div[@id="web前端"]
4. 索引定位：div[@id="web前端"]/p[1]     索引从1开始不是0
5. 取标签之间的文本：
   1.  /text()		获取的是标签中直系的文本内容
   2.  //text()       获取的是标签中非直系的文本内容（改标签下的所有内容）

6. 取标签中的属性：
   1.  img/@src      取出src对应的地址

## 16.验证码和爬虫之间的爱恨情仇

​			反爬机制：验证码。识别验证码图片中的数据，用于模拟登录操作。

​	识别验证码的操作：

1.   人工肉眼识别。（不推荐）
2.   第三方自动（平台）识别。（推荐）					

### 16.1.使用平台识别验证码的编码流程：

1. 将验证码图片进行本地下载
2. 调用平台提供的示例代码进行图片数据识别

## 17.模拟登录

1. 爬取基于某些用户的用户信息

2. http/https协议特性：无状态

   1. 没有请求到对应用户的页面数据的原因：

      ​			—发起的第二次基于个人主页页面请求的时候，服务器端并不知道该请求是基于登录状态下的请求

   2. cookie:  用来让服务端记录客户端的相关状态

      1. 手动处理：通过抓包工具获取cookie值，将该值封装到headers中（不建议）

         ```python
         headers={    'cookie':'****'}
         ```

         

      2. 自动处理：

         1. cookie值得来源：模拟登录post请求后，由服务器端创建。
         2. session会话对象（作用）：
            1. 可以进行请求的发送
            2. 如果请求过程中产生了cookie，则cookie会被自动存储/携带在该session对象中
         3. 步骤：
            1. 创建一个session对象：session=requests.Session()
            2. 使用session对象进行模拟登录post请求的发送（cookie就会被存储在session中）
            3. session对象对个人主页对应的get请求进行发送（携带了cookie）



## 18.代理

1. 什么是代理 ：代理服务器。能破解封ip这种反爬机制
2. 作用：
   1. 突破自身ip访问的限制
   2. 掩藏自身真实ip
3. 相关网站：
   1. 快代理
   2. 西祠代理
4. 代理ip的类型：
   1. http:应用到http协议对应的url中
   2. https:应用到https协议对应的url中
5. 代理ip的匿名度
   1. 透明：服务器知道该次请求使用了代理，也知道请求对应的真实ip
   2. 匿名：知道使用了代理，不知道真实ip
   3. 高匿：不知道使用了代理，跟不知道真实的ip 

## 19.高性能异步爬虫

1. 目的：在爬虫中使用异步实现高性能的数据爬取操作
2. 方式：
   1. 多线程，多进程（不建议）：
      1. 优点：可以为相关阻塞的操作（get请求）单独开启线程或者进程，阻塞操作就可以异步执行。
      2. 弊端：无法无限制的开启多线程或者多进程。
   2. 线程池，进程池(适当使用 )：
      1. 优点：我们可以降低系统对进程或线程的创建和销毁的频率，从而很好的降低系统的开销。
      2. 弊端：池中线程或进程的数量是有上限的。
   3. 单线程+异步协程（推荐）：
      1. even_loop:事件循环，相当于一个无限循环，我们可以把一些函数注册到这个事件循环上，当满足某些条件的时候，函数就会被循环执行。
      2. coroutine：协程对象，我们可以将协程对象注册到事件循环中，他会被事件循环调用。我们可以用 async 关键字来定义一个方法，这个方法在调用时不会立即被执行，而是返回一个协程对象。
      3. task:任务，它是对协程对象的进一步封装，包含了任务的各个状态。
      4. future：代表将来执行或还没有执行的任务，实际上和 task 没有本质区别。
      5. async ：定义一个协程。
      6. await：用来挂起阻塞方法的执行。



## 20.selenium模块

1. 基于浏览器自动化的模块
2. 便捷的获取网站中动态加载的数据
3. 便捷实现模拟登录
4. 下载驱动地址：https://www.cnblogs.com/nullnullnull/p/11114370.html

### 20.1.selenium处理iframe

1. iframe:页面之间实现嵌套时所用到的标签

2. 如果定位得到标签存在于iframe中，则必须使用driver.switch_to.frame(id)来切换浏览器标签定位的作用域

3. 动作链（拖动）：from selenium.webdriver import ActionChains

   1. 实例化一个动作链对象：action=ActionChains(driver)

   2. 长按且点击操作指定的标签：click_and_hold(div)

   3. 移动像素：move_by_offest(x,y)

   4. 让动作链立即执行：perform()

   5. 释放动作链对象：action.release

      

## 22.scrapy框架

1. 什么是框架：就是集成了很多功能并且具有很强通用性的一个项目模版。
2. 如何学习框架：专门学习框架封装的各种功能的详细用法。
3. 什么是scrapy: 爬虫中封装好的一个明星框架。
   1. 功能：高性能的持久化存储，异步的数据下载，高性能的数据分析，分布式。

### 22.1.scrapy框架的基本使用

1. 环境安装：
   1. mac or linux:pip install scrapy
   2. windows:      pip install wheel
   3. 下载twisted。  [Python Extension Packages for Windows - Christoph Gohlke (uci.edu)](https://www.lfd.uci.edu/~gohlke/pythonlibs/#twisted) 
   4. 安装twisted。pip install 下载的资源
   5. pip install pywin32
   6. pip install scrapy

2. 创建一个工程：scrapy startproject 工程名
3. cd 工程名
4. 在spiders子目录中创建一个爬虫文件
   1. scrapy genspider spiderName www.xxx.com
5. 执行工程
   1. scrapy crawl spidername

### 22.2.scrapy数据解析

使用xpath返回的是一个Selector对象，需要用指令.extract()来返回Selector对象中data参数对应的字符串。

### 22.3.scrapy持久化存储

1. 基于终端指令：
   1. 要求：只可以将parse方法的返回值存储到本地的文本文件中
   2. 注意：持久化存储对应的文本文件的类型只可以为：'json'，'jsonlines'，'jl'，'csv'，'xml'，'marshal'，'pickle'
   3. 指令：scrapy crawl 文件名称 -o filePath
   4. 优点：简洁高效便捷
   5. 缺点：局限性比较强（数据只可以存储到指定后缀的文本文件中）
2. 基于管道：
   1. 编码流程：
      1. 数据解析
      2. 在item类中定义相关属性
      3. 将解析的数据封装存储到item类型的对象
      4. 将item类型的对象提交给管道进行持久化存储的操作
      5. 在管道类的process_item中将其接收到的item对象中存储的数据进行持久化存储操作
      6. 在配置文件中开启管道
   2. 优点：通用性强
   3. 面试题：将爬取到的数据一份存储到本地一份存储到数据库
      1. 管道文件中的一个管道类对应的是将数据存储到一种平台
      2. 爬虫文件提交的item只会给管道文件中第一个被执行的管道两位接收
      3. process_item中的return item表示将item传递给下一个即将被执行的管道类

### 22.4.基于Spider的全站数据爬取

  —就是将网站中某板块下的全部页码对应的页面数据进行爬取

1. 实现方式：
   1. 将所有页面的url都添加到start_url列表中（不推荐）
   2. 自行手动进行请求发送（推荐）
      1. yield scrapy.Request(url,callback)
      2. callback:回调函数，专门用作与数据解析

### 22.5五大核心组件

1. 引擎（Scrapy）

   用来处理系统的整个数据流处理，触发事务（核心框架）

2. 调度器（Scheduler）（之中含有过滤器和队列）

   用来接收引擎发过来的请求，压入队列中，并在引擎再次请求的时候返回（可以想成接收引擎发过来的请求，压入队列中，通过过滤器去重后，当引擎再次请求的时候返回）

3. 下载器（Downloader）

   用于下载网页内容，并将网页内容返回给蜘蛛（spider）（scrapy下载器是建立在twisted这个高效的异步模型上的）

4. 爬虫（Spiders）

   爬虫是主要干活的，用于在特定的网页中提取自己需要的信息，即所谓的实体（Item），用户也可以从中提取出链接，让Scrapy继续抓取下一个页面

5. 项目管道（Pipeline）

   负责处理爬虫从网页中抽取的实体，主要的功能是持久化实体，验证实体的有效性，清楚不需要的信息。当页面被爬虫解析后，将被发送到项目管道，并经过几个特定的次序处理数据。

### 22.6请求传参

1. 使用场景：如果爬取解析的数据不在同一个页面中。（深度爬取）

### 22.7.ImagesPipeline

1. 基于Scrapy爬取字符串类型数据和爬取图片类型数据的区别
   1. 字符串：只需要基于xpath进行解析且提交管道进行持久化存储
   2. xpath解析出图片src的属性值。单独的对图片地址发起请求获取图片二进制类型的数据
2. ImagesPipeline：只需要将img的src的属性进行解析，提交到管道，管道就会就会对图片的src进行请求发送获取图片并进行持久化存储。

### 22.8中间件

-下载中间件

1. 位置：引擎和下载器之间
2. 作用：批量拦截到整个工程中所有的请求和响应
3. 拦截请求：
   1. UA伪装
   2. 代理IP
4. 拦截响应：篡改响应对象，响应数据


