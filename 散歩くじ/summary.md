# さんぽくじ（仮称）
## アイデア
- ユーザーが入力した出発地、行先のキーワードを元に、さんぽ旅（行先と目的）を提案してくれるアプリ

## 考案の背景
- 私たちが普段行く範囲は限られていて、新しい場所にいく機会はあまり多くないと思います。身近な場所に観光スポットがあっても気づきづらいことが多く、もったいないなと思っていました。
- そういった意味では、新しい旅先をガチャ形式で提案してくれるPeach航空の[旅くじ](https://www.flypeach.com/campaign/shakelabo/tabikuji/)がすごく良いアイデアだなと感じています。
- ただ、飛行機を使う旅行は日程が長くなりがちで、もっと気軽にいける（数時間〜半日程度で行ける）さんぽ旅をくじ引きの形で提案できるサービスがあれば良いなと考えました。
- Railsを触れる中でCRUDアプリを作る機会はあるのですが、APIを使う機会は少なかったので、外部APIを使ってアプリを作りたいと考えたことも背景の一つです。

## ターゲット
- 散歩などプチお出かけが好きなユーザー
  - いわゆる旅好きなユーザーは近場のお出かけはあまり好まなさそうな印象があります。
- SNSでシェアできるようにしたいので、SNSを利用するユーザー

### 類似のサービス
- [旅くじ](https://www.flypeach.com/campaign/shakelabo/tabikuji/)
- その他で類似サービスは見当たらず、単純なルーレット系のアプリがいくつか存在する。
  - [日本全国 47都道府県のルーレット](https://hajityoro.com/roulette)
  - [旅行先ランダムメーカー](https://kononedan.com/ryokoRandom/)
  - [おひとり様向け。温泉旅行行き先ルーレット](https://shindanmaker.com/1091866)
- Androidアプリの[shiorin ～感動の旅をあなたにお届け～](https://play.google.com/store/apps/details?id=com.joron.waffle.itineraryapp)がイメージに近いかと思う。
  - こちらは、都道府県ごとに行先のタイプを（日本の百名選など）選ぶことができ、さんぽくじとは違うアプローチだが素晴らしいと思った。
  - 「徒歩に特化」と「任意のエリアを細かく指定できる」という点で差別化していきたいと思う。

## 機能の概要（箇条書き）
### 方向性
- アプリのコンテンツがルーレット系であるため、アプリを診断系ツールと位置づけて、アプリ内での履歴閲覧や他ユーザーとの共有機能は最小限（または無し）で良いのではないかと思っています。
### 未ログインでできること
- 行先のタイプを選択し、出発地を入力・送信すると、以下のくじが作成される。
  - 行先
    - GoogleのPlaceAPIを使い、指定したエリア内(例えば半径3km以内など)でキーワードに合致するスポットをランダムで提示 
  - アクション（行先でチャレンジすること）
    - 予め用意したアクションからランダムで選択されるようにする。自撮りする、おすすめグルメを食べる、など。 
  - 作成したくじはTwitterなどSNSでシェアできる。
- 行先の近場のスポットを併せて提示し、寄り道を勧める。

### ログイン後にできること
- 作成したくじをユーザーに紐付けて保存できる。
- 考案したアクションを作成（提案）できる。
  - 開発元の審査が必要という仕様にした方が良さそう。

### その他
#### 管理ユーザーの機能について
- リソースの管理権限（ユーザー、くじ、アクション）
  - ただし、診断系ツールであるため、実際の運用でCRUD機能を使うことはほとんどなさそう。
- ユーザーから提案されたアクションの内容を確認し、公開できる。
#### 通知機能
- アクションが作成・提案されたとき、管理ユーザーに承認依頼の通知をする。
- アクションが承認されたとき、作成したユーザーに承認完了の通知をする。

#### リリース後などに追加したい機能
- Twitterを使ったログイン
- くじを作成し、結果（詳細画面）が表示されるまでに待ち時間を設けると、（僅かですが）結果を待つワクワク感を期待できるのでは。
  - ベストはガラポンのアニメーションだが、ちょっと難しそう。
  - JSを使った簡単なローディングページをはさみ、３秒程度時間を作ってもう良いかなと思いました。
  - ただ、本番環境にどれだけ課金するか未定ですが、そもそもの読み込み時間が長いと逆効果になりそう。
- くじの作成画面にて、現在地の取得もできたら良い。
- 検索範囲をユーザーが指定できると良いかもしれない。

### 使いそうなAPI
#### 出発地の取得と表示
- Google Maps Javascript API
#### 行先と経路の表示
- Google Place API
- Google Direction API

### 懸念点
- ユーザーが出発前に投稿した場合、それ以降の行動予定をSNSで周知してしまうため、何らかの配慮が必要な気がする。
  - 行先とミッションだけをシェアできるようにし、出発地はシェアしない、など。
- ログイン済ユーザーはアクションを作成できるようにしたいが、開発元の審査が必要な仕様とする。
  - 不適切なアクションを許可しないため。

### アイデア出しについてコメント
- 当初は「公共交通で○○円以下で行けるお出かけ」を提案できるサービスを想定していました。ただ、そういったサービスを提供しているAPIは無く、また実装も厳しそうな印象があったので、上記の内容となりました。
- 上記だけだとサービスとしての面白みに欠ける気もするので、ミッションを尖らせる、またはジャンルを絞ると面白くなりそうです。
  - 少し勇気を必要とするミッション（行先で人と交流する、など）
  - ちょっとオカルトっぽいミッション、など。

### Figma
https://www.figma.com/file/diej53AtUK8CtG908Lva8l/%E3%81%95%E3%82%93%E3%81%BD%E3%81%8F%E3%81%98%EF%BC%88%E4%BB%AE%E7%A7%B0%EF%BC%89?node-id=0%3A1

### ER図の再作成（Mermaid記法）
#### レビューメモ
- activityは複数のLocation_typeを持つため、中間テーブルを設ける
  - 例えば、写真を撮るというactivityは様々なLocation_typeの場所でできるため。
- Lotに設定するLocation_type（destination_type)は一つだけなので、中間テーブルは設けずにbelongs_toで紐付けるだけとする。
```mermaid
erDiagram


user ||--o{ activity: ""
user ||--o{ lot: ""
user ||--o{ notification: ""
activity ||--|{ activity_location_type: ""
activity ||--o{ lot_activity: ""
activity ||--|{ notification: ""
location_type ||--o{ activity_location_type: ""
lot ||--|| location_type: ""
lot ||--|| lot_activity: ""

user {
    name string
    email string
    encrypted_password string
    admin boolean
    created_at datetime
    updated_at datetime
}

lot {
    user_id integer
    start_point_name string
    start_point_address string
    start_point_latitude float
    start_point_longitude float
    destination_type_id integer
    destination_name string
    destination_address string
    destination_latitude float
    destination_longitude float
    neaby_locations json
    created_at datetime
    updated_at datetime
}

activity {
  location_type_id integer
  user_id integer
  content text
  released boolean
  created_at datetime
  updated_at datetime
}

lot_activity {
  lot_id integer
  activity_id integer
  created_at datetime
  updated_at datetime
}

location_type {
  key integer
  name string
  created_at datatime
  updated_at datatime
}

activity_location_type {
  activity_id integer
  location_type_id integer
  created_at datatime
  updated_at datatime
}

notification {
  notification_type integer
  user_id integer
  read boolean
  created_at datetime
  updated_at datetime
}
```
### エンドポイントとコントローラー
| やりたいこと | HTTPメソッド | エンドポイント | コントローラ#アクション | 
|:-----------|:------------:|:------------|:------------|
| ログイン画面を表示 | GET | /login | user_sessions#new |
| ログイン | POST | /login | user_sessions#create |
| ログアウト | DELETE | /logout | user_sessions#destroy |
| くじ作成画面を表示 | GET | /lots/new | lots#new |
| くじを作成 | POST | /lots | lots#create |
| くじの詳細を表示 | GET | /lots/:id | lots#show |
| ユーザー登録画面を表示 | GET | /signup |	userss#new |
| ユーザー登録 | POST | /signup | users#create |
| ユーザー情報の削除 | DELETE | /users/:id | users#destroy |
| アクション作成画面を表示 | GET | /activities/new | activities#new |
| アクションを作成 | POST | /activities | activities#create |
| マイページを表示 | GET | /mypage/account | /mypage/account#show |
| マイページの編集画面を表示 | GET | /mypage/account/edit | mypage/account#edit |
| マイページを更新 | PATCH/PUT | /mypage/account | mypage/account#update |
| アクションの詳細を表示 | GET | /mypage/activities/:id | mypage/activities#show |
| アクションの編集画面を表示 | GET | /mypage/activities/:id | mypage/activities#edit |
| アクションを更新 | GET | /mypage/activities/:id | mypage/activities#update |
| アクションを削除 | DELETE | /mypage/activities/:id | mypage/activities#destroy |
| 通知の一覧を表示 | GET | /mypage/notifications | mypage/notifications#index |
| 通知の詳細を表示 | GET | /mypage/notifications/:id | mypage/notifications#show |
| 通知を既読にする | GET | /mypage/notifications/:id/read| mypage/notifications#read |
| （管理画面）ユーザー一覧を表示 | GET | /admin/users | admin/users#index |
| （管理画面）ユーザー情報の詳細を表示 | GET | /admin/users/:id | admin/users#show |
| （管理画面）ユーザー情報の編集ページを表示 | GET | /admin/users/:id/edit | admin/users#edit |
| （管理画面）ユーザー情報を更新 | PATCH/PUT | /admin/users/:id | admin/users#update |
| （管理画面）ユーザー情報を削除 | DELETE | /admin/users/:id | admin/users#destroy |
| （管理画面）くじ一覧を表示 | GET | /admin/lots | admin/lots#index |
| （管理画面）くじ詳細を表示 | GET | /admin/lots/:id | admin/lots#show |
| （管理画面）くじを削除 | DELETE | /admin/lots/:id | admin/lots#destroy |
| （管理画面）アクション一覧を表示 | GET | /admin/activities | admin/activities#index |
| （管理画面）アクション詳細を表示 | GET | /admin/activities/:id | admin/activities#show |
| （管理画面）アクションの編集ページを表示 | GET | /admin/activities/:id/edit | admin/activities#edit |
| （管理画面）アクションを更新 | PATCH/PUT | /admin/activities/:id | admin/activities#update |
| （管理画面）アクションを削除 | DELETE | /admin/activities/:id | admin/activities#destroy |
| 使い方 | GET | /about | static_pages#about |
| さんぽのコツ・ヒントを紹介 | GET | /tips_to_enjoy | static_pages#tips |
| 利用規約 | GET | /rules | static_pages#rules |
| プライバシーポリシー | GET | /privacy | static_pages#privacy |

### 決めていないこと・ご相談したいこと
- 寄り道スポットの情報の管理について
  - lotsのカラム`neaby_location_infos`に持たせるのか、従属するモデルを作成して情報を保存するか。

### その他メモ
- `lot`が作成されたときに、行先でのアクションを保存する`lot_activity`をコールバックで作成する。
- `user`の削除については、通常の削除にしようと思います（論理削除ではなく）
  - `user`と`activity`の関係は`dependent: :destroy`の予定ですが、activityの候補が消えても影響は軽微と思われるため。

### ポートフォリオ案レビュー会で頂いた意見について整理
- スパルタコースの皆さんに色々とご意見をいただいた。
- レビュー会の[メモ](https://hackmd.io/KENAQAtOSTub95NXzirMmg?view)はこちら

### アイデア全般について
#### 目的地を指定するのもアイデアとして良いのでは
- 目的地をユーザーに入力させて、その目的地近辺の寄り道スポットを提示するサービスも良いのでは。
- 出発地を自宅近辺に設定する場合、プライバシーの観点からあまりシェアしたくなさそう（特に女性ユーザー）
  - 利用範囲を近所に絞らず、どこでも使えることをアピールするか。
  - シェアする場合の注意喚起は必要そう（自宅周辺で使うと生活圏内がわかってしまう）
  - もしくは、検索する範囲を指定してもらうか。
- ユーザーがアクションにチャレンジする動機はどうしても弱くなりそう。
  - 一応「くじ」を謳うサービスなので、アクションの難易度を大吉〜大凶で設定すると面白いかもしれない。(初期リリース後に検討か)
  - アクションの数を増やす。
  - ジャンルを絞る
  - 複数名での散歩を念頭に置いたアクションを作る、など(人数の情報をもたせるなら、カラムを増やして管理する必要ありそう)
- ぼんやりと、アプリ名の「さんぽ」は平仮名のほうが親しみやすいかなと思いましたが、アプリ内の表示に統一性がなくなるので、漢字の散歩に統一しても良いのかなと思います。

#### 実装についてのご意見の整理
##### User・認証関連
- 未ログインでくじを作成する場合は、Userの匿名認証は使わずにuser_idをnullで作成しても良いのでは。
- Twitterログインはハードル高くないので、ぜひともチャレンジしてみては。
- Adminユーザーはtodakaしか使わないはずなので、Basic認証やgemを使って簡単に実装する。
##### Lot関連
- 寄り道スポットについて、外部APIから取得したJSON形式のデータはjson型でそのまま保存するのが良いかも。あとから情報を取得する際に便利。
- 行先のタイプはGoogleのPlaceAPIの場所のタイプに依存するが、日本語の名前などを表記する場合は、別にテーブルをもたせたほうが良さそう。
  - 行先のタイプは、原則はどこにでもあるものを提示する。
  - 行先を多く表示する場合、グルーピングが必要そう。
- SNSシェアについて、LINENとInstagramも実装したい。
- いいね機能があっても良いかも。
  - ツールとして簡素な機能に絞るなら見送り、SNS上のみやすさ・シェアのしやすさに注力するほうが良いかもしれない。
- くじの作成画面にて、緯度経度の欄は不要かも。
##### activity関連
- アクションについて、公開状況のフラグとは別に、承認状況のフラグを設ける必要がある。また、差し戻す場合はコメントもつけることができると更に良し。

##### その他全般的なこと
- 型を保存するカラム`_type`はenumで保存しても良いかもしれない。
- 最低限の機能で、とりあえず動くものをリリースしたほうが良さそう。
  - activity周りの実装の優先度を下げると、通知や管理画面の実装が不要となるため、開発期間は短くなりそう。
- 自分でデザインを考えるとダサくなりがちなので、有料テーマや諸々のツールを活用すると良い。
