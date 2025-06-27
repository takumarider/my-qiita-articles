---
title: "【Rails × Devise】初心者エンジニアがユーザー認証を実装してみた話 〜ログイン機能はDeviseにおまかせ！〜"
tags: ["Rails", "Devise", "ポートフォリオ", "初心者"]
  - ''
private: false
updated_at: ''
id: null
organization_url_name: null
slide: false
ignorePublish: false
---
# 【Rails × Devise】ユーザー認証を実装！モニター開発の実践記録
実装に使ったのは、Ruby on Rails界隈で超有名なライブラリ `Devise`。  
初心者でも扱いやすく、コマンド1つでログイン機能をほぼ自動生成してくれる神ツールです。

## はじめに

この記事は、前回公開した  
[「モニター開発」で進めたら、いい感じの話](https://qiita.com/takumarider/items/6995a3bfef870a9eece7) の続編です。


> 「ログイン機能、ゼロから書かなくてもいいの…！？」
そんな感動を味わった駆け出しエンジニアの記録を、ステップごとにまとめました。


## 🚀 今回実装したこと（Issue#1）

- ユーザー管理機能の導入（新規登録／ログイン／ログアウト）
- ログイン後のマイページ遷移
- ログアウト後はログイン画面にリダイレクト
- ユーザーごとに情報を管理する土台を構築


## 🔧 使用技術

- フレームワーク：Ruby on Rails 7
- ユーザー認証：Devise
- DB：MySQL（ローカル環境）


## 🌱 Deviseの導入手順

```bash
# Gemfile に追記
gem 'devise'

# Bundle install
$ bundle install

# Devise をインストール
$ rails generate devise:install

# Userモデルを作成
$ rails generate devise User

# マイグレーション実行
$ rails db:migrate

# 自動で生成されたログイン／登録画面は、次のコマンドでカスタマイズ可能に
$ rails generate devise:views
```

🌟 Deviseのすごいところ
>初心者の僕が感動したのは、Deviseの「やってくれる力」。
>以下のような処理を、たった数行のコマンドで 自動生成 してくれます。

## ✅自動でルーティングを設定してくれる！
```ruby
# routes.rb
devise_for :users
```
これだけで、下記のようなルーティングが追加されます👇
|         パス        	|       説明       	|
|:-------------------:	|:----------------:	|
|    /users/sign_in   	| ログインフォーム 	|
|   /users/sign_out   	|    ログアウト    	|
|    /users/sign_up   	|     新規登録     	|
| /users/password/new 	| パスワード再設定 	|
|     /users/edit     	| プロフィール編集 	|

## ✅必要なカラムも自動で用意！
例えば、マイグレーションファイルにはこんな感じでカラムが定義されます👇

```ruby
## db/migrate/xxxxxxxxxx_devise_create_users.rb

t.string :email,              null: false, default: ""
t.string :encrypted_password, null: false, default: ""

t.string   :reset_password_token
t.datetime :reset_password_sent_at

t.datetime :remember_created_at

# さらに確認機能やロック機能などもコメントで用意されています
```
>その他にも、パスワードリセットやログイン情報の追跡に必要なカラムが最初から用意されています。
>🔰 認証に必要な情報がすべて揃っているので、初心者には本当にありがたい…！

## 🙌 まとめ
Deviseを使えば、面倒なユーザー認証機能をあっという間に実装できます。

>初心者の僕でも、詰まることなく最低限の機能が揃いました。
>「ログイン機能どうやって作ればいいの？」と悩んでる人には、まず Devise をおすすめしたい！


## 👤 プロフィール
駆け出しバックエンドエンジニア。Railsを中心にポートフォリオ開発中。
「困ったら調べて深めて、、学んだことをQiitaに残す」が最近のモットーです！

