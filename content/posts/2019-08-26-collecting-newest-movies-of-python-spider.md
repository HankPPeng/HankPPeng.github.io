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

&emsp;8.28--下午阳光猛烈，可我心里只想实现之前的功能，写文本乱码的问题已经解决，添加上发送邮件的功能，现在只差一个定时抓取的功能，有时间再去研究。将阳光电影的代码删除了，因为写得太丑了...

* 这是电影天堂2018的代码。

```
import requests
import re
from lxml import etree
import csv
import os
from smtplib import SMTP_SSL
from email.header import Header
from email.mime.text import MIMEText
from email.mime.multipart import MIMEMultipart

#抓取电影数据
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
                print('更新日期：', movie_date, movie_name)
                writer.writerow((page, movie_date, movie_name))
            print('Collecting successfully on Page %d.\n' %page)
        except:
            print('Try Later.\n')
    if r.status_code == 200:
        send_mail()
    else:
        print("Cannot collect all details, thus, we wouldn't send an e-mail.\n")

#发送邮件
def send_mail():
    # qq邮箱smtp服务器
    host_server = 'smtp.qq.com'
    # sender_qq为发件人的qq号码
    sender_qq = '*'
    # pwd为qq邮箱的授权码
    pwd = '*'
    # 发件人的邮箱
    sender_qq_mail = '*'
    # 收件人邮箱
    receiver = '*'

    # 邮件的正文内容
    mail_title = '电影天堂电影更新数据'
    msg = MIMEMultipart()
    msg["Subject"] = Header(mail_title, 'utf-8')
    msg["From"] = sender_qq_mail
    msg["To"] = Header("*", 'utf-8')  # 接收者的别名
    mail_content = "电影更新信息。"
    msg.attach(MIMEText(mail_content, 'html', 'utf-8'))

    # 构造附件1，传送当前目录（py程序目录）下的附件
    att1 = MIMEText(open('dytt2018.csv', 'rb').read(), 'base64', 'utf-8')
    att1["Content-Type"] = 'application/octet-stream'
    # 这里的filename可以任意写，写什么名字，邮件中显示什么名字
    att1["Content-Disposition"] = 'attachment; filename="dytt2018.csv"'
    msg.attach(att1)

    # ssl登录
    smtp = SMTP_SSL(host_server)
    # set_debuglevel()是用来调试的。参数值为1表示开启调试模式，参数值为0关闭调试模式
    smtp.set_debuglevel(1)
    smtp.ehlo(host_server)
    smtp.login(sender_qq, pwd)
    smtp.sendmail(sender_qq_mail, receiver, msg.as_string())
    smtp.quit()


if __name__ == '__main__':

    fp = open(r'*:\*\dytt2018.csv', 'w+', newline='', encoding='utf-8-sig')
    writer = csv.writer(fp)
    writer.writerow(('页数', '更新日期', '标题'))
    get_newm('https://www.dy2018.com/html/gndy/dyzz/index.html')
    os.system("pause")
```