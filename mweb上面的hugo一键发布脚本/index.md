# MWeb上面的hugo一键发布脚本


使用前需要先指定hugo sites的路径
```
#!/bin/sh

fileDir={{fileDir}}
cdate=$(date +%Y-%m-%dT%H:%M:%S+08:00)
hugo_path="${fileDir%????}/hugo_blogs"
pagesname="{{title}}"
filename={{fileName}}
mweb_mediapath={{fileDir}}/media/${filename%???}

#拷贝文档目录
pagespath=$hugo_path/content/posts/$pagesname
if [ ! -d "$pagespath" ]; then
    mkdir "$pagespath"
fi
cp -rf "{{filePath}}" "$pagespath"/index.md

sed -i "" -e '1 d' "$pagespath"/index.md

sed -i "" -e '1 i \
---\
title: '"${pagesname}"'\
date: '"${cdate}"'\
author:\
  name: 猫大人\
description: \
keywords:\
tags:\
  - 随笔\
categories:\
  - 随笔\
hiddenFromHomePage: false\
hiddenFromSearch: false\
resources:\
  - name: \
    src: \
toc: true\
password:\
---\' "$pagespath"/index.md


#拷贝资源目录
if [ -d "$mweb_mediapath" ]; then
    mediapath=$hugo_path/content/posts/"$pagesname"/media/
if [ ! -d "$mediapath" ]; then
   mkdir "$mediapath"
fi
cp -rf "$mweb_mediapath" "$mediapath"
fi

cd $hugo_path
git add .
git commit -m "pages: $pagesname"
git push

```

---

> 作者: 猫大人  
> URL: /mweb%E4%B8%8A%E9%9D%A2%E7%9A%84hugo%E4%B8%80%E9%94%AE%E5%8F%91%E5%B8%83%E8%84%9A%E6%9C%AC/  

