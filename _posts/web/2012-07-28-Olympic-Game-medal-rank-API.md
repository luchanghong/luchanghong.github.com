--- 
wordpress_id: 475
wordpress_url: http://luchanghong.com/rosemary/?p=475
date: 2012-07-28 21:30:10 +08:00
layout: post
title: !binary |
  5aWl6L+Q5Lya5aWW54mM5o6S6KGM5qacQVBJ
category: web
tags: [api]
description: 奥运会开始了，咱也来凑凑热闹，看一下新浪提供的奥运金牌榜的 API 。
---
奥运会开始了，关注一下，新浪官网上的奖牌排行榜看了一下，找到一个JS，在JS里面找到了一个API：

<a title="奥运会奖牌排行榜JS" href="http://match.2012.sina.com.cn/api/Gl/medalsOrganization?app_key=fb647ad03c03ab15e9cb9bed461cbe7d&amp;limit=4">http://match.2012.sina.com.cn/api/Gl/medalsOrganization?app_key=fb647ad03c03ab15e9cb9bed461cbe7d&amp;limit=4</a>
把JSON字符串格式化一下（<a href="http://jsonformat.com/">http://jsonformat.com/</a>），输出结果：
<pre class="prettyprint">
{ "result" : { "data" : { "medalLines" : [ { "M_Bronze" : "0",
                "M_Gold" : "0",
                "M_Silver" : "0",
                "M_Total" : "0",
                "OrgFlag" : "http://www.sinaimg.cn/ty/08ay/data/logo/new/CHN.jpg",
                "OrgTrans" : "中国",
                "OrgUrl" : "http://match.2012.sina.com.cn/medals/country/CHN",
                "Organisation" : "CHN",
                "Rank" : "1",
                "TOT_Bronze" : "1",
                "TOT_Gold" : "1",
                "TOT_Silver" : "0",
                "TOT_Total" : "2",
                "W_Bronze" : "1",
                "W_Gold" : "1",
                "W_Silver" : "0",
                "W_Total" : "2",
                "X_Bronze" : "0",
                "X_Gold" : "0",
                "X_Silver" : "0",
                "X_Total" : "0"
              },
              { "M_Bronze" : "0",
                "M_Gold" : "0",
                "M_Silver" : "0",
                "M_Total" : "0",
                "OrgFlag" : "http://www.sinaimg.cn/ty/08ay/data/logo/new/POL.jpg",
                "OrgTrans" : "波兰",
                "OrgUrl" : "http://match.2012.sina.com.cn/medals/country/POL",
                "Organisation" : "POL",
                "Rank" : "2",
                "TOT_Bronze" : "0",
                "TOT_Gold" : "0",
                "TOT_Silver" : "1",
                "TOT_Total" : "1",
                "W_Bronze" : "0",
                "W_Gold" : "0",
                "W_Silver" : "1",
                "W_Total" : "1",
                "X_Bronze" : "0",
                "X_Gold" : "0",
                "X_Silver" : "0",
                "X_Total" : "0"
              },
              { "M_Bronze" : 0,
                "M_Gold" : 0,
                "M_Silver" : 0,
                "M_Total" : 0,
                "OrgFlag" : "http://www.sinaimg.cn/ty/08ay/data/logo/new/USA.jpg",
                "OrgTrans" : "美国",
                "OrgUrl" : "http://match.2012.sina.com.cn/medals/country/USA",
                "Organisation" : "USA",
                "Rank" : 3,
                "TOT_Bronze" : 0,
                "TOT_Gold" : 0,
                "TOT_Silver" : 0,
                "TOT_Total" : 0,
                "W_Bronze" : 0,
                "W_Gold" : 0,
                "W_Silver" : 0,
                "W_Total" : 0,
                "X_Bronze" : 0,
                "X_Gold" : 0,
                "X_Silver" : 0,
                "X_Total" : 0
              },
              { "M_Bronze" : 0,
                "M_Gold" : 0,
                "M_Silver" : 0,
                "M_Total" : 0,
                "OrgFlag" : "http://www.sinaimg.cn/ty/08ay/data/logo/new/RUS.jpg",
                "OrgTrans" : "俄罗斯",
                "OrgUrl" : "http://match.2012.sina.com.cn/medals/country/RUS",
                "Organisation" : "RUS",
                "Rank" : 4,
                "TOT_Bronze" : 0,
                "TOT_Gold" : 0,
                "TOT_Silver" : 0,
                "TOT_Total" : 0,
                "W_Bronze" : 0,
                "W_Gold" : 0,
                "W_Silver" : 0,
                "W_Total" : 0,
                "X_Bronze" : 0,
                "X_Gold" : 0,
                "X_Silver" : 0,
                "X_Total" : 0
              }
            ],
          "summary" : { "BjDate" : "2012-07-28 18:32:01",
              "DateTime" : "2012-07-28 18:32:01",
              "FinishedEvents" : "3",
              "M_Bronze" : "0",
              "M_Gold" : "0",
              "M_Silver" : "0",
              "M_Total" : "0",
              "TOT_Bronze" : "1",
              "TOT_Gold" : "1",
              "TOT_Silver" : "1",
              "TOT_Total" : "3",
              "W_Bronze" : "1",
              "W_Gold" : "1",
              "W_Silver" : "1",
              "W_Total" : "3",
              "X_Bronze" : "0",
              "X_Gold" : "0",
              "X_Silver" : "0",
              "X_Total" : "0"
            }
        },
      "encoding" : "utf-8",
      "language" : "cn",
      "status" : { "code" : 0,
          "msg" : 1
        }
    } }</pre>