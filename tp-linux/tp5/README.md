Windows PowerShell
Copyright (C) Microsoft Corporation. Tous droits réservés.

Installez la dernière version de PowerShell pour de nouvelles fonctionnalités et améliorations ! https://aka.ms/PSWindows

PS C:\Users\vende> ssh edgy@10.3.1.88
The authenticity of host '10.3.1.88 (10.3.1.88)' can't be established.
ED25519 key fingerprint is SHA256:0BEPF+9T9bXqyrnqtGvBM+bGf6et4FvOr5YhZ3+605k.
This host key is known by the following other names/addresses:
    C:\Users\vende/.ssh/known_hosts:6: 10.3.1.12
    C:\Users\vende/.ssh/known_hosts:9: 10.3.1.11
    C:\Users\vende/.ssh/known_hosts:12: 10.4.1.11
    C:\Users\vende/.ssh/known_hosts:13: 10.3.1.2
    C:\Users\vende/.ssh/known_hosts:14: 10.4.1.2
    C:\Users\vende/.ssh/known_hosts:15: 10.3.1.3
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.3.1.88' (ED25519) to the list of known hosts.
edgy@10.3.1.88's password:
Last login: Tue Jan  3 14:37:28 2023
[edgy@localhost ~]$ cd /etc/hostname
-bash: cd: /etc/hostname: Not a directory
[edgy@localhost ~]$ cd /
[edgy@localhost /]$ ls
afs  bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
[edgy@localhost /]$ cd etc
[edgy@localhost etc]$ ls
adjtime            DIR_COLORS               inputrc         makedumpfile.conf.sample  profile.d               subgid-
aliases            DIR_COLORS.lightbgcolor  iproute2        man_db.conf               protocols               subuid
alternatives       dnf                      issue           microcode_ctl             rc.d                    subuid-
anacrontab         dracut.conf              issue.d         mke2fs.conf               rc.local                sudo.conf
audit              dracut.conf.d            issue.net       modprobe.d                redhat-release          sudoers
authselect         environment              kdump           modules-load.d            resolv.conf             sudoers.d
bash_completion.d  ethertypes               kdump.conf      motd                      rocky-release           sudo-ldap.conf
bashrc             exports                  kernel          motd.d                    rocky-release-upstream  sysconfig
binfmt.d           filesystems              krb5.conf       mtab                      rpc                     sysctl.conf
chrony.conf        firewalld                krb5.conf.d     nanorc                    rpm                     sysctl.d
chrony.keys        fonts                    ld.so.cache     NetworkManager            rsyslog.conf            systemd
cifs-utils         fstab                    ld.so.conf      networks                  rsyslog.d               system-release
cron.d             gcrypt                   ld.so.conf.d    nftables                  rwtab.d                 system-release-cpe
cron.daily         gnupg                    libaudit.conf   nsswitch.conf             sasl2                   terminfo
cron.deny          GREP_COLORS              libibverbs.d    nsswitch.conf.bak         security                tmpfiles.d
cron.hourly        groff                    libnl           openldap                  selinux                 tpm2-tss
cron.monthly       group                    libreport       opt                       services                trusted-key.key
crontab            group-                   libssh          os-release                sestatus.conf           udev
cron.weekly        grub2.cfg                libuser.conf    pam.d                     shadow                  vconsole.conf
crypto-policies    grub.d                   locale.conf     passwd                    shadow-                 vimrc
crypttab           gshadow                  localtime       passwd-                   shells                  virc
csh.cshrc          gshadow-                 login.defs      pkcs11                    skel                    X11
csh.login          gss                      logrotate.conf  pki                       ssh                     xattr.conf
dbus-1             host.conf                logrotate.d     pm                        ssl                     xdg
default            hostname                 lvm             popt.d                    sssd                    yum
depmod.d           hosts                    machine-id      printcap                  statetab.d              yum.conf
dhcp               inittab                  magic           profile                   subgid                  yum.repos.d
[edgy@localhost etc]$ echo 'web.tp5.linux' | sudo tee /etc/hostname
[edgy@localhost etc]$ echo 'web.tp5.linux' | sudo tee /etc/hostname
[edgy@localhost etc]$ echo 'web.tp5.linux' | sudo tee /etc/hostname
[sudo] password for edgy:
web.tp5.linux
[edgy@localhost etc]$ hostname
localhost.localdomain
[edgy@localhost etc]$ reboot
Failed to set wall message, ignoring: Access denied
Failed to reboot system via logind: Access denied
Failed to open initctl fifo: Permission denied
Failed to talk to init daemon.
[edgy@localhost etc]$ sudo reboot
[edgy@localhost etc]$ Connection to 10.3.1.88 closed by remote host.
Connection to 10.3.1.88 closed.
PS C:\Users\vende> ssh edgy@10.3.1.88
edgy@10.3.1.88's password:
Last login: Tue Jan  3 14:44:33 2023 from 10.3.1.1
[edgy@web ~]$ hostname
web.tp5.linux
[edgy@web ~]$ sudo firewall-cmd --list-all
[sudo] password for edgy:
public (active)
  target: default
  icmp-block-inversion: no
  interfaces: enp0s3
  sources:
  services: cockpit dhcpv6-client ssh
  ports:
  protocols:
  forward: yes
  masquerade: no
  forward-ports:
  source-ports:
  icmp-blocks:
  rich rules:
