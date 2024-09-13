# 恢复macho被清空的Function Starts


从ida提取
```
import idautils
import idc

with open("symbols.txt", "w") as f:
    for func_ea in idautils.Functions():
        func_name = get_func_name(func_ea)
        if func_name.startswith("sub_") :
            f.write(f"{hex(func_ea)} {func_name}\n")
```

lief 恢复
```
import lief

# 打开你的 Mach-O 文件
binary = lief.parse("target_macho")

with open("symbols.txt", "r") as sym_file:
    for line in sym_file:
        address, name = line.split()
        binary.add_local_symbol(int(address, 16),name);
        binary.function_starts.add_function(int(address, 16))
        
function_starts = binary.function_starts
print(function_starts);
binary.write("target_macho_symbol1")

```

---

> 作者: 猫大人  
> URL: /%E6%81%A2%E5%A4%8Dmacho%E8%A2%AB%E6%B8%85%E7%A9%BA%E7%9A%84function-starts/  

