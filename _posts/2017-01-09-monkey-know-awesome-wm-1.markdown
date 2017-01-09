---
layout: post
title:  "連猴子都會用的 Awesome WM 教學 - 壹" 
date: 2016-01-09 06:23:59+0800
post_author: 0mu
category: Linux
---

## 簡介
Linux 中我們常常見到各種 Xwindow (X.org / Wayland )應用程式，不管是各種功能完善的 DE (Desktop Envirement) 還是相較輕量的WM (Window Manager) ，在開放原始碼以及 Linux 社群都有擁護者。    

### DE / WM / XWindow / DM 到底是啥！？    
     
今天我們要介紹的 Awesome 是一個極輕量的 WM 環境，那一定會有人問什麼是 WM 什麼是 DE 什麼是 DM 什麼是 Xwindow ，在此我們就簡單的介紹每一個東西的差異。     

``` 
XWindow System (X視窗系統) :    
提供了 Linux GUI 的一切基本，不管要用 QT / GTK+ 等圖形化函數庫都要基於它，
目前常見的是 X.org 以及相對於 X.org 新的 Wayland 。
    
Desktop Envirement  (DE / 桌面環境):    
用來控制桌面，視窗，目錄，通知，以及相關圖形化相關的界面的環境，提供給使用者一個統一完整的圖形化界面體驗，
目前常見的為 Gnome / KDE / Unity 等等 。    
     
Windows Manager (WM/視窗管理器):    
可以獨立於 DE 存在的，用途是拿來控制視窗的表現，主要有三大類型，浮動/平鋪/動態式，
不過其實每一個 DE 幾乎都有提供一個自己的 WM 來統一風格，    
目前常見的DE 自帶:  Mutter(Gnome) /Kwin (KDE) ，而相對比較有名氣的獨立WM有 Awesome / i3 / WMFS 。       
     
Display Manager (DM/顯示管理器):    
一樣可以是獨立於 DE 的，基本上提供一個登入界面，讓你選擇 DE / WM ，
目前常見的是 GDM / LightDM / KDM / SDDM。
```

### 動態：平鋪 浮動傻傻分不清！？         
     

看完了以上相關的介紹，我相信你大概懂每個東西所執掌的東西不同了，再來我們會以 WM 當成我們的主要討論對象，剛剛上有介紹過 WM 有三種主要的類型分別是 **平鋪式 動態式 浮動式** 在這邊我們簡單介紹這三種的差異。

```
平鋪式 (Tiling): 
對於一般的 CLI 使用者有可能較為熟識基本上就是畫面沒有圖層化的概念，
只有平面顯示或是切割顯示一途，可以依照習慣的 Layout 讓畫面看起來規律，並且常常大量採用快速鍵切換視窗/功能。
目前常見的是 i3 / WMFS / WMFS2 / StumpWM

浮動式 (Stacking/Floating): 
對於一般的圖型化使用者不管是 macOS / Windows 或是 常見的 DE 較為熟識，畫面可以分層可以堆疊可以誰前誰後但是無法規律化整理。
目前常見的是 Mutter(Gnome) / Kwin(KDE) / OpenBox / Fluxbox

動態式(Dynamic):
即為兩種模式都支援可透過 Layout Switch 切換。
目前常見的是 Awesome / xmonad /wmii / dwm
```