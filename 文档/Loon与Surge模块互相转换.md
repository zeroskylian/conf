# Loon 转 Surge

## [Rule]
同 Rule

## [Rewrite]

Loon 的 Rewrite 分为 
1. reject: 直接断开连接
2. reject-200: 返回一个code为200，body内容为空的response
3. reject_img: 返回一个code为200，body内容一像素图片的的response
4. reject_dict: 返回一个code为200，body内容为"{}"的空json对象字符串
5. reject_array: 返回一个code为200，body内容为"[]"的空json数组字符串

转换到 Surge:
1. reject 在 [URL Rewrite] 中 
```loon
^http://example.com reject
```
转换为
```
[URL Rewrite]
^http://example.com - reject
```

2. reject-200 在 [Map Local] 中
```loon
[Map Local]
^http://example.com data-type=text data=" " status-code=200
```

3. reject_img 在 [Map Local] 中
```loon
^http://example.com reject-img
```
转换为
```
[Map Local]
^http://example.com data-type=tiny-gif status-code=200
```
4. reject_dict 在 [Map Local] 中
```loon
^http://example.com reject-dict
```
转换为
```
[Map Local]
^http://example.com data-type=text data="{}" status-code=200 header="Content-Type:application/json"
```

5. reject_array 在 [Map Local] 中
```loon
^http://example.com reject-array
```
转换为
```
[Map Local]
^http://example.com data-type=text data="[]" status-code=200
```

# [Script]

举例对比

Loon
```
http-response ^https:\/\/ocean\.shuqireader\.com\/sqios\/render\/render\/page\/bookstore$ script-path = https://kelee.one/Resource/Script/ShuQiCenterReader/ShuQiCenterReader_remove_ads.js, requires-body = true, tag = 移除书城横幅和阅读广告
```

Surge

```
移除书城横幅和阅读广告 = type=http-response, pattern=^https:\/\/ocean\.shuqireader\.com\/sqios\/render\/render\/page\/bookstore$, script-path=https://kelee.one/Resource/Script/ShuQiCenterReader/ShuQiCenterReader_remove_ads.js, requires-body=true
```

# [MitM]
在 Surge 中是 [MITM] 大写的