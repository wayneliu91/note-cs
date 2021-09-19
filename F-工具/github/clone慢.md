# GitHub clone慢

## 第一种（推荐）

chrome 浏览器下载【github加速】插件

## 第二种

1. 设置终端的HTTP请求走代理
```shell
// port 换成你自己的端口号
git config --global http.proxy 'socks5://127.0.0.1:1086' 
git config --global https.proxy 'socks5://127.0.0.1:1086'
```
2. git clone 的地址用https地址，不要用SSH地址。

