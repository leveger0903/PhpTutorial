# 作用域 (Scope)

> http://php.net/manual/zh/language.oop5.paamayim-nekudotayim.php

- 類外部使用::操作符
- 類內部使用::操作符
- 調用父類::操作符

## 類外部使用::操作符

````
class SimpleClass
{
  public static $foo = 'Foo';
}

# 外部調用
echo SimpleClass::$foo;
````

## 類內部使用::操作符

````
class SimpleClass
{
  public static $foo = 'Foo';
  public function response()
  {
    # 內部調用
    return self::$foo;
  }
}
````

## 調用父類::操作符

````
class SimpleClass
{
  public static $foo = 'Foo';
}

class ExtendClass extends SimpleClass
{
  public function response()
  {
    # 調用父類
    return parent::$foo;
  } 
}
````