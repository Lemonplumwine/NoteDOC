+ 设置中文：将Windows的字体放入到Ubuntu里面

  ```shell
  sudo mkdir /usr/share/fonts/windows
  sudo cp -r /mnt/c/Windows/Fonts/*.ttf /usr/share/fonts/windows/
  fc-cache
  ```






+ 换源

  ```shell
  cd /etc/apt
  sudo cp sources.list sources.list_bak
  sudo chmod 777 /etc/apt/sources.list
  vim sources.list
  ```

  然后去清华源获取配置文件：[https://mirror.tuna.tsinghua.edu.cn/](https://links.jianshu.com/go?to=https%3A%2F%2Fmirror.tuna.tsinghua.edu.cn%2F)从相关链接的使用帮助链接中找到Ubuntu；把其中的文件复制到上面的文件中。



+ 更新软件

  ```shell
  sudo apt-get update 
  sudo apt-get upgrade
  ```



+ 清理系统，清理系统中不需要的包

  ```shell
  #删除系统不再使用的孤立软件
  sudo apt autoremove
  
  #清理旧版本的软件缓存，APT的底层包是dpkg, 而dpkg 安装Package时, 会将 *.deb 放在 /var/cache/apt/archives/中，apt-get autoclean 只会删除 /var/cache/apt/archives/ 已经过期的deb
  sudo apt-get autoclean
  
  #会将 /var/cache/apt/archives/ 的 所有 deb 删掉，可以理解为 rm /var/cache/apt/archives/*.deb。
  sudo apt-get clean
  
  #包管理的临时文件目录/var/cache/apt/archives
  #没有下载完的在/var/cache/apt/archives/partial 
  ```




+ 卸载软件

  ```
  #查询安装的所有软件信息
  dpkg --list
  #1 卸载软件及其配置信息
  sudo apt-get --purge remove 软件名
  #2 卸载软件 不删除配置信息
  sudo apt-get remove 软件名
  #3 删除残余的配置文件
  dpkg -l |grep ^rc|awk ‘{print $2}’ |sudo xargs dpkg -P
  ```



+ 删除旧的内核

  ```shell
  #1 查询当前的内核版本
  uname -a
  #2 查询所有的内核版本
  sudo dpkg --get-selections |greb linux
  
  #3 先删除image文件，根据需要删除内核版本，image-4.15.0-55，一般都是比uname的小
  sudo apt-get purge linux-image-4.15.0-55
  #该执行步骤删除了 linux-image-4.15.0-55-generic  以及 linux-image-extra-4.15.0-55-generic ，同时也安装了 linux-image-unsigned-4.15.0-55-generic ，此时先不用管，后面可以一起删除。
  
  #4 如果有旧版本对应的header文件
  sudo apt-get purge linux-headers-4.15.0-55
  
  #5 如果存在modules文件，则删除modules文件，此操作还会删除第3步提到的 linux-image-unsigned-4.15.0-55-generic 文件，同理选择相应内核版本。
  sudo apt-get purge linux-modules-4.15.0-55
  
  #对于最后还存在module-extra 的情况没找到说明就没有轻易的删除，似乎这只在20.04这个版本才有
  
  #6 更新引导
  sudo update-grub
   #或者
  sudo update-grub2
  
  # 不要使用sudo apt-get remove来删除,会出现linux-signed-image-3.13.0-63-generic deinstall
  ```

  

+ 查看分区空间

  ```shell
  df -hl
  ```




+ 查看文件的权限

  ```shell
  #l 查看所有文件包括文件夹
  ll
  #2 只查看当前文件夹下文件
  ls -l
  
  #r w x , 4 2 1
  ```

  