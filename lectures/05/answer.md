# 第5回 解答

## 演習

- 回答モデル（実行時の`[model]`）：
- 比較した質問：
- 対象資料（全文投入と他の方式で範囲が違う点も記録）：

--- trace ---
[time] 回答完了まで 2.03 秒（検索・埋め込みを含む）
[usage] 入力 117,025 tokens / 出力 172 tokens / モデル応答 1 回
[cost] 生成APIの推定コスト: $0.004581 USD（埋め込み・索引構築分は含まない）


--- trace ---
[tool call]   search_materials({'query': 'LoRA 固定 学習 QLoRA 加える'})
[tool return] search_materials -> {"results": [{"page_id": "lec02-main-p052", "document_id": "lec02-main", "lecture": 2, "document_title": "基盤モデルの仕組み", "s…
[time] 回答完了まで 2.93 秒（検索・埋め込みを含む）
[usage] 入力 3,028 tokens / 出力 203 tokens / モデル応答 2 回
[cost] 生成APIの推定コスト: $0.001416 USD（埋め込み・索引構築分は含まない）


--- trace ---
[tool call]   browse_tree({})
[目次を開く] AIエンジニアリング実践講座2026 資料

...

[tool call]   browse_tree({'node_id': 'lec02-main-p052'})
[目次を開く] AIエンジニアリング実践講座2026 資料 → 基盤モデルの仕組み → 選んだモデルを、いつ・どう直すか → LoRA と QLoRA ：大きなモデルも GPU 1 台で調整できる
    （末端ページ・本文717文字を取得）
[time] 回答完了まで 38.94 秒（検索・埋め込みを含む）
[usage] 入力 9,214 tokens / 出力 343 tokens / モデル応答 5 回
[cost] 生成APIの推定コスト: $0.003622 USD（埋め込み・索引構築分は含まない）


--- trace ---
[tool call]   search_graph({'query': 'LoRA QLoRA 固定 学習'})
[tool return] search_graph -> {"seeds": [{"page_id": "lec02-main-p052", "document_id": "lec02-main", "lecture": 2, "document_title": "基盤モデルの仕組み", "sec…
[time] 回答完了まで 2.65 秒（検索・埋め込みを含む）
[usage] 入力 5,771 tokens / 出力 177 tokens / モデル応答 2 回
[cost] 生成APIの推定コスト: $0.001626 USD（埋め込み・索引構築分は含まない）

### 各方式の結果

| 方式 | 回答の要点・正しさ | 引用ページと根拠 | 時間（秒） | 入力／出力トークン数 | モデル応答回数 | 生成API推定費用（USD） |
|---|---|---|---|---|---|---|
| 全文投入 | | | | | | |
| ベクトル検索 | | | | | | |
| Tree | | | | | | |
| Graph | | | | | | |

<!-- 未実行・算定不可の場合は、その旨を書いてください。質問を追加した場合は、質問ごとに表を分けてください。 -->

- 検索だけを実行して見つかったページと、その本文で確認できたこと：
- `--trace`で見えたツール呼び出し（ツール名・引数・回数、Treeでたどった目次など）：
- 取得した情報が足りなかった例、取得できたのに回答へ使えなかった例：
- 今回の用途に採用したい方式と、その理由・比較条件の限界：
- 気づき・疑問：

## 宿題

<!-- 基礎・実装・発展のどれか1つに取り組めば完了です。 -->

- 取り組んだレベル：基礎
- やったこと（変更したファイル・設定・処理、または設計した取得方法）： `app/config.py`の`RAG_TOP_K`を 5->2
- 比較した質問と、固定した条件（モデル・対象資料など）：
  -  質問: '第2回の説明では、LoRAは何を固定し何を学習しますか？ QLoRAは何を加えますか？'
  -  モデル: gemini-3.5-flash-lite
- 結果（変更前後の回答・根拠、`--trace`の出力、時間・トークン数・生成API推定費用など）：
  -  `RAG_TOP_K = 5`
```
[model] gemini-3.5-flash-lite
第2回の講義資料によると、LoRAとQLoRAの仕組みについて以下のように説明されています。

* **LoRA（Low-Rank Adaptation）**
  * **何を固定するか：** 元の重み（本体）を固定し、学習しない（第2回・PDF p.52）。
  * **何を学習するか：** 変化分を表す小さな行列（細長い2つの行列の積。資料のイメージではオレンジ色の部分）だけを学習する（第2回・PDF p.52）。

* **QLoRA**
  * **何を加える（あるいは組み合わせる）か：** LoRAの仕組みに加えて、**元の重みを4bitに圧縮（数値を粗く保存）すること**を加える（第2回・PDF p.51, p.52）。

--- trace ---
[tool call]   search_materials({'query': 'LoRA 固定 学習 QLoRA 加える'})
[tool return] search_materials -> {"results": [{"page_id": "lec02-main-p052", "document_id": "lec02-main", "lecture": 2, "document_title": "基盤モデルの仕組み", "s…
[time] 回答完了まで 2.93 秒（検索・埋め込みを含む）
[usage] 入力 3,028 tokens / 出力 203 tokens / モデル応答 2 回
[cost] 生成APIの推定コスト: $0.001416 USD（埋め込み・索引構築分は含まない）
```
  -  `RAG_TOP_K = 2`
```
[model] gemini-3.5-flash-lite
第2回の講義資料によると、LoRAとQLoRAの仕組みについて以下の説明があります。

* **LoRAが固定するもの・学習するもの**
  * 元の重み $W$ は固定して学習せず、小さな「差分」（細長い2つの行列の積で変化分を表す、図の橙色の小さな行列 A・B）だけを学習します（第2回・PDF p.52）。

* **QLoRAが加えるもの**
  * 元の重みを4bit（数値を粗く保存）に圧縮すること（量子化）を加えます（第2回・PDF p.52）。

--- trace ---
[tool call]   search_materials({'query': 'LoRA QLoRA 固定 学習 第2回'})
[tool return] search_materials -> {"results": [{"page_id": "lec02-main-p052", "document_id": "lec02-main", "lecture": 2, "document_title": "基盤モデルの仕組み", "s…
[time] 回答完了まで 2.30 秒（検索・埋め込みを含む）
[usage] 入力 1,531 tokens / 出力 164 tokens / モデル応答 2 回
[cost] 生成APIの推定コスト: $0.000869 USD（埋め込み・索引構築分は含まない）
```


- 改善した例と、残る失敗・制約：
  -  今回の例については、文章の表現が変化したものの内容に露骨な誤りが生じるようなことはなかった。
  -  入力トークンが半減したほか、出力も4/5程になった。モデル応答回数は変化せず、API推定コストも半分ほどに減少
- 気づき・疑問：
  -  今回の例はすぐにサーチでヒットしただけで、検索した2件の結果に回答のための情報がなければ精度は大幅に落ちていたと考えられる
