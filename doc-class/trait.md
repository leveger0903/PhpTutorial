# 特徵 (Trait)

> http://php.net/manual/zh/language.oop5.traits.php

有兩組類別需要繼承特定類別部分變數與方法,\
而非全部繼承時,\
可使用特徵繼承部分特定類別方法.

介面也可達到同樣作法,\
但缺點是兩個繼承的類別均需要實作同樣的方法,\
此時用特徵是比較好的做法.

## 特徵可以
- 繼承類別部份的變數與方法
- 繼承 public, protected 層級的變數方法
- 繼承多的特徵
- 延伸繼承特徵
- 抽象方法
- 靜態方法與定義屬性

## 特徵不能
- 繼承 private 層級的變數方法

## 基本使用

````
trait Mix 
{
  public function foo()
  {
    // ...
  }

  public function bar()
  {
    // ...
  }
}

class SimpleClass extends Mix
{
  use Mix;
}
````

## 優先級別 : Trait > Parent Class > Trait

````
class Bar
{
  public function sayHello()
  {
    echo 'Hello';
  }
}

trait Mix
{
  public function sayHello()
  {
    echo 'World';
  }
}

class Foo extends Base 
{
  use Mix;
}

$class = new Foo();
$class->sayHello(); // World;
````

## 多個特徵

````
trait Foo
{
  public function name()
  {
    // ...
  }
}

trait Bar
{
  public function habit()
  {
    // ...
  }
}

class SimpleClass
{
  use Foo, Bar;
  public function sexual()
  {
    // ...
  }
}
````

## 同名稱特徵衝突

instandof 選擇保留項目,\
as 將同名稱方法進行更名

````
trait Foo
{
  public function small()
  {
    echo 'Foo::Small';
  }
}

trait Bar
{
  public function small()
  {
    echo 'Bar::Small';
  }
}

class Mix
{
  use Foo, Bar {
    Bar::small insteadof Foo;
    Bar::small as barSmall;
  }
}

$class = new Mix();
$class->small(); // Foo::Small
$class->barSmall(); // Bar::Small
````

## 變更方法層級屬性

可以利用 as 將 trait 內方法可層級進行調整,\
例如將 private 變更為 public 層級

````
trait Foo
{
  private function bar()
  {
    // ...
  }
}

class Mix
{
  use Foo 
  {
    bar as public hello;
  }
}

$class = new Mix();
$class->hello();
````

## 延伸繼承特徵

````
trait Foo
{
  public function name()
  {
    // ...
  }
}

trait Bar
{
  public function birthday()
  {
    // ...
  }
}

trait Mix
{
  use Foo, Bar;
}

class SimpleClass
{
  use Mix;
}
````

## 抽象方法

````
trait Foo
{
  public function bar()
  {
    echo 'Bar';
  }

  abstract public function mix();
}

class SimpleClass {
  use Foo;
  public function mix() 
  {
    return $this->bar();
  }
}
````

## 靜態變數與屬性

````
trait Foo
{
  static $bar = 0;
  public $mix = 'Kaoru';

  public function add()
  {
    self::$bar = self::$bar + 1;
    return self::$bar;
  }

  public function showName()
  {
    return $this->mix; 
  }
}
````