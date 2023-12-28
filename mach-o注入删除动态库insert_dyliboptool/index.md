# Mach-O注入删除动态库insert_dyliboptool

如果要让现成的App，执行自己的代码可以通过注入动态库，

静态的注入可以使用optool工具修改MachO的Load Commands然后重签，

动态运行时可以使用dlopen 或 Bundle(path: "**.bundle").load()加载


```
通过 otool -L命令查看你生成的.dylib文件
otool -L ioswechatselectall.dylib

查看CPU框架
lipo -archs ioswechatselectall.dylib

首先要找到libsubstrate.dylib文件,该文件应该在/opt/thoes/lib/目录下,然后将其拷贝到与你生成的的.dylib一个目录下,通过下面的指令修改依赖,
install_name_tool -change /Library/Frameworks/CydiaSubstrate.framework/CydiaSubstrate @loader_path/libsubstrate.dylib wx.dylib

添加可执行文件的依赖
此处用的是insert_dylib 下载地址在https://github.com/Tyilo/insert_dylib
编译后,将其与其他两个文件拷贝到同一目录下

然后将其插入到执行文件中
1.将wx.dylib 和libsubstrate.dylib拷贝进你的WeChat.app 
2.记住要把WeChat_patched的名字改回来WeChat

@executable_path是一个环境变量，指的是二进制文件所在的路径

insert_dylib命令格式： ./insert_dylib 动态库路径 目标二进制文件

//注入动态库
./insert_dylib @executable_path/ioswechatselectall.dylib WeChat 
./yololib  Wechat wxhbts.dylib
//打包成ipa 
xcrun -sdk iphoneos PackageApplication -v WeChat.app -o `pwd`/WeChat.ipa

注入神器optool

编译获取
因为 optool 添加了 submodule，因为需要使用 --recuresive 选项，将子模块全部 clone 下来
git clone --recursive https://github.com/alexzielenski/optool.git
cd optool
xcodebuild -project optool.xcodeproj -configuration Release ARCHS="i386 x86_64" build

使用 optool 把 wx.dylib 注入到二进制文件中

./optool install -c load -p "@executable_path/wx.dylib" -t WeChat

codesign签名dylib

errSecInternalComponent 错误
在Mac终端上运行codesign命令，并“始终允许” /usr/bin/codesign访问密钥
security unlock-keychain login.keychain
解锁命令移至，~/.bash_profile以便在SSH客户端启动时对钥匙串进行解锁
AppleWWDRCAG3 不收信任证书
获取证书
security find-identity -v -p codesigning 
签名Dylib 
codesign -f -s B3C14FC452E835C09B70D5C24961EBAFC0A2C4B9 hook.dylib 
ldid -S dumpdecrypted.dylib
1.安装 Xcode 
2.安装 Command Line Tools 
xcode-select --install 
codesign --force --deep --sign - /xxx/xxx.app 
签名错误 resource fork, Finder information, or similar detritus not allowed 
解决方法 xattr -lr <path_to_app_bundle> 
查看 xattr -cr <path_to_app_bundle> 删除 
或者 find . -type f -name '*.jpeg' -exec xattr -c {} \; find . -type f -name '*.png' -exec xattr -c {} \; 
find . -type f -name '*.tif' -exec xattr -c {} \; 
整个App签名 codesign -fs "授权证书" --no-strict --entitlements=生成的plist文件 WeChat.app 
验证Dylib签名 codesign –verify hook.dylib codesign -vv -d WeChat.app

Xcode升级到8.3后 用命令进行打包 提示下面这个错误
后面根据对比发现新版的Xcode少了这个PackageApplication  (QQ群下载)
先去找个旧版的Xcode里面copy一份过来
放到下面这个目录：
/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/usr/bin/

然后执行命令
sudo xcode-select -switch /Applications/Xcode.app/Contents/Developer/

chmod +x /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/usr/bin/PackageApplication

theos

ARCHS = armv7 armv7s arm64
TARGET = iphone:8.4:7.0
#指定路径，否则默认在 /Library/MobileSubstrate/DynamicLibraries
LOCAL_INSTALL_PATH = /usr/bin
include theos/makefiles/common.mk
TWEAK_NAME = oooo
oooo_FILES = oooo.xm
#指定版本
_THEOS_TARGET_LDFLAGS += -current_version 1.0
_THEOS_TARGET_LDFLAGS += -compatibility_version 1.0
#tweak2.mk是我修改过的，去掉了CydiaSubstrate链接，因为这个dylib用不到
include $(THEOS_MAKE_PATH)/tweak2.mk
-current_version、-compatibility_version参数参考自苹果官方！！
https://developer.apple.com/library/mac/documentation/DeveloperTools/Conceptual/DynamicLibraries/100-Articles/CreatingDynamicLibraries.html

theos编译不越狱可用的dylib

SUBSTRATE ?= yes
instance_USE_SUBSTRATE = $(SUBSTRATE)
把上面的instance替换成你的TweakName
在上面的这个例子中就是XXXX 
include _USE_SUBSTRATE = no，请转换为使用include _LOGOS_DEFAULT_GENERATOR = internal
编译：make SUBSTRATE=no
编译DEB：make SUBSTRATE=no package
编译DEB并安装：make SUBSTRATE=no package  install

```

---

> 作者: 猫大人  
> URL: /mach-o%E6%B3%A8%E5%85%A5%E5%88%A0%E9%99%A4%E5%8A%A8%E6%80%81%E5%BA%93insert_dyliboptool/  

