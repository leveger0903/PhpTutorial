# 函數處理 (Function Handling)

> http://php.net/manual/en/book.funchand.php

- call_user_func_array
- call_user_func
- forward_static_call_array
- forward_static_call
- func_get_arg
- func_get_args
- func_num_args
- function_exists
- get_defined_function
- register_shutdown_function
- register_tick_function
- unregister_tick_function

## call_user_func_array

呼叫方法, 並把以陣列方式傳入參數

````
function Foo($name, $age)
{
  return "Hello, I am ${name}, My age is ${age}";
}

var_export(call_user_func_array(
  'Foo', 
  ['kaoru', 25]
));
````

結果

````
Hello, I am kaoru, My age is 25
````

## call_user_func

呼叫方法, 並以 reference 方式傳入參數

````
function Foo($name, $age)
{
  return "Hello, I am ${name}, My age is ${age}";
}

var_export(call_user_func(
  'Foo', 
  'kaoru',
  25
));
````

結果

````
Hello, I am kaoru, My age is 25
````

## forward_static_call_array

呼叫物件靜態方法,\
並以陣列方式帶入參數,\
通常用於解決繼承類別的後期靜態綁定問題上.

````
class Foo
{
  public static function calc($x, $y)
  {
    return $x + $y;
  }
}

class Bar extends Foo
{
  public static function calc($x, $y)
  {
    return forward_static_call_array(
      ['Foo', 'calc'],
      [$x, $y]
    );
  }
}

var_export(Bar::calc(10, 15));
````

結果

````
'(Bar)25'
````

比較 call_user_func_array() 與 forward_static_call_array() 差異

````
class Foo
{
  public static function calc($x, $y)
  {
    $total = $x + $y;
    $class = get_called_class();
    return "(${class})${total}";
  }

  public static function calc_static($x, $y)
  {
    $total = $x + $y;
    $class = get_called_class();
    return "(${class})${total}";
  }
}

class Bar extends Foo
{
  public static function calc($x, $y)
  {
    return call_user_func_array(
      ['Foo', 'calc'],
      [$x, $y]
    );
  }

  public static function calc_static($x, $y)
  {
    return forward_static_call_array(
      ['Foo', 'calc'],
      [$x, $y]
    );
  }
}

var_export(Bar::calc(10, 15));
var_export(Bar::calc_static(10, 15));
````

結果

````
(Foo)25
(Bar)25
````

## forward_static_call

呼叫物件靜態方法,\
並以  reference 方式帶入參數,\
通常用於解決繼承類別的後期靜態綁定問題上.

````
class Foo
{
  public static function calc($x, $y)
  {
    return $x + $y;
  }
}

class Bar extends Foo
{
  public static function calc($x, $y)
  {
    return forward_static_call(
      ['Foo', 'calc'],
      $x,
      $y
    );
  }
}

var_export(Bar::calc(10, 15));
````

比較 call_user_func() 與 forward_static_call() 差異

````
class Foo
{
  public static function calc($x, $y)
  {
    $total = $x + $y;
    $class = get_called_class();
    return "(${class})${total}";
  }

  public static function calc_static($x, $y)
  {
    $total = $x + $y;
    $class = get_called_class();
    return "(${class})${total}";
  }
}

class Bar extends Foo
{
  public static function calc($x, $y)
  {
    return call_user_func(
      ['Foo', 'calc'],
      $x,
      $y
    );
  }

  public static function calc_static($x, $y)
  {
    return forward_static_call(
      ['Foo', 'calc'],
      $x,
      $y
    );
  }
}

var_export(Bar::calc(10, 15));
var_export(Bar::calc_static(10, 15));
````

結果

````
(Foo)25
(Bar)25
````

## func_get_arg

取得其中一組方法所帶入的參數

````
function Foo()
{
  $data = [];
  for ($i = 0; $i < func_num_args(); $i++)
    $data[] = func_get_arg($i);
  
  return $data;
}

var_export(Foo(1, 2, 3));
````

結果

````
array (
  0 => 1,
  1 => 2,
  2 => 3,
)
````

## func_get_args

取得所有方法所帶入的參數

````
function Foo()
{
  return func_get_args();
}

var_export(Foo(1, 2, 3));
````

結果

````
array (
  0 => 1,
  1 => 2,
  2 => 3,
)
````

## func_num_args

取得所有方法所帶入的參數的總數

````
function Foo()
{
  return func_num_args();
}

var_export(Foo(1, 2, 3));
````

結果

````
3
````

## function_exists

檢查方法是否存在

````
function Foo() { }
var_export(function_exists('Foo'));
var_export(function_exists('Bar'));
````

結果

````
true
false
````

## get_defined_function

取得所有被定義的內建方法與使用者方法

````
function Foo() { }
function Bar() { }

var_export(get_defined_functions());
````

結果

````
array (
  'internal' => 
  array (
    0 => 'zend_version',
    1 => 'func_num_args',
    2 => 'func_get_arg',
    ...
  ),
  'user' => 
  array (
    0 => 'foo',
    1 => 'bar',
  ),
)
````

## register_shutdown_function

註冊一組在 PHP 中止時執行的函數

````
register_shutdown_function(function() {
  echo 'PHP STOPPED';
});

exit;
````

## register_tick_function

註冊一組在 PHP 每執行一個或 n 個步驟時所執行的函數

- 以函式編程方式撰寫

````
function goTick()
{
  if ($GLOBALS['tickCount'] > 0)
    echo "SETP ${GLOBALS['tickCount']}";
  
  $GLOBALS['tickCount']++;
}

# 執行 n 個步驟觸發一次
declare(ticks=1);
$GLOBALS['tickCount'] = 0;
register_tick_function('goTick', true);

function Foo() { }
function Bar() { }
echo Foo();
echo Bar();
````

結果
````
SETP 1
SETP 2
SETP 3
SETP 4
````

- 以物件導向方式

````
class Tick
{
  var $count = 0;
  public function addCount() 
  {
    $this->count++;
    echo 'STEP ' . $this->count;
  }
}

declare(ticks=1);
$class = new Tick();
register_tick_function(
  [&$class, 'addCount'], 
  true
);

function Foo() { }
function Bar() { }
echo Foo();
echo Bar();
````

結果

````
SETP 0
SETP 1
SETP 2
SETP 3
SETP 4
````

## unregister_tick_function

解除 PHP 每執行一個或 n 個步驟時所執行的函數

````
function goTick()
{
  if ($GLOBALS['tickCount'] > 0)
    echo "SETP ${GLOBALS['tickCount']}";
  
  $GLOBALS['tickCount']++;
}

# 執行 n 個步驟觸發一次
declare(ticks=1);
$GLOBALS['tickCount'] = 0;
register_tick_function('goTick', true);

function Foo() { }
function Bar() { }
unregister_tick_function('goTick');
echo Foo();
echo Bar();
````