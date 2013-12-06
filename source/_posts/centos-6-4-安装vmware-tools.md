title: "CentOS 6.4 安装VMware Tools "
id: 934
date: 2013-09-23 16:32:49
tags: 
- VMware Tools
categories: 
- linux
---

1、在VMWare图形界面中，将CentOS光驱设定为D:\Program Files (x86)\VMware\VMware Workstation\linux.iso，根据你的VM安装目录进行设定；
<!-- more -->
2、挂载光驱
<div>mount -t auto /dev/cdrom /mnt/cdrom  这命令就是把CentOS CDROM挂载在/mnt/cdrom目录中，就可以访问里面的内容了；</div>
<div></div>
<div>3、可能发生的问题：
<div>/mnt/cdrommount: mount point /mnt/cdrom does not exist --/mnt/cdrom目录不存在，需要先创建。</div>
<div></div>
<div>[root@hanfeng/]# cd /mnt</div>
<div>[root@hanfeng/]# mkdir -p /mnt/cdrom  <wbr />--创建/mnt/cdrom目录</div>
<div>[root@hanfeng/]# mount -t auto /dev/cdrom /mnt/cdrom  <wbr />--挂载CentOS CDROM挂载mount: block device /dev/cdrom is write-protected, mounting read-only --挂载成功。</div>
<div></div>
<div>4、
<div>使用光驱中的文件，进行安装</div>
<div></div>
<div>[root@hanfeng/]# cd /mnt/cdrom</div>
<div>[root@hanfeng /]# ls -a</div>
<div>[root@hanfeng/]# cp VMwareTools-8.6.1-19175.tar.gz /tmp</div>
<div>[root@hanfeng/]# cd /tmp</div>
<div>[root@hanfeng/]# tar zxpf VMwareTools-8.6.1-19175.tar.gz</div>
<div>[root@hanfeng /]# cd vmware-tools-distrib</div>
<div>[root@hanfeng vmware-tools-distrib]# ./vmware-install.pl</div>
<div>Creating a new installer database using the tar3 format.</div>
<div></div>
<div>Installing the content of the package.</div>
</div>
</div>
<div></div>
<div># 安装过程的画面，全部使用默认值，一直按 Enter 就对了</div>
<div></div>
<div>一直到出现：</div>
<div>To use the vmxnet driver, restart networking using the following commands:</div>
<div>/etc/init.d/network stop</div>
<div>rmmod pcnet32</div>
<div>rmmod vmxnet</div>
<div>depmod -a</div>
<div>modprobe vmxnet</div>
<div>/etc/init.d/network start</div>
<div></div>
<div>Enjoy,</div>
<div></div>
<div>--the VMware team</div>
<div></div>
<div>出现以上，则基本安装完!</div>
<div></div>
<div>5 shutdown -r now 重启</div>
<div></div>
<div>6 重新启动计算机再次登入之后，我们就会发觉到，当我们要离开 Guest OS 的时候，不再需要按「Ctrl + Alt」</div>
&nbsp;