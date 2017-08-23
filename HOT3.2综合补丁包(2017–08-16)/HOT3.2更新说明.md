# HOT3.2综合文件包更新说明和使用说明
**重要更新**
---
本文件包为综合性文件包，适用于Thinkpad系列机器（T410系列除外）、宏基Acer部分机器。

---
## 第一部分：HOT3.2综合文件包更新说明

### 一、更新内容：
1. 随RM的更新做了同步更新。
2. 进一步简化了电池补丁SSDT-BATT_Think。
3. 整合了独显屏蔽补丁SSDT-NDGP#。
4. 新增SSDT-SHUT#USB补丁，彻底解决关机问题。（用法见部分补丁文件的使用方法）

### 二、变频和睿频
   使用SSDT.aml实现变频和睿频。

### 三、关于驱动
   删除FakePCIID_XHCIMux.kext。

### 四、选择机型：
   建议机型设定为与CPU相匹配的机型。

---
## 第二部分：HOT3.2综合文件包使用说明

### 一、文件包内容：

   文件包包括：“公共包”、“公用/选用”、“E系列”、“WTX系列”、“WTX可选”、“USB端口”以及2个配置文件。

### 二、文件包的使用

根据你的机器选择正确的HOTpatch文件和config.plist：

1. WTX系列机器=“公共包”+“公用/选用”+“WTX系列”+"USB端口"+“WTX可选”+“WTX系列config.plist”；

2. E系列机器=“公共包”+“公用/选用”+“E系列”+"USB端口"+“E系列config.plist”；

3. 宏基系列机器同属E系列。

4. 本文件包不支持“T410系列”，有需要的请向作者或者“修缘”索取文件包。

### 三、部分补丁文件的使用方法

1. “公用/选用”文件包：
  1. SSDT-HDAU#45————4、5代机器需要添加该文件；
  2. SSDT-NDGP#————如果需要屏蔽独显，添加该文件；
  3. SSDT-CPUPlug#4+————4代以上CPU添加该文件；
  4. SSDT-IMEI_0x16和SSDT-IMEI_Drop16————二选一，选用方法如下：
    在DSDT中，搜索0x00160000,如果搜索到了0x00160000，添加SSDT-IMEI_Drop16，
    反之，添加SSDT-IMEI_0x16。3代机器应当选用SSDT-IMEI_0x16；
  5. SSDT-SHUT#USB补丁用法：
    - SSDT-SHUT#USB补丁的作用是，在关机期间，为了防止USB3.0设备触发重启而注销掉USB3.0。
      因使用SSDT-SHUT#USB会和FakePCIID_XHCIMux.kext发生冲突。所以，当你使用SSDT-SHUT#USB时，必须删除FakePCIID_XHCIMux.kext。
    - SSDT-config文件的XPEE控制着SSDT-SHUT#USB补丁启用或者不启用，建议设置XPEE=1。对于非Thinkpad笔记本，如果XPEE=1机器出现异常，设置XPEE=0。
    - 因为删除了FakePCIID_XHCIMux.kext驱动，USB信息里将显示USB设备所在的接口位置，不再和总线关联。

2. “WTX可选”文件包：
  1. SSDT-9285-WTX————9285无线网卡补丁；
  2. SSDT-4360-WTX————4360无线网卡补丁；
  3. SSDT-BATTCD_WTX————第二块电池补丁，T440s,T450s,T460s,T470s等需添加SSDT-BATTCD_WTX。

### 四、SSDT-Config#32.aml配置文件：

   SSDT-Config#32.aml的配置使用参见《SSDT-Config配置说明3.2》。

### 五、config.plist配置文件

- 文件包提供了“WTX系列config”和“E系列config”两个配置文件。请正确选用。
- 对于config.plist的修改请在提供的模版文件基础上修改。一般需要做如下修改：
  1. 修改你需要的机型。
  2. 在KexttoPatch区域，修改勾选项目。
  3. Skylake及以后的CPU应开启完整HWP(SpeedShift)电源管理特性，即：config.plist/CPU/HWPEnable=Yes，并添加**SSDT-CPUPlug#4+**。

### 六、睡眠模式设置为0
终端：`sudo pmset -a hibernatemode 0`
---
***END***







---
# 以下是早期HOTpatch的相关说明

HOTpatch-3.1更新说明
-----
一、更新内容：
1 更新显卡补丁SSDT-IGPU.aml为SSDT-IGPU#.aml，增加了对HD4400以及以后的HD520，HD620的支持。
2 更新并简化了电池补丁为SSDT-BATT_Think，支持目前已知的Thinkpad的WTX系列和E系列机器，需MODE配合。
3 增加SSDT-CMOR_WTX，屏蔽DSDT无用的风扇方法。
4 增加了E系列的独显屏蔽补丁SSDT-NDGP_Exx。
5 恢复USB总线补丁：SSDT-EHCx_Think。
6 恢复LPC的ID为7系ID，WTX为QM77，E系列为HM77。
7 删除了SSDT-PluginType3+补丁，需要的官网自行下载。
8 重要更新：SSDT-PTSWAK更新为SSDT-PTSWAKnew，下面是简单描述：
  SSDT-PTSWAK为睡眠和唤醒补丁。原理是：首先屏蔽DSDT原来的睡眠和唤醒部分，由SSDT-PTSWAK接管，在执行了
