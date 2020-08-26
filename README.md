# songle-sample

vueでSongle APIを触ってみた記録

## 準備
`npm install jquery`

なんとjQueryを使う．Vueを使い始めてから触る日が来ると思ってなかった．

Songle APIは，`<script>`タグから読み込む．（ここのscriptタグを動的に生成する部分でjQueryを使う）



### やったこと
- 楽曲を指定秒数から再生開始
- 100sを超えたら次の曲に自動で遷移
- 別プロジェクトで使用するときはAPIからプレイリストデータをとってきてそれを使う
