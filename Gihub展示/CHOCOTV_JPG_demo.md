
# 影音平台圖檔爬蟲
```python
import requests
import time
from bs4 import BeautifulSoup
import os
import re
import urllib.request
import json
from selenium import webdriver
import random
import pandas as pd


driver_path = r"D:\Program Files\chromedriver\chromedriver.exe" # chromedriver path #使用chrome瀏覽器

link_tw = 'https://www.chocotv.com.tw/drama?area=tw'


def get_web_page(link):
    driver = webdriver.Chrome(executable_path = driver_path)
    driver.get(link)#開啟chocotv台劇頁面
    
    close_ad_button = driver.find_element_by_css_selector('.modal-close-img')#將廣告關閉 => 按右上角的叉叉
    close_ad_button.click()
    
    time.sleep(0.5)  # 每次爬取前暫停 0.5 秒
    jpg_list = []
    jpg_name = []
    try:
        while driver.find_element_by_xpath("//a[contains(text(),看更多)]"):#   進入網頁後，須將網頁完整展開
            next_pag = driver.find_element_by_xpath("//div[@class='more-drama-btn']")
            next_pag.click()
    
    except :
        #抓取圖檔網址
        for url_jpg in driver.find_elements_by_xpath("//img[2]"):
            urljpg=url_jpg.get_attribute("src")
            jpg_list.append(urljpg)
        #抓取圖檔名稱
        for name_jpg in driver.find_elements_by_xpath("//img[2]"):   
            namejpg=name_jpg.get_attribute("title")
            jpg_name.append(namejpg)
            
    print (jpg_list)
    print (jpg_name)       
    
    return jpg_list , jpg_name 
    
    driver.close()
    
def save(picture,tl):
    
    if picture:
        try:
            dname = tl.strip()  # 用 strip() 去除字串前後的空白
#             os.makedirs(dname) #建立資料夾(資料夾名稱)
#             for img_url in picture_list:
            fname = dname + '.jpg'
            urllib.request.urlretrieve(picture,  fname)
        except Exception as e:
            print(e)             

jpg_list , jpg_name = get_web_page(link_tw)


for i in range(len(jpg_list)):            
    save(jpg_list[i],jpg_name[i])      
```

    ['https://i.pinimg.com/236x/01/25/1a/01251a421192378432b59522d9f63005.jpg', 'https://i.pinimg.com/236x/c7/61/a8/c761a8e17123df1d627c760fd4e20181.jpg', 'https://i.pinimg.com/236x/de/ae/06/deae062a352c61e9b0c0379d87d36642.jpg', 'https://i.pinimg.com/236x/68/6a/9a/686a9a8066f50ca172a02bcade2e0d4e.jpg', 'https://i.pinimg.com/236x/bc/fd/77/bcfd77c69242f0ce720dafabe60735f7.jpg', 'https://i.pinimg.com/236x/16/a2/00/16a200059f833633df3e951061d11602.jpg', 'https://i.pinimg.com/236x/b6/d0/64/b6d064c8dd5644cf54aa16928e6662e5.jpg', 'https://i.pinimg.com/236x/9e/4b/df/9e4bdfd83ca51afc5cd72316ddaca565.jpg', 'https://i.pinimg.com/236x/8b/23/4e/8b234e21fb2675d79d235e3a5e779442.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/64/00/5a/64005ad7ceba7bdaddeb937fea615479.jpg', 'https://i.pinimg.com/236x/49/65/f2/4965f2c60e53bfa33987ecdd1190bf87.jpg', 'https://i.pinimg.com/236x/06/21/6d/06216dc6b6fc87451d77e643ef6787d8.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/0a/b7/fa/0ab7faec20ea339857d3083e4a69e593.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/84/8e/54/848e549a7156c7e6a4efd94bb2afa67e.jpg', 'https://i.pinimg.com/236x/12/ce/16/12ce16bd184253837326599b26e16c44.jpg', 'https://i.pinimg.com/236x/63/ba/7a/63ba7a31554a5412ab6fd1bdb13070bb.jpg', 'https://i.pinimg.com/236x/3c/55/14/3c5514f8148deb977be46044aeba158c.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/a9/11/bc/a911bc05b5e914314b96f504fd65c911.jpg', 'https://i.pinimg.com/236x/c9/8e/e1/c98ee108ceb0bfc06470fb16d6e56f53.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/db/4c/ab/db4cab84207b149c5f6b981af9553beb.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/64/95/ad/6495ad11a74436efc24a381d72390cae.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/ce/57/c1/ce57c1cd7dbb8a8cfc8eb06b3d344402.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/5f/e0/46/5fe046b7a89a4f48afe0717d6591c481.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/b2/3d/64/b23d643a835947b25efc20c04b4e79e0.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/2c/47/d6/2c47d6f7ec7de1010c48c27c9c264070.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/8d/21/09/8d21091185f506ea4cff62ae8188b055.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/f2/a5/5a/f2a55a4a034c3d051866044a4a571d70.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/4b/9d/c5/4b9dc53f65d3df740289cb000b3be52a.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/15/9a/09/159a0927b2dca64854ea5d5da7cd8efe.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/36/70/69/3670693f0200e0960352704ab8c68f10.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/62/96/b7/6296b79e897c7dd7d5aeb9581a773188.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/1b/af/65/1baf658955fc560bd532642f6e4fb00b.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/18/61/0f/18610f4a6f43cf4701262f119a310cc3.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/75/31/fe/7531fed5723465db7aaa547b5644f96a.jpg', 'http://c8.staticflickr.com//6//5491//30401603303_d3e7526b0c_z.jpg', 'https://img.chocotv.com.tw/indexPage/%E8%BD%9F.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/2c/60/4c/2c604cee73027ccb1814a0f0a5b708ad.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/b2/d1/87/b2d187febc229225fea47637af80e597.jpg', 'https://lh3.googleusercontent.com/-2mMY8cVqyNs/V6GeTudF-ZI/AAAAAAAAX-Y/0ZllPFIj28YZo5m0MIez7a8hrKQBMUB-ACCo/s512/20150424%25E8%25BF%25B7%25E5%25BE%2592Claire%25E6%25B5%25B7%25E5%25A0%25B1-%25E5%259B%259B%25E4%25BA%25BA%25E7%2589%2588-O-01.jpg', 'https://i.pinimg.com/236x/e2/2e/ef/e22eef24f06a76086f0ea19c08cde37f.jpg', 'https://lh3.googleusercontent.com/-w2eBl9y7M_g/VzWHGq84NBI/AAAAAAAAVvY/cQRL4jMBhrsWipu7m9LToVaPa2M68Bz0wCCo/s336/1458892524020.jpg', 'https://lh3.googleusercontent.com/-OSOLtVHXRH8/VzV8nSnOBQI/AAAAAAAAVuE/cRRjPEUyNIgFDC8sUnMK60D3n1GRwbdhQCCo/s512/f8004a7c-444c-4e88-bb29-faea7692021f.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/69/6c/55/696c55ba56983102e4d9d45fb46b681f.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/30/35/49/30354912dd432a2475428a003661784a.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/2c/f6/2c/2cf62cc4d018a0cd05c6e8655983accc.jpg', 'https://img.chocotv.com.tw/CHOCOTV-WEB/googleplay.png']
    ['搖滾畢業生', '愛情來了，請打卡', '女兵日記', '烏鴉燒', '愛的3.14159', '前男友不是人', '愛情教會我的事-秘密情人', '台北歌手', '高塔公主', '翻牆的記憶', 'gogo講客話', '大崎下微電影', '戀夏38℃', '我的男孩', '獅子王強大', '逃婚100次', '麻醉風暴2', '勇敢說出我愛你', '老爸上身', '我和我的四個男人', '稍息立正我愛你', '東區小巷的大廟', '替身', '偽婚男女', '咕咾小姐', '私室', '鐘樓愛人', '女仨的婚事', '守護寶地', '勞動之王', '酸甜之味', '迷徒CHLOE', '如朕親臨', '志明與春嬌', '明天一起去樂園', '轟電視', 'X情人', '必勝練習生', '迷徒CLAIRE', '我們是歐爸', '黑盒子', '谷風少年', '十里桂花香', '出境事務所', '落日', '']
    HTTP Error 403: Forbidden
    HTTP Error 403: Forbidden
    