一些工作后再交回DSDT。新的SSDT-PTSWAK补丁在睡眠过程和原先补丁一样，然而，唤醒部分变更为完全废除DSDT，
由苹果继续处理，这样做是尽可能的保证睡眠前和唤醒后诸多状态的一致性，也许有弊端。文件包的保留部分包括了旧
的SSDT-PTSWAK。

二、对“Drop Oem”选项盖棺定论

原文：
With static patching, DropOem=true is used and patched DSDT and SSDTs are added to ACPI/patched. 
With hotpatch, instead use DropOem=false, and only add-on SSDTs are placed in ACPI/patched.

    对于笔记本，尽可能不要勾选“Drop Oem”，否则会屏蔽一些重要的ACPI文件。然而，某些机器却因此无法正常
进入系统。那么，我们的做法是：在“Drop Tables”添加你要屏蔽的ACPI的文件，而不是所有的ACPI文件。aml文件的
“OEM Table ID” 就是你要屏蔽这个文件的“TableID”号。
    为了变频，我们需要添加了自己SSDT.aml，“Drop Tables”中首先要屏蔽的文件ID是“CpuPm”。还是否需要屏
蔽其它文件，根据你的具体情况决定。
    通常的做法是只屏蔽“CpuPm”和“Cpu0Ist”即可。其中“Cpu0Ist”也可以不屏蔽。

三、变频和睿频

    根据以上，必须提取你自己的SSDT.aml并添加到patched，不要勾选"PS"和“CS”。

四、USB电源管理

    10.12以后的机器，对USB的电源管理由机型决定。现在知道的是：9,1、9,2、10,1、10,2、11,x、12,1
等机型的USB电源管理参数完全相同，USB在这些机型下表现一致。
    6,1和6,2机型的USB电源管理参数不同，T410系列的机器可以尝试6,1或者6,2机型，以决定哪个更适合你。
    8,1和8,2机型的USB电源管理参数相同，与8,3不同，420系列参考使用。
    10.12对13,x机型未提供电源管理参数，对于T460以及以后的机器需要仿冒USB电源参数。

五、更新的驱动

  声卡驱动更新为合集版的AppleALC.kext，支持410至460的Thinkpad。该驱动由慕容制作。
  企鹅制作了Thinkpad黑苹果安装包，用于HOTpatch一键式安装，简单方便。
  在此，感谢二位的辛苦和付出！感谢二位为Thinkpad黑苹果作出的贡献！
​    
  随着Lilu越来越强大，强烈建议Broadwell(HD6200)之前CPU使RM的FakePCIID_Intel_HD_Graphics.kext。
而之后的CPU使用Lilu的IntelGraphicsDVMTFixup.kext。

  另外，使用了Lilu的IntelGraphicsFixup.kext将有助于改善开机8苹果问题。

  FakePCIID_Intel_HD_Graphics.kext必须和FakePCIID.kext同时使用。
  IntelGraphicsDVMTFixup.kext和IntelGraphicsFixup.kext必须和Lilu.kext同时使用。


六、两个重要的配置文件的配置

  对于HOTpatch，首先，根据你的具体情况配置SSDT-Config#31.aml和config.plist文件。然后，选择适合你
的SSDT-*.aml文件。
   对于机型：
   建议机型设定为与CPU相匹配的机型。
   如：410系列为MacBookPro6,1或者MacBookPro6,2。
   如：420系列为MacBookPro8,1或者MacBookPro8,2。  
   如：430系列为MacBookPro9,1、MacBookPro9,2、MacBookPro10,1或者MacBookPro10,2。
   如：440系列为MacBookPro11,1、MacBookPro11,2。
   如果3代机器机型设定为MacBookPro11,1或者MacBookPro11,2，请仿冒Haswell，开启XCPM。

   关于SSDT-Config#31.aml的使用参见《SSDT-Config配置说明(3.1)》。
   对于config,plist的修改请在提供的模版文件基础上修改并正确选择KexttoPatch。

七、文件包的使用

根据你的机器选择对应的HOTpatch文件和config.plist：

1: WTX系列机器=“公共包”+“WTX系列”+"USB端口"+“WTX可选”+“WTX1-4config.plist”；

2: E系列机器=“公共包”+“E系列”+"USB端口"+“E系列可选”+“E系列config.plist”；

3: 关于“WTX可选”补丁的使用
  “WTX可选”补丁包括：
  a SSDT-9285-WTX——9285无线网卡补丁
  b SSDT-BATTCD_WT——第二块电池补丁
  c SSDT-NDGP_WTX——独立显卡屏蔽补丁，BIOS中设置双显模式
  d SSDT-SHUT_WTX——强制关机补丁，见“BIOS设置5-关机“图
4: 保留部分包括旧方法的睡眠和唤醒补丁：SSDT-PTSWAK

以上补丁根据你的实际情况选择，选择后添加到patched。

八、睡眠模式设置为0
终端：`sudo pmset -a hibernatemode 0`

九、适用
适用机型：Thinkpad W、T、X系列的1代、2代、3代、4代以及E系列机器
适用系统：10.11、10.12和10.13
适用屏幕：1366X768，1600X900，1920X1080，2560x1440

****END****
