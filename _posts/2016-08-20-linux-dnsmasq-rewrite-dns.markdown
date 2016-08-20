---
layout: post
title:  "Linux-在 Network Manager 環境之下透過 dnsmasq 管理 DNS 記錄" 
date: 2016-08-16 01:30:10+0800
category: Linux
post_author: 0mu
---
## DNSMASQ 管理筆記   
     
起因：不爽用 network manager 的 dns 管理方式，想自架 DNS Server 然後自己 resolve 自己    
環境：Arch Linux + Gnome3 + NetworkManager (<s>fuck u nmcli</s>)    
原因：在玩 docker 時想要自動對應每一台 Container 的連線(<s>問候</s>)方式，因此想用 hostname 管理，發現了容器互通了但是外對內依然不互通    
    
---
    
## 先來說說看我想要怎樣弄
    
一般上網時 nameserver 用 8.8.8.8 或是 hinet dns    
對容器連線時 ns 用 172.99.0.255 這臺綁定IP的 dns container 進行解析    
但是我好懶的動 resolv.conf 或是用 resolvconf.conf 來做 search 動作    
    
		
## 讓我們開始吧    
    
---
    
### Step 1.    Fxck default dns manager in  NM    
我們先把 nm 的 dns 管理功能拔了 要不然你會發現 每次開機你的電腦都會幫你透過 nm 建立 resolv.conf
    
		sudo vim /etc/NetworkManager/NetworkManager.conf    
		
用自己常用的編輯器 將此config 的 dns 由 default 變成 **none**

### Step 2.    Install DNSMASQ & config     
安裝就老樣子沒有技術可言
    
		sudo pacman -S dnsmasq    
		
安裝後修改以下的 Config      
    
		sudo vim /etc/dnsmasq.conf   
		
其實你可以自己研究看看 dnsmasq.conf 裡面的 example 很完善 而且 man dnsmasq 也很棒    

		no-resolv <-不繼承系統本身的 resolv.conf
    no-poll <-不檢測 resolv.conf的變更
    server=8.8.8.8
    server=8.8.4.4
		server=/docker/172.99.0.255 <- 把 .docker 的域名一律導向用 這臺 dns 去解析
    
這樣其實就完成了，只要把 dnsmasq enable / start 就好    

		
### Step 3. resolvconf.conf 將resolver 設定為 127.0.0.1    
    
最後透過 openresolv 提供的 resolvconf 將本地的 nameserver 設定為本機    

    sudo vim /etc/resolvconf.conf 
		
檢查name_servers 有沒有東西有就全刪掉，改成

    name_servers=127.0.0.1


### Done , enjoy it><





