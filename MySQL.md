# MySQL
####rootのパスワード初期化
※ rootのパスワードを忘れた場合
#####起動中のmySQLを停止
```
service mysqld stop
```
#####mySQLをセーフモードで起動
```
mysqld_safe --skip-grant-tables &
```
#####rootでログイン
```
mysql -u root
```
#####パスワードの変更
```
use mysql;
update user set password=password("hogehoge") where user='root';
flush privileges;
quit
```
