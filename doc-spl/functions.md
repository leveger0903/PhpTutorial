# SPL函式庫 (SPL Functions)

> http://php.net/manual/zh/ref.spl.php

- class_implements
- class_parents
- class_uses
- iterator_apply
- iterator_count
- iterator_to_array
- spl_autoload_call
- spl_autoload_extensions
- spl_autoload_functions
- spl_autoload_register
- spl_autoload_unregister
- spl_autoload
- spl_classes
- spl_object_hash
- spl_object_id

## class_implements

輸出該類別所使用的物件

````
interface Foo {}
interface Bar {}
class Mix implements Foo, Bar {}
  
echo '<pre>';
var_export(class_implements('Mix'));
echo '</pre>';
````

結果

````
array (
  'Foo' => 'Foo',
  'Bar' => 'Bar',
)
````

## class_parents

取得物件所繼承的所有父類別

````
class Foo {}
class Bar extends Foo {}
class Mix extends Bar {}
  
echo '<pre>';
var_export(class_parents('Mix'));
echo '</pre>';
````

結果

````
array (
  'Foo' => 'Foo',
  'Bar' => 'Bar',
)
````

## class_uses

取得物件所使用的特徵機制

````
trait Foo {}
trait Bar {}
class Mix
{
  use Foo, Bar;
}

echo '<pre>';
var_export(class_uses('Mix'));
echo '</pre>';
````

結果

````
array (
  'Foo' => 'Foo',
  'Bar' => 'Bar',
)
````

## iterator_apply

為疊代器中的每個元素調用自定義方法

````
$it = new ArrayIterator([
  'Apples',
  'Bananas',
  'Cherries'
]);

iterator_apply($it, function(Iterator $iterator) {
  echo strtoupper($iterator->current()) . "\n";
  return true;
}, [$it]);
````

結果

````
APPLES
BANANAS
CHERRIES
````

## iterator_count

計算疊代器中元素總個數

````
$it = new ArrayIterator([
  'Apples',
  'Bananas',
  'Cherries'
]);

echo iterator_count($it);
````

結果

````
3
````

## iterator_to_array

將疊代器中元素轉成陣列,\
第二組參數用於是否將元素鍵當作索引

````
$it = new ArrayIterator([
  'red' => 'Apples',
  'yellow' => 'Bananas',
  'purple' => 'Cherries'
]);

var_dump(iterator_to_array($it, 1));
````

結果

````
array(3) {
  ["red"]=>
  string(6) "Apples"
  ["yellow"]=>
  string(7) "Bananas"
  ["purple"]=>
  string(8) "Cherries"
}
````

## spl_autoload_call

嘗試調用所有已註冊於 __autoload() 函數來掛載請求類別

````
# import.php
class Import
{
  function calc($x, $y)
  {
    return $x + $y;
  }
}

# index.php
spl_autoload_call('Import');
echo Import::calc(10, 15);
````

## spl_autoload_extensions

設定 __autoload() 允許的擴展文件類型

````
# import.php
class Import
{
  ...
}

# index.php
spl_autoload_extensions('.inc,.lib');
spl_autoload_call('Import');
echo Import::calc(10, 15);
````

結果

````
Fatal error: Uncaught Error: Class 'Import' not found
````

## spl_autoload_functions

取得 __autoload() 使用的方法名稱

````
function init($c)
{
  require $c . '.php';
}

spl_autoload_register('init');
var_dump(spl_autoload_functions());
````

結果

````
array(1) {
  [0]=>
  string(4) "init"
}
````

## spl_autoload_register

實例化或是靜態調用該類別時,\
執行 _autoload() 所註冊好的方法.\
(通常是自動載入類別)

````
spl_autoload_register(function ($c) {
  require $c . '.php';
});

echo Import::calc(10, 15);
$c = new Import();
````

## spl_autoload_unregister

解除 _autoload() 所註冊好的方法.\
(通常是自動載入類別)

````
spl_autoload_register(function ($c) {
  require $c . '.php';
});
spl_autoload_register(function ($c) {
  var_export($c);
});

foreach (spl_autoload_functions() as $key => $val)
  spl_autoload_unregister($val);
````

## spl_autoload

