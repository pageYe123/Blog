## 响应头

HTTP 响应头中 Cache-Control 字段用于设置缓存，决定文件是发 HTTP 请求还是从硬盘缓存读取。

```
Cache-Control: public, max-age=31536000
```

max-age 单位为秒，31536000秒是365天。

注意：

*.html 文件不要设置缓存，否则无法得知其中的 CSS、JS 文件是否更新。