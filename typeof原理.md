### typeof是如何识别类型的

在JavaScript中，`typeof`操作符作为语言早期提供的类型检测工具是通过硬编码+特征检测进行区分数据类型的。语法产生式如下：
```
UnaryExpression : typeof UnaryExpression
```

在算法过程中，`typeof`操作符首先会对一元表达式（UnaryExpression）求值，并对结果进行处理：

1. 如果是一个引用记录类型（ECMAScript规范类型），并且是一个未能解析成功的引用（通过 [IsUnresolvableReference](https://tc39.es/ecma262/#sec-isunresolvablereference)算法）则返回“undefined”
2. 得到引用记录的值，将其命名为*val*（这个名字并不重要）
3. 尝试判别*val*,对于undefined和null是硬编码写入类型的，分别为“undefined”和“object”
4. 进行特征检测，如果数据的底层表示具有[[StringData]]、[[SymbolData]]、[[BooleanData]] 、[[NumberData]] 、[[BigIntData]]其中之一的内部槽^1^则取其对应的表示形式,如[[StringData]]的类型为“string”
5. 如果具有[[call]]内部槽则类型为“function”
6. 剩下的皆为“object”类型

> 内部槽：底层用来存储数据或方法

> 注：早期的是使用存储在[[PrimitiveValue]]中的具体类型值来断定的，现在的方式只需断定是否具有属性省略了取值的计算成本，为了兼容浏览器代表HTML元素的[[isHtmlDDA]]也将返回“undefined”
