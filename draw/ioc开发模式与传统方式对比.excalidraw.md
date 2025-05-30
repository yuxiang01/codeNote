---

excalidraw-plugin: parsed
tags: [excalidraw]

---
==⚠  Switch to EXCALIDRAW VIEW in the MORE OPTIONS menu of this document. ⚠==


# Excalidraw Data

## Text Elements
[JdbcUtils / 反射] ^g4PmhFx8

传统的开发模式 ^jKgRc5RH

程序 ^kBoMtzzA

// 比如连接数据库JDBCUtils对象 ^xmUiUffJ

根据读取到的数据完成任务 ^X0UCeEm5

环境(理解成配置文件) ^46Bf6CMm

username = root
password = root
url = jdbc:mysql...
... ^JJfkJ2yV

读取信息 ^2gmk8Q6j

程序 ^UTrs7qBH

1.读取信息
2.可以通过相关技术,比如反射来创建
3.完成任务 ^7obkMoo2

配置文件 ^Kt3tMYj9

某个对象的配置 ^wr5ozc7O

读取信息 ^H2592Yw7

对象 ^hrd97Yr2

解读上图(以连接到数据库为例说明) 
1. 程序员编写程序, 在程序中读取配置信息
2. 通过反射方式创建对象
3. 使用对象完成任务 ^CvgF0Tji

IOC 的开发模式 ^SljmVHAI

程序 ^iWkaM2Gh

说明:
程序不再负责对象的创建,
而是直接使用ioc容器的对象 ^uUf0c6SS

环境/容器,会创建你配置的对象实例,
并完成属性的注入 ^fhvwdp50

解读上图
1、Spring 根据配置文件 xml/注解, 创建对象，
 并放入到容器(ConcurrentHashMap)中, 并且可以完成对象之间的依赖
2、当需要使用某个对象实例的时候, 就直接从容器中获取即可
3、程序员可以更加关注如何使用对象完成相应的业务 ^VQdfgJeh

程序------>环境 //程序读取环境配置， 然后自己创建对象. ^4FONgDUt

程序<-----容器 //容器创建好对象，程序直接使用. ^VLIIeZ6F

1. 获取容器
2. 获取容器的对象
3. 完成相关的业务逻辑 ^Z27D7J5Q

## Embedded Files
4071a1710b3741fccb8751d53dc1a202a2160c58: [[Pasted Image 20221229084835_431.png]]

%%
## Drawing
```compressed-json
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
```
%%