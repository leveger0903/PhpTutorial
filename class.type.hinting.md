# 型態約束 (Type Hinting)

> http://php.net/manual/en/language.oop5.typehinting.php

方法可透過型態約束指定帶入參數的方法型態,
若不符合即跳出錯誤.

## 支援類型(支援版本)

- class(5.0, Class 名稱)
- interface(5.0, Interface 名稱)
- self(5.0)
- array(5.1)
- callable(5.4)
- bool(7.0)
- float(7.0)
- int(7.0)
- string(7.0)
- iterable(7.1)
- object(7.2)

## 方法範例

````
function Foo(array $bar)
{
  return $bar;
}

$string = '123';
var_dump(Foo($string));
````

結果

````
Fatal error: Uncaught TypeError: Argument 1 passed to Foo() must be of the type array, string given, called in D:\www\index.php on line 9 and defined in D:\www\index.php:3 Stack trace: #0 D:\www\index.php(9): Foo('123') #1 {main} thrown in D:\www\index.php on line 3
````

## 物件範例

````
class Foo
{
  public function setObject(Bar $input)
  {
    return new $input;
  }
}

$string = '123';
$class = new Foo;
$class->setObject($string);
````

結果

````
Fatal error: Uncaught TypeError: Argument 1 passed to Foo::setObject() must be an instance of Bar, string given, called in D:\www\index.php on line 18 and defined in D:\www\index.php:5 Stack trace: #0 D:\www\index.php(18): Foo->setObject('123') #1 {main} thrown in D:\www\index.php on line 5
````