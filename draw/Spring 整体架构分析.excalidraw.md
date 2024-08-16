---

excalidraw-plugin: parsed
tags: [excalidraw]

---
==⚠  Switch to EXCALIDRAW VIEW in the MORE OPTIONS menu of this document. ⚠==


# Text Elements
Spring容器 init阶段 流程分析 ^dCXRVOlB

读取bean.xml文件 ^t5JG9Apc

扫描指定包 spring.component ^01Udfx7h

1.扫描包,得到bean的class对象,并排除包下不是bean的 ^B95KVTGs

2.扫描将bean信息封装到BeanDefinition对象,并放入到Map中 ^AqTdnrfX

3.初始化单例池也就是如果Bean是单例就实例化,并放到单例池Map ^belCbz63

容器创建/init完成后结果 ^pxPgULaE

BeanDefinition Map集合
Key[beanName]
value[BeanDefinition对象] ^M3XQt8fw

单例Bean Map集合Singleton
Key[beanName]
value[单例bean对象] ^DHzyGYhy

Spring容器 getBean(name) 实现机制 ^yU2sC6u2

执行 getName(name) ^AqDMYIFb

如果这个bean不存在,就抛出异常 ^7V1EzGsG

如果这个bean是singleton,那就从单例池获取即可 ^ksIJcYJt

如果这个bean是prototype,创建对象并返回 ^1rgpuFQt

到singleton单例池中
返回已经创建好了的Bean ^D3hnGEPJ

到Bean Definition Map中
得到Bean的Class对象,
使用反射,创建bean返回 ^Uo5FTwUl

到BeanDefinition Map
获取bean信息 ^NjkcEmS4

Spring的扩展功能 ^yKCnqqsI

--依赖注入
如果创建某个Bean对象,
存在依赖注入,需要进行
bean组装操作 
@AutoWired ^9EjgrWex

--Bean后置处理器机制
如果存在
BeanPostProcessor机制
扩展,就继续处理 ^KpA93lm7

-- AOP机制 ^rHmpfiGW

需要Bean
后置处理机制+
动态代理机制 ^GaTuTfZr

BeanDefinition
用来封装记录Bean信息:
1.scope以及属性值[单例与否]
2.Bean对应的Class对象,
反射可以生成对应对象 ^xiJsgjZT


# Embedded files
c6ad4938367c9f38ef4afe8c5c33aaa31b744ffb: [[Pasted Image 20230112130928_535.png]]

