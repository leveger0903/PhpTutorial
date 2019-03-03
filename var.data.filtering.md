# 資料過濾 (Data Filtering)

> http://php.net/manual/zh/book.filter.php

- filter_has_var
- filter_id
- filter_input_array
- filter_input
- filter_list
- filter_var_array
- filter_var

## filter_has_var

檢查指定接口是否有該變數名稱,\
存在返回 TRUE,\
反之為 FALSE.

````
var_export(filter_has_var(INPUT_GET, 'id'));
var_export(filter_has_var(INPUT_POST, 'name'));
````

## filter_list

返回支援的過濾器列表.

````
var_export(filter_list());
````

結果

````
array (
  0 => 'int',
  1 => 'boolean',
  2 => 'float',
  3 => 'validate_regexp',
  4 => 'validate_domain',
  5 => 'validate_url',
  6 => 'validate_email',
  7 => 'validate_ip',
  ...
)
````

## filter_id

返回所支援的過濾器 ID,
不存在則返回 FALSE.

````
$filters = filter_list();
$list = [];
foreach ($filters as $val)
  $list[$val] = filter_id($val);

var_export($list);
````

結果

````
array (
  'int' => 257,
  'boolean' => 258,
  'float' => 259,
  'validate_regexp' => 272,
  'validate_domain' => 277,
  'validate_url' => 273,
  'validate_email' => 274,
  'validate_ip' => 275,
  ...
)
````

## filter_input

取得指定接口特定變數,
並透過過濾器處理並返回,
不符合返回 false.

````
# name 參數為 'JACK-POT'
$filtered = filter_input(
  INPUT_GET,
  'name',
  FILTER_SANITIZE_SPECIAL_CHARS
);

var_export($filtered);
````

結果

````
\'JACK-POT\'
````

## filter_input_array

取得指定接口所有變數,
並透過過濾器處理並返回,
不符合返回 false.

````
# id 參數為 10
# component[0] 參數為 5
# scalar 參數為 A
# array[0] 參數為 3
# array[1] 參數為 5

$args = [
  'id'  => FILTER_SANITIZE_ENCODED,
  'fake_arg'  => FILTER_VALIDATE_INT,
  'component' => [
    'filter' => FILTER_VALIDATE_INT,
    'flags'  => FILTER_REQUIRE_ARRAY,
    'options' => [
      'min_range' => 1,
      'max_range' => 10,
    ],
  ],
  'scalar' => [
    'filter' => FILTER_VALIDATE_INT,
    'flags'  => FILTER_REQUIRE_SCALAR,
  ],
  'array' => [
    'filter' => FILTER_VALIDATE_INT,
    'flags'  => FILTER_REQUIRE_ARRAY,
  ],
];

$filtered = filter_input_array(
  INPUT_POST,
  $args
);

var_export($filtered);
````

結果

````
array (
  'id' => '123',
  'fake_arg' => NULL, # 未帶入參數
  'component' => 
  array (
    0 => 5,
  ),
  'scalar' => false, # 型態不正確
  'array' => 
  array (
    0 => 1,
    1 => 3,
  ),
)
````

## filter_var

輸入一組變數,
並透過過濾器處理並返回,
不符合返回 false.

````
$filtered = filter_var(
  'http://example.com',
  FILTER_VALIDATE_URL, 
  FILTER_FLAG_PATH_REQUIRED
);

var_export($filtered);
````

結果

````
false
````

````
$filtered = filter_var(
  'bob@example.com',
  FILTER_VALIDATE_EMAIL
);

var_export($filtered);
````

結果

````
bob@example.com
````

## filter_var_array

輸入一組變數,
並透過過濾器處理並返回,
不符合返回 false.

````
$input = [
  'id' => 'libgd<script>',
  'component' => [5],
  'scalar' => ['A'],
  'array' => [3, 5],
];

$args = [
  'id'  => FILTER_SANITIZE_ENCODED,
  'fake_arg'  => FILTER_VALIDATE_INT,
  'component' => [
    'filter' => FILTER_VALIDATE_INT,
    'flags'  => FILTER_REQUIRE_ARRAY,
    'options' => [
      'min_range' => 1,
      'max_range' => 10,
    ],
  ],
  'scalar' => [
    'filter' => FILTER_VALIDATE_INT,
    'flags'  => FILTER_REQUIRE_SCALAR,
  ],
  'array' => [
    'filter' => FILTER_VALIDATE_INT,
    'flags'  => FILTER_REQUIRE_ARRAY,
  ],
];

