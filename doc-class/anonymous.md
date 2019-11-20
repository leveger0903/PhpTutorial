# 匿名類別 (Anonymous Class)

> http://php.net/manual/zh/language.oop5.anonymous.php

## PHP 7 以前範例

````
class SimpleClass
{
  public function getValue($input)
  {
    return $input->read(1);
  }
}

class AnonymousClass
{
  public $num = 100;
  public function read($num)
  {
    return $this->num + $num;
  }
}

echo SimpleClass::getValue(new AnonymousClass());
````

## PHP 7 以後範例

````
class SimpleClass
{
  public function getValue($input)
  {
    return $input->read(1);
  }
}

echo SimpleClass::getValue(new class {
  public $num = 100;
  public function read($num)
  {
    return $this->num + $num;
  }
});
````

## 匿名類可使用繼承、介面、特徵機制

````
class SimpleClass
{
  public function Foo()
  {
    return 'Foo';
  }
}

interface SimpleInterface
{
  public function mix();
}

trait FlexTrait
{
  public function bar()
  {
    return 'Bar';
  }
}

var_dump(new Class (100) extends SimpleClass implements SimpleInterface {
  use FlexTrait;
  public $num;
  function __construct($num) 
  {
    $this->num = $num;
  }

  public function mix()
  {
    return $this->num;
  }
});
````

## 匿名類 private / protected 使用

匿名類無法存取外部類的 private / protected 屬性,
因此需要透過繼承該外部類來取得 protected 屬性,
以及傳值取得外部類的 private 屬性.

````
class SimpleClass
{
  private $prop = 1;
  protected $prop2 = 2;

  public function func2()
  {
    return new class($this->prop) extends SimpleClass
    {
      private $prop3;
      public function __construct($prop)
      {
        $this->prop3 = $prop;
      }

      public function func3()
      {
        return $this->prop2 + $this->prop3;
      }
    };
  }
}

echo (new SimpleClass)->func2()->func3();
````

## 匿名類實例化

同一個匿名類, 所創建的物件都是這個匿名類的實例

````
function anonymous_class()
{
  return new class {};
}

get_class(anonymous_class()) === get_class(anonymous_class());
````

## 匿名類名稱

````
get_class(new class{});
````