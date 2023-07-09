
- [1. 概要](#1-概要)
  - [1.1. 共通の変数](#11-共通の変数)
  - [1.2. 参考URL](#12-参考url)
- [2. リスト一覧取得：getLists](#2-リスト一覧取得getlists)
  - [2.1. IF定義](#21-if定義)
    - [2.1.1. INパラメータ](#211-inパラメータ)
    - [2.1.2. OUTパラメータ](#212-outパラメータ)
  - [2.2. 処理フロー](#22-処理フロー)
- [3. リスト内のカード一覧取得：getCards](#3-リスト内のカード一覧取得getcards)
  - [3.1. IF定義](#31-if定義)
    - [3.1.1. INパラメータ](#311-inパラメータ)
    - [3.1.2. OUTパラメータ](#312-outパラメータ)
  - [3.2. 処理フロー](#32-処理フロー)
- [4. メンバーID取得：getMemberId](#4-メンバーid取得getmemberid)
  - [4.1. IF定義](#41-if定義)
    - [4.1.1. INパラメータ](#411-inパラメータ)
    - [4.1.2. OUTパラメータ](#412-outパラメータ)
  - [4.2. 処理フロー](#42-処理フロー)
- [5. カード作成：CreateCard](#5-カード作成createcard)
  - [5.1. IF定義](#51-if定義)
    - [5.1.1. INパラメータ](#511-inパラメータ)
    - [5.1.2. OUTパラメータ](#512-outパラメータ)
  - [5.2. 処理フロー](#52-処理フロー)


# 1. 概要
- Trello関連のServiceクラス

## 1.1. 共通の変数
- 下記の変数はPropertyService.ScriptPropertyに保持
- このページで[key名]と出てきたものは下記を示す

| key | 内容 | 保管プロパティ名 |
| :--: | :-- | :-- |
| key | TrelloのAPI Key | TrelloKey |
| token | TrelloのToken | TrelloToken |
| todoBoardId | 滋賀フリーダムTODOリストのボードID | TrelloTodoBoardId |

## 1.2. 参考URL
- [Trelloドキュメント](https://developer.atlassian.com/cloud/trello/rest/api-group-actions/)
- [Google Apps Script からTrello API を使ってTrelloを操作する](https://virment.com/use-trello-api-google-apps-script/)

# 2. リスト一覧取得：getLists
- リスト一覧を取得
- [公式ドキュメント参照](https://developer.atlassian.com/cloud/trello/rest/api-group-lists/#api-lists-id-get)

## 2.1. IF定義
### 2.1.1. INパラメータ
- なし

### 2.1.2. OUTパラメータ
- 下記をListにして返却

| Key | Value |
| :--: | :-- |
| id | リストのID |
| name | リストの名前 |

## 2.2. 処理フロー
1. 下記URLをGetで叩く
  - https://api.trello.com/1/boards/[todoBoardId]/lists?key=[key]&token=[token]&fields=name
2. 返却値をそのまま返す

# 3. リスト内のカード一覧取得：getCards
- リスト内のカード一覧を取得
- [公式ドキュメント参照](https://developer.atlassian.com/cloud/trello/rest/api-group-cards/#api-cards-id-get)


## 3.1. IF定義
### 3.1.1. INパラメータ

| Key | Value |
| :--: | :-- |
| name | リスト名 |

### 3.1.2. OUTパラメータ
- Trello APIで取得した値をそのまま返却
  - [参照](https://developer.atlassian.com/cloud/trello/rest/api-group-actions/#api-actions-id-card-get-response)

## 3.2. 処理フロー
1. [TrelloService.getLists()]()からリスト一覧を取得し、name = INパラメータ.nameと一致するデータからidを取得
2. 下記URLをGetで叩く
  - https://api.trello.com/1/lists/[1.のid]/cards?key=[key]&token=[token]
3. 返却値をそのまま返す

# 4. メンバーID取得：getMemberId
- メンバーIDをを取得
- [公式ドキュメント参照](https://developer.atlassian.com/cloud/trello/rest/api-group-members/#api-members-id-get)

## 4.1. IF定義
### 4.1.1. INパラメータ

| Key | Value | その他 |
| :--: | :-- | :-- |
| memberId | メンバーID | フリーダムのメンバーID |

### 4.1.2. OUTパラメータ

| Key | Value |
| :--: | :-- |
| id | メンバーID |

## 4.2. 処理フロー
1. MemberService.getMember()からtrelloMemberNameを取得
2. 下記URLをGetで叩く
  - https://api.trello.com/1/members/[1.のtrelloMemberName]?key=[key]&token=[token]
3. 取得したデータからidを返却する

# 5. カード作成：CreateCard
- カードを作成する
- [公式ドキュメント参照](https://developer.atlassian.com/cloud/trello/rest/api-group-cards/#api-cards-post)

## 5.1. IF定義
### 5.1.1. INパラメータ

| Key | Value | 型 |その他 |
| :--: | :-- | :-- | :-- |
| name | カードタイトル | string | |
| description | 概要 | string | null許容 |
| listName | リスト名 | string | null許容 |
| due | 期限 | string or date | null許容 |
| memberID | メンバーID | string | フリーダムのメンバーID |

### 5.1.2. OUTパラメータ
- Trello APIで取得した値をそのまま返却

## 5.2. 処理フロー
1. [TrelloService.getLists()]()からリスト一覧を取得し、name = INパラメータ.listNameと一致するデータからidを取得
  - INパラメータ.listNameがnullの場合は'未設定'と一致するデータを取得
2. INパラメータ.memberIDがnull以外の場合、TrelloService.getMemberId()からidを取得
3. 下記URLをPOSTで叩く
  - https://api.trello.com/1/cards
  - 以下パラメータ
| key | value |
| :--: | :-- |
| idList | 1.のid(必須) |
| name | INパラメータ.name |
| desc | INパラメータ.description |
| due | INパラメータ.due |
| idMembers | 2.のid |
4. 返却値をそのまま返す