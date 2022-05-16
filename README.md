# OJT test
dmm webcamp OJT卒業テスト
## debugした項目
### 1. 準備
- ホストを許可する
- `rails db:migrate` を実行
- `bundle exec rails webpacker:install` を実行
<br><br>
### 2. ログイン・サインアップができない問題(ページが動作していない)
routes.rbの記述の順番<br>
`devise_for` は `resources` よりも上の行に書く
<br><br>
### 3. ログイン・サインアップができない問題(サインアップできない&名前ログイン)
- app/models/user.rb のバリデーションの設定
`validates :introduction, presence: true, length: { maximum: 50 }`のうち`presence: true`はいらない。イントロダクションが必須になってしまう
- 名前ログイン
config/initializers/devise.rb の`config.authentication_keys = [:email]` を `[:name]` にする<br>
application_controller.rb のkeysを `keys: [:email]` にする
<br><br>
### 4. ユーザの画像が表示されない(ファイルがない)
app/models/user.rb のもしユーザの画像がなかったらの記述で、画像のファイル名が間違っている。正しくは `no_image.jpg`
<br><br>