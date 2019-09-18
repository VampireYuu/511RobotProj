*Copyright ZhuoyangDu,2018*

# Ubuntu16.04+Nvidia1080Ti+Nvidia390驱动+Cuda8.0+Cudnn6.0安装教程

参考教程：

https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html#running-binaries
https://blog.csdn.net/u012253191/article/details/78997529

## CUDA安装

- 验证GPU型号

    ```
    $ lspci | grep -i nvidia
    ```

- 根据当前型号下载对应的CUDA版本：https://developer.nvidia.com/cuda-downloads

  验证当前安装包是否是正确的：http://developer.nvidia.com/cuda-downloads/checksums 
    ```
    $ md3sum <file>
    ```

- 卸载原来的cuda和nvidia驱动

    卸载用runfile安装的cuda：
    
    ```
    $ sudo /usr/local/cuda-X.Y/bin/uninstall_cuda_X.Y.pl
    ```
    
    卸载用runfile安装的nvidia驱动：
    
    ```
    $ sudo /usr/bin/nvidia-uninstall
    ```
    
    卸载用RPM/Deb安装的nvidia驱动：
    
    ```
    $ sudo apt-get --purge remove nvidia-*
    ```
    
- 用runfile安装cuda

    首先禁用nouveau：创建文件`/etc/modprobe.d/blacklist-nouveau.conf`并写入：
    
    ```
    blacklist nouveau
    options nouveau modeset=0
    ```
    
    更新更改：
    
    ```
    $ sudo update-initramfs -u
    ```
    
    关机后重启进入boot，将boot中security设置为enable，开机之后输入：
    
    ```
    $ lsmod | grep nouveau
    ```
    
    没有输入则nouveau禁用成功。
    
    关闭图形界面：`Ctrl`+`Alt`+`F1`进入命令行界面（按`Ctrl`+`Alt`+`F7`可退出）
    
    禁用X服务：
    
    ```
    $ sudo service lightdm stop
        ```
    
    运行run文件：
    
    ```
    $ sudo chmod u+x cuda.run
    $ sudo ./cuda.run
    ```
    
    由于cuda8.0安装中自带的nvidia驱动版本过低，与1080Ti不兼容，在cuda的安装中注意先不要装nvidia驱动，其余可按默认。
    
    安装过程可能会报错，原因是缺少相关依赖，可apt-get安装即可。

    重启电脑，在~/.bashrc中加入环境变量：
    
    ```
    export PATH=/usr/local/cuda-8.0/bin${PATH:+:${PATH}}
    export LD_LIBRARY_PATH=/usr/local/cuda-8.0/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
    ```

- PPA安装nvidia驱动

    禁用nouveau，步骤同上。
    
    添加Graphic Drivers PPA：
    
    ```
    $ sudo add-apt-repository ppa:graphics-drivers/ppa
    $ sudo apt-get update 
    ```
    
    查找合适的版本：
    
    ```
    $ ubuntu-drivers devices
    ```
    
    其中一个driver后面加上了recommend字样，记住该版本，如下图，推荐版本是390。
    
    按`Ctrl`+`Alt`+`F1`进入命令行界面：
    
    ```
    $ sudo service lightdm stop
    $ sudo apt-get install nvidia-390
    ```
    
$     重启
    
    ```
    sudo reboot
    ```
    
    验证是否安装成功
    
    ```
    $ nvidia-smi
    ```

- 验证cuda和nvidia驱动是否安装成功

    执行下面命令可查看驱动版本：
    
    ```
    $ cat /proc/driver/nvidia/version
    ```
    
    编译cuda samples
    
    ```
    $ cd .../NVIDIA_CUDA-8.0_Samples
    $ make -j6
    $ cd bin/x86_64/linux/release/
    $ ./devideQuery
    ```
    
## CUDNN安装

- 从 https://developer.nvidia.com/cudnn 下载对应的压缩包，这里选择了v6.0

- 解压该文件至/usr/local/

    ```
    $ sudo tar -zxvf ~/Downloads/cudnn-8.0-linux-x64-v6.0.tgz -C /usr/local/
    ```
    
- 关联环境变量
    
    打开.bashrc
    
    ```
    $ cd ~
    $ sudo gedit .bashrc
    ```
    
    在最后一行加入
    
```
$ export LD_LIBRARY_PATH=/your/path/to/cudnn/lib64:$LD_LIBRARY_PATH
```
    
    保存后执行
    
```
$ source .bashrc
```
