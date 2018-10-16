# アクセス制御
ディレクトリ単位、ユーザー、グループ単位でアクセス許可の権限を細分化できます  

## Chroot設定
特定のグループを除きFTP接続時にはChrootさせる  

```
# vim /etc/proftpd.conf
DefaultRoot ~ !hogegroup
```

## ディレクティブ制限
基本ディレクトリ制御のみ`proftpd.conf`に記述し、用途別の制御は`proftpd.d`配下で個別ファイル管理する  

```
Include /etc/proftpd.d/custom.conf
```

例）ベース許可設定  

```
# vim /etc/proftpd.conf

# 全てを拒否しホワイトリスト方式で管理
<Directory / >
  <Limit ALL>
    IgnoreHidden on
  </Limit>
  <Limit READ WRITE>
    DenyAll
  </Limit>
</Directory>

# ユーザーホームディレクトリは全て許可  
<Directory ~/>
  AllowOverwrite on
  <Limit All>
    AllowGroup hogegroup
  </Limit>
</Directory>
```

ディレクティブで定義するパスはサーバから見た絶対パスではなく、FTPログインユーザーからみた相対パスであることを注意  
ホワイトリスト方式でかつChrootユーザーの場合上記２つのディレクティブ定義は同じディレクトリに対する権限設定となる  

例）ユーザー毎のアクセス制御例  

```
<Directory /data/dir>
  AllowOverwrite on
  <Limit DIRS READ WRITE >
    AllowUser hogehoge
  </Limit>
</Directory>
```

proftpd側で読み書き権限を許可していても、OS上で権限がなければ操作できないので注意  
