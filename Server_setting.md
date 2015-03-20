#サーバー構築
##zshの導入

#####インストールされているシェル一覧
```sh
$ cat /etc/shells
```

#####インストール
```sh
$ yum install zsh
```

#####ログインシェルの変更
```sh
$ chsh
パスワード：
新しいシェル[/bin/bash]: /bin/zsh
シェルを変更しました。
```

##Gitのupdate
```sh
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
```sh
$ ssh-keygen -t rsa
```

#####サーバに公開鍵の登録
```sh
$ ssh-copy-id -i ~/.ssh/id_rsa.pub user@remote_host
```
Macではデフォルトでは`ssh-copy-id`がないので`brew install ssh-copy-id`という感じに導入しておく必要がある。

##パスワード生成 (mkpasswd)
```sh
$ ym install expect
```


##mySQLの導入
参考：http://www.kakiro-web.com/linux/centos6-mysql-yum-repository-install.html

#####インストール
```sh
$ wget http://dev.mysql.com/get/mysql-community-release-el6-5.noarch.rpm
$ yum --enablerepo=mysql56-community install mysql-community-server
```

#####サービスの起動
```sh
$ service mysqld start
```

#####登録されているユーザの確認
```sql
select user, host from mysql.user;
```

#####ユーザ権限の確認
```sql
show grants for 'hoge'@'host';
```

#####ユーザの作成
```sql
create user user_name;
create user user_name identified by passwd;
```

#####権限の付与
```sql
grant select, insert, update on *.* to hoge;
grant select, insert, update on *.* to hoge indentified by passwd;
```

#####パスワードの設定
```sql
set password = password('hogehoge');
set password for user = password('hogehoge');
```

##Rubyのインストール
###rbenv
参考：
http://qiita.com/chezou/items/86ee6ded253c094a23b6
http://office.tsukuba-bunko.org/ppoi/entry/systemwide-rbenv

まず`rvm`をインストールしてしまった場合はアンインストールする。
```sh
$ rvm seppuku
```
#####rbenvのインストール
今回はシステムワイドな利用を想定して`su -`でsuperuserになって作業する。


```sh
# 必要なパッケージの導入
$ yum install -y git gcc gcc-c++ openssl-devel readline-devel

# gitからソース取得して配置
$ cd /usr/local
$ git clone git://github.com/sstephenson/rbenv.git rbenv
$ git clone git://github.com/sstephenson/ruby-build.git rbenv/plugins/ruby-build
```

#####rbenvの設定

```sh
export RBENV_ROOT="/usr/local/rbenv"
export PATH="${RBENV_ROOT}/bin:${PATH}"
eval "$(rbenv init --no-rehash -)"
```
を`/etc/profile.d/rbenv.sh`に保存

#####gemのインストール
```sh
$ rbenv exec gem install bundler
```

#####rbenvの使い方
```sh
# 一覧の取得
$ rbenv install --list

# インストール
$ rbenv install 2.2.0

# 再度読み込み
$ rbenv rehash

# システムワイドに利用するrubyのバージョンを指定
$ rbenv gobal 2.2.0
```

##rvm
#####インストール
```sh
$ curl -sSL https://get.rvm.io | bash -s stable

#失敗するようならsudoで実行してみる
$ curl -sSL https://get.rvm.io | sudo bash -s stable
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
```sh
$ rvm get latest
$ rvm reload
```

#####インストールできるバージョンの一覧を確認
```sh
$ rvm list known
```
#####インストール
```sh
$ rvm install 2.2
```
#####利用するバージョンを指定する
```sh
$ rvm use 2.2
```
#####確認
```sh
$ ruby -v
$ which ruby
```


