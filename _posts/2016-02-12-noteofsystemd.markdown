---
layout: post
title:  "解決 Systemd Suspend 因為 USB 3.0 造成的休眠喚醒" 
date: 2016-02-12 11:31:50 +0800
post_author: 0mu
---
<!--
	請依照以下格式填寫上面的發文標注
	layout: post
	title:  "你要的標題"
	date:   20xx-xx-xx xx:xx:xx +0800
	post_author: 作者
-->
<!-- 內文  -->
		
前情提要，我是個 ArchLinux 的用戶，最近發現我的筆電蓋上螢幕後無法正常休眠，
	
就是進入休眠然後馬上醒來，心想很像是被某個裝置喚醒，開始追查，
	
偶然在 ArchForums看到了這篇文章，想說 bingo 就是你！
		
結論就是被 U3 給喚醒了...，解決方法給他一個 Service 讓他把 wakeup 關閉 U3...
	
 
 
 https://bbs.archlinux.org/viewtopic.php?pid=1575617#p1575617

 
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