%%
# Drawing
```json
{
	"type": "excalidraw",
	"version": 2,
	"source": "https://github.com/zsviczian/obsidian-excalidraw-plugin/releases/tag/1.8.23",
	"elements": [
		{
			"type": "rectangle",
			"version": 418,
			"versionNonce": 1169161271,
			"isDeleted": false,
			"id": "vt_AP1jD6qaBZu9qxrdMk",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": -373.7869944852939,
			"y": -297.47196691176475,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 1544.3483455882351,
			"height": 348.131893382353,
			"seed": 1539256183,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1681720598061,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 209,
			"versionNonce": 713443001,
			"isDeleted": false,
			"id": "dCXRVOlB",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": -363.051700367647,
			"y": -286.6125919117647,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 300.625,
			"height": 24,
			"seed": 1973038807,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1681720598061,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 3,
			"text": "Spring容器 init阶段 流程分析",
			"rawText": "Spring容器 init阶段 流程分析",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "Spring容器 init阶段 流程分析",
			"lineHeight": 1.2,
			"baseline": 19
		},
		{
			"type": "rectangle",
			"version": 555,
			"versionNonce": 1559321943,
			"isDeleted": false,
			"id": "01TjVm9XZXocLGnLZBE9K",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": -354.33318895640605,
			"y": -167.9728808851334,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 433.65234375,
			"height": 144.39453125,
			"seed": 518952985,
			"groupIds": [
				"4bbPiGcG5r0YHvcvdH6Uf"
			],
			"roundness": null,
			"boundElements": [
				{
					"id": "geeYpkx5VKohlfXpdOt-S",
					"type": "arrow"
				}
			],
			"updated": 1681720598061,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			}
		},
		{
			"type": "text",
			"version": 465,
			"versionNonce": 727648153,
			"isDeleted": false,
			"id": "t5JG9Apc",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": -345.27850145640605,
			"y": -163.0158496351334,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 173.75,
			"height": 24,
			"seed": 10339129,
			"groupIds": [
				"4bbPiGcG5r0YHvcvdH6Uf"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1681720598061,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 3,
			"text": "读取bean.xml文件",
			"rawText": "读取bean.xml文件",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "读取bean.xml文件",
			"lineHeight": 1.2,
			"baseline": 19
		},
		{
			"type": "image",
			"version": 646,
			"versionNonce": 61446775,
			"isDeleted": false,
			"id": "1PlqUTEMrQ-IZhDXjPzxy",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": -355.79022020640605,
			"y": -136.1095996351334,
			"strokeColor": "transparent",
			"backgroundColor": "transparent",
			"width": 437.82958447802196,
			"height": 107.828125,
			"seed": 181566007,
			"groupIds": [
				"4bbPiGcG5r0YHvcvdH6Uf"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1681720598061,
			"link": null,
			"locked": false,
			"status": "pending",
			"fileId": "c6ad4938367c9f38ef4afe8c5c33aaa31b744ffb",
			"scale": [
				1,
				1
			]
		},
		{
			"type": "rectangle",
			"version": 770,
			"versionNonce": 422358137,
			"isDeleted": false,
			"id": "D8Qwpp1kSzpJT5qOkUAR9",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 139.8735966157808,
			"y": -209.88606645269869,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 620.16015625,
			"height": 183.05859375,
			"seed": 255856311,
			"groupIds": [
				"fO1KP3cLTU0YXKs3XmW9F"
			],
			"roundness": null,
			"boundElements": [
				{
					"id": "gBpRsbVVeZNQFCwypXq45",
					"type": "arrow"
				}
			],
			"updated": 1681720598061,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			}
		},
		{
			"type": "text",
			"version": 451,
			"versionNonce": 1587360663,
			"isDeleted": false,
			"id": "01Udfx7h",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 145.1196903657808,
			"y": -197.64847454093393,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 299.21875,
			"height": 24,
			"seed": 1072283929,
			"groupIds": [
				"fO1KP3cLTU0YXKs3XmW9F"
			],
			"roundness": null,
			"boundElements": [
				{
					"id": "geeYpkx5VKohlfXpdOt-S",
					"type": "arrow"
				}
			],
			"updated": 1681720598061,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			},
			"fontSize": 20,
			"fontFamily": 3,
			"text": "扫描指定包 spring.component",
			"rawText": "扫描指定包 spring.component",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "扫描指定包 spring.component",
			"lineHeight": 1.2,
			"baseline": 19
		},
		{
			"type": "rectangle",
			"version": 961,
			"versionNonce": 1270003033,
			"isDeleted": false,
			"id": "F2exzsLcLDiB8CkM9CATh",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 152.0923466157808,
			"y": -137.04105266593393,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 180,
			"height": 88,
			"seed": 68656825,
			"groupIds": [
				"fO1KP3cLTU0YXKs3XmW9F"
			],
			"roundness": null,
			"boundElements": [
				{
					"type": "text",
					"id": "B95KVTGs"
				},
				{
					"id": "geeYpkx5VKohlfXpdOt-S",
					"type": "arrow"
				}
			],
			"updated": 1681720598061,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			}
		},
		{
			"type": "text",
			"version": 791,
			"versionNonce": 100706487,
			"isDeleted": false,
			"id": "B95KVTGs",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 157.9673466157808,
			"y": -121.84105266593393,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 168.25,
			"height": 57.599999999999994,
			"seed": 9318041,
			"groupIds": [
				"fO1KP3cLTU0YXKs3XmW9F"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1681720598061,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "1.扫描包,得到bean的\nclass对象,并排除包下\n不是bean的",
			"rawText": "1.扫描包,得到bean的class对象,并排除包下不是bean的",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "F2exzsLcLDiB8CkM9CATh",
			"originalText": "1.扫描包,得到bean的class对象,并排除包下不是bean的",
			"lineHeight": 1.2,
			"baseline": 53
		},
		{
			"type": "rectangle",
			"version": 999,
			"versionNonce": 1447338553,
			"isDeleted": false,
			"id": "1pOytSFzO-TNhaobvF0r8",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 358.9790653657808,
			"y": -137.83206829093393,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 181,
			"height": 88,
			"seed": 1108322745,
			"groupIds": [
				"fO1KP3cLTU0YXKs3XmW9F"
			],
			"roundness": null,
			"boundElements": [
				{
					"type": "text",
					"id": "AqTdnrfX"
				}
			],
			"updated": 1681720598061,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			}
		},
		{
			"type": "text",
			"version": 918,
			"versionNonce": 393771479,
			"isDeleted": false,
			"id": "AqTdnrfX",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 365.3540653657808,
			"y": -122.63206829093393,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 168.25,
			"height": 57.599999999999994,
			"seed": 1734584919,
			"groupIds": [
				"fO1KP3cLTU0YXKs3XmW9F"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1681720598061,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "2.扫描将bean信息封装\n到BeanDefinition对\n象,并放入到Map中",
			"rawText": "2.扫描将bean信息封装到BeanDefinition对象,并放入到Map中",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "1pOytSFzO-TNhaobvF0r8",
			"originalText": "2.扫描将bean信息封装到BeanDefinition对象,并放入到Map中",
			"lineHeight": 1.2,
			"baseline": 53
		},
		{
			"type": "rectangle",
			"version": 999,
			"versionNonce": 365294361,
			"isDeleted": false,
			"id": "sWcblp7nOrDGVtSTIe6it",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 558.9360966157808,
			"y": -137.86722454093393,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 181,
			"height": 88,
			"seed": 513064825,
			"groupIds": [
				"fO1KP3cLTU0YXKs3XmW9F"
			],
			"roundness": null,
			"boundElements": [
				{
					"type": "text",
					"id": "belCbz63"
				}
			],
			"updated": 1681720598061,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			}
		},
		{
			"type": "text",
			"version": 1002,
			"versionNonce": 135866103,
			"isDeleted": false,
			"id": "belCbz63",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 566.6860966157808,
			"y": -122.66722454093393,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 165.5,
			"height": 57.599999999999994,
			"seed": 1370738839,
			"groupIds": [
				"fO1KP3cLTU0YXKs3XmW9F"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1681720598061,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "3.初始化单例池也就是\n如果Bean是单例就实例\n化,并放到单例池Map",
			"rawText": "3.初始化单例池也就是如果Bean是单例就实例化,并放到单例池Map",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "sWcblp7nOrDGVtSTIe6it",
			"originalText": "3.初始化单例池也就是如果Bean是单例就实例化,并放到单例池Map",
			"lineHeight": 1.2,
			"baseline": 53
		},
		{
			"type": "rectangle",
			"version": 217,
			"versionNonce": 2125080569,
			"isDeleted": false,
			"id": "YVMxJTTeQeWJmySnYRtqK",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 771.9688648897059,
			"y": -259.448486328125,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 369.9159007352942,
			"height": 286.7954963235294,
			"seed": 1993989593,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1681720598061,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 63,
			"versionNonce": 912732183,
			"isDeleted": false,
			"id": "pxPgULaE",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 779.9210707720588,
			"y": -252.44228228400738,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 190.875,
			"height": 19.2,
			"seed": 1978202199,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1681720598061,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "容器创建/init完成后结果",
			"rawText": "容器创建/init完成后结果",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "容器创建/init完成后结果",
			"lineHeight": 1.2,
			"baseline": 15
		},
		{
			"type": "ellipse",
			"version": 490,
			"versionNonce": 720683225,
			"isDeleted": false,
			"id": "-QTGQs1_AjJu-Xuk1i_WE",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 809.548828125,
			"y": -215.602783203125,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 293,
			"height": 103,
			"seed": 743523289,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"type": "text",
					"id": "M3XQt8fw"
				}
			],
			"updated": 1681720598061,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			}
		},
		{
			"type": "text",
			"version": 423,
			"versionNonce": 1012340023,
			"isDeleted": false,
			"id": "M3XQt8fw",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 841.611328125,
			"y": -192.902783203125,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 228.875,
			"height": 57.599999999999994,
			"seed": 90403735,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1681720598061,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "BeanDefinition Map集合\nKey[beanName]\nvalue[BeanDefinition对象]",
			"rawText": "BeanDefinition Map集合\nKey[beanName]\nvalue[BeanDefinition对象]",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "-QTGQs1_AjJu-Xuk1i_WE",
			"originalText": "BeanDefinition Map集合\nKey[beanName]\nvalue[BeanDefinition对象]",
			"lineHeight": 1.2,
			"baseline": 53
		},
		{
			"type": "ellipse",
			"version": 526,
			"versionNonce": 674839993,
			"isDeleted": false,
			"id": "l5Q9tfIgWZEBXMmK4toha",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 802.525390625,
			"y": -84.389892578125,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 293,
			"height": 103,
			"seed": 1948865623,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"type": "text",
					"id": "DHzyGYhy"
				},
				{
					"id": "e_3d37MUQLyj4xGlJnjpH",
					"type": "arrow"
				}
			],
			"updated": 1681720598061,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			}
		},
		{
			"type": "text",
			"version": 565,
			"versionNonce": 21845591,
			"isDeleted": false,
			"id": "DHzyGYhy",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 837.337890625,
			"y": -61.689892578125,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 223.375,
			"height": 57.599999999999994,
			"seed": 1023803545,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1681720598062,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "单例Bean Map集合Singleton\nKey[beanName]\nvalue[单例bean对象]",
			"rawText": "单例Bean Map集合Singleton\nKey[beanName]\nvalue[单例bean对象]",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "l5Q9tfIgWZEBXMmK4toha",
			"originalText": "单例Bean Map集合Singleton\nKey[beanName]\nvalue[单例bean对象]",
			"lineHeight": 1.2,
			"baseline": 53
		},
		{
			"type": "arrow",
			"version": 334,
			"versionNonce": 94051993,
			"isDeleted": false,
			"id": "geeYpkx5VKohlfXpdOt-S",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 80.31915479359395,
			"y": -96.15266899569363,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 54.162507786433196,
			"height": 49.49805142105319,
			"seed": 561448889,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1681720598062,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "01TjVm9XZXocLGnLZBE9K",
				"gap": 1,
				"focus": 0.7349350041551007
			},
			"endBinding": {
				"elementId": "F2exzsLcLDiB8CkM9CATh",
				"gap": 15.420912798713289,
				"focus": 1.1956742670639295
			},
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "arrow",
			"points": [
				[
					0,
					0
				],
				[
					54.162507786433196,
					-49.49805142105319
				]
			]
		},
		{
			"type": "rectangle",
			"version": 666,
			"versionNonce": 203322231,
			"isDeleted": false,
			"id": "slfc4sxmeHA0N5Iv4iSCn",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": -375.5562959558821,
			"y": 90.02903837316188,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 1139.818237687617,
			"height": 365.44532504775754,
			"seed": 1873869079,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1681720598062,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 109,
			"versionNonce": 567141241,
			"isDeleted": false,
			"id": "yU2sC6u2",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": -368.0675551470589,
			"y": 96.50766888786768,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 366.09375,
			"height": 24,
			"seed": 1728285591,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1681720598062,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 3,
			"text": "Spring容器 getBean(name) 实现机制",
			"rawText": "Spring容器 getBean(name) 实现机制",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "Spring容器 getBean(name) 实现机制",
			"lineHeight": 1.2,
			"baseline": 19
		},
		{
			"type": "rectangle",
			"version": 191,
			"versionNonce": 353299607,
			"isDeleted": false,
			"id": "9g5xXCJcFvr-AsADmJQPI",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": -317.71945867722286,
			"y": 222.659431743009,
			"strokeColor": "#000000",
			"backgroundColor": "#be4bdb",
			"width": 237,
			"height": 120,
			"seed": 1333521911,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"type": "text",
					"id": "AqDMYIFb"
				},
				{
					"id": "kT0sQrLZDo8z7T6BE2cD9",
					"type": "arrow"
				},
				{
					"id": "q-q_naVqtjoq82QXqpUY2",
					"type": "arrow"
				},
				{
					"id": "AGwV1sseyuyisrQ_M0_yi",
					"type": "arrow"
				},
				{
					"id": "gBpRsbVVeZNQFCwypXq45",
					"type": "arrow"
				}
			],
			"updated": 1681720598062,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			}
		},
		{
			"type": "text",
			"version": 176,
			"versionNonce": 957680729,
			"isDeleted": false,
			"id": "AqDMYIFb",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": -301.25070867722286,
			"y": 270.659431743009,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 204.0625,
			"height": 24,
			"seed": 1113389303,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1681720598062,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 3,
			"text": "执行 getName(name)",
			"rawText": "执行 getName(name)",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "9g5xXCJcFvr-AsADmJQPI",
			"originalText": "执行 getName(name)",
			"lineHeight": 1.2,
			"baseline": 19
		},
		{
			"type": "rectangle",
			"version": 258,
			"versionNonce": 1473462711,
			"isDeleted": false,
			"id": "iap90Lq6iRI-u8Kg_Ip1h",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 49.428357925345495,
			"y": 138.1777635637368,
			"strokeColor": "#000000",
			"backgroundColor": "#fcfdc1",
			"width": 332,
			"height": 74,
			"seed": 1146097111,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"type": "text",
					"id": "7V1EzGsG"
				},
				{
					"id": "kT0sQrLZDo8z7T6BE2cD9",
					"type": "arrow"
				}
			],
			"updated": 1681720598062,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			}
		},
		{
			"type": "text",
			"version": 233,
			"versionNonce": 860100921,
			"isDeleted": false,
			"id": "7V1EzGsG",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 66.1314829253455,
			"y": 163.1777635637368,
			"strokeColor": "#000000",
			"backgroundColor": "#fcfdc1",
			"width": 298.59375,
			"height": 24,
			"seed": 392296439,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1681720598062,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 3,
			"text": "如果这个bean不存在,就抛出异常",
			"rawText": "如果这个bean不存在,就抛出异常",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "iap90Lq6iRI-u8Kg_Ip1h",
			"originalText": "如果这个bean不存在,就抛出异常",
			"lineHeight": 1.2,
			"baseline": 19
		},
		{
			"type": "rectangle",
			"version": 317,
			"versionNonce": 630190807,
			"isDeleted": false,
			"id": "-kghsn9UTYYgLsyU9nkLk",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 41.20644750227473,
			"y": 233.6555612398206,
			"strokeColor": "#000000",
			"backgroundColor": "#fcfdc1",
			"width": 339,
			"height": 96,
			"seed": 405599801,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"type": "text",
					"id": "ksIJcYJt"
				},
				{
					"id": "q-q_naVqtjoq82QXqpUY2",
					"type": "arrow"
				},
				{
					"id": "e_3d37MUQLyj4xGlJnjpH",
					"type": "arrow"
				}
			],
			"updated": 1681720598062,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			}
		},
		{
			"type": "text",
			"version": 342,
			"versionNonce": 834570777,
			"isDeleted": false,
			"id": "ksIJcYJt",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 48.67519750227473,
			"y": 257.6555612398206,
			"strokeColor": "#000000",
			"backgroundColor": "#fcfdc1",
			"width": 324.0625,
			"height": 48,
			"seed": 99575255,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1681720598062,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 3,
			"text": "如果这个bean是singleton,那就从\n单例池获取即可",
			"rawText": "如果这个bean是singleton,那就从单例池获取即可",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "-kghsn9UTYYgLsyU9nkLk",
			"originalText": "如果这个bean是singleton,那就从单例池获取即可",
			"lineHeight": 1.2,
			"baseline": 43
		},
		{
			"type": "rectangle",
			"version": 331,
			"versionNonce": 920223735,
			"isDeleted": false,
			"id": "yDp804DT36msCx4o0149b",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 32.45174828701579,
			"y": 344.06741841880523,
			"strokeColor": "#000000",
			"backgroundColor": "#fcfdc1",
			"width": 341,
			"height": 92,
			"seed": 1879923193,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"type": "text",
					"id": "1rgpuFQt"
				},
				{
					"id": "AGwV1sseyuyisrQ_M0_yi",
					"type": "arrow"
				}
			],
			"updated": 1681720598062,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			}
		},
		{
			"type": "text",
			"version": 392,
			"versionNonce": 1568136953,
			"isDeleted": false,
			"id": "1rgpuFQt",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 40.92049828701579,
			"y": 366.06741841880523,
			"strokeColor": "#000000",
			"backgroundColor": "#fcfdc1",
			"width": 324.0625,
			"height": 48,
			"seed": 1703419415,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1681720598062,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 3,
			"text": "如果这个bean是prototype,创建对\n象并返回",
			"rawText": "如果这个bean是prototype,创建对象并返回",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "yDp804DT36msCx4o0149b",
			"originalText": "如果这个bean是prototype,创建对象并返回",
			"lineHeight": 1.2,
			"baseline": 43
		},
		{
			"type": "arrow",
			"version": 196,
			"versionNonce": 1035233559,
			"isDeleted": false,
			"id": "kT0sQrLZDo8z7T6BE2cD9",
			"fillStyle": "hachure",
			"strokeWidth": 2,
			"strokeStyle": "dashed",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": -79.60222226235193,
			"y": 276.5538074503081,
			"strokeColor": "#000000",
			"backgroundColor": "#fcfdc1",
			"width": 124.62624447478605,
			"height": 100.84118231621426,
			"seed": 1580502425,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1681720598062,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "9g5xXCJcFvr-AsADmJQPI",
				"gap": 1.1172364148709448,
				"focus": 0.5817302883521983
			},
			"endBinding": {
				"elementId": "iap90Lq6iRI-u8Kg_Ip1h",
				"gap": 4.404335712911347,
				"focus": 0.8017081812268282
			},
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "arrow",
			"points": [
				[
					0,
					0
				],
				[
					124.62624447478605,
					-100.84118231621426
				]
			]
		},
		{
			"type": "arrow",
			"version": 175,
			"versionNonce": 1962121177,
			"isDeleted": false,
			"id": "q-q_naVqtjoq82QXqpUY2",
			"fillStyle": "hachure",
			"strokeWidth": 2,
			"strokeStyle": "dashed",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": -79.47529094515698,
			"y": 280.7467301328199,
			"strokeColor": "#000000",
			"backgroundColor": "#fcfdc1",
			"width": 115.60904370124979,
			"height": 5.154850897395022,
			"seed": 1162897753,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1681720598062,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "9g5xXCJcFvr-AsADmJQPI",
				"gap": 1.2441677320658755,
				"focus": -0.11108226381867968
			},
			"endBinding": {
				"elementId": "-kghsn9UTYYgLsyU9nkLk",
				"gap": 5.072694746181924,
				"focus": -0.21653098917070754
			},
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "arrow",
			"points": [
				[
					0,
					0
				],
				[
					115.60904370124979,
					5.154850897395022
				]
			]
		},
		{
			"type": "arrow",
			"version": 138,
			"versionNonce": 932523575,
			"isDeleted": false,
			"id": "AGwV1sseyuyisrQ_M0_yi",
			"fillStyle": "hachure",
			"strokeWidth": 2,
			"strokeStyle": "dashed",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": -75.39317978416422,
			"y": 292.6903515604582,
			"strokeColor": "#000000",
			"backgroundColor": "#fcfdc1",
			"width": 104.56601910528057,
			"height": 105.581469642841,
			"seed": 1242986871,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1681720598062,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "9g5xXCJcFvr-AsADmJQPI",
				"gap": 5.326278893058657,
				"focus": -0.6401188883616112
			},
			"endBinding": {
				"elementId": "yDp804DT36msCx4o0149b",
				"gap": 3.2789089658994044,
				"focus": -0.8419255611287169
			},
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "arrow",
			"points": [
				[
					0,
					0
				],
				[
					104.56601910528057,
					105.581469642841
				]
			]
		},
		{
			"type": "arrow",
			"version": 200,
			"versionNonce": 440743097,
			"isDeleted": false,
			"id": "e_3d37MUQLyj4xGlJnjpH",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 382.2653002416132,
			"y": 280.096781789905,
			"strokeColor": "#000000",
			"backgroundColor": "#fcfdc1",
			"width": 460.9070920836855,
			"height": 275.7677876550847,
			"seed": 950881783,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"type": "text",
					"id": "D3hnGEPJ"
				}
			],
			"updated": 1681720598062,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			},
			"startBinding": {
				"elementId": "-kghsn9UTYYgLsyU9nkLk",
				"gap": 2.0588527393384766,
				"focus": 0.7356124962695179
			},
			"endBinding": {
				"elementId": "l5Q9tfIgWZEBXMmK4toha",
				"gap": 1.6845290194434597,
				"focus": 0.14817476703957927
			},
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "arrow",
			"points": [
				[
					0,
					0
				],
				[
					184.76630256179988,
					-145.33024981571714
				],
				[
					460.9070920836855,
					-275.7677876550847
				]
			]
		},
		{
			"type": "text",
			"version": 95,
			"versionNonce": 2137018199,
			"isDeleted": false,
			"id": "D3hnGEPJ",
			"fillStyle": "hachure",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 453.5941028034131,
			"y": 110.76653197418784,
			"strokeColor": "#c92a2a",
			"backgroundColor": "#fcfdc1",
			"width": 226.875,
			"height": 48,
			"seed": 1374812377,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1681720598062,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 3,
			"text": "到singleton单例池中\n返回已经创建好了的Bean",
			"rawText": "到singleton单例池中\n返回已经创建好了的Bean",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "e_3d37MUQLyj4xGlJnjpH",
			"originalText": "到singleton单例池中\n返回已经创建好了的Bean",
			"lineHeight": 1.2,
			"baseline": 43
		},
		{
			"type": "arrow",
			"version": 864,
			"versionNonce": 1628452249,
			"isDeleted": false,
			"id": "-JsJ4VLh3WYBufv9h0Ies",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 373.2887174895795,
			"y": 398.73464451862174,
			"strokeColor": "#000000",
			"backgroundColor": "#fcfdc1",
			"width": 501.2060763290546,
			"height": 521.2713789512477,
			"seed": 1063663161,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"type": "text",
					"id": "Uo5FTwUl"
				}
			],
			"updated": 1681720598062,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			},
			"startBinding": null,
			"endBinding": null,
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "arrow",
			"points": [
				[
					0,
					0
				],
				[
					244.9317469122529,
					-149.5707869299568
				],
				[
					501.2060763290546,
					-521.2713789512477
				]
			]
		},
		{
			"type": "text",
			"version": 257,
			"versionNonce": 527060087,
			"isDeleted": false,
			"id": "Uo5FTwUl",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 486.8923394018324,
			"y": 213.16385758866494,
			"strokeColor": "#c92a2a",
			"backgroundColor": "#fcfdc1",
			"width": 262.65625,
			"height": 72,
			"seed": 124707417,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1681720598062,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 3,
			"text": "到Bean Definition Map中\n得到Bean的Class对象,\n使用反射,创建bean返回",
			"rawText": "到Bean Definition Map中\n得到Bean的Class对象,\n使用反射,创建bean返回",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "-JsJ4VLh3WYBufv9h0Ies",
			"originalText": "到Bean Definition Map中\n得到Bean的Class对象,\n使用反射,创建bean返回",
			"lineHeight": 1.2,
			"baseline": 67
		},
		{
			"type": "arrow",
			"version": 914,
			"versionNonce": 478681721,
			"isDeleted": false,
			"id": "gBpRsbVVeZNQFCwypXq45",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": -79.45403762448655,
			"y": 223.19453745006382,
			"strokeColor": "#000000",
			"backgroundColor": "#fcfdc1",
			"width": 935.9310968262303,
			"height": 349.70875859336513,
			"seed": 1820566071,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"type": "text",
					"id": "NjkcEmS4"
				}
			],
			"updated": 1681720598062,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			},
			"startBinding": {
				"elementId": "9g5xXCJcFvr-AsADmJQPI",
				"gap": 1.2654210527363148,
				"focus": -0.14125030314440357
			},
			"endBinding": null,
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "arrow",
			"points": [
				[
					0,
					0
				],
				[
					444.40174641378496,
					-166.00289649730416
				],
				[
					935.9310968262303,
					-349.70875859336513
				]
			]
		},
		{
			"type": "text",
			"version": 81,
			"versionNonce": 1668785559,
			"isDeleted": false,
			"id": "NjkcEmS4",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 249.47895878929842,
			"y": 33.191640952759656,
			"strokeColor": "#c92a2a",
			"backgroundColor": "#fcfdc1",
			"width": 230.9375,
			"height": 48,
			"seed": 363730617,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1681720598062,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 3,
			"text": "到BeanDefinition Map\n获取bean信息",
			"rawText": "到BeanDefinition Map\n获取bean信息",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "gBpRsbVVeZNQFCwypXq45",
			"originalText": "到BeanDefinition Map\n获取bean信息",
			"lineHeight": 1.2,
			"baseline": 43
		},
		{
			"type": "rectangle",
			"version": 430,
			"versionNonce": 1836082009,
			"isDeleted": false,
			"id": "eP3Mz5pS5zshVdnGjh87e",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 782.4898978629753,
			"y": 87.93903043459079,
			"strokeColor": "#e67700",
			"backgroundColor": "#fff",
			"width": 537.8689179876726,
			"height": 359.1445461243569,
			"seed": 1345425241,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1681720598062,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 284,
			"versionNonce": 717019831,
			"isDeleted": false,
			"id": "yKCnqqsI",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 793.416145647125,
			"y": 101.01757313724602,
			"strokeColor": "#000000",
			"backgroundColor": "#ffe2af",
			"width": 170.3125,
			"height": 24,
			"seed": 1906755735,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1681720598062,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 3,
			"text": "Spring的扩展功能",
			"rawText": "Spring的扩展功能",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "Spring的扩展功能",
			"lineHeight": 1.2,
			"baseline": 19
		},
		{
			"type": "rectangle",
			"version": 867,
			"versionNonce": 270307385,
			"isDeleted": false,
			"id": "dZ4oOd4Q7lJ3TxI2T-0gi",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 795.6673524236332,
			"y": 137.7316475438646,
			"strokeColor": "#000000",
			"backgroundColor": "#ffe2af",
			"width": 245,
			"height": 150,
			"seed": 2144123959,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1681720598062,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 234,
			"versionNonce": 2044619735,
			"isDeleted": false,
			"id": "9EjgrWex",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 804.6571830979183,
			"y": 144.41285185988886,
			"strokeColor": "#000000",
			"backgroundColor": "#ffe2af",
			"width": 218.59375,
			"height": 120,
			"seed": 301501657,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1681720598062,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 3,
			"text": "--依赖注入\n如果创建某个Bean对象,\n存在依赖注入,需要进行\nbean组装操作 \n@AutoWired",
			"rawText": "--依赖注入\n如果创建某个Bean对象,\n存在依赖注入,需要进行\nbean组装操作 \n@AutoWired",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "--依赖注入\n如果创建某个Bean对象,\n存在依赖注入,需要进行\nbean组装操作 \n@AutoWired",
			"lineHeight": 1.2,
			"baseline": 115
		},
		{
			"type": "rectangle",
			"version": 987,
			"versionNonce": 1785733401,
			"isDeleted": false,
			"id": "9FjyBI6jNXjm-GW1sNzuV",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 1057.5438605705538,
			"y": 134.02642978597873,
			"strokeColor": "#000000",
			"backgroundColor": "#ffe2af",
			"width": 253.46378023056585,
			"height": 131.36648263576683,
			"seed": 1940936025,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1681720598062,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 123,
			"versionNonce": 1290852599,
			"isDeleted": false,
			"id": "KpA93lm7",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 1065.7244390520054,
			"y": 142.5038048492753,
			"strokeColor": "#000000",
			"backgroundColor": "#ffe2af",
			"width": 239.21875,
			"height": 96,
			"seed": 425716985,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"id": "R5nVSRXFyILBDT9OVhI65",
					"type": "arrow"
				}
			],
			"updated": 1681720598062,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			},
			"fontSize": 20,
			"fontFamily": 3,
			"text": "--Bean后置处理器机制\n如果存在\nBeanPostProcessor机制\n扩展,就继续处理",
			"rawText": "--Bean后置处理器机制\n如果存在\nBeanPostProcessor机制\n扩展,就继续处理",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "--Bean后置处理器机制\n如果存在\nBeanPostProcessor机制\n扩展,就继续处理",
			"lineHeight": 1.2,
			"baseline": 91
		},
		{
			"type": "rectangle",
			"version": 1037,
			"versionNonce": 1507759609,
			"isDeleted": false,
			"id": "qVMfHzKWNBJAxgvfJzDIa",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 807.1275001303701,
			"y": 311.59896695191134,
			"strokeColor": "#000000",
			"backgroundColor": "#ffe2af",
			"width": 211,
			"height": 110,
			"seed": 291781751,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"type": "text",
					"id": "rHmpfiGW"
				},
				{
					"id": "R5nVSRXFyILBDT9OVhI65",
					"type": "arrow"
				}
			],
			"updated": 1681720598062,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			}
		},
		{
			"type": "text",
			"version": 22,
			"versionNonce": 695771671,
			"isDeleted": false,
			"id": "rHmpfiGW",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 857.4712501303701,
			"y": 354.59896695191134,
			"strokeColor": "#000000",
			"backgroundColor": "#ffe2af",
			"width": 110.3125,
			"height": 24,
			"seed": 116122041,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1681720598062,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 3,
			"text": "-- AOP机制",
			"rawText": "-- AOP机制",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "qVMfHzKWNBJAxgvfJzDIa",
			"originalText": "-- AOP机制",
			"lineHeight": 1.2,
			"baseline": 19
		},
		{
			"type": "arrow",
			"version": 383,
			"versionNonce": 1097559769,
			"isDeleted": false,
			"id": "R5nVSRXFyILBDT9OVhI65",
			"fillStyle": "hachure",
			"strokeWidth": 2,
			"strokeStyle": "dashed",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 1022.5982547218158,
			"y": 382.5366449043982,
			"strokeColor": "#000000",
			"backgroundColor": "#ffe2af",
			"width": 154.31648938265278,
			"height": 115.8580108484287,
			"seed": 2012629559,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"type": "text",
					"id": "GaTuTfZr"
				}
			],
			"updated": 1681720598062,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			},
			"startBinding": {
				"elementId": "qVMfHzKWNBJAxgvfJzDIa",
				"gap": 4.470754591445598,
				"focus": 0.4417962044337158
			},
			"endBinding": {
				"elementId": "KpA93lm7",
				"focus": -0.028616139059378844,
				"gap": 12.174829206694199
			},
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "arrow",
			"points": [
				[
					0,
					0
				],
				[
					137.09089982333978,
					-18.09043309211478
				],
				[
					154.31648938265278,
					-115.8580108484287
				]
			]
		},
		{
			"type": "text",
			"version": 203,
			"versionNonce": 1886013239,
			"isDeleted": false,
			"id": "GaTuTfZr",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 1093.8297795451556,
			"y": 328.4462118122834,
			"strokeColor": "#c92a2a",
			"backgroundColor": "#ffe2af",
			"width": 131.71875,
			"height": 72,
			"seed": 704022329,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1681720598063,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 3,
			"text": "需要Bean\n后置处理机制+\n动态代理机制",
			"rawText": "需要Bean\n后置处理机制+\n动态代理机制",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "R5nVSRXFyILBDT9OVhI65",
			"originalText": "需要Bean\n后置处理机制+\n动态代理机制",
			"lineHeight": 1.2,
			"baseline": 67
		},
		{
			"type": "arrow",
			"version": 516,
			"versionNonce": 1470770105,
			"isDeleted": false,
			"id": "9GBkrP5ylTCPeYZwVzI4A",
			"fillStyle": "hachure",
			"strokeWidth": 2,
			"strokeStyle": "dashed",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 1069.3771046550094,
			"y": -148.64107190255805,
			"strokeColor": "#0b7285",
			"backgroundColor": "#fff",
			"width": 172.6787127207274,
			"height": 68.37058572134782,
			"seed": 567214903,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1681720598063,
			"link": null,
			"locked": false,
			"startBinding": null,
			"endBinding": {
				"elementId": "P-Ptdn9fn9a1Gn3a6kVj9",
				"gap": 3.9667056487160903,
				"focus": 0.3211970459789611
			},
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "arrow",
			"points": [
				[
					0,
					0
				],
				[
					172.6787127207274,
					-68.37058572134782
				]
			]
		},
		{
			"type": "rectangle",
			"version": 383,
			"versionNonce": 1881819223,
			"isDeleted": false,
			"id": "P-Ptdn9fn9a1Gn3a6kVj9",
			"fillStyle": "hachure",
			"strokeWidth": 2,
			"strokeStyle": "dashed",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 1246.022523024453,
			"y": -309.262930813903,
			"strokeColor": "#0b7285",
			"backgroundColor": "#fff",
			"width": 301,
			"height": 148,
			"seed": 1049704343,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"id": "9GBkrP5ylTCPeYZwVzI4A",
					"type": "arrow"
				},
				{
					"type": "text",
					"id": "xiJsgjZT"
				}
			],
			"updated": 1681720598063,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			}
		},
		{
			"type": "text",
			"version": 528,
			"versionNonce": 884886681,
			"isDeleted": false,
			"id": "xiJsgjZT",
			"fillStyle": "hachure",
			"strokeWidth": 2,
			"strokeStyle": "dashed",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 1253.788148024453,
			"y": -295.262930813903,
			"strokeColor": "#0b7285",
			"backgroundColor": "#fff",
			"width": 285.46875,
			"height": 120,
			"seed": 1113958775,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1681720598063,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 3,
			"text": "BeanDefinition\n用来封装记录Bean信息:\n1.scope以及属性值[单例与否]\n2.Bean对应的Class对象,\n反射可以生成对应对象",
			"rawText": "BeanDefinition\n用来封装记录Bean信息:\n1.scope以及属性值[单例与否]\n2.Bean对应的Class对象,\n反射可以生成对应对象",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "P-Ptdn9fn9a1Gn3a6kVj9",
			"originalText": "BeanDefinition\n用来封装记录Bean信息:\n1.scope以及属性值[单例与否]\n2.Bean对应的Class对象,\n反射可以生成对应对象",
			"lineHeight": 1.2,
			"baseline": 115
		}
	],
	"appState": {
		"theme": "light",
		"viewBackgroundColor": "#ffffff",
		"currentItemStrokeColor": "#0b7285",
		"currentItemBackgroundColor": "#fff",
		"currentItemFillStyle": "hachure",
		"currentItemStrokeWidth": 2,
		"currentItemStrokeStyle": "dashed",
		"currentItemRoughness": 2,
		"currentItemOpacity": 100,
		"currentItemFontFamily": 3,
		"currentItemFontSize": 20,
		"currentItemTextAlign": "left",
		"currentItemStartArrowhead": null,
		"currentItemEndArrowhead": "arrow",
		"scrollX": 456.53961373844186,
		"scrollY": 673.9255336964917,
		"zoom": {
			"value": 0.55
		},
		"currentItemRoundness": "sharp",
		"gridSize": null,
		"colorPalette": {},
		"currentStrokeOptions": null,
		"previousGridSize": null
	},
	"files": {}
}
```
%%