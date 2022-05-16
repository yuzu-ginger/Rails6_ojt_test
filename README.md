# OJT test
dmm webcamp OJT卒業テスト
## debugした項目
### 0. 準備
- ホストを許可する
- `rails db:migrate` を実行
- `bundle exec rails webpacker:install` を実行
<br><br>
### 1-1. ログイン・サインアップができない問題(ページが動作していない)
routes.rbの記述の順番<br>
`devise_for` は `resources` よりも上の行に書く
<br><br>
### 1-2. ログイン・サインアップができない問題(サインアップできない&名前ログイン)
- app/models/user.rb のバリデーションの設定
`validates :introduction, presence: true, length: { maximum: 50 }`のうち`presence: true`はいらない。イントロダクションが必須になってしまう
- 名前ログイン
config/initializers/devise.rb の`config.authentication_keys = [:email]` を `[:name]` にする<br>
application_controller.rb のkeysを `keys: [:email]` にする
<br><br>
### 2. ユーザの画像が表示されない(ファイルがない)
app/models/user.rb のもしユーザの画像がなかったらの記述で、画像のファイル名が間違っている。正しくは `no_image.jpg`
<br><br>
### 3-1. ユーザ詳細ページから投稿しようとするとルーティングエラーになる
usersのshowページで投稿フォームの部分テンプレートの部分の記述が間違っている<br>
コントローラでは新規投稿は `@book` になっているのに、viewぺーじでは `@new_book` になっていた
<br><br>
### 3-2. 新規投稿しても反映されない
books_controller.rb で、createアクションに `@book.user_id = current_user.id` の記述がない
<br><br>
### 7. 本の一覧から本の詳細ページに遷移できない
booksのviewページの_index部分テンプレートで、本のタイトルのパスが `books_path(book.id)` になっていた。 `book` に直す
