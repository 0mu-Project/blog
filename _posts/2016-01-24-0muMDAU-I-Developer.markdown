---
layout: post
title:  "0MuMDAU 分散式部落格管理系統開發歷程" 
date: 2016-01-23 11:30:00
post_author: 0mu
---

部落格系統這種東西百家齊放 像是Wordpress, October, Ghost... 

但是總覺得不是我所喜歡使用的，加上自己又會 Python 因此就有了這個東西出現了
此系統的全名是 “0mu Markdown Distributed Automatic-git Upload-sys”

現在的部落格都有一些問題，例如？   
舉個最簡單的例子，很完整的系統會依賴著 MySQL , PHP , MarinaDB 等等的後端以及資料庫系統。    


而比較簡單的使用靜態產生的部落格系統卻沒有一個好用的後臺，導致很難發送文章。   
因此腦子有點洞的我就想說要寫個管理靜態部落格的後臺，跟呆翰討論了一下，   

我們認為新形態的系統應該要可以分散跟使用 git 進行管理。   
所以就算是顛覆了目前後臺的架構，我們使用了 Git / Pypy / SQLite(For User Control) / Flask / ShellScript 完成了大部分的功能，   

您所看見的這個靜態部落格就是使用 github page + 0muMDAU 所進行管理的。   

這篇文章只是拿來記錄一些開發上面有趣的過程。

1月23日 新增 imgur 上傳功能：

<img src="http://i.imgur.com/rNgcipa.png" style="max-width: 100%">

技術上面使用了 [flask-imgur](https://github.com/exaroth/flask-imgur) + ajaxfileupload

這邊有幾點要注意 flask-imgur 的作者做的 site-package，有點問題所以建議直接 fork github的在 py 內 import，  

使用方法如下：



    from muMDAU_app.flask_imgur import Imgur
    app.config['IMGUR_ID'] = setting.imgurkey
    imgur_handler = Imgur(app)
 
    @app.route('/upload/imgur', methods=['GET', 'POST'])
    def imgurupload():
          if request.method == 'POST':
              image = request.files['fileToUpload']
              image_data = imgur_handler.send_image(image)
              return image_data['data']['link']


這邊先在後端新增一個 route ，使用 Post Only ，return upload url
使用 imgur-API 的方式進行連接 （[詳細看這](https://api.imgur.com/)）

前端再用個簡單地 ajaxuploadfile 去接 ([詳細請看105-116行](https://github.com/0mu-Project/0muMDAU-Flask/blob/master/muMDAU_app/templates/panel.html))



最後附上 Code ，請喜歡我們的專案的朋友們不吝嗇的給我們 Stars：

<a class="embedly-card" href="https://github.com/0mu-Project/0muMDAU-Flask">0mu-Project/0muMDAU-Flask</a>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>





