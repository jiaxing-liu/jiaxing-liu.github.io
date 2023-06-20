---
layout:     post
title:      "Bouk download of journals"
subtitle:   "Nuclear Fusion"
date:       2023-06-20 23:00:00
author:     "JXLIU"
header-img: "img/post-bg-2015.jpg"
mathjax: true
tags:
    - Python
    - Journal download
---

## 批量下载某一杂志 PDF 文件
以下 python 脚本可以批量下载 Nuclear Fusion 某一期 PDF 文件，以第一作者命名并保存到文件夹 `NF/vol/issue` 中

```python
import os
import time
import requests
from bs4 import BeautifulSoup

baseurl = 'https://iopscience.iop.org'
vol = 63
issue = 6
isbn = '/0029-5515/'
journal = 'NF'
url = baseurl + '/issue' + isbn + str(vol) + '/' +str(issue)
print(url)
#url = 'https://iopscience.iop.org/issue/0029-5515/63/8'
response = requests.get(url)
soup = BeautifulSoup(response.text, 'html.parser')
print(soup)
#meta_list = soup.select('.small.art-list-item-meta, .mr-2.nowrap')


meta_list = soup.select('.small.art-list-item-meta')
pdf_names = []  # 用于保存 PDF 文件链接的列表for meta in meta_list:
    authors = meta.text.strip()
    print(authors)
    if ',' in authors:
        firstauthor = authors.split(',')[0].replace(' ', '')
    else:
        firstauthor = authors.strip().replace(' ', '')
    print(firstauthor)
    pdf_names.append(firstauthor+'.pdf')
    
link_list = soup.select('.mr-2.nowrap')
pdf_urls = []  # 用于保存 PDF 文件链接的列表for link in link_list:
    href = link.get('href')
    if href[-3:].upper() == 'PDF':
        pdf_url = baseurl+href
        pdf_urls.append(pdf_url)
        print(pdf_url)

## 创建文件夹os.makedirs(os.path.join(journal,str(vol),str(issue)),exist_ok=True)
## 下载 PDF
# 初始化计数器count = 0
for pdf_url, pdf_name in zip(pdf_urls, pdf_names):
    # 检查文件是否存在    if not os.path.exists(os.path.join(journal, str(vol), str(issue), pdf_name)):
        with open(os.path.join(journal, str(vol), str(issue), pdf_name), "wb") as f:
            resp = requests.get(pdf_url)
            f.write(resp.content)
        print(pdf_name + "文件已下载保存到文件夹 " + os.path.join(journal, str(vol), str(issue)))
        count += 1
    else:
        # 如果文件已经存在，则重命名文件        name, ext = os.path.splitext(pdf_name)
        i = 1
        new_name = name + "_" + str(i) + ext
        while os.path.exists(os.path.join(journal, str(vol), str(issue), new_name)):
            i += 1
            new_name = name + "_" + str(i) + ext
        with open(os.path.join(journal, str(vol), str(issue), new_name), "wb") as f:
            resp = requests.get(pdf_url)
            f.write(resp.content)
        print(new_name + "文件已下载保存到文件夹 " + os.path.join(journal, str(vol), str(issue)))
        # 计数器加 1
        count += 1
    #time.sleep(5*60)
    time.sleep(1)
    # 如果所有的 PDF 文件都已经被下载并保存，则退出循环if count == len(pdf_names):
    print('volume '+str(vol) +', issue ' + str(issue) + '已经下载完毕.')
else:
    print('下载文件数目有错。')
```
