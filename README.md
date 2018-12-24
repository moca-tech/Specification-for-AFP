<p align="center">
  <a href="http://moca-tech.net/">
    <img src="./logo.png" alt="Bootstrap logo" width=72 height=72>
  </a>

  <h3 align="center">Moca Tech</h3>

  <p align="center">
    Mobile Media Revolution.
    <br>
    <a href="http://www.moca-tech.net" target="_blank"><strong>More »</strong></a>
    <br>
    <br>
    <a href="http://www.adstailor.com" target="_blank">Adstailor</a>
    ·
    <a href="mailto:business@moca-tech.net">Business@moca-tech.net</a>
  </p>






## 目录

- [前言](#前言)
- [开始](#开始)
- [接口定义](#接口定义)
- [接口示例](#接口示例)
- [附录](#附录)



## 前言

#### 1.&nbsp;目的

Adstailor for Publishers(简称AFP), 是Moca Technology提供的品牌广告资源管理平台，帮助媒体 / 流量方便捷管理广告资源,通过结合品牌广告DSP, Adstailor AdExchange, 提高广告位填充率, 实现广告位收益最大化。
该文档旨在帮助媒体 / 流量方对接AFP。

#### 2.&nbsp;参考协议

1. [IAB VAST](https://www.iab.com/guidelines/digital-video-ad-serving-template-vast/)
2. [OpenRTB](https://www.iab.com/wp-content/uploads/2015/05/OpenRTB_API_Specification_Version_2_3_1.pdf)
3. [OpenRTB Native Ad](https://github.com/openrtb/OpenRTB/blob/76a6d25c74a0cc8f15b119549257856acfc62246/OpenRTB-Native-Ads-Specification-1_0-Final.pdf)




## 开始

#### 1.&nbsp;概念

媒体 / 流量方(publisher): AFP唯一识别标识publisher_key, 用于api请求时标识身份。publisher_key在与Moca沟通后通过你的客户经理获取。

广告位库存(inventory):AFP中指publisher所拥有的应用(app)或者站点(Site)。

广告位(Placement): AFP中指广告投放的最小单元，可能是app或者site中的一个广告位。

#### 2.&nbsp;申请成为adstailor流量合作伙伴

访问moca-tech.net或者通过邮件business@moca-tech.net 联系我们,我们的商务经理会在第一时间协助你开通账户。

#### 3.&nbsp;广告投放准备

成为Adstailor流量合作伙伴后，可以通过AFP管理平台(http://admin.adstailor.com )创建广告位库存(inventory)和广告位(placement)。至此已经完成广告投放的准备工作，接下来只要接入AFP广告投放接口就可以实现广告投放和收益预览。AFP平台还提供丰富的广告过滤选项，投放信息报表和收益报表等功能。



## 接口定义

#### 1.&nbsp;预拉取广告接口

当有广告展示请求时，客户端调用预拉取广告接口一次性获得一个或多个广告位(placement)的有效广告资源，预先下载广告素材。客户端须在广告有效展示截止时间内完成广告展示，并根据具体广告投放需求上报展示、点击、追踪事件以完成投放计数，从而获得收益。

#### 2.&nbsp;接口信息

请求方法: GET
请求地址: http://x.adstailor.com/v2/pfad/[publisher_key]/[inventory_id]
说明: 媒体 / 流量方须将[publisher_key]替换为自己的publisher_key, [inventory_id]替换为广告位库存id, 接口编码方式为utf8,响应数据格式为json, 含响应头 Content-Type: application/json 。

#### 3.&nbsp;请求参数

afp平台默认会从用户的User-Agent获取设备信息, 如果媒体 / 流量方的广告位不是以webview方式展示广告则需要上传设备信息等用户targeting参数。即使媒体 / 流量方的广告位是以webview方式展示广告, 也可以上传请求参数，修正设备信息。

| 字段名         | 字段说明                                                     | 必要性      |
| -------------- | ------------------------------------------------------------ | ----------- |
| plcmt          | Ad Placement广告位, 多个广告位用半角逗号","分隔, 缺省表示该广告位库存下的所有广告位。广告位库存和广告位需要预先在AFP后台添加并设置有效。 | optional    |
| w              | 广告位宽度修正值,当w / h都有值的时候生效, 覆盖原广告位宽高,仅此次生效, 当广告位类型为native时, w / h 只覆盖主图片的宽高, icon / logo 宽高比不变。 | optional    |
| h              | 广告位高度修正值,当w / h都有值的时候生效, 覆盖原广告位宽高,仅此次生效, 当广告位类型为native时, w / h 只覆盖主图片的宽高, icon / logo 宽高比不变。 | optional    |
| ua             | 用户的User-Agent                                             | recommended |
| devicetype     | 用户设备类型, 枚举类型, 见附录A                              | recommended |
| make           | 用户设备的制造商                                             | recommended |
| model          | 用户的设备型号                                               | recommended |
| os             | 用户设备的操作系统                                           | recommended |
| osv            | 用户设备的操作系统版本号                                     | recommended |
| hwv            | 用户设备的型号版本                                           | recommended |
| language       | 设备语言, 请使用ISO-639-1-alpha-2标准                        | recommended |
| carrier        | 设备的网络运营商                                             | recommended |
| connectiontype | 设备的网络连接类型, 枚举类型, 参见附录B                      | recommended |
| ip             | 设备ip地址, 缺省时尝试获取x-forwarded-for, remote_addr       | recommended |
| lat            | Latitude 维度, 范围-90.0到+90.0                              | recommended |
| lon            | Longitude 经度, 范围-180到+180                               | recommended |
| yob            | 出生年份, 如1990                                             | optional    |
| gender         | 用户性别, M代表male, F代表female, O代表第三性别, 不确定请缺省 | optional    |
| keywords       | 关键词, 搜索、兴趣爱好、行为等关键词                         | optional    |
| gaid           | Google Advertising ID 针对安卓设备                           | recommended |
| idfa           | Advertising Identifier 针对iOS设备                           | recommended |
| dpid           | Platform device ID 如Android ID                              | recommended |
| did            | Hardware device ID 如IMEI                                    | optional    |
| mac            | mac地址                                                      | optional    |

#### 4.&nbsp;响应信息

4.1 响应对象

| 字段名                                                       | 字段类型                      | 字段说明                          |
| ------------------------------------------------------------ | ----------------------------- | --------------------------------- |
| status &emsp;&emsp;&emsp;&emsp; | 字符串&emsp;&emsp;&emsp;&emsp; | 接口正确响应时为Ok，出错为Fail   &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp; |
| message                                                      | 字符串                        | 当接口出错时，message携带错误信息 |
| data                                                         | plcmt对象数组, 字段名为plcmts |                                   |

4.2 plcmt对象信息

| 字段名                                                       | 字段类型                                                     | 字段说明           |
| :----------------------------------------------------------- | ------------------------------------------------------------ | ------------------ |
|<img width=20%>|<img width=50%>|<img width=30%>|
| id| 整型| placement 广告位ID |
| ads                                                          | ad对象数组                                                   |                    |

4.3 ad对象

| 字段名                                                       | 字段类型                                                     | 字段说明        |
| ------------------------------------------------------------ | ------------------------------------------------------------ | --------------- |
| id &emsp;&emsp;&emsp;&emsp;&emsp;&emsp; | 整数&emsp; | 广告ID |
| pay_for                                                      | 字符串                                                       | 结算方式CPM/CPC |
| currency                                                     | 字符串                                                       | 结算货币        |
| bid                                                          | 浮点数                                                       | 结算价格        |
| expires                                                      | 整数                                                         | 过期时间戳      |
| adm                                                          | 当广告类型为video时为VAST xml, 详情请参考 [IAB VAST协议](https://www.iab.com/guidelines/digital-video-ad-serving-template-vast/); 当广告类型为banner时为html TAG; 当广告类型为native时为native对象, 详情请参考[IAB OpenRTB Native Ad v1.0协议](https://github.com/openrtb/OpenRTB/blob/76a6d25c74a0cc8f15b119549257856acfc62246/OpenRTB-Native-Ads-Specification-1_0-Final.pdf)。 | 广告素材        |



## 接口示例

#### 1.&nbsp;请求示例

请求地址: http://x.adstailor.com/v2/pfad/[publisher_token]/[inventory_id]?plcmt=101,102&w=720&h=1280

#### 2.&nbsp;banner响应示例

```json
{
    "status": "Ok",
    "data": {
        "plcmts": [
            {
                "id": 101,
                "ads": [
                    {
                        "id": 2263,
                        "pay_for": "cpm",
                        "currency": "USD",
                        "bid": 1.2,
                        "expires": 1577923140,
                        "adm": "<span onclick=\"i=document.createElement('iframe');i.width=0;i.scrolling='no';i.height=0;;i.name='adstc';i.style='position:absolute;top:-15000px;left:-15000px';i.src='http://x.adstailor.com/clk/pfad;pubid=101;invid=113/145;tagid=145;adt=b;=::1;did=;devmk=;devmd=;os=Other;crr=;connt=0;gdr=;agegrp=;apri=1.2;acur=USD/206cc617-44d6-49e8-b8a7-02fede4384a8/adst;206cc617-44d6-49e8-b8a7-02fede4384a8;2;1;adid=2263;cid=1445;crid=2263;exp=1577923140?url=';document.body.append(i)\"><html>sdfsfsdf</html><iframe width=\"0\" scrolling=\"no\" height=\"0\" frameborder=\"0\" src=\"http://x.adstailor.com/pxl/pfad;pubid=101;invid=113/145;tagid=145;adt=b;=::1;did=;devmk=;devmd=;os=Other;crr=;connt=0;gdr=;agegrp=;apri=1.2;acur=USD/206cc617-44d6-49e8-b8a7-02fede4384a8/adst;206cc617-44d6-49e8-b8a7-02fede4384a8;2;1;adid=2263;cid=1445;crid=2263;exp=1577923140?url=\" style=\"position:absolute;top:-15000px;left:-15000px\" vspace=\"0\" hspace=\"0\" marginwidth=\"0\" marginheight=\"0\" allowtransparency=\"true\" name=\"adsti\"></iframe></span>"
                    }
                ]
            }
        ]
    }
}
```

#### 3.&nbsp;native响应示例

```json
{
    "status": "Ok",
    "data": {
        "plcmts": [
            {
                "id": 144,
                "ads": [
                    {
                        "id": 1908,
                        "pay_for": "cpm",
                        "currency": "USD",
                        "bid": 0.6,
                        "expires": 1577946600,
                        "adm": {
                            "native": {
                                "assets": [
                                    {
                                        "id": 1,
                                        "img": {
                                            "url": "http://stor.adstailor.com/m/1001/53356235424efe29e8db.jpg",
                                            "w": 720,
                                            "h": 1280
                                        }
                                    }
                                ],
                                "link": {
                                    "url": "http://xxx.com",
                                    "clicktrackers": [
                                        "http://localhost/clk/pfad;pubid=101;invid=113/144;tagid=144;adt=n;=::1;did=;devmk=;devmd=;os=Other;crr=;connt=0;gdr=;agegrp=;apri=0.6;acur=USD/b81a8f3e-5bbc-405a-ac72-1c2a0912d21a/adst;b81a8f3e-5bbc-405a-ac72-1c2a0912d21a;1;1;adid=1908;cid=1001;crid=1908;exp=1577946600?url="
                                    ]
                                },
                                "imptrackers": [
                                    "http://xxx.com",
                                    "http://localhost/pxl/pfad;pubid=101;invid=113/144;tagid=144;adt=n;=::1;did=;devmk=;devmd=;os=Other;crr=;connt=0;gdr=;agegrp=;apri=0.6;acur=USD/b81a8f3e-5bbc-405a-ac72-1c2a0912d21a/adst;b81a8f3e-5bbc-405a-ac72-1c2a0912d21a;1;1;adid=1908;cid=1001;crid=1908;exp=1577946600?url="
                                ]
                            }
                        }
                    }
                ]
            }
        ]
    }
}
```

#### 4.&nbsp;video响应示例

```json
{
    "status": "Ok",
    "message": "",
    "data": {
        "plcmts": [
            {
                "id": 133,
                "ads": [
                    {
                        "id": 2798,
                        "pay_for": "cpm",
                        "currency": "USD",
                        "bid": 1.2,
                        "expires": 1577923140,
                        "adm": "<VAST version=\"2.0\"><Ad><InLine><AdTitle></AdTitle><Impression><![CDATA[https://ad.doubleclick.net/ddm/trackimp/N34304.3446215SLIM_MILK/B21952205.233919222;dc_trk_aid=431418538;dc_trk_cid=108363155;ord=1545385364;dc_lat=;dc_rdid=;tag_for_child_directed_treatment=;tfua=]]></Impression><Impression id=\"moca-it\"><![CDATA[http://localhost/pxl/pfad;pubid=111;invid=109/133;tagid=133;adt=v;=::1;did=;devmk=;devmd=;os=Other;crr=;connt=0;gdr=;agegrp=;apri=1.2;acur=USD/939646e6-8939-4887-ab0e-666dfc9bbf74/adst;939646e6-8939-4887-ab0e-666dfc9bbf74;2;1;adid=2798;cid=1445;crid=2798;exp=1577923140?url=]]></Impression><Creatives><Creative><Linear><Duration>00:00:18</Duration><TrackingEvents><Tracking event=\"creativeView\"><![CDATA[http://localhost/trk/pfad;pubid=111;invid=109/133;tagid=133;adt=v;=::1;did=;devmk=;devmd=;os=Other;crr=;connt=0;gdr=;agegrp=/939646e6-8939-4887-ab0e-666dfc9bbf74/adst;939646e6-8939-4887-ab0e-666dfc9bbf74;2;1;adid=2798;cid=1445;crid=2798;exp=1577923140?ev=1&url=]]></Tracking><Tracking event=\"start\"><![CDATA[http://localhost/trk/pfad;pubid=111;invid=109/133;tagid=133;adt=v;=::1;did=;devmk=;devmd=;os=Other;crr=;connt=0;gdr=;agegrp=/939646e6-8939-4887-ab0e-666dfc9bbf74/adst;939646e6-8939-4887-ab0e-666dfc9bbf74;2;1;adid=2798;cid=1445;crid=2798;exp=1577923140?ev=2&url=]]></Tracking><Tracking event=\"firstQuartile\"><![CDATA[http://localhost/trk/pfad;pubid=111;invid=109/133;tagid=133;adt=v;=::1;did=;devmk=;devmd=;os=Other;crr=;connt=0;gdr=;agegrp=/939646e6-8939-4887-ab0e-666dfc9bbf74/adst;939646e6-8939-4887-ab0e-666dfc9bbf74;2;1;adid=2798;cid=1445;crid=2798;exp=1577923140?ev=3&url=]]></Tracking><Tracking event=\"midpoint\"><![CDATA[http://localhost/trk/pfad;pubid=111;invid=109/133;tagid=133;adt=v;=::1;did=;devmk=;devmd=;os=Other;crr=;connt=0;gdr=;agegrp=/939646e6-8939-4887-ab0e-666dfc9bbf74/adst;939646e6-8939-4887-ab0e-666dfc9bbf74;2;1;adid=2798;cid=1445;crid=2798;exp=1577923140?ev=4&url=]]></Tracking><Tracking event=\"thirdQuartile\"><![CDATA[http://localhost/trk/pfad;pubid=111;invid=109/133;tagid=133;adt=v;=::1;did=;devmk=;devmd=;os=Other;crr=;connt=0;gdr=;agegrp=/939646e6-8939-4887-ab0e-666dfc9bbf74/adst;939646e6-8939-4887-ab0e-666dfc9bbf74;2;1;adid=2798;cid=1445;crid=2798;exp=1577923140?ev=5&url=]]></Tracking><Tracking event=\"complete\"><![CDATA[http://localhost/trk/pfad;pubid=111;invid=109/133;tagid=133;adt=v;=::1;did=;devmk=;devmd=;os=Other;crr=;connt=0;gdr=;agegrp=/939646e6-8939-4887-ab0e-666dfc9bbf74/adst;939646e6-8939-4887-ab0e-666dfc9bbf74;2;1;adid=2798;cid=1445;crid=2798;exp=1577923140?ev=6&url=]]></Tracking></TrackingEvents><VideoClicks><ClickThrough><![CDATA[https://ad.doubleclick.net/ddm/trackclk/N34304.3446215SLIM_MILK/B21952205.233919222;dc_trk_aid=431418538;dc_trk_cid=108363155;dc_lat=;dc_rdid=;tag_for_child_directed_treatment=;tfua=]]></ClickThrough><ClickTracking id=\"moca-ct\"><![CDATA[http://localhost/clk/pfad;pubid=111;invid=109/133;tagid=133;adt=v;=::1;did=;devmk=;devmd=;os=Other;crr=;connt=0;gdr=;agegrp=;apri=1.2;acur=USD/939646e6-8939-4887-ab0e-666dfc9bbf74/adst;939646e6-8939-4887-ab0e-666dfc9bbf74;2;1;adid=2798;cid=1445;crid=2798;exp=1577923140?url=]]></ClickTracking></VideoClicks><MediaFiles><MediaFile delivery=\"\" type=\"video/mp4\" width=\"0\" height=\"0\"><![CDATA[http://stor.adstailor.com/cr/slimmilk1.mp4]]></MediaFile></MediaFiles></Linear></Creative></Creatives><Description></Description><Survey></Survey></InLine></Ad></VAST>"
                    }
                ]
            }
        ]
    }
}
```




## 附录

#### 1.&nbsp;device type 设备类型说明

| 有效值 | 描述              | 备注         |
| ------ | ----------------- | ------------ |
| 1      &emsp;&emsp;&emsp;| Mobile / Tablet   | 优先采用4、5&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;|
| 2      | Personal Computer |              |
| 3      | Connected TV      |              |
| 4      | Phone             |              |
| 5      | Tablet            |              |
| 6      | Connected Device  |              |
| 7      | Set Top Box       |              |

#### 2.&nbsp;connection type数据连接类型说明

| 有效值                                                       | 描述                                  |
| ------------------------------------------------------------ | ------------------------------------- |
| 0 &emsp;&emsp;&emsp;| Unknown&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;|
| 1                                                            | Ethernet                              |
| 2                                                            | WiFi                                  |
| 3                                                            | Cellular Network - Unknown Generation |
| 4                                                            | Cellular Network - 2G                 |
| 5                                                            | Cellular Network - 3G                 |
| 6                                                            | Cellular Network - 4G                 |