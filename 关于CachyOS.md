## 序
CachyOS的默认设置需要进行调整，以优化其使用体验

## 目录
- [字体](#字体)
- [家目录](#家目录)
- [系统设置](#系统设置)
- [第三方仓库](#第三方仓库)
- [装机必备](#装机必备)

## 字体
在CachyOS中，中文字符默认匹配的是韩语字形，所以我们要修改字体匹配规则
```bash
sudo pacman -S inter-font ttf-sarasa-gothic
wget https://github.com/szclsya/dotfiles/raw/refs/heads/master/fontconfig/fonts.conf
mkdir -p ~/.config/fontconfig
cp fonts.conf ~/.config/fontconfig
```

## 家目录
如果你是用中文安装CachyOS的，那么家目录下的常用文件夹也会变成中文的
对于CachyOS这种经常要用到终端的发行版，切个目录还得换输入法实在是太麻烦了
所以我们可以用下面这段命令，把常用文件夹变回英文
```bash
LC_ALL=C.UTF-8 xdg-user-dirs-update --force
```
然后在Dolphin里把常用位置都改成新的就好了

## 系统设置
1. 鼠标和触摸板，鼠标：禁用指针加速度
2. 锁屏，自动锁定屏幕：不自动锁屏
3. 电源管理，空闲时：无操作
4. 桌面会话，登录时自动启动应用程序：启动为空会话

## 第三方仓库
Arch Linux中文社区仓库是由Arch Linux中文社区驱动的非官方用户仓库
包含许多官方仓库未提供的额外的软件包，以及已有软件的git版本等变种
一部分软件包的打包脚本来源于AUR，但也有许多包与AUR不一样
这里用的是科大源，对于我来说下载速度快点
你可以根据自身情况设置其他镜像源
```bash
printf '%s' '[archlinuxcn]
Server = https://mirrors.ustc.edu.cn/archlinuxcn/$arch
' | sudo tee -a /etc/pacman.conf
```
然后通过下面这条命令导入GPG密钥
```bash
sudo pacman -Sy archlinuxcn-keyring
```

## 装机必备
基本设置完成后，我们可以装一些必备软件包来弥补系统本身缺失的功能

### Nerd Fonts
很多命令行工具会用到Nerd Fonts，来显示各种图标
```bash
sudo pacman -S paru
paru ttf-maplemono-nf-cn-unhinted
```
26.06版本的安装程序移除了paru，所以我们得把他装回来
当然，你也可以用CachyOS官方推荐的Shelly
不过，我个人是不太推荐
Shelly的GUI少了很多功能，CLI比paru麻烦
举个例子，paru安装软件包是这样的：
```bash
paru <(多个)软件包>
```
Shelly安装软件包是这样的：
```bash
shelly aur install <(多个)软件包>
```
再说回字体，我这里用的是[Maple Mono](https://github.com/subframe7536/maple-font/blob/variable/README_CN.md)
原因很简单，中英文2:1完美对齐且风格统一

要让终端用这个字体显示，在终端界面按`Ctrl+Shift+,`
配置方案，新建，勾选“默认配置方案”
外观，选择，勾选“显示所有字体”
然后在字体列表里选择“Maple Mono NF CN”，再确定就好了

其他终端如何配置请自行搜索

### 中文输入法
1. 安装软件：
```bash
sudo pacman -S fcitx5-im fcitx5-rime
```
2. 系统设置：
键盘，虚拟键盘，选择Fcitx 5 Wayland启动器，应用
3. 配置词库：
```bash
git clone --depth 1 https://github.com/gaboolic/rime-frost Rime
cp -r Rime/* ~/.local/share/fcitx5/rime
```
4. 重新部署：按`Ctrl`+空格键切换输入法，右键im图标，打开“朙月拼音”的菜单
点击重新部署即可
5. 美化主题：
```bash
git clone https://github.com/sanweiya/fcitx5-mellow-themes.git
mkdir -p ~/.local/share/fcitx5/themes && cp -r fcitx5-mellow-themes/kwinblur-mellow-* ~/.local/share/fcitx5/themes
```
系统设置，输入法，配置附加组件，经典用户界面，主题
选择自己喜欢的，应用

### 文件管理器
Dolphin对于大多数普通用户应该够用了
但是对于我来说，功能还是有点太少了
所以安装功能更加强大的Yazi来管理文件
Yazi内置zoxide方便快速跳转常用目录
fd可以快速定位文件位置，还有图片预览和智能解压缩等功能
```bash
sudo pacman -S --needed yazi ffmpeg 7zip jq poppler fd ripgrep fzf zoxide resvg imagemagick wl-clipboard
```
如果你想了解Yazi的自定义配置：[[关于Yazi]]
### 环境变量
个人向环境变量：
```bash
source /usr/share/cachyos-fish-config/cachyos-config.fish

# overwrite greeting
# potentially disabling fastfetch
function fish_greeting
    # smth smth
end

# yazi
function y
	set tmp (mktemp -t "yazi-cwd.XXXXXX")
	command yazi $argv --cwd-file="$tmp"
	if read -z cwd < "$tmp"; and [ "$cwd" != "$PWD" ]; and test -d "$cwd"
		builtin cd -- "$cwd"
	end
	command rm -f -- "$tmp"
end

# editor
set -gx EDITOR micro

# zoxide
zoxide init fish | source
```

## 游戏
### GameMode
[GameMode](https://github.com/FeralInteractive/gamemode)是一个Linux下的守护进程和库组合，允许游戏请求一组优化暂时应用于主机操作系统或游戏进程
1. 安装：
```bash
sudo pacman -S --needed gamemode lib32-gamemode
```
2. 配置：
```bash
sudo gpasswd -a curarpikt gamemode
```
