---

excalidraw-plugin: parsed
tags: [excalidraw]

---
==⚠  Switch to EXCALIDRAW VIEW in the MORE OPTIONS menu of this document. ⚠==


# Text Elements
浏览器 ^b0YsLMOE

中央控制器/分发控制器
DispatcherServlet ^BSrvZe5Q

1.发送请求url ^29lTBsPr

HandlerMapping
处理器映射器 ^OwGm0ZHF

2.调用处理器映射 ^Lx6KMbq3

3.返回处理器执行链条 ^LwsMNApn

HandlerExecutionChain
处理器执行器链 ^7FmYdQkC

HandlerInterceptor
拦截器[多个] ^wUqzSmPU

Handler ^rhsMN4g0

处理器适配器
HandlerAdapter ^VpVgBlw5

4.调用处理器适配器 ^R445T9g7

处理器Handler
也就是Controller
或者是Servlet ^NZcWEeNd

Service -> Dao ^wOGiSRZE

MVC ^7HjvoMiF

5.调用Handler ^tzwLRrWS

6.返回ModelAndView ^2xJlTsDD

7.返回ModelAndView ^laxpfOgX

视图解析器
ViewResolve ^InsfgSOC

8.调用视图解析器 ^RMxjH1fd

9.返回view ^VSyZ4Nay

view视图
HTML/JSP/thymeleaf ^8r56Tz7R

10.视图渲染
将Model中的数据填充到view ^L2kQpfwW

