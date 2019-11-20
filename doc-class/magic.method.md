# 魔術方法 (Magic Methods)

> http://php.net/manual/zh/language.oop5.magic.php

- __sleep
- __wakeup
- __toString
- __invoke
- __set_state
- __debugInfo

*下列魔術方法於其他章節*

- construct
- destruct
- call
- callStatic
- get
- set
- isset
- unset
- clone

## __sleep、__wakeup

serialize() 方法會檢查是否存在 __sleep() 魔術方法,\
保留 __sleep() 所返回的屬性並轉換成 string.\
用於清理類不需要的屬性.

unserialize() 方法會檢查是否存在 __wakeup() 魔術方法,\
作用於將透過 serialize() 轉成 string 的物件再次使用,\
類似於喚醒.

> 額外參考: https://my.oschina.net/sallency/blog/629206

````
class Person
{
  private $name, $age, $sex, $info;

  public function __construct($name, $age, $sex)
  {
    $this->name = $name;
    $this->age  = $age;
    $this->sex  = $sex;
    $this->info = sprintf(
      'do construct: %s, %s and %s',
      $this->name,
      $this->age,
      $this->sex
    );
  }

  public function getInfo()
  {
    echo $this->info . PHP_EOL;
  }

  public function __sleep()
  {
    echo __METHOD__ . PHP_EOL;
    return ['name', 'age', 'sex'];
  }

  public function __wakeup()
  {
    echo __METHOD__ . PHP_EOL;
    $this->info = sprintf(
      'do wakeup: %s, %s and %s',
      $this->name,
      $this->age,
      $this->sex
    );
  }
}

$class = new Person('stacy', 25, 'female');
$class->getInfo();
````

結果

````
do construct: stacy, 25 and female
````

````
echo '<pre>';
var_dump($class);
echo '</pre>';
````

結果

````
object(Person)#1 (4) {
  ["name":"Person":private]=>
  string(5) "stacy"
  ["age":"Person":private]=>
  int(25)
  ["sex":"Person":private]=>
  string(6) "female"
  ["info":"Person":private]=>
  string(34) "do construct: stacy, 25 and female"
}
````

````
# 序列化僅保留 name, age, sex 屬性(參考 __sleep())
$temp = serialize($class);
echo $temp . PHP_EOL;
````

結果

````
Person::__sleep O:6:"Person":3:{s:12:"Personname";s:5:"stacy";s:11:"Personage";i:25;s:11:"Personsex";s:6:"female";} stringPerson::__wakeup

````

````
$class = unserialize($temp);

echo '<pre>';
var_dump($class);
echo '</pre>';
````

結果

````
object(Person)#2 (4) {
  ["name":"Person":private]=>
  string(5) "stacy"
  ["age":"Person":private]=>
  int(25)
  ["sex":"Person":private]=>
  string(6) "female"
  ["info":"Person":private]=>
  string(31) "do wakeup: stacy, 25 and female"
}
````

## __toString

透過 __toString() 魔術方法,\
可將物件當作字串輸出.

````
class Foo
{
  public $foo;
  public function __construct($foo)
  {
    $this->foo = $foo;
  }

  public function __toString()
  {
    return $this->foo;
  }
}

$class = new Foo('Yuan');
echo $class;
````

結果

````
Yuan
````

## __invoke

透過 __toString() 魔術方法,\
可將物件當作方法輸出.

````
class Foo
{
  public $foo;
  public function __construct($foo)
  {
    $this->foo = $foo;
  }

  public function __invoke()
  {
    return $this->foo;
  }
}

$class = new Foo('Yuan');
echo $class();
````

結果

````
Yuan
````

## __set_state


透過多個屬性並調用 var_export() 方法時,
__set_state 會調出所輸入的屬性內容

````
class Foo
{
  public static function __set_state($property)
  {
    $obj = new BAR;
    foreach ($property as $key => $val) {
      $obj->data .= "$key : $val";
    }
    return $obj;
  }
}

class BAR
{
  public $data;
}

$class = new Foo;
$class->name = 'KT';
$class->address = 'TPE';

eval('$bar=' . var_export($class, 1) . ';');
var_dump($bar);
````

## __debugInfo

當執行 var_dump() 方法,
將原先的 var_dump() 訊息改寫成自訂訊息

````
class Foo
{
  private $prop;
  public function __construct($prop)
  {
    $this->prop = $prop;
  }

  public function __debugInfo()
  {
    return [
      'prop' => $this->prop,
      'name' => 'KT'
    ];
  }
}

$class = new Foo(100);
var_dump($class);
````

結果

````
object(Foo)#1 (2) {
  ["prop"]=>
  int(100)
  ["name"]=>
  string(2) "KT"
}
````