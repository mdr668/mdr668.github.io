# 使用 dtrace 绕过 mac 上越狱检测



其中 -n 参数表示打印特定的 probs 内容的调用。现在这样仅仅显示了调用，但是调用的信息还是不详细的。这个时候就需要使用 dtrace 的脚本获取更多的信息。

dtrace 的脚本

dtrace 的名字暗示了自己的脚本，dtrace 使用了 D 语言作为脚本语言。
首先，我们需要创建一个叫 `syscall.d` 文件
```
syscall::read:entry
{printf ("%s called read, asking for %d bytes\n", execname, arg2);
}
```
execname 代表了进程名字
`sudo dtrace -s syscall.d`

```
sudo dtrace -n 'syscall::exit:entry { printf ("%s called exit %s\n", execname ,probefunc); ustack();}' -o ~/Desktop/dtrace3.log


sudo dtrace -n 'syscall::open:entry { printf("%s %s", execname, copyinstr(arg0)); }' -o ~/Desktop/dtrace4.log


```



打印open的调用参数
```
sudo dtrace -n 'syscall::open:entry { printf("open(\"%s\", %d, %d)\n", copyinstr(arg0), arg1, arg2); ustack();}'

```
监听指定 pid 的信息
```
sudo dtrace -qn 'syscall::write:entry, syscall::sendto:entry /pid == $target/ { printf("(%d) %s %s", pid, probefunc, copyinstr(arg1)); }' -p [PID]
```

跟踪ptrtace并打印堆栈  可以对抗syscall调用
```
dtrace -q -n 'syscall:::entry /pid == $target && probefunc == "ptrace"/ { ustack(); }' -p <pid>
```






---

> 作者: 猫大人  
> URL: /%E4%BD%BF%E7%94%A8-dtrace-%E7%BB%95%E8%BF%87-mac-%E4%B8%8A%E8%B6%8A%E7%8B%B1%E6%A3%80%E6%B5%8B/  

