<p align="center">
  <a href="http://moca-tech.net/">
    <img src="../logo.png" alt="Bootstrap logo" width=72 height=72>
  </a>

  <h3 align="center">Moca Tech</h3>

  <p align="center">
    Mobile Media Revolution.
    <br>
    <a href="github.com/moca-tech/Specification-for-AFP" target="_blank">zh</a>
    ·
    en
    <br>
    <a href="http://www.moca-tech.net" target="_blank"><strong>More »</strong></a>
    <br>
    <br>
    <a href="http://www.adstailor.com" target="_blank">Adstailor</a>
    ·
    <a href="mailto:business@moca-tech.net">Business@moca-tech.net</a>
  </p>






##  Contents

- [Introduction](#Introduction)
- [Start](#Start)
- [API](#API)
- [Sample](#Sample)
- [Appendix](#Appendix)



## Introduction

#### 1.&nbsp;Purpose

Adstailor for Publishers(AFP) is managed Ad resource platform which is provided by Moca Technology, AFP helps media or publisher to manage their traffic conveniently. AFP also helps you to achieve the best Ad revenue via DSP (Demand-Side Platform), Adstailor AdExchange to increase the fill-rate. This guide is for media or publisher to integrate AFP.

#### 2.&nbsp;References Docuemnt

1. [IAB VAST](https://www.iab.com/guidelines/digital-video-ad-serving-template-vast/)
2. [OpenRTB](https://www.iab.com/wp-content/uploads/2015/05/OpenRTB_API_Specification_Version_2_3_1.pdf)
3. [OpenRTB Native Ad](https://github.com/openrtb/OpenRTB/blob/76a6d25c74a0cc8f15b119549257856acfc62246/OpenRTB-Native-Ads-Specification-1_0-Final.pdf)




## Start

#### 1.&nbsp;Conception

Media / Publisher: AFP has unique identification “publisher_key” which is indicated whether advertising request is legitimate. “publisher_key” can be provided by our Business Department. 
Inventory: Inventory means the Publisher own App or Website.
Placement: AFP means the smallest advertising unit which is one of placement in App or Website.


#### 2.&nbsp;Apply as an Adstailor partner

Contact us through visit moca-tech.net or email us ( business@moca-tech.net ). Our business manager will help you open an account and integrate our AFP immediately.


## API

#### 1.&nbsp;Pre-Load Advertising Interface

The client will get one or more valid placement of advertising resource and pre-load the creative when requesting an ad through the interface. The client should complete display the creative within the deadline and report the impression, click and track event according to the concrete demand to complete the counting of the advertisement, so as to gain revenue.

#### 2.&nbsp;Interface Information

Request Method: GET
Request URI: http://x.adstailor.com/pfad/[publisher_key]/[inventory_id]
[publisher_key]: You need to replace this parameter to your publisher_key.
[inventory_id]: Replace it to ad inventory id.
Encoding: UTF-8
Response Format: JSON
Content-Type: application/json


#### 3.&nbsp;Request Parameter

AFP get the user’s device information via User-Agent by default, if media or publisher is not displayed the ad in the webview, you have to upload the user’s device information as targeting parameter. Even though the media or publisher display the creative in the webview, you also can upload the request parameter to modify the device information.

| Field         | Description                                                     | Optional      |
| -------------- | ------------------------------------------------------------ | ----------- |
| plcmt          | Ad Placement, multiple placements are separated by half angle comma. Inventory and placement need to be added and set up in AFP。 | optional    |
| w              | Width of the placement, it will take effect only one time and cover original placement width and height when w/h has value. When the ad type is native, w/h value only modify the main picture’s width and height, and icon/ logo width-height ratio unchanged. | optional    |
| h              | Height of the placement, it will take effect only one time and cover original placement width and height when w/h has value. When the ad type is native, w/h value only modify the main picture’s width and height, and icon/ logo width-height ratio unchanged. | optional    |
| ua             | User-Agent                                             | recommended |
| devicetype     | User device type, enumeration type. Please refer appendix A.                             | recommended |
| make           | Device manufacturer.                                             | recommended |
| model          | Device model.                                               | recommended |
| os             | Device operation system.                                           | recommended |
| osv            | Device operation system version.                                     | recommended |
| hwv            | Hardware version of the device.                                           | recommended |
| language       | Device language using IOS-639-1-alpha-2.                        | recommended |
| carrier        | Device carrier or ISP.                                             | recommended |
| connectiontype | Network connection type. Refer to appendix B.                      | recommended |
| ip             | IPv4 address closest to device, if ip is null, please try to get it via x-forwarded-for, remote_addr.       | recommended |
| lat            | Latitude from -90.0 to +90.0.                              | recommended |
| lon            | Longitude from -180.0 to +180.0.                               | recommended |
| yob            | Year of birth as a 4-digit integer.                                             | optional    |
| gender         | Gender, where “M” = male, “F” = female, “O” = known to be other (i.e., omitted is unknown). | optional    |
| keywords       | Comma separated list of keywords, interests, or intent.                         | optional    |
| gaid           | Google Advertising ID.                            | recommended |
| idfa           | Identifier for Advertising (IDFA) for iOS.                           | recommended |
| dpid           | Platform device ID. (i.e.,Android ID)                              | recommended |
| did            | Hardware device ID. (i.e., IMEI)                                    | optional    |
| mac            | MAC address of the device                                                      | optional    |

#### 4.&nbsp;Response

4.1 Response

| Field                                                  | Type                                                     | Description                                                     |
| ------------------------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| status &emsp;&emsp;&emsp;&emsp;&emsp;&emsp; | string&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp; | Ok is interface response correctly, Fail is something wrong.   &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp; |
| message                                                 | string                                                       | When interface response fail, message carries the error information.                            |
| data                                                    | plcmt object array name plcmts                                |                                                              |

4.2 plcmt Object Information

| Field                                                       | Type                                                     | Description           |
| ----------------------------------------------------------- | ------------------------------------------------------------ | ------------------ |
| id &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp; | integer&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&nbsp; | Placement ID&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp; |
| ads                                                          | Ad object array                                                   |                    |

4.3 ad Object

| Field                                                       | Type                                                     | Description        |
| ------------------------------------------------------------ | ------------------------------------------------------------ | --------------- |
| id &emsp;&emsp;&emsp;&emsp;&emsp;&emsp; | String | ADID |
| pay_for                                                      | String                                                       | Settlement (CPM/CPC) |
| currency                                                     | String                                                       | Settlement currency        |
| bid                                                          | float number                                                       | Settlement price        |
| expires                                                      | integer                                                         | expire timestamp      |
| adm                                                          | When the ad type is video, it is VAST xml. For details, please refer to [IAB VAST Protocol](https://www.iab.com/guidelines/digital-video-ad-serving-template-vast/); When the ad type is banner, it is html tag. When the ad type is native, it is native object. For details, please refer to [IAB OpenRTB Native Ad v1.0 Protocol](https://github.com/openrtb/OpenRTB/blob/76a6d25c74a0cc8f15b119549257856acfc62246/OpenRTB-Native-Ads-Specification-1_0-Final.pdf)。 | Ad Creative        |



## Sample

#### 1.&nbsp;Request Sample

Request URI: http://x.adstailor.com/pfad/[publisher_token]/[inventory_id]?plcmt=101,102&w=720&h=1280

#### 2.&nbsp;Banner Response Sample

```json
{
    "status": "Ok",
    "data": {
        "plcmts": [
            {
                "id": 101,
                "ads": [
                    {
                        "id": "2263",
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

#### 3.&nbsp;Native Response Sample

```json
{
    "status": "Ok",
    "data": {
        "plcmts": [
            {
                "id": 144,
                "ads": [
                    {
                        "id": "1908",
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

#### 4.&nbsp;Video Response Sample

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
                        "id": "2798",
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




## Appendix

#### 1.&nbsp;Device Type Explanation

| Valid Value | Description              | Remarks         |
| ------ | ----------------- | ------------ |
| 1      &emsp;&emsp;&emsp;| Mobile / Tablet   | Priority use 4、5&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;|
| 2      | Personal Computer |              |
| 3      | Connected TV      |              |
| 4      | Phone             |              |
| 5      | Tablet            |              |
| 6      | Connected Device  |              |
| 7      | Set Top Box       |              |

#### 2.&nbsp;Connection Type Description

| Valid Value                                                       | Description                                  |
| ------------------------------------------------------------ | ------------------------------------- |
| 0 &emsp;&emsp;&emsp;| Unknown&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;|
| 1                                                            | Ethernet                              |
| 2                                                            | WiFi                                  |
| 3                                                            | Cellular Network - Unknown Generation |
| 4                                                            | Cellular Network - 2G                 |
| 5                                                            | Cellular Network - 3G                 |
| 6                                                            | Cellular Network - 4G                 |