$filtered = filter_var_array(
  $input,
  $args
);

var_export($filtered);
````

結果

````
array (
  'id' => 'libgd%3Cscript%3E',
  'fake_arg' => NULL, # 未帶入參數
  'component' => 
  array (
    0 => 5,
  ),
  'scalar' => false, # 型態不正確
  'array' => 
  array (
    0 => 3,
    1 => 5,
  ),
)
````

## 可用接口

filter_input() 與 filter_input_array() 支援接口類型

- INPUT_GET
- INPUT_POST
- INPUT_COOKIE
- INPUT_SERVER
- INPUT_ENV

## 驗證過濾器(Validate filters)
### FILTER_VALIDATE_BOOLEAN

````
過濾是否為布林值,
失敗返回 false.
````

````
# 範例
$var = '123';
var_export(filter_var(
  $var,
  FILTER_VALIDATE_BOOLEAN
));

# 輸出結果
false
````

#### 可用標記(Filter flags)
- FILTER_NULL_ON_FAILURE

````
除 true/false, on/off, yes/no, 1/0 外,
其餘均返回NULL.
````

### FILTER_VALIDATE_DOMAIN

````
過濾是否為網址,
失敗返回 false.
````

````
# 範例
$var = 'https://ten.fu-ming.tw/';
var_export(filter_var(
  $var,
  FILTER_VALIDATE_DOMAIN
));

# 輸出結果
https://ten.fu-ming.tw/
````

#### 可用標記(Filter flags)
- FILTER_FLAG_HOSTNAME

````
測試是否為純網址(不含協定與路徑等),
失敗返回 false.
````

### FILTER_VALIDATE_EMAIL

````
測試是否為電子信箱,
失敗返回 false.
````

````
# 範例
$var = 'ten@fu-ming.tw';
var_export(filter_var(
  $var,
  FILTER_VALIDATE_EMAIL
));

# 輸出結果
ten@fu-ming.tw
````

#### 可用標記(Filter flags)
- FILTER_FLAG_EMAIL_UNICODE

````
測試是否為電子信箱(允許使用者名稱使用unicode),
失敗返回 false.
````

### FILTER_VALIDATE_FLOAT

````
測試是否為浮點數,
失敗返回 false.
````

````
# 範例
$var = '1000.188';
var_export(filter_var(
  $var,
  FILTER_VALIDATE_FLOAT
));

# 輸出結果
1000.188
````

#### 可用標記(Filter flags)
- FILTER_FLAG_ALLOW_THOUSAND

````
測試是否為浮點數(允許使用千位數分號),
失敗返回 false.
````

````
# 範例
$var = '1,000,000.188';
var_export(filter_var(
  $var,
  FILTER_VALIDATE_FLOAT,
  FILTER_FLAG_ALLOW_THOUSAND
));

# 輸出結果
1000000.188
````

### FILTER_VALIDATE_INT

````
測試是否為整數,
失敗返回 false.
````

````
# 範例
$var = '188288';
var_export(filter_var(
  $var,
  FILTER_VALIDATE_INT
));

# 輸出結果
188288
````

#### 可用標記(Filter flags)
- FILTER_FLAG_ALLOW_OCTAL

````
測試是否為整數(8進位),
失敗返回 false.
````

````
# 範例
$var = '0274';
var_export(filter_var(
  $var,
  FILTER_VALIDATE_INT,
  ['flags' => FILTER_FLAG_ALLOW_OCTAL]
));

# 輸出結果
188
````

- FILTER_FLAG_ALLOW_HEX

````
測試是否為整數(16進位),
失敗返回 false.
````

````
# 範例
$var = '0xBC';
var_export(filter_var(
  $var,
  FILTER_VALIDATE_INT,
  ['flags' => FILTER_FLAG_ALLOW_HEX]
));

# 輸出結果
188
````

### FILTER_VALIDATE_IP

````
測試是否為 IP 位置,
失敗返回 false.
````

````
# 範例
$var = '192.168.0.1';
var_export(filter_var(
  $var,
  FILTER_VALIDATE_IP
));

