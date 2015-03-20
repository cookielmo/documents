##rvm
#####インストール
```
$ curl -sSL https://get.rvm.io | bash -s stable
```
失敗するようならsudoで実行してみる
```
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
