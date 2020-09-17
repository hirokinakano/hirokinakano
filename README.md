# 業務経歴書

## 基本情報

|  key  |  value  |
| ---- | ---- |
|  名前  |  中野大貴 |
| 年齢   | 25歳 (2020/9月時点)  |
| 住み   | 東京都港区  |
| 職業   | Webエンジニア(正社員)  |
| 得意領域   | バックエンド  |

### 自己紹介
エンジニア歴2年となりWeb系アプリケーション、ビジネス系アプリケーションなど様々なシステム開発に携わってきました。
主にRuby、Ruby on Railsを用いたバックエンドのシステム開発を得意としています。
フロントエンドではReact、インフラではAWS、GCPを用いたシステム開発を行うことも可能です。

最近では個人プロジェクトでTypeScript、Goでのバックエンド開発、ReactとFirebaseのサーバーレスのシステム開発なども行なっています。

## スキル

#### サーバーサイド
*Ruby、Ruby on Rails、Python*

#### フロントエンド
*React、TypeScript、JavaScript*

#### インフラ
*Docker、AWS、GCP、Terraform、Ansible、CircleCI、Nginx、Unicorn*

## 業務経歴
業務で担当したプロジェクトの概要を記載しています。
※時系列順での記載となっていませんが、ご了承ください。

---------------------------------------

### 住宅購入補助Webサービス開発

【担当工程】
提案、要件定義、設計、コーディング、テスト

【役割】
バックエンド、フロントエンド、PM

【参画期間】
2019年10月~2020年現在

【使用技術】
Ruby、Ruby on Rails、RSpec、Capistrano、Unicorn、Ansible、Nginx、CircleCI、Docker、Azure

#### プロジェクト概要
大手ハウスメーカーの住宅づくりの契約から住宅の引き渡しまでの煩雑な打ち合わせプロセスをWebシステムで行うことにより、家づくりがスムーズに進むようにお客様をナビゲーションすることを目指して作られたWebシステムになります。従来の紙ベースでやりとりを行う打ち合わせだと下記3つの課題を抱えており、それらの課題解決を目指しました。

**課題**
- 家づくりの道筋・現在地が分かりづらい
- 家づくりに関する情報共有(紙資料)ができない
- 打合せ品質の差が担当者によりばらつく

バックエンドにRuby、Ruby on Rails、フロントエンドにReact.jsを用いた開発を行っていました。
使用した主な技術は下記になります。


#### 実装担当箇所
**Google API連携(Googleサービスアカウント、Google Calendar API、Google Directory API)**

Googleサービスアカウントを用いたGoogle APIの本サービス内での認証基盤、打合せ登録時のGoogle API Calendar APIとの連携、先方が保有している会議室情報を本システムに連携するためのGoogle Directory APIを用いたバッチ処理の実装を担当しました。
当初カレンダーへの予定登録のためGoogleのOAuthを用いた都度の認証を行う方針でしたが、その都度、認証画面を開くとUXが悪くなってしまう問題があったためGoogleサービスアカウントを用いた代理認証を提案しました。

**外部連携サービスからのユーザーデータ取得バッチ**

本サービスはユーザーのデータ情報がすべて先方が保有しているデータウェアハウスに存在するため、それらを取得する日次バッチを作成しました。
連携データの中には、本サービスのデータと整合性が取れないデータも多々存在するため、それらが連携されてきたときの例外処理出力、また、ケアが素早くできるようにSentryで適切な情報を通知できることを考慮し実装しました。
また、連携元のデータは本システムで使用することが想定されていない(正規化がされていない)データ構造であり、日次で数万件のデータが連携されるため、如何にパフォーマンスを考慮しつつ、本システムで扱える形へデータを加工できるかが課題でした。
そのため、Active Recordでのデータ作成を多用するのではなくSQLを直接発行するなど工夫を行いました。

**パフォーマンス悪化箇所のリファクタリング**

本システムは多くのレイヤーを用いていたため、それらを考慮できていない(モデルのプリロードなどで解決できない)、エンドポイントがいくつかありました(主に参照系)。
また、上記が原因で先方よりページ速度が遅いなどの、非機能要件を満たしていない旨の指摘がいくつかあり、改善が必要でした。

原因はレスポンスを整形しているレイヤーでモデルメソッドを直接呼び出しており(当該メソッドはexist?やcountを使用)、その改善が必要でした。
従来の仕様を壊さないよう、リファクタリングを加え、SQL発行数を1/3程度まで抑えることができました。
また、その他重い処理は非同期処理やRedisでのキャッシュを行うなどの対策を行いました。

**その他、全体的なエンドポイントやモデルの作成**

その他にも本システムのCRUDのエンドポイントの作成や、モデルの作成、先方からの仕様改善要望によるモデル改修等を行い、システム全体の機能開発に携わっていました。
また、Webフロントの改修(React.js)にも携わっていました。

**PRレビューでの品質担保**