# 輸出結果
192.168.0.1
````

#### 可用標記(Filter flags)
- FILTER_FLAG_IPV4

````
測試是否為 IP 位置(IPv4),
失敗返回 false.
````

- FILTER_FLAG_IPV6

````
測試是否為 IP 位置(IPv6),
失敗返回 false.
````

- FILTER_FLAG_NO_PRIV_RANGE

````
測試是否為 IP 位置(排除私有IP位置, 192.168.0.0/16),
失敗返回 false.
````

- FILTER_FLAG_NO_RES_RANGE

````
測試是否為 IP 位置(排除保留IP位置, 127.0.0.0/8),
失敗返回 false.
````

### FILTER_VALIDATE_MAC

````
測試是否為 MAC 位置,
失敗返回 false.
````

````
# 範例
$var = '9E-B6-3A-DD-C5-8D';
var_export(filter_var(
  $var,
  FILTER_VALIDATE_MAC
));

# 輸出結果
9E-B6-3A-DD-C5-8D
````

### FILTER_VALIDATE_REGEXP

````
驗證是否符合指定正規表示格式,
失敗返回 false.
````

````
# 範例
$var = '886912188349';
var_export(filter_var(
  $var,
  FILTER_VALIDATE_REGEXP,
  [
    'options' => [
      'regexp' => '/^[0-9]*$/',
    ],
  ]
));

# 輸出結果
886912188349
````

### FILTER_VALIDATE_URL

````
驗證是否符合 URL 網址,
失敗返回 false.
````

````
# 範例
$var = 'https://fu-ming.tw/';
var_export(filter_var(
  $var,
  FILTER_VALIDATE_URL
));

# 輸出結果
https://fu-ming.tw/
````

#### 可用標記(Filter flags)
- FILTER_FLAG_SCHEME_REQUIRED(Deprecated)
- FILTER_FLAG_HOST_REQUIRED(Deprecated)
- FILTER_FLAG_PATH_REQUIRED

````
驗證是否符合 URL 網址(必須有路徑),
失敗返回 false.
````

- FILTER_FLAG_QUERY_REQUIRED

````
驗證是否符合 URL 網址(必須有查詢字串),
失敗返回 false.
````

## 消毒過濾器(Sanitize filters)
### FILTER_SANITIZE_EMAIL

````
用於過濾電子郵件格式,
文字保留英數與!#$%&'*+-=?^_`{|}~@.[],
````

````
# 範例
$var = '//(kaoru@fu-ming.tw)';
var_export(filter_var(
  $var,
  FILTER_SANITIZE_EMAIL
));

# 輸出結果
kaoru@fu-ming.tw
````

### FILTER_SANITIZE_ENCODED

````
用於過濾字串(過濾方式以 encode 方式)
額外參考：https://stackoverflow.com/questions/12769462
````

````
# 範例
$var = 'http://fu-ming.tw/測試';
var_export(filter_var(
  $var,
  FILTER_SANITIZE_ENCODED
));

# 輸出結果
http%3A%2F%2Ffu-ming.tw%2F%E6%B8%AC%E8%A9%A6%E9%80%A3%E7%B7%9A
````

#### 可用標記(Filter flags)
- FILTER_FLAG_STRIP_LOW

````
用於過濾字串(過濾方式以去除方式)
編碼 ASCII 31(包含)以前的字元組(如 NULL, 空白之類)
````

````
# 範例
$var = "\0aä\x80";
var_export(filter_var(
  $var, 
  FILTER_SANITIZE_STRING, 
  FILTER_FLAG_STRIP_LOW
));

# 輸出結果
aä\x80
````

- FILTER_FLAG_STRIP_HIGH

````
用於過濾字串(過濾方式以去除方式)
編碼 ASCII 128(包含)以後的字元組(如 unicode 等)
````

````
# 範例
$var = "http://fu-ming.tw/測試";
var_export(filter_var(
  $var, 
  FILTER_SANITIZE_STRING, 
  FILTER_FLAG_STRIP_HIGH
));

# 輸出結果
http://fu-ming.tw/
````

- FILTER_FLAG_ENCODE_LOW

````
用於過濾字串(過濾方式以 encode 方式)
編碼 ASCII 31(包含)以前的字元組
````

