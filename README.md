# OJT test
dmm webcamp OJT卒業テスト
## debugした項目
### 1. 準備
- ホストを許可する
- `rails db:migrate` を実行
- `bundle exec rails webpacker:install` を実行
<br><br>
### 2. ログイン・サインアップができない問題
routes.rbの記述の順番<br>
`devise_for` は `resources` よりも上の行に書く
<br><br>
### 3.