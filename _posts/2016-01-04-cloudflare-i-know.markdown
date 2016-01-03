---
layout: post
title:  "" 
date: 2016-01-04 01:28:00
---
<!--
請依照以下格式填寫上面的發文標注
layout: post
title:  "你要的標題"
date:   20xx-xx-xx xx:xx:xx
-->
<!-- 內文  -->
CloudFlare提供了網站的CDN服務，不少證據也指出他們使用nginx的網頁伺服器。那麼他們的nginx是怎麼設定的？

起先，我以為他們是這樣做的（這也是我想讓一個網址用瀏覽器開會出現Google Sites頁面、用MINECRAFT卻可以連到我的伺服器的解法）：

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
            proxy_set_header host ;
            proxy_pass http://163.17.2.2;
        }
    }

也就是說，每個使用CloudFlare CDN服務的主機，在nginx裡面都是一個server。不過後來我發現不對，因為nginx設定檔有這麼快的部屬到每個節點嗎？！仔細想想才想到，其實nginx的設定檔還有這種寫法：

    server {
        listen my-ip:80 default_server;
        resolver 8.8.8.8;
        
        location / {
            proxy_set_header Host $http_host
            proxy_pass       $scheme://$host$request_uri;
        }
    }


沒錯啊，nginx設定檔確實可以這樣寫沒錯啊，這樣根本不用等nginx部屬到全球的節點，只要修改DNS紀錄就可以讓CDN接管了呢！！

那麼，我們來驗證看看吧，首先我們得找出一個CloudFlare代管的節點IP是多少：

    