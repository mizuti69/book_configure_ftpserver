# FTPS設定
FTPS通信を有効化する  


```
# vim /etc/proftpd.conf

<IfModule mod_tls.c>
  TLSEngine on
  TLSLog /var/log/proftpd/tls.log
  TLSProtocol TLSv1
  TLSRequired off
  TLSRSACertificateFile    /etc/proftpd.d/tls/local.crt
  TLSRSACertificateKeyFile /etc/proftpd.d/tls/local.key
  TLSVerifyClient off
  TLSOptions NoCertRequest
  TLSRenegotiate none
</IfModule>
```

FTPS接続を強制したい場合は`TLSRequired on`とする  