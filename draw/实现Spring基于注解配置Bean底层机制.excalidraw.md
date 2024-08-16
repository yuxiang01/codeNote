---

excalidraw-plugin: parsed
tags: [excalidraw]

---
==⚠  Switch to EXCALIDRAW VIEW in the MORE OPTIONS menu of this document. ⚠==


# Text Elements
beans.xml ^aUtKPDYo

<context:component-scan base-package="spring.component"/> ^cNfORTNH

使用component-scan,指定了要扫描的包 ^5pNSw8qd

ClassPathXmlApplicationContext Spring容器 ^vjsbsZ1E

1.根据/加载beans.xml

2.根据配置扫描指定包下的类(标识注解)

3.实例化对象,并且放入(单例池)容器singletonObjects

4.提供getBean() ^IlxaWZfq

fishx_SpringConfig 配置类 ^ez0fYL8f

1. 作用类似beans.xml配置文件
2. 该类会标识一个注解@ComponentScan ^l3zieTNo

@ComponentScan(value=“com.xx”)
public class fishx_SpringConfig{} ^z5MK9Z61

自定义一个注解ComponentScan ^rcdEi1ro

1. 这个注解只有一个属性value
2. 这个value指定要扫描的包
3. 作用类似 component-scan ^j7V3uK6Q

Spring容器IOC注解方式 ^bl9LuJ78

简单手动实现Spring容器IOC注解方式 ^hGz3ctT3

fishx_ApplicationContext Spring容器 ^yRSMn9gV

1.传入fishx_SpringConfig.class
2.根据传入的class能够解析到@ComponentScan的value=> 扫描的包
3.通过(类的加载器、IO、注解解析)得到包下的加载资源(.class)
4.再查看该类class是不是要被实例化,需要=>反射new
5.将你反射生成的对象,放入容器(singletonObjects)
6.提供 Object getBean(String key),Object指容器中对应的对象 ^zE5xtjCa

