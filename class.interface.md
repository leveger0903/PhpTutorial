# 介面 (Interface)

> http://php.net/manual/zh/language.oop5.interfaces.php

多個類別有共同方法但實作有差異,\
可將共用方法寫成介面,\
並讓子類別去繼承該介面

## 介面可以

- const
- abstract
- 定義 multi interface
- 延伸 interface
- 實作多個 interface

## 介面不能

- attribute
- method
- instance

## 基本使用

````
interface Foo
{
  public function foo($num);
  public function bar($num);
}

interface Bar
{
  public function calc();
}

class SampleClass implements Foo, bar
{
  public static $num;
  public static $mix;
  public function foo($num) {
    SampleClass::$num = $num;
  }

  public function bar($num) {
    SampleClass::$mix = $num;
  }

  public function calc() {
    return SampleClass::$num + SampleClass::$mix;
  }
}

$class = new SampleClass();
$class->foo(3);
$class->bar(5);
echo $class->calc();
````

## 類別常數 (const)

````
interface class Foo
{
  const Bar = 'property';
}
````

## 抽象方法 (abstract)

````
interface iTemplate
{
  # 介面的抽象方法不需要加上 abstract 前綴
  public function foo($mix);
  public function bar($var);
}

class SimpleClass implements iTemplate
{
  private $vars = array(); 
 
  public function foo($mix) {
    // ...
  }

  public function bar($var) {
    // ...
  }
}
````

## 延伸介面 (extend interface)

````
interface iParent
{
  public function foo($mix);
}

interface iTemplate extends iParent
{
  public function bar();
}

class SimpleClass implements iTemplate
{
  public function foo($mix)
  {
    // ...
  }

  public function bar()
  {
    // ...
  }
}
````

## 實作多個介面 (Multiple Interface)

````
interface iParent
{
  public function foo($mix);
}

interface iTemplate
{
  public function bar();
}

class SimpleClass implements iParent, iTemplate
{
  public function foo($mix)
  {
    // ...
  }

  public function bar()
  {
    // ...
  }
}
````
