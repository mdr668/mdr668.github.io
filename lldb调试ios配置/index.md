# lldb调试iOS配置


1.拷贝debugserver到本地计算机中：
```
scp -P 2222 root@127.0.0.1:/Developer/usr/bin/debugserver ./debugserver。
```
2.然后用ldid添加权限。
由于ldid不支持fat二进制文件，所以要给debugserver瘦身，通过lipo指定要支持的指令类型，例如：
```
lipo -thin arm64 ./debugserver -output ./debugserver
```
3.给debugserver添加task_for_pid权限，保存以下内容为ent.xml文件：
```
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
        <key>com.apple.springboard.debugapplications</key>
        <true/>
        <key>get-task-allow</key>
        <true/>
        <key>task_for_pid-allow</key>
        <true/>
        <key>run-unsigned-code</key>
        <true/>
</dict>
</plist>
```
ldid -Sent.xml debugserver

5.重新拷贝debugserver回手机中：
```
scp -P 2222 ./debugserver root@127.0.0.1:/usr/sbin/debugserver
scp -P 2222 ./ent.xml root@127.0.0.1:/usr/sbin/ent.xml
```


通过进程id附加： 
```
ps aux | grep UserName | grep ProcName

(lldb)process attach --pid PidNum
```

通过进程名附加：  
```
(lldb)process attach -name ProcName
```

使用debugserver启动App
```
debugserver -x auto localhost:1234 /var/containers/Bundle/Application/EEC8363A-0EEE-4344-B590-F81477B2C3D0/com_kwai_gif.app/com_kwai_gif
```

```
process connect connect://localhost:1234
```

运行进程
```
process launch --stop-at-entry
```


```
breakpoint set --func-regex ptrace
```

对动态库函数下断
```
breakpoint set --shlib foo.dylib --name foo
```

对某个OC方法下断
```
breakpoint set --method/--name "-[TSRegistWinCtrl showRegister]"
```

对某个OC类的所有方法下断
```
breakpoint set -r '\[RegisterWindowController .*\]'
```

条件断点
```
watchpoint modify -c '*(int *)0x1457fa70 == 20'
```

找按钮事件
```
(lldb) po [[self btnTest] allTargets]
{(
    <ViewController: 0x166af1f0>
)}
(lldb) po [[self btnTest] actionsForTarget:(id)0x166af1f0 forControlEvent:0]
<__NSArrayM 0x165b8950>(
testAction:
)

```












---

> 作者: 猫大人  
> URL: /lldb%E8%B0%83%E8%AF%95ios%E9%85%8D%E7%BD%AE/  

