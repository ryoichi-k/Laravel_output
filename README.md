# Laravel_output

public →js css公開してもいいもの

routes　→アプリのURL設定

storage →セッションやログなど

vendor composerの依存内容

マイグレーション→sql不要でDB管理できる。

マイグレーションファイル作成

ファイルにテーブル定義を書く

マイグレーションを実行しDBに反映

実行コマンド

php artisan migrate

schemaファザードでテーブル作成

テーブル名は複数形

カラムの作成はblueprintオブジェクトのtableメソッドを使う

```php
public function up()
    {
        Schema::create('articles', function (Blueprint $table) {
            $table->bigIncrements('id');
            $table->string('title');
            $table->text('body');
            $table->bigInteger('user_id');
            $table->foreign('user_id')->references('id')->on('users');
            $table->timestamps();
        });
    }
```

## MVC

controller処理の割り振り

Model

Eloquent ORM

モデルとDBを対応させる

ORM→DBから取得したデータをオブジェクトとして扱える。

php artisan make:model モデル名

モデル名はテーブル名の単数形

複数代入

fillable　保存、更新したいカラム

guarded　ブラックリスト

テーブルが増えたらモデルを作っていく

ルーティング

どのコントローラー動かすかを定義する場所

web.php

httpメソッドも定義できる

ルーティングは処理ごとに定義

名前付きルート　ルートに名前がつけられる。→URL変更の際に便利

ここで名前をつけたら大体他では名前の方を使う。

```php
Route::get('/', 'BlogController@showList')->name('blogs');
```

コントローラー

ルートで定義されたHTTPリクエストの処理を書いていく

ルートから先に作る。そしてコントローラー

抜粋した最後の1行の`Method`の列には、`POST`と書かれています。

また、`Action`列を見ると、「登録」を意味する`register`アクションメソッドが処理されることがわかります。

HTTPのPOSTメソッドは、サーバー側にリソースを登録する時に使う、と先ほど説明しましたが、これらのことから`RegisterController`の`register`アクションメソッドではユーザー登録の処理を行うということが想像できます。

この`register`アクションメソッドの実際の処理内容についても、次のパートで確認していきます。

認証関連のルーティングを定義したので、通常であれば次はコントローラーを作成

トレイト

`trait`と宣言されているものがトレイト
```php
<?php
// 略
class RegisterController extends Controller
{
    // 略
    use RegistersUsers;
    // 略
}
```

トレイトは、そのままではクラスとして使用できず、

他のクラスの中で`use トレイト名`と記述します。

このようにすると、このクラスにおいて、トレイト内で定義している機能が使えるようになります。

汎用性の高い機能をトレイトとしてまとめておき、他の複数のクラスで共通して使う、といった使い方をします。

`vendor`ディレクトリは`composer install`コマンドによってインストールされたPHPの各種ライブラリのコードが配置される箇所ですが、通常はここにあるコードを直接修正することはしません。

Laravelの`route`関数は、与えられた名前付きルートのURLを返します。

csrfは、Cross-Site Request Forgeries(クロスサイト・リクエスト・フォージェリ)というWebアプリケーションの脆弱性の略称で、上記の`input`タグはこの脆弱性からWebサービスを守るためのトークン情報です。

Laravelでは、クライアントからのPOSTメソッドのリクエストを受け付ける際に、リクエスト中のこのトークン情報の内容を見て、不正なリクエストでないかどうかをチェックします。

POSTメソッドであるリクエストにこのトークン情報が無いと、Laravelではエラーをレスポンスします。

ですので、**POST送信を行うBladeには`@csrf`を含めるようにしてください**

`old`関数は、引数にパラメータ名を渡すと、直前のリクエストのパラメータを返します。

ユーザー登録処理でバリデーションエラーになると再びユーザー登録画面が表示されますが、特に何も対応していなかった場合、全ての項目が空で表示されて入力を最初からやり直さなければなりません。

`old`関数を使うことで、入力した内容が保持された状態でユーザー登録画面が表示されるようになり、ユーザーはエラーになった箇所だけを修正すれば良くなります。

ただし、`password`と`password_confirmation`は`old`関数を使っても`null`が返ります。

そのため、これら入力項目の`input`タグに対しては`old`関数を使っていません。

## [補足]もしログアウト後のリダイレクト先を変えたい場合は
別にLaravelでWebアプリケーションを今後作ることになり、ログアウト後のリダイレクト先を`/`ではない別のURLにする必要があった場合の対応方法を説明します。

`AuthenticatesUsers`トレイトの`loggedout`メソッドの処理内容を変えることができれば良いのですが、`AuthenticatesUsers`トレイトは以下の通り`vendor`ディレクトリ配下にあります。

