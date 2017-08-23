# HOTpatch使用方法 #

一、设置bios

   充分了解你的机器配置，正确设置bios,建议按图示设置。

二、使用方法

1. 修改配置文件

  根据你的具体情况修改SSDT-config*.aml文件。如：你的屏幕是1366X768，修改HIGH=0；如果你是T430，
则修改MODE=30.
详见[SSDT-Config配置说明](https://raw.githubusercontent.com/jzhjm/ThinkPad_HotPatch/master/HOT3.2综合补丁包(2017–08-16)/SSDT-Config配置说明3.2.md/)。

2. 简单使用方法

  下载包括HOTpatch的EFI文件包并完整替换你的ESP分区的EFI，注意是完整替换。
  这种方法通用，但是不一定适用你。比如，网卡、显卡、屏幕可能不同。注意保存好你原先的EFI，做好
恢复EFI的准备。

3. 正常使用方法

    1. 删除EFI\clover\ACPI\patched\所有文件，把“HOTpatch文件更新区”有关SSDT-xxx*.aml文件
   （适合你的）拷贝到该目录。

    2. 删除原来的config文件，替换成“HOTpatch文件更新区”和你机器匹配的config.plist文件。并根据你自己的情况进一步修改config文件。

    3. 关于睿频和变频\
自行决定使用下列方法之一：
1 clover自动方式：勾选PS和CS；
2 SSDT方式：不勾选PS和CS；正确提取你的SSDT.aml，放到EFI\clover\ACPI\patched\。

    4. 关于驱动\
  HOTpatch是补丁文件，用于说明你的机器有什么硬件和驱动硬件的方法，机器的正常工作还需要硬件的支持和正确的驱动。在EFI\clover\Kexts\Other\里应当有必要的驱动，请到下载你需要的驱动。

> 通用性驱动请到官网下载：https://bitbucket.org/RehabMan/
