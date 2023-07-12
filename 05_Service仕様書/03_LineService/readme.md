

- [1. 概要](#1-概要)
- [2. LineToken取得：getLineToken](#2-linetoken取得getlinetoken)
  - [2.1. 使用するテーブル](#21-使用するテーブル)
  - [2.2. IF定義](#22-if定義)
    - [2.2.1. INパラメータ](#221-inパラメータ)
    - [2.2.2. OUTパラメータ](#222-outパラメータ)
  - [2.3. 処理フロー](#23-処理フロー)
- [3. LINEメッセージ送信処理：sendMessage](#3-lineメッセージ送信処理sendmessage)
  - [3.1. 使用するテーブル](#31-使用するテーブル)
  - [3.2. IF定義](#32-if定義)
    - [3.2.1. INパラメータ](#321-inパラメータ)
  - [3.3. 処理フロー](#33-処理フロー)


# 1. 概要
- 認証関連のServiceクラス

# 2. LineToken取得：getLineToken
- LINE Tokenを取得する

## 2.1. 使用するテーブル
- メイン：group_line_infos

## 2.2. IF定義
### 2.2.1. INパラメータ

| Key | Value | その他 |
| :--: | :-- | :-- |
| lineId | LINE ID | [データ定義](../../03_テーブル/readme.md#241-line-idpkline_id) |

### 2.2.2. OUTパラメータ
| Key | Value |
| :--: | :-- |
| token | LINE Token |
| memo | メモ |

## 2.3. 処理フロー
1. 下記条件で検索
  - [group_line_infos].[line_id] = INパラメータ.lineId
2. 下記データを返却

| Key | Value |
| :--: | :-- |
| token | [group_line_infos].[token] |
| memo | [group_line_infos].[memo] |

# 3. LINEメッセージ送信処理：sendMessage
- LINEへメッセージ送信する

## 3.1. 使用するテーブル
- メイン：group_line_infos

## 3.2. IF定義
### 3.2.1. INパラメータ

| Key | Value | その他 |
| :--: | :-- | :-- |
| lineId | LINE ID | [データ定義](../../03_テーブル/readme.md#241-line-idpkline_id) |
| message | 送信文 | |

## 3.3. 処理フロー
1. [LineService.getLineToken](#2-linetoken取得getlinetoken)を使用してINパラメータ.lineIdのLINE Tokenを取得
2. 取得したLINE Token向けにINパラメータ.messageをLINE送信