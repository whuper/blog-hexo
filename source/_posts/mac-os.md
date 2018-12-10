#### ios手机恢复出厂设置会恢复到出厂时候的旧系统吗

不会

#### mac电脑 恢复出厂设置会恢复到出厂时候的旧系统吗

不会

如果安装了双系统,要把windows分区合回去,才能恢复系统

#### 制作OS X El Capitan 10.11安装盘

1. 下载并打开mac 10.11 DMG 文件将安装文件拖入到：应用程序
2. 准备一个 8GB 或以上容量的 U 盘,把U盘用磁盘工具格式化。命名为`udisk`
3. 打开终端输入如下命名并在提示下输入密码。等待即可。

```sh
sudo /Applications/Install\ OS\ X\ El\ Capitan.app/Contents/Resources/createinstallmedia --volume /Volumes/udisk --applicationpath /Applications/Install\ OS\ X\ El\ Capitan.app --nointeraction
```

4. 完成以后,关机,再按住Option键开机，用U盘开始全新安装。

> 选择 U 盘的图标回车，即可通过 U 盘来安装 OS X El Capitan 了！你可以直接覆盖安装系统(升级)，也可以在磁盘工具里面格式化抹掉整个硬盘，或者重新分区等实现全新的干净的安装。

## 重新安装系统有三种方法

1. 使用苹果自带的联网恢复功能进行安装（适合进不了系统的，最简单但是耗时） 
2. 制作U盘启动进行安装（适合进不了系统的，快但是肯能会遇到一些问题，比较复杂） 
3. 从appstore下载进行安装（最简单，适合升级系统）



> 第一种相对比较简单，开机的时候按住`command+r`，等待他下载完成。
>
> `(格式化格式一定要选择GUID模式！！！千万别选ASPF格式)`



#### 验证dmg文件

> 打开 应用程序 – 实用工具 – 终端
>
> 先输入 md5 空格
> 然后将 InstallESD.dmg 拖拽到终端窗口内
> 随后回车验证MD5


## mac下Homebrew的安装与使用

安装XCode或者Command Line Tools for Xcode,Xcode可以从AppStore里下载安装

`Command Line Tools for Xcode`需要在终端中输入以下代码运行安装：

```sh
xcode-select --install
```

安装Homebrew

```sh
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

移除Homebrew

```sh
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/uninstall)"
```

基本使用


```sh
// 搜索包
brew search mysql

// 安装包
brew install mysql

// 查看包信息，比如目前的版本，依赖，安装后注意事项等
brew info mysql

// 卸载包
brew uninstall wget

// 显示已安装的包
brew list

// 查看brew的帮助
brew –help

// 更新， 这会更新 Homebrew 自己
brew update

// 检查过时（是否有新版本），这会列出所有安装的包里，哪些可以升级
brew outdated
brew outdated mysql

// 升级所有可以升级的软件们
brew upgrade
brew upgrade mysql

// 清理不需要的版本极其安装包缓存
brew cleanup
```


## How to download XCode for MAC 10.11.6

I am searching for a proper IDE for iOS developement but MAC OS X version is 10.11.6 & because of this I am unable to install XCode. It says "I should have MAC OS X 10.12+"



>Go here for other downloads including older versions of Xcode:

>developer.apple.com/download/more 

>I think you can use version 8.2.1 for OS X 10.11.6.
