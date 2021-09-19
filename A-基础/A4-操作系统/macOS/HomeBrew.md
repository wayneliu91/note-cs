# 常用命令

```sh
#搜索包
brew search mysql
#安装包
brew install mysql
#查看包信息，比如目前的版本，依赖，安装后注意事项等
brew info mysql
#卸载包
brew uninstall wget
#显示已安装的包
brew list
#查看brew的帮助
brew –help
#更新， 这会更新 Homebrew 自己
brew update
#检查过时（是否有新版本），这会列出所有安装的包里，哪些可以升级
brew outdated
brew outdated mysql
#升级
brew upgrade #所有可升级软件
brew upgrade mysql #指定软件
#清理：不需要的版本 + 安装包缓存
brew cleanup
brew cleanup mysql
```

# 镜像仓库

阿里云：https://developer.aliyun.com/mirror/homebrew?spm=a2c6h.13651102.0.0.24311b11tSZIvB

