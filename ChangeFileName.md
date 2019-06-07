### 非递归版本(以实现）
```py


```

### 递归处理文件名：参考链接[传送](https://blog.csdn.net/u014735301/article/details/51203445)

```py

#!/usr/bin/env python3
# -*- coding: utf-8 -*-
import sys, string, os  
 
path="E:\XXX\XXX" #路径
prefix = "ZX-" #前缀
 
dirname = ""
index = 1	
def RenameFiles(path,prefix):
	global index 
	global dirname
	#获取目录下所有文件，包括文件夹
	parents = os.listdir(path)
	for parent in parents:
		#拼接一个当前文件的路径
		child = os.path.join(path,parent)
		#print(child)
		if os.path.isdir(child):#判断是否为文件夹 是文件夹 递归调用函数
			dirname = parent
			RenameFiles(child,prefix)
			#print(child)
		else:#不是文件夹 递归调用函数
			#获取文件名称 例 "aaa.png"  filename = aaa
			filename = os.path.splitext(parent)[0]#parent.split('.')[0][-1]
			#获取文件后缀名 例 "aaa.png"  sufix =.png
			sufix = os.path.splitext(child)[1]
			#判断后缀名
			if (sufix == ".png" or sufix == ".PNG" or sufix == ".jpg"):
				#print(sufix)			
				#拼接一个新的文件路径
				newfile = path + "\\" + prefix + dirname +"-%04d"%(index) + sufix
				print(newfile)
				os.rename(child, newfile)#替换
				#meta文件也要改名
				oldfile_meta = child+".meta"
				newfile_meta = newfile+".meta"
				os.rename(oldfile_meta, newfile_meta)
				index += 1
 
RenameFiles(path,prefix)

```
