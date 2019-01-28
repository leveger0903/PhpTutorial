# Final 關鍵字 (Final Keyword)

> http://php.net/manual/zh/language.oop5.final.php

## Final 方法

父類方法被宣告 final 時,\
所繼承的子類無法覆蓋該方法.

````
class Foo
{
  public function test() 
  {
    echo "Foo::test() called\n";
  }

  final public function more()
  {
    echo "Foo::more() called\n";
  }
}

class Bar extends Foo
{
  public function more()
  {
    echo "Bar::more() called\n";
  }
}
````

結果

````
Fatal error: Cannot override final method Foo::more() in D:\www\index.php on line 22
````

## Final 類別

一個類別被宣告 final 時,\
則不能被繼承.

````
final class Foo
{
  public function test() 
  {
    echo "Foo::test() called\n";
  }
}

class Bar extends Foo
{
  public function more()
  {
    echo "Bar::more() called\n";
  }
}
````

結果

````
Fatal error: Class Bar may not inherit from final class (Foo) in D:\www\index.php on line 17
````