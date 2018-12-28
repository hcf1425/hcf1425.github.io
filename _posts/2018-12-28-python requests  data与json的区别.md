# python requests 模块data与json参数区别
requests.post()方法 源码如下：
~~~
def post(url, data=None, json=None, **kwargs):
    r"""Sends a POST request.

    :param url: URL for the new :class:`Request` object.
    :param data: (optional) Dictionary (will be form-encoded), bytes, or file-like object to send in the body of the :class:`Request`.
    :param json: (optional) json data to send in the body of the :class:`Request`.
    :param \*\*kwargs: Optional arguments that ``request`` takes.
    :return: :class:`Response <Response>` object
    :rtype: requests.Response
    """

    return request('post', url, data=data, json=json, **kwargs)
~~~
可以看到，参数中明确的参数有`data`与`json`。
**注意**： `data`与`json`既可以是`str`，也可以是`dict`
 区别如下：
- 不管`json`是`str`还是`dict`，如果不指定`headers`中的`content-type`，默认为`application/json` 
-  `data`为`dict`时，如果不指定`content-type`，默认为`application/x-www-form-urlencoded`，相当于普通form表单提交的形式，此时数据可以从`request.POST`里面获取，而`request.body`的内容则为`a=1&b=2`的这种形式，注意，即使指定`content-type=application/json`，request.body的值也是类似于`a=1&b=2`，所以并不能用`json.loads(request.body.decode())`得到想要的值
-  `data`为`str`时，如果不指定`content-type`，默认为`application/json`
`content-type=application/json`下，获取值的方法如下：
```
json.loads(request.body.decode())
```
---
layout:     post                    # 使用的布局（不需要改）
<!-- title:      python request data              # 标题 
subtitle:   Hello World, Hello Blog #副标题 -->
<!-- date:       2017-02-06              # 时间
author:     BY                      # 作者 -->
<!-- header-img: img/post-bg-2015.jpg    #这篇文章标题背景图片 -->
catalog: true                       # 是否归档
tags:                               #标签
    - python
    - requests
---

  

 

 

 