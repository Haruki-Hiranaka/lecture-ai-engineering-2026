# 第6回 解答

## 演習

<!-- [tool call] の行は、表示された行をそのまま貼り付けてください。終わらなかった STEP は、空欄のままでかまいません。 -->

- STEP 1 のツールの名前：glossary_search_glossary
- STEP 2 の [tool call]：[tool call]   glossary_search_glossary({'query': '3レベル'})
- STEP 3 の [tool call]：[tool call]   fetch_fetch({'url': 'https://modelcontextprotocol.io/'})
- STEP 4 の [tool call]：[tool call]   aws_aws___search_documentation({'search_phrase': 'Amazon S3 バージョニング 概要'}) / [tool call]   aws_aws___search_documentation({'search_phrase': 'What is S3 Versioning Amazon Simple Storage Service', 'topics': ['general']})
- 気づき・疑問：モデルは gemini-3.5-flash-lite。STEP 1 では app.agent のツール（search_materials、make_quiz）も MCP サーバーのツールも、名前・説明文・引数が同じ形で表示された。ツール数は glossary だけで 1 個、fetch を足して 2 個、aws を足して 7 個になった。aws のツール名は「aws_」のあとにサーバー側の名前「aws___search_documentation」が続き、aws が 2 重になっていた。入力トークンは STEP 2 が 715、STEP 3 が 2,230、STEP 4 が 11,355（モデル応答 3 回、検索を 2 回呼んだ）で、ツールの説明文と返り値が増えるほどトークンが増えることが分かった。

## 宿題

<!-- 基礎・実装・発展のどれか1つに取り組めば完了です。 -->

- 取り組んだレベル：基礎 / 実装 / 発展
- やったこと：
- 結果：
- 気づき・疑問：