預設實作 _autoload() 方法,\
當 spl_autoload_register() 未定義參數時,\
預設使用 spl_autoload() 方法.

````
# import.php
class Import
{
  function calc($x, $y)
  {
    return $x + $y;
  }
}

# index.php
spl_autoload_register(function ($class) {
  return spl_autoload($class);
});

$c = new Import();
echo $c->calc(20, 15);
````

## spl_classes

返回所有可用的 SPL 類別

````
var_export(spl_classes());
````

結果

````
array (
  'AppendIterator' => 'AppendIterator',
  'ArrayIterator' => 'ArrayIterator',
  'ArrayObject' => 'ArrayObject',
  'BadFunctionCallException' => 'BadFunctionCallException',
  'BadMethodCallException' => 'BadMethodCallException',
  'CachingIterator' => 'CachingIterator',
  'CallbackFilterIterator' => 'CallbackFilterIterator',
  'Countable' => 'Countable',
  'DirectoryIterator' => 'DirectoryIterator',
  'DomainException' => 'DomainException',
  'EmptyIterator' => 'EmptyIterator',
  'FilesystemIterator' => 'FilesystemIterator',
  'FilterIterator' => 'FilterIterator',
  'GlobIterator' => 'GlobIterator',
  'InfiniteIterator' => 'InfiniteIterator',
  'InvalidArgumentException' => 'InvalidArgumentException',
  'IteratorIterator' => 'IteratorIterator',
  'LengthException' => 'LengthException',
  'LimitIterator' => 'LimitIterator',
  'LogicException' => 'LogicException',
  'MultipleIterator' => 'MultipleIterator',
  'NoRewindIterator' => 'NoRewindIterator',
  'OuterIterator' => 'OuterIterator',
  'OutOfBoundsException' => 'OutOfBoundsException',
  'OutOfRangeException' => 'OutOfRangeException',
  'OverflowException' => 'OverflowException',
  'ParentIterator' => 'ParentIterator',
  'RangeException' => 'RangeException',
  'RecursiveArrayIterator' => 'RecursiveArrayIterator',
  'RecursiveCachingIterator' => 'RecursiveCachingIterator',
  'RecursiveCallbackFilterIterator' => 'RecursiveCallbackFilterIterator',
  'RecursiveDirectoryIterator' => 'RecursiveDirectoryIterator',
  'RecursiveFilterIterator' => 'RecursiveFilterIterator',
  'RecursiveIterator' => 'RecursiveIterator',
  'RecursiveIteratorIterator' => 'RecursiveIteratorIterator',
  'RecursiveRegexIterator' => 'RecursiveRegexIterator',
  'RecursiveTreeIterator' => 'RecursiveTreeIterator',
  'RegexIterator' => 'RegexIterator',
  'RuntimeException' => 'RuntimeException',
  'SeekableIterator' => 'SeekableIterator',
  'SplDoublyLinkedList' => 'SplDoublyLinkedList',
  'SplFileInfo' => 'SplFileInfo',
  'SplFileObject' => 'SplFileObject',
  'SplFixedArray' => 'SplFixedArray',
  'SplHeap' => 'SplHeap',
  'SplMinHeap' => 'SplMinHeap',
  'SplMaxHeap' => 'SplMaxHeap',
  'SplObjectStorage' => 'SplObjectStorage',
  'SplObserver' => 'SplObserver',
  'SplPriorityQueue' => 'SplPriorityQueue',
  'SplQueue' => 'SplQueue',
  'SplStack' => 'SplStack',
  'SplSubject' => 'SplSubject',
  'SplTempFileObject' => 'SplTempFileObject',
  'UnderflowException' => 'UnderflowException',
  'UnexpectedValueException' => 'UnexpectedValueException',
)
````

## spl_object_hash

返回指定物件的 hash id

````
spl_autoload_register(function ($class) {
  return spl_autoload($class);
});

$c = new Import();
var_dump(spl_object_hash($c));
````

結果

````
string(32) "000000001f25df0600000000462171f8"
````

## spl_object_id

返回指定物件的 編號

````
spl_autoload_register(function ($class) {
  return spl_autoload($class);
});

$c = new Import();
var_dump(spl_object_id($c));
````

結果

````
int(2)
````