

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


```python
df_LiTV_drama_info#檢查檔案內容
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>title</th>
      <th>display</th>
      <th>landing_url</th>
      <th>country</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>君主－假面的主人</td>
      <td>全 40 集</td>
      <td>https://www.litv.tv/vod/drama/content.do?id=VO...</td>
      <td>KR</td>
    </tr>
    <tr>
      <th>1</th>
      <td>黃金稻浪</td>
      <td>全 20 集</td>
      <td>https://www.litv.tv/vod/drama/content.do?id=VO...</td>
      <td>TW</td>
    </tr>
    <tr>
      <th>2</th>
      <td>三流之路</td>
      <td>全 16 集</td>
      <td>https://www.litv.tv/vod/drama/content.do?id=VO...</td>
      <td>KR</td>
    </tr>
    <tr>
      <th>3</th>
      <td>天地風雲錄之決戰時刻</td>
      <td>全 20 集</td>
      <td>https://www.litv.tv/vod/drama/content.do?id=VO...</td>
      <td>TW</td>
    </tr>
    <tr>
      <th>4</th>
      <td>天地風雲錄之九龍變</td>
      <td>全 36 集</td>
      <td>https://www.litv.tv/vod/drama/content.do?id=VO...</td>
      <td>TW</td>
    </tr>
    <tr>
      <th>5</th>
      <td>你也是人類嗎</td>
      <td>更新至第 2 集</td>
      <td>https://www.litv.tv/vod/drama/content.do?id=VO...</td>
      <td>KR</td>
    </tr>
    <tr>
      <th>6</th>
      <td>偉大的誘惑者</td>
      <td>全 32 集</td>
      <td>https://www.litv.tv/vod/drama/content.do?id=VO...</td>
      <td>KR</td>
    </tr>
    <tr>
      <th>7</th>
      <td>傻春</td>
      <td>全 34 集</td>
      <td>https://www.litv.tv/vod/drama/content.do?id=VO...</td>
      <td>CN</td>
    </tr>
    <tr>
      <th>8</th>
      <td>菸田少年</td>
      <td>全 20 集</td>
      <td>https://www.litv.tv/vod/drama/content.do?id=VO...</td>
      <td>TW</td>
    </tr>
    <tr>
      <th>9</th>
      <td>月滿水沙漣</td>
      <td>全 20 集</td>
      <td>https://www.litv.tv/vod/drama/content.do?id=VO...</td>
      <td>TW</td>
    </tr>
    <tr>
      <th>10</th>
      <td>老伴</td>
      <td>全 1 集</td>
      <td>https://www.litv.tv/vod/drama/content.do?id=VO...</td>
      <td>TW</td>
    </tr>
    <tr>
      <th>11</th>
      <td>李衛辭官</td>
      <td>全 42 集</td>
      <td>https://www.litv.tv/vod/drama/content.do?id=VO...</td>
      <td>CN</td>
    </tr>
    <tr>
      <th>12</th>
      <td>親親小爸</td>
      <td>全 62 集</td>
      <td>https://www.litv.tv/vod/drama/content.do?id=VO...</td>
      <td>KR</td>
    </tr>
    <tr>
      <th>13</th>
      <td>李衛當官</td>
      <td>全 30 集</td>
      <td>https://www.litv.tv/vod/drama/content.do?id=VO...</td>
      <td>CN</td>
    </tr>
    <tr>
      <th>14</th>
      <td>女人家</td>
      <td>全 1 集</td>
      <td>https://www.litv.tv/vod/drama/content.do?id=VO...</td>
      <td>TW</td>
    </tr>
    <tr>
      <th>15</th>
      <td>十里桂花香</td>
      <td>全 5 集</td>
      <td>https://www.litv.tv/vod/drama/content.do?id=VO...</td>
      <td>TW</td>
    </tr>
    <tr>
      <th>16</th>
      <td>親親小爸(國語配音)</td>
      <td>全 62 集</td>
      <td>https://www.litv.tv/vod/drama/content.do?id=VO...</td>
      <td>KR</td>
    </tr>
    <tr>
      <th>17</th>
      <td>宮心計</td>
      <td>全 33 集</td>
      <td>https://www.litv.tv/vod/drama/content.do?id=VO...</td>
      <td>HK</td>
    </tr>
    <tr>
      <th>18</th>
      <td>紫禁驚雷(國語配音)</td>
      <td>全 26 集</td>
      <td>https://www.litv.tv/vod/drama/content.do?id=VO...</td>
      <td>HK,CN</td>
    </tr>
    <tr>
      <th>19</th>
      <td>王在相愛</td>
      <td>全 27 集</td>
      <td>https://www.litv.tv/vod/drama/content.do?id=VO...</td>
      <td>KR</td>
    </tr>
    <tr>
      <th>20</th>
      <td>嫁掉頭家娘</td>
      <td>全 1 集</td>
      <td>https://www.litv.tv/vod/drama/content.do?id=VO...</td>
      <td>TW</td>
    </tr>
    <tr>
      <th>21</th>
      <td>阿青，回家了</td>
      <td>全 1 集</td>
      <td>https://www.litv.tv/vod/drama/content.do?id=VO...</td>
      <td>TW</td>
    </tr>
    <tr>
      <th>22</th>
      <td>動物系戀人啊</td>
      <td>全 20 集</td>
      <td>https://www.litv.tv/vod/drama/content.do?id=VO...</td>
      <td>TW</td>
    </tr>
    <tr>
      <th>23</th>
      <td>咱家</td>
      <td>全 54 集</td>
      <td>https://www.litv.tv/vod/drama/content.do?id=VO...</td>
      <td>CN</td>
    </tr>
    <tr>
      <th>24</th>
      <td>解密</td>
      <td>全 41 集</td>
      <td>https://www.litv.tv/vod/drama/content.do?id=VO...</td>
      <td>CN</td>
    </tr>
    <tr>
      <th>25</th>
      <td>無心法師2</td>
      <td>全 22 集</td>
      <td>https://www.litv.tv/vod/drama/content.do?id=VO...</td>
      <td>CN</td>
    </tr>
    <tr>
      <th>26</th>
      <td>多金社長小資女</td>
      <td>更新至第 10 集</td>
      <td>https://www.litv.tv/vod/drama/content.do?id=VO...</td>
      <td>KR</td>
    </tr>
    <tr>
      <th>27</th>
      <td>幸福在一起</td>
      <td>全 45 集</td>
      <td>https://www.litv.tv/vod/drama/content.do?id=VO...</td>
      <td>CN</td>
    </tr>
    <tr>
      <th>28</th>
      <td>思美人</td>
      <td>全 78 集</td>
      <td>https://www.litv.tv/vod/drama/content.do?id=VO...</td>
      <td>CN</td>
    </tr>
    <tr>
      <th>29</th>
      <td>推理的女王 2</td>
      <td>全 16 集</td>
      <td>https://www.litv.tv/vod/drama/content.do?id=VO...</td>
      <td>KR</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>660</th>
      <td>只想比你多活一天</td>
      <td>全 1 集</td>
      <td>https://www.litv.tv/vod/drama/content.do?id=VO...</td>
      <td>TW</td>
    </tr>
    <tr>
      <th>661</th>
      <td>水源地</td>
      <td>全 1 集</td>
      <td>https://www.litv.tv/vod/drama/content.do?id=VO...</td>
      <td>TW</td>
    </tr>
    <tr>
      <th>662</th>
      <td>刑法第274條</td>
      <td>全 1 集</td>
      <td>https://www.litv.tv/vod/drama/content.do?id=VO...</td>
      <td>TW</td>
    </tr>
    <tr>
      <th>663</th>
      <td>回家路上</td>
      <td>全 1 集</td>
      <td>https://www.litv.tv/vod/drama/content.do?id=VO...</td>
      <td>TW</td>
    </tr>
    <tr>
      <th>664</th>
      <td>失憶證</td>
      <td>全 1 集</td>
      <td>https://www.litv.tv/vod/drama/content.do?id=VO...</td>
      <td>TW</td>
    </tr>
    <tr>
      <th>665</th>
      <td>公主與王子</td>
      <td>全 1 集</td>
      <td>https://www.litv.tv/vod/drama/content.do?id=VO...</td>
      <td>TW</td>
    </tr>
    <tr>
      <th>666</th>
      <td>大路</td>
      <td>全 1 集</td>
      <td>https://www.litv.tv/vod/drama/content.do?id=VO...</td>
      <td>TW</td>
    </tr>
    <tr>
      <th>667</th>
      <td>三朵花純理髮</td>
      <td>全 1 集</td>
      <td>https://www.litv.tv/vod/drama/content.do?id=VO...</td>
      <td>TW</td>
    </tr>
    <tr>
      <th>668</th>
      <td>仲夏夜府城</td>
      <td>全 1 集</td>
      <td>https://www.litv.tv/vod/drama/content.do?id=VO...</td>
      <td>TW</td>
    </tr>
    <tr>
      <th>669</th>
      <td>6局下半</td>
      <td>全 1 集</td>
      <td>https://www.litv.tv/vod/drama/content.do?id=VO...</td>
      <td>TW</td>
    </tr>
    <tr>
      <th>670</th>
      <td>我的爸爸是流氓</td>
      <td>全 1 集</td>
      <td>https://www.litv.tv/vod/drama/content.do?id=VO...</td>
      <td>TW</td>
    </tr>
    <tr>
      <th>671</th>
      <td>十七號出入口</td>
      <td>全 1 集</td>
      <td>https://www.litv.tv/vod/drama/content.do?id=VO...</td>
      <td>TW</td>
    </tr>
    <tr>
      <th>672</th>
      <td>回聲</td>
      <td>全 1 集</td>
      <td>https://www.litv.tv/vod/drama/content.do?id=VO...</td>
      <td>TW</td>
    </tr>
    <tr>
      <th>673</th>
      <td>台北‧叢林</td>
      <td>全 1 集</td>
      <td>https://www.litv.tv/vod/drama/content.do?id=VO...</td>
      <td>TW</td>
    </tr>
    <tr>
      <th>674</th>
      <td>我的冒泡屋</td>
      <td>全 1 集</td>
      <td>https://www.litv.tv/vod/drama/content.do?id=VO...</td>
      <td>TW</td>
    </tr>
    <tr>
      <th>675</th>
      <td>山上的理髮師</td>
      <td>全 1 集</td>
      <td>https://www.litv.tv/vod/drama/content.do?id=VO...</td>
      <td>TW</td>
    </tr>
    <tr>
      <th>676</th>
      <td>不死搖滾夢</td>
      <td>全 1 集</td>
      <td>https://www.litv.tv/vod/drama/content.do?id=VO...</td>
      <td>TW</td>
    </tr>
    <tr>
      <th>677</th>
      <td>三十秒過後</td>
      <td>全 1 集</td>
      <td>https://www.litv.tv/vod/drama/content.do?id=VO...</td>
      <td>TW</td>
    </tr>
    <tr>
      <th>678</th>
      <td>今天不上班</td>
      <td>全 1 集</td>
      <td>https://www.litv.tv/vod/drama/content.do?id=VO...</td>
      <td>TW</td>
    </tr>
    <tr>
      <th>679</th>
      <td>生命紀念冊</td>
      <td>全 1 集</td>
      <td>https://www.litv.tv/vod/drama/content.do?id=VO...</td>
      <td>TW</td>
    </tr>
    <tr>
      <th>680</th>
      <td>2隻魚，游啊游上岸</td>
      <td>全 1 集</td>
      <td>https://www.litv.tv/vod/drama/content.do?id=VO...</td>
      <td>TW</td>
    </tr>
    <tr>
      <th>681</th>
      <td>天使一刻鐘</td>
      <td>全 1 集</td>
      <td>https://www.litv.tv/vod/drama/content.do?id=VO...</td>
      <td>TW</td>
    </tr>
    <tr>
      <th>682</th>
      <td>仙人掌男人</td>
      <td>全 1 集</td>
      <td>https://www.litv.tv/vod/drama/content.do?id=VO...</td>
      <td>TW</td>
    </tr>
    <tr>
      <th>683</th>
      <td>幸福返鄉30+</td>
      <td>全 6 集</td>
      <td>https://www.litv.tv/vod/drama/content.do?id=VO...</td>
      <td>TW</td>
    </tr>
    <tr>
      <th>684</th>
      <td>牛</td>
      <td>全 1 集</td>
      <td>https://www.litv.tv/vod/drama/content.do?id=VO...</td>
      <td>TW</td>
    </tr>
    <tr>
      <th>685</th>
      <td>再看我一眼</td>
      <td>全 1 集</td>
      <td>https://www.litv.tv/vod/drama/content.do?id=VO...</td>
      <td>TW</td>
    </tr>
    <tr>
      <th>686</th>
      <td>六輪之戀</td>
      <td>全 1 集</td>
      <td>https://www.litv.tv/vod/drama/content.do?id=VO...</td>
      <td>TW</td>
    </tr>
    <tr>
      <th>687</th>
      <td>九歲那年</td>
      <td>全 1 集</td>
      <td>https://www.litv.tv/vod/drama/content.do?id=VO...</td>
      <td>TW</td>
    </tr>
    <tr>
      <th>688</th>
      <td>公主徹夜未眠</td>
      <td>全 1 集</td>
      <td>https://www.litv.tv/vod/drama/content.do?id=VO...</td>
      <td>TW</td>
    </tr>
    <tr>
      <th>689</th>
      <td>再見童年</td>
      <td>全 1 集</td>
      <td>https://www.litv.tv/vod/drama/content.do?id=VO...</td>
      <td>TW</td>
    </tr>
  </tbody>
</table>
<p>690 rows × 4 columns</p>
</div>


