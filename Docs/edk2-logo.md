# UEFI 固件更换 Logo

## 1. 安装 UEFITool 软件

从 https://github.com/LongSoft/UEFITool 下载安装 UEFITool 工具软件。
![](.//media/logo1.png)

## 2. 备份原 logo

#### 以下内容以龙芯 3A5000 参考板的固件为例，说明操作步骤，合作伙伴应使用合适的固件文件进行修改。

从 https://gitee.com/loongson/Firmware/blob/main/5000Series/PC/L5BMB01-CRB/UDK2018-LoongArch-CRB-pre-beta9.fd 下载固件文件到本地。
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
“old.bin”，此文件即为固件中原始的 logo。

![](.//media/logo4.png)

## 3. 检测原有 Logo 文件格式

考虑固件容量有限, 固件已经引入了压缩算法, 在替换固件 Logo 之前，**务必首先检测 Logo 文件是否为压缩格式**，以决定后续是否需要对新 Logo 进行压缩处理。

### 1. 使用 `file` 命令对保存的`old.bin`检测文件格式：

   ```sh
   file old.bin
   ```

   - 如果输出类似：
     ```
     old.bin: PC bitmap, Windows 3.x format, 450 x 120 x 24, resolution 2834 x 2834 px/m
     ```
     说明该文件为标准 BMP 格式，无需解压缩。

   - 如果输出类似：
     ```
     old.bin: LZMA compressed data, non-streamed, size 162294
     ```
     说明该文件已被 LZMA 压缩，需按压缩流程处理。

### 2. 按照检测结果操作

- **原 Logo 为未压缩格式：**
  - 新 Logo 可直接使用 BMP 格式替换。

- **原 Logo 为 LZMA 压缩格式：**
  - 新 Logo 需要先压缩，再进行替换。可使用 EDK2 工具进行压缩：
    ```sh
    edk2/BaseTools/Source/C/bin/LzmaCompress -e Logo.bmp -o Logo.bmp.lzma
    ```
基于EDK2的解压工具: edk2/BaseTools/Source/C/bin/LzmaCompress 第一次需要手动编译;
1. `git clone https://github.com/tianocore/edk2.git`
2. `cd edk2 & git submodule update --init BaseTools/ & make`
- **解压缩Un-Compress**: BaseTools/Source/C/bin/LzmaCompress -d old.bin -o old.bmp
- **压缩Compress**: BaseTools/Source/C/bin/LzmaCompress -e new.bmp -o new.bmp.lzma

![](.//media/logo5.png)

## 4. 创建新 logo

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
## 5. 替换 logo
**确保原有Logo是否经过压缩?** 如压缩,请参考[3. 检测原有 Logo 文件格式](检测Logo文件格式)将原数据进行**LZMA**压缩:
`edk2/BaseTools/Source/C/bin/LzmaCompress -e new1.bmp -o new1.bin`

在“备份原logo” 步骤中，选中的 “Raw section” 上右击，选择 menu:Replace body 菜单项， 选中新创建
的 logo 文件 new1.bin。
注files”意：。如果找不到 new1.bin 文件，需要将文件选择对话框中的文件类型从 “Raw files” 修改为 "All files"
![](.//media/logo8.png)


## 6. 保存新固件

### 选newlogo.fd”择 File › Save image file...。 ，保存当前文件为新固件，如 “UDK2018-LoongArch-CRB-pre-beta9-

将 “UDK2018-LoongArch-CRB-pre-beta9-newlogo.fd” 烧入机器，将看到新logo 生效。



