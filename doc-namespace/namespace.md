# 命名空間 (Namespace)

> http://php.net/manual/zh/language.namespaces.php

## 基本使用

````
# class/fm/foo.php;
namespace Fm;

class Foo
{
  function __construct()
  {
    // ...
  }
}

# index.php
require 'class/fm/foo.php';

$class = new Fm\Foo();
````

## 命名空間解析

- 非限定名稱(本身的檔案上)

````
# index.php
namespace Foo\Bar;

function foo() 
{ 
  return 'Mix';
}

echo foo();
````

- 限定名稱(相對路徑)

````
# require.php
namespace Foo\Bar\subnamespace;

function foo() 
{ 
  return 'Mix';
}

# index.php, 以 Foo\Bar 為基準進行相對路徑解析
namespace Foo\Bar;
require 'require.php';

echo subnamespace\foo();
````

- 完全限定名稱(絕對路徑)

````
# require.php
namespace Foo\Bar\subnamespace;

function foo() 
{ 
  return 'Mix';
}

# index.php
require 'require.php';

echo \Foo\Bar\subnamespace\foo();
````

## 全局訪問

````
namespace Foo;

function strlen($a) {
  return $a;
}

# 調用全局 string 方法(偵測字串長度),
# 而非 Foo\strlen($a)
var_dump(\strlen('Hi'));
````

## 動態類別、方法與常數

- 非命名空間下動態類別、方法與常數

````
# 動態類別
class Foo
{
  function __construct()
  {
    echo __METHOD__ . "\n";
  }
}

$a = 'Foo';
$class = new $a;

# 動態方法
function Bar()
{
  echo __METHOD__ . "\n";
}

$b = 'Bar';
$b();

# 動態常數
const Mix = 'Mix';
echo constant('Mix');
````

結果

````
Foo::__construct
Bar
Mix
````

- 命名空間下動態類別、方法與常數

````
namespace MyNamespace;

# 動態類別
class Foo
{
  function __construct()
  {
    echo __METHOD__ . "\n";
  }
}

$a = 'MyNamespace\Foo';
$class = new $a;

# 動態方法
function Bar()
{
  echo __METHOD__ . "\n";
}

$b = 'MyNamespace\Bar';
$b();

# 動態常數
const Mix = 'Mix';
echo constant('MyNamespace\Mix');
````

結果

````
MyNamespace\Foo::__construct
MyNamespace\Bar
Mix
````

## 取得 namespace

````
namespace MyNamespace;
echo __namespace__ ;
````

結果

````
MyNamespace
````

## 動態實例化類別

````
namespace MyNamespace;

function get($classname)
{
  $class = sprintf(
    '%s\%s', 
    __NAMESPACE__, 
    $classname
  );

  return new $class;
}

# 以下這行相當於 $class = new MyNamespace\Foo;
$class = get('Foo');
````

## namespace 操作符

相同的檔案下,\
\`namespace\` 關鍵字相當於該命名空間

````
namespace MyNamespace;

function Foo()
{
  echo __NAMESPACE__;
}

# 以下這行相當於 echo MyNamespace\Foo();
echo namespace\Foo();
````

## 全局空間

當命名空間下有與全局空間相同的名稱時,\
在方法前加上 \ 將視為全局方法.

````
namespace MyNamespace;

function rand()
{
  # \rand(); 為全局的 rand()
  return 'Rand:' . \rand(1, 1000);
}

echo namespace\rand();
````

相同地,\
命名空間也可以相同的方式訪問全局類別.

````
namespace MyNamespace;

function Foo($no) 
{
  # 異常處理 \MyNamespace\Exception
  if ($no < 0)
    throw new Exception('minus');

  # 異常處理 \Exception
  if ($no >= 0)
    throw new \Exception('plus');
}

class Exception extends \Exception 
{
  public function getFullMessage()
  {
    return 'Error: ' . $this->getMessage();
  }  
}

try {

  $input = namespace\Foo(-100);
  $input = namespace\Foo(100);

} catch (Exception $e) {

  # 異常處理 \MyNamespace\Exception
  echo $e->getFullMessage();

} catch (\Exception $e) {

  # 異常處理 \Exception
  echo $e->getMessage();

}
````

## 方法與常量引用

對於方法與常量來說,\
當前的命名空間不存在該方法或常量時,\
PHP 會改使用全局空間中的函數與常量.

````
namespace MyNamespace;

const E_ERROR = 45;

# 在 MyNamespace 中有找到該常量, 輸出 45
echo E_ERROR;

# 在 MyNamespace 未找到該常量, 改用全局 INI_ALL 常量 7
echo INI_ALL;
````