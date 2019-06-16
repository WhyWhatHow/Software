[传送门](https://blog.csdn.net/asdwkl2584561379/article/details/46375325)

然后找到android studio 的目录打开bin目录编辑studio.sh,在文件的最上面加入下面三行:

```sh
export XMODIFIERS=”@im=fcitx” 
export GTK_IM_MODULE=”fcitx” 
export QT_IM_MODULE=”fcitx”

```