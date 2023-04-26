# IDA 后台分析

```
import idc
# generate an empty idb
idc.auto_mark_range(0, idc.BADADDR, idc.AU_FINAL)
idc.auto_wait()
idc.qexit(0)
```
然后走命令行模式无人值守：
```
idat64 -c -A -Sidb.py -Lcache.log -odyld_shared_cache_arm64e dyld_shared_cache_arm64e
```




---

> 作者: 猫大人  
> URL: /ida-%E5%90%8E%E5%8F%B0%E5%88%86%E6%9E%90/  

