# MAC 环境配置


* 安装端口转发工具 `brew install usbmuxd`
* ssh 链接免验证
```
sodu vim /etc/ssh/ssh_config

StrictHostKeyChecking no
UserKnownHostsFile /dev/null
```
如果提示 E45: 'readonly' option isset (add ! to override)
使用 :q! 强制关闭文件,用sudo打开

* 安装frida
```
sudo pip3 install frida==16.2.2
sudo pip3 install frida-tools -i https://mirrors.aliyun.com/pypi/simple
```

* 安装nodejs工具
```
brew install node
//砸壳工具
npm install -g bagbak

//frida工具
npm install -g igf
```



---

> 作者: 猫大人  
> URL: /mac-%E7%8E%AF%E5%A2%83%E9%85%8D%E7%BD%AE/  

