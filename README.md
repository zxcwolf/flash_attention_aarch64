# flash_attention_aarch64
适用于ARM64(aarch64)的flash_attention包，基于flash_attention2.8.3重新编译的zip ,亲测可用

因为https://github.com/Dao-AILab/flash-attention 只有x86机器安装包，我的arm64机器无法安装。

# 安装教程
# 先下载flash_attention_complete_source.zip，并上传服务器

unzip flash_attention_complete_source.zip
cd flash-attention

#这里MAX_JOBS=1很重要，我128G 在>1都崩溃了
MAX_JOBS=1 pip install . --no-build-isolation --no-cache-dir

##可以先设置swap 交换空间防止失败。
fallocate -l 16G /swapfile
chmod 600 /swapfile
mkswap /swapfile
swapon /swapfile

# 附上源码重新编译打包教程，以后更新使用。
步骤 1: Git 克隆 FlashAttention 仓库
你需要先在一台联网的机器上（或如果你的 Spark 机器可以联网）

git clone https://github.com/Dao-AILab/flash-attention.git
cd flash-attention

步骤 2: 下载所有子模块 (包括 CUTLASS)
这一步至关重要，它会去下载 csrc/cutlass 目录所需的文件
git submodule update --init --recursive

这一步windows 会出现文件名过长的情况
"""
git clone https://github.com/Dao-AILab/flash-attention.git
cd flash-attention
git checkout v2.8.3

git submodule update --init --recursive
"""

##文件名过长问题：
系统层面 (Windows 注册表/组策略): 
将 HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\FileSystem 下的 LongPathsEnabled 设置为 1。这告诉新的 Windows API（UWP、某些 Win32 API）不再强制执行 260 字符的限制。
应用层面 (Git 配置): 执行 git config --global core.longpaths true，这告诉 Git 在执行文件操作时使用支持长路径的 API 或路径格式。


###清除失败目录（必要时）
##Remove-Item -Path csrc\composable_kernel -Recurse -Force
##Remove-Item -Path csrc\cutlass -Recurse -Force

##打包上传
# 导航到父目录
cd D:\fa_src
# 打包整个文件夹
Compress-Archive -Path .\flash-attention\ -DestinationPath .\flash_attention_complete_source.zip