サーバーサイドではコードレビューを行い、品質担保の役割を担っていました。
サーバーサイドチーム(6人)のメンバーに出していただくPRのレビューチェックを行っていました。
仕様を満たしている、バグがでないなどのシステムの必須条件をチェックするのはもちろんなのですが、そのコードが負債にならないか(見通しが悪いコードになっていないか)、パフォーマンス悪化を招いていないかなどの十分条件の確認、指摘も合わせて行っていました。
また、メンバーへは指摘箇所をどう改善してほしいか、具体的に情報を伝えるようにし、必要ならばコードや参考情報を添付するなどし、できるだけそれを受け取った側が嫌な気持ちにならないようにソフトな表現を心がけるなど、基本的なことですがエンジニアに欠けがちな心がけを常に意識していました。

---------------------------------------

### 棚卸在庫管理システム開発
【担当工程】
設計(API設計、DB設計、インフラ設計)、コーディング、テスト

【役割】
PM、バックエンド、インフラ

【参画期間】
2020年7月~2020年9月

【使用技術】
Ruby、Ruby on Rails、RSpec、Capistrano、Unicorn、Ansible、Nginx、CircleCI、Docker、GCP(Compute Engine、Cloud SQL、Cloud Storage、Cloud Load Balancing)

#### プロジェクト概要

モバイル向けの棚卸在庫管理機能システムの開発になります。主にAPI、管理画面開発を担当しました。
設計の工程から携わり、アプリケーション設計、API設計、DB設計、インフラ設計等のサーバーサイド領域にまたがる設計の担当をしていました。
また、先方との仕様調整、メンバーのタスク管理などのPM業務も担当していました。

#### 実装担当箇所
**全体的な設計**

新規プロジェクトだったため、アプリケーション、API、DB，インフラなどサーバーサイド全般の設計を担当しました。
仕様が複雑なシステムかつ、要件が固まりきっていない状態からのプロジェクトのスタートだったため、いずれもシンプルな構成にするように心がけ、急な仕様変更、複雑なビジネスロジックに耐えられる設計を心がけていました。

**Salesforceとの結合バッチ**

1日に1度、先方のSalesforceからマスタデータを取得、更新するバッチの作成を担当しました。
OAuthを使用し認証を行い、マスタデータの取得を行うバッチとなります。OAuth認証とデータ取得のロジックを分離(クラスの責務を細分化)し、できるだけ再利用をしやすいようなコードを意識していました。

**API作成**

モバイルとの結合用のREST APIの実装をメインで担当していました。モバイル(クライアント)から使用しやすいAPIの作成を心がけており、データロジックはサーバーサイド側で処理を行うことを最大限意識していました。また、クライアント、サーバーサイドともに仕様が確定しきらないことも多かったため、コミュニケーションを綿密にとることを心がけ、双方の認識のズレがでないように意識していました。

---------------------------------------
　
### 眼鏡購入補助アプリのサーバーレス画像加工システム
【担当工程】
設計、コーディング

【役割】
バックエンド、インフラ

【参画期間】
2019/9~2020/10

【使用技術】
Python、pandas、AWS(Lambda、S3)

#### プロジェクト概要
BtoC向けのアプリの画像加工処理をマイクロサービスとしてLambdaで作成、デプロイしたプロジェクトになります。
運用を開始しているiOS向けにアプリのDB内に保存してある画像をスナップショットとして加工、S3内に保存するバッチ処理をLambdaで実装しました。

#### 実装担当箇所
**Lambda内のコード実装からデプロイまで**

1週間に一度、アプリDB内に保存された画像を正方形へ加工、その後、先方環境のS3へ保存を行うという処理を行いました。
コードの実装ではPython、画像加工はPandasを使用しました。

---------------------------------------

### 自社メディア系サービスの管理画面開発
【担当工程】
設計、コーディング

【役割】
バックエンド、フロントエンド

【参画期間】
2019/12~2020/3

【使用技術】
Ruby、Ruby on Rails、RSpec、Slim、Stimulas.js、Flocss

#### 実装担当箇所
**場所登録機能の一貫した実装**

メディア系のサービスで行われるスポットと呼ばれるデータレコードを扱う、機能をモデル、コントローラー、ビューを一貫して担当しました。
カラム量が多くまた、ひとつひとつに厳密した整合性が必要なので、緻密なバリデーション、それをあつかうフォームとも各項目を合わせなければならないなど様々な工夫が必要でした。

**ギャラリー登録の一貫した実装**

ギャラリー登録機能のモデル、コントローラー、ビュー開発を一貫して担当しました。
衣装、髪型といった様々なタイプを扱う必要があり、必然的にテーブル数が多くなりそうだったため、STIによりテーブル数を必要最小限に留める工夫を行いました。

<!--
**hirokinakano/hirokinakano** is a ✨ _special_ ✨ repository because its `README.md` (this file) appears on your GitHub profile.

Here are some ideas to get you started:

- 🔭 I’m currently working on ...
- 🌱 I’m currently learning ...
- 👯 I’m looking to collaborate on ...
- 🤔 I’m looking for help with ...
- 💬 Ask me about ...
- 📫 How to reach me: ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...
-->
