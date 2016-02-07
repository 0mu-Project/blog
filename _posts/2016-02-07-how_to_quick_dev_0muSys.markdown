---
layout: post
title: "Step By Step 快速基於0muMDAU/0mu-Sys二次開發" 
date: 2016-02-07 11:01:00 +0800
post_author: 0mu
---

![](http://i.imgur.com/dCf5iWf.png =100x)

看到這個標題我想有很多人應該都嚇壞了，恩，沒錯雖然這是0MuOfficalBlog，

一般來說你在 Offical Blog 會看到的文章，在這個部落格都看不到，

我不會在這裡告訴你我的系統有他媽多完美這些屁話，我只會告訴你如何修改，如何改進，

因此就有了這篇文章的出現，好了廢話不多說進入這次的正題!!!

我們先來探討所謂的分散式概念，如何達成，在這邊我們需要一些概念

---

* 何為分散式？

分散式顧名思義就是分散風險（其實可以想成雲化的概念），將每個 Partment 分散在不同主機，但是能夠集中控管！

* 為何後臺前臺要切離

因為有時後自己養主機真的是累事，但是用代管主機或是 Github Page 沒有動態後臺！

* 最後是實作

最簡便就是 --濫用-- Git 版本控制，不但可以達成分散控管，也可以版本控制，相對傳統 Client-Server 方便許多

---

概念有了那其實可以開始基於 0muMDAU/Server-Sys 進行你自己的系統研發了

先講解技術細節因為本後臺管控系統重度採用了以下的東西，所以建議你要先會這些東西再來跟我談二次開發（至少要會前三項）

1. 最基礎: Python會撰寫 看得懂 ShellScript 看得懂資料庫在幹嘛
2. 次基礎: 知道 Flask 框架以及 他的應用
3. 次次基礎: Javascript 
3. pypy performance coding

是的後台系統我重度的倚賴了 Python Flask 跟 Linux 所以我們只以 Linux Side python 為主。

---

好了 再來我們看看該如何開始二次研發，程式的第一個執行點勢必要先了解清楚，我在此直接給出我的流程運作方式

envri.py => MDAUServer.py => Import muMDAU_app => 去引入各個模組.py


