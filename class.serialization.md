# 序列化對象 (Serialization)

> http://php.net/manual/zh/language.oop5.serialization.php

透過序列化將物件當前執行的狀態儲存,\
再透過反序列化讀取上一個儲存的狀態.

## 範例

````
# require.php
class Foo
{
  public $name;
  public $address;

  public function readProperties()
  {
    return [
      'name' => $this->name,
      'address' => $this->address,
    ];
  }
}

# index.php
require('require.php');

$a = new Foo();
$a->name = 'KT';
$a->address = 'TPE';

$s = serialize($a);
file_put_contents('store', $s);

# read.php
require('require.php');

$s = file_get_contents('store');
$b = unserialize($s);

var_export($b->readProperties());
````

結果

````
array ( 'name' => 'KT', 'address' => 'TPE', )
````