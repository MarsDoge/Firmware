# UEFI 固件更换 Logo

## 目录

1. 安装 UEFITool 软件...................................................................................  
2. 备份原 logo..........................................................................................  
3. 创建新 logo..........................................................................................  
4. 替换 logo............................................................................................  
5. 保存新固件..........................................................................................  

## 1. 安装 UEFITool 软件

从 https://github.com/LongSoft/UEFITool 下载安装 UEFITool 工具软件。
![](.//media/logo1.png)

## 2. 备份原 logo

#### 以下内容以龙芯 3A5000 参考板的固件为例，说明操作步骤，合作伙伴应使用合适的固件文件进行修改。

从 https://gitee.com/loongson/Firmware/blob/main/5000Series/PC/L5BMB01-CRB/UDK2018-LoongArch-
CRB-pre-beta9.fd 下载固件文件到本地。
![](.//media/logo2.png)

### 1.打开 UEFITool 菜单 File › Open image file...，选择固件文件 UDK2018-LoongArch-CRB-pre-beta9.fd。

### 2.打开 File › Search... 子菜单，在新打开的窗口中，选择 GUID 标签页

#### GUID 中输入: 7BB28B99-61BB-11D5-9A5D-0090273FC14D

Search scope 选中 Header only
点击 [ OK ] 开始查找。
如果找到此 GUID，在窗口下面部分的 “Messages” 区域会多出一行，内容大致为:
GUID pattern "7BB28B99-61BB-11D5-9A5D-0090273FC14D" found as ...
![](.//media/logo3.png)

3.双击 “Messages” 区域中找到的这行内容，窗口上部的 Structure 区域，会高亮 7BB28B99-61BB-11D5-
9A5D-0090273FC14D 行，双击此行展开，将显示 “Raw section” 行。
在 “Raw section” 行上右击，选择 “Extract body...”，在弹出的文件保存对话框中，将文件保存为
“old.bmp”，此文件即为固件中原始的 logo。

![](.//media/logo4.png)

$ file old.bmpold.bmp: PC bitmap, Windows 3.x format, 450 x 120 x 24, resolution 2834 x 2834 px/m,
cbSize 162294, bits offset 54

![](.//media/logo5.png)

## 3. 创建新 logo

将 old.bmp 复制为 new.bmp，并使用图片编辑器打开修改，保持图片大小不变，建议背景为黑色，以便效果
更佳。如：
![](.//media/logo6.png)

### 如果使用 GIMP 编辑，保存时，请使用 文件 › 导出为(X)...，在文件保存对话框中命名文件为 new1.bmp

#### 在B8”弹出。的 “导出图像为BMP” 设置窗口中，分别选中 “不要写入色彩空间信息” 和 “24位” “R8 G
![](.//media/logo7.png)


```
$ file *.bmpnew1.bmp: PC bitmap, Windows 3.x format, 450 x 120 x 24, image size 162240, resolution
2834 x 2834 px/m, cbSize 162294, bits offset 54
new.bmp: PC bitmap, Windows 3.x format, 450 x 120 x 24, resolution 2834 x 2834 px/m,
cbSize 162294, bits offset 54old.bmp: PC bitmap, Windows 3.x format, 450 x 120 x 24, resolution 2834 x 2834 px/m,
cbSize 162294, bits offset 54
```
## 4. 替换 logo

在“备份原logo” 步骤中，选中的 “Raw section” 上右击，选择 menu:Replace body 菜单项， 选中新创建
的 logo 文件 new1.bmp。
注files”意：。如果找不到 new1.bmp 文件，需要将文件选择对话框中的文件类型从 “Raw files” 修改为 "All files"
![](.//media/logo8.png)


## 5. 保存新固件

### 选newlogo.fd”择 File › Save image file...。 ，保存当前文件为新固件，如 “UDK2018-LoongArch-CRB-pre-beta9-

将 “UDK2018-LoongArch-CRB-pre-beta9-newlogo.fd” 烧入机器，将看到新logo 生效。



