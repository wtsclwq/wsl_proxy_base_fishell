参考网友[未来遗迹](https://www.ruin-of-future.online/0xa8/wsl2-%E9%85%8D%E7%BD%AE%E4%BB%A3%E7%90%86ssh-git-%E5%8F%AF%E7%94%A8/)的bash版本，改编了一下

取得 host ip： `grep nameserver /etc/resolv.conf | awk { print \$2 }`

fish 语法请参考：[deepin wiki](https://gitee.com/deepinwiki/wiki/blob/master/fish%20%E8%84%9A%E6%9C%AC%E7%BC%96%E7%A8%8B.md)

````bash
set -xg HOST_IP  (grep nameserver /etc/resolv.conf | awk { print \$2 })
set -xg WSL_IP (hostname -I | awk '{print $1}')
set -xg PROXY_PORT 7890
set -xg PROXY_HTTP "http://$HOST_IP:$PROXY_PORT"
set -xg PROXY_SOCKS5 "socks5://$HOST_IP:$PROXY_PORT"
echo $PROXY_HTTP

# 自动代理
set -xg ALL_PROXY $PROXY_SOCKS5
set -xg all_proxy $PROXY_SOCKS5

set -xg HTTPS_PROXY $PROXY_HTTP
set -xg https_proxy $PROXY_HTTP

set -xg HTTP_PROXY  $PROXY_HTTP
set -xg http_proxy  $PROXY_HTTP

git config --global http.proxy $PROXY_HTTP
git config --global https.proxy $PROXY_HTTP

# 手动设置代理
function proxy 
    set -xg ALL_PROXY $PROXY_SOCKS5
    set -xg all_proxy $PROXY_SOCKS5

    set -xg HTTPS_PROXY $PROXY_HTTP
    set -xg https_proxy $PROXY_HTTP

    set -xg HTTP_PROXY  $PROXY_HTTP
    set -xg http_proxy  $PROXY_HTTP

    git config --global http.proxy $PROXY_HTTP
    git config --global https.proxy $PROXY_HTTP

end 
# 手动取消代理
function noproxy 
    set -e ALL_PROXY
    set -e all_proxy
    set -e HTTPS_PROXY 
    set -e HTTP_PROXY 
    set -e http_proxy 
    set -e https_proxy 
    git config --global --unset http.proxy $PROXY_HTTP
    git config --global --unset https.proxy $PROXY_HTTP
end
````