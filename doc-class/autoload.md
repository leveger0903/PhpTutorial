# 自動加載 (Autoload)

> http://php.net/manual/zh/language.oop5.autoload.php

- 自動加載
- 自動加載與異常處理

## 自動加載

````
# index.php
spl_autoload_register(function ($classname) {
  require_once $classname . '.php';
});

$class = new Fm\Foo();

# Fm\Foo.php
namespace Fm;

class Foo
{
  function __construct()
  {
    echo 'initialized foo.';
  }
}
````

## 自動加載 (with try/catch)

````
spl_autoload_register(function ($classname) {
  $classname = $classname . '.php';
  if (is_readable($classname))
    require_once $classname . '.php';
  else
    throw new Exception('Unable to read:' . $classname);
});

try {
  $class = new Fm\Bamboo();
} catch (Exception $e) {
  echo $e->getMessage();
}
````