11.返回响应 ^uSW9B1fv

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
			"version": 262,
			"versionNonce": 426821258,
			"isDeleted": false,
			"id": "3cvU7sTx3h8LwVUlPihmZ",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": -427.998046875,
			"y": -57.7421875,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 161,
			"height": 92,
			"seed": 1458698156,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"type": "text",
					"id": "b0YsLMOE"
				},
				{
					"id": "DjkwgUUJWLW1n2mDkS-YA",
					"type": "arrow"
				}
			],
			"updated": 1673862739762,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 166,
			"versionNonce": 1168170582,
			"isDeleted": false,
			"id": "b0YsLMOE",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": -377.998046875,
			"y": -23.7421875,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 61,
			"height": 24,
			"seed": 267015572,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1673863194888,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 3,
			"text": "浏览器",
			"rawText": "浏览器",
			"baseline": 19,
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "3cvU7sTx3h8LwVUlPihmZ",
			"originalText": "浏览器"
		},
		{
			"type": "rectangle",
			"version": 325,
			"versionNonce": 1500210506,
			"isDeleted": false,
			"id": "yCQSzVuLEYR1glWd8kkYg",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": -53.04296875,
			"y": -130.662109375,
			"strokeColor": "#000000",
			"backgroundColor": "#ced4da",
			"width": 276,
			"height": 147,
			"seed": 900666540,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"type": "text",
					"id": "BSrvZe5Q"
				},
				{
					"id": "DjkwgUUJWLW1n2mDkS-YA",
					"type": "arrow"
				},
				{
					"id": "llNRJJBakcqlvOGcAJA9N",
					"type": "arrow"
				},
				{
					"id": "0GJcCDPMFKOOSzQgXNOZ6",
					"type": "arrow"
				},
				{
					"id": "tDWyYA5ui-pVSEcn1QX-0",
					"type": "arrow"
				},
				{
					"id": "aFrweEmFC4J8760RIoiN0",
					"type": "arrow"
				},
				{
					"id": "8lA-ks3y5iFhnX8yGKy1c",
					"type": "arrow"
				},
				{
					"id": "LP2IUq1kzkLOa4daeTwwj",
					"type": "arrow"
				},
				{
					"id": "uJjQF_7F73sann7UGzQCC",
					"type": "arrow"
				},
				{
					"id": "KlCQT5Y24Jv_fKOMMT12j",
					"type": "arrow"
				}
			],
			"updated": 1673862739762,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 294,
			"versionNonce": 1135791446,
			"isDeleted": false,
			"id": "BSrvZe5Q",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": -21.54296875,
			"y": -81.162109375,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 213,
			"height": 48,
			"seed": 1070574228,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1673863194910,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 3,
			"text": "中央控制器/分发控制器\nDispatcherServlet",
			"rawText": "中央控制器/分发控制器\nDispatcherServlet",
			"baseline": 43,
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "yCQSzVuLEYR1glWd8kkYg",
			"originalText": "中央控制器/分发控制器\nDispatcherServlet"
		},
		{
			"type": "arrow",
			"version": 359,
			"versionNonce": 31368086,
			"isDeleted": false,
			"id": "DjkwgUUJWLW1n2mDkS-YA",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": -265.8927542264344,
			"y": -41.09458079060302,
			"strokeColor": "#000000",
			"backgroundColor": "#ced4da",
			"width": 211.8497854764344,
			"height": 37.6348340782252,
			"seed": 1516886932,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"type": "text",
					"id": "29lTBsPr"
				}
			],
			"updated": 1673863194892,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "3cvU7sTx3h8LwVUlPihmZ",
				"gap": 1.1052926485655732,
				"focus": -0.2463539704454564
			},
			"endBinding": {
				"elementId": "yCQSzVuLEYR1glWd8kkYg",
				"gap": 1,
				"focus": 0.47197089603450204
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
					211.8497854764344,
					-37.6348340782252
				]
			]
		},
		{
			"type": "text",
			"version": 28,
			"versionNonce": 658002762,
			"isDeleted": false,
			"id": "29lTBsPr",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": -215.96786148821718,
			"y": -69.41197210071262,
			"strokeColor": "#000000",
			"backgroundColor": "#ced4da",
			"width": 112,
			"height": 19,
			"seed": 1924019372,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1673863194887,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "1.发送请求url",
			"rawText": "1.发送请求url",
			"baseline": 15,
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "DjkwgUUJWLW1n2mDkS-YA",
			"originalText": "1.发送请求url"
		},
		{
			"type": "rectangle",
			"version": 411,
			"versionNonce": 1435900618,
			"isDeleted": false,
			"id": "tJBrI_Vh1gwbzyZhZKzGc",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": -0.5927734375,
			"y": -363.728515625,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 184,
			"height": 73,
			"seed": 1087070252,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"type": "text",
					"id": "OwGm0ZHF"
				},
				{
					"id": "0GJcCDPMFKOOSzQgXNOZ6",
					"type": "arrow"
				},
				{
					"id": "llNRJJBakcqlvOGcAJA9N",
					"type": "arrow"
				}
			],
			"updated": 1673862739763,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 272,
			"versionNonce": 5237194,
			"isDeleted": false,
			"id": "OwGm0ZHF",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 25.4072265625,
			"y": -346.228515625,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 132,
			"height": 38,
			"seed": 6270892,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1673863194917,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "HandlerMapping\n处理器映射器",
			"rawText": "HandlerMapping\n处理器映射器",
			"baseline": 34,
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "tJBrI_Vh1gwbzyZhZKzGc",
			"originalText": "HandlerMapping\n处理器映射器"
		},
		{
			"type": "arrow",
			"version": 374,
			"versionNonce": 1708831382,
			"isDeleted": false,
			"id": "llNRJJBakcqlvOGcAJA9N",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 23.42739541371303,
			"y": -135.04248847336066,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 3.1430737761333774,
			"height": 152.95274590163933,
			"seed": 479203500,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"type": "text",
					"id": "Lx6KMbq3"
				}
			],
			"updated": 1673863194914,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "yCQSzVuLEYR1glWd8kkYg",
				"gap": 4.380379098360663,
				"focus": -0.4464772074600337
			},
			"endBinding": {
				"elementId": "tJBrI_Vh1gwbzyZhZKzGc",
				"gap": 2.7332812500000045,
				"focus": 0.7766624078661711
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
					0.15365927378697108,
					-82.22093926101434
				],
				[
					-2.9894145023464063,
					-152.95274590163933
				]
			]
		},
		{
			"type": "text",
			"version": 33,
			"versionNonce": 347193558,
			"isDeleted": false,
			"id": "Lx6KMbq3",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": -42.4189453125,
			"y": -226.763427734375,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 132,
			"height": 19,
			"seed": 195686292,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1673863194895,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "2.调用处理器映射",
			"rawText": "2.调用处理器映射",
			"baseline": 15,
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "llNRJJBakcqlvOGcAJA9N",
			"originalText": "2.调用处理器映射"
		},
		{
			"type": "arrow",
			"version": 490,
			"versionNonce": 531356426,
			"isDeleted": false,
			"id": "0GJcCDPMFKOOSzQgXNOZ6",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 165.80221686954465,
			"y": -281.069453125,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 2.8462186057225267,
			"height": 147.24507428278687,
			"seed": 1149544980,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"type": "text",
					"id": "LwsMNApn"
				}
			],
			"updated": 1673863194913,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "tJBrI_Vh1gwbzyZhZKzGc",
				"gap": 9.659062500000005,
				"focus": -0.8018132628925307
			},
			"endBinding": {
				"elementId": "yCQSzVuLEYR1glWd8kkYg",
				"gap": 3.1622694672131217,
				"focus": 0.5300477677185281
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
					0.735869067955349,
					88.356806640625
				],
				[
					-2.1103495377671777,
					147.24507428278687
				]
			]
		},
		{
			"type": "text",
			"version": 106,
			"versionNonce": 833566230,
			"isDeleted": false,
			"id": "LwsMNApn",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 84.5380859375,
			"y": -202.212646484375,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 164,
			"height": 19,
			"seed": 1233041812,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1673863194896,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "3.返回处理器执行链条",
			"rawText": "3.返回处理器执行链条",
			"baseline": 15,
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "0GJcCDPMFKOOSzQgXNOZ6",
			"originalText": "3.返回处理器执行链条"
		},
		{
			"type": "rectangle",
			"version": 179,
			"versionNonce": 1500567306,
			"isDeleted": false,
			"id": "S3o-1_LMqAcPJgR0JGrdz",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 368.4892578125,
			"y": -369.072021484375,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 259.20312499999994,
			"height": 159.81640625,
			"seed": 1542015404,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"id": "vud964fvPFZfICeLfEqeA",
					"type": "arrow"
				}
			],
			"updated": 1673862739763,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 112,
			"versionNonce": 1408464534,
			"isDeleted": false,
			"id": "7FmYdQkC",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 394.4794921875,
			"y": -365.900146484375,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 197,
			"height": 41,
			"seed": 1861925012,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1673862739763,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "HandlerExecutionChain\n处理器执行器链",
			"rawText": "HandlerExecutionChain\n处理器执行器链",
			"baseline": 36,
			"textAlign": "center",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "HandlerExecutionChain\n处理器执行器链"
		},
		{
			"type": "rectangle",
			"version": 201,
			"versionNonce": 806708682,
			"isDeleted": false,
			"id": "c7J0ZKB4xdhaPYxOAsIER",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dotted",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 397.8974609375,
			"y": -315.468505859375,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 192,
			"height": 48,
			"seed": 931076372,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"type": "text",
					"id": "wUqzSmPU"
				}
			],
			"updated": 1673862739763,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 157,
			"versionNonce": 1549004758,
			"isDeleted": false,
			"id": "wUqzSmPU",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 408.8974609375,
			"y": -310.468505859375,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 170,
			"height": 38,
			"seed": 590776212,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1673863194920,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "HandlerInterceptor\n拦截器[多个]",
			"rawText": "HandlerInterceptor\n拦截器[多个]",
			"baseline": 34,
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "c7J0ZKB4xdhaPYxOAsIER",
			"originalText": "HandlerInterceptor\n拦截器[多个]"
		},
		{
			"type": "rectangle",
			"version": 150,
			"versionNonce": 1220156554,
			"isDeleted": false,
			"id": "5_rGqZogudxPkD3tvWzvP",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dotted",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 438.8193359375,
			"y": -255.704833984375,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 113,
			"height": 32,
			"seed": 1591864596,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"type": "text",
					"id": "rhsMN4g0"
				}
			],
			"updated": 1673862739763,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 81,
			"versionNonce": 1290488086,
			"isDeleted": false,
			"id": "rhsMN4g0",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dotted",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 461.8193359375,
			"y": -249.204833984375,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 67,
			"height": 19,
			"seed": 1206102828,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1673862739764,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "Handler",
			"rawText": "Handler",
			"baseline": 15,
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "5_rGqZogudxPkD3tvWzvP",
			"originalText": "Handler"
		},
		{
			"type": "arrow",
			"version": 100,
			"versionNonce": 1092221770,
			"isDeleted": false,
			"id": "vud964fvPFZfICeLfEqeA",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dotted",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 246.4248046875,
			"y": -194.868896484375,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 120.375,
			"height": 71.21484375,
			"seed": 1669721876,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1673862739764,
			"link": null,
			"locked": false,
			"startBinding": null,
			"endBinding": {
				"elementId": "S3o-1_LMqAcPJgR0JGrdz",
				"focus": 0.34865389950842096,
				"gap": 1.689453125
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
					120.375,
					-71.21484375
				]
			]
		},
		{
			"type": "rectangle",
			"version": 181,
			"versionNonce": 1913501270,
			"isDeleted": false,
			"id": "po5KfMka6hH-GkGGtuimJ",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 456.0849609375,
			"y": -124.1826171875,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 203,
			"height": 84,
			"seed": 2112885396,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"type": "text",
					"id": "VpVgBlw5"
				},
				{
					"id": "aFrweEmFC4J8760RIoiN0",
					"type": "arrow"
				},
				{
					"id": "tDWyYA5ui-pVSEcn1QX-0",
					"type": "arrow"
				},
				{
					"id": "qm9GbYF9obmk4LeiRO_zy",
					"type": "arrow"
				},
				{
					"id": "ondX3diXQPNC028bm-A2K",
					"type": "arrow"
				}
			],
			"updated": 1673862739764,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 144,
			"versionNonce": 1517456266,
			"isDeleted": false,
			"id": "VpVgBlw5",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 491.5849609375,
			"y": -101.1826171875,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 132,
			"height": 38,
			"seed": 1720941972,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1673863194937,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "处理器适配器\nHandlerAdapter",
			"rawText": "处理器适配器\nHandlerAdapter",
			"baseline": 34,
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "po5KfMka6hH-GkGGtuimJ",
			"originalText": "处理器适配器\nHandlerAdapter"
		},
		{
			"type": "arrow",
			"version": 170,
			"versionNonce": 1810788950,
			"isDeleted": false,
			"id": "tDWyYA5ui-pVSEcn1QX-0",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 224.72558593750003,
			"y": -95.6865234375,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 226.42578124999997,
			"height": 4.46875,
			"seed": 574687276,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"type": "text",
					"id": "R445T9g7"
				}
			],
			"updated": 1673863194933,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "yCQSzVuLEYR1glWd8kkYg",
				"gap": 1.7685546875,
				"focus": -0.4692240611054801
			},
			"endBinding": {
				"elementId": "po5KfMka6hH-GkGGtuimJ",
				"gap": 4.93359375,
				"focus": 0.4561766120352451
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
					226.42578124999997,
					-4.46875
				]
			]
		},
		{
			"type": "text",
			"version": 48,
			"versionNonce": 57214218,
			"isDeleted": false,
			"id": "R445T9g7",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 263.9384765625,
			"y": -107.4208984375,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 148,
			"height": 19,
			"seed": 680229036,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1673863194898,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "4.调用处理器适配器",
			"rawText": "4.调用处理器适配器",
			"baseline": 15,
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "tDWyYA5ui-pVSEcn1QX-0",
			"originalText": "4.调用处理器适配器"
		},
		{
			"type": "rectangle",
			"version": 181,
			"versionNonce": 2110298326,
			"isDeleted": false,
			"id": "oA9E5iZCkUoOarL7iBXX8",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 450.5146484375,
			"y": 84.2921142578125,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 192,
			"height": 97,
			"seed": 519366444,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"type": "text",
					"id": "NZcWEeNd"
				},
				{
					"id": "q27lz50QD4OlzBYm7nDFz",
					"type": "arrow"
				},
				{
					"id": "ondX3diXQPNC028bm-A2K",
					"type": "arrow"
				},
				{
					"id": "qm9GbYF9obmk4LeiRO_zy",
					"type": "arrow"
				}
			],
			"updated": 1673862739764,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 96,
			"versionNonce": 648952982,
			"isDeleted": false,
			"id": "NZcWEeNd",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 475.0146484375,
			"y": 104.2921142578125,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 143,
			"height": 57,
			"seed": 1969929876,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1673863194943,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "处理器Handler\n也就是Controller\n或者是Servlet",
			"rawText": "处理器Handler\n也就是Controller\n或者是Servlet",
			"baseline": 53,
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "oA9E5iZCkUoOarL7iBXX8",
			"originalText": "处理器Handler\n也就是Controller\n或者是Servlet"
		},
		{
			"type": "arrow",
			"version": 452,
			"versionNonce": 237386198,
			"isDeleted": false,
			"id": "q27lz50QD4OlzBYm7nDFz",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 550.0042793660509,
			"y": 182.2921142578125,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 3.224422820694599,
			"height": 63.368452936810854,
			"seed": 240398740,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1673863194945,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "oA9E5iZCkUoOarL7iBXX8",
				"gap": 1,
				"focus": -0.009862381459874656
			},
			"endBinding": {
				"elementId": "US11n08MjnVxsjtLwbZuB",
				"gap": 1.9014445241266458,
				"focus": 0.023791531507094982
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
					3.224422820694599,
					63.368452936810854
				]
			]
		},
		{
			"type": "rectangle",
			"version": 234,
			"versionNonce": 1415906890,
			"isDeleted": false,
			"id": "US11n08MjnVxsjtLwbZuB",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 466.2080078125,
			"y": 247.56201171875,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 173,
			"height": 58,
			"seed": 1201294892,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"id": "q27lz50QD4OlzBYm7nDFz",
					"type": "arrow"
				},
				{
					"type": "text",
					"id": "wOGiSRZE"
				}
			],
			"updated": 1673862739764,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 81,
			"versionNonce": 1426905942,
			"isDeleted": false,
			"id": "wOGiSRZE",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 486.7080078125,
			"y": 267.06201171875,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 132,
			"height": 19,
			"seed": 66890900,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1673862739764,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "Service -> Dao",
			"rawText": "Service -> Dao",
			"baseline": 15,
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "US11n08MjnVxsjtLwbZuB",
			"originalText": "Service -> Dao"
		},
		{
			"type": "rectangle",
			"version": 131,
			"versionNonce": 1933683978,
			"isDeleted": false,
			"id": "ft6A5uOQQf5A7fPlh0ie1",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 398.3310546875,
			"y": 70.35498046875,
			"strokeColor": "#5c940d",
			"backgroundColor": "transparent",
			"width": 331.87109375000006,
			"height": 248.23046875,
			"seed": 1631774996,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"id": "qm9GbYF9obmk4LeiRO_zy",
					"type": "arrow"
				},
				{
					"id": "ondX3diXQPNC028bm-A2K",
					"type": "arrow"
				}
			],
			"updated": 1673862739764,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 179,
			"versionNonce": 482541718,
			"isDeleted": false,
			"id": "7HjvoMiF",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 692.5966796875,
			"y": 189.73388671875,
			"strokeColor": "#5c940d",
			"backgroundColor": "transparent",
			"width": 29,
			"height": 19,
			"seed": 593473172,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1673862739764,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "MVC",
			"rawText": "MVC",
			"baseline": 15,
			"textAlign": "center",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "MVC"
		},
		{
			"type": "arrow",
			"version": 410,
			"versionNonce": 716150614,
			"isDeleted": false,
			"id": "ondX3diXQPNC028bm-A2K",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 616.4035576901099,
			"y": -38.36433015046296,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 4.733635104428004,
			"height": 120.06997973736839,
			"seed": 434051092,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"type": "text",
					"id": "tzwLRrWS"
				}
			],
			"updated": 1673863194939,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "po5KfMka6hH-GkGGtuimJ",
				"gap": 1.818287037037038,
				"focus": -0.5184090007697589
			},
			"endBinding": {
				"elementId": "oA9E5iZCkUoOarL7iBXX8",
				"gap": 2.586464670907077,
				"focus": 0.7779912056386644
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
					4.376715747390108,
					46.30183015046296
				],
				[
					4.733635104428004,
					120.06997973736839
				]
			]
		},
		{
			"type": "text",
			"version": 226,
			"versionNonce": 1269454038,
			"isDeleted": false,
			"id": "tzwLRrWS",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 562.2802734375,
			"y": -1.5625,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 117,
			"height": 19,
			"seed": 44246164,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1673863194936,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "5.调用Handler",
			"rawText": "5.调用Handler",
			"baseline": 15,
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "ondX3diXQPNC028bm-A2K",
			"originalText": "5.调用Handler"
		},
		{
			"type": "arrow",
			"version": 720,
			"versionNonce": 1262006538,
			"isDeleted": false,
			"id": "qm9GbYF9obmk4LeiRO_zy",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 476.7919921875,
			"y": 80.07409667968749,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 2.5859375,
			"height": 116.45312499999999,
			"seed": 1449045548,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"type": "text",
					"id": "2xJlTsDD"
				}
			],
			"updated": 1673863194941,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "oA9E5iZCkUoOarL7iBXX8",
				"gap": 4.218017578125,
				"focus": -0.7201765116021659
			},
			"endBinding": {
				"elementId": "po5KfMka6hH-GkGGtuimJ",
				"gap": 3.8035888671875,
				"focus": 0.7427536711729052
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
					-0.359375,
					-53.777343749999986
				],
				[
					2.2265625,
					-116.45312499999999
				]
			]
		},
		{
			"type": "text",
			"version": 288,
			"versionNonce": 827963286,
			"isDeleted": false,
			"id": "2xJlTsDD",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 394.4326171875,
			"y": 16.7967529296875,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 164,
			"height": 19,
			"seed": 1143801236,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1673863194935,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "6.返回ModelAndView",
			"rawText": "6.返回ModelAndView",
			"baseline": 15,
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "qm9GbYF9obmk4LeiRO_zy",
			"originalText": "6.返回ModelAndView"
		},
		{
			"type": "arrow",
			"version": 151,
			"versionNonce": 1127449878,
			"isDeleted": false,
			"id": "aFrweEmFC4J8760RIoiN0",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 452.2021484375,
			"y": -57.00592765045921,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 228.2451171875,
			"height": 37.697318010178265,
			"seed": 1904690452,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"type": "text",
					"id": "laxpfOgX"
				}
			],
			"updated": 1673863194931,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "po5KfMka6hH-GkGGtuimJ",
				"gap": 3.8828125,
				"focus": -0.13224852196938622
			},
			"endBinding": {
				"elementId": "yCQSzVuLEYR1glWd8kkYg",
				"gap": 1,
				"focus": 0.6315246306348853
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
					-228.2451171875,
					37.697318010178265
				]
			]
		},
		{
			"type": "text",
			"version": 27,
			"versionNonce": 1377281494,
			"isDeleted": false,
			"id": "laxpfOgX",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 256.07958984375,
			"y": -47.650874459193815,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 164,
			"height": 19,
			"seed": 716027540,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1673863194904,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "7.返回ModelAndView",
			"rawText": "7.返回ModelAndView",
			"baseline": 15,
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "aFrweEmFC4J8760RIoiN0",
			"originalText": "7.返回ModelAndView"
		},
		{
			"type": "rectangle",
			"version": 168,
			"versionNonce": 1061541898,
			"isDeleted": false,
			"id": "ef9xqTIzE-WYxOdLhxBDk",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 9.9228515625,
			"y": 136.35546875,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 164,
			"height": 95,
			"seed": 99212460,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"type": "text",
					"id": "InsfgSOC"
				},
				{
					"id": "8lA-ks3y5iFhnX8yGKy1c",
					"type": "arrow"
				},
				{
					"id": "LP2IUq1kzkLOa4daeTwwj",
					"type": "arrow"
				}
			],
			"updated": 1673862739765,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 65,
			"versionNonce": 832232458,
			"isDeleted": false,
			"id": "InsfgSOC",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 39.9228515625,
			"y": 164.85546875,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 104,
			"height": 38,
			"seed": 1765384212,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1673863194954,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "视图解析器\nViewResolve",
			"rawText": "视图解析器\nViewResolve",
			"baseline": 34,
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "ef9xqTIzE-WYxOdLhxBDk",
			"originalText": "视图解析器\nViewResolve"
		},
		{
			"type": "arrow",
			"version": 157,
			"versionNonce": 1479315222,
			"isDeleted": false,
			"id": "8lA-ks3y5iFhnX8yGKy1c",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 155.8896484375,
			"y": 20.671875,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 3.59375,
			"height": 111.04296875,
			"seed": 443011756,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"type": "text",
					"id": "RMxjH1fd"
				}
			],
			"updated": 1673863194951,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "yCQSzVuLEYR1glWd8kkYg",
				"gap": 4.333984375,
				"focus": -0.48735041349330704
			},
			"endBinding": {
				"elementId": "ef9xqTIzE-WYxOdLhxBDk",
				"gap": 4.640625,
				"focus": 0.8289474189378698
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
					3.59375,
					111.04296875
				]
			]
		},
		{
			"type": "text",
			"version": 30,
			"versionNonce": 1620633930,
			"isDeleted": false,
			"id": "RMxjH1fd",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 91.6865234375,
			"y": 66.693359375,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 132,
			"height": 19,
			"seed": 52365100,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1673863194905,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "8.调用视图解析器",
			"rawText": "8.调用视图解析器",
			"baseline": 15,
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "8lA-ks3y5iFhnX8yGKy1c",
			"originalText": "8.调用视图解析器"
		},
		{
			"type": "arrow",
			"version": 65,
			"versionNonce": 1362508886,
			"isDeleted": false,
			"id": "LP2IUq1kzkLOa4daeTwwj",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 29.0537109375,
			"y": 131.67578125,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 1.375000000000007,
			"height": 109.48828125,
			"seed": 1878688020,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"type": "text",
					"id": "VSyZ4Nay"
				}
			],
			"updated": 1673863194952,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "ef9xqTIzE-WYxOdLhxBDk",
				"gap": 4.6796875,
				"focus": -0.7532259509438154
			},
			"endBinding": {
				"elementId": "yCQSzVuLEYR1glWd8kkYg",
				"gap": 5.849609375,
				"focus": 0.4194755923121967
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
					-1.375000000000007,
					-109.48828125
				]
			]
		},
		{
			"type": "text",
			"version": 19,
			"versionNonce": 2062626198,
			"isDeleted": false,
			"id": "VSyZ4Nay",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": -16.1337890625,
			"y": 67.431640625,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 89,
			"height": 19,
			"seed": 914954796,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1673863194907,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "9.返回view",
			"rawText": "9.返回view",
			"baseline": 15,
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "LP2IUq1kzkLOa4daeTwwj",
			"originalText": "9.返回view"
		},
		{
			"type": "rectangle",
			"version": 163,
			"versionNonce": 1604492362,
			"isDeleted": false,
			"id": "WcNrN3hM-YPowS8CEGwVs",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": -315.2236328125,
			"y": 136.453369140625,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 207,
			"height": 81,
			"seed": 1369146004,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"type": "text",
					"id": "8r56Tz7R"
				},
				{
					"id": "uJjQF_7F73sann7UGzQCC",
					"type": "arrow"
				}
			],
			"updated": 1673862739765,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 141,
			"versionNonce": 621256406,
			"isDeleted": false,
			"id": "8r56Tz7R",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": -296.7236328125,
			"y": 157.953369140625,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 170,
			"height": 38,
			"seed": 1498199724,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1673863194959,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "view视图\nHTML/JSP/thymeleaf",
			"rawText": "view视图\nHTML/JSP/thymeleaf",
			"baseline": 34,
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "WcNrN3hM-YPowS8CEGwVs",
			"originalText": "view视图\nHTML/JSP/thymeleaf"
		},
		{
			"type": "arrow",
			"version": 143,
			"versionNonce": 1589037770,
			"isDeleted": false,
			"id": "uJjQF_7F73sann7UGzQCC",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": -57.579242023222776,
			"y": 17.18263907919352,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 165.03662806812937,
			"height": 118.27073006143148,
			"seed": 1278021164,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"type": "text",
					"id": "L2kQpfwW"
				}
			],
			"updated": 1673863194956,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "yCQSzVuLEYR1glWd8kkYg",
				"gap": 4.6142578125,
				"focus": 0.16126580208401384
			},
			"endBinding": {
				"elementId": "WcNrN3hM-YPowS8CEGwVs",
				"gap": 1,
				"focus": -0.4299736433666816
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
					-165.03662806812937,
					118.27073006143148
				]
			]
		},
		{
			"type": "text",
			"version": 116,
			"versionNonce": 2093495690,
			"isDeleted": false,
			"id": "L2kQpfwW",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": -246.8818359375,
			"y": 57.521728515625,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 213,
			"height": 38,
			"seed": 1737151252,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1673863194908,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "10.视图渲染\n将Model中的数据填充到view",
			"rawText": "10.视图渲染\n将Model中的数据填充到view",
			"baseline": 34,
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "uJjQF_7F73sann7UGzQCC",
			"originalText": "10.视图渲染\n将Model中的数据填充到view"
		},
		{
			"type": "arrow",
			"version": 54,
			"versionNonce": 992428054,
			"isDeleted": false,
			"id": "KlCQT5Y24Jv_fKOMMT12j",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": -55.6064453125,
			"y": -17.046630859375,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 214.4140625,
			"height": 37.44140625,
			"seed": 531490732,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"type": "text",
					"id": "uSW9B1fv"
				}
			],
			"updated": 1673863194908,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "yCQSzVuLEYR1glWd8kkYg",
				"gap": 2.5634765625,
				"focus": -0.15953230552301528
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
					-214.4140625,
					37.44140625
				]
			]
		},
		{
			"type": "text",
			"version": 26,
			"versionNonce": 1576858698,
			"isDeleted": false,
			"id": "uSW9B1fv",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": -209.3134765625,
			"y": -7.825927734375,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 93,
			"height": 19,
			"seed": 131172372,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1673863194909,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "11.返回响应",
			"rawText": "11.返回响应",
			"baseline": 15,
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "KlCQT5Y24Jv_fKOMMT12j",
			"originalText": "11.返回响应"
		}
	],
	"appState": {
		"theme": "light",
		"viewBackgroundColor": "#ffffff",
		"currentItemStrokeColor": "#000000",
		"currentItemBackgroundColor": "transparent",
		"currentItemFillStyle": "hachure",
		"currentItemStrokeWidth": 1,
		"currentItemStrokeStyle": "solid",
		"currentItemRoughness": 2,
		"currentItemOpacity": 100,
		"currentItemFontFamily": 3,
		"currentItemFontSize": 16,
		"currentItemTextAlign": "center",
		"currentItemStartArrowhead": null,
		"currentItemEndArrowhead": "arrow",
		"scrollX": 465.92051866319446,
		"scrollY": 485.34745279947913,
		"zoom": {
			"value": 0.9
		},
		"currentItemRoundness": "sharp",
		"gridSize": null,
		"colorPalette": {}
	},
	"files": {}
}
```
%%