---

title: mac命令
date: 2021-12-30

---

## 压缩包去掉系统文件

    zip -d filename.zip "__MACOSX*" 

## 硬盘挂载

    diskutil list

    diskutil mountDisk /dev/disk4


    diskutil unmountDisk /dev/disk4


## Mac中终端关机命令

1.立即关机命令 

        sudo halt

    或者 

        sudo shutdown -h now 
 
2. 10分钟后关机 

   sudo shutdown -h +10 
 
3. 晚上8点关机 

  sudo shutdown -h 20:00 
 
4. 立即重启

    sudo reboot  
或者 
    
    sudo shutdown -r now

5.设定时间为2012年7月12日15：00分关机,命令为：

    sudo shutdown -h 1207121500 
同理： 
    2014年7月11日15：00分重启,命令：

    sudo shutdown -r 1407111500 

 命令的主体位：shutdown（关闭）

     h/r/s //分别代表：关机/重启/睡眠。

 最后加上时间就可行了。

## 关闭Mac的Microsoft AutoUpdate弹框提示 


设置权限不可访问
打开终端

    cd /Library/Application\ Support/Microsoft/MAU2.0
    sudo chmod 000 Microsoft\ AutoUpdate.app

## 解决brew安装包一直卡在Updating Homebrew

运行命令brew install node，结果界面一直卡在Updating Homebrew上，有两种解决办法

方法一：关闭brew每次执行命令时的自动更新（推荐）

    vim ~/.bash_profile

###  新增一行

    export HOMEBREW_NO_AUTO_UPDATE=true

方法二(懒人方法)：

    出现Updating Homebrew的时候ctrl+c一下就行


## csrutil

csrutil status


cd /library/updates

rm -rf ./*


重新启动你的Mac电脑，在开机时一直按住Command+R迸入Recovery模式

csrutil disable