# 安装和卸载

```sh
rpm -i yum-version.rpm #安装
yum --version #显示 Yum 版本然后退出
rpm -e yum #卸载
```

# 配置

## 公共配置

/etc/yum.conf

```shell
# 主名称，固定名称
[main]
# 缓存目录
cachedir=/var/cache/yum/$basearch/$releasever
# 要不要保存缓存
keepcache=0
debuglevel=2
logfile=/var/log/yum.log
# 要不要做精确严格的平台匹配
exactarch=1
obsoletes=1
# 检查来源法性和完整性
gpgcheck=1
# 要不要支持插件
plugins=1
# 同时安装几个
installonly_limit=5
bugtracker_url=http://bugs.centos.org/set_project.php?project_id=23&ref=http://bugs.centos.org/bug_report_page.php?category=yum
distroverpkg=centos-release
```

## 仓库配置

/etc/yum.repos.d/*.repo

```shell
# TODO
```

# 常用命令

## 缓存

```sh
#将软件包信息提前在本地缓存一份，用来提高搜索安装软件的速度
yum makecache
#清除缓存
yum clean packages #清除缓存目录下的软件包
yum clean headers #清除缓存目录下的 headers
yum clean oldheaders #清除缓存目录下旧的 headers
yum clean all #(= yum clean packages;yum clean oldheaders) 清除缓存目录下的软件包及旧的headers。
```

## 软件包管理

```sh
# 搜索
yum search <软件名>
# 查看基本信息
yum info <软件名>
# 列出所有【可安装】软件
yum list
# 列出所有【已安装】软件
yum list installed
# 列出所有可更新软件
yum check-update

# 安装
yum install -y <软件名> #-y:当安装过程提示选择,回答全部问题为是，-q:不显示安装过程
# 更新所有软件
yum update
# 更新指定软件
yum update <软件名>
# 卸载
yum remove <软件名> / yum erase <软件名>
```

