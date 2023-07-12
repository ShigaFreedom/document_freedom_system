
- [1. 概要](#1-概要)
- [2. バッジ実行タイミング](#2-バッジ実行タイミング)
- [3. 処理フロー](#3-処理フロー)

# 1. 概要
- HP試合結果の入力のリマインド機能

# 2. バッジ実行タイミング
- 日曜日:12:00
    - 本日以降に作業をしたいため

# 3. 処理フロー
1. [ScheduleService.getDatas()](../../../05_Service仕様書/06_ScheduleService/readme.md#2-スケジュール情報取得getdatas)からスケジュールデータを取得
     - パラメータは下記のようになる

| Key | Value | その他 |
| :--: | :-- | :-- |
| activeDate | 本日日付 |  |
| categories | ["2"(練習試合),"3"(大会),"4"(市大会・市リーグ戦)] | [データ定義参照](../../../03_テーブル/readme.md#2211-データ定義) |

2. 1.のデータが存在している場合、Trelloチケット作成
   1. [MemberService.getDataFromRole](../../../05_Service仕様書/05_MemberService/readme.md#2-役割区分からのメンバー情報取得getdatafromrole)からメンバー情報を取得
        - 下記パラメータを送信

| Key | Value | その他 |
| :--: | :-- | :-- |
| roleCategory | "T01" | [データ定義参照](../../../03_テーブル/readme.md#2511-データ定義)

   2. [TrelloService.createCard](#5-カード作成createcard)でTrelloのチケット作成
      - 下記パラメータを送信

| Key | Value | その他 |
| :--: | :-- | :-- |
| name | [HP試合結果更新]XX/XX(X) | string |
| due | 1. で取得したcategoryが"3"(大会)なら本日21:00,それ以外なら翌日21:00 | 大会は当日中に入力してほしいという要望があったから<br>21:00なのは定例会が20:00からあるのでその間に作業するため |
| memberID | 2-1. で取得したtrello_member_name |  |

3. [LineService.sendMessage()](../../../05_Service仕様書/03_LineService/readme.md#3-lineメッセージ送信処理sendmessage)でLINE送信

| Key | Value | その他 |
| :--: | :-- | :-- |
| lineId | "03"(HP更新係向け) | [データ定義参照](../../../03_テーブル/readme.md#2411-データ定義) |
| message | 本日の試合結果の更新をお願いします。\n<br>タイトル：[Trelloのタイトル]\n<br>期限:[Trelloの期限]\n<br>url：2-2. のshortUrl | 1. で取得したcategoryが"3"(大会)なら末尾に下記追加<br>"※本日は大会だったため、本日中に更新をお願いします。" |