# python requests 模块data与json参数区别

**提示，requests模块在低于2.4.2版本时不支持使用json关键字方式进行传递参数**

如果想要使用低于2.4.2的requests模块进行参数传递，则只能使用data进行传参：

```
headers = {"Content-Type": "application/json"}
requsts.post(url, data=json.dumps(params),headers=headers)
```

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

  

 

 

 