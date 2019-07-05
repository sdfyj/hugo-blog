---
title: "用python下载和上传文件"
date: 2019-07-05
categories:
  - 计算机编程
tags:
  - python
slug: python_download_file
---

1、urlretrieve下载
```python
from urllib.request import urlretrieve
url = 'http://**.jpg'
savepath = '/Users/**.jpg'
urlretrieve(url,savepath)
```

2、request下载
```python
def download_file(pdCode, date, savepath):
    url = URLHEAD2+URLCODE1+URLTAIL2.format(pdCode, date)
    local_filename = savepath
    r = requests.get(url)
    with open(local_filename, 'wb') as f:
        f.write(r.content)
    return local_filename
```

3、视频等大文件下载

* 当使用requests的get下载大文件/数据时，建议使用使用stream模式。

* 当把get函数的stream参数设置成False时，它会立即开始下载文件并放到内存中，如果文件过大，有可能导致内存不足。

* 当把get函数的stream参数设置成True时，它不会立即开始下载，当你使用`iter_content`或`iter_lines`遍历内容或访问内容属性时才开始下载。需要注意一点：文件没有下载之前，它也需要保持连接。

* `iter_content`：一块一块的遍历要下载的内容

* `iter_lines`：一行一行的遍历要下载的内容

* 使用上面两个函数下载大文件可以防止占用过多的内存，因为每次只下载小部分数据

```python
def download_file(url):
    local_filename = path+'alpinemacro/'+'-'.join(url.split('/')[3:])
    r = requests.get(url, stream=True)
    with open(local_filename, 'wb') as f:
        for chunk in r.iter_content(chunk_size=1024):
            if chunk: # filter out keep-alive new chunks
                f.write(chunk)
                f.flush()
    return local_filename
```
```python
def download_mp4(url):
    local_filename = path+'alpinemacro/'+'-'.join(url.split('/')[3:])
    print("开始下载"+local_filename)
    r = requests.get(url, stream=True)
 
    with open(local_filename, "wb") as mp4:
        for chunk in r.iter_content(chunk_size=1024):
            if chunk:
                mp4.write(chunk)
                #mp4.flush()
    return "下载结束"+local_filename
```

4、文件上传
```python
def upload(path, filename):
    filepath = os.path.join(path, filename)
    files = {'file': (filename, open(filepath, 'rb'))}
    url = 'http://****:8001/'
    r = requests.post(url, files=files)
    return r
```
服务器端要设置请求处理