````
# 範例
$var = "\0aä\x80";
var_export(filter_var(
  $var, 
  FILTER_SANITIZE_STRING, 
  FILTER_FLAG_ENCODE_LOW
));

# 輸出結果
&#0;aä\x80
````

- FILTER_FLAG_ENCODE_HIGH

````
用於過濾字串(過濾方式以 encode 方式)
編碼 ASCII 128(包含)以後的字元組(如 unicode 等)
````

````
# 範例
$var = "http://fu-ming.tw/測試";
var_export(filter_var(
  $var, 
  FILTER_SANITIZE_STRING, 
  FILTER_FLAG_ENCODE_HIGH
));

# 輸出結果
http://fu-ming.tw/&#230;&#184;&#172;&#232;&#169;&#166;
````

- FILTER_FLAG_STRIP_BACKTICK

````
用於過濾字串(過濾方式以去除方式)
過濾所有 ` 字串
````

### FILTER_SANITIZE_MAGIC_QUOTES

````
過濾字串內包含 ', ", \, NULL字元
並轉換成 encode 格式
````

````
# 範例
$var = "SELECT * FROM table WHERE name = 'kaoru'";
var_export(filter_var(
  $var, 
  FILTER_SANITIZE_STRING, 
  FILTER_FLAG_STRIP_BACKTICK
));

# 輸出結果
SELECT * FROM table WHERE name = &#39;kaoru&#39;
````

### FILTER_SANITIZE_NUMBER_FLOAT

````
過濾字串為數字內容
````

````
# 範例
$var = "LM188288";
var_export(filter_var(
  $var, 
  FILTER_SANITIZE_NUMBER_FLOAT
));

# 輸出結果
188288
````

#### 可用標記(Filter flags)
- FILTER_FLAG_ALLOW_FRACTION

````
過濾字串為數字內容,
但保留.作為小數點部分
````

- FILTER_FLAG_ALLOW_THOUSAND

````
過濾字串為數字內容,
但保留,作為千分位符號
````

- FILTER_FLAG_ALLOW_SCIENTIFIC

````
過濾字串為數字內容,
但保留 e 作為科學記號(指數)表示
````

### FILTER_SANITIZE_NUMBER_INT

````
過濾字串為數字內容(整數)
````

````
# 範例
$var = "LM188288";
var_export(filter_var(
  $var, 
  FILTER_SANITIZE_NUMBER_INT
));

# 輸出結果
188288
````

### FILTER_SANITIZE_SPECIAL_CHARS

````
過濾字串包含 HTML 跳脫字元與 ASCII 編碼小於 32 字元
HTML 跳脫字元：'"<>&
ASCII 編碼小於 32 字元：如 NULL, 空白
````

````
# 範例
$var = "<title>That's\nmy\nbook.</title>";
var_export(filter_var(
  $var, 
  FILTER_SANITIZE_SPECIAL_CHARS
));

# 輸出結果
&#60;title&#62;That&#39;s&#10;my&#10;book.&#60;/title&#62;
````

#### 可用標記(Filter flags)
- FILTER_FLAG_STRIP_LOW

````
用於過濾字串(過濾方式以去除方式)
編碼 ASCII 31(包含)以前的字元組(如 NULL, 空白之類)
````

- FILTER_FLAG_STRIP_HIGH

````
用於過濾字串(過濾方式以去除方式)
編碼 ASCII 128(包含)以後的字元組(如 unicode 等)
````

- FILTER_FLAG_ENCODE_HIGH

````
用於過濾字串(過濾方式以 encode 方式)
編碼 ASCII 128(包含)以後的字元組(如 unicode 等)
````

- FILTER_FLAG_STRIP_BACKTICK

````
用於過濾字串(過濾方式以去除方式)
過濾所有 ` 字串
````

### FILTER_SANITIZE_FULL_SPECIAL_CHARS

````
同等於使用 htmlspecialchars() 方法過濾字串
````

````
# 範例
$var = "<title>That's\nmy\nbook.</title>";
var_export(filter_var(
  $var, 
  FILTER_SANITIZE_FULL_SPECIAL_CHARS
));

# 輸出結果
&lt;title&gt;That&#039;s
my
book.&lt;/title&gt;
````

#### 可用標記(Filter flags)
- FILTER_FLAG_NO_ENCODE_QUOTES

