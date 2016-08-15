---
layout: post
title:  "Linux-FakeRoot 學習筆記" 
date: 2016-08-16 01:30:10+0800
category: Linux
post_author: 0mu
---
What is fakeroot ? 
--
如名字 Fake (假的) Root ，其實就是提供一個讓你以為你是 root (uid0) 的環境去騙程式，    
其實他的目的就是要提供你一個環境去實作一些該用 Root 卻不能給 Root 權限的事情，    
這樣說有點饒口，我們舉個例子吧，今天在 Arch 之下你要使用 Arch Build System (ABS) 打包 .pkg.tar.xz。
    
正常我們會需要先透過 make 指令進行編譯，編譯後你不能 make install 到本機因為你是要進行打包 ， 
你會需要 make install DESTDIR= 到 pkg 的根目錄 ($pwd) 的 tmp 進行相關的 bin 製作以及建立目錄結構，
然後透過 ABS 進行 PKGBUILD 等等的 maintainer script 生成撰寫，這時候你會發現很尷尬的事情。
    
你會發現你會需要這個包裡面的文件所有者是 root 但是，你又不希望 ABS 動到你的 System Root ，    
這時候你就會不太願意透過 sudo 去進行 文件操作，因此就有了 Fakeroot 這個 Fake Env 了。
    
		
How fakeroot Done it ?
--
先來說說 FakeRoot 的 Kernel 吧 faked 這個 binary ，跟 libfakeroot-sysv.so 這個動態連接函式庫    

#### Faked  -
他是一個 Linux Daemon （其實就是個程式會在後臺不停運作或是等候命令） 大陸稱為守護進程，    
其目的就是管理虛擬的文件所有者(owner)/權限訊息的一隻小程式。    

#### libfakeroot-sysv.so -
他是一個動態連接函式庫提供了以下的function，  
    
getuid() , geteuid() , getguid() , getegid()    
mknod(),chown(),lchown(),fchown()    
chmod(),fchmod(),mkdir(),lstat()    
fstat(),stat(),unlink(),remove(),rmdir(),rename()    
    
看了這些大概能理解這些東西在幹嘛了，其實就是讓 fakeroot -> /usr/lib/libfakeroot/libfakeroot-sysv.so -> faked





