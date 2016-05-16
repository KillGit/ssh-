1.安装ssh.  sudo apt-get install ssh. 安装完成后会在~目录（~/.ssh）下产生一个隐藏文件夹.ssh（ls -a 可以查看隐藏文件）。如果没有这个文件，自己新建即可（mkdir .ssh）.

2.进入.ssh目录下面，在每台机器上执行：ssh-keygen -t  rsa  之后一路回车，产生密钥；

3.完成第二步后会产生两个文件：

id-rsa     #私钥
id-rsa.pub   #公钥
4.在第一台机器的目录.ssh下执行命令，cat  id-rsa.pub >> authorized_keys；此后.ssh下面会出现authorized_keys文件。

5.然后将第一台机器的.ssh目录下面的authorized_keys文件拷贝到第二台计算机的.ssh目录下，如：scp authorized_keys root@slave1:~/.ssh/

6.再转到第二台机器的.ssh目录下，会发现刚刚传输过来的文件-authorized_keys，然后执行命令，将第二台计算机的公钥也加进来，如：cat id-rsa.pub >> authorized_keys.

7.将第二台计算机新生成的authorized_keys传输第三台计算机，将第三台计算机的公钥-id-rsa.pub添加到从第二台计算机传过来的authorized_keys里面。

8.依次类推，直至集群中的最后一台计算机。

9.在集群的最后一台计算机执行完添加后，生成的authorized_keys文件就包含集群中所有计算机的公钥，如果以后还有机器加进到集群中来，可以直接添加到文件-authorized_keys。最后，将最后生成的authorized_keys复制到集群中的每一台计算机的.ssh目录下，覆盖掉之前的authorized_keys。

10.完沉第九步后，就可以在集群中任意一台计算机上，免密码ssh登录到其他计算了。