````
同等於使用 htmlspecialchars() 方法過濾字串,
但保留 ' 與 " 不被轉換
````

### FILTER_SANITIZE_STRING

````
同等於使用 strip_tags() 方法過濾字串
````

````
# 範例
$var = "<title>That's\nmy\nbook.</title>";
var_export(filter_var(
  $var, 
  FILTER_SANITIZE_STRING
));

# 輸出結果
That&#39;s
my
book.
````

#### 可用標記(Filter flags)
- FILTER_FLAG_NO_ENCODE_QUOTES

````
同等於使用 strip_tags() 方法過濾字串,
但保留 ' 與 " 不被轉換
````

- FILTER_FLAG_STRIP_LOW

````
同等於使用 strip_tags() 方法過濾字串,
另外過濾編碼 ASCII 31(包含)以前的字元組(如 NULL, 空白之類)
````

- FILTER_FLAG_STRIP_HIGH

````
同等於使用 strip_tags() 方法過濾字串,
另外過濾編碼 ASCII 128(包含)以後的字元組(如 unicode 等)
````

- FILTER_FLAG_ENCODE_LOW

````
同等於使用 strip_tags() 方法過濾字串,
另外 encode 編碼 ASCII 31(包含)以前的字元組
````

- FILTER_FLAG_ENCODE_HIGH

````
同等於使用 strip_tags() 方法過濾字串,
另外 encode 編碼 ASCII 128(包含)以後的字元組(如 unicode 等)
````

- FILTER_FLAG_STRIP_BACKTICK

````
同等於使用 strip_tags() 方法過濾字串,
另外過濾所有 ` 字串
````

- FILTER_FLAG_ENCODE_AMP

````
同等於使用 strip_tags() 方法過濾字串,
另外 encode 所有 & 字串
````

### FILTER_SANITIZE_STRIPPED

````
相同於 FILTER_SANITIZE_STRING
````

### FILTER_SANITIZE_URL

````
用於過濾 URL (過濾方式以去除方式)
僅保留英數與 $-_.+!*'(),{}|\\^~[]`<>#%";/?:@&=.
````

````
# 範例
$var = "http://fu-ming.tw/富明";
var_export(filter_var(
  $var, 
  FILTER_SANITIZE_URL
));

# 輸出結果
http://fu-ming.tw/
````

### FILTER_UNSAFE_RAW

````
不做任何行為,
但可利用可用標記進行 encode 行為
````

````
# 範例
$var = "<script>var x = 'TEST';</script>";
var_export(filter_var(
  $var, 
  FILTER_UNSAFE_RAW
));

# 輸出結果
<script>var x = 'TEST';</script>
````

#### 可用標記(Filter flags)
- FILTER_FLAG_STRIP_LOW

````
同等於使用 strip_tags() 方法過濾字串,
另外過濾編碼 ASCII 31(包含)以前的字元組(如 NULL, 空白之類)
````

- FILTER_FLAG_STRIP_HIGH

````
同等於使用 strip_tags() 方法過濾字串,
另外過濾編碼 ASCII 128(包含)以後的字元組(如 unicode 等)
````

- FILTER_FLAG_ENCODE_LOW

````
同等於使用 strip_tags() 方法過濾字串,
另外 encode 編碼 ASCII 31(包含)以前的字元組
````

- FILTER_FLAG_ENCODE_HIGH

````
同等於使用 strip_tags() 方法過濾字串,
另外 encode 編碼 ASCII 128(包含)以後的字元組(如 unicode 等)
````

- FILTER_FLAG_STRIP_BACKTICK

````
同等於使用 strip_tags() 方法過濾字串,
另外過濾所有 ` 字串
````

- FILTER_FLAG_ENCODE_AMP

````
同等於使用 strip_tags() 方法過濾字串,
另外 encode 所有 & 字串
````

## 其他過濾器(Other filters)
### FILTER_CALLBACK

````
用於過濾是否為方法,
若不是則回傳 NULL
````

````
# 範例
$var = ' Kaoru ';
var_export(filter_var(
  $var,
  FILTER_CALLBACK,
  [
    'options' => 'trimString'
  ]
));

# 輸出結果
Warning: filter_var(): First argument is expected to be a valid callback in ...
NULL
````