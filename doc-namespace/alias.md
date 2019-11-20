# 別名與導入 (Alias and Importing)

> http://php.net/manual/en/language.namespaces.importing.php

- 類別使用別名
- 接口使用別名
- 命名空間使用別名

## 基本使用

導入類別

````
# import.php
namespace MyNamespace\Import;

class Foo
{
  function __construct()
  {
    return __NAMESPACE__;
  }
}

# index.php
require 'import.php';
use MyNamespace\Import;

$class = new MyNamespace\Import\Foo;
````

以別名方式導入類別

````
# import.php
namespace MyNamespace\Import;

class Foo
{
  function __construct()
  {
    return __NAMESPACE__;
  }
}

# index.php
require 'import.php';
use MyNamespace\Import as Import;

$class = new Import\Foo;
````

導入方法 (PHP5.6+, 可搭配別名)

````
# import.php
namespace MyNamespace\Import 
{
  function showNamespace()
  {
    echo __NAMESPACE__;
  }
}

# index.php
require 'import.php';
use function MyNamespace\Import\showNamespace;

echo MyNamespace\Import\showNamespace();
````

導入常數 (PHP5.6+, 可搭配別名)

````
# import.php
namespace MyNamespace\Import 
{
  const Foo = '123';
}

# index.php
require 'import.php';
use const MyNamespace\Import\Foo;

echo MyNamespace\Import\Foo;
````

數組導入(也可以 usr 分拆多行)

````
# import.php
namespace MyNamespace\Import 
{
  const Foo = '123';
  const Bar = '456';
}

# index.php
require 'import.php';
use const MyNamespace\Import\Foo, MyNamespace\Import\Bar;

echo MyNamespace\Import\Foo;
echo MyNamespace\Import\Bar;
````

## 動態導入

````
# import.php
namespace MyNamespace;

class Foo
{
  function __construct()
  {
    return __NAMESPACE__;
  }
}

# index.php
require 'import.php';
use MyNamespace\Foo as Mine;

$a = 'MyNamespace\Foo';
$class = new $a;
````

## 類別中使用 use 為特徵機制

類別中使用 use 為特徵機制,\
但可於類別之外使用 use

````
# import.php
namespace MyNamespace\Import;

class Foo
{
  function showNamespace()
  {
    return __NAMESPACE__;
  }
}

# index.php
require 'import.php';
use MyNamespace\Import\Foo as Foo;

class Bar extends Foo
{
  function showClass()
  {
    return __CLASS__;
  }
}

$class = new Bar;
var_dump($class->showNamespace());
var_dump($class->showClass());
````