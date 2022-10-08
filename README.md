# Laravelのパッケージの作成ガイドだよ
ここではLaravel用のパッケージを作成するやり方を紹介するよ

## 新規Laravelプロジェクトの作成

まず初めにLaravelの新規プロジェクト(ここでは名前を"larapack"にしておきます)を作成します

```zsh
laravel new larapack
```

## 新規プロジェクトにパッケージスケルトンを導入

新規Laravelプロジェクト配下に移動して、パッケージ作成用のスケルトンリポジトリをコピーしてきます

パッケージ作成用のスケルトンリポジトリは[こちら](https://github.com/askdkc/package-skeleton-laravel)
> **ちなみに：**
> 
> スケルトンリポジトリの使い方に従ってリポジトリテンプレートを自分のGitHubリポジトリに複製した場合には、基本的にそちらを使ってください

```zsh
cd larapack
```

ここで新規laravelプロジェクト配下に packages/パッケージ名 となるようにcloneします

```zsh
git clone git@github.com:askdkc/laravel-package-tools.git packages/mylarapack
```

## スケルトンの自動生成機能を使い、パッケージをセットアップ

git cloneしたスケルトン内にある`configure.php`を実行してパッケージの初期セットアップをします

```zsh
cd packages/mylarapack
php ./configure.php

セットアップでは下記のような質問が出てくるので、自分の環境に合わせて入力していきます
()内はデフォルト値なので、何も入力せずにEnterすると、この()内の値が使われます

---以下、実際のパッケージ生成のための質問---
作者名は？(そのままEnterすると()内の値が使われます) (dkc): askdkc 
E-mailアドレスは？ (askdkc@users.noreply.github.com): 
作者のユーザ名は？ (askdkc): 
Vendor名は？ (askdkc): 
Vendor namespaceは？ (Askdkc):    ← vendor名は基本githubのユーザ名と一緒です
パッケージ名は何にする？ (mylarapack):    ← パッケージ名を変えたい時はここで変えます
Class名はどうする？ (Mylarapack): 
パッケージの概要はどうする？ (このパッケージはmylarapackです): 
PhpStanを有効化する？ (Y/n):  ← この辺はONにしておくと後が楽
Laravel Pintを有効化する？ (Y/n):   ← この辺はONにしておくと後が楽
GitHubのDependabotを有効化する？ (Y/n):   ← この辺はONにしておくと後が楽
デバッグ用にSpatieのRayを使う？ (Y/n): n.   ← SpatieのRayを使ってない人は基本 n です
変更履歴（Changelog）を自動更新するGitHubのworkflowを有効化する？ (Y/n): 
------
Author     : askdkc (askdkc, askdkc@users.noreply.github.com)
Vendor     : askdkc (askdkc)
Package    : mylarapack <このパッケージはmylarapackです>
Namespace  : Askdkc\Mylarapack
Class name : Mylarapack
---
Packages & Utilities
Use Laravel/Pint       : yes
Use Larastan/PhpStan : yes
Use Dependabot       : yes
Use Ray App          : no
Use Auto-Changelog   : yes
------
このスクリプトは上記の内容でスケルトンの中身を全て書き換えます
変更しちゃっていい？ (Y/n): 
`composer install`を実行して、初期テストしちゃう? (y/N): 
さて、もうこのスクリプト自身が不要なので、消しときましょうか？ (Y/n): 

---質問終了---

```

## 実際にパッケージをいじってみよう




