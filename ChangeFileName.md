### 非递归版本(以实现）
```py
#coding:utf-8
import os
import traceback
def reName(): 
    while 1:
        try: 
            # 1获取重命名的文件夹
            folder_name = input('请输入文件夹的名字：')
            print("folder_name:" +folder_name)
            run(folder_name)
        except:
             print("Unexpected error:", sys.exc_info()[0])
            
def run(folder_name): 
#2获取文件夹中的文件名
    file_names = os.listdir(folder_name)
    os.chdir(folder_name)
    for name in file_names:
            print("name: "+name)
            flag = name.find("2019考研")
            findflag =name.rfind('.')    #找到文件属性后缀
            print (str(flag))
            if flag != 0: 
                new_name = name[0:flag-1]+name[findflag:]
                print(new_name)
                os.rename(name,new_name)
                print("success---")
    os.chdir("../")
def main():
    reName()
if __name__ == '__main__':
    main()

### 递归