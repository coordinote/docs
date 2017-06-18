# DB メソッド

## 概要
DBのメソッドと使用方法に関する設計

## 情報
  * 設計書作成日時
    - 2017/06/18
  * 設計書作成者
    - RS
  * 実装環境
    - nedb
  * 実装先ブランチ
    - `database`
  * 実装先
    - `database/`
  * 重要度
    - 高

## 設計

### データ受取方法, 受渡し方法, etc.
* `/path/to/coordinote/database/nedb_module.js`をrequire
* requireしたオブジェクトのインスタンスを`new` で作成
* 作成したインスタンスからメソッドを実行
* 引数の最後のコールバック関数に戻り値を代入する

### 主なプロセス
* `Node.js` オブジェクトのrequireに準拠
* `nedb` のクエリメソッドを使用
* マスタースレーブ方式
  - 複数のDBファイルの検索結果を連結して1つのオブジェクトとする
  - 連結したオブジェクトをコールバック関数の引数に入れる

### メソッド(関数)
* `set_clip(tag, callback)`
  - `tag`: 型: array, クリップのタグ
  - `callback`: 型: function, 関数の第1引数に生成されたドキュメント(型: object)
* `set_tile(instance, callback)`
  - `instance`: 型: object, `cid`, `idx`, `col`, `tag`, `sty`, `con`で構成
    - `cid`: 型: string, 対応するclipのドキュメントの`_id`
    - `idx`: 型: integer, タイルのインデックス
    - `col`: 型: integer, タイルのカラムサイズ
    - `tag`: 型: array, タイルのタグ
    - `sty`: 型: string, {`txt`, `svg`, `fig`}のいずれかを記述, タイルの種類
    - `con`: 型: string, タイルのコンテンツ
  - `callback`: 型: function, コールバック関数の第1引数に生成されたドキュメント(型: object)
* `get_clip_id(id, callback)`
  - `id`: 型: string, 検索対象タイルの`_id`
  - `callback`: 型: function, 関数の第1引数にクリップ + 対応する全てのタイルドキュメント(型: object)
* `get_tiles_cid(cid, callback)`
  - `cid`: 型: string, 検索対象タイルの`cid`
  - `callback`: 型: function, 関数の第1引数に対応する全てのタイルドキュメント(型: array)

### フィールド(変数)
* なし(jsに各種)

## 備考
* `set_clip()`のサンプルコードを以下に記述

```
const NeDB_Module = require('/path/to/coordinote/database/nedb_module.js')  
let nedb_module = new NeDB_Module()  
nedb_module.set_clip(['test', 'coordinote'], (newdoc) => {  
  console.log(newdoc)  
})  
```