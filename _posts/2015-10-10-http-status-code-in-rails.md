---
author: StevenTTuD
layout: post
title: HTTP Status Code In Rails
published: true
date: 2015-10-10 11:22
tags:
  - Rails
comments: true

---
Rails將這些HTTP Status定義成有意義的單字。讓我們可以在使用的時候減少因為背錯而發生錯誤的機率。

### 使用方式

用symbol加上單字即可取代原本的HTTP Status Code(500)

```rb
render status: 500
render status: :forbidden
```

### 這些Symbol其實定義在Rack中

[原始碼 rack/utils.rb](https://github.com/rack/rack/blob/master/lib/rack/utils.rb#L452-L515)

```rb
    HTTP_STATUS_CODES = {

    # Informational
      100 => 'Continue',
      101 => 'Switching Protocols',
      102 => 'Processing',

    # Success
      200 => 'OK',
      201 => 'Created',
      202 => 'Accepted',
      203 => 'Non-Authoritative Information',
      204 => 'No Content',
      205 => 'Reset Content',
      206 => 'Partial Content',
      207 => 'Multi-Status',
      208 => 'Already Reported',
      226 => 'IM Used',

    # Redirection
      300 => 'Multiple Choices',
      301 => 'Moved Permanently',
      302 => 'Found',
      303 => 'See Other',
      304 => 'Not Modified',
      305 => 'Use Proxy',
      307 => 'Temporary Redirect',
      308 => 'Permanent Redirect',

    # Client Error
      400 => 'Bad Request',
      401 => 'Unauthorized',
      402 => 'Payment Required',
      403 => 'Forbidden',
      404 => 'Not Found',
      405 => 'Method Not Allowed',
      406 => 'Not Acceptable',
      407 => 'Proxy Authentication Required',
      408 => 'Request Timeout',
      409 => 'Conflict',
      410 => 'Gone',
      411 => 'Length Required',
      412 => 'Precondition Failed',
      413 => 'Payload Too Large',
      414 => 'URI Too Long',
      415 => 'Unsupported Media Type',
      416 => 'Range Not Satisfiable',
      417 => 'Expectation Failed',
      421 => 'Misdirected Request',
      422 => 'Unprocessable Entity',
      423 => 'Locked',
      424 => 'Failed Dependency',
      426 => 'Upgrade Required',
      428 => 'Precondition Required',
      429 => 'Too Many Requests',
      431 => 'Request Header Fields Too Large',

    # Server Error
      500 => 'Internal Server Error',
      501 => 'Not Implemented',
      502 => 'Bad Gateway',
      503 => 'Service Unavailable',
      504 => 'Gateway Timeout',
      505 => 'HTTP Version Not Supported',
      506 => 'Variant Also Negotiates',
      507 => 'Insufficient Storage',
      508 => 'Loop Detected',
      510 => 'Not Extended',
      511 => 'Network Authentication Required'
    }
```
還是不知道從何下手記憶嘛？來看保哥已經整理好一篇完整的HTTP Status Code文章囉^^

[The Will Will Web 網頁開發人員應了解的 HTTP 狀態碼](http://blog.miniasp.com/post/2009/01/16/Web-developer-should-know-about-HTTP-Status-Code.aspx)