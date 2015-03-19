#サーバー構築
##zshの導入

#####インストールされているシェル一覧
```
$ cat /etc/shells
```

#####インストール
```
$ yum install zsh
```

#####ログインシェルの変更
```
$ chsh
パスワード：
新しいシェル[/bin/bash]: /bin/zsh
シェルを変更しました。
```

##Gitのupdate
```
$ yum remove git
$ yum install curl-devel expat-devel gettext-devel openssl-devel zlib-devel perl-ExtUtils-MakeMaker
$ wget https://www.kernel.org/pub/software/scm/git/git-2.3.3.tar.gz
$ tar -zxf git-2.3.3.tar.gz
$ cd git-2.3.0
$ make prefix=/usr/local all
$ make prefix=/usr/local install
$ git --ver
```

##公開鍵・秘密鍵

#####鍵の生成
```
ssh-keygen -t rsa
```

#####サーバに公開鍵の登録
```
ssh-copy-id -i ~/.ssh/id_rsa.pub user@remote_host
```
Macではデフォルトでは`ssh-copy-id`がないので`brew install ssh-copy-id`という感じに導入しておく必要がある。

##パスワード生成 (mkpasswd)
```
$ ym install expect
```


##mySQLの導入
参考：http://www.kakiro-web.com/linux/centos6-mysql-yum-repository-install.html

#####インストール
```
$ wget http://dev.mysql.com/get/mysql-community-release-el6-5.noarch.rpm
$ yum --enablerepo=mysql56-community install mysql-community-server
```

#####サービスの起動
```
$ service mysqld start
```

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

##Rubyのインストール
#####rvmのインストール
```
$ curl -sSL https://get.rvm.io | bash -s stable
```
#####zshrcの編集
```
# Linux
source usr/local/rvm/scripts/rvm
export rvmsudo_secure_path=1

# Mac
source ~/.rvm/scripts/rvm
```
##### update
root権限が必要な場合は、`sudo`の代わりに`rmvsudo`を使う。
```
$ rvm get latest

$ rvm reload
```

#####インストールできるバージョンの一覧を確認
```
$ rvm list known
```
#####インストール
```
$ rvm install 2.2
```
#####利用するバージョンを指定する
```
$ rvm use 2.2
```
#####確認
```
$ ruby -v
$ which ruby
```