[https://www.techpit.jp/courses/11/curriculums/12/sections/108/parts/396](https://www.techpit.jp/courses/11/curriculums/12/sections/108/parts/396)

## **@guest, @auth**

`@guest`から`@endguest`に囲まれた部分は、ユーザーがまだログインしていない状態の時のみ処理されます。

逆に`@auth`から`@endauth`に囲まれた部分は、ユーザーがログイン済みの状態の時のみ処理されます。

これらを使って、ログイン前、ログイン済みそれぞれで見せるべきメニュー、見せるべきでないメニューを出し分けるようにしました。

## **ユーザー登録画面への遷移を可能にする**

`route`関数を使って、ユーザー登録画面への遷移を可能にしています

```php
<a class="nav-link" href="{{ route('register') }}">ユーザー登録</a> {{--この行を編集--}}
```
blade

テンプレートを継承できる
```php
@extends('bladeを指定')
カッコ内のbladeを継承する

@yield('content')
カッコ内が同じ@sectionを読み込む→contentの名前は任意

@section('content')~@endsection
このブロック内容をyieldに追加する　→ htmlとか書く

@include(file)
カッコ内のファイルを読み込む
```
seeder でテストデータを用意

seederはsql不要でDBにデータ挿入できる。テストデータ挿入の際によく使われる。

よく使う

Factory→seederに自動で名前や住所などのランダムデータを与えられる仕組み
```php
php artisan make:seeder シーダー名
```

## Carbon
```php

$date = new Carbon();
$date = Carbon::now(); // 現在時刻


new Carbon('today'); // 今日
new Carbon('tomorrow'); // 明日
new Carbon('yesterday'); // 昨日
new Carbon('now'); // 今
new Carbon('last month'); // 前の月
new Carbon('next year'); // 次の年
new Carbon('+1 day'); // 1日後
new Carbon('-2 weeks'); // 2週間前
new Carbon('+3 minutes'); // 3分後
new Carbon('-4 seconds'); // 4秒前
new Carbon('+5 years'); // 5年後
new Carbon('-6 months'); // 6ヵ月前
new Carbon('2021-01-01'); // 2021年1月1日
new Carbon('2021-01-01 01:02:03'); // 2021年1月1日 01時02分03秒
```

Laravelトランザクション
```php
DB::transaction(function () {
    // 更新処理
    DB::table('users')->update(['votes' => 1]);

    DB::table('posts')->delete();
});
```

DBファザードのtransactionメソッドを利用した実装
```php
<?php

namespace App\Http\Controllers;

use App\User;
use Illuminate\Http\Request;
use App\Http\Controllers\Controller;

class UserController extends Controller
{
    public function update (Request $request)
    {
        $user = User::find($request->id);

        return DB::transaction(function () use ($user, $request) {
            $user->fill($request->all();)->save();

        return $user;
    });
}
```

### **手動のトランザクション（DBファザードのbeginTransaction, rollBack, commit を利用する）**

try,catchを用いて更新処理を行います。
```php
<?php

namespace App\Http\Controllers;

use App\User;
use Illuminate\Http\Request;
use App\Http\Controllers\Controller;

class UserController extends Controller
{
    public function update (Request $request)
    {
        $user = User::find($request->id);

        DB::beginTransaction();
        try {
            $user->fill($request->all();)->save();
            DB::commit();
        } catch (\Exception $e) {
            DB::rollback();
        }
        return $user;
}
```

### DBファザード・・・SQLをそのまま実行できる、ベタがき

```php
<?php

namespace App\Http\Controllers;

use App\Http\Controllers\Controller;
use Illuminate\Support\Facades\DB;  //ここを追加しないといけない

class UserController extends Controller
{
    /**
     * アプリケーションの全ユーザーリストを表示
     *
     * @return Response
     */
    public function index()
    {
        $users = DB::select('select * from users where active = ?', [1]);

        return view('user.index', ['users' => $users]);
    }
}
```

マイグレーションファイル編集

マイグレーション実行

```php
docker-compose exec workspace php artisan migrate
```

- 作成するマイグレーションファイルですが、基本的には作成するテーブル１つにつき１つのマイグレーションファイルを作成します。
- `create_`と`_table`の箇所は書かなくても良いですが、何の為のファイルかを一目で分かるように書いておくのが無難です。(laravelの公式の書き方に倣っています)
- crete_`books`_tableのように、テーブル名は複数形で作るようにしましょう。理由は、ここで記述した名前(books)がそのままテーブル名になるからです。
- ここでの名前は、生成するマイグレーションファイルのクラス名に反映されるので、そのことを考慮した上で名前をつけましょう。

カラム名変更方法[https://qiita.com/kamotetu/items/b28a684ed4eecdb96a31](https://qiita.com/kamotetu/items/b28a684ed4eecdb96a31)

モデルの作成

モデルは単数形

```php
docker-compose exec workspace php artisan make:model Article
```

コマンドが成功すると、`laravel/app`ディレクトリに`Article.php`が作成されます。

[https://www.techpit.jp/courses/11/curriculums/12/sections/107/parts/389](https://www.techpit.jp/courses/11/curriculums/12/sections/107/parts/389)

**ルーティングの追加**

`laravel/routes/web.php`を以下の通り編集

```php
<?php

Auth::routes(); //-- この行を追加
Route::get('/', 'ArticleController@index');
```

ルート確認方法

```php
docker-compose exec workspace php artisan route:list
```

**コントローラーの確認とリダイレクト先の変更**

デバッグ

dd($配列名);

コントローラーで使えて、そこで処理を止めてくれる。

流れ

詳細表示したいブログのIDでリンクを作って飛ばす

routeでIDを受け取りcontrollerへ渡す

Modelで該当データを取り出す

Blog::find(1)

Viewで表示する

controllerのshowDetailから渡す

/blog/1

ルートパラメータを使う

```php
Route::get('/blog/{id}', 'BlogController@showDetails')
```

コンポーネントとスロット

[https://qiita.com/shizen-shin/items/c211fb4d2962753a73c1](https://qiita.com/shizen-shin/items/c211fb4d2962753a73c1)

## **コンポーネントの使い方**

コンポーネントの中身を表示する方法は大きく３つ。

**1. コンポーネントの中身をそのまま表示2. 変数slotでデータを渡す3. 指定したslot名でデータを渡す**

２と３でslotが登場するのがわかりにくい、、それぞれ用途が異なる。

サブビュー

@include

コレクションビュー

@each

## クエリパラメータ

```php
$var = $request->query('id');
```

?id = 123456ならquery('id')で$varに123456の値が取得できる。

### クエリパラメータを付けてリダイレクト

```php
return redirect()->route(ルート, 渡したい情報の連想配列);
```
