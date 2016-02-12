---
layout: post
title:  "解決 SystemD Suspend 因為 USB 3.0 造成的睡眠喚醒" 
date: 2016-02-12 11:31:50 +0800
category: Linux
post_author: 0mu
---
前情提要，零夢我是個 ArchLinux 的用家，最近發現我的筆電蓋上螢幕後無法正常睡眠，
	
就是進入睡眠然後馬上醒來，心想很像是電源管理有問題，開始追查，
	
一開始完全沒有任何邏輯找錯誤，心想會不會是 acpid 引起的，但是，

等等 ArchLinux 已經完全拋棄  System V  的架構了，所以  acpid 應該已經被 systemd-logind 取代了，

然後突然恍然大悟，這應該是有東西造成系統喚醒才對，然後就習慣的利用 
	
	journalctl -b -u systemd-logind 

查詢系統，然後就看到了這些 訊息 : 
	
	2月 11 12:01:26 reimu-sx2 systemd-logind[307]: Lid closed.
	2月 11 12:01:26 reimu-sx2 systemd-logind[307]: Suspending...
	
所以系統的確有進入睡眠，很明顯就是被喚醒無誤！
	
然後就很偶然在 ArchForums看到了這篇文章，bingo 就是你！

 [https://bbs.archlinux.org/viewtopic.php?pid=1575617#p1575617](https://bbs.archlinux.org/viewtopic.php?pid=1575617#p1575617)
		
結論就是被 U3 給喚醒了...，解決方法給他一個 Service 讓他把 wakeup 關閉 U3...
 
 cd /etc/systemd/system/

 sudo vim disable-USB-wakeup.service
	
新增以下的內容
	
	[Unit]
	Description=Disable USB wakeup triggers in /proc/acpi/wakeup
	[Service]
	Type=oneshot
	ExecStart=/bin/sh -c "echo EHC1 > /proc/acpi/wakeup; echo EHC2 > /proc/acpi/wakeup; echo XHC > /proc/acpi/wakeup; echo GLAN > /proc/acpi/wakeup"
	ExecStop=/bin/sh -c "echo EHC1 > /proc/acpi/wakeup; echo EHC2 > /proc/acpi/wakeup; echo XHC > /proc/acpi/wakeup; echo GLAN > /proc/acpi/wakeup"
	RemainAfterExit=yes
	[Install]
	WantedBy=multi-user.target

然後再利用 systemctl 去打開個這個 service

