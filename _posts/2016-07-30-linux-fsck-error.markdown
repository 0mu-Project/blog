---
layout: post
title:  "Linux-FSCK錯誤-導致系統無法開機" 
date: 2016-07-30 10:25:10+0800
post_author:零夢
category: Linux
---
<!--
	請依照以下格式填寫上面的發文標注
	layout: post
	title:  "你要的標題"
	date:   20xx-xx-xx xx:xx:xx +0800
	post_author: 作者
-->
<!-- 內文  -->

What is fsck !?
> **f**ile **s**ystem **c**onsistency chec**k**
> 簡單說:檔案系統檢測

What is fsck error !?
> 你的fs有點問題導致 fstab 開機時炸了
> Linux 會跳出類似 mount error 

Why fsck error !?
> 你不小心讓 fs 髒髒的
> 你手殘動了 /etc/fstab 裡面的 uuid 
> 你的硬碟真的要炸了..

How fix fsck error !?
> 如果真的是硬碟炸了就任命吧
> fstab 動到 .. 還原吧
> fsck -y /dev/sd?