%%
# Drawing
```json
{
	"type": "excalidraw",
	"version": 2,
	"source": "https://excalidraw.com",
	"elements": [
		{
			"type": "rectangle",
			"version": 836,
			"versionNonce": 302376653,
			"isDeleted": false,
			"id": "Nzgok7x7JhtMWGl0q_fQz",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 211.7734375,
			"y": -223.703125,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 544.515625,
			"height": 113.73828124999999,
			"seed": 710568589,
			"groupIds": [
				"pGJG8EihBW4uY58Tyn0N0"
			],
			"roundness": null,
			"boundElements": [
				{
					"id": "0RQ_7Rgh9F8aTB6qEjKJk",
					"type": "arrow"
				},
				{
					"id": "YndeZn_cSx102K0bEoceC",
					"type": "arrow"
				}
			],
			"updated": 1672889456254,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 553,
			"versionNonce": 1724545155,
			"isDeleted": false,
			"id": "aUtKPDYo",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 216.890625,
			"y": -219.84375,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 90,
			"height": 25,
			"seed": 383901123,
			"groupIds": [
				"pGJG8EihBW4uY58Tyn0N0"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1672889456254,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 1,
			"text": "beans.xml",
			"rawText": "beans.xml",
			"baseline": 18,
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "beans.xml"
		},
		{
			"type": "text",
			"version": 647,
			"versionNonce": 463045933,
			"isDeleted": false,
			"id": "cNfORTNH",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 216.68359375,
			"y": -183.90625,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 535,
			"height": 19,
			"seed": 112236515,
			"groupIds": [
				"pGJG8EihBW4uY58Tyn0N0"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1672889456255,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "<context:component-scan base-package=\"spring.component\"/>",
			"rawText": "<context:component-scan base-package=\"spring.component\"/>",
			"baseline": 15,
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "<context:component-scan base-package=\"spring.component\"/>"
		},
		{
			"type": "text",
			"version": 598,
			"versionNonce": 680423459,
			"isDeleted": false,
			"id": "5pNSw8qd",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 218.10546875,
			"y": -152.328125,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 302,
			"height": 22,
			"seed": 1157174723,
			"groupIds": [
				"pGJG8EihBW4uY58Tyn0N0"
			],
			"roundness": null,
			"boundElements": [
				{
					"id": "fgwZx563WEhDAGGo0-ATu",
					"type": "arrow"
				}
			],
			"updated": 1672889456255,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "使用component-scan,指定了要扫描的包",
			"rawText": "使用component-scan,指定了要扫描的包",
			"baseline": 17,
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "使用component-scan,指定了要扫描的包"
		},
		{
			"type": "rectangle",
			"version": 675,
			"versionNonce": 1105876877,
			"isDeleted": false,
			"id": "dfSJtwJjBQwbYrAqC4uGE",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": -331.02734375,
			"y": -266.0703125,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 450.9921875,
			"height": 196.046875,
			"seed": 1428457411,
			"groupIds": [
				"tBeBgAXNEZfJRdu1cslbG"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1672889456255,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 397,
			"versionNonce": 1781691331,
			"isDeleted": false,
			"id": "vjsbsZ1E",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": -316.4375,
			"y": -257.59765625,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 380,
			"height": 22,
			"seed": 1889521741,
			"groupIds": [
				"tBeBgAXNEZfJRdu1cslbG"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1672889456255,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "ClassPathXmlApplicationContext Spring容器",
			"rawText": "ClassPathXmlApplicationContext Spring容器",
			"baseline": 17,
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "ClassPathXmlApplicationContext Spring容器"
		},
		{
			"type": "text",
			"version": 587,
			"versionNonce": 1597103597,
			"isDeleted": false,
			"id": "IlxaWZfq",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": -316.546875,
			"y": -223.9609375,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 421,
			"height": 145,
			"seed": 21798445,
			"groupIds": [
				"tBeBgAXNEZfJRdu1cslbG"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1672889456255,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "1.根据/加载beans.xml\n\n2.根据配置扫描指定包下的类(标识注解)\n\n3.实例化对象,并且放入(单例池)容器singletonObjects\n\n4.提供getBean()",
			"rawText": "1.根据/加载beans.xml\n\n2.根据配置扫描指定包下的类(标识注解)\n\n3.实例化对象,并且放入(单例池)容器singletonObjects\n\n4.提供getBean()",
			"baseline": 140,
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "1.根据/加载beans.xml\n\n2.根据配置扫描指定包下的类(标识注解)\n\n3.实例化对象,并且放入(单例池)容器singletonObjects\n\n4.提供getBean()"
		},
		{
			"type": "arrow",
			"version": 512,
			"versionNonce": 1272251235,
			"isDeleted": false,
			"id": "fgwZx563WEhDAGGo0-ATu",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 103.08203125000003,
			"y": -162.4802433076208,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 110.27576678934088,
			"height": 3.4659762480455356,
			"seed": 126697283,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1672889456255,
			"link": null,
			"locked": false,
			"startBinding": null,
			"endBinding": {
				"elementId": "5pNSw8qd",
				"focus": 0.8123350164112562,
				"gap": 6.686142059575275
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
					110.27576678934088,
					3.4659762480455356
				]
			]
		},
		{
			"type": "rectangle",
			"version": 705,
			"versionNonce": 1249482829,
			"isDeleted": false,
			"id": "y6jxzlgZ_9iyr8S_zUWxE",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 72.59251644736844,
			"y": 16.52147152549344,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 333.6796875,
			"height": 135.2734375,
			"seed": 49942093,
			"groupIds": [
				"ESE_7G5IEo6OLNWI6Dh_Y"
			],
			"roundness": null,
			"boundElements": [
				{
					"id": "Rs28HqscJ1J80v6QD04Z3",
					"type": "arrow"
				}
			],
			"updated": 1672889456255,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 639,
			"versionNonce": 1707384579,
			"isDeleted": false,
			"id": "ez0fYL8f",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 77.71361019736844,
			"y": 21.75194027549344,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 227,
			"height": 22,
			"seed": 1827169859,
			"groupIds": [
				"ESE_7G5IEo6OLNWI6Dh_Y"
			],
			"roundness": null,
			"boundElements": [
				{
					"id": "0RQ_7Rgh9F8aTB6qEjKJk",
					"type": "arrow"
				}
			],
			"updated": 1672889456255,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "fishx_SpringConfig 配置类",
			"rawText": "fishx_SpringConfig 配置类",
			"baseline": 17,
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "fishx_SpringConfig 配置类"
		},
		{
			"type": "text",
			"version": 608,
			"versionNonce": 1033569965,
			"isDeleted": false,
			"id": "l3zieTNo",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 81.91673519736844,
			"y": 50.06444027549344,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 304,
			"height": 44,
			"seed": 1848665731,
			"groupIds": [
				"ESE_7G5IEo6OLNWI6Dh_Y"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1672889456255,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "1. 作用类似beans.xml配置文件\n2. 该类会标识一个注解@ComponentScan",
			"rawText": "1. 作用类似beans.xml配置文件\n2. 该类会标识一个注解@ComponentScan",
			"baseline": 39,
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "1. 作用类似beans.xml配置文件\n2. 该类会标识一个注解@ComponentScan"
		},
		{
			"type": "text",
			"version": 628,
			"versionNonce": 1115670179,
			"isDeleted": false,
			"id": "z5MK9Z61",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 81.20189144736844,
			"y": 102.02928402549344,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 310,
			"height": 38,
			"seed": 1288925987,
			"groupIds": [
				"ESE_7G5IEo6OLNWI6Dh_Y"
			],
			"roundness": null,
			"boundElements": [
				{
					"id": "mnrSbWUTYqyDBd40Wgsth",
					"type": "arrow"
				}
			],
			"updated": 1672889456255,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "@ComponentScan(value=“com.xx”)\npublic class fishx_SpringConfig{}",
			"rawText": "@ComponentScan(value=“com.xx”)\npublic class fishx_SpringConfig{}",
			"baseline": 34,
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "@ComponentScan(value=“com.xx”)\npublic class fishx_SpringConfig{}"
		},
		{
			"type": "rectangle",
			"version": 726,
			"versionNonce": 2067769613,
			"isDeleted": false,
			"id": "Rhwu5fZO7O1bY5biJpQ9Q",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 482.33203125,
			"y": 26.346923828125,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 286.64453125,
			"height": 116.2109375,
			"seed": 1773940579,
			"groupIds": [
				"UaveGVeqa90pDyaPONWPX"
			],
			"roundness": null,
			"boundElements": [
				{
					"id": "Rs28HqscJ1J80v6QD04Z3",
					"type": "arrow"
				},
				{
					"id": "YndeZn_cSx102K0bEoceC",
					"type": "arrow"
				}
			],
			"updated": 1672889456255,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 646,
			"versionNonce": 414513731,
			"isDeleted": false,
			"id": "rcdEi1ro",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 487.453125,
			"y": 31.577392578125,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 235,
			"height": 22,
			"seed": 1122708557,
			"groupIds": [
				"UaveGVeqa90pDyaPONWPX"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1672889456255,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "自定义一个注解ComponentScan",
			"rawText": "自定义一个注解ComponentScan",
			"baseline": 17,
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "自定义一个注解ComponentScan"
		},
		{
			"type": "text",
			"version": 720,
			"versionNonce": 887160685,
			"isDeleted": false,
			"id": "j7V3uK6Q",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 491.42578125,
			"y": 59.659423828125,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 235,
			"height": 66,
			"seed": 367414019,
			"groupIds": [
				"UaveGVeqa90pDyaPONWPX"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1672889456255,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "1. 这个注解只有一个属性value\n2. 这个value指定要扫描的包\n3. 作用类似 component-scan",
			"rawText": "1. 这个注解只有一个属性value\n2. 这个value指定要扫描的包\n3. 作用类似 component-scan",
			"baseline": 61,
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "1. 这个注解只有一个属性value\n2. 这个value指定要扫描的包\n3. 作用类似 component-scan"
		},
		{
			"type": "arrow",
			"version": 961,
			"versionNonce": 1162000867,
			"isDeleted": false,
			"id": "Rs28HqscJ1J80v6QD04Z3",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 410.66673519736844,
			"y": 86.67324504937886,
			"strokeColor": "#d9480f",
			"backgroundColor": "transparent",
			"width": 63.77857730263156,
			"height": 0.6446772818966053,
			"seed": 1313960237,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1672889456255,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "y6jxzlgZ_9iyr8S_zUWxE",
				"focus": 0.015062794261879474,
				"gap": 4.39453125
			},
			"endBinding": {
				"elementId": "Rhwu5fZO7O1bY5biJpQ9Q",
				"focus": -0.07378085696533451,
				"gap": 7.88671875
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
					63.77857730263156,
					0.6446772818966053
				]
			]
		},
		{
			"type": "text",
			"version": 116,
			"versionNonce": 818448845,
			"isDeleted": false,
			"id": "bl9LuJ78",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": -534.3411458333333,
			"y": -184.29777018229169,
			"strokeColor": "#364fc7",
			"backgroundColor": "transparent",
			"width": 181,
			"height": 22,
			"seed": 489182531,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1672889456255,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "Spring容器IOC注解方式",
			"rawText": "Spring容器IOC注解方式",
			"baseline": 17,
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "Spring容器IOC注解方式"
		},
		{
			"type": "text",
			"version": 303,
			"versionNonce": 87524739,
			"isDeleted": false,
			"id": "hGz3ctT3",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": -554.2790178571429,
			"y": -35.159563337053555,
			"strokeColor": "#d9480f",
			"backgroundColor": "transparent",
			"width": 277,
			"height": 22,
			"seed": 1657783011,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1672889456255,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "简单手动实现Spring容器IOC注解方式",
			"rawText": "简单手动实现Spring容器IOC注解方式",
			"baseline": 17,
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "简单手动实现Spring容器IOC注解方式"
		},
		{
			"type": "rectangle",
			"version": 335,
			"versionNonce": 806340653,
			"isDeleted": false,
			"id": "brl5XZay3FcYoI40A2tOv",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": -564.4648437500001,
			"y": -317.607666015625,
			"strokeColor": "#364fc7",
			"backgroundColor": "transparent",
			"width": 1342.8229166666667,
			"height": 265.97005208333337,
			"seed": 1057645901,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1672889456255,
			"link": null,
			"locked": false
		},
		{
			"type": "rectangle",
			"version": 1372,
			"versionNonce": 1025990157,
			"isDeleted": false,
			"id": "ykMTxzZ7wmwz4a0o1AGGw",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": -535.409333881579,
			"y": -5.248132538377348,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 536.650082236842,
			"height": 183.83059210526326,
			"seed": 117145763,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1672889657056,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 845,
			"versionNonce": 1748672387,
			"isDeleted": false,
			"id": "yRSMn9gV",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": -526.8682154605264,
			"y": 0.502484237938404,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 324,
			"height": 22,
			"seed": 418680589,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1672889641510,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "fishx_ApplicationContext Spring容器",
			"rawText": "fishx_ApplicationContext Spring容器",
			"baseline": 17,
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "fishx_ApplicationContext Spring容器"
		},
		{
			"type": "text",
			"version": 1591,
			"versionNonce": 1641348163,
			"isDeleted": false,
			"id": "zE5xtjCa",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": -519.7816611842104,
			"y": 25.763380619517307,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 515,
			"height": 132,
			"seed": 162100291,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"id": "mnrSbWUTYqyDBd40Wgsth",
					"type": "arrow"
				}
			],
			"updated": 1672889661498,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "1.传入fishx_SpringConfig.class\n2.根据传入的class能够解析到@ComponentScan的value=> 扫描的包\n3.通过(类的加载器、IO、注解解析)得到包下的加载资源(.class)\n4.再查看该类class是不是要被实例化,需要=>反射new\n5.将你反射生成的对象,放入容器(singletonObjects)\n6.提供 Object getBean(String key),Object指容器中对应的对象",
			"rawText": "1.传入fishx_SpringConfig.class\n2.根据传入的class能够解析到@ComponentScan的value=> 扫描的包\n3.通过(类的加载器、IO、注解解析)得到包下的加载资源(.class)\n4.再查看该类class是不是要被实例化,需要=>反射new\n5.将你反射生成的对象,放入容器(singletonObjects)\n6.提供 Object getBean(String key),Object指容器中对应的对象",
			"baseline": 127,
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "1.传入fishx_SpringConfig.class\n2.根据传入的class能够解析到@ComponentScan的value=> 扫描的包\n3.通过(类的加载器、IO、注解解析)得到包下的加载资源(.class)\n4.再查看该类class是不是要被实例化,需要=>反射new\n5.将你反射生成的对象,放入容器(singletonObjects)\n6.提供 Object getBean(String key),Object指容器中对应的对象"
		},
		{
			"type": "arrow",
			"version": 256,
			"versionNonce": 1194549485,
			"isDeleted": false,
			"id": "0RQ_7Rgh9F8aTB6qEjKJk",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 306.98887467725024,
			"y": -106.54850260416674,
			"strokeColor": "#d9480f",
			"backgroundColor": "transparent",
			"width": 48.52520915084108,
			"height": 115.26048519736844,
			"seed": 1947665347,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1672889456256,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "Nzgok7x7JhtMWGl0q_fQz",
				"focus": 0.5112916874891806,
				"gap": 3.4163411458332646
			},
			"endBinding": {
				"elementId": "ez0fYL8f",
				"focus": 0.4836079805463726,
				"gap": 13.039957682291742
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
					-48.52520915084108,
					115.26048519736844
				]
			]
		},
		{
			"type": "arrow",
			"version": 48,
			"versionNonce": 14757987,
			"isDeleted": false,
			"id": "YndeZn_cSx102K0bEoceC",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 603.1640625,
			"y": -106.17350260416674,
			"strokeColor": "#d9480f",
			"backgroundColor": "transparent",
			"width": 32.62109375,
			"height": 120.65625,
			"seed": 1311946253,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1672889456256,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "Nzgok7x7JhtMWGl0q_fQz",
				"focus": -0.35716467126828466,
				"gap": 3.7913411458332646
			},
			"endBinding": {
				"elementId": "Rhwu5fZO7O1bY5biJpQ9Q",
				"focus": 0.1826559807805819,
				"gap": 11.864176432291742
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
					32.62109375,
					120.65625
				]
			]
		},
		{
			"type": "rectangle",
			"version": 200,
			"versionNonce": 1565702989,
			"isDeleted": false,
			"id": "teXJu2ehBYFvfztWIND8z",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": -567.2080004699251,
			"y": -40.40877064487404,
			"strokeColor": "#d9480f",
			"backgroundColor": "transparent",
			"width": 1348.9062500000005,
			"height": 249.60379464285722,
			"seed": 26619053,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1672889456256,
			"link": null,
			"locked": false
		},
		{
			"type": "arrow",
			"version": 94,
			"versionNonce": 1582163917,
			"isDeleted": false,
			"id": "mnrSbWUTYqyDBd40Wgsth",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": -0.46457941729249796,
			"y": 94.01325135959962,
			"strokeColor": "#d9480f",
			"backgroundColor": "transparent",
			"width": 74.375,
			"height": 2.635690789473756,
			"seed": 2053022765,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1672889661804,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "zE5xtjCa",
				"focus": -0.0935553132123743,
				"gap": 4.317081766917909
			},
			"endBinding": {
				"elementId": "z5MK9Z61",
				"focus": 0.7605919978611541,
				"gap": 7.291470864660937
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
					74.375,
					2.635690789473756
				]
			]
		},
		{
			"id": "hKdLXsjb",
			"type": "text",
			"x": -522.2285596804511,
			"y": 73.94335004381071,
			"width": 10,
			"height": 19,
			"angle": 0,
			"strokeColor": "#d9480f",
			"backgroundColor": "transparent",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"groupIds": [],
			"roundness": null,
			"seed": 577834435,
			"version": 3,
			"versionNonce": 1966103907,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1672889637399,
			"link": null,
			"locked": false,
			"text": "",
			"rawText": "",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 15,
			"containerId": null,
			"originalText": ""
		}
	],
	"appState": {
		"theme": "light",
		"viewBackgroundColor": "#ffffff",
		"currentItemStrokeColor": "#d9480f",
		"currentItemBackgroundColor": "transparent",
		"currentItemFillStyle": "hachure",
		"currentItemStrokeWidth": 1,
		"currentItemStrokeStyle": "solid",
		"currentItemRoughness": 2,
		"currentItemOpacity": 100,
		"currentItemFontFamily": 3,
		"currentItemFontSize": 16,
		"currentItemTextAlign": "left",
		"currentItemStartArrowhead": null,
		"currentItemEndArrowhead": "arrow",
		"scrollX": 626.4390859962406,
		"scrollY": 486.69809732461033,
		"zoom": {
			"value": 0.9500000000000001
		},
		"currentItemRoundness": "sharp",
		"gridSize": null,
		"colorPalette": {}
	},
	"files": {}
}
```
%%