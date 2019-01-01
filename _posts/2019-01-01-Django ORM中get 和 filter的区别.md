# Django ORM中get 和 filter的区别

## 1. get

~~~
Returns the object matching the given lookup parameters, which should be in the format described in Field lookups.

get() raises MultipleObjectsReturned if more than one object was found. The MultipleObjectsReturned exception is an attribute of the model class.

get() raises a DoesNotExist exception if an object wasn’t found for the given parameters. This exception is also an attribute of the model class
~~~

## 2. filter

~~~
Returns a new QuerySet containing objects that do not match the given lookup parameters.

The lookup parameters (**kwargs) should be in the format described in Field lookups below. Multiple parameters are joined via AND in the underlying SQL statement, and the whole thing is enclosed in a NOT().
~~~

## 3. 区别对比

- 输入参数

  get 的参数只能是model中定义的那些字段，只支持严格匹配
  filter 的参数可以是字段，也可以是扩展的where查询关键字，如in，like等

- 返回值

  get 返回值是一个定义的model对象

​	filter 返回值是一个新的QuerySet对象，然后可以对QuerySet在进行查询返回新的QuerySet对象，支持链式操作
	QuerySet一个集合对象，可使用迭代或者遍历，切片等，但是不等于list类型(使用一定要注意)

- 异常

  get 只有一条记录返回的时候才正常,也就说明get的查询字段必须是主键或者唯一约束的字段。当返回多条记录或者是没有找到记录的时候都会抛出异常

  filter 有没有匹配的记录都可以