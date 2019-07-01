---
title: "文本挖掘：用python中WordCloud制作词云"
date: 2019-06-21
categories:
  - 计算机编程
tags:
  - python
  - 文本挖掘
slug: worldcloud
---


[WordCloud说明文档](http://amueller.github.io/word_cloud/generated/wordcloud.WordCloud.html)

[下载地址](https://pypi.org/project/wordcloud/)


例子：

```python
import jieba   
import numpy as np  
import matplotlib.pyplot as plt                          
from PIL import Image                                                                            
from wordcloud import WordCloud, ImageColorGenerator,get_single_color_func

text = open('最好的我们.txt',"r").read()

cut_text= jieba.cut(text)
result= "/".join(cut_text)

image = Image.open('heart.jpg')
graph = np.array(image)

wc = WordCloud(font_path='msyh.ttf',background_color='white',
               width=1600,height=1200,max_font_size=1000,mask=graph,max_words=100000).generate(result)

backgroud_Image =  plt.imread('heart.jpg') 
color_func = ImageColorGenerator(backgroud_Image )  
#color_func=get_single_color_func('#EE00EE')
wc.recolor(color_func=color_func)


plt.figure("wordcloud") 
plt.imshow(wc)      
plt.axis("off")     
plt.show()

wc.to_file('heart.png')
```

![](/images/heart.png)