---
# try also 'default' to start simple
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://source.unsplash.com/collection/94734566/1920x1080
# apply any windi css classes to the current slide
class: 'text-center'
# https://sli.dev/custom/highlighters.html
highlighter: shiki
# show line numbers in code blocks
lineNumbers: false
title: ActiveRecordパターンの呪縛を学びほぐして挑むクリーンアーキテクチャへの入り口
# some information about the slides, markdown enabled
info: |
 ActiveRecordパターンに引きづられて設計が歪みがちになるのを学びほぐすことでクリーンアーキテクチャでの開発の良いスタートラインに立てるのではないでしょうか？

# persist drawings in exports and build
drawings:
  persist: false
# page transition
transition: slide-left
# use UnoCSS
css: unocss
fonts:
  # basically the text
  sans: 'Noto Sans JP'
  # use with `font-serif` css class from windicss
  serif: 'Noto Serif JP'
  # for code blocks, inline code, etc.
  mono: 'Noto Sans Mono'
addons:
  - "@katzumi/slidev-addon-qrcode"
  - "@katzumi/slidev-addon-blog-card"
---

# ActiveRecordパターンの呪縛を学びほぐして挑むクリーンアーキテクチャへの入り口

[PHPカンファレンス沖縄2023](https://phpcon.okinawa.jp/)　Sep 16th, 2023.  
v0.0.1

<div class="pt-12">
  <span @click="$slidev.nav.next" class="px-2 py-1 rounded cursor-pointer" hover="bg-white bg-opacity-10">
    Press Space for next page <carbon:arrow-right class="inline"/>
  </span>
</div>

<div class="abs-br m-6 flex gap-2">
  <button @click="$slidev.nav.openInEditor()" title="Open in Editor" class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-white">
    <carbon:edit />
  </button>
  <a href="https://github.com/k2tzumi/clean-architecture-anti-pattern/blob/main/slides.md" target="_blank" alt="GitHub"
    class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-logo-github />
  </a>
</div>

<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
-->

---
transition: fade-out
layout: two-cols-header
---

# 自己紹介

katzumi（かつみ）と申します  

「障害のない社会をつくる」をビジョンに掲げている「りたりこ」という会社に所属しています
<a href="https://litalico.co.jp/">
<img src="https://litalico.co.jp/ogp.png" class="w-40" />
</a>

以下のアカウントで活動しています    

::left::

<img src="https://pbs.twimg.com/profile_images/799890486773170176/KN4gKfS2_400x400.jpg" class="rounded-full w-40 mt-16 mr-12　float-left"/>  
<QRCode width="180" height="180" value="https://twitter.com/katzchum" color="4329B9" image="Logo_of_Twitter.svg" />

<logos-twitter /> [katzchum](https://twitter.com/katzchum)

::right::

<img src="https://avatars.githubusercontent.com/u/1182787?v=4" class="rounded-full w-40 mt-16 mr-12"/>

<logos-github-octocat /> [k2tzumi](https://github.com/k2tzumi)  
<simple-icons-zenn /> [katzumi](https://zenn.dev/katzumi)  

<br />

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

---
layout: default
---

# ActiveRecordパターンとは？

皆んな大好きActiveRecordパターン  

データベースの操作をオブジェクト指向のアプローチで行うためのデザインパターン  
具体的には、ActiveRecordパターンでは、データベースの各テーブルに対応するモデル（クラス）を作成します。  
このモデルは、データベーステーブルの行と1対1で対応し、各行の属性（カラム）はモデルのプロパティになります。  
これにより、データベースの操作をオブジェクト指向の方法で行えるようになります。[^1]

[^1]: ChatGPTさん曰く


---
layout: center
---

# PHPのクラスでパターンを実装してみると

---
layout: two-cols
---

```php
class User {
    /**
     * @var int $id
     * @var string $username
     * @var string $email
     */
    public function __construct(
      private int $id,
      public string $username,
      public string $email
    ) {
    }

    // ユーザー情報をデータベースに保存するメソッド
    public function save(): void {
        // データベースに接続し、情報を保存
    }

    // ユーザー情報を更新するメソッド
    public function update(): void {
        // データベースに接続し、情報を更新する
    }

    // ユーザー情報を削除するメソッド
    public function delete(): void {
        // データベースから情報を削除する
    }
```

::right::

```php
    /**
     * IDを指定してユーザーを取得する静的メソッド
     * @param int $id
     * @return self
     */
    public static function find(int $id): self {
        // データベースから指定されたIDのユーザー情報を取得する
    }

    /**
     * すべてのユーザーを取得する静的メソッド
     * @return list<self>
     */
    public static function all(): array {
        // データベースからすべてのユーザー情報を取得する
    }
}
```

---
layout: center
---

# 各種フレームワーク・ライブラリで採用

---
layout: two-cols
---

## フルスタックフレームワーク

* Laravel <skill-icons-laravel-light />  
Eloquent ORM
* Symfony <skill-icons-symfony-light />  
Doctrine ORM
* Yii framework <devicon-yii-wordmark />  
ActiveRecord
* Ruby on Rails <logos-rails />  
ActiveRecord
* Spring Boot <devicon-spring-wordmark />  
Spring Data JPA

::right::

## ライブラリ

* Java <devicon-java-wordmark />  
Hibernate
* Golang <logos-gopher />  
GORM, XORM

---

# Eloquent ORMでのモデル作成


```console
$ php artisan make:model User
```
こうして

<v-click>
```php
namespace App\Models;

use Illuminate\Database\Eloquent\Model;

class User extends Model
{
    protected $fillable = ['username', 'email'];

    protected $table = 'users';
}
```

こうなる（する）
</v-click>

---

# Eloquentモデルの使い勝手

```php {1-7|8-12|13-16|17-22}
use App\Models\User;

// ユーザーを作成
$user = new User();
$user->username = 'john_doe';
$user->email = 'john@example.com';
$user->save();

// ユーザーを更新
$user = User::find(1);
$user->email = 'updated_email@example.com';
$user->save();

// ユーザーを削除
$user = User::find(1);
$user->delete();

// ユーザーを検索
$users = User::all();
foreach ($users as $user) {
    echo $user->username . ': ' . $user->email . PHP_EOL;
}
```

---
layout: center
---

# 👍 超絶便利！

---
layout: center
---

# SQL書かなくていい！
# テーブルとオブジェクトが自動的にマッピングされる！

---
layout: center
---

# 深掘りしてみる

---
layout: image-right
image: https://m.media-amazon.com/images/I/51QgGZaWj-L._SX515_BO1,204,203,200_.jpg
---

# 由来

RailsのActiveRecordが初出ではない

Ruby on RailsのActiveRecordという名称は、2002年にマーチンファウラー氏が著した書籍「Patterns of Enterprise Application Architecture（PoEAA）」で紹介されたActiveRecordパターンに由来しています。[^1]

[^1]: パターン集なので、用語自体はもっと先からあった模様

<!--
エンタープライズ・アプリケーション開発者が直面する厳しい課題に直接応えるために書かれた。
著者が同じ設計アイデアが共通の問題に適用できると気づき、専門家の協力で40種類のパターンを抽出してまとめたもの。
Railsの最初のバージョン公開は2004年になります
-->

---

# PoEAAがまとめたパターンのグループ
以下の9つのグループに40種類のパターンをまとめられている [^1]

1. Domain Logic Patterns
2. Data Source Architectural Patterns
3. Object-Relational Behavioral Patterns  
4. Object-Relational Structural Patterns
5. Object-Relational Metadata Mapping Patterns
6. Web Presentation Patterns
7. Distribution Patterns
8. Session State Patterns
9. Base Patterns

[^1]: 各パターンの邦訳名は以下URLを参照  
https://bliki-ja.github.io/pofeaa/CatalogOfPofEAA_Ja

---

# PoEAAがまとめたパターンのグループ
ActiveRecordパターンに言及されている領域

1. Domain Logic Patterns

<div class="mention">
2. Data Source Architectural Patterns　←　ココ！
</div>  

3. Object-Relational Behavioral Patterns  
4. Object-Relational Structural Patterns  
5. Object-Relational Metadata Mapping Patterns  
6. Web Presentation Patterns  
7. Distribution Patterns  
8. Session State Patterns  
9. Base Patterns  

---

# Data Source Architectural Patterns

Data Source Architectural Patternsは、データベースとのやり取りをするためのパターンをまとめたもの

* Table Data Gateway  
データベースのテーブルに対する操作を提供するオブジェクトのパターン
* Row Data Gateway  
データベースの行に対応するオブジェクトのパターン
* <div class="mention">Active Record　←　コレ！</div>  

  データベースの行に対応するオブジェクトで、自分自身を保存や削除できるパターン  
* Data Mapper  
データベースの行とドメインオブジェクトの間のマッピングを担当するオブジェクトのパターン

---
layout: center
---

# どういうことだってばよ？

---

# 改めてActiveRecordとは？

https://bliki-ja.github.io/pofeaa/ActiveRecord より

"An object that wraps a row in a database table or view, encapsulates the database access, and adds domain logic on that data.  
An object carries both data and behavior.  "

> データベースのテーブルやビューの列をラップし、データベースアクセスをカプセル化し、ドメインロジックを追加するオブジェクト
> データと振る舞いの両方を持つオブジェクト

---
layout: center
---

# 🤔

---

# 改めてActiveRecordとは？

https://bliki-ja.github.io/pofeaa/ActiveRecord より

"An object that wraps a row in a database table or view, encapsulates the database access, and <div class="mention">adds domain logic on that data.</div>  
An object <div class="mention">carries both data and behavior. </div> "

> データベースのテーブルやビューの列をラップし、データベースアクセスをカプセル化し、<div class="mention">ドメインロジックを追加するオブジェクト</div>
> <div class="mention">データと振る舞いの両方を持つ</div>オブジェクト

---

# データベースとのやり取りをするだけではない！

Data Source Architectural Patterns以外のパターン（機能とその責務）も含まれている

* <div class="mention">Domain Logic Patterns　←　特にココ！</div>  
ビジネスロジックの一部であるドメインロジックを実装するためのパターンをまとめたもの  
* Object-Relational Behavioral Patterns  
オブジェクトとリレーショナルデータベースの間の振る舞いの問題を解決するためのパターンをまとめたもの  
* Object-Relational Structural Patterns  
オブジェクトとリレーショナルデータベースの間の構造の問題を解決するためのパターンをまとめたもの  
* Object-Relational Metadata Mapping Patterns  
オブジェクトとリレーショナルデータベースの間のマッピングをメタデータで管理するためのパターンをまとめたもの  

<!--
* Object-Relational Behavioral Patterns  
  * Unit of Work  
Active Recordは、オブジェクトの変更を追跡し、データベースに一括で反映するパターンを有する
  * Identity Map  
Active Recordは、同じデータベースのレコードに対応するオブジェクトを一意に管理するパターンを有する
* Object-Relational Structural Patterns  
  * Identity Field  
Active Recordは、オブジェクトに一意な識別子を持たせるパターンを有する
  * Foreign Key Mapping  
Active Recordは、オブジェクトの関連をデータベースの外部キーで表現するパターンを有する
* Object-Relational Metadata Mapping Patterns  
  * Metadata Mapping  
Active Recordは、オブジェクトとデータベースのマッピングは設定より規約（convention over configuration）で表現します
-->

---
layout: center
---

# ActiveRecordはもの凄くリッチなパターン

---
layout: center
---

# アーキテクチャパターンとしての向き・不向きがある

---

# ActiveRecordパターンが向いているケース

* ドメインロジックがシンプル
* ドメインモデルとテーブルとの構造が非常に近い  
Active Recordパターンでは、テーブルとクラス、行とインスタンス、カラムとプロパティを「全て1対1で割り当てる」ルールが前提になります

<br />

<v-click>

<div class="fusen-003">
上記の前提が守られている場合は非常に強力に機能する！<br />  
強力すぎるが故に、その前提を保つことに注力するように意識が向くようになる<br />  
</div>

</v-click>

<style>
.fusen-003 {
    display: flex;
    align-items: center;
    position: relative;
    padding: 2em 2.2em;
    border-left: 27px solid #2589d0;
    background-color: #a9ceec;
    color: #333333;
}

.fusen-003::before {
    position: absolute;
    bottom: -5px;
    right: 7px;
    z-index: -1;
    transform: rotate(5deg);
    width: 70%;
    height: 50%;
    background-color: #d0d0d0;
    content: "";
    filter: blur(4px);
}
</style>

---

# パターンの前提が崩れシンプルでなくなってきた時に現れるワード

* Fat Model
* Fat Controller
* MVAC
* Service Object

---
layout: center
---

# 責務が色々溢れてきてしまっている

---

# 関心事（＝責務）を分けて考えると
ドメインの振る舞いとデータの永続化は別レイヤーの関心事

ActiveRecordパターンは

<div class="mention">Domain Model</div>(Domain Logic Patterns) + <div class="mention">Row Data Gateway</div>(Data Source Architectural Patterns)

となる。  
業務ロジックを持ったRow Data Gateway。  
ActiveRecordパターンは敢えてレイヤーを密結合にしてDRY [^1] で書くことにフォーカスしています。

[^1]: Don't Repeat Your Self：繰り返しを避けること

---

# レイヤードにしていく理由
複雑性に対峙する為

* ひとつのことをうまくうまくこなすように分離する
* テストをしやすくする  
Mockしやすくなる。DBに依存しないテストになる
* 認知負荷を下げる為に小さくする

---
layout: center
---

# レイヤー増えるだけなの？それってServiceLayer足しただけでは？🤔

---
layout: center
---

# マインドセットが違う

---
layout: image-right
image: https://www.martinfowler.com/eaaCatalog/ServiceLayerSketch.gif
---

# データソースを中心にした設計

ドメインがデータソースに依存している

* データソースが変更した場合にドメインモデルが影響を受ける

<arrow x1="500" y1="435" x2="700" y2="300" color="red" width="3" arrowSize="2" />

---

# ドメインモデルを中心にした設計

[The Clean Architecture](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html)
<img src="https://blog.cleancoder.com/uncle-bob/images/2012-08-13-the-clean-architecture/CleanArchitecture.jpg" class="h-90 rounded shadow" alt="CleanArchitecture" />


データソースレイヤーは外に配置されている

<arrow x1="90" y1="480" x2="110" y2="380" color="red" width="3" arrowSize="2" />

---
layout: center
---

# 重要な原則
複雑さに対峙する為に

---
layout: center
---

# SRP: 単一責務の原則（Single Responsibility Principle）


---
layout: end
---

ご清聴ありがとうございます