```python
len(jpg_list) , len(jpg_name)
```


```python
import requests
import time
from bs4 import BeautifulSoup
import os
import re
import urllib.request
import json
from selenium import webdriver
import random
import pandas as pd


driver_path = r"D:\Program Files\chromedriver\chromedriver.exe" # chromedriver path #使用chrome瀏覽器

link_jp = 'https://www.chocotv.com.tw/drama?area=jp'


def get_web_page(link):
    driver = webdriver.Chrome(executable_path = driver_path)
    driver.get(link)#開啟chocotv日劇頁面
    
    close_ad_button = driver.find_element_by_css_selector('.modal-close-img')#將廣告關閉 => 按右上角的叉叉
    close_ad_button.click()
    
    time.sleep(0.5)  # 每次爬取前暫停 0.5 秒
    jpg_list = []
    jpg_name = []
    try:
        while driver.find_element_by_xpath("//a[contains(text(),看更多)]"):#   進入網頁後，須將網頁完整展開
            next_pag = driver.find_element_by_xpath("//div[@class='more-drama-btn']")
            next_pag.click()
    
    except :
        for url_jpg in driver.find_elements_by_xpath("//img[2]"):
            urljpg=url_jpg.get_attribute("src")
            jpg_list.append(urljpg)
        for name_jpg in driver.find_elements_by_xpath("//img[2]"):   
            namejpg=name_jpg.get_attribute("title")
            jpg_name.append(namejpg)
            
    print (jpg_list)
    print (jpg_name)       
    
    return jpg_list , jpg_name 
    
    driver.close()
    
def save(picture,tl):
    
    if picture:
        try:
            dname = tl.strip()  # 用 strip() 去除字串前後的空白
#             os.makedirs(dname) #建立資料夾(資料夾名稱)
#             for img_url in picture_list:
            fname = dname + '.jpg'
            urllib.request.urlretrieve(picture,  fname)
        except Exception as e:
            print(e)             

jpg_list , jpg_name = get_web_page(link_jp)


for i in range(len(jpg_list)):            
    save(jpg_list[i],jpg_name[i])      
```

    ['https://s-media-cache-ak0.pinimg.com/236x/b9/b3/79/b9b379796c78ebd5f6b7a98d8c8a3ba9.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/98/a4/59/98a459b976ef48795b5e6a0f7684c5d6.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/e3/90/bf/e390bfe325d7aa8842b2937407192c8f.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/db/8b/2d/db8b2db9e90bd81a98c467aaebf13f4f.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/c5/a0/89/c5a0895029b1b07766a34f4473bbed55.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/f4/6e/b7/f46eb7890c0aae5711e2510b770f1f4d.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/3a/b7/e2/3ab7e213ba3a80f9bce085324b4599e1.jpg', 'https://img.chocotv.com.tw/CHOCOTV-WEB/googleplay.png']
    ['天才婦科醫', '天皇的御廚', '月薪嬌妻', '砂之塔 知道太多事情的鄰居', '下町火箭', '拜託請愛我', '重版出來', '']
    HTTP Error 403: Forbidden
    


