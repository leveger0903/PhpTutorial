# 類別 (Class)

> http://php.net/manual/zh/language.oop5.basic.php

## 類別基礎

- 基本使用
- 靜態調用
- 實例化 (instance)
- 複製物件
- 匿名方法
- 繼承
- 解析類別 (::class)

## 基本使用

````
class SimpleClass
{
  public $foo = 'test';
  public function bar() {
    return $this->foo;
  }
}
````

## 靜態調用

````
class SimpleClass
{
  public function foo()
  {
    echo 'hello';
  }
}

class Foo
{
  function __construct()
  {
    SimpleClass::foo();
  }
}

SimpleClass::foo();
````

## 實例化 (instance)

````
class SimpleClass
{
  public function foo()
  {
    echo 'hello';
  }
}

$class = new SimpleClass();
$class->foo();
````

## 複製物件

````
class SimpleClass
{
  public function hello()
  {
    return 'hello';
  }
}
  
$class = new SimpleClass();
  
# reference
$foo = $class;

# clone
$bar = new $class;

# clone (複製部分物件)
$mix = SimpleClass::hello();
var_dump($mix instanceof SimpleClass);
````

## 匿名方法

````
class SimpleClass
{
  public function hello()
  {
    return 'hello';
  }
}
  
echo (new SimpleClass)->hello();
````

## 匿名方法

````
class SimpleClass
{
  public $bar;
  public function __construct()
  {
    $this->bar = function() {
      return 'hello';
    };
  }
}
  
$class = new SimpleClass();
echo ($class->bar)();
````

## 繼承

````
class ExtendClass extends SimpleClass
{
  # 改寫父層的方法
  function foo()
  {
    echo 'foo';
    parent::foo();
  }
}

$class = new ExtendClass();
$class->foo();
````

## 解析類別 (::class)

````
namespace NS 
{
  class ClassName 
  {
    // ...
  }
    
  echo ClassName::class; // NS\ClassName
}
````