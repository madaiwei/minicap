# MiniCap build指南

## 1. 便捷方案
官方已提供build便捷方案，若不修改jni/minicap-shared可如下build
```bash
# 获取Android版本
adb shell getprop ro.build.version.sdk
# 获取abi
adb shell getprop ro.product.cpu.abi

# 以SDK:30 Abi:arm64-v8a为例 成果物在libs目录
ndk-build APP_PLATFORM=android-30 APP_ABI=arm64-v8a
```


## 2. 自定义build方案(linux环境)
适用自己修改minicap源码后的build方案  
1. 环境准备(Ubuntu18.04)
```bash
sudo apt-get install git-core gnupg flex bison build-essential zip curl zlib1g-dev gcc-multilib g++-multilib libc6-dev-i386 libncurses5 lib32ncurses5-dev x11proto-core-dev libx11-dev lib32z1-dev libgl1-mesa-dev libxml2-utils xsltproc unzip fontconfig
```
2. 安装repo
```bash
curl https://mirrors.tuna.tsinghua.edu.cn/git/git-repo -o repo
chmod +x repo
# 设置repo镜像
export REPO_URL='https://mirrors.tuna.tsinghua.edu.cn/git/git-repo'
```
3. 下载对应AOSP源码，如果需要某个特定的 Android 版本([列表](https://source.android.com/setup/start/build-numbers#source-code-tags-and-builds))：
```bash
# 初始化
mkdir AOSP
cd AOSP
repo init -u https://mirrors.tuna.tsinghua.edu.cn/git/AOSP/platform/manifest -b android-11.0.0_r2
# 同步源码树
repo sync
```
4. build
```bash
# 放置路径如下
cp xxx/jni/minicap-shared/aosp Android_1100r2/external

# 进入aosp根目录
cd Android_1100r2

# build
. build/envsetup.sh
lunch

# 我这里确定为arm64所以输入以下命令
# lunch aosp_arm64-eng
# make 产物路径根据指定目标abi会有差异，按照本流程产物路径在/out/target/product/generic_arm64/system/lib64/minicap.so
make minicap -j <your_number_of_cpu>
```