```python
import requests
import time
from bs4 import BeautifulSoup
import os
import re
import urllib.request
import json
from selenium import webdriver
import random
import pandas as pd


driver_path = r"D:\Program Files\chromedriver\chromedriver.exe" # chromedriver path #使用chrome瀏覽器

link_kr = 'https://www.chocotv.com.tw/drama?area=kr'


def get_web_page(link):
    driver = webdriver.Chrome(executable_path = driver_path)
    driver.get(link)#開啟chocotv韓劇頁面
    
    close_ad_button = driver.find_element_by_css_selector('.modal-close-img')#將廣告關閉 => 按右上角的叉叉
    close_ad_button.click()
    
    time.sleep(0.5)  # 每次爬取前暫停 0.5 秒
    jpg_list = []
    jpg_name = []
    try:
        while driver.find_element_by_xpath("//a[contains(text(),看更多)]"):#   進入網頁後，須將網頁完整展開
            next_pag = driver.find_element_by_xpath("//div[@class='more-drama-btn']")
            next_pag.click()
    
    except :
        for url_jpg in driver.find_elements_by_xpath("//img[2]"):
            urljpg=url_jpg.get_attribute("src")
            jpg_list.append(urljpg)
        for name_jpg in driver.find_elements_by_xpath("//img[2]"):   
            namejpg=name_jpg.get_attribute("title")
            jpg_name.append(namejpg)
            
    print (jpg_list)
    print (jpg_name)       
    
    return jpg_list , jpg_name 
    
    driver.close()
    
def save(picture,tl):
    
    if picture:
        try:
            dname = tl.strip()  # 用 strip() 去除字串前後的空白
#             os.makedirs(dname) #建立資料夾(資料夾名稱)
#             for img_url in picture_list:
            fname = dname + '.jpg'
            urllib.request.urlretrieve(picture,  fname)
        except Exception as e:
            print(e)             

jpg_list , jpg_name = get_web_page(link_kr)


for i in range(len(jpg_list)):            
    save(jpg_list[i],jpg_name[i])      
```

    ['https://i.pinimg.com/236x/da/d2/4a/dad24a68ef593b4cf82077befb282a8a.jpg', 'https://i.pinimg.com/236x/6f/12/a1/6f12a1e7a9372044c34d01ac4bcace64.jpg', 'https://i.pinimg.com/236x/b3/94/0b/b3940b58e7b7d9f0281009534efd51c3.jpg', 'https://i.pinimg.com/236x/d6/3e/f9/d63ef90e9a7f78e6a0d7ac23d83c5069.jpg', 'https://i.pinimg.com/236x/f8/0a/72/f80a72c8e193b94d086a410326ab6240.jpg', 'https://i.pinimg.com/236x/ea/04/67/ea04671f0ab74d44cc59cc8cb4fcae86.jpg', 'https://i.pinimg.com/236x/dc/a9/2b/dca92b8cf7f97a4581e85a8d861a2dbf.jpg', 'https://i.pinimg.com/236x/f2/bb/29/f2bb296c12291d3f633faf93339b7d07.jpg', 'https://i.pinimg.com/236x/55/f3/b8/55f3b822f351fe5b652cacbd88e18993.jpg', 'https://i.pinimg.com/236x/44/b1/60/44b1607e87749104853adea7020df933.jpg', 'https://i.pinimg.com/236x/24/be/ec/24beec0cc5d04a6e45d7cc61880550a5.jpg', 'https://i.pinimg.com/236x/70/bb/03/70bb03ac8f4fcd1ce4d02fc35f450cee.jpg', 'https://i.pinimg.com/236x/b6/31/9a/b6319a580445f760476bf2f667600732.jpg', 'https://i.pinimg.com/236x/8e/4f/04/8e4f0455d0c344f13ef3e90f33e99bc1.jpg', 'https://i.pinimg.com/236x/b4/99/e3/b499e3d14f8d9298ea6dc3e340c9cf9e.jpg', 'https://i.pinimg.com/236x/65/7f/20/657f207779fb28abdad3a990dd48bf5e.jpg', 'https://i.pinimg.com/236x/4a/1f/6f/4a1f6f6ac12e34e8de039a58d5a07a71.jpg', 'https://i.pinimg.com/236x/c8/5a/c1/c85ac19dfae378865d8f0ba5023a81fb.jpg', 'https://i.pinimg.com/236x/73/90/45/739045883c7e7f1fc4f9e4975cfcc710.jpg', 'https://i.pinimg.com/236x/47/52/57/47525727aee13065d9a616a982e3021d.jpg', 'https://i.pinimg.com/236x/47/52/57/47525727aee13065d9a616a982e3021d.jpg', 'https://i.pinimg.com/236x/af/61/70/af6170e3fb93ecdd16c9637c7b0f4cb3.jpg', 'https://i.pinimg.com/236x/2f/dd/18/2fdd181ed18e754e70fb0a41b3d07597.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/27/d2/2c/27d22c53127b71ccd800df099aee2040.jpg', 'https://i.pinimg.com/236x/8d/eb/a8/8deba8de1bf91e7f03b82c45717c178e.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/0c/67/4b/0c674b53657879b199155fbc3cc0e9d0.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/04/8c/6b/048c6b37ed374e8ae8a2fc8dd1ef6d0b.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/16/5b/fa/165bfa4eb39607ae2215a3c7e22d6c4a.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/91/34/d7/9134d78ac2d1aac4f57d501f570bc7c9.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/36/73/44/3673440a594689d7c20c46dd33e5ad64.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/ad/ea/22/adea225382a294433b7886cb64e5c8f2.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/b4/cb/2e/b4cb2e6cb2c077201925a282374ba8a8.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/3b/f6/9b/3bf69b3a8989b32636b4381fb2f3e199.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/3a/e4/8c/3ae48cff0d7a113ca3347c72bbf10267.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/c8/d0/8d/c8d08d396eeacad9773dc66aa33730d0.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/49/eb/f4/49ebf490faf3b66dbe5e7f12167f9d9a.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/c0/01/2c/c0012c349f7a6afd49b4ad1f4f3f33bf.jpg', 'https://i.pinimg.com/236x/a9/7b/0c/a97b0c71d6bf139c5fa5bcea04f30e1e.jpg', 'https://i.pinimg.com/236x/5c/22/7a/5c227ae1cdcdb338f47b18cec995bc1a.jpg', 'https://i.pinimg.com/236x/a1/e4/8d/a1e48da39deff60b630a6928dffafc62.jpg', 'https://i.pinimg.com/236x/88/2e/22/882e223418fe3c6cbd85661db0956616.jpg', 'https://i.pinimg.com/236x/95/50/7e/95507ecbb1f45b9acbaa32b033f0327e.jpg', 'https://i.pinimg.com/236x/ca/d7/83/cad7832258d159b017576b5804218d6b.jpg', 'https://i.pinimg.com/236x/ac/35/6c/ac356c59d54ccfdd987b67167a35bf6c.jpg', 'https://i.pinimg.com/236x/6d/13/ff/6d13ff173747102b1f9fe2eafde98703.jpg', 'https://i.pinimg.com/236x/03/cd/f1/03cdf1184d1f064db4dfbd34b00cfdbd.jpg', 'https://i.pinimg.com/236x/b4/61/b7/b461b71339b5b56434caa2f173ae3891.jpg', 'https://i.pinimg.com/236x/69/e6/13/69e6138e3ec400a6d69301fe9a575e13.jpg', 'https://i.pinimg.com/236x/56/23/3f/56233fd14f5ae5564688a79751023ba3.jpg', 'https://i.pinimg.com/236x/63/da/2b/63da2b948ba002fd79eac530467b0f8c.jpg', 'https://i.pinimg.com/236x/a9/ce/04/a9ce0471b332d0798b56c6c69a8988a8.jpg', 'https://i.pinimg.com/236x/57/2d/09/572d095bc280364ced81eaced96d8b01.jpg', 'https://i.pinimg.com/236x/80/30/32/8030329ca9d0429168584450d7ad8885.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/f6/fd/e0/f6fde0566f2299162dc28e8641949033.jpg', 'https://i.pinimg.com/236x/04/5b/20/045b20884bfb7c7f7717eba88f698d46.jpg', 'https://i.pinimg.com/236x/43/cb/69/43cb6980279510e78ca5f9c6b82bafb6.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/1c/87/43/1c8743922329e7af90f09d7ca53fb768.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/93/cb/57/93cb57b40f25ecc0f22908e7ce2ba38b.jpg', 'https://i.pinimg.com/236x/6f/a2/40/6fa2409439c170b490b3c9c9a0c9a9f9.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/45/b0/1c/45b01c88ab661195df0cd797a20059cc.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/8e/e7/e4/8ee7e42c14437a7f1f92aff183267a6c.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/6e/74/49/6e7449869df9e676cd7c322e3ffcde93.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/ff/fe/53/fffe53de9d8bdc87950395c7f072b2af.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/c0/8c/ba/c08cbaced66e3f1ed2bc5c8b2adf8e1c.jpg', 'https://i.pinimg.com/236x/49/4d/51/494d513b68589e52531f58c301277d33.jpg', 'https://s3.amazonaws.com/choco-tv/image/%E8%81%BD%E8%A6%8B%E4%BD%A0%E7%9A%84%E8%81%B2%E9%9F%B3-PWsyMvaT3j/poster/12787445_908978535886415_232213245_o.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/53/dc/43/53dc43f29308195fb55bf8627a85c396.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/b9/1b/cd/b91bcd91ef8b7efec37acbd43a1a116e.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/a8/3b/5b/a83b5badaef20aa025559cebd02382f9.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/f7/af/30/f7af303aa0b9d447c96b56fb175c0f44.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/fb/2a/35/fb2a35beb0019ba096d186a5c4e75860.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/9e/07/76/9e0776ebf91f7efe3ab8e00e2ee64c59.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/48/6f/28/486f28cbf85f0e8e1249a808ff4d9cf6.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/dc/04/d0/dc04d08e3acbaaabff3765a92c48a9ca.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/c7/7b/11/c77b114ad7a52175db013a0c095c1f0b.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/80/39/94/8039943d1ab886604521667e99e9f3d0.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/b6/b4/8b/b6b48b0c9498764ca46fce533811e24a.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/41/07/d6/4107d668ea6dab3db7fe956849061668.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/fd/11/ba/fd11ba1f95a2a7b7f0323196f688ede6.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/fa/de/1f/fade1f88fa2c7be295682fc43b48d39a.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/ba/89/24/ba892467205356da5b9b4396695fa025.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/c9/89/60/c989603d7d9b5b975bcc6a56c080f076.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/57/8a/5d/578a5d57229bbcbfd718d247488f8616.jpg', 'http://i.ksd-i.com/s/610*500_86400_edd477f4f88c42f01da6b307f0d3c732/static.koreastardaily.com/2016-08-23/83185-451051.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/04/82/dd/0482dd73883c3766f28f9360128a3296.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/ab/1b/84/ab1b8400f463d2765b05831e8be4188b.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/4f/a1/57/4fa157158da56af539ddbb2e3772d162.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/5d/59/46/5d5946d68a4fcf8506f4b91e658394a2.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/cc/86/d4/cc86d4acddb395012107e1191ce5e2ec.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/c0/60/cf/c060cf9bbfada4e67a8fb85e691603d5.jpg', 'https://2.bp.blogspot.com/-D1prbPhTTYw/WAh9h7yQ8XI/AAAAAAAAAbY/1K_5wc-P4yYhrGO16Nx-26OEvFV8WSexwCLcB/s1600/%25E9%259F%2593%25E5%258A%2587%25E5%258F%2588%25E7%2594%259C%25E5%258F%2588%25E8%25BE%25A3.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/df/bd/c1/dfbdc1afdebf663952b1fa4f551b880b.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/7f/66/d5/7f66d58b874297957240c26c1b08fb98.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/45/2d/2d/452d2dca737028f87a9923fa69e874fc.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/14/e1/90/14e19046e035104512a5d9362aee4144.jpg', 'http://i.ksd-i.com/s/800_86400_30b9c96d32ccfb1517a0d4acbf9d549b/static.koreastardaily.com/2015-10-29/70114-368800.jpg', 'https://i.pinimg.com/236x/8c/dd/c7/8cddc73d792b3a223636dfa3e68b1343.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/1c/70/a7/1c70a7a1bced205dd5376db691b4d4e2.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/32/fe/7d/32fe7d0ab3a48ef27a44595fbbfc52ba.jpg', 'https://i.pinimg.com/236x/c1/ab/9d/c1ab9d535762f57dcedb87849a0247a9.jpg', 'https://i.pinimg.com/236x/0a/5d/32/0a5d32f972621a4723fefd91ed4c004f.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/44/45/ec/4445ec58be39324230b14d4a0ea3cf9f.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/9b/27/98/9b2798cfa2a857b5630b50205a33fb8e.jpg', 'https://i.pinimg.com/236x/2e/d4/83/2ed483909d3af4292c780d73d2110826.jpg', 'https://4.bp.blogspot.com/-uaZaA-Y5SCg/V7pbHLjDkqI/AAAAAAAAcsk/SayaI4M3mIcHmolT3Z44_o7EfAYoE8UtgCPcB/s1600/W%25E2%2580%2594%25E5%2585%25A9%25E5%2580%258B%25E4%25B8%2596%25E7%2595%258C.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/97/2f/1b/972f1bcdf2c822f9f4892a26b2f598a9.jpg', 'https://i.pinimg.com/236x/57/fd/4b/57fd4b96aa2ffce68b68d0a10169d182.jpg', 'https://images.plurk.com/6rcC3rv4YYU6Z457i25Xxy.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/5e/e7/6f/5ee76f14843cb60e0066171732adcf4e.jpg', 'https://i.pinimg.com/236x/1d/73/68/1d7368ea905aaa82ccd4b91067c65daf.jpg', 'https://pic.pimg.tw/sabellawu/1471920225-3050080940.jpg', 'http://farm9.staticflickr.com//8621//30083916571_176964c6dd_z.jpg', 'https://s-media-cache-ak0.pinimg.com/236x/b0/97/0c/b0970cd40b4d7bfb67ee4387db54177d.jpg', 'https://img.chocotv.com.tw/CHOCOTV-WEB/googleplay.png']
    ['友情與愛情之間 (比舍堂遠,比議政府近)', '雖已30但仍17(雖然30但仍17)', '飲酒羅曼史 (飲酒歌舞)', '雪地裡的擁抱 (雪路)', '復仇筆記', '青春期集成曲 (青春期混合曲)', '金錢花(金錢之花)', '檢法男女', '過來抱抱我', '瘋了，因為你！', '人生中最閃亮的時光', 'Page Turner', '辦公室的鐘 Office Watch', '想停止的瞬間：About Time', '說出你的心願', '像妳一樣的女兒', '油膩的Melo', '女生們的世界', '所謂單戀-全知單戀視角', '上流愛情(國語發音)', '上流愛情(韓語發音)', '我的大叔', '偉大的誘惑者', '那個男人吳秀', '小神的孩子們', 'Voice', '只是相愛的關係', 'Signal', 'Misty 謎霧', 'Cross：神的禮物', 'Mother 兩個媽媽', '20世紀少男少女', 'Two Cops 我的鬼神搭檔', '今生是第一次', '操心', 'Jugglers神級秘書', '黑騎士', '舉重妖精金福珠', '七日的王妃', 'Melo Holic愛情中毒', '醫療船', '卞赫的愛情', 'Go Back夫婦(告白夫婦)', '搜查官愛麗絲 2', '魔女的法庭', '搜查官愛麗絲 1', 'Beauty Master', 'Mask', '戀愛禁止社 (禁戀術士)', '有品位的她', '內衣少女時代', 'To Be Continued', '想念, 春天', '穿梭戀人', '名不虛傳', '0時的她', '無限動力合宿院', '救救我', 'Love in Memory', 'Remember－兒子的戰爭', '愛爾蘭上勾拳之戀', '離婚律師戀愛中', '回來吧大叔', '紅心! 少年射箭部', '學校2017', '聽見你的聲音', '美味的愛戀', '河伯的新娘', 'About Love', '美女孔心', '龍八夷', 'Click Your Heart (點選你的心)', '獄中花', "What's with money", '不滅的女神', '為純情著迷', '灰姑娘與四騎士', '學校2015', '奔跑吧薔薇', '焦急的羅曼史', '我的野蠻公主', '冰丘(冰戀)', '三流之路', '嫉妒的化身', '奇怪的搭檔', '帥氣管家 KDI-109', '大力女子都奉順', '被告人', '假面', '仕女們', '甜辣之戀', '明天男孩', '噩夢老師', 'Spark-觸電男孩', '奇蹟', '愛上挑戰', '積極的體質', '愛上就會死的女人奉順', '金科長', '巧克力銀行', '千年戀愛中', '任意依戀', '花樣排球2', '花樣排球1', 'W兩個世界', '高品格單戀', '評價女王', '撲通撲通LOVE', '奶酪陷阱', '浪漫醫生金師傅', '雲畫的月光', 'Fantastic', '太陽的後裔', '']
    HTTP Error 403: Forbidden
    
