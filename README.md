#サーバー構築
##zshの導入
#####インストールされているシェル一覧
```
$ cat /etc/shells
```
#####インストール
```
$ sudo yum install zsh
```
#####ログインシェルの変更
```
$ chsh
パスワード：
新しいシェル[/bin/bash]: /bin/zsh
シェルを変更しました。
```

#公開鍵・秘密鍵
#####鍵の生成
```
ssh-keygen -t rsa
```
#####サーバに公開鍵の登録
```
ssh-copy-id -i ~/.ssh/id_rsa.pub user@remote_host
```
Macではデフォルトでは`ssh-copy-id`がないので`brew install ssh-copy-id`という感じに導入しておく必要がある。


##mySQLの設定
##rootのパスワード初期化
#####起動中のmySQLを停止
```
sudo servicd mysqld stop
```
#####mySQLをセーフモードで起動
```
mysqld_safe --skip-grant-tables &
```
#####rootでログイン
```
sudo mysql -u root
```
#####パスワードの変更
```
use mysql;
update user set password=password("hogehoge") where user='root';
flush privileges;
quit
```
#####パスワードの保管
`/var/conf`に400で保存しておいた。
##ユーザの追加
#####登録されているユーザの確認
```
select user, host from mysql.user;
```
#####ユーザ権限の確認
```
show grants for 'hoge'@'host';
```
#####ユーザの作成
```
create user user_name;
create user user_name identified by passwd;
```
#####権限の付与
```
grant select, insert, update on *.* to hoge;
grant select, insert, update on *.* to hoge indentified by passwd;
```
#####パスワードの設定
```
set password = password('hogehoge');
set password for user = password('hogehoge');
```

