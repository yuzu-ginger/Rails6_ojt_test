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
`devise_for` は `resources` よりも上の行に書く<br>
routes.rbの`devise_for :users`を`resources`の上に移動する。<br>
`resources :users, only: :show, :index, :edit, :update]`が先に読み込まれ、<br>
`GET '/users/:id' => 'users#show'`になる。<br>
deviseで`GET '/users/sign_in'`で、id = sign_inという判定になる。<br>
usersコントローラでログイン制限がかかっているので<br>
`/users/(idが)sign_in` => `users#show` => ログインしていないので `/users/sign_in`にリダイレクト => `users#show` => ...<br>
という無限ループになる。<br>
devise_forを上に記述することで<br>
`GET '/users/sign_in'`=> `devise/sessions#new`になる
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
### 3. 新規投稿しても反映されない
books_controller.rb で、createアクションに `@book.user_id = current_user.id` の記述がない
<br><br>
### 4. 本の編集画面でエラーが出る
books_controllerにeditアクションの中身がない。 `@book = Book.find(params[:id])` を追記
<br><br>
### 5. 本の編集画面でエラーメッセージが出ない
books_controllerのupdateアクションで、失敗したときに `redirect_to` になっている<br>
`render 'edit'` にする
<br><br>
### 6. 本の削除ができない
books_controllerのdestroyアクションの綴りミス
<br><br>
### 7. 本の一覧から本の詳細ページに遷移できない
booksのviewページの_index部分テンプレートで、本のタイトルのパスが `books_path(book.id)` になっていた。 `book` に直す
<br><br>
### 8. ユーザ詳細ページから投稿しようとするとルーティングエラーになる
usersのshowページで投稿フォームの部分テンプレートの部分の記述が間違っている<br>
コントローラでは新規投稿は `@book` になっているのに、viewぺーじでは `@new_book` になっていた
<br><br>