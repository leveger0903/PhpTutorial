# 抽象類別 (Abstract Class)

> http://php.net/manual/zh/language.oop5.abstract.php

多個類別有共用方法或屬性時,\
可將共用方法寫成抽象類別,\
並讓子類別繼承該抽象類別

## 抽象類別可以

- attribute
- const
- method
- 繼承 1 個父類

## 抽象類別不能

- 實作抽象方法
- instance

## 基本使用

````
abstract class Foo
{
  # 抽象方法
  abstract public function getTel($no);
  abstract public function getAddress($add);

  # 實作方法
  public function report($no, $add)
  {
    echo sprintf(
     'Tel: %s, Address: %s.',
     $this->getTel($no),
     $this->getAddress($add)
    );
  }
}

class Bar extends Foo
{
  public function getTel($no)
  {
    return $no;
  }

  public function getAddress($add)
  {
    return $add;
  }
}

$class = new Bar();
$class->report(10888, 'Taichung');
````

## 類別屬性 (attribute)

````
abstract class Foo
{
  public $bar = 'property'; 
  public static $mix = 'property';
}
````

## 類別常數 (const)

````
abstract class Foo
{
  const Bar = 'property';
}
````

## 實作方法 (methods)

````
abstract class Foo
{
  public function bar()
  {
    // ...
  }
}
````

## 繼承父類別

````
# parent class
abstract class SimpleClass
{
  public function bar()
  {
    // ...
  }
}

# child class
class ExtendClass extends SimpleClass
{
  // ...
}

$extended = new ExtendClass();
$extended->bar();
````