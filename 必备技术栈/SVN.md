## 一、svn安装
在 windows 下安装 SVN，[准备 svn 的安装文件](https://sourceforge.net/projects/win32svn/)
![](https://www.runoob.com/wp-content/uploads/2016/08/svn-windows-install02a.gif)
一般会自动把 svn 安装目录里的 bin 目录添加到 path 路径中，cmd 输入 `svnserve --help`
![](https://www.runoob.com/wp-content/uploads/2016/08/windows-install02.png)
## 二、创建版本库
```bash
# 创建仓库
svnadmin create C:\Users\26908\Desktop\team\document
```
![[Pasted image 20231110154510.png]]
## 三、修改配置
进入 `config` 文件夹