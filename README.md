# OpenWrt 21.x 打印包编译指南

## 1. 准备 OpenWrt 源码环境

```bash
git clone https://github.com/openwrt/openwrt.git
cd openwrt
git checkout v21.02.7
cp feeds.conf.default feeds.conf
sed -i '1i\src-git xxx https://github.com/woniuzfb/openwrt-21-printing-packages.git' feeds.conf
sed -i '/--keep-system-cflags\|--keep-system-libs/d' tools/pkgconf/files/pkg-config
```

## 2. 下载配置文件

下载对应版本架构的 config.buildinfo：

```bash
wget https://downloads.openwrt.org/releases/21.02.7/targets/x86/64/config.buildinfo
cp config.buildinfo .config
```

## 3. 更新 feeds

```bash
./script/feeds update -a
./script/feeds install -a
# 退出并保存 .config
make menuconfig
```

## 4. 修复 b43-tools 编译问题

修改 `tools/b43-tools/Makefile`：

```makefile
# 把 +$(MAKE) $(HOST_JOBS) -C $(HOST_BUILD_DIR)/assembler 下面一行改成
CFLAGS="$(HOST_CFLAGS) -include endian.h -Wno-error=int-conversion"
```

## 5. 修复 ninja Python 兼容性问题

新增 `feeds/packages/devel/ninja/patches/101-fix_pipes.patch`：

<details>
<summary>点击展开 ninja Python 兼容性修复补丁</summary>

```patch
--- a/configure.py
+++ b/configure.py
@@ -23,7 +23,7 @@

 from optparse import OptionParser
 import os
-import pipes
+import shlex
 import string
 import subprocess
 import sys
@@ -264,7 +264,7 @@
 env_keys = set(['CXX', 'AR', 'CFLAGS', 'CXXFLAGS', 'LDFLAGS'])
 configure_env = dict((k, os.environ[k]) for k in os.environ if k in env_keys)
 if configure_env:
-    config_str = ' '.join([k + '=' + pipes.quote(configure_env[k])
+    config_str = ' '.join([k + '=' + shlex.quote(configure_env[k])
                            for k in configure_env])
     n.variable('configure_env', config_str + '$ ')
 n.newline()
```

</details>

## 6. 编译打印包

```bash
make dirclean
make toolchain/install -j $(nproc) V=s
make target/compile -j $(nproc) V=s

# 生成全部 ipk 包
make package/hplip/compile -j $(nproc) V=s
```

## 7. 安装到 OpenWrt 设备

编译完成的文件在 `bin/packages/x86_64/xxx/` 目录中。

将 ipk 文件上传到 OpenWrt 设备并执行：

```bash
opkg install *.ipk

# 惠普打印机安装插件
hp-plugin -i

# 连接打印机到 openwrt 后执行
hp-setup -i
```

## 注意事项

- 新版 OpenWrt 使用[这个仓库](https://github.com/woniuzfb/openwrt-24-printing-packages)
- 安装打印包 ipk 前删除 `kmod-usb-printer`，重启 OpenWrt
