# CookNote
## 概要
Cookpadの簡易版クローンアプリを作る

## 動機と目的
- 比較的簡単なCRUDアプリを作り、Railsアプリケーションの理解を深める。
- 趣味が料理だが、レシピのメモを作りあぐねているので、試しに作ってみたい。

## 実装したい機能
- レシピの登録などCRUD機能
  - レシピの説明
  - 完成写真の登録
  - 材料と分量の登録
  - 調理工程の登録、写真の登録
    - 工程に写真を登録する。
    - 本家Cookpadのように、JSで調理工程の順番を変えるのは難しいかもしれない
  - コツ、生い立ち
- レシピのタグ付（お気に入り）
- レシピのストック（ジャンルを登録）
- 自分とフォローしているユーザーのレシピを一覧で表示
- SNSへのシェア機能
- ユーザー関連の機能
  - 基本的なログイン機能
  - ユーザーのフォロー機能
- 通知機能
  - お気に入り登録、フォローなど

## 設計する
### ER図
[![Image from Gyazo](https://i.gyazo.com/58c9ebaad3606c4f1c89cc76542fc2f2.png)](https://gyazo.com/58c9ebaad3606c4f1c89cc76542fc2f2)
