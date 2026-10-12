# 第三方署名与运行库替换

Mouse Orchestra v0.2.38, Windows x64, 2026-10-13.

本说明区分应用原创内容与第三方组件，不为整个应用授予同一种许可证。
完整许可文本在发行包 `THIRD_PARTY` 和 `_internal/assets` 中；公开仓库也保留副本。

## Qt / PySide6 / Shiboken 6.11.2

Copyright The Qt Company Ltd. and contributors. 使用 LGPL-3.0 的动态链接版本。
随包提供 LGPL-3.0 和 GPL-3.0 的完整文本；其余 Qt 内嵌依赖的许可和版权通知保留于 THIRD_PARTY/Qt。
官网：https://doc.qt.io/qt-6/lgpl.html 。

这是目录式运行包，`_internal/PySide6` 与 `_internal/shiboken6` 保持可替换。
允许修改和替换第三方库，以及为调试这些修改进行逆向工程；没有签名或校验锁阻止替换。
先退出应用、备份目录，再用同一 Python/架构和 Qt/PySide6 ABI 的修改版 DLL/PYD 替换对应文件。
同时保持依赖 DLL 与插件版本兼容。Python 为 3.13.14，Windows x86-64；PySide6/Shiboken 均为 6.11.2。
源码内附 CMake 构建说明，Qt for Python 构建说明：https://doc.qt.io/qtforpython-6/building_from_source/index.html 。

发布页附件 `MouseOrchestra-v0.2.38-third-party-sources.zip` 提供下列对应版本的原始第三方源码归档，
附每个文件的 SHA-256 和原始地址；不包含本项目私有源码。免费官方下载地址也可直接使用：

- [qtbase 6.11.2](https://download.qt.io/archive/qt/6.11/6.11.2/submodules/qtbase-everywhere-src-6.11.2.tar.xz)
- [qtsvg 6.11.2](https://download.qt.io/archive/qt/6.11/6.11.2/submodules/qtsvg-everywhere-src-6.11.2.tar.xz)
- [qtmultimedia 6.11.2](https://download.qt.io/archive/qt/6.11/6.11.2/submodules/qtmultimedia-everywhere-src-6.11.2.tar.xz)
- [qtimageformats 6.11.2](https://download.qt.io/archive/qt/6.11/6.11.2/submodules/qtimageformats-everywhere-src-6.11.2.tar.xz)
- [qttranslations 6.11.2](https://download.qt.io/archive/qt/6.11/6.11.2/submodules/qttranslations-everywhere-src-6.11.2.tar.xz)
- [pyside-setup 6.11.2](https://download.qt.io/official_releases/QtForPython/pyside6/PySide6-6.11.2-src/pyside-setup-everywhere-src-6.11.2.tar.xz)

应用不用 Qt PDF/虚拟键盘/QML/Quick 或 FFmpeg 视频组件，公开包未收录它们。
音频使用 Qt 原生 Windows 后端，系统监听使用 Windows WASAPI。

## Python / NumPy / ONNX Runtime / PyInstaller

- Python 3.13.14，Python Software Foundation License 及自带第三方许可；THIRD_PARTY/Python.txt。
- NumPy 2.5.3，BSD-3-Clause 与自带第三方组件许可；THIRD_PARTY/NumPy 和包内 dist-info/licenses。
- ONNX Runtime 1.30.0，Microsoft / contributors，MIT；THIRD_PARTY/ONNX-Runtime.txt 和 ONNX-ThirdPartyNotices.txt。
- PyInstaller 6.22.3 引导程序，GPL-2.0-or-later 附引导程序分发例外；THIRD_PARTY/PyInstaller.txt。
  该例外允许所打包应用采用自身许可，不把应用自动变为 GPL 软件。
- Microsoft Visual C++ runtime 使用随上游 Python/Qt 运行库提供的可再发行版本。

## 音频模型与模板

- YAMNet，Google / TensorFlow models，Apache-2.0；ONNX 格式由 Audio Magic 转换，
  https://huggingface.co/audiomagic/yamnet-onnx ，固定修订 f25b741；未重训原网络。
  许可文本 `_internal/assets/yamnet/LICENSE`。
- AudioSet 标签，Google，CC BY 4.0；https://research.google.com/audioset/ 。
- `instrument_head.npz` / `shamisen_head.npz`：Mouse Orchestra 派生的实验线性权重，
  **CC BY-NC-SA 3.0，仅限非商业使用**。IRMAS 作者 Juan J. Bosch、Jordi Janer、
  Ferdinand Fuhrmann、Perfecto Herrera / MTG, Universitat Pompeu Fabra；
  https://www.upf.edu/web/mtg/irmas ，https://doi.org/10.5281/zenodo.1290750 。
  使用原 YAMNet 特征、重采样及拟合新头；不分发原录音，不表示原作者背书。
  完整署名与许可 `_internal/assets/yamnet/INSTRUMENT_HEAD_LICENSE.txt`、
  `_internal/assets/yamnet/SHAMISEN_HEAD.md`、THIRD_PARTY/shamisen-head.txt。
  删除这两个 npz 可撤除补充识别；其他试听与独立声部仍可使用，但这不授予应用原创内容商业许可。
- `orchestral_head.npz`：CC0，来自 Versilian Studios / Sam Gossner 与贡献者的 VSCO-2 CE CC0 样本，
  固定提交 440300901dfe9275fd84e0b7763af1f8443ae62e；https://github.com/sgossner/VSCO-2-CE 。
  重采样、提取嵌入并拟合小型线性头；源码许可 ORCHESTRAL_SOURCE_LICENSE.txt。
- `tuba_head.npz`：CC BY 4.0。TinySOL v1 作者 Carmine-Emanuele Cella、Daniele Ghisi、
  Vincent Lostanlen、Fabien Lévy、Joshua Fineberg、Yan Maresz；https://zenodo.org/records/3632193 。
  同时使用上述 VSCO 训练样本；派生转换与小头训练的说明在 TUBA_HEAD.md。
- `drum_templates.json`：Vincent “Tchackpoum” Sermone / Tchimera 的派生频谱统计，CC BY-SA 4.0；
  https://github.com/michaelwillis/tchimera-drum-kit ，固定提交 fc8a6daa22a20708de962e915ddc1ae17bd2aa32。
  下混、重采样、平均归一化频带；署名与许可 `_internal/assets/DRUM_TEMPLATES_LICENSE.md`。
- 木击模板：Joseph Sardin / BigSoundBank，CC0；https://bigsoundbank.com/drumsticks-1-s1488.html 。
  攻击窗的归一化谱统计；THIRD_PARTY/wood-click.txt。

上述模板和权重不是源录音。研究用歌曲、IRMAS/TinySOL原音频及仅评测录音不随包分发。
第三方作者不为本项目的识别结果背书。
