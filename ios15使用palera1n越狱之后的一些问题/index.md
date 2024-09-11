# iOS15使用palera1n越狱之后的一些问题


1. 使用-c -f 先创建FakeFS, 再使用-f 进行有根越狱
2. 越狱之后先使用`ssh mobile@127.0.0.1 -p 2222` 登录,然后使用 `sudo passwd root` 设置root密码
3. root登录之后没有命令的历史记录,在 /var/root 创建文件 .zshrc
```
# Lines configured by zsh-newuser-install
HISTFILE=~/.histfile
HISTSIZE=999
SAVEHIST=1000
# End of lines configured by zsh-newuser-install
```
然后执行以下 `zsh`

---

> 作者: 猫大人  
> URL: /ios15%E4%BD%BF%E7%94%A8palera1n%E8%B6%8A%E7%8B%B1%E4%B9%8B%E5%90%8E%E7%9A%84%E4%B8%80%E4%BA%9B%E9%97%AE%E9%A2%98/  

