# Python 修改 pip 为国内源

[TOC]

## 国内源

>pypi 清华大学源：[https://pypi.tuna.tsinghua.edu.cn/simple](https://pypi.tuna.tsinghua.edu.cn/simple)
>pypi 豆瓣源 ：[http://pypi.douban.com/simple/](http://pypi.douban.com/simple/)
>pypi 腾讯源：[http://mirrors.cloud.tencent.com/pypi/simple](http://mirrors.cloud.tencent.com/pypi/simple)
>pypi 阿里源：[http://mirrors.aliyun.com/pypi/simple/](http://mirrors.aliyun.com/pypi/simple/)

## 临时使用

在安装包的时候，后面加上

`-i https://pypi.tuna.tsinghua.edu.cn/simple`

列如：

`pip install you-get -i https://pypi.tuna.tsinghua.edu.cn/simple`


## 永久设置

```bash
# 清华源
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple

# 或：
# 阿里源
pip config set global.index-url https://mirrors.aliyun.com/pypi/simple/
# 腾讯源
pip config set global.index-url http://mirrors.cloud.tencent.com/pypi/simple
# 豆瓣源
pip config set global.index-url http://pypi.douban.com/simple/
```

