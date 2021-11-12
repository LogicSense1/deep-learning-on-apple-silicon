# deep-learning-on-apple-silicon
记录转向arm平台后的踩坑日常

## brew
In Terminal
```
uname -a
```
确认terminal运行在arm下
```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

## conda environment
下载并安装miniforge https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-MacOSX-arm64.sh

安装后即可使用conda的环境控制，此时默认channel应该拥有架构版本为`osx-arm64`，可以通过`conda info`确认

## torch
现阶段torch只包含cpu版本，并没有gpu/npu加速，使用conda即可安装
```
conda install pytorch torchvision -c pytorch
```

## scikit-learn
scikit-learn若使用pip安装会出现编译错误的问题，可以使用condo安装
```
conda install scikit-learn -c conda-forge
```

## Transformers
编译tokenizer之前需要安装rust
```
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```
之后就可以正常安装transformers
```
pip install transformers
```

## grpcio
需要导入以下变量，在进行pip安装时的编译wheel，否则会有关于gcc报错
```
export GRPC_PYTHON_BUILD_SYSTEM_OPENSSL=1
export GRPC_PYTHON_BUILD_SYSTEM_ZLIB=1
pip install grpcio
```

## 编译QT5
大部分可以参考 https://github.com/qbittorrent/qBittorrent/wiki/Compilation:-macOS#building-qt
其中需要修改
```
--- a/qtbase/src/plugins/platforms/cocoa/qiosurfacegraphicsbuffer.h
+++ b/qtbase/src/plugins/platforms/cocoa/qiosurfacegraphicsbuffer.h
@@ -43,4 +43,6 @@
 #include <qpa/qplatformgraphicsbuffer.h>
 #include <private/qcore_mac_p.h>
+ 
+#include <CoreGraphics/CGColorSpace.h>

 QT_BEGIN_NAMESPACE
```
