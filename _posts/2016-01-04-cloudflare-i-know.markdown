---
layout: post
title:  "CloudFlare簡單研究" 
date: 2016-01-04 01:28:00
---
<!--
請依照以下格式填寫上面的發文標注
layout: post
title:  "你要的標題"
date:   20xx-xx-xx xx:xx:xx
-->
<!-- 內文  -->
## 作者：dd-han

CloudFlare提供了網站的CDN服務，不少證據也指出他們使用nginx的網頁伺服器。那麼他們的服務到底是怎麼運作的呢？

起先，因為我個自己各種把玩過nginx設定，我知道有一種誇張的作法是把nginx當DNS用，於是我以為他們是這樣做的（這也是我想讓一個網址用瀏覽器開會出現Google Sites頁面、用MINECRAFT卻可以連到我的伺服器的解法），也就是把nginx當DNS Server用：

    server {
        server_name minecraft.dd-han.tw;
        
        location / {
            ## 天殺的，我用nginx處理了cname紀錄轉換！！
            proxy_set_header host "minecraft-page.dd-han.tw";
            proxy_pass http://ghs.google.com;
        }
    }
    server {
        server_name website2.example.com;
        
        location / {
            proxy_set_header host "website2.dd-han.tw";
            proxy_pass http://163.17.2.2;
        }
    }

也就是說，每個使用CloudFlare CDN服務的主機，在nginx裡面都是一個server。不過後來我發現不對，因為nginx設定檔有這麼快的部屬到每個節點嗎？！仔細想想才想到，其實nginx的設定檔還有這種寫法：

    ## 把nginx當成普通的http代理伺服器用
    server {
        listen my-ip:80 default_server;
        resolver 8.8.8.8;
        
        location / {
            proxy_set_header Host $http_host
            proxy_pass       $scheme://$host$request_uri;
        }
    }


沒錯啊，nginx設定檔確實可以這樣寫沒錯啊，這樣根本不用等nginx部屬到全球的節點，只要修改DNS紀錄就可以讓CDN接管整個服務了呢！！

讓我們來驗證看看吧，首先我們得找出一個CloudFlare代管的節點IP是多少：

    $ nslookup cloud-test1111.dd-han.tw
    Server:         192.168.1.1
    Address:        192.168.1.1#53

    Non-authoritative answer:
    Name:   cloud-test1111.dd-han.tw
    Address: 104.18.45.37
    Name:   cloud-test1111.dd-han.tw
    Address: 104.18.44.37


這個範例我們找出我們身邊的CloudFlare節點IP是104.18.45.37跟104.18.44.37，接著我們再用curl指令測試看看：

    curl "http://104.18.45.37/" -H "Host: test1111.dd-han.tw"

結果我沒有在CloudFlare中設定test1111.dd-han.tw要經過他們的CDN服務，但是跑了上述的指令，卻出現了test1111.dd-han.tw的資料。因此我才發現我整個想錯了，根本不需要什麼都靠nginx去做啊，有些東西直接用DNS去做就好啦！

今天的研究結論：不用什麼東西都靠Layer 7的http協定做，有時候DNS也可以做不少事情！