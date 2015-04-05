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
# ソースのダウンロード
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

##### apacheのMPMの設定
PHPを利用する場合はpreforkが推奨されている。
```sh
# -------------------------------------------
# file name : /etc/httpd/conf/httpd.conf
# -------------------------------------------

# befor
LoadModule ssl_module lib64/httpd/modules/mod_ssl.so

# after
LoadModule ssl_module lib64/httpd/modules/mod_ssl.so
```
## PHP5.6
##### 前準備
（余分なパッケージがあるかも）
```sh
# EPELからインストール
$ yum --enablerepo=epel install firebird-devel
$ yum --enablerepo=epel install libmcrypt-devel

# バージョン指定なし
$ yum install libjpeg-turbo-devel　bzip2-devel pam-devel libedit-devel libtool-ltdl-devel systemtap-sdt-devel libacl-devel libc-client-devel net-snmp-devel t1lib-devel libpng-devel freetype-devel libXpm-devel libvpx-devel gmp-devel tokyocabinet-devel libtidy-deve recode-devel libtidy-devel

#バージョン指定あり
$ yum install libxslt-devel aspell-devel libicu-devel enchant-devel

```
##### 本編
IUSからPHP5.6のSPRMをダウンロードしてRPMを作成し、インストールを行う。
最新のソース → http://dl.iuscommunity.org/pub/ius/stable/CentOS/6/SRPMS/repoview/php56u.html
アーカイブ → http://dl.iuscommunity.org/pub/ius/archive/CentOS/6/SRPMS/
```sh
# ソースのダウンロード
$  wget http://dl.iuscommunity.org/pub/ius/archive/CentOS/6/SRPMS/php56u-5.6.4-1.ius.centos6.src.rpm

# パッケージの作成
$ rpmbuild --rebuild --clean php56u-5.6.4-1.ius.centos6.src.rpm
```
`httpd-devel < 2.4 is php56u-5.6.6-2.ius.el6.x86_64` と怒られるが、RPMから httpd-devel を導入した場合はパスが違うので出てしまうようだ。`./rpmbuild/SPECS/php56u.spec`を以下のように変更して対応する。
```sh
# 56行目
# before
%{!?_httpd_apxs:       %{expand: %%global _httpd_apxs       %%{_sbindir}/apxs}}

# afer
%{!?_httpd_apxs:       %{expand: %%global _httpd_apxs       %%{_bindir}/apxs}}
```
##### 再開
```sh
# パッケージの作成
$ rpmbuild -bb --clean php56u.spec

# 依存関係の解消にとりあえず依存関係を無視してインストール
$ rpm -Uvh --nodeps php56u-cli-5.6.4-1.ius.el6.x86_64.rpm php56u-xml-5.6.4-1.ius.el6.x86_64.rpm php56u-common-5.6.4-1.ius.el6.x86_64.rpm php56u-process-5.6.4-1.ius.el6.x86_64.rpm php56u-devel-5.6.4-1.ius.el6.x86_64.rpm

# php56u-pearのソースをダウンロード
$ wget http://dl.iuscommunity.org/pub/ius/stable/CentOS/6/SRPMS/php56u-pear-1.9.5-1.ius.centos6.src.rpm

# php56u-pearのパッケージの作成
$ rpmbuild --rebuild --clean php56u-pear-1.9.5-1.ius.centos6.src.rpm

# php56u-pearのインストール
$ rpm -Uvh php56u-pear-1.9.5-1.ius.el6.noarch.rpm

# php56u-pecl-jsoncのソースをダウンロード
# 最新 → http://dl.iuscommunity.org/pub/ius/stable/CentOS/6/SRPMS/repoview/php56u-pecl-jsonc.html
$ wget http://dl.iuscommunity.org/pub/ius/stable/CentOS/6/SRPMS/php56u-pecl-jsonc-1.3.7-1.ius.centos6.src.rpm

# php56u-pecl-jsoncのパッケージの作成
$ rpmbuild --rebuild --clean php56u-pecl-jsonc-1.3.7-1.ius.centos6.src.rpm

# php56u-pecl-jsoncのインストール
$ rpm -Uvh php56u-pecl-jsonc-1.3.7-1.ius.el6.x86_64.rpm php56u-pecl-jsonc-devel-1.3.7-1.ius.el6.x86_64.rpm

# phpのインストール
$ rpm -Uvh php56u-5.6.4-1.ius.el6.x86_64.rpm

# その他利用するパッケージの導入
$ rpm -Uvh php56u-mbstring-5.6.4-1.ius.el6.x86_64.rpm php56u-mysqlnd-5.6.4-1.ius.el6.x86_64.rpm php56u-pdo-5.6.4-1.ius.el6.x86_64.rpm
```

#### Apacheの設定
読み込ませるモジュールのパスが実際のものと異なるので変える
```sh
$ rpm -ql php56u
```
```sh
# -------------------------------------------
# filename : /etc/httpd/conf.d/10-php.conf
# ------------------------------------------

# before
  LoadModule php5_module modules/libphp5.so
# after
  LoadModule php5_module lib64/httpd/modules/libphp5.so


# before
  LoadModule php5_module modules/libphp5-zts.so
# after
  LoadModule php5_module lib64/httpd/modules/libphp5-zts.so
```

##### PHPのモジュールが読み込まれない時は。。。。
`/etc/httpd/conf/httpd.conf` に `Includeoptional /etc/httpd/conf/*.conf` があるかを確認し、なければ追加する。

## FuelPHP
#### 導入
```sh
# oilの取得
$ curl get.fuelphp.com/oil | sh

# プロジェクトの作成
$ mkdir /usr/fuel
$ cd /usr/fuel
$ oil create {my project name}

# 公開
$ ln -s /usr/fuel/{my project name} {document root}
```

##### 設定

```apache
# -------------------------------
# file name : /etc/httpd/conf.d/fuel.conf
# --------------------------------

<Directory /var/www/html/{my project name}>
     AllowOverride All
     Require all granted
</Directory>
```

```apache
# -------------------------------
# file name : public/.htaccess
# -------------------------------

# <-- 略 -->

    # deal with php5-cgi first
    <IfModule mod_fcgid.c>
        # RewriteRule ^(.*)$ index.php?/$1 [QSA,L] # default
        RewriteRule ^(.*)$ {my project name}/index.php?/$1 [QSA,L]
    </IfModule>

    <IfModule !mod_fcgid.c>

        # for normal Apache installations
        <IfModule mod_php5.c>
            # RewriteRule ^(.*)$ index.php/$1 [L] # default
            RewriteRule ^(.*)$ {my project name}/index.php/$1 [L]
        </IfModule>

        # for Apache FGCI installations
        <IfModule !mod_php5.c>
            # RewriteRule ^(.*)$ index.php?/$1 [QSA,L] # default
            RewriteRule ^(.*)$ {my project name}/index.php?/$1 [QSA,L]
        </IfModule>

    </IfModule>

</IfModule>

```

#### permission denyの対応
SELinuxが有効の場合に`mkdir()`, `fopen`をPHPで実行すると発生することがある。

```sh
$ chcon -R -t httpd_sys_script_rw_t {target dir}
```

exec('sudo hogehoge')を実行する → http://ja.528p.com/linux/centos6/RC004-selinux.html


## mecab
```sh
$ rpm -ivh http://packages.groonga.org/centos/groonga-release-1.1.0-1.noarch.rpm

$ yum install groonga-tokenizer-mecab
```
