# サーバー構築
## zshの導入

```sh
# インストールされているシェル一覧
$ cat /etc/shells

# インストール
$ yum install zsh

# ログインシェルの変更
```sh
$ chsh
パスワード：
新しいシェル[/bin/bash]: /bin/zsh
シェルを変更しました。
```

## Gitのupdate
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

## 公開鍵・秘密鍵

```sh
# 鍵の生成
$ ssh-keygen -t rsa

# サーバに公開鍵の登録
$ ssh-copy-id -i ~/.ssh/id_rsa.pub user@remote_host
```
Macではデフォルトでは`ssh-copy-id`がないので`brew install ssh-copy-id`という感じに導入しておく必要がある。

## パスワード生成 (mkpasswd)
```sh
$ ym install expect
```

## Run Level
| Level |                      mean                      |
| :---: | :--------------------------------------------- |
|   0   | シャットダウン（システムの停止）               |
|   1   | シングルユーザーモード（rootのみ）             |
|   2   | ネットワークなしのマルチユーザーモード         |
|   3   | 通常のマルチユーザーモード（テキストログイン） |
|   4   | 未使用                                         |
|   5   | グラフィカルログインによるマルチユーザーモード |
|   6   | システムの再起動                               |

## MySQLの導入
参考：http://www.kakiro-web.com/linux/centos6-mysql-yum-repository-install.html

```sh
# インストール
$ wget http://dev.mysql.com/get/mysql-community-release-el6-5.noarch.rpm
$ yum --enablerepo=mysql56-community install mysql-community-server

# サービスの起動
$ service mysqld start

# 自動起動
$ chkconfig mysqld on
```

```sql
-- 登録されているユーザの確認
select user, host from mysql.user;

-- ユーザ権限の確認
show grants for 'hoge'@'host';

-- ユーザの作成
create user user_name;
create user user_name identified by passwd;

-- 権限の付与
grant select, insert, update on *.* to hoge;
grant select, insert, update on *.* to hoge indentified by passwd;

-- パスワードの設定
set password = password('hogehoge');
set password for user = password('hogehoge');
```

## Rubyのインストール
### rbenv
参考：
http://qiita.com/chezou/items/86ee6ded253c094a23b6
http://office.tsukuba-bunko.org/ppoi/entry/systemwide-rbenv

まず`rvm`をインストールしてしまった場合はアンインストールする。
```sh
$ rvm seppuku
```
##### rbenvのインストール
今回はシステムワイドな利用を想定して`su -`でsuperuserになって作業する。


```sh
# 必要なパッケージの導入
$ yum install -y git gcc gcc-c++ openssl-devel readline-devel

# gitからソース取得して配置
$ cd /usr/local
$ git clone git://github.com/sstephenson/rbenv.git rbenv
$ git clone git://github.com/sstephenson/ruby-build.git rbenv/plugins/ruby-build
```

##### rbenvの設定

```sh
export RBENV_ROOT="/usr/local/rbenv"
export PATH="${RBENV_ROOT}/bin:${PATH}"
eval "$(rbenv init --no-rehash -)"
```
を`/etc/profile.d/rbenv.sh`に保存

##### gemのインストール
```sh
$ rbenv exec gem install bundler
```

##### rbenvの使い方
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

## rvm
##### インストール
```sh
$ curl -sSL https://get.rvm.io | bash -s stable

# 失敗するようならsudoで実行してみる
$ curl -sSL https://get.rvm.io | sudo bash -s stable
```
##### zshrcの編集
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

##### rvmの使い方
```sh
# インストールできるバージョンの一覧を確認
$ rvm list known

# インストール
$ rvm install 2.2

# 利用するバージョンを指定する
$ rvm use 2.2

# 確認
$ ruby -v
$ which ruby
```

## Apache
参考：http://www.kakiro-web.com/linux/centos6-apache-install.html
##### APRのインストール
```sh
# ソースのダウンロード
$ wget http://archive.apache.org/dist/apr/apr-1.5.1.tar.bz2

# パッケージの作成
$ rpmbuild -tb --clean apr-1.5.1.tar.bz2

# インストール
$ rpm -Uvh apr-1.5.1-1.x86_64.rpm apr-devel-1.5.1-1.x86_64.rpm
```

##### freetds-devlのインストール
EPELのリポジトリからインストールを行う。<br>
※ リポジトリを追加していない場合は http://www.kakiro-web.com/linux/centos6-epel-install.html を参考に行う。
```sh
$ yum --enablerepo=epel install freetds-devel
```

##### APR-utilのインストール
```sh
# ソースのダウンロード
$ wget http://archive.apache.org/dist/apr/apr-util-1.5.4.tar.bz2

# パッケージの作成
$ rpmbuild -tb --clean apr-util-1.5.4.tar.bz2

# インストール
$ rpm -Uvh apr-util-1.5.4-1.x86_64.rpm apr-util-devel-1.5.4-1.x86_64.rpm
```

##### distcache-develのインストール
```sh
# ソースの取得
$ wget http://ftp.riken.jp/Linux/fedora/releases/16/Everything/source/SRPMS/distcache-1.4.5-22.src.rpm

# パッケージの作成
$ rpmbuild --rebuild --clean distcache-1.4.5-22.src.rpm

# インストール
$ rpm -Uvh distcache-1.4.5-22.x86_64.rpm distcache-devel-1.4.5-22.x86_64.rpm
```

##### Apacheのインストール
```sh
# ソースのダウンロード
$ wget http://archive.apache.org/dist/httpd/httpd-2.4.9.tar.bz2

# パッケージの作成
$ rpmbuild -tb --clean httpd-2.4.9.tar.bz2

# インストール
$ rpm -Uvh httpd-2.4.9-1.x86_64.rpm httpd-devel-2.4.9-1.x86_64.rpm
```

##### apachectlの使い方的なもの
|        command         |           effect           |
| :--------------------- | :------------------------- |
| apachectl -V           | バージョンの確認           |
| apachectl configtest   | 設定ファルのSyntaxチェック |
| apachectl start        | サービスの開始             |
| apachectl restart      | サービスの再起動           |
| apachectl stop         | サービスの終了             |
| apachectl httpd reload | 設定ファルのリロード       |

##### 関連ファイル
|            path            |        contents        |
| :------------------------- | :--------------------- |
| /etc/httpd/conf/httpd.conf | 設定ファイル           |
| /var/log/httpd/error_l     | エラーログ (default)   |
| /var/log/httpd/access_log  | アクセスログ (default) |

##### apacheの自動起動設定（service に登録
```sh
$ chkconfig httpd on
```

## php 5.6
