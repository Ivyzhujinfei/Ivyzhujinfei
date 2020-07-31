import requests
import re
from lxml import etree
import pandas as pd
import time
import random

#设置浏览器的头信息
headers = {'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_3) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/56.0.2924.87 Safari/537.36'}
#首先访问一个页面
url = 'https://www.anjuke.com/fangjia/cs2018/'
#对页面发起get请求
dfs = requests.get(url,headers = headers).text
html = etree.HTML(dfs)
#提取每个区以及每个区的链接
areas = html.xpath('//span[@class="elem-l"]//a/text()')[1:]
urls = html.xpath('//span[@class="elem-l"]//a/@href')[1:]


ls1 = []
ls2 = []
ls3 = []
ls4 = []
#循环爬取每一年的数据
for year in list(range(2014,2021)):
    print(year)
    #爬取每个区的数据
    for area,url in zip(areas,urls):
        print(area)
        #访问每个区每年的数据
        url_now = url.replace('2018',str(year))
        try:
            data = requests.get(url_now,headers = headers).text
        except Exception:
            time.sleep(3)
            data = requests.get(url_now,headers = headers).text
        html = etree.HTML(data)
        #提取里面的价格信息和年月信息
        price = html.xpath('//div[@class="fjlist-box boxstyle2"][1]/ul/li/a/span/text()')
        month = html.xpath('//div[@class="fjlist-box boxstyle2"][1]/ul/li/a/b/text()')
        area_ = [area] * len(price)
        year_ = [year] * len(price)
        if len(price) == 0:
            break
        #存储这些信息
        ls1.extend(area_)
        ls2.extend(year_)
        ls3.extend(month)
        ls4.extend(price)
        time.sleep(random.uniform(1,3))

#保存到dataframe表格数据中     
result = pd.DataFrame()
result['地区'] = ls1
result['年份'] = ls2
result['月份'] = ls3
result['房价'] = ls4 
#存储为excel数据
result.to_excel('D:/data.xlsx',index = None)

# In[]
dfs = pd.read_excel('D:/data.xlsx')
#提取年份数据
dfs['年月'] = dfs['月份'].map(lambda x:x.split('月')[0].replace('年','-'))
#提取房价数值型数据
dfs['房价'] = dfs['房价'].map(lambda x:float(x.split('元')[0])) 
df = dfs[['地区','年月','房价']]
df.to_excel('D:/datas.xlsx',index = None)
#透视表操作
dff = df.pivot_table(index='年月', columns='地区', values='房价')
dff.to_excel('D:/datas1.xlsx')# -
