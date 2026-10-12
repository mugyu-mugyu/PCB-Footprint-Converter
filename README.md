# PCB Footprint Converter

输入单个立创商城料号，将对应的 EasyEDA 标准版封装及可用 STEP 模型转换为 Cadence Allegro 17.4 原生封装库。

当前版本：**v1.0.9**。Windows 64 位，需已安装并能正常使用 Allegro 17.4。安装包自带转换引擎，无需另外安装 Python 或 FreeCAD。

## 安装与使用

运行 `LCSC2Allegro174_Setup_1.0.9.exe`，安装后从桌面或开始菜单启动。输入料号，确认 Allegro 程序路径及输出目录，再点击“开始下载并转换”。一次转换一个料号。

勾选“自定义封装名”可以填写自己的名称；允许英文字母、数字、下划线、短横杠（减号 `-`）及等号 `=`，最多 31 个字符。生成 Allegro 库时将等号替换为短横杠，并在日志显示对照。PAD 命名为 `封装名称_pad_x.pad`，不同规格按首次出现顺序编号。

默认成功后清理中间文件，保留 `.dra`、`.psm`、`.pad`、可用的 `.step` 及必要 `.ssm`。STEP 是模型本体，绑定后仍需保留。失败时保留诊断文件。

将输出子目录分别加入 Allegro 的 `psmpath`、`padpath` 和 `steppath`。移动库后更新这些路径；已有 PCB 中的器件需更新封装。


完整版本更新说明见 [CHANGELOG.md](CHANGELOG.md)。

## v1.0.9 更新

修复 C19077395、C7420331、C456018、C27636748 的图形区域裁剪；保留 HDMI 板边参考线；USB 屏蔽焊盘按 5、6 生成并提供对照。C2691448 的自定义名允许输入 `=`，生成时转换为 `-`。详见 [v1.0.9 发布说明](outputs/RELEASE_NOTES_v1.0.9.md)。

## v1.0.8 更新

自定义封装名新增短横杠支持，例如 `C-0201`、`SOT-23`；DRA、PSM、PAD、STEP 文件名及引用保持一致。包括以短横杠开头的名称，命令行使用 `--name=-C0402`。详见 [v1.0.8 发布说明](outputs/RELEASE_NOTES_v1.0.8.md)。

## v1.0.7 更新

修复 Allegro 已有 STEP 库路径覆盖转换临时配置后出现的 `Cannot map STEP model`。新版使用本次模型的完整路径导入，并检查封装保存的模型名，避免找不到文件或误用已有同名模型。

C85923（C0201）和 C1525（C0402）已通过真实 Allegro 17.4 S039 转换、引脚核验、3D 绑定及移动库后的重新绑定检查。保留原有 Logo 与云图清理；两个电容模型的本体和电极几何检查通过。

详见 [v1.0.7 发布说明](outputs/RELEASE_NOTES_v1.0.7.md)、[使用说明](outputs/LCSC2Allegro174/使用说明.md)及 [验证记录](outputs/LCSC2Allegro174/验证记录.md)。本机结果不能覆盖所有器件或所有 Allegro 补丁版本。

## 升级与卸载

在原目录覆盖安装即可升级，保留已有设置与封装库。卸载可使用 Windows 设置、开始菜单入口或安装目录的 `unins000.exe`；卸载保留已生成的 Library。

## 源码

`outputs/LCSC2Allegro174/source/Gui.cs` 为 Windows 界面；`core.py`、`cli.py` 为下载与转换引擎；`step_cleanup.py` 为可识别标识清理；`installer/` 为 Inno Setup 安装工程。构建步骤见使用说明。

封装和模型来源于嘉立创EDA/EasyEDA 官方库。代码不包含 Cadence 安装文件，使用库数据时请遵循源文件声明及适用授权。
