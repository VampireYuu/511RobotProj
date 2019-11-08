
*Copyright DongLi, 2018-2019, revised on 2019-11-08*

# 工作站现已升级为ubuntu18.04

   1. 升级后出现桌面环境无法使用的问题，具体解决方式可参考：
      https://blog.csdn.net/weixin_37851840/article/details/84503671
   
   2. 建议使用浙大校内镜像源：
      - 设置方式参考：https://blog.csdn.net/weixin_38481963/article/details/102750629
      - 最新镜像源地址请查询：http://mirrors.zju.edu.cn/
   
   3. 如重新安装系统，可参考下方步骤，并将ubuntu16.04替换为需要的版本
----

## 511工作站安装ubuntu16.04步骤

1. 选择一个镜像源下载ubuntu16.04桌面版64位镜像：

   - 官方镜像：<http://releases.ubuntu.com/16.04/ubuntu-16.04-desktop-amd64.iso>
   - 浙大镜像站：<http://mirrors.zju.edu.cn/>

   ​       <http://mirrors.zju.edu.cn/ubuntu-releases/16.04.4/ubuntu-16.04.4-desktop-amd64.iso>

2. 下载Ultraiso工具制作U盘启动盘

   具体参考<https://blog.csdn.net/yaoyut/article/details/78003061>

   ​      

   亦可采用其他工具制作U盘启动盘：

   ​       <https://jingyan.baidu.com/article/fedf0737bc587f35ac89771b.html>

3. 设置工作站BIOS

   - 开机后在主板界面按F2进入BIOS

   - 按F7进入高级设置界面，左右键移动至Boot菜单，选择Fast Boot模式为Disabled。

   - 向下移动Boot子菜单，进入CSM兼容性支持模块，选择Boot Device Control为UEFI and Legacy OPROM模式。

   - 返回Boot子菜单，进入CSM下方的Secure Boot模块，选择OS Type为 Other OS。

   - 上图中Secure Boot State已经为Disabled，一般情况不会变动，若进行主板恢复，Secure Boot State则为Enable状态，此时需要关闭此状态。

     参考：<https://jingyan.baidu.com/article/6dad50753a5c3da123e36e23.html>

     设置好以上步骤后，进入Key Management模块。

     点击 Clear Secure Boot Keys，即完成了对Secure Boot State的清除。

   -  进入Exit子菜单，选择Save Changes & Reset，或者直接F10进行保存并重启。

4. 重启

   设置好BIOS后，将制作好的U盘启动盘插入工作站主机，重启后再次进入BIOS界面。按F8进入Boot Menu，选择UEFI模式的U盘启动引导程序。若没有UEFI模式，则可能前序步骤设置错误或者尝试直接选择U盘引导。

5. 安装Ubuntu16.04
    
    - 从U盘引导后，选择Install Ubuntu。

    - 选择语言，推荐English版；推荐暂不安装第三方软件。

    - 后续根据需要自行选择，此处无展示图，可以参考网上教程。
        https://www.linuxidc.com/Linux/2016-04/130520.htm

    - 设置分区
    
        工作站SATA1口连接固态硬盘（500G），SATA2口连接机械硬盘（4T）。只安装单一ubuntu系统，需要设置引导，分区部分需要特别注意，最好按照以下推荐设置进行配置。

        - /home 分区，机械硬盘，主分区，500G

        - /boot 分区，固态硬盘，主分区，2G，Ext4日志文件系统

        - /swap_area 分区，固态硬盘，逻辑分区，80G

        - /EFI 分区，固态硬盘，主分区，1G

        - / 分区， 固态硬盘，主分区，剩余所有空间， Ext4日志文件系统

    - 后续根据需要设置时区、键盘布局、用户账号密码等等，等待安装完成后重启即可。

