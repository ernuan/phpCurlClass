## 基本使用 ##
环境要求：php7+，需开启CURL扩展。

    // 请求地址
    $url = 'https://baidu.com';
    // get请求
    HttpCurl::url($url)->get();
    // post请求
    HttpCurl::url($url)->post();
    // put请求
    HttpCurl::url($url)->put();
    // delete请求
    HttpCurl::url($url)->delete();

发起请求后返回 `HttpCurl` 的实例化对象，如果需要获取响应结果数据可以通过调用 `response()` 获取：

    // 获取响应结果数据
    HttpCurl::url('https://baidu.com')->get()->response();

如果需要将JSON响应数据转换成数组形式，可以使用内置 `jsonToArray()` 方法

    HttpCurl::url( 'https://baidu.com/')->post()->jsonToArray();

## 请求传参 ##

    // 使用字符串
    HttpCurl::url('https://baidu.com')
        ->data('a=1&b=2&c=3')
        ->get()
        ->response();
    
    // 使用数组
    HttpCurl::url('https://baidu.com')->data([
            'a' => 1,
            'b' => 2,
            'c' => 3
        ])->get()->response();

## 多线程 ##
当前url参数为数组时，将自动使用多线程方式发起请求，同样支持使用内置的 `jsonToArray()` 方法对JSON数据进行转换

    HttpCurl::url([
        'https://baidu.com/',
        'https://baidu.com/',
        'https://baidu.com/'
    ])->post()->response();

## 获取header信息 ##

    // 获取header头信息 数组格式 并且包含body信息
    $header = HttpCurl::url('https://baidu.com/')->getHeaders(true,false);
    
    // 获取header头信息 字符串格式 默认不包含body信息
    $header = HttpCurl::url('https://baidu.com/')->getHeaders();

使用 `getHeaders()` 方法请求方式 `method` 为 `head` 请求 如果在之前调用 `get(),post(),delete(),put()` 方法 将会导致请求两次。

## 获取请求结果信息 ##

    HttpCurl::url('https://baidu.com/')->getResInfo();

返回 `curl_getinfo()` 获取的信息，并包含响应数据，`getResInfo()` 默认获取全部，接收一个参数为数组键名，可单独获取某个信息

    // 获取响应数据
    HttpCurl::url('https://baidu.com/')->getResInfo('body');
    
    // 获取请求总耗时
    HttpCurl::url('https://baidu.com/')->getResInfo('total_time');
    
    // 获取连接耗时
    HttpCurl::url('https://baidu.com/')->getResInfo('connect_time');

## 获取301/302重定向地址 ##
该方法是获取 `header` 中的 `location`值内容。

    HttpCurl::url('https://api.btstu.cn/sjbz/api.php')->post()->getRedirectUrl();

## 请求配置 ##

    // 设置 HTTP 请求头
    HttpCurl::url('https://baidu.com/')->httpHeader([
        'Accept: application/json'
    ])->get()->response();
    
    // 设置cookie
    HttpCurl::url('https://baidu.com/')->cookie('')->get()->response();
    
    // 设置referer
    HttpCurl::url('https://baidu.com/')->referer('')->get()->response();
    
    // 设置ua
    HttpCurl::url('https://baidu.com/')->ua('')->get()->response();
    
    // 设置超时时间 单位 ms 默认3000ms
    HttpCurl::url('https://baidu.com/')->timeout(5000)->get()->response();
    
    // 连贯操作
    HttpCurl::url('https://baidu.com/')->httpHeader()->referer()->cookie()->ua()->timeout(5000)->get()->response();

更多方式请翻看源代码查看