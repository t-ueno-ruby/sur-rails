
issue  
> [AR does not infer primary key in association for composite primary keys. · Issue #49622 · rails/rails](https://github.com/rails/rails/issues/49622)

PR  
> [Raise on \`foreign\_key:\` being passed as an array in associations by nvasilevski · Pull Request #49625 · rails/rails](https://github.com/rails/rails/pull/49625)

## PRの概要(翻訳)

<details>
<summary>(見出し)Raise on foreign_key: being passed as an array in associations</summary>

<details>
<summary>Google Translate</summary>

> foreign_key で発生する: 関連付けで配列として渡される

</details>

<details>
<summary>自力</summary>

> 外部キーでRaiseします(例外が発生します): アソシエーションで配列として通過します
</details>

<!-- >
> infer: 推論する
> composite: 複合 -->
</details>
<br>
<details>
<summary>(詳細)Associations have never allowed nor supported foreign_key option being passed as an Array. This still holds true for Rails 7.1 However with Rails 7.1 supporting composite primary keys it may become more common for applications to mistakenly pass an array to foreign_key:. This commit adds an exception to raise when foreign_key: is passed as an array.</summary>
<br>

<details>
<summary>Google Translate</summary>

> アソシエーションではこれまで、foreign_keyオプションに配列を渡すことは認められていませんでしたし、サポートされていませんでした。  
これはRails 7.1でも変わりませんが、Rails 7.1では複合主キーがサポートされたため、アプリケーションが誤ってforeign_key:に配列を渡すことが一般化するかもしれません。  
今回のコミットでは、foreign_key:に配列が渡されたときに発生する例外を追加しました。  

</details>

<details>
<summary>自力</summary>

> 関連づけは 配列として通過させるオプションもサポートしていません。  
これはまだRails7.1では true のままですが、Rails7.1は複合主キーをサポートしており、間違って配列を外部キーに追加させるようになってしまっているかもしれません。  
このコミットはforeign_key: が配列として通過した時にraiseするために例外を追加します

</details>

<!-- >
> nor: また
> composite: 複合
> 
-->
</details>

## コード分析

ref: [Raise on \`foreign\_key:\` being passed as an array in associations by nvasilevski · Pull Request #49625 · rails/rails](https://github.com/rails/rails/pull/49625/commits/529d1f55a8138798d92a6f2aba01681315958ab3)

### 変更ファイル一覧

4ファイル

1. activerecord/CHANGELOG.md
    - 変更ログ
1. activerecord/lib/active_record/reflection.rb
    - 修正箇所
1. activerecord/test/cases/associations_test.rb
1. activerecord/test/cases/reflection_test.rb
    - 修正箇所のテスト

#### activerecord/CHANGELOG.md

PRの見出しとusernameを追加している。  

#### activerecord/lib/active_record/reflection.rb

PRの変更箇所
[rails/activerecord/lib/active\_record/reflection.rb at e1d58cfd05ae1cc0bfc1006b7ce973a7730831df · rails/rails](https://github.com/rails/rails/blob/e1d58cfd05ae1cc0bfc1006b7ce973a7730831df/activerecord/lib/active_record/reflection.rb)

#### activerecord/test/cases/associations_test.rb

アソシエーションのテストのコード
[rails/activerecord/test/cases/associations\_test.rb at 9f8a3db328db9386e47a84138c49c410d36b7e78 · rails/rails](https://github.com/rails/rails/blob/9f8a3db328db9386e47a84138c49c410d36b7e78/activerecord/test/cases/associations_test.rb)

#### activerecord/test/cases/reflection_test.rb

PRの変更箇所のテストコード

### ググる

[active record reflectionsとは - Google 検索](https://www.google.com/search?q=active+record+reflections%E3%81%A8%E3%81%AF&oq=active+record+reflections%E3%81%A8%E3%81%AF&gs_lcrp=EgZjaHJvbWUyBggAEEUYOTIKCAEQABiABBiiBDIKCAIQABiABBiiBDIKCAMQABiABBiiBDIKCAQQABiABBiiBNIBCTExOTI5ajBqN6gCALACAA&sourceid=chrome&ie=UTF-8)

#### ActiveRecord::Reflection::ClassMethods

<details>
<summary>Reflection enables the ability to examine the associations and aggregations of Active Record classes and objects. This information, for example, can be used in a form builder that takes an Active Record object and creates input fields for all of the attributes depending on their type and displays the associations to other objects</summary>

<details>
<summary>Google Translate</summary>

> Reflection を使用すると、Active Record のクラスやオブジェクトの関連や集約を調べることができます。この情報は、例えば、Active Record オブジェクトを受け取り、そのタイプに応じてすべての属性の入力フィールドを作成し、他のオブジェクトへの関連付けを表示するフォームビルダーで使用することができます。

</details>

参考リンク

> [ActiveRecord::Reflection::ClassMethods](https://api.rubyonrails.org/classes/ActiveRecord/Reflection/ClassMethods.html)
>> [ActiveRecord::Reflection で Model 間の関連付け情報を取得する #Ruby - Qiita](https://qiita.com/foolproof-htb/items/c715d0121e4b5ffc481d)
</details>

###### reflectionとは

直訳すると反射。

##### テストコード

1. `class MacroReflection`内で定義されている
    1. `AbstractReflection`(同ファイル内)を継承している

#### `Sharded::BlogPost`

テストを見ていると`Sharded::BlogPost`という内容が出てくる。  
GitHubで検索  
> [Code search results](https://github.com/search?q=repo%3Arails%2Frails%20Sharded%3A%3ABlogPost&type=code)

テストで使うためのテストデータっぽい。
命名から考えると

- ブログの投稿された記事(Post)のモデル
    - ブログアプリ

ぽい。

- belongs_to(オプションなし)
- belongs_to(polymorphic)
- has_many(オプションなし)
- has_many(class_name, dependent)
- has_many(as)
- has_many(through)

の関連づけが定義されている。

## 要件定義

- foreign_key が 配列の時にraiseするようにする
    - raiseするErrorは ArgumentError
- メッセージを今までの内容と変更する
    - foreign_key が 配列の時

        - > Passing #{options[:foreign_key]} array to :foreign_key option
            on the #{active_record}##{name} association is not supported.
            Use the query_constraints: #{options[:foreign_key]} option instead to represent a composite foreign key.
    - それ以外の時
        - 現在のまま
- テストを追加する
    - activerecord/test/cases/associations_test.rb
        - test_has_many_with_foreign_key_as_an_array_raises
            - 既存の動作？
        - test_belongs_to_with_foreign_key_as_an_array_raises
            - 新しいraiseのテスト
    - activerecord/test/cases/reflection_test.rb
        - test_has_many_reflection_with_array_fk_raises
        - has_one_reflection_with_array_fk_raises
        - test_belongs_to_reflection_with_array_fk_raises

## 余談

- `validate_reflection!`はmainブランチからなくなっている？
- これは使えそう?
    - [DeepL for Visual Studio Codeを使用してみた #VSCode - Qiita](https://qiita.com/makoto-ogata@github/items/8d6e8f2474f9cc0988c4)

## 学んだこと

```
# git操作のコマンド
- git fetch upstream
- git checkout main
- git merge upstream/main
# 条件
作業中のブランチでコミットされていないファイルがあったらエラーメッセージを出力してコマンドを終了する
コマンドを終了するときは適切なエラーメッセージを出力する
```

- Railsのテストはminitestを使用
- 追加したメソッドはprivateで定義(?)
- squishはrailsのメソッド
    - > [squish (String) - APIdock](https://apidock.com/rails/String/squish)
    - [RailsでSQLのログを改行無しで出力する「squish」メソッド - その辺にいるWebエンジニアの備忘録](https://kossy-web-engineer.hatenablog.com/entry/2021/05/09/162942)
        - [ruby squish - Google 検索](https://www.google.com/search?q=ruby+squish&oq=ruby+squish&gs_lcrp=EgZjaHJvbWUyBggAEEUYOTIGCAEQABgeMgYIAhAAGB4yBggDEAAYHjIGCAQQABgeMgYIBRAAGB4yCAgGEAAYCBgeMggIBxAAGAgYHjIICAgQABgIGB4yCAgJEAAYCBge0gEIMjc1M2owajeoAgCwAgA&sourceid=chrome&ie=UTF-8)
- forkしたリポジトリをfork元のリポジトリの最新コミットと同期できる
    - [github forkしたリポジトリ 最新コミット - Google 検索](https://www.google.com/search?q=github+fork%E3%81%97%E3%81%9F%E3%83%AA%E3%83%9D%E3%82%B8%E3%83%88%E3%83%AA+%E6%9C%80%E6%96%B0%E3%82%B3%E3%83%9F%E3%83%83%E3%83%88&oq=github+fork%E3%81%97%E3%81%9F%E3%83%AA%E3%83%9D%E3%82%B8%E3%83%88%E3%83%AA+%E6%9C%80%E6%96%B0%E3%82%B3%E3%83%9F%E3%83%83%E3%83%88&gs_lcrp=EgZjaHJvbWUyBggAEEUYOTIKCAEQABiABBiiBDIKCAIQABiABBiiBDIKCAMQABiABBiiBDIKCAQQABiABBiiBDIKCAUQABiABBiiBDIGCAYQRRhA0gEJMjAxODRqMGo3qAIAsAIA&sourceid=chrome&ie=UTF-8)
    - 同期する方法は2つ
        - gitコマンドで同期
            - [フォークしたリポジトリを最新化する方法 #Git - Qiita](https://qiita.com/Nossa/items/ace2ab802adc85f86b20)
        - GitHub.comで同期
            - [GitHub で Fork を最新に同期する](https://zenn.dev/mtmatma/articles/4f4238257af1fe)

## 気になったこと(わからないこと？)

##### CHANGELOGに該当のコードがない

mainブランチにmergeされたにも関わらず以下のリンクでusernameで検索してもヒットしない。なぜ？
> [rails/activerecord/CHANGELOG.md at main · rails/rails](https://github.com/rails/rails/blob/main/activerecord/CHANGELOG.md)
>
##### `# :nodoc:`は何？

該当コード
[rails/activerecord/lib/active\_record/reflection.rb at dcb1d1f4c4987c3352f8c5f94bb44393b0f18252 · rails/rails](https://github.com/rails/rails/blob/dcb1d1f4c4987c3352f8c5f94bb44393b0f18252/activerecord/lib/active_record/reflection.rb#L7)

Rubyに標準的に備わっているドキュメント生成のライブラリ(rdoc)のオプション。  
指定した要素をドキュメントに含めないための記述
> これ？
>> [RubyのYARDにはRDocのような:nodoc:は存在しない - コード日進月歩](https://shinkufencer.hateblo.jp/entry/2022/06/11/000000)

##### `# = Active Record Reflection`は何？

該当コード
[rails/activerecord/lib/active\_record/reflection.rb at dcb1d1f4c4987c3352f8c5f94bb44393b0f18252 · rails/rails](https://github.com/rails/rails/blob/dcb1d1f4c4987c3352f8c5f94bb44393b0f18252/activerecord/lib/active_record/reflection.rb#L6)

## 困ったこと

- minitestの書き方を忘れている
- squishメソッド、ヒアドキュメントの使い方がわからなかった
- optionsがどのように渡されるかわからなかった
- `../test/cases/..`の`cases`ってなに？
