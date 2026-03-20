<h1 align="center">云编译 Auto-OpenWrt</h1>

---

   ## 📚 目录
- [📖 动态 DNS](#-动态DNS)   
- [📖 云编译 Lede](#-云编译Lede)
- [📖 云编译 immortalwrt](#-云编译immortalwrt)
- [📖 云编译 immortalwrt-mt798x](#-云编译immortalwrt-mt798x)

---

## 📖 云编译Lede

1. 更新依赖包

   打开依赖包仓库进行更新：https://github.com/sky2016cn/AutoBuild-lede  （按照依赖包仓库说明进行更新）
   
2. 编译某个插件，在“xxx.config”文件添加
   
   例如：CONFIG_PACKAGE_luci-app-dockerman=y

3. 取消编译某个插件，在“xxx.config”文件添加
   
   例如：# CONFIG_PACKAGE_luci-app-dockerman is not set

4. 若要使用 Lean 的“luci-theme-argon”皮肤，在“xxx.config”文件添加
   
   CONFIG_PACKAGE_luci-theme-argon=y

   编译为默认皮肤：在“customize_x86.sh”和“customize.sh”恢复注释掉的“修改默认皮肤”

5. 若要使用“jerrykuku/luci-theme-argon/”皮肤，在“xxx.config”文件添加

   CONFIG_PACKAGE_luci-theme-argon=y
   
   然后在所有的“Build_xxx.yml”脚本中恢复注释掉的“luci-theme-argon”代码

   编译为默认皮肤：在“customize_x86.sh”和“customize.sh”恢复注释掉的“修改默认皮肤”

6. 编译完成后，请检查编译过程是否出错。
  
   尤其注意“Initialization environment”安装依赖包部分，展开来细看是否出错
   
---

## 📖 云编译immortalwrt

1. 编译前，可以选择immortalwrt分支编译，在脚本的拉取命令“-b openwrt-24.10”分支，替换成最新“-b openwrt-25.12”分支，即可编译openwrt-25.12

2.更新依赖包
- 方法1：自动更新（推荐，目前编译脚本使用此方法）
- 打开官网：https://github.com/immortalwrt/immortalwrt
- 左上角选择对应的版本号
- 找到“Method 2“ 脚本命令（该命令会自动安装依赖包并更新编译环境，与官方保持一致的编译环境，编译的固件更稳定）
   
- 方法2：手动更新（不推荐）
- 修改脚本：
- sudo bash -c 'bash <(curl -s https://build-scripts.immortalwrt.org/init_build_environment.sh)'
- sudo apt --fix-broken install -y
- 将上方官方脚本命令修改为下方手动更新命令
- sudo -E apt-get -qq update
- sudo -E apt-get -qq install $(curl -fsSL raw.githubusercontent.com/sky2016cn/AutoBuild-lede/master/depends-immortalwrt)
   
- 再打开：https://github.com/sky2016cn/AutoBuild-lede  （按照依赖包仓库说明进行更新）

3. 每个分支的依赖包不同，如果编译其他版本，进入immortalwrt官网，选择该版本分支

   点击下方“Setup dependencies via APT”，复制更新“depends-immortalwrt”文件

---

## 📖 云编译immortalwrt-mt798x

1. 更新依赖包
- 方法1：自动更新（推荐，目前编译脚本使用此方法）
- 打开官网：https://github.com/hanwckf/immortalwrt-mt798x
- 找到“Method 2“ 脚本命令（该命令会自动安装依赖包并更新编译环境，与官方保持一致的编译环境，编译的固件更稳定）
   
- 方法2：手动更新（不推荐）
- 修改脚本：
- sudo bash -c 'bash <(curl -sL https://build-scripts.immortalwrt.eu.org/init_build_environment.sh)'
- sudo apt --fix-broken install -y
- 将上方官方脚本命令修改为下方手动更新命令
- sudo -E apt-get -qq update
- sudo -E apt-get -qq install $(curl -fsSL raw.githubusercontent.com/sky2016cn/AutoBuild-lede/master/depends-immortalwrt_mt798x)
   
- 再打开：https://github.com/sky2016cn/AutoBuild-lede  （按照依赖包仓库说明进行更新）

2. 配置文件.config   
- 使用官方的 defconfig/mt7986-ax6000.config 作为基础配置，云编译的目录和官方目录不同，所以命令略有不同
- cp -f immortalwrt/defconfig/mt7986-ax6000.config immortalwrt/.config
    
- 追加你要的插件配置
- cat >> immortalwrt/.config <<EOF
- CONFIG_PACKAGE_luci-app-msd_lite=y
- CONFIG_PACKAGE_luci-app-openclash=y
- CONFIG_PACKAGE_luci-app-passwall=y
- CONFIG_PACKAGE_xray-core=y
- CONFIG_PACKAGE_luci-app-passwall_INCLUDE_Xray=y
- CONFIG_PACKAGE_luci-app-passwall_INCLUDE_ChinaDNS_NG=y
- CONFIG_PACKAGE_kmod-tun=y
- CONFIG_PACKAGE_curl=y
- CONFIG_PACKAGE_ca-bundle=y
- EOF
  
---

## 📖 动态DNS

- 按需添加到 config 文件中

- 二选一
1. 动态 DNS
- CONFIG_PACKAGE_ddns-scripts-cloudflare=y
- CONFIG_PACKAGE_luci-app-ddns=y

2. ddns-go
- CONFIG_PACKAGE_luci-app-ddns-go=y

---
