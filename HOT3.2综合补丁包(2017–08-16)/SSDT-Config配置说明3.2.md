# SSDT-Config#32.aml 配置说明

- TYPE:机器类别
0: 台式机
1: 笔记本 调用文件:SSDT-IGPU.aml

- HIGH:屏幕分辨率
0: 低分辨率或其他
1: 高分辨率 1600x900+或者 Haswell/Broadwell 
调用文件:SSDT-IGPU.aml

- IGPI: 强制注入显卡 ID《慎用》
0:无注入
数字: 注入的 ID(ig-platform-id 等)如:0x19160000
调用文件: SSDT-IGPU.aml

- AUDL: 声卡 ID
数字:Audio LayoutID
Ones: 不注入 ID
调用文件:SSDT-HDEF.aml 

- BKLT: 亮度驱动
0: 使用 IntelBacklight.kext
1: 使用 AppleBacklight.kext 和 AppleBacklightInjector.kext 
调用文件:SSDT-PNLF.aml

- LMAX: 最大亮度补偿
Ones: 默认设置(Ivy/Sandy:0x710;Haswell/Broadwell:0xad9) 其他:必须和 framebuffer 匹配。如:Skylake/KabyLake=0x56c 
调用文件:SSDT-PNLF.aml

- MODE: Thinkpad 机器型号(十六进制)
0x10:一代机器
0x20:二代机器
0x30:三代机器
0x40:四代机器
。。。。。。依次类推
Thinkpad E 系列加 0x05。例如:E 系列 3 代 CPU，MODE=0x35 
调用文件: SSDT-BATT_Think.aml

- XPEE: 关机期间注销 USB3.0(解决关机重启问题) 0:不启用该功能
1:启用该功能
调用文件: SSDT-PTSWAKnew.aml

- FBTP: 亮度补偿类型
0:基于系统 ID
1:Ivy/Sandy 
2:Haswell/Broadwell/Skylake/KabyLake 
调用文件: SSDT-PNLF.aml

- XKEY: 键盘驱动(亮度快捷键修补) 
0:使用 VoodooPS2Controller.kext 
1:使用 ApplePS2SmartTouchPad.kext 
调用文件: SSDT-KEY_think.aml