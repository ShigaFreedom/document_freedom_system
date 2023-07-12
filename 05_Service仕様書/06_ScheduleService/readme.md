
- [1. 概要](#1-概要)
- [2. スケジュール情報取得:getDatas](#2-スケジュール情報取得getdatas)
  - [2.1. 使用するテーブル](#21-使用するテーブル)
  - [2.2. IF定義](#22-if定義)
    - [2.2.1. INパラメータ](#221-inパラメータ)
    - [2.2.2. OUTパラメータ](#222-outパラメータ)
  - [2.3. 処理フロー](#23-処理フロー)

# 1. 概要
- schedule関連のservice

# 2. スケジュール情報取得:getDatas
- スケジュール情報を取得する

## 2.1. 使用するテーブル
- メイン：schedules

## 2.2. IF定義
### 2.2.1. INパラメータ

| Key | Value | 型 | その他 |
| :--: | :-- | :-- | :-- |
| activeDate | 活動日 | date or string | null許容 |
| categories | カテゴリー | array\<string> | null許容 |

### 2.2.2. OUTパラメータ
- 全カラムを返却
    - 0件時はnullを返す

## 2.3. 処理フロー
1. 下記条件で絞る(AND条件、各INパラメータが存在している時のみそれぞれ絞る)
    - [schedules].[active_date] = INパラメータ.activeDate
    - [schedules].[active_category] in INパラメータ.categories
2. 取得したデータを全て返却する
    - 0件時はnullを返す