### 本地编译

> #### **⚠️ 注意事项**
> - **请勿使用 `root` 用户进行编译！**
> - **国内用户编译前建议开启网络代理。**
> - **编译环境要求：至少 4GB 内存和 25GB 可用磁盘空间。**
> - **默认配置：**
>   - 登陆 IP: `10.0.0.1`
>   - 密码: `none`

#### 1. 系统环境准备
推荐使用 **Ubuntu 22.04 LTS** 或 **Debian 11** 系统。

#### 2. 安装编译依赖
```bash
sudo apt update -y
sudo apt full-upgrade -y
sudo apt install -y ack antlr3 asciidoc autoconf automake autopoint binutils bison build-essential \
bzip2 ccache cmake cpio curl device-tree-compiler fastjar flex gawk gettext gcc-multilib g++-multilib \
git gperf haveged help2man intltool libc6-dev-i386 libelf-dev libglib2.0-dev libgmp3-dev libltdl-dev \
libmpc-dev libmpfr-dev libncurses5-dev libncursesw5-dev libreadline-dev libfuse-dev libssl-dev libtool lrzsz \
genisoimage msmtp nano ninja-build p7zip p7zip-full patch pkgconf python3 python3-pip libpython3-dev qemu-utils \
rsync scons squashfs-tools subversion swig texinfo uglifyjs upx-ucl unzip vim wget xmlto xxd zlib1g-dev
```

#### 3. 下载源码与配置
```bash
# 克隆源码
git clone --depth 1 --single-branch https://github.com/QuickWrt/immortalwrt-ipq.git
cd immortalwrt-ipq

# 更新 feeds
./scripts/feeds update -a && ./scripts/feeds install -a

# 进入菜单配置
make menuconfig
```

#### 4. 首次编译
```bash
# 下载依赖库 (多线程)
make download -j$(nproc)

# 开始编译 (建议使用单线程，方便排查错误)
make -j1 V=s
```
编译完成后，固件位于 `bin/targets` 目录下。

#### 5. 二次编译
```bash
cd immortalwrt-ipq

# 拉取最新代码
git fetch && git reset --hard origin/master

# 更新 feeds
./scripts/feeds update -a && ./scripts/feeds install -a

# 加载默认配置并编译
make defconfig
make -j$(nproc) V=s 
```

#### 6. 重新配置
如果需要修改配置，请执行以下命令：
```bash
# 清除旧配置
rm -rf .config

# 进入菜单配置
make menuconfig

# 开始编译
make -j$(nproc) V=s 
```
---
