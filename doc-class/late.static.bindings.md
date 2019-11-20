# 後期靜態綁定 (Late Static Bindings)

> http://php.net/manual/zh/language.oop5.late-static-bindings.php

當父類、子類均有相同的方法(A)時,\
使用當呼叫任一父類方法時,\
內含 self 或 static 作用域,\
透過 self 作用域將調用父類方法(A),\
透過後期靜態綁定 static 作用域則是調用子類方法(A)

## self 作用域結果

````
class Foo
{
  public static function who()
  {
    echo __CLASS__;
  }

  public static function test()
  {
    self::who();
  }
}

class Bar extends Foo
{
  public static function who()
  {
    echo __CLASS__;
  }
}

Bar::test();
````

結果

````
Foo
````

## static 作用域結果

````
class Foo
{
  public static function who()
  {
    echo __CLASS__;
  }

  public static function test()
  {
    static::who();
  }
}

class Bar extends Foo
{
  public static function who()
  {
    echo __CLASS__;
  }
}

Bar::test();
````

結果

````
Bar
````

## 目標解析

後期靜態綁定的解析會一直取得到完全解析的靜態調用為止.\
如果作用域為 parent 或 self 將會轉發調用訊息.

````
class Foo
{
  public static function pass()
  {
    static::who();
  }

  public static function who()
  {
    echo __CLASS__ . "\n";
  }
}

class Bar extends Foo
{
  public static function test()
  {
    Foo::pass();
    parent::pass();
    self::pass();
  }

  public static function who()
  {
    echo __CLASS__ . "\n";
  }
}

class Mix extends Bar
{
  public static function who()
  {
    echo __CLASS__ . "\n";
  }
}

Mix::test();
````

結果

````
Foo
Mix
Mix
````