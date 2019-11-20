# 異常處理 (Exception)

> http://php.net/manual/en/language.exceptions.php

## 基礎

````
function inverse($x)
{
  if (!$x)
    throw new Exception('Division by zero.');

  return 1/$x;
}

try {
  echo inverse(5) . "\n";
  echo inverse(0) . "\n";
} catch (Exception $e) {
  echo 'Caught exception: ' . $e->getMessage() . "\n";
}

echo 'Continue';
````

輸出結果

````
0.2
Caught exception: Division by zero.
Continue
````

## Finally 

PHP5.5 新增功能,\
不管 try ... catch 是否有成功執行,\
最後一定會執行 finally

````
function inverse($x)
{
  if (!$x)
    throw new Exception('Division by zero.');

  return 1/$x;
}

try {
  echo inverse(5) . "\n";
  echo inverse(0) . "\n";
} catch (Exception $e) {
  echo 'Caught exception: ' . $e->getMessage() . "\n";
} finally {
  echo 'Function inverse done.' . "\n";
}
`````

````
0.2
Caught exception: Division by zero.
Continue
Function inverse done.
````

## 巢狀異常處理

PHP5.5 後的版本支援巢狀異常處理

````
class MyException extends Exception
{

}

class Foo
{
  public function testing()
  {
    try {
      try {
        throw new MyException('Hello');
      } catch (MyException $e) {
        throw $e;
      }
    } catch (Exception $e) {
      var_dump($e->getMessage());
    }
  }
}

$foo = new Foo();
$foo->testing();
````




