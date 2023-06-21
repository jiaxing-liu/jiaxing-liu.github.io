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

### release v1
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

### release v2

优化提取数据方式、命名规则，可下载多卷内容
```python
import os
import sys
import subprocess
import time
import requests
import random
from bs4 import BeautifulSoup

for issue in range (1, 2):

    baseurl = 'https://iopscience.iop.org'
    vol = 63
    isbn = '/0029-5515/'
    journal = 'NF'
    url = baseurl + '/issue' + isbn + str(vol) + '/' +str(issue)
    print(url)
    #url = 'https://iopscience.iop.org/issue/0029-5515/63/8'
    response = requests.get(url)
    soup = BeautifulSoup(response.text, 'html.parser')
    if soup.find('h2', string='We apologize for the inconvenience...'):
        print(soup)
        print('Please unlock the website manually.')
        sys.exit()
    #meta_list = soup.select('.small.art-list-item-meta, .mr-2.nowrap')

    # Find all divs with class="art-list-item-body"
    art_list_item_body_divs = soup.find_all('div', {'class': 'art-list-item-body'})
    
    # Create empty lists to store values
    #indexer_list = []
    #authors_list = []
    #mr_2_nowrap_list = []
    pdf_names = []  # 用于保存 PDF 文件链接的列表    pdf_urls = []  # 用于保存 PDF 文件链接的列表    # Loop through each div and extract values of class="indexer", "small art-list-item-meta", "mr-2 nowrap"
    for div in art_list_item_body_divs:
        indexer = div.find('div', {'class': 'indexer'})
        small_art_list_item_meta = div.find('p', {'class': 'small art-list-item-meta'})
        mr_2_nowrap = div.find('div', {'class': 'art-list-item-title-wrapper'})
        a = mr_2_nowrap.find('a', {'class': 'art-list-item-title'})
        href = a.get('href')
        if small_art_list_item_meta is not None: 
            authors = small_art_list_item_meta.get_text(strip=True)
            if ',' in authors:
                firstauthor = authors.split(',')[0].replace(' ', '')
            else:
                firstauthor = authors.strip().replace(' ', '')
        else:
            print('No small art-list-item-meta found in this art-list-item-body')
            firstauthor = 'None'
        indexer = indexer.text.strip()
        title = mr_2_nowrap.text.strip().replace(' ','_')
        pdf_name = firstauthor + '_' + indexer + '_' + title + '.pdf'
        pdf_url = baseurl + href + '/pdf'
        pdf_names.append(pdf_name)
        pdf_urls.append(pdf_url)
        print(pdf_name)
        print(pdf_url)
    # sys.exit()
    # 检查 pdf_name 和 pdf_url 的一致性    print(str(issue),str(len(pdf_names)),str(len(pdf_urls)))
    if(len(pdf_names) != len(pdf_urls)):
        print('The number of authors and urls is different in volume ' + str(vol) + ', issue ' + str(issue) + '.')
        continue
    print(str(issue))
    ## 创建文件夹    os.makedirs(os.path.join(journal,str(vol),str(issue)),exist_ok=True)
    ## 下载 PDF
    # 初始化计数器    count = 0
    for pdf_url, pdf_name in zip(pdf_urls, pdf_names):
        #print(pdf_url)
        #print(pdf_name)
        # 检查文件是否存在        if not os.path.exists(os.path.join(journal, str(vol), str(issue), pdf_name)):
            with open(os.path.join(journal, str(vol), str(issue), pdf_name), "wb") as f:
                resp = requests.get(pdf_url)
                f.write(resp.content)
            print(pdf_name + "文件已下载保存到文件夹 " + os.path.join(journal, str(vol), str(issue)))
            pdfpath = os.path.join(journal, str(vol), str(issue), pdf_name)
            try:
                subprocess.check_output(['gs', '-q', '-dNOPAUSE', '-dBATCH', '-sDEVICE=nullpage', pdfpath])
                print(pdf_name + ' can be opened')
            except subprocess.CalledProcessError:
                print(pdf_name + ' is damaged or corrupted')
                sys.exit()
            count += 1
        else:
            continue
        #time.sleep(5*60)
        sleep_time = random.randint(2, 10)
        time.sleep(sleep_time)
    # 如果所有的 PDF 文件都已经被下载并保存，则退出循环    if count == len(pdf_names):
        print('volume '+str(vol) +', issue ' + str(issue) + '已经下载完毕.')
    else:
        print('共需下载 '+str(len(pdf_names))+' 个文件，已下载文件数目:'+ str(count))
```
