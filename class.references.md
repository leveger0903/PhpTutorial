# 對象和引用 (Objects And References)

> http://php.net/manual/en/language.oop5.references.php

PHP5 的物件預設是透過引用傳遞內容,\
但更正確說法是兩個參數為別名,\
但指向到相同的內容.

## 識別符拷貝

````
class Foo
{
  public $bar = 1;
}

$a = new Foo;
$b = $a;

$b->bar = 2;
echo $a->bar;
````

結果

````
2
````

## 引用

````
class Foo
{
  public $bar = 1;
}

$a = new Foo;
$b = &$a;

$b->bar = 2;
echo $a->bar;
````

結果

````
2
````

## 方法引用

````
class Foo
{
  public $bar = 1;
}

function bar($obj)
{
  $obj->bar = 2;
}

$a = new Foo;
bar($a);
echo $a->bar;
````

結果

````
2
````