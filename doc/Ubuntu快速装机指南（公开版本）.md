# Ubuntu快速装机配置教程

部分软件源可能有变化，遇到问题请百度。另外最好安装下代理上网，方便下载。

## 一、 修改软件源

选择下载自阿里云镜像；然后更新软件系统

```shell
sudo apt-get update
sudo apt-get upgrade
```

## 二、依次下载常用的软件
### 1. git、cmake、vim等；

```shell
sudo apt-get install git cmake shutter unrar vim curl
```

### 2. 下载：SysPeek、classicmenu-indicator、typora

系统指示器SysPeek、经典菜单指示器、Markdown编辑器

```shell
sudo add-apt-repository ppa:nilarimogard/webupd8
sudo add-apt-repository ppa:diesch/testing
sudo add-apt-repository 'deb http://typora.io linux/'
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys BA300B7755AFCFAE
sudo apt-get update
sudo apt-get install syspeek
sudo apt-get install classicmenu-indicator
sudo apt-get install typora
```

### 3.  pdf阅读器 ：

```shell
sudo apt-get install okular
sudo apt-get install kde-l10n-zhcn
```

### 4. 下载搜狗拼音输入法：
```shell
http://cdn2.ime.sogou.com/dl/index/1524572264/sogoupinyin_2.2.0.0108_amd64.deb?st=JM3VRj2FkaQO5C8oMeJL6w&e=1544969413&fn=sogoupinyin_2.2.0.0108_amd64.deb
```

​       then,

```shell
sudo apt-get install gdebi
sudo gdebi sogoupinyinxxxx.deb
```
   最后在系统设置界面更新语言包；

### 5. 下载sublime_text_3：

参考 https://www.jianshu.com/p/6862ae9dccc5，推荐离线安装。

配置：https://blog.csdn.net/u014465934/article/details/72810763
    
https://blog.csdn.net/alxe_made/article/details/80756040
    
注意：包管理工具安装被墙后可以通过 https://packagecontrol.io/installation 离线安装

汉化：https://blog.csdn.net/qq_43722079/article/details/97777585

setting 配置：https://blog.csdn.net/g_wins/article/details/49509185

我的设置：
```
{
	"font_size": 15,
	"translate_tabs_to_spaces": true,
	"word_wrap": "auto",
	"highlight_line": true,
	"scroll_past_end": true,
	"trim_trailing_white_space_on_save": true,
	"ensure_newline_at_eof_on_save": true,
	"save_on_focus_lost": true,
	"bold_folder_labels": true,
	"font_face": "Microsoft YaHei",
	"highlight_line": true
}
```

### 6. 设置管理员权限：


    sudo passwd（首次进入设置密码）
     su root(以后要求进入管理员权限都这样输入命令)

## 三、显卡相关配置

配置驱动(drivers)、显卡工具箱(cuda)及深度学习加速库(cudnn)

### 1. 软件下载地址
drivers：https://www.geforce.cn/drivers;
cuda：https://developer.nvidia.com/cuda-toolkit-archive;
cudnn：https://developer.nvidia.com/rdp/form/cudnn-download-survey;

### 2.安装教程
（以10.2为例）
drivers：https://blog.csdn.net/lemonxiaoxiao/article/details/105688497；
cuda/cudnn：https://blog.csdn.net/lemonxiaoxiao/article/details/105693389；
+ 备注1：已预先下载好1060及1080Ti系列驱动，cuda8.0、cuda9.0、cuda10.2及对应的cudnn；

+ 备注2：显卡驱动在“软件和更新”中“附加驱动选项卡”中默认安装并使用nvidia384.130，手动更新驱动后被标记为手动安装。

+ 备注3：当重启后有比例显示不正确现象时，显卡驱动被自动调整到了xserver，此时重装驱动即可。

+ 备注4：keras、tensorflow-gpu、cuda、cudnn有版本匹配要求：推荐设置cuda9.0+cudnn7.0.5+TensorFlow-gpu1.5.0+keras2.1.4

+ 切换cuda环境：

    ```bash
    cd /usr/local
    sudo rm -rf cuda
    sudo ln -s ./cuda-9.2 ./cuda
    sudo gedit ~/.zshrc #修改位置
    source ~/.zshrc 
    nvcc -V
    ```

## 四、配置opencv

### 1. 安装依赖项：
#### 	opencv3.4.4为例
如果源码下载需要手动修改源文件，详见教程；但是文件夹下已经提供了修整好的版本：

教程：https://blog.csdn.net/weixin_43327725/article/details/105852173

辅助参考：https://zhuanlan.zhihu.com/p/38738976

下载速度过慢：https://blog.csdn.net/CSDN330/article/details/86747867

修正后下载后在build文件夹下（如果有东西请删清重来，下同）：
```shell
mkdir build
cd build
cmake -D CMAKE_BUILD_TYPE=Release -D CMAKE_INSTALL_PREFIX=/usr/local -D WITH_CUDA=ON - D PYTHON_EXECUTABLE=~/usr/bin/python3.5 -D INSTALL_PYTHON_EXAMPLES=OFF -D INSTALL_C_EXAMPLES=OFF -D OPENCV_ENABLE_NONFREE=ON -D OPENCV_EXTRA_MODULES_PATH=/home/demo/opencv_contrib-3.4.4/modules/ ..
（注意这里的opencv_contrib位置需要手动修改）
make -j5
```

（根据电脑的几核，量力而为）

```
sudo make install
```

带有opencv的CMakeList.txt的简单写法详见附录。

#### 	opencv2.4.11为例
如果源码下载需要手动修改源文件，详见教程；但是文件夹下已经提供了修整好的版本：

教程：https://blog.csdn.net/ljl1015ljl/article/details/100749835

修正后下载后在build文件夹下：

```shell
安装 gitlabcmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local/opencv2.4.11  -D WITH_CUDA=ON -D BUILD_PYTHON_SUPPORT=ON -D WITH_FFMPEG=OFF -D BUILD_EXAMPLES=ON   -D BUILD_TIFF=ON ..
make -j5
sudo make install
```
+ 默认安装配置为opencv3，当使用opencv2时，在CMakeList.txt中加入：
```
set(OpenCV_DIR /home/demo/opencv-2.4.11/build)
```

其中，/home/demo/opencv-2.4.11是本人安装opencv2的位置，修改对应位置即可。

+ opencv使用参考：http://www.opencv.org.cn/forum/

## 五、 虚拟容器环境配置

该部分主要用于配置深度学习

### 1. 更新依赖和配置

```shell
wget https://bootstrap.pypa.io/get-pip.py （安装py35不能下载最新版本）
sudo python3 get-pip.py
## or in ubuntu
sudo apt-get install python3-pip
sudo pip3 install virtualenv virtualenvwrapper
sudo rm -rf ~/.cache/pip get-pip.py
```

### 2. 配置文件：
```shell
sudo gedit ~/.bashrc
```
在文件结尾出写入：sudo apt install proxychains
```shell
export WORKON_HOME=$HOME/.virtualenvs
export VIRTUALENVWRAPPER_PYTHON=/usr/bin/python3
source /usr/local/bin/virtualenvwrapper.sh
```

更新配置：
```shell
source ~/.bashrc
```

### 3. 创建虚拟环境

```shell
mkvirtualenv dl4cv -p python3
```
当以后启用虚拟环境时，调用命令：
```shell
workon dl4cv
```
在虚拟容器中，可以随意更换不同版本的python、tensorflow、keras等

### 4. 虚拟环境切换python版本问题：

https://blog.csdn.net/be_clever/article/details/100917687

```bash
sudo add-apt-repository ppa:deadsnakes/ppa
sudo apt update
sudo apt install python3.6
sudo apt-get install python3.6-dev
python3.6 -V
mkvirtualenv py36 -p python3.6  # (之后就可以用workon py36登录)
```

+ 删除虚拟环境：没有使用virtualenvwrapper前，可以直接删除venv文件夹来删除环境

+ 其他虚拟环境参考：https://zhuanlan.zhihu.com/p/60647332

### 5. 更换pip国内源：

    参考：https://blog.csdn.net/Gushiyuta/article/details/95055884，推荐清华源。

### 6. 下载相关python库：

```bash
pip install numpy
pip install scipy matplotlib pillow
pip install imutils h5py requests progressbar2
pip install scikit-learn scikit-image
pip install tensorflow==1.3.0（当安装maskrcnn时不要安装太高，其他随意）
pip install tensorflow-gpu
pip install keras==2.0.8（注意和tf版本保持一致）
```
### 7. 链接opencv到虚拟环境中：
```shell
cd ~/.virtualenvs/dl4cv/lib/python3.5/site-packages/
ln -s /usr/local/lib/python3.5/site-packages/cv2.cpython-35m-x86_64-linux-gnu.so cv2.so
cd ~
```

## 六、跨系统数据传输

win与ubuntu之间的数据传输：https://blog.csdn.net/songyunli1111/article/details/79792958

## 七、ubuntu下windows虚拟机

安装QQ,微信,迅雷等windows应用:

+ 利用wine: https://blog.csdn.net/weixin_39583302/article/details/105617534
+ 配置qq出现问题：选择qq.im而不是qq2012
+ 配置微信出问题：降低微信版本

## 八、录屏软件

```
sudo apt-get install kazam
```

## 九、桌面主题、终端美化：

1. 参考：https://blog.csdn.net/YuYunTan/article/details/85052956#t9

2. 推荐其中的安装配置如下：
    + unity-tweak-tool主题：Arc-dark-solid
    + unity-tweak-tool图标：Ultra-flat
3. oh-my-zsh安装[解决GitHub的raw.githubusercontent.com无法连接问题](https://www.cnblogs.com/sinferwu/p/12726833.html)
    + oh-my-zsh主题：ys、透明度调整10%
5. 经典菜单指示器：如果美化主题后程序崩溃，可卸载重装
6. 使用zsh后，bashrc文件修改的部分需要复制到zshrc中
    + 切换：chsh -s /bin/zsh 、 chsh -s /bin/bash
7. docky：tansparent+智能隐藏

## 十四、CMAKE更新

1. 网址：https://cmake.org/files/v3.15/

2. 下载`cmake-3.15.0-Linux-x86_64.tar.gz`

3. ```bash
   sudo mv cmake-3.15.0-Linux-x86_64/ /opt/cmake-3.15.0
   sudo ln -sf /opt/cmake-3.15.0/bin/*  /usr/bin/
   cmake --version
   ```

##　十五、vscode设置

1. 字体：https://blog.csdn.net/study_in/article/details/104389345
2. 扩展：Chinese (Simplified) Language Pack for Visual Studio Code，Bracket Pair Colorizer 2， C++（以及扩展包）， Todo Tree， GitLens，Sublime Text Keymap and Settings Importer，Git Graphs, C++ Intellisense等





















