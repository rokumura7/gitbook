# MySQLの認証

[公式](https://dev.mysql.com/doc/refman/8.0/en/upgrading-from-previous-series.html#upgrade-caching-sha2-password)

MySQL8系から認証方式が代わり、クライアントソフトによっては接続ができない

## 回避方法

接続方法を、`caching_sha2_password`から従来の`mysql_native_password`に変更する

※根本的な解決方法ではない

### 設定で回避

```text
[mysqld]
default_authentication_plugin=mysql_native_password
```

### ユーザー単位で回避

```sql
mysql> SELECT host, user, plugin FROM mysql.user;
+-----------+------------------+-----------------------+
| host      | user             | plugin                |
+-----------+------------------+-----------------------+
| localhost | hogehoge         | caching_sha2_password |
+-----------+------------------+-----------------------+
```

```sql
mysql> ALTER USER 'ユーザー名'@"localhost" IDENTIFIED WITH mysql_native_password BY 'パスワード';
```

```sql
mysql> SELECT host, user, plugin FROM mysql.user;
+-----------+------------------+-----------------------+
| host      | user             | plugin                |
+-----------+------------------+-----------------------+
| localhost | hogehoge         | mysql_native_password |
+-----------+------------------+-----------------------+
```

値が変わっていることを確認したら設定を反映する

```sql
mysql> FLUSH PRIVILEGES;
```

