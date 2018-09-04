# 影音平台片單爬蟲
- json版
```python
import requests
from bs4 import BeautifulSoup
import json
import pandas as pd

#經過網頁觀察發現LiTV將影片資訊存為json檔
#抓取該json檔
litv_list = requests.get("https://www.litv.tv/vod/ajax/getAllSimpleProgramByContentType?contentType=drama&_=1529467186962")
list = json.loads(litv_list.text) #經過json.load後，從string轉為list type

#設定需要收集資訊的list
title_list=[]#標題
display_list=[]#集數
landing_url_list=[]#網址
country_list=[]#國家

for i in range(len(list)):
    
    title=list[i]['title']
    title_list.append(title)
    
    display=list[i]['display_count']
    display_list.append(display)
    
    landing_url="https://www.litv.tv"+list[i]['landing_url']
    landing_url_list.append(landing_url)
     
    country=list[i]['f_countries']
    country_list.append(country)
    
   

    LiTV_drama_info={
        'title': title_list,
        'display': display_list,
        'landing_url':landing_url_list,
        'country':country_list,    
    }
    
df_LiTV_drama_info=pd.DataFrame(LiTV_drama_info)
#存檔為csv檔
df_LiTV_drama_info.to_csv('df_LiTV_drama_0703.csv', encoding = ('utf-8'))
```



- 一頁式展開版
```python
from selenium import webdriver
import requests
import time
import random
import pandas as pd
from bs4 import BeautifulSoup
import csv

driver_path = r"D:\Program Files\chromedriver\chromedriver.exe" # chromedriver path #使用chrome瀏覽器

def Linetv_link(link) :
    driver = webdriver.Chrome(executable_path = driver_path)
    driver.get(link)#開啟節目頁面
#     #將廣告關閉 => 按右上角的叉叉
#     close_ad_button = driver.find_element_by_css_selector('.modal-close-img')
#     close_ad_button.click()
    #抓取url
    url_list=[]
    try:
        while driver.find_element_by_xpath("//a[contains(text(),查看更多)]"):#將頁面完全展開
            next_pag = driver.find_element_by_xpath("//span[@class='btn_box']")
            next_pag.click()
    
    except :
        for url_top in driver.find_elements_by_xpath("//a[@class='inner']"):#收集每部片的網址
            urltop=url_top.get_attribute("href")
            url_list.append(urltop)
            
    #關閉瀏覽器
    driver.close()
    #回傳資料
    return url_list


def linetv_mov_detail(url_list) :
    tv_title = []
    tv_tag = []
    for link in url_list:
        driver = webdriver.Chrome(executable_path = driver_path)
        driver.get(link)
        #抓取劇名收集
        title_list = driver.find_element_by_xpath("//h1[@class='tit']")
        title = title_list.text
        tv_title.append(title)
        #抓取類型標籤
        try:
            tag_list = driver.find_element_by_xpath("//ul[@class='detail']/li[2]/span")
            t = tag_list.text
            tv_tag.append(t)
        except :
            tag_list = "null"#若無法成功收集則放入null
            tv_tag.append(tag_list)
            
        time.sleep(random.randint(1, 2))#休息一至二秒
        
        driver.close()
    
    return  tv_title, tv_tag


tv_all = pd.DataFrame()

#執行抓取url
url_list = Linetv_link("https://tv.line.me/c/drama/channels")
#執行抓取細節   
tv_title , tv_tag = linetv_mov_detail(url_list)
#彙整一切資訊
tv_info = {
        "1.劇名": tv_title,
        "2.url": url_list,
        "3.標籤": tv_tag
        }   
#將資料轉換成pandas格式

tv_info_df = pd.DataFrame(tv_info)
tv_all=tv_all.append(tv_info_df)

#將資料以utf8編碼儲存為csv檔
tv_all.reset_index(drop=True).to_csv('Linetv_tv_0703.csv', encoding = ('utf-8'))
```
