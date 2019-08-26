---
title: Basic function of Python-Spider
author: ''
date: '2019-08-22'
slug: basic-function-of-python-spider
categories: []
tags:
  - Study
description: ''
externalLink: ''
series: []
---
&emsp;最近重温重温Python的内容，在我实际的应用中，用的比较多的是 `import...` 和 `def ...`，在记住一些基本的语法规则以后就可以在一些场合比较好地使用Python。近日也突发奇想，用爬虫来抓取某网站的电影数据，之后将数据推送到自己的设备上面，虽然现在也存在着一些App能够做到这种功能，不过自定义程度不够高，希望学习一下能够满足自己的小小愿望。

&emsp;8.26--早上写了段抓取‘阳光电影’中‘最新电影’第一页的spider，本来想实现‘日期，电影名称，IMDb评分，电影详情页链接’这四个内容写入.csv文件中，不过像是编码问题写进去的中文是乱码，暂时没有头绪，还有就是IMDb的评分怎么也抓不下来，不过在命令窗口里看到更新了什么电影已经很满足了...日后再研究。附上Python的代码。


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