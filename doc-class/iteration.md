# 物件遍歷 (Iterations)

> http://php.net/manual/zh/language.oop5.iterations.php

PHP5 提供一種方法可以將物件進行遍歷,\
類似於 foreach,\
預設情況會遍歷所有可見屬性.

## 基本使用 (透過物件內部方法)

````
class Foo
{
  public $var1 = 'value 1';
  public $var2 = 'value 2';
  public $var3 = 'value 3';

  protected $protected = 'protected var';
  private $private = 'private var';

  function iterateVisible () {
    echo "Foo::iterateVisible:\n";
    foreach ($this as $key => $value) {
      echo sprintf(
        "%s => %s\n",
        $key,
        $value
      );
    }
  }
}

$class = new Foo();
$class->iterateVisible();
````

結果

````
Foo::iterateVisible:
var1 => value 1
var2 => value 2
var3 => value 3
protected => protected var
private => private var
````

## 基本使用 (透過外部方式)

````
class Foo
{
  public $var1 = 'value 1';
  public $var2 = 'value 2';
  public $var3 = 'value 3';

  protected $protected = 'protected var';
  private $private = 'private var';

  function iterateVisible () {
    echo "Foo::iterateVisible:\n";
    foreach ($this as $key => $value) {
      echo sprintf(
        "%s => %s\n",
        $key,
        $value
      );
    }
  }
}

$class = new Foo();
foreach ($class as $key => $value) {
  echo sprintf(
    "%s => %s\n",
    $key,
    $value
  );
}
````

結果

````
var1 => value 1
var2 => value 2
var3 => value 3
````

## 實作 Interator 介面物件遍歷

````
class MyIterator implements Iterator
{
  private $var = [];

  public function __construct($array)
  {
    if (is_array($array))
      $this->var = $array;
  }

  public function rewind()
  {
    echo "rewinding\n";
    reset($this->var);
  }

  public function current()
  {
    $var = current($this->var);
    echo "current: $var\n";
    return $var;
  }

  public function key()
  {
    $var = key($this->var);
    echo "key: $var\n";
    return $var;
  }

  public function next()
  {
    $var = next($this->var);
    echo "next: $var\n";
    return $var;
  }

  public function valid()
  {
    $var = $this->current() !== false;
    echo "valid: {$var}\n";
    return $var;
  }
}


$values = [1, 2, 3];
$it = new MyIterator($values);

foreach ($it as $a => $b) {
  echo "$a: $b\n";
}
````

結果

````
rewinding
current: 1
valid: 1
current: 1
key: 0
0: 1
next: 2
current: 2
valid: 1
current: 2
key: 1
1: 2
next: 3
current: 3
valid: 1
current: 3
key: 2
2: 3
next: 
current: 
valid: 
````

## 利用 IteratorAggregate 實作 Iterator 方法

````
class MyIterator implements Iterator
{
  private $var = [];

  public function __construct($array)
  {
    if (is_array($array))
      $this->var = $array;
  }

  public function rewind()
  {
    echo "rewinding\n";
    reset($this->var);
  }

  public function current()
  {
    $var = current($this->var);
    echo "current: $var\n";
    return $var;
  }

  public function key()
  {
    $var = key($this->var);
    echo "key: $var\n";
    return $var;
  }

  public function next()
  {
    $var = next($this->var);
    echo "next: $var\n";
    return $var;
  }

  public function valid()
  {
    $var = $this->current() !== false;
    echo "valid: {$var}\n";
    return $var;
  }
}

class MyCollection implements IteratorAggregate
{
  private $items = array();
  private $count = 0;

  public function getIterator()
  {
    return new MyIterator($this->items);
  }

  public function add($value)
  {
    $this->items[$this->count++] = $value;
  }
}

$colleciton = new MyCollection();
$colleciton->add('value 1');
$colleciton->add('value 2');
$colleciton->add('value 3');

foreach ($colleciton as $key => $value) {
  echo "key/value: [$key -> $value]\n\n";
}
````

結果

````

rewinding
current: value 1
valid: 1
current: value 1
key: 0
key/value: [0 -> value 1]

next: value 2
current: value 2
valid: 1
current: value 2
key: 1
key/value: [1 -> value 2]

next: value 3
current: value 3
valid: 1
current: value 3
key: 2
key/value: [2 -> value 3]

next: 
current: 
valid: 
````