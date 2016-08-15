---
layout: post
title:  "Linux-FakeRoot 學習筆記" 
date: 2016-08-16 01:30:10+0800
category: Linux
post_author: 0mu
---
## What is fakeroot ?     
    
如名字 Fake (假的) Root ，其實就是提供一個讓你以為你是 root (uid0) 的環境去騙程式，    
其實他的目的就是要提供你一個環境去實作一些該用 Root 卻不能給 Root 權限的事情，    
這樣說有點饒口，我們舉個例子吧，今天在 Arch 之下你要使用 Arch Build System (ABS) 打包 .pkg.tar.xz。
    
正常我們會需要先透過 make 指令進行編譯，編譯後你不能 make install 到本機因為你是要進行打包 ， 
你會需要 make install DESTDIR= 到 pkg 的根目錄 ($pwd) 的 tmp 進行相關的 bin 製作以及建立目錄結構，
然後透過 ABS 進行 PKGBUILD 等等的 maintainer script 生成撰寫，這時候你會發現很尷尬的事情。
    
你會發現你會需要這個包裡面的文件所有者是 root 但是，你又不希望 ABS 動到你的 System Root ，    
這時候你就會不太願意透過 sudo 去進行 文件操作，因此就有了 Fakeroot 這個 Fake Env 了。    
    
		
## How fakeroot Done it ?
--
先來說說 FakeRoot 的 Kernel 吧 faked 這個 binary ，跟 libfakeroot-sysv.so 這個動態連接函式庫    

### Faked  -
他是一個 Linux Daemon （其實就是個程式會在後臺不停運作或是等候命令） 大陸稱為守護進程，    
其目的就是管理虛擬的文件所有者(owner)/權限訊息的一隻小程式。    

### libfakeroot-sysv.so -
他會在兩個主要的位置 /usr/lib/libfakeroot-sysv.so /usr/lib/libfakeroot/libfakeroot-sysv.so    
    
> **/usr/lib/libfakeroot/libfakeroot-sysv.so** - 是一個動態連接函式庫提供了以下的function：
    
    getuid() , geteuid() , getguid() , getegid()    
    mknod(),chown(),lchown(),fchown()    
    chmod(),fchmod(),mkdir(),lstat()    
    fstat(),stat(),unlink(),remove(),rmdir(),rename()    

這些 function 會跟 對 faked 進行命令另 faked 進行虛擬檔案操作。    
     
 > **/usr/lib/libfakeroot-sysv.so** - 是一個 dummy 函式庫，原因是為了 suid 這個動作而生    
    
在使用 fakeroot 運作一般的程式時，fakeroot 會產生幾個環境變數如下：    
    
    FAKEROOTKEY- 基本上就是 Fakeroot 跟 faked 的 authkey    
    LD_LIBRARY_PATH - 存放著 libfakeroot-sysv.so 存在的目錄位置 (/usr/lib/libfakeroot)    
    LD_PRELOAD - lib 本身 libfakeroot-sysv.so    
		
然後當我們在使用 suid 這個動作時會自動忽略 LD_LIBRARY_PATH 這個變數，    
導致 fakeroot  去直接尋找位於 /usr/lib/ 底下的 preload，如果沒有這個 lib 就會噴出錯誤，    
因此fakeroot 特別在此放入一個 dummy 函式庫，此時 suid 這個程式就會不經過 faked 正常執行。    
      
    
## 完整流程 

看了這些大概能理解這些東西在幹嘛了，其實 fakeroot 本身是一隻 shellscript 他是這樣運作的：
    
fakeroot -> 打開 faked 取得 fakerootkey -> 設定三個環境變數 FAKEROOTKEY / LD_LIBRARY_PATH  / LD_PRELOAD -> 執行命令    








