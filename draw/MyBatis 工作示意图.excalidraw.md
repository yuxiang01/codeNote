---

excalidraw-plugin: parsed
tags: [excalidraw]

---
==⚠  Switch to EXCALIDRAW VIEW in the MORE OPTIONS menu of this document. ⚠==


# Text Elements
Java程序 ^OokxM3XB

Java操作DB ^naxa8aCg

这里我们要操作DB-monster
1.先创建Monster对象
2.通过MyBatis的mybatis-config文件获取到
SessionFactory对象(可以理解成连接池)
3.通过SessionFactory对象获取到SqlSession(可以理解成是连接)
4.获取到MonsterMapper的对象
5.调用MonsterMapper的方法,完成monster表各种操作 ^uExJZzgQ

Monster类-POJO ^sxsgwGEO

MonsterMapper.java接口声明方法CRUD
(1).方法实现可以是注解
(2).也可以是对应的XMapper.xml去完成 ^sPBVr2KQ

MyBatis框架 ^O7Z9HGSQ

mybatis-config.xml
1.配置数据库连接/数据源
2.管理XXMapper.xml ^zz9FIy5A

连接池 ^I8cvlNA6

xxMapper.xml
配置SQL,完成对Monster表的CRUD ^pl38d1jQ

分析MyBatis优势:
1.数据库的连接/连接池,只需配置即可
2.程序是以OOP方式来操作DB
3.SQL语句是可以写在xml文件,实现了解耦
4.MyBatis可以对DB操作进行优化,提高效率 ^Bqt2q056

