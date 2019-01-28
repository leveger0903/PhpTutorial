# 常數 (Const)

> http://php.net/manual/zh/language.oop5.constants.php

## 類別屬性 (attribute)

````
class MyClass
{
  const mix = 'constant value';
}
  
# 存取常量方法
echo MyClass::mix;
  
# 存取常量方法
$class = new MyClass();
echo $class::mix;
````