[edgy@web ~]$ sudo shutdown
Shutdown scheduled for Tue 2023-01-03 14:57:59 CET, use 'shutdown -c' to cancel.
[edgy@web ~]$ sudo shutdown -fs
shutdown: invalid option -- 's'
[edgy@web ~]$ sudo shutdown -f
Shutdown scheduled for Tue 2023-01-03 14:58:09 CET, use 'shutdown -c' to cancel.
[edgy@web ~]$ sudo shutdown -c
[edgy@web ~]$
[edgy@web ~]$ client_loop: send disconnect: Connection reset
PS C:\Users\vende> ssh edgy@10.0.2.15
ssh: connect to host 10.0.2.15 port 22: Connection timed out
PS C:\Users\vende> ssh edgy@10.0.2.15
ssh: connect to host 10.0.2.15 port 22: Connection timed out
PS C:\Users\vende> ssh edgy@10.0.2.15/24
ssh: Could not resolve hostname 10.0.2.15/24: H\364te inconnu.
PS C:\Users\vende> ssh edgy@10.0.2.15
ssh: connect to host 10.0.2.15 port 22: Connection timed out
PS C:\Users\vende> ping 10.0.2.15

Envoi d’une requête 'Ping'  10.0.2.15 avec 32 octets de données :
Délai d’attente de la demande dépassé.

Statistiques Ping pour 10.0.2.15:
    Paquets : envoyés = 1, reçus = 0, perdus = 1 (perte 100%),
Ctrl+C
PS C:\Users\vende> ssh edgy@10.0.2.15/24
ssh: Could not resolve hostname 10.0.2.15/24: H\364te inconnu.
PS C:\Users\vende> ssh edgy@10.0.2.15
ssh: connect to host 10.0.2.15 port 22: Connection timed out
PS C:\Users\vende> ^C
PS C:\Users\vende> ssh edgy@10.0.2.15
ssh: connect to host 10.0.2.15 port 22: Connection timed out
PS C:\Users\vende> ssh edgy@10.3.1.88
ssh: connect to host 10.3.1.88 port 22: Connection timed out
PS C:\Users\vende> ssh edgy@10.3.1.88
ssh: connect to host 10.3.1.88 port 22: Connection timed out
PS C:\Users\vende> ssh edgy@10.3.1.88
ssh: connect to host 10.3.1.88 port 22: Connection timed out
PS C:\Users\vende> ssh edgy@10.4.1.88
The authenticity of host '10.4.1.88 (10.4.1.88)' can't be established.
ED25519 key fingerprint is SHA256:0BEPF+9T9bXqyrnqtGvBM+bGf6et4FvOr5YhZ3+605k.
This host key is known by the following other names/addresses:
    C:\Users\vende/.ssh/known_hosts:6: 10.3.1.12
    C:\Users\vende/.ssh/known_hosts:9: 10.3.1.11
    C:\Users\vende/.ssh/known_hosts:12: 10.4.1.11
    C:\Users\vende/.ssh/known_hosts:13: 10.3.1.2
    C:\Users\vende/.ssh/known_hosts:14: 10.4.1.2
    C:\Users\vende/.ssh/known_hosts:15: 10.3.1.3
    C:\Users\vende/.ssh/known_hosts:16: 10.3.1.88
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.4.1.88' (ED25519) to the list of known hosts.
edgy@10.4.1.88's password:
Last login: Tue Jan  3 15:41:06 2023
[edgy@localhost ~]$ echo 'web.tp5.linux' | sudo tee /etc/hostname
[sudo] password for edgy:
web.tp5.linux
[edgy@localhost ~]$ exit
logout
Connection to 10.4.1.88 closed.
PS C:\Users\vende> ssh edgy@10.4.1.88
edgy@10.4.1.88's password:
Last login: Tue Jan  3 15:43:22 2023 from 10.4.1.1
[edgy@localhost ~]$ sudo reboot
[sudo] password for edgy:
[edgy@localhost ~]$ Connection to 10.4.1.88 closed by remote host.
Connection to 10.4.1.88 closed.
PS C:\Users\vende> ssh edgy@10.4.1.88
edgy@10.4.1.88's password:
Last login: Tue Jan  3 15:51:35 2023 from 10.4.1.1
[edgy@web ~]$ sestatus
SELinux status:                 enabled
SELinuxfs mount:                /sys/fs/selinux
SELinux root directory:         /etc/selinux
Loaded policy name:             targeted
Current mode:                   permissive
Mode from config file:          permissive
Policy MLS status:              enabled
Policy deny_unknown status:     allowed
Memory protection checking:     actual (secure)
Max kernel policy version:      33
[edgy@web ~]$ cd /etc/httpd/
-bash: cd: /etc/httpd/: No such file or directory
[edgy@web ~]$ cd /etc/
[edgy@web etc]$ sudo dnf install httpd -y
[sudo] password for edgy:
Rocky Linux 9 - BaseOS                                                                                    633  B/s | 3.6 kB     00:05
Rocky Linux 9 - BaseOS                                                                                    671 kB/s | 1.7 MB     00:02
Rocky Linux 9 - AppStream                                                                                 5.8 kB/s | 4.1 kB     00:00
Rocky Linux 9 - AppStream                                                                                 1.2 MB/s | 6.4 MB     00:05
Rocky Linux 9 - Extras                                                                                    6.0 kB/s | 2.9 kB     00:00
Rocky Linux 9 - Extras                                                                                     13 kB/s | 8.3 kB     00:00
Dependencies resolved.
==========================================================================================================================================
 Package                               Architecture               Version                             Repository                     Size