Monster表 ^X71DMtf5

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
			"version": 324,
			"versionNonce": 1813908594,
			"isDeleted": false,
			"id": "xm7U_i9F2Yx56O2OtehmE",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": -485.02734375,
			"y": -260.34375,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 1195.4140625,
			"height": 409.59765625,
			"seed": 161598858,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"id": "70mOHcCGFsU1vVmvQQuOA",
					"type": "arrow"
				}
			],
			"updated": 1674826612090,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 160,
			"versionNonce": 1978444718,
			"isDeleted": false,
			"id": "OokxM3XB",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": -475.16015625,
			"y": -253.87890625,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 88,
			"height": 28,
			"seed": 1361187670,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1674826612090,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 3,
			"text": "Java程序",
			"rawText": "Java程序",
			"baseline": 21,
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "Java程序"
		},
		{
			"type": "rectangle",
			"version": 548,
			"versionNonce": 759672370,
			"isDeleted": false,
			"id": "PNoFEOjvBxC41H56zD88w",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": -466.75,
			"y": -209.99609375,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 813.421875,
			"height": 300.046875,
			"seed": 1360921802,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1674826612090,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 65,
			"versionNonce": 1861682670,
			"isDeleted": false,
			"id": "naxa8aCg",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": -460.7734375,
			"y": -205.70703125,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 111,
			"height": 28,
			"seed": 1316745674,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1674826612090,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 3,
			"text": "Java操作DB",
			"rawText": "Java操作DB",
			"baseline": 21,
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "Java操作DB"
		},
		{
			"type": "rectangle",
			"version": 695,
			"versionNonce": 1683429362,
			"isDeleted": false,
			"id": "47utzmi3CbvR6GzJBXu2m",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": -454.611328125,
			"y": -163.20703125,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 461,
			"height": 226,
			"seed": 1468543946,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"type": "text",
					"id": "uExJZzgQ"
				}
			],
			"updated": 1674826612091,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 766,
			"versionNonce": 2110125458,
			"isDeleted": false,
			"id": "uExJZzgQ",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": -449.611328125,
			"y": -158.20703125,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 433,
			"height": 216,
			"seed": 691297354,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1675249472730,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 3,
			"text": "这里我们要操作DB-monster\n1.先创建Monster对象\n2.通过MyBatis的mybatis-config文件获取到\nSessionFactory对象(可以理解成连接池)\n3.通过SessionFactory对象获取到SqlSessio\nn(可以理解成是连接)\n4.获取到MonsterMapper的对象\n5.调用MonsterMapper的方法,完成monster表\n各种操作",
			"rawText": "这里我们要操作DB-monster\n1.先创建Monster对象\n2.通过MyBatis的mybatis-config文件获取到\nSessionFactory对象(可以理解成连接池)\n3.通过SessionFactory对象获取到SqlSession(可以理解成是连接)\n4.获取到MonsterMapper的对象\n5.调用MonsterMapper的方法,完成monster表各种操作",
			"baseline": 211,
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": "47utzmi3CbvR6GzJBXu2m",
			"originalText": "这里我们要操作DB-monster\n1.先创建Monster对象\n2.通过MyBatis的mybatis-config文件获取到\nSessionFactory对象(可以理解成连接池)\n3.通过SessionFactory对象获取到SqlSession(可以理解成是连接)\n4.获取到MonsterMapper的对象\n5.调用MonsterMapper的方法,完成monster表各种操作"
		},
		{
			"type": "rectangle",
			"version": 255,
			"versionNonce": 1632848306,
			"isDeleted": false,
			"id": "IASqfRw78VLV58Tkymuzz",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 64.06338778409088,
			"y": -182.82688210227272,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 226,
			"height": 60,
			"seed": 270245014,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"type": "text",
					"id": "sxsgwGEO"
				}
			],
			"updated": 1674826612091,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 282,
			"versionNonce": 569074830,
			"isDeleted": false,
			"id": "sxsgwGEO",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 96.06338778409088,
			"y": -164.82688210227272,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 162,
			"height": 24,
			"seed": 2126009238,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1675249472734,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 3,
			"text": "Monster类-POJO",
			"rawText": "Monster类-POJO",
			"baseline": 19,
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "IASqfRw78VLV58Tkymuzz",
			"originalText": "Monster类-POJO"
		},
		{
			"type": "rectangle",
			"version": 820,
			"versionNonce": 1533231986,
			"isDeleted": false,
			"id": "hn7kFBU2J_qne08ZETckU",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 32.48828125,
			"y": -97.380859375,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 302,
			"height": 143,
			"seed": 89753354,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"type": "text",
					"id": "sPBVr2KQ"
				}
			],
			"updated": 1674826612091,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 252,
			"versionNonce": 1347777362,
			"isDeleted": false,
			"id": "sPBVr2KQ",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 37.48828125,
			"y": -92.380859375,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 281,
			"height": 120,
			"seed": 316496714,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1675249472738,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 3,
			"text": "MonsterMapper.java接口声\n明方法CRUD\n(1).方法实现可以是注解\n(2).也可以是对应的XMapper.\nxml去完成",
			"rawText": "MonsterMapper.java接口声明方法CRUD\n(1).方法实现可以是注解\n(2).也可以是对应的XMapper.xml去完成",
			"baseline": 115,
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": "hn7kFBU2J_qne08ZETckU",
			"originalText": "MonsterMapper.java接口声明方法CRUD\n(1).方法实现可以是注解\n(2).也可以是对应的XMapper.xml去完成"
		},
		{
			"type": "rectangle",
			"version": 145,
			"versionNonce": 214828338,
			"isDeleted": false,
			"id": "MhhBm4AauIEsimdWXQpCy",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 363.9921875,
			"y": -243.3828125,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 330.59765625,
			"height": 371.86328125,
			"seed": 1469013142,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"id": "70mOHcCGFsU1vVmvQQuOA",
					"type": "arrow"
				}
			],
			"updated": 1674826612091,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 44,
			"versionNonce": 1927651054,
			"isDeleted": false,
			"id": "O7Z9HGSQ",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 379.4609375,
			"y": -243.34375,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 123,
			"height": 28,
			"seed": 1220680586,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1674826612091,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 3,
			"text": "MyBatis框架",
			"rawText": "MyBatis框架",
			"baseline": 21,
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "MyBatis框架"
		},
		{
			"type": "rectangle",
			"version": 234,
			"versionNonce": 1325043442,
			"isDeleted": false,
			"id": "21D-INk8C5ROlO-54JhVj",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 382.736328125,
			"y": -188.10546875,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 264,
			"height": 91,
			"seed": 1199900950,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"type": "text",
					"id": "zz9FIy5A"
				}
			],
			"updated": 1674826612091,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 140,
			"versionNonce": 543924942,
			"isDeleted": false,
			"id": "zz9FIy5A",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 387.736328125,
			"y": -183.10546875,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 235,
			"height": 72,
			"seed": 720152790,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1675249472743,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 3,
			"text": "mybatis-config.xml\n1.配置数据库连接/数据源\n2.管理XXMapper.xml",
			"rawText": "mybatis-config.xml\n1.配置数据库连接/数据源\n2.管理XXMapper.xml",
			"baseline": 67,
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": "21D-INk8C5ROlO-54JhVj",
			"originalText": "mybatis-config.xml\n1.配置数据库连接/数据源\n2.管理XXMapper.xml"
		},
		{
			"type": "ellipse",
			"version": 187,
			"versionNonce": 913301682,
			"isDeleted": false,
			"id": "4LAFy5Q4eJSu9SOMukIpp",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 573.3984375,
			"y": -87.8515625,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 115,
			"height": 85,
			"seed": 626467850,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"type": "text",
					"id": "I8cvlNA6"
				},
				{
					"id": "70mOHcCGFsU1vVmvQQuOA",
					"type": "arrow"
				}
			],
			"updated": 1674826612091,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 133,
			"versionNonce": 665794834,
			"isDeleted": false,
			"id": "I8cvlNA6",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 606.3984375,
			"y": -54.8515625,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 49,
			"height": 19,
			"seed": 993209814,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1675249472754,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "连接池",
			"rawText": "连接池",
			"baseline": 15,
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "4LAFy5Q4eJSu9SOMukIpp",
			"originalText": "连接池"
		},
		{
			"type": "rectangle",
			"version": 661,
			"versionNonce": 785709682,
			"isDeleted": false,
			"id": "-Tt98ZMX7XA3iGG3ldlD0",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 387.14453125,
			"y": 10.875,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 274,
			"height": 97,
			"seed": 1478977302,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"type": "text",
					"id": "pl38d1jQ"
				}
			],
			"updated": 1674826612091,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 300,
			"versionNonce": 1489639694,
			"isDeleted": false,
			"id": "pl38d1jQ",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 392.14453125,
			"y": 15.875,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 249,
			"height": 72,
			"seed": 766950730,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1675249472759,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 3,
			"text": "xxMapper.xml\n配置SQL,完成对Monster表\n的CRUD",
			"rawText": "xxMapper.xml\n配置SQL,完成对Monster表的CRUD",
			"baseline": 67,
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": "-Tt98ZMX7XA3iGG3ldlD0",
			"originalText": "xxMapper.xml\n配置SQL,完成对Monster表的CRUD"
		},
		{
			"type": "line",
			"version": 4477,
			"versionNonce": 384883762,
			"isDeleted": false,
			"id": "OpCWP2-CypyR-NtgB6zA0",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 6.269506981971325,
			"x": 899.8686394028308,
			"y": -91.28627169799329,
			"strokeColor": "#0a11d3",
			"backgroundColor": "#228be6",
			"width": 88.21658171083376,
			"height": 113.8575037534261,
			"seed": 1287807538,
			"groupIds": [
				"ojiou7z4HO6gw9satYPjX",
				"Tr3wpofl9AgJsscJB4PLK"
			],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1674826612091,
			"link": null,
			"locked": false,
			"startBinding": null,
			"endBinding": null,
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": null,
			"points": [
				[
					0,
					0
				],
				[
					0.29089298333313673,
					86.05288422061678
				],
				[
					0.013613108737802165,
					95.84963140781468
				],
				[
					4.543349062013738,
					100.08268472409586
				],
				[
					20.317928500125443,
					103.66521849306073
				],
				[
					46.98143617553956,
					104.78076599153316
				],
				[
					72.45665455006592,
					102.9996310009587
				],
				[
					85.99182564238487,
					98.74007888522631
				],
				[
					87.90077837148979,
					95.14923176741362
				],
				[
					88.16888387182134,
					87.26194204835767
				],
				[
					87.95845222911922,
					7.219356674957439
				],
				[
					87.48407176050935,
					-0.3431928547433216
				],
				[
					81.81967725989045,
					-4.569951534960701
				],
				[
					69.89167127292335,
					-7.017866506201685
				],
				[
					42.70935725136615,
					-9.076737761892943
				],
				[
					20.91603533578692,
					-7.849028196182914
				],
				[
					3.775735655469765,
					-3.684787148572539
				],
				[
					-0.047697839012426885,
					-0.0517060607782156
				],
				[
					0,
					0
				]
			]
		},
		{
			"type": "line",
			"version": 2211,
			"versionNonce": 843036654,
			"isDeleted": false,
			"id": "6XausnR17z0NwuU_cE1zC",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 6.269506981971325,
			"x": 900.8497457974471,
			"y": -26.009145324174234,
			"strokeColor": "#0a11d3",
			"backgroundColor": "transparent",
			"width": 88.30808627974527,
			"height": 9.797916664247975,
			"seed": 548508142,
			"groupIds": [
				"ojiou7z4HO6gw9satYPjX",
				"Tr3wpofl9AgJsscJB4PLK"
			],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1674826612091,
			"link": null,
			"locked": false,
			"startBinding": null,
			"endBinding": null,
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": null,
			"points": [
				[
					0,
					0
				],
				[
					2.326538897826852,
					3.9056133261361587
				],
				[
					12.359939318521995,
					7.182387014695761
				],
				[
					25.710950037209347,
					9.166781347006062
				],
				[
					46.6269757640547,
					9.347610268342288
				],
				[
					71.03526003420632,
					8.084235941711592
				],
				[
					85.2899738827162,
					3.4881086608341767
				],
				[
					88.30808627974527,
					-0.45030639590568633
				]
			]
		},
		{
			"type": "line",
			"version": 2298,
			"versionNonce": 1554679282,
			"isDeleted": false,
			"id": "DN5n4GefGZMVikX4FaSec",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 6.269506981971325,
			"x": 899.3108476757699,
			"y": -59.07212653780331,
			"strokeColor": "#0a11d3",
			"backgroundColor": "transparent",
			"width": 88.30808627974527,
			"height": 9.797916664247975,
			"seed": 710626290,
			"groupIds": [
				"ojiou7z4HO6gw9satYPjX",
				"Tr3wpofl9AgJsscJB4PLK"
			],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1674826612091,
			"link": null,
			"locked": false,
			"startBinding": null,
			"endBinding": null,
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": null,
			"points": [
				[
					0,
					0
				],
				[
					2.326538897826852,
					3.9056133261361587
				],
				[
					12.359939318521995,
					7.182387014695761
				],
				[
					25.710950037209347,
					9.166781347006062
				],
				[
					46.6269757640547,
					9.347610268342288
				],
				[
					71.03526003420632,
					8.084235941711592
				],
				[
					85.2899738827162,
					3.4881086608341767
				],
				[
					88.30808627974527,
					-0.45030639590568633
				]
			]
		},
		{
			"type": "ellipse",
			"version": 5317,
			"versionNonce": 2107542062,
			"isDeleted": false,
			"id": "vYXU_hRVYE5cNiKKPtbJu",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 6.269506981971325,
			"x": 897.6989185822839,
			"y": -99.28592503216511,
			"strokeColor": "#0a11d3",
			"backgroundColor": "#fff",
			"width": 87.65074610854188,
			"height": 17.72670397681366,
			"seed": 885335086,
			"groupIds": [
				"ojiou7z4HO6gw9satYPjX",
				"Tr3wpofl9AgJsscJB4PLK"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1674826612091,
			"link": null,
			"locked": false
		},
		{
			"type": "ellipse",
			"version": 683,
			"versionNonce": 420018098,
			"isDeleted": false,
			"id": "iU6R_38j0XH8lcENqEKQl",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 6.269506981971325,
			"x": 969.5157215338062,
			"y": -75.17876150982327,
			"strokeColor": "#0a11d3",
			"backgroundColor": "#fff",
			"width": 12.846057046979809,
			"height": 13.941904362416096,
			"seed": 1085341106,
			"groupIds": [
				"ojiou7z4HO6gw9satYPjX",
				"Tr3wpofl9AgJsscJB4PLK"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1674826612091,
			"link": null,
			"locked": false
		},
		{
			"type": "ellipse",
			"version": 732,
			"versionNonce": 300487790,
			"isDeleted": false,
			"id": "M7dUCzRSrTtS2kKk0MdKM",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 6.269506981971325,
			"x": 969.9342987971806,
			"y": -44.57917041336457,
			"strokeColor": "#0a11d3",
			"backgroundColor": "#fff",
			"width": 12.846057046979809,
			"height": 13.941904362416096,
			"seed": 489451118,
			"groupIds": [
				"ojiou7z4HO6gw9satYPjX",
				"Tr3wpofl9AgJsscJB4PLK"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1674826612091,
			"link": null,
			"locked": false
		},
		{
			"type": "ellipse",
			"version": 786,
			"versionNonce": 741552498,
			"isDeleted": false,
			"id": "6z1-izVuAC486FzNDkBrS",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 6.269506981971325,
			"x": 970.3892366658048,
			"y": -11.32148068143794,
			"strokeColor": "#0a11d3",
			"backgroundColor": "#fff",
			"width": 12.846057046979809,
			"height": 13.941904362416096,
			"seed": 1827305330,
			"groupIds": [
				"ojiou7z4HO6gw9satYPjX",
				"Tr3wpofl9AgJsscJB4PLK"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1674826612091,
			"link": null,
			"locked": false
		},
		{
			"type": "line",
			"version": 258,
			"versionNonce": 1223186094,
			"isDeleted": false,
			"id": "xoanL3HQ",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 980.3994140625,
			"y": -29.663352272726016,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 0,
			"height": 0,
			"seed": 82182,
			"groupIds": [
				"Tr3wpofl9AgJsscJB4PLK"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1674826612092,
			"link": null,
			"locked": false,
			"startBinding": null,
			"endBinding": null,
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": null,
			"points": [
				[
					0,
					0
				],
				[
					0,
					0
				]
			]
		},
		{
			"type": "arrow",
			"version": 730,
			"versionNonce": 1500525362,
			"isDeleted": false,
			"id": "70mOHcCGFsU1vVmvQQuOA",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 714.4168401117247,
			"y": -32.93493879649198,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 181.93890678625462,
			"height": 6.521166028017419,
			"seed": 1000716590,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1674826612092,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "MhhBm4AauIEsimdWXQpCy",
				"focus": 0.16236950850334436,
				"gap": 19.826996361724696
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
					181.93890678625462,
					-6.521166028017419
				]
			]
		},
		{
			"type": "rectangle",
			"version": 162,
			"versionNonce": 960358514,
			"isDeleted": false,
			"id": "U8EFd9ZY-jUhsRaRAqHo_",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": -479.8417267141764,
			"y": 186.6755588074996,
			"strokeColor": "#000000",
			"backgroundColor": "#ced4da",
			"width": 417,
			"height": 208,
			"seed": 652563634,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"type": "text",
					"id": "Bqt2q056"
				}
			],
			"updated": 1674826612092,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 292,
			"versionNonce": 55199442,
			"isDeleted": false,
			"id": "Bqt2q056",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": -474.8417267141764,
			"y": 230.6755588074996,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 402,
			"height": 120,
			"seed": 1853539118,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1675249472765,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 3,
			"text": "分析MyBatis优势:\n1.数据库的连接/连接池,只需配置即可\n2.程序是以OOP方式来操作DB\n3.SQL语句是可以写在xml文件,实现了解耦\n4.MyBatis可以对DB操作进行优化,提高效率",
			"rawText": "分析MyBatis优势:\n1.数据库的连接/连接池,只需配置即可\n2.程序是以OOP方式来操作DB\n3.SQL语句是可以写在xml文件,实现了解耦\n4.MyBatis可以对DB操作进行优化,提高效率",
			"baseline": 115,
			"textAlign": "left",
			"verticalAlign": "middle",
			"containerId": "U8EFd9ZY-jUhsRaRAqHo_",
			"originalText": "分析MyBatis优势:\n1.数据库的连接/连接池,只需配置即可\n2.程序是以OOP方式来操作DB\n3.SQL语句是可以写在xml文件,实现了解耦\n4.MyBatis可以对DB操作进行优化,提高效率"
		},
		{
			"type": "text",
			"version": 69,
			"versionNonce": 407388270,
			"isDeleted": false,
			"id": "X71DMtf5",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 896.615581789977,
			"y": 21.740585569088807,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 103,
			"height": 28,
			"seed": 1176207790,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1674826618770,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 3,
			"text": "Monster表",
			"rawText": "Monster表",
			"baseline": 21,
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "Monster表"
		},
		{
			"type": "text",
			"version": 919,
			"versionNonce": 476435794,
			"isDeleted": true,
			"id": "JEofweRY",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 448.0786587515812,
			"y": 370.16490023023863,
			"strokeColor": "#000000",
			"backgroundColor": "white",
			"width": 46,
			"height": 25,
			"seed": 1652541262,
			"groupIds": [
				"PBicbmEEFiYNBBkgNEEdJ"
			],
			"roundness": null,
			"boundElements": null,
			"updated": 1675249491690,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 1,
			"text": "User",
			"rawText": "",
			"baseline": 18,
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "User"
		},
		{
			"type": "line",
			"version": 1228,
			"versionNonce": 629825742,
			"isDeleted": true,
			"id": "15ue4KMPsAtd1zQjxMQpz",
			"fillStyle": "cross-hatch",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 445.2914381030738,
			"y": 360.5119742202599,
			"strokeColor": "#000000",
			"backgroundColor": "#ced4da",
			"width": 49.26942071813747,
			"height": 43.87300421060919,
			"seed": 1539153042,
			"groupIds": [
				"-R0rXtEO-nUqflk-LobE7",
				"PBicbmEEFiYNBBkgNEEdJ"
			],
			"roundness": {
				"type": 2
			},
			"boundElements": null,
			"updated": 1675249491690,
			"link": null,
			"locked": false,
			"startBinding": null,
			"endBinding": null,
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": null,
			"points": [
				[
					0,
					0
				],
				[
					5.518175120431392,
					-29.072472669680792
				],
				[
					23.649321944705985,
					-43.87300421060919
				],
				[
					41.780468768980576,
					-32.244015142736835
				],
				[
					49.26942071813747,
					-3.0188078795298896
				],
				[
					0,
					0
				]
			]
		},
		{
			"type": "ellipse",
			"version": 965,
			"versionNonce": 392387346,
			"isDeleted": true,
			"id": "_RD-eqS6KieBLAgo1P7T-",
			"fillStyle": "cross-hatch",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 457.7453089393895,
			"y": 293.82092643440353,
			"strokeColor": "#000000",
			"backgroundColor": "#ced4da",
			"width": 25.225943407686408,
			"height": 22.072700481725683,
			"seed": 2112204174,
			"groupIds": [
				"-R0rXtEO-nUqflk-LobE7",
				"PBicbmEEFiYNBBkgNEEdJ"
			],
			"roundness": null,
			"boundElements": null,
			"updated": 1675249491690,
			"link": null,
			"locked": false
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
		"currentItemFontSize": 20,
		"currentItemTextAlign": "left",
		"currentItemStartArrowhead": null,
		"currentItemEndArrowhead": "arrow",
		"scrollX": 585.2058809800109,
		"scrollY": 551.2157920248221,
		"zoom": {
			"value": 0.7000000000000001
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