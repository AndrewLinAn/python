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


