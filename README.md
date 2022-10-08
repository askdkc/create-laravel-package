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
それでは実際にパッケージをいじってみましょう

### ServiceProvider
まず始めにサービスプロバイダーを確認します<br>
次のファイルをお好みのエディタで開きます<br>

`src/MylarapackServiceProvider.php`

```vim
このパッケージでは簡単なビューを表示させる例を作成しますので、下記のように修正します

18行目付近
---before---
$package
    ->name('mylarapack')
    ->hasConfigFile()
    ->hasViews()
    ->hasMigration('create_mylarapack_table')
    ->hasCommand(mylarapackCommand::class);
------------
↓
---after---
$package
    ->name('mylarapack')
    ->hasViews()
    ->hasRoute('web');
-----------
```

### Routeの作成
次にRouteファイルを作成します<br>
Routeファイルは通常のLaravelと同様、routes/web.phpファイルを作成します
```zsh
ファイルの作成（エディタから作ってもOKです）
touch routes/web.php
```
routes/web.phpをお好みのエディタで開き、下記のように`/larapack`というrouteを作成します
```vim
<?php

use Illuminate\Support\Facades\Route;

Route::get('/larapack', function () {
    return view('mylarapack::index');
});
```
パッケージ内のviewファイルを読み込ませる場合には `パッケージ名::viewファイル名` を指定する必要があります<br>
(そうしないとLaravelが指定されたviewがどこにあるのか探せないので)

### Viewの作成
Routeで作成したviewを作ります<br>
場所は通常のLaravel同様のパス`resources/views/`をパッケージ内に作るイメージです
```zsh
ファイルの作成（エディタから作ってもOKです）
touch resources/views/index.blade.php
```
上記Viewファイルをお好みのエディタで開き、適当な文字を書いておきます
```vim
<h1>Hello from Mylarapack!</h1>
見えてる？
```

## テスト
それでは作った内容が上手く動くかテストを書いてみましょう<br>
このスケルトンパッケージでは上記の具体例のようにPhpStanを有効化してあると、お手軽にテストが実行できるようになっています
<br><br>
まずはテストに必要なパッケージを `composer` でインストールしましょう

```zsh
composer install
```
次にテスト用ファイルを編集しましょう<br>
お好みのエディタで `tests/ExampleTest.php` を開きます

```vim
---before---
<?php

it('can test', function () {  //この辺は初期サンプルなので消します
    expect(true)->toBeTrue();  //この辺は初期サンプルなので消します
});
------------
↓
---after---
<?php

// エルデのテスト
test('この先、Status 200があるぞ', function() {
    $this->get('larapack')->assertStatus(200);
});

test('この先、ViewにHelloがあるぞ', function() {
    $this->get('larapack')->assertSee('Hello');
});
----------
```

ではテストを実行してみましょう<br>
```zsh
composer test
````
問題なければ下記の様に出力されます
```zsh
> vendor/bin/pest

   PASS  Tests\ExampleTest
  ✓ この先、Status 200があるぞ
  ✓ この先、ViewにHelloがあるぞ

  Tests:  2 passed
  Time:   0.08s

```

## Laravelにパッケージをインストール
それでは作ったパッケージを実際にLaravelにインストールしてみましょう<br>
このパッケージはテストしやすいように既に新規Laravel内に作られているので、後はLaravelがそれを発見できるようにするだけです
<br><br>
まず、Laravelのトップディレクトリまで戻ります
```zsh
現在のディレクトリを確認すると恐らく下記の様な感じでパッケージの中にいると思います

pwd
/Users/UserName/Sites/larapack/packages/mylarapack

なのでLaravelプロジェクト(larapack)のディレクトリまで戻ります
cd ../..

pwd
/Users/UserName/Sites/larapack
```
次にLaravelのcomposer.jsonを編集して作成したパッケージが読み込める様にします<br>
お好みのエディタで下記の様に修正
```vim
composer.json 末尾付近
---before---
    "minimum-stability": "dev",
    "prefer-stable": true
}
------------
↓
---after---
    "minimum-stability": "dev",
    "prefer-stable": true,  // ← ここに , 足すの忘れないでね
    "repositories": {
        "local": {
            "type": "path",
            "url": "./packages/*",
            "options": {
                "symlink": true
            }
        }
    }
}
-----------
```
作成したパッケージをLaravelにインストールします
```zsh
composer require askdkc/mylarapack  ← パッケージ名を変えてる場合は、ここも適宜変えてね
```

## 動作確認
Laravelを起動して動作確認します
```zsh
php artisan serve
```

下記にアクセス<br>
http://localhost:8000/larapack

画面に<br><br>
`Hello from Mylarapack!`
<br><br>
と表示されたら成功です

## 追加の開発
現在の状態になればLaravelのディレクトリ内にある `packages/mylarapack` に加えた変更は直ぐにLaravelから確認可能になっています<br>
Viewファイルに変更を加える等、色々といじってみましょう

## パッケージのGitHubへの登録
作成したパッケージはGitHubに登録し、その登録したリポジトリを[packagist](https://packagist.org)に登録することで、直ぐに全世界に公開することができます

### パッケージのgitへのファイル追加と初期コミット
今回の例では、次の様にすることによってGitHubへのpushの準備が可能です<br>
1. パッケージのディレクトリに移動します

```zsh
cd packages/mylarapack 
```
2. gitコマンドでファイルを追加してコミットします（既にスケルトンが.gitignoreを用意してるので楽々です）

```zsh
git add .
git commit -am "Initial Commit"
```

3. GitHubで新規リポジトリを作成します

4. GitHubのリポジトリの説明にある **…or push an existing repository from the command line** の手順を実行します
一例：
```zsh
git remote add origin git@github.com:USERNAME/YOUR_REPOSITORY.git
git branch -M main
git push -u origin main 
```

## それでは楽しい開発ライフを！



