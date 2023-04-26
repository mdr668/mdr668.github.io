# LLVM 编译脚本


https://heroims.github.io/2021/11/09/OLLVM%E4%BB%A3%E7%A0%81%E6%B7%B7%E6%B7%86%E7%A7%BB%E6%A4%8D%E4%B8%8E%E4%BD%BF%E7%94%A8(%E7%BB%AD)/

2.1 新建build目录（mkdir build）

2.2 进入build目录（cd build）

2.3 cmake -DCMAKE_BUILD_TYPE=Release -DLLVM_CREATE_XCODE_TOOLCHAIN=ON -DLLVM_INCLUDE_TESTS=OFF ../obfuscator/

2.4 make -j8 

3.1 sudo make install-xcode-toolchain

3.2 sudo mv /usr/local/Toolchains  /Library/Developer/  或者手动将/usr/local/Toolchains目录下文件移动至/Library/Developer目录下

-mllvm -fla



cd Build && cmake -G "Ninja" -DLLVM_ENABLE_PROJECTS="lld;clang;clang-tools-extra" -DLLVM_INCLUDE_TESTS=OFF -DCMAKE_BUILD_TYPE=Release -DLLVM_APPEND_VC_REV=on -DLLDB_USE_SYSTEM_DEBUGSERVER=YES -DLLVM_CREATE_XCODE_TOOLCHAIN=on -DCMAKE_INSTALL_PREFIX=~/Library/Developer/ ../llvm && ninja && ninja install-xcode-toolchain

cmake -DCMAKE_BUILD_TYPE=Release -DLLVM_CREATE_XCODE_TOOLCHAIN=ON ../obfuscator/llvm

-G "Ninja" -DLLVM_ENABLE_PROJECTS="lld;clang;clang-tools-extra" -DLLVM_EXPERIMENTAL_TARGETS_TO_BUILD=WebAssembly -DLLVM_DEFAULT_TARGET_TRIPLE=wasm32-wasi -DLLVM_INSTALL_BINUTILS_SYMLINKS=TRUE


 ./bin/clang ../test_wasm/wasm_interpreter.c ../test_wasm/invokeNative_aarch64.s -o ../test_wasm/vmp_md5 -target arm64-apple-ios11.0 -isysroot /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS15.2.sdk -mllvm -svmp -mllvm -sobf -mllvm -sub -mllvm -fla -mllvm -bcf



---test----
mkdir cmake-build-release
cd cmake-build-release
cmake -G "Ninja" -DLLVM_ENABLE_PROJECTS="lld;clang;clang-tools-extra" -DCMAKE_BUILD_TYPE=Release -DLLVM_INCLUDE_TESTS=OFF -DLLVM_CREATE_XCODE_TOOLCHAIN=ON -DCMAKE_INSTALL_PREFIX=~/Library/Developer/ ../../llvm
ninja install-xcode-toolchain


### 正经可用的
```
cmake -G "Ninja" -DCMAKE_BUILD_TYPE=Release -DLLVM_TARGETS_TO_BUILD="X86;AArch64;ARM;WebAssembly" -DLLVM_INCLUDE_TESTS=OFF -DLLVM_ENABLE_NEW_PASS_MANAGER=OFF -DLLVM_CREATE_XCODE_TOOLCHAIN=ON -DLLVM_ENABLE_PROJECTS="clang;libcxx;libcxxabi" -DCMAKE_INSTALL_PREFIX=~/Library/Developer/ ../../llvm

ninja -j8
ninja install-xcode-toolchain
```

```
cmake -G "Ninja" -DLLVM_ENABLE_PROJECTS="lld;clang;clang-tools-extra" -DLLVM_EXPERIMENTAL_TARGETS_TO_BUILD=WebAssembly -DLLVM_DEFAULT_TARGET_TRIPLE=wasm32-wasi -DLLVM_INSTALL_BINUTILS_SYMLINKS=TRUE -DLLVM_ENABLE_NEW_PASS_MANAGER=OFF -DLLVM_INCLUDE_TESTS=OFF
```


mkdir cmake-build-release
cd cmake-build-release
cmake -G "Ninja" -DLLVM_ENABLE_PROJECTS="lld;clang;clang-tools-extra" -DLLVM_INCLUDE_TESTS=OFF -DCMAKE_BUILD_TYPE=Release -DLLVM_APPEND_VC_REV=on -DLLDB_USE_SYSTEM_DEBUGSERVER=YES -DLLVM_CREATE_XCODE_TOOLCHAIN=on -DCMAKE_INSTALL_PREFIX=~/Library/Developer/ ../llvm 
 ninja && ninja install-xcode-toolchain


__attribute((__annotate__(("sub"))));
```
-G Ninja：使用 ninja 进行编译源码
-DLLVM_ENABLE_PROJECTS="clang"：启用clang，有多个选择 但我们只需要clang，官方文档有说明
-DCMAKE_BUILD_TYPE=Release：构建 release 版本，比 debug 版本编译快很多
-DLLVM_INCLUDE_TESTS=OFF：关闭 llvm 的头文件测试，也是为了加快编译速度
-DLLVM_ENABLE_NEW_PASS_MANAGER=OFF：这个非常重要，llvm-12.x 开始默认使用 newPM进行编译源码，导致 ollvm 不起作用！因此，需要加上这个参数禁用掉 newPM。（虽然可以用 -flegacy-pass-manager  让 llvm 不走 newPM 编译，但是...你不觉得在你的源码上加一串怎么长的命令很难受吗）
```



## VMP自动化存在的问题
1. 目前只能单个Module遍历函数,不能同时对多个Module中的函数做VMP处理





```
StringRef ImplAssembly = R"(
    define void @foo() {
      ret void
    }
  )";
  auto InjectedModule = parseAssemblyString(ImplAssembly, error, M.getContext());

  auto* ImplFunction = InjectedModule->getFunction("foo");
  auto DeclFunction = Function::Create(ImplFunction->getFunctionType(), ImplFunction->getLinkage(), "foo", M);
```

---

> 作者: 猫大人  
> URL: /llvm-%E7%BC%96%E8%AF%91%E8%84%9A%E6%9C%AC/  

