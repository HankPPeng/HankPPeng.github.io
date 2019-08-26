---
title: Using Python-Spider to watch the newest movies online
author: ''
date: '2019-08-26'
slug: collecting-newest-movies-of-python-spider
categories: []
tags:
  - Study
description: ''
externalLink: ''
series: []
---
&emsp;8.26--早上写了段抓取‘阳光电影’中‘最新电影’第一页的spider，本来想实现‘日期，电影名称，IMDb评分，电影详情页链接’这四个内容写入.csv文件中，不过像是编码问题写进去的中文是乱码，暂时没有头绪，还有就是IMDb的评分怎么也抓不下来，不过在命令窗口里看到更新了什么电影已经很满足了...日后再研究。附上Python的代码。

&emsp;8.26--下午犯困，想着去其他的电影网站，接着发现dy2018的界面做得不错，而且每个标题都有评分，貌似是豆瓣和IMDb混合着使用的，不过满足我的需求，花了半个小时更改了一下代码，将没有用的删除。

* 这是阳光电影的代码。

```

import requests
import re
from lxml import etree
import csv
import os


def get_newm(url):
    r = requests.get(url)
    r.encoding = 'gb2312'
    html = etree.HTML(r.text)
    infos = html.xpath('//div[@class="co_content8"]/ul//table')
    for info in infos:
        detail_url = 'http://www.ygdy8.com' + info.xpath('tr[2]/td[2]/b/a/@href')[0]
        movie_name = info.xpath('tr[2]/td[2]/b/a/text()')[0]
        print(movie_name)
        get_mdetail(detail_url, movie_name)
        # print(page_url)


def get_mdetail(detail_url, movie_name):
    r = requests.get(detail_url)
    r.encoding = 'gb2312'
    html = etree.HTML(r.text)
    movie_date = html.xpath('//div[@class="co_content8"]/ul/text()')
    date = re.findall(r'发布时间：(.*)\r',movie_date[0])
    
    '''
    movie_text = html.xpath('//div[@id="Zoom"]/span/p[1]/text()[10]')
    movie_IMDb = re.findall('◎IMDb评分(.*)', movie_text)
    '''
    
    writer.writerow((date,movie_name,movie_text,detail_url))
    print('successful!')


if __name__ == '__main__':
    
    '''
    fp = open(r'E:\Github\Python Spider\ygdy-spider\ygdy.csv', 'w+', newline='', encoding='utf-8')
    writer = csv.writer(fp)
    writer.writerow(('Date', 'movie_name', 'movie_url'))
    '''
    
    get_newm('https://www.ygdy8.com/html/gndy/dyzz/index.html')
    print('\nAll Done!\n')
    os.system("pause")

```

* 这是电影天堂2018的代码。

```
import requests
import re
from lxml import etree
import os


def get_newm(url):
    for page in range(1,6):
        if page == 1:
            pageurl = url
        else:
            pageurl = 'https://www.dy2018.com/html/gndy/dyzz/index_' + str(page) + '.html'
        try:
            r = requests.get(pageurl)
            r.raise_for_status()
            r.encoding = 'gb2312'
            html = etree.HTML(r.text)
            infos = html.xpath('//div[@class="co_content8"]/ul//table')
            print("\nThis is the Page %d." %page)
            for info in infos:
                movie_name = info.xpath('tr[2]/td[2]/b/a/@title')
                movie_date_text = info.xpath('tr[3]/td[2]/font/text()')
                movie_date = re.findall('日期：(.*) ',movie_date_text[0])
                print(movie_date, movie_name)
        except:
            print('\nTry Later.\n')


if __name__ == '__main__':

    get_newm('https://www.dy2018.com/html/gndy/dyzz/index.html')
    os.system("pause")
```