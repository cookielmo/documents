#Ruby
##Scraping with Ruby
#####Nokogiriのインストール
```sh
$ gem install nokogiri
```
#####Sample1
```ruby
# URLにアクセスするためのライブラリの読み込み
require 'open-uri'
# Nokogiriライブラリの読み込み
require 'nokogiri'

# スクレイピング先のURL
url = 'http://www.yahoo.co.jp/'

charset = nil
html = open(url) do |f|
  charset = f.charset # 文字種別を取得
  f.read # htmlを読み込んで変数htmlに渡す
end

# htmlをパース(解析)してオブジェクトを生成
doc = Nokogiri::HTML.parse(html, nil, charset)

# タイトルを表示
p doc.title
```

#####Sample2
```ruby
require 'open-uri'
require 'nokogiri'

# スクレイピング先のURL
url = 'http://matome.naver.jp/tech'

charset = nil
html = open(url) do |f|
  charset = f.charset # 文字種別を取得
  f.read # htmlを読み込んで変数htmlに渡す
end

# htmlをパース(解析)してオブジェクトを作成
doc = Nokogiri::HTML.parse(html, nil, charset)

doc.xpath('//li[@class="mdTopMTMList01Item"]').each do |node|
  # tilte
  p node.css('h3').inner_text

  # 記事のサムネイル画像
  p node.css('img').attribute('src').value

  # 記事のサムネイル画像
  p node.css('a').attribute('href').value
end
```

##mySQL
#####ruby-mysqlのインストール
```sh
$ gem install ruby-mysql
```
#####Sample
```ruby
require 'mysql'
client= Mysql.connect('hostname', 'username', 'password', 'dbname')
client.query("SELECT col1, col2 FROM tblname").each do |col1, col2|
  p col1, col2
end
stmt = client.prepare('INSERT INTO tblname (col1,col2) VALUES (?,?)')
stmt.execute 123, 'abc'
```
