# 類別與物件 (Class/Object)

> http://php.net/manual/zh/book.classobj.php

- class_alias
- class_exists
- get_called_class
- get_class_methods
- get_class_vars
- get_class
- get_declared_classes
- get_declared_interfaces
- get_declared_traits
- get_object_vars
- get_parent_class
- is_a
- is_subclass_of
- method_exists
- property_exists
- trait_exists

## class_alias

為一個類別建立別名

````
class Foo
{
  function calc($x, $y)
  {
    return $x + $y;
  }
}

class_alias('Foo', 'Bar');
$a = new Foo();
$b = new Bar();

var_dump($a === $b);
var_dump($a instanceof $b);
var_dump($a instanceof Bar);
var_dump($b instanceof Foo);
````

結果

````
bool(false)
bool(true)
bool(true)
bool(true)
````

## class_exists

檢查類別是否存在

````
spl_autoload_register(function ($class) {
  return spl_autoload($class);
});

if (class_exists('Import')) {
  $class = new Import();
}
````

## get_called_class

取得後期靜態綁定類別

````
class Foo
{
  static function test()
  {
    return get_called_class();
  }
}

class Bar extends Foo { }

echo Foo::test();
echo Bar::test();
````

結果

````
Foo
Bar
````

## get_class_methods

取得類別中所有公開的方法

````
class Foo
{
  public function add($x, $y)
  {
    return $x + $y;
  }

  public function subtraction($x, $y)
  {
    return $x - $y;
  }

  private function multiplication($x, $y)
  {
    return $x * $y;
  }
}

$methods = get_class_methods('Foo');
var_export($methods);
````

結果

````
array (
  0 => 'add',
  1 => 'subtraction',
)
````

## get_class_vars

取得類別中所有公開的屬性

````
class Foo
{
  public $name = 'Kaoru';
  public $sex = 'Male';
  private $add = 'Taiwan';
}

$vars = get_class_vars('Foo');
var_export($vars);
````

結果

````
array (
  'name' => 'Kaoru',
  'sex' => 'Male',
)
````

## get_class

取得類別的名稱

- 內部呼叫

````
class Foo
{
  function className()
  {
    return get_class($this);
  }
}

$c = new Foo();
echo $c->className();
````

- 外部呼叫

````
class Foo
{
  ...
}

$c = new Foo();
echo get_class($c);
````

結果

````
Foo
````

- 抽象類呼叫

````
abstract class Foo
{
  public function __construct()
  {
    echo get_class($this);
    echo get_class();
  }
}

class Bar extends Foo {}
new Bar;
````

結果

````
Bar
Foo
````

- 命名空間下類別呼叫

````
namespace Foo\Bar;
class Baz { }

$baz = new \Foo\Bar\Baz();
echo get_class($baz);
````

結果

````
Foo\Bar\Baz
````

## get_declared_classes

取得所有已定義類別

````
class Foo { }
class Bar { }
class Baz { }

var_export(get_declared_classes());
````

結果

````
array (
  0 => 'stdClass',
  1 => 'Exception',
  ...
  143 => 'Foo',
  144 => 'Bar',
  145 => 'Baz',
)
````

## get_declared_interfaces

取得所有已定義介面

````
interface Foo { }
interface Bar { }

var_export(get_declared_interfaces());
````

結果

````
array (
  0 => 'Traversable',
  1 => 'IteratorAggregate',
  2 => 'Iterator',
  ...
  18 => 'Foo',
  19 => 'Bar',
)
````

## get_declared_traits

取得所有已定義特徵

````
trait Foo { }
trait Bar { }

var_export(get_declared_traits());
````

結果

````
array (
  0 => 'Foo',
  1 => 'Bar',
)
````

## get_object_vars

取得類別內所有的變數與值

````
class Foo 
{
  var $x, $y;
  var $label;

  function setLabel($val)
  {
    $this->label = $val;
  }
}

$class = new Foo();
var_export(get_object_vars($class));

$class->setLabel('PHP');
var_export(get_object_vars($class));
````

結果

````
array (
  'x' => NULL,
  'y' => NULL,
  'label' => NULL,
)

array (
  'x' => NULL,
  'y' => NULL,
  'label' => 'PHP',
)
````

## get_parent_class

取得類別所繼承的父類名

````
class Foo { }
class Bar extends Foo {  }

$class = new Bar();
var_export(get_parent_class($class));
````

結果

````
Foo
````

## interface_exists

檢查介面是否已被定義

````
interface Foo { }
var_export(interface_exists('Foo'));
````

結果 

````
true
````

## is_a

檢查物件是否為該類別所實例化,\
相當於 instanceof

````
class Foo
{
  var $name;  
}
$class = new Foo();
var_export(is_a($class, 'Foo'));

# is_a 相當於
var_export($class instanceof Foo);
````

結果 

````
true
````

## is_subclass_of

檢查類別是否繼承於某特定類別

````
class Foo { }
class Bar extends Foo { }

$class = new Bar();
var_export(is_subclass_of($class, 'Foo'));

# 靜態類別
var_export(is_subclass_of('Bar', 'Foo'));
````

## method_exists

檢查類別是否存在該方法

````
class Foo 
{
  function read()
  {
    return true;
  }

  private function create()
  {
    return true;
  }
}

$class = new Foo();
var_export(method_exists($class, 'read'));

# 靜態類別
var_export(method_exists('Foo', 'create'));
````

## property_exists

檢查類別是否存在該屬性

````
class Foo 
{
  var $a;
  static private $b;
}

$class = new Foo();
var_export(property_exists($class, 'a'));
var_export(property_exists($class, 'b'));

# 靜態類別
var_export(property_exists('Foo', 'a'));
var_export(property_exists('Foo', 'b'));
````

## trait_exists

檢查特徵是否存在

````
trait Foo { }
var_export(trait_exists('Foo'));
````