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

- 取り組んだレベル：基礎
- やったこと：app/mcp_server.py の TODO の下に server.tool()(tools.get_lecture_summary) を 1 行書き足し、用語集サーバーから get_lecture_summary も公開した。git status で変更が app/mcp_server.py だけであることを確かめ、エージェント側（app/mcp_servers.json、app/mcp_agent.py）は変えていない。モデルは gemini-3.5-flash-lite。
- 結果：--list-tools のツールが 7 個から 8 個に増え、glossary_get_lecture_summary が表示された。「第2回では何を学びますか？」と質問すると [tool call]   glossary_get_lecture_summary({'lecture': 2}) が呼ばれ、第2回「ファインチューニングとLoRA」の要点をもとに、事前学習との違い、SFT、PEFT と LoRA、QLoRA、過学習の注意を答えた（入力 5,942 tokens / 出力 258 tokens / モデル応答 2 回）。uv run pytest -q は 21 passed。
- 気づき・疑問：エージェントは起動のたびにサーバーへツールの一覧を問い合わせるので、サーバー側を 1 行変えるだけで、エージェント側をそのままに新しいツールが使えた。INSTRUCTIONS に「回の内容を聞かれたら get_lecture_summary を使う」と書かれているので、モデルは search_glossary ではなくこちらを選んだ。ツールの名前だけでなく、指示文と説明文で呼び分けが決まると分かった。一方、fetch と aws もつないだままなので、この質問でも 8 個分の説明文が渡り、入力が 5,942 tokens と演習の STEP 2（715 tokens）より大きくなった。使わないサーバーをつなぐだけでも費用が増えるので、用途ごとに設定を分けるのがよいのかが疑問。