==========================================================================================================================================
Installing:
 httpd                                 x86_64                     2.4.53-7.el9                        appstream                      48 k
Installing dependencies:
 apr                                   x86_64                     1.7.0-11.el9                        appstream                     123 k
 apr-util                              x86_64                     1.6.1-20.el9                        appstream                      94 k
 apr-util-bdb                          x86_64                     1.6.1-20.el9                        appstream                      13 k
 httpd-core                            x86_64                     2.4.53-7.el9                        appstream                     1.4 M
 httpd-filesystem                      noarch                     2.4.53-7.el9                        appstream                      15 k
 httpd-tools                           x86_64                     2.4.53-7.el9                        appstream                      82 k
 mailcap                               noarch                     2.1.49-5.el9                        baseos                         32 k
 rocky-logos-httpd                     noarch                     90.13-1.el9                         appstream                      24 k
Installing weak dependencies:
 apr-util-openssl                      x86_64                     1.6.1-20.el9                        appstream                      15 k
 mod_http2                             x86_64                     1.15.19-2.el9                       appstream                     149 k
 mod_lua                               x86_64                     2.4.53-7.el9                        appstream                      62 k

Transaction Summary
==========================================================================================================================================
Install  12 Packages

Total download size: 2.0 M
Installed size: 6.0 M
Downloading Packages:
(1/12): mailcap-2.1.49-5.el9.noarch.rpm                                                                   134 kB/s |  32 kB     00:00    A
(2/12): rocky-logos-httpd-90.13-1.el9.noarch.rpm                                                           25 kB/s |  24 kB     00:00
(3/12): mod_lua-2.4.53-7.el9.x86_64.rpm                                                                    64 kB/s |  62 kB     00:00
(4/12): httpd-filesystem-2.4.53-7.el9.noarch.rpm                                                          194 kB/s |  15 kB     00:00
(5/12): httpd-2.4.53-7.el9.x86_64.rpm                                                                     373 kB/s |  48 kB     00:00
(6/12): apr-util-openssl-1.6.1-20.el9.x86_64.rpm                                                          237 kB/s |  15 kB     00:00
(7/12): apr-util-bdb-1.6.1-20.el9.x86_64.rpm                                                              184 kB/s |  13 kB     00:00
(8/12): httpd-tools-2.4.53-7.el9.x86_64.rpm                                                                79 kB/s |  82 kB     00:01
(9/12): apr-util-1.6.1-20.el9.x86_64.rpm                                                                  414 kB/s |  94 kB     00:00
(10/12): mod_http2-1.15.19-2.el9.x86_64.rpm                                                               518 kB/s | 149 kB     00:00
(11/12): apr-1.7.0-11.el9.x86_64.rpm                                                                      445 kB/s | 123 kB     00:00
(12/12): httpd-core-2.4.53-7.el9.x86_64.rpm                                                               1.1 MB/s | 1.4 MB     00:01
------------------------------------------------------------------------------------------------------------------------------------------
Total                                                                                                     605 kB/s | 2.0 MB     00:03
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                                                                  1/1
  Installing       : apr-1.7.0-11.el9.x86_64                                                                                         1/12
  Installing       : apr-util-bdb-1.6.1-20.el9.x86_64                                                                                2/12
  Installing       : apr-util-1.6.1-20.el9.x86_64                                                                                    3/12
  Installing       : apr-util-openssl-1.6.1-20.el9.x86_64                                                                            4/12
  Installing       : httpd-tools-2.4.53-7.el9.x86_64                                                                                 5/12
  Running scriptlet: httpd-filesystem-2.4.53-7.el9.noarch                                                                            6/12
useradd warning: apache's uid 48 outside of the SYS_UID_MIN 201 and SYS_UID_MAX 999 range.

  Installing       : httpd-filesystem-2.4.53-7.el9.noarch                                                                            6/12
  Installing       : rocky-logos-httpd-90.13-1.el9.noarch                                                                            7/12
  Installing       : mailcap-2.1.49-5.el9.noarch                                                                                     8/12
  Installing       : httpd-core-2.4.53-7.el9.x86_64                                                                                  9/12
  Installing       : mod_lua-2.4.53-7.el9.x86_64                                                                                    10/12
  Installing       : mod_http2-1.15.19-2.el9.x86_64                                                                                 11/12
  Installing       : httpd-2.4.53-7.el9.x86_64                                                                                      12/12
  Running scriptlet: httpd-2.4.53-7.el9.x86_64                                                                                      12/12
  Verifying        : mailcap-2.1.49-5.el9.noarch                                                                                     1/12
  Verifying        : rocky-logos-httpd-90.13-1.el9.noarch                                                                            2/12
  Verifying        : mod_lua-2.4.53-7.el9.x86_64                                                                                     3/12
  Verifying        : httpd-tools-2.4.53-7.el9.x86_64                                                                                 4/12
  Verifying        : httpd-2.4.53-7.el9.x86_64                                                                                       5/12
  Verifying        : httpd-filesystem-2.4.53-7.el9.noarch                                                                            6/12
  Verifying        : apr-util-openssl-1.6.1-20.el9.x86_64                                                                            7/12
  Verifying        : apr-util-bdb-1.6.1-20.el9.x86_64                                                                                8/12
  Verifying        : apr-util-1.6.1-20.el9.x86_64                                                                                    9/12
  Verifying        : mod_http2-1.15.19-2.el9.x86_64                                                                                 10/12
  Verifying        : apr-1.7.0-11.el9.x86_64                                                                                        11/12
  Verifying        : httpd-core-2.4.53-7.el9.x86_64                                                                                 12/12

Installed:
  apr-1.7.0-11.el9.x86_64      apr-util-1.6.1-20.el9.x86_64    apr-util-bdb-1.6.1-20.el9.x86_64      apr-util-openssl-1.6.1-20.el9.x86_64
  httpd-2.4.53-7.el9.x86_64    httpd-core-2.4.53-7.el9.x86_64  httpd-filesystem-2.4.53-7.el9.noarch  httpd-tools-2.4.53-7.el9.x86_64
  mailcap-2.1.49-5.el9.noarch  mod_http2-1.15.19-2.el9.x86_64  mod_lua-2.4.53-7.el9.x86_64           rocky-logos-httpd-90.13-1.el9.noarch

Complete!
[edgy@web etc]$ sudo vim /etc/httpd/conf/httpd.conf
[edgy@web etc]$ cat /etc/httpd/conf/httpd.conf

ServerRoot "/etc/httpd"

Listen 80

Include conf.modules.d/*.conf

User apache
Group apache


ServerAdmin root@localhost


<Directory />
    AllowOverride none
    Require all denied
</Directory>


DocumentRoot "/var/www/html"

<Directory "/var/www">
    AllowOverride None
    Require all granted
</Directory>

<Directory "/var/www/html">
    Options Indexes FollowSymLinks

    AllowOverride None

    Require all granted
</Directory>

<IfModule dir_module>
    DirectoryIndex index.html
</IfModule>

<Files ".ht*">
    Require all denied
</Files>

ErrorLog "logs/error_log"

LogLevel warn

<IfModule log_config_module>
    LogFormat "%h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\"" combined
    LogFormat "%h %l %u %t \"%r\" %>s %b" common

    <IfModule logio_module>
      LogFormat "%h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\" %I %O" combinedio
    </IfModule>


    CustomLog "logs/access_log" combined
</IfModule>

<IfModule alias_module>


    ScriptAlias /cgi-bin/ "/var/www/cgi-bin/"

</IfModule>

<Directory "/var/www/cgi-bin">
    AllowOverride None
    Options None
    Require all granted
</Directory>

<IfModule mime_module>
    TypesConfig /etc/mime.types

    AddType application/x-compress .Z
    AddType application/x-gzip .gz .tgz



    AddType text/html .shtml
    AddOutputFilter INCLUDES .shtml
</IfModule>

AddDefaultCharset UTF-8

<IfModule mime_magic_module>
    MIMEMagicFile conf/magic
</IfModule>


EnableSendfile on

IncludeOptional conf.d/*.conf
[edgy@web etc]$ sudo systemctl start httpd; sudo systemctl enable httpd
[sudo] password for edgy:
Created symlink /etc/systemd/system/multi-user.target.wants/httpd.service → /usr/lib/systemd/system/httpd.service.
[edgy@web etc]$ sudi ss-altnp | grep httpd
-bash: sudi: command not found
[edgy@web etc]$ sudo ss-altnp | grep httpd
sudo: ss-altnp: command not found
[edgy@web etc]$ sudo ss -altnp | grep httpd
LISTEN 0      511                *:80              *:*    users:(("httpd",pid=10774,fd=4),("httpd",pid=10773,fd=4),("httpd",pid=10772,fd=4),("httpd",pid=10769,fd=4))
[edgy@web etc]$ sudo firewall-cmd --list-all | grep 80
[edgy@web etc]$ sudo firewall-cmd --list-all
public (active)
  target: default
  icmp-block-inversion: no
  interfaces: enp0s3 enp0s8
  sources:
  services: cockpit dhcpv6-client ssh
  ports:
  protocols:
  forward: yes
  masquerade: no
  forward-ports:
  source-ports:
  icmp-blocks:
  rich rules:
[edgy@web etc]$ sudo firewall-cmd --add-port=80/tcp --permanent
[sudo] password for edgy:
success
[edgy@web etc]$ sudo firewall-cmd reload
usage: see firewall-cmd man page
firewall-cmd: error: unrecognized arguments: reload
[edgy@web etc]$ sudo firewall-cmd --reload
success
[edgy@web etc]$ sudo firewall-cmd --list-all | grep 80
  ports: 80/tcp
[edgy@web etc]$ sudo systemctl status httpd | grep active
     Active: active (running) since Tue 2023-01-03 16:14:39 CET; 19min ago
[edgy@web etc]$ sudo systemctl status httpd | grep enable
     Loaded: loaded (/usr/lib/systemd/system/httpd.service; enabled; vendor preset: disabled)
[edgy@web etc]$ curl localhost
<!doctype html>
<html>
  <head>
    <meta charset='utf-8'>
    <meta name='viewport' content='width=device-width, initial-scale=1'>
    <title>HTTP Server Test Page powered by: Rocky Linux</title>
    <style type="text/css">
      /*<![CDATA[*/

      html {
        height: 100%;
        width: 100%;
      }
        body {
  background: rgb(20,72,50);
  background: -moz-linear-gradient(180deg, rgba(23,43,70,1) 30%, rgba(0,0,0,1) 90%)  ;
  background: -webkit-linear-gradient(180deg, rgba(23,43,70,1) 30%, rgba(0,0,0,1) 90%) ;
  background: linear-gradient(180deg, rgba(23,43,70,1) 30%, rgba(0,0,0,1) 90%);
  background-repeat: no-repeat;
  background-attachment: fixed;
  filter: progid:DXImageTransform.Microsoft.gradient(startColorstr="#3c6eb4",endColorstr="#3c95b4",GradientType=1);
        color: white;
        font-size: 0.9em;
        font-weight: 400;
        font-family: 'Montserrat', sans-serif;
        margin: 0;
        padding: 10em 6em 10em 6em;
        box-sizing: border-box;

      }


  h1 {
    text-align: center;
    margin: 0;
    padding: 0.6em 2em 0.4em;
    color: #fff;
    font-weight: bold;
    font-family: 'Montserrat', sans-serif;
    font-size: 2em;
  }
  h1 strong {
    font-weight: bolder;
    font-family: 'Montserrat', sans-serif;
  }
  h2 {
    font-size: 1.5em;
    font-weight:bold;
  }

  .title {
    border: 1px solid black;
    font-weight: bold;
    position: relative;
    float: right;
    width: 150px;
    text-align: center;
    padding: 10px 0 10px 0;
    margin-top: 0;
  }

  .description {
    padding: 45px 10px 5px 10px;
    clear: right;
    padding: 15px;
  }

  .section {
    padding-left: 3%;
   margin-bottom: 10px;
  }

  img {

    padding: 2px;
    margin: 2px;
  }
  a:hover img {
    padding: 2px;
    margin: 2px;
  }

  :link {
    color: rgb(199, 252, 77);
    text-shadow:
  }
  :visited {
    color: rgb(122, 206, 255);
  }
  a:hover {
    color: rgb(16, 44, 122);
  }
  .row {
    width: 100%;
    padding: 0 10px 0 10px;
  }

  footer {
    padding-top: 6em;
    margin-bottom: 6em;
    text-align: center;
    font-size: xx-small;
    overflow:hidden;
    clear: both;
  }

  .summary {
    font-size: 140%;
    text-align: center;
  }

  #rocky-poweredby img {
    margin-left: -10px;
  }

  #logos img {
    vertical-align: top;
  }

  /* Desktop  View Options */

  @media (min-width: 768px)  {

    body {
      padding: 10em 20% !important;
    }

    .col-md-1, .col-md-2, .col-md-3, .col-md-4, .col-md-5, .col-md-6,
    .col-md-7, .col-md-8, .col-md-9, .col-md-10, .col-md-11, .col-md-12 {
      float: left;
    }

    .col-md-1 {
      width: 8.33%;
    }
    .col-md-2 {
      width: 16.66%;
    }
    .col-md-3 {
      width: 25%;
    }
    .col-md-4 {
      width: 33%;
    }
    .col-md-5 {
      width: 41.66%;
    }
    .col-md-6 {
      border-left:3px ;
      width: 50%;


    }
    .col-md-7 {
      width: 58.33%;
    }
    .col-md-8 {
      width: 66.66%;
    }
    .col-md-9 {
      width: 74.99%;
    }
    .col-md-10 {
      width: 83.33%;
    }
    .col-md-11 {
      width: 91.66%;
    }
    .col-md-12 {
      width: 100%;
    }
  }

  /* Mobile View Options */
  @media (max-width: 767px) {
    .col-sm-1, .col-sm-2, .col-sm-3, .col-sm-4, .col-sm-5, .col-sm-6,
    .col-sm-7, .col-sm-8, .col-sm-9, .col-sm-10, .col-sm-11, .col-sm-12 {
      float: left;
    }

    .col-sm-1 {
      width: 8.33%;
    }
    .col-sm-2 {
      width: 16.66%;
    }
    .col-sm-3 {
      width: 25%;
    }
    .col-sm-4 {
      width: 33%;
    }
    .col-sm-5 {
      width: 41.66%;
    }
    .col-sm-6 {
      width: 50%;
    }
    .col-sm-7 {
      width: 58.33%;
    }
    .col-sm-8 {
      width: 66.66%;
    }
    .col-sm-9 {
      width: 74.99%;
    }
    .col-sm-10 {
      width: 83.33%;
    }
    .col-sm-11 {
      width: 91.66%;
    }
    .col-sm-12 {
      width: 100%;
    }
    h1 {
      padding: 0 !important;
    }
  }


  </style>
  </head>
  <body>
    <h1>HTTP Server <strong>Test Page</strong></h1>

    <div class='row'>

      <div class='col-sm-12 col-md-6 col-md-6 '></div>
          <p class="summary">This page is used to test the proper operation of
            an HTTP server after it has been installed on a Rocky Linux system.
            If you can read this page, it means that the software is working
            correctly.</p>
      </div>

      <div class='col-sm-12 col-md-6 col-md-6 col-md-offset-12'>


        <div class='section'>
          <h2>Just visiting?</h2>

          <p>This website you are visiting is either experiencing problems or
          could be going through maintenance.</p>

          <p>If you would like the let the administrators of this website know
          that you've seen this page instead of the page you've expected, you
          should send them an email. In general, mail sent to the name
          "webmaster" and directed to the website's domain should reach the
          appropriate person.</p>

          <p>The most common email address to send to is:
          <strong>"webmaster@example.com"</strong></p>

          <h2>Note:</h2>
          <p>The Rocky Linux distribution is a stable and reproduceable platform
          based on the sources of Red Hat Enterprise Linux (RHEL). With this in
          mind, please understand that:

        <ul>
          <li>Neither the <strong>Rocky Linux Project</strong> nor the
          <strong>Rocky Enterprise Software Foundation</strong> have anything to
          do with this website or its content.</li>
          <li>The Rocky Linux Project nor the <strong>RESF</strong> have
          "hacked" this webserver: This test page is included with the
          distribution.</li>
        </ul>
        <p>For more information about Rocky Linux, please visit the
          <a href="https://rockylinux.org/"><strong>Rocky Linux
          website</strong></a>.
        </p>
        </div>
      </div>
      <div class='col-sm-12 col-md-6 col-md-6 col-md-offset-12'>
        <div class='section'>

          <h2>I am the admin, what do I do?</h2>

        <p>You may now add content to the webroot directory for your
        software.</p>

        <p><strong>For systems using the
        <a href="https://httpd.apache.org/">Apache Webserver</strong></a>:
        You can add content to the directory <code>/var/www/html/</code>.
        Until you do so, people visiting your website will see this page. If
        you would like this page to not be shown, follow the instructions in:
        <code>/etc/httpd/conf.d/welcome.conf</code>.</p>

        <p><strong>For systems using
        <a href="https://nginx.org">Nginx</strong></a>:
        You can add your content in a location of your
        choice and edit the <code>root</code> configuration directive
        in <code>/etc/nginx/nginx.conf</code>.</p>

        <div id="logos">
          <a href="https://rockylinux.org/" id="rocky-poweredby"><img src="icons/poweredby.png" alt="[ Powered by Rocky Linux ]" /></a> <!-- Rocky -->
          <img src="poweredby.png" /> <!-- webserver -->
        </div>
      </div>
      </div>

      <footer class="col-sm-12">
      <a href="https://apache.org">Apache&trade;</a> is a registered trademark of <a href="https://apache.org">the Apache Software Foundation</a> in the United States and/or other countries.<br />
      <a href="https://nginx.org">NGINX&trade;</a> is a registered trademark of <a href="https://">F5 Networks, Inc.</a>.
      </footer>

  </body>
</html>
[edgy@web etc]$ ls
adjtime            DIR_COLORS.lightbgcolor  iproute2                  man_db.conf        protocols               subuid-
aliases            dnf                      issue                     microcode_ctl      rc.d                    sudo.conf
alternatives       dracut.conf              issue.d                   mime.types         rc.local                sudoers
anacrontab         dracut.conf.d            issue.net                 mke2fs.conf        redhat-release          sudoers.d
audit              environment              kdump                     modprobe.d         resolv.conf             sudo-ldap.conf
authselect         ethertypes               kdump.conf                modules-load.d     rocky-release           sysconfig
bash_completion.d  exports                  kernel                    motd               rocky-release-upstream  sysctl.conf
bashrc             filesystems              krb5.conf                 motd.d             rpc                     sysctl.d
binfmt.d           firewalld                krb5.conf.d               mtab               rpm                     systemd
chrony.conf        fonts                    ld.so.cache               nanorc             rsyslog.conf            system-release
chrony.keys        fstab                    ld.so.conf                NetworkManager     rsyslog.d               system-release-cpe
cifs-utils         gcrypt                   ld.so.conf.d              networks           rwtab.d                 terminfo
cron.d             gnupg                    libaudit.conf             nftables           sasl2                   tmpfiles.d
cron.daily         GREP_COLORS              libibverbs.d              nsswitch.conf      security                tpm2-tss
cron.deny          groff                    libnl                     nsswitch.conf.bak  selinux                 trusted-key.key
cron.hourly        group                    libreport                 openldap           services                udev
cron.monthly       group-                   libssh                    opt                sestatus.conf           vconsole.conf
crontab            grub2.cfg                libuser.conf              os-release         shadow                  vimrc
cron.weekly        grub.d                   locale.conf               pam.d              shadow-                 virc
crypto-policies    gshadow                  localtime                 passwd             shells                  X11
crypttab           gshadow-                 login.defs                passwd-            skel                    xattr.conf
csh.cshrc          gss                      logrotate.conf            pkcs11             ssh                     xdg
csh.login          host.conf                logrotate.d               pki                ssl                     yum
dbus-1             hostname                 lvm                       pm                 sssd                    yum.conf
default            hosts                    machine-id                popt.d             statetab.d              yum.repos.d
depmod.d           httpd                    magic                     printcap           subgid
dhcp               inittab                  mailcap                   profile            subgid-
DIR_COLORS         inputrc                  makedumpfile.conf.sample  profile.d          subuid
[edgy@web etc]$ systemctl cat httpd
# /usr/lib/systemd/system/httpd.service
# See httpd.service(8) for more information on using the httpd service.

# Modifying this file in-place is not recommended, because changes
# will be overwritten during package upgrades.  To customize the
# behaviour, run "systemctl edit httpd" to create an override unit.

# For example, to pass additional options (such as -D definitions) to
# the httpd binary at startup, create an override unit (as is done by
# systemctl edit) and enter the following:

#       [Service]
#       Environment=OPTIONS=-DMY_DEFINE

[Unit]
Description=The Apache HTTP Server
Wants=httpd-init.service
After=network.target remote-fs.target nss-lookup.target httpd-init.service
Documentation=man:httpd.service(8)

[Service]
Type=notify
Environment=LANG=C

ExecStart=/usr/sbin/httpd $OPTIONS -DFOREGROUND
ExecReload=/usr/sbin/httpd $OPTIONS -k graceful
# Send SIGWINCH for graceful stop
KillSignal=SIGWINCH
KillMode=mixed
PrivateTmp=true
OOMPolicy=continue

[Install]
WantedBy=multi-user.target
[edgy@web etc]$ sudo cat /etc/httpd/conf/httpd.conf | grep usr
[sudo] password for edgy:
[edgy@web etc]$ sudo cat /etc/httpd/conf/httpd.conf | grep user
[edgy@web etc]$ sudo cat /etc/httpd/conf/httpd.conf | grep User
User apache
    LogFormat "%h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\"" combined
      LogFormat "%h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\" %I %O" combinedio
[edgy@web etc]$ sudo cat /etc/httpd/conf/httpd.conf | grep 'User'
User apache
    LogFormat "%h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\"" combined
      LogFormat "%h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\" %I %O" combinedio
[edgy@web etc]$ ##################################################la première ligne de la comande au dessus ##############################
###################"
[edgy@web etc]$ cat /etc/passwd
root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin
adm:x:3:4:adm:/var/adm:/sbin/nologin
lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
sync:x:5:0:sync:/sbin:/bin/sync
shutdown:x:6:0:shutdown:/sbin:/sbin/shutdown
halt:x:7:0:halt:/sbin:/sbin/halt
mail:x:8:12:mail:/var/spool/mail:/sbin/nologin
operator:x:11:0:operator:/root:/sbin/nologin
games:x:12:100:games:/usr/games:/sbin/nologin
ftp:x:14:50:FTP User:/var/ftp:/sbin/nologin
nobody:x:65534:65534:Kernel Overflow User:/:/sbin/nologin
systemd-coredump:x:999:997:systemd Core Dumper:/:/sbin/nologin
dbus:x:81:81:System message bus:/:/sbin/nologin
tss:x:59:59:Account used for TPM access:/dev/null:/sbin/nologin
sssd:x:998:995:User for sssd:/:/sbin/nologin
sshd:x:74:74:Privilege-separated SSH:/usr/share/empty.sshd:/sbin/nologin
chrony:x:997:994::/var/lib/chrony:/sbin/nologin
systemd-oom:x:992:992:systemd Userspace OOM Killer:/:/usr/sbin/nologin
edgy:x:1000:1000:edgy:/home/edgy:/bin/bash
tcpdump:x:72:72::/:/sbin/nologin
apache:x:48:48:Apache:/usr/share/httpd:/sbin/nologin
[edgy@web etc]$ ps -ef | grep apache
apache     10770   10769  0 16:14 ?        00:00:00 /usr/sbin/httpd -DFOREGROUND
apache     10772   10769  0 16:14 ?        00:00:00 /usr/sbin/httpd -DFOREGROUND
apache     10773   10769  0 16:14 ?        00:00:00 /usr/sbin/httpd -DFOREGROUND
apache     10774   10769  0 16:14 ?        00:00:00 /usr/sbin/httpd -DFOREGROUND
edgy       11134     825  0 16:56 pts/0    00:00:00 grep --color=auto apache
[edgy@web etc]$
