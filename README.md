# Automated Redpill Loader (i18n)

本库为 arpl i18n (多语言优化版): 

### 原版：
<b>https://github.com/syno-community/arpl</b>
* [作者说明(En)](./arpl-README-En.md)
* [作者说明(Zh)](./arpl-README-Zh.md)

### 汉化：
<b>https://github.com/wjz304/arpl-zh_CN</b>
* 仅同步汉化原版, 所以功能与原版保持一致.

### i18n: 
<b>https://github.com/syno-community/arpl-i18n</b>
* 多语言支持.
* 包含我的修改.


## 说明
* ### [命令输入方法演示](https://www.bilibili.com/video/BV1T84y1P7Kq)  https://www.bilibili.com/video/BV1T84y1P7Kq  
* arpl各版本间切换(菜单更新, 增量):  
    ```shell
    # shell 下输入以下命令修改更新 repo. 
    # 如果要切换原版修改第二条命令中的 syno-community/arpl-i18n 为 fbelavenuto/arpl
    # 如果切换中文版修改第二条命令中的 syno-community/arpl-i18n 为 wjz304/arpl-zh_CN
    CURREPO=`grep "github.com.*update" menu.sh | sed -r 's/.*com\/(.*)\/releases.*/\1/'`
    sed -i "s|${CURREPO}|syno-community/arpl-i18n|g; s|ACTUALVERSION=\"v\${ARPL_VERSION}\"|ACTUALVERSION=\"v0.0\"|g" /opt/arpl/menu.sh
    # 进入设置菜单执行更新arpl操作即可.
    # 更新后请重启.
    ```
* arpl各版本间切换(手动方式, 全量):  
    ```shell
    # shell 下下载需要的版本或者手动上传到/opt/arpl/下
    curl -kL https://github.com/fbelavenuto/arpl/releases/download/v1.1-beta2a/arpl-1.1-beta2a.img.zip -o /opt/arpl/arpl.zip
    # 解压
    unzip /opt/arpl/arpl.zip
    # 挂载 img
    losetup /dev/loop0 /opt/arpl/arpl.img
    # 复制 p1 p3 分区
    mkdir -p /mnt/loop0p1; mount /dev/loop0p1 /mnt/loop0p1; cp -r /mnt/loop0p1/* /mnt/p1/; umount /mnt/loop0p1
    mkdir -p /mnt/loop0p3; mount /dev/loop0p3 /mnt/loop0p2; cp -r /mnt/loop0p3/* /mnt/p3/; umount /mnt/loop0p3
    # 卸载 img
    losetup -d /dev/loop0
    # 如果安装的版本中无你当前安装的DSM请尽量删除 /mnt/p1/user-config.yml, /mnt/p3/*-dsm, /mnt/p2/*
    rm -rf /mnt/p1/user-config.yml /mnt/p3/*-dsm /mnt/p2/*
    # 重启
    reboot
    ```


## 翻译
```shell
sudo apt install gettext
git clone https://github.com/syno-community/arpl-i18n.git
cd arpl-i18n/files/board/arpl/overlayfs/opt/arpl
xgettext -L Shell --keyword=TEXT *.sh -o lang/arpl.pot
sed -i 's/charset=CHARSET/charset=UTF-8/' lang/arpl.pot    # The above process has been completed.
msginit -i lang/arpl.pot -l zh_CN.UTF-8 -o lang/zh_CN.po    # Replace the language you need.
# translate the lang/zh_CN.po.
msgfmt lang/zh_CN.po -o lang/zh_CN.mo    # This process will be automatically processed during packaging.
```




