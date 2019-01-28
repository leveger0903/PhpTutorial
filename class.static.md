# 靜態 (Static)

> http://php.net/manual/zh/language.oop5.static.php

- 靜態存取

## 靜態存取

````
class SimpleClass
{
  public static $foo = 'Foo';
}

# 存取靜態變數
echo SimpleClass::$foo;

# 存取靜態變數
$class = new SimpleClass();
echo $class::$foo;
````