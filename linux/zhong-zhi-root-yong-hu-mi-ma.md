# RHEL系统重置root用户密码

1. 第一步: 重启Linux系统主机并出现引导界面时，按下键盘上的e键进入内核编辑界面，如图所示。

   1. 

1. 第二步:在linux16参数这行的最后面追加“rd.break”参数，然后按下Ctrl + X组合键来运行修改过的内核程序

2. 第三步:大约30秒过后，进入到系统的紧急求援模式

3. 第四步:依次输入以下命令，等待系统重启操作完毕，然后就可以使用新密码linuxprobe来登录Linux系统了

```bash
mount -o remount,rw /sysroot
chroot /sysroot
passwd
touch /.autorelabel
exit
reboot
```

执行结果如下:

![](/assets/root4.png)

