# proftpdのインストール

## サードパーティリポジトリの追加
標準リポジトリではパッケージが提供されていない場合が多いため、ない場合はEPELリポジトリを追加  

```
# wget http://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
# rpm -ivh epel-release-latest-7.noarch.rpm
```

インストール後はデフォルトでパッケージ検索しないよう無効化しておく  

```
# yum-config-manager --disable epel
```

## proftpdのインストール

```
# yum install --enablerepo=epel proftpd
```

## 起動設定  
スタンドアローンモード（デーモン起動)とスーパーサーバモード（ystemctl起動）がある  
利用頻度に応じて負荷の少ない方法で起動する  

### スタンドアローンモード
デフォルトでスタンドアローンモードとなっている  

```
# systemctl status proftpd
● proftpd.service - ProFTPD FTP Server
   Loaded: loaded (/usr/lib/systemd/system/proftpd.service; disabled; vendor preset: disabled)
   Active: inactive (dead)
```

### スーパーサーバモード
スタンドアローンモードの起動設定を無効化し  

```
# systemctl disable proftpd
```

以下の起動ファイルを作成する  
https://github.com/proftpd/proftpd/tree/master/contrib/dist/systemd  

```
# vim /usr/lib/systemd/system/proftpd@.service
[Unit]
Description=ProFTPD FTP Server
After=local-fs.target

[Service]
ExecStart=-/usr/sbin/in.proftpd
StandardInput=socket
```

```
# vim /usr/lib/systemd/system/proftpd.socket
# Note that to use proftpd via this socket file, you will need:
#
#   ServerType inetd
#
# in /etc/proftpd.conf

[Unit]
Description=ProFTPD FTP Server Activation Socket
Conflicts=proftpd.service

[Socket]
ListenStream=21
Accept=true

[Install]
WantedBy=sockets.target
```

有効化  

```
# systemctl enable proftpd.socket
```
