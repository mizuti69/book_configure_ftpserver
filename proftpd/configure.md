# 基本設定

## サーバ設定
**サーバ名**  

```
# vim /etc/proftpd.conf
ServerName "localhost.localdomain"
```

**HELOメッセージ**  

```
# vim /etc/proftpd.conf
ServerName "ftp server"
```

**サーバ起動タイプ**  
スタンドアローンモードの場合は`standalone`  
スーパーサーバモードの場合は`Inetd`  

```
# vim /etc/proftpd.conf
ServerType Inetd
```

**起動ユーザー**  
デーモン起動時に利用  

```
# vim /etc/proftpd.conf
User nobody
Group nobody
```

**デフォルトサーバ設定**  
複数のFTPサーバを仮想サーバで立てている場合のデフォルトサーバを設定する  

```
# vim /etc/proftpd.conf
DefaultServer On
```

**リスニングポート**  

```
# vim /etc/proftpd.conf
Port 21
```

**パッシブポート範囲**  
パッシブモードで利用するポート範囲を指定できる  

```
# vim /etc/proftpd.conf
PassivePorts 60000 60040
```

**デフォルトUmask**  

```
# vim /etc/proftpd.conf
Umask 002
```

**時刻設定**  
表示やログで利用するサーバ時刻  

```
# vim /etc/proftpd.conf
TimesGMT off
SetEnv TZ JST-9
```

**IDENTプロトコルの利用**  

```
# vim /etc/proftpd.conf
IdentLookups off
```

**接続元の名前解決**  

```
# vim /etc/proftpd.conf
UseReverseDNS off
```

**IPv6の無効化**  

```
# vim /etc/proftpd.conf
UseIPv6 off
```

**NAT対応**  
クライアント <-> サーバ間でNW機器によりNAT変換される場合  
FTPサーバは自身のIPで通信回答し接続が上手くいかないため、外部アドレスを明記する  

```
# vim /etc/proftpd.conf
MasqueradeAddress 127.0.0.1
```

## セキュリティ設定

**利用認証モジュール**  

```
# vim /etc/proftpd.conf
AuthPAMConfig proftpd
AuthOrder mod_auth_pam.c* mod_auth_unix.c
```

**認証試行回数制限**  

```
# vim /etc/proftpd.conf
MaxLoginAttempts 10
```

**ROOTユーザーのログイン無効化**  

```
# vim /etc/proftpd.conf
RootLogin off
```

**OS認証ファイルの利用許可**  
認証、ユーザー/グループ検索、ユーザー/グループと名前のマッピングをproftpdがどのように扱うかを制御  
"on"に設定すると、proftpdはシステム全体の/ etc / passwd、/ etc / group（と/ etc / shadow、潜在的に）ファイル自体を開き、chroot（）のログイン時にも開いたままにします  

```
# vim /etc/proftpd.conf
PersistentPasswd off
```

## ロギング

**ログフォーマット**  

```
# vim /etc/proftpd.conf
LogFormat default "%h %l %u %t \"%r\" %s \"%f\" %b"
```

**アクセスログ出力先**  

```
# vim /etc/proftpd.conf
ExtendedLog /var/log/proftpd/proftpd_access.log all
```

**システムログ出力先**  

```
# vim /etc/proftpd.conf
SystemLog /var/log/proftpd/proftpd.log
```

**ログのローテート設定**  

```
# vim /etc/logrotate.d/proftpd
/var/log/proftpd/*.log /var/log/xferlog {
    compress
    missingok
    notifempty
    sharedscripts
    postrotate
        systemctl reload proftpd.service
    endscript
}
```

## セッション設定

**アイドル状態での接続を維持している時間**  

```
# vim /etc/proftpd.conf
TimeoutIdle 600
```

**クライアント認証タイムアウト時間**  

```
# vim /etc/proftpd.conf
TimeoutLogin 1800
```

**認証後、無転送タイムアウト時間**  

```
# vim /etc/proftpd.conf
TimeoutNoTransfer 1800
```

**認証後、制御コネクションタイムアウト時間**  

```
# vim /etc/proftpd.conf
TimeoutSession 3600
```

**認証後、データコネクションタイムアウト時間**  

```
# vim /etc/proftpd.conf
TimeoutStalled 1800
```
