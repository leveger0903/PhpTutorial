# 類別複製 (Clone)

> http://php.net/manual/zh/language.oop5.cloning.php

複製物件時,\
觸發 __clone() 方法並定義自訂行為.

## 基本使用

````
class Foo
{
  public $count = 1;
  public function __clone()
  {
    $this->count++;
  }
}

$a = new Foo;
var_dump($a);
$b = clone $a;
var_dump($b);
var_dump($a === $b);
````

結果

````
object(Foo)#1 (1) {
  ["count"]=>
  int(1)
}

object(Foo)#2 (1) {
  ["count"]=>
  int(2)
}

bool(false)
````

## 類別屬性為類別時

類別的屬性是另一類別時,\
複製類別後其該屬性僅參考相同類別.

````
class Foo
{
  public $obj;
  
  function __construct()
  {
    $this->obj = new Bar;
  }
}

class Bar
{
  public $name = 'KT';
}

$a = new Foo;
$b = clone $a;
$a->obj->name = 'AK';
var_dump($a->obj->name);
var_dump($b->obj->name);
var_dump($a->obj === $b->obj);
````

結果

````
string(2) "AK"
string(2) "AK"
bool(true)
````

透過 __clone 一併複製類別內子屬性類別

````
class Foo
{
  public $obj;
  function __construct()
  {
    $this->obj = new Bar;
  }

  function __clone()
  {
    $this->obj = clone $this->obj;
  }
}

class Bar
{
  public $name = 'KT';
}

$a = new Foo;
$b = clone $a;
$a->obj->name = 'AK';
var_dump($a->obj->name);
var_dump($b->obj->name);
var_dump($a->obj === $b->obj);
````

結果

````
string(2) "AK"
string(2) "KT"
bool(false)
````