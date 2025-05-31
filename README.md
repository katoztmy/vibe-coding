# VibeCoding テンプレートリポジトリ

## 概要

このリポジトリは、VibeCoding プロジェクトのテンプレートとして使用されます。以下のルールに従って開発を進めてください。

## 使い方

「use this template」ボタンを使用し、自身のリポジトリを作成してください。
以後の作業はすべて自身のリポジトリで実施してください。

## 開発ルール

1. **仕様書作成フェーズ**

   - プロジェクト開始時に、`prd.md` を GPTs を使ってつくったものに置き換えてください
   - prd をもとに、画面設計、api 設計、DB 設計、技術選定を行います。
     - それぞれ、Agent に「api 設計仕様書を `api.md` に記述してください」など指示すればベースが完成します。
   - UI デザインは上記仕様書を使用し、以下のツールなどを活用して作ってください。（以下以外でももちろん OK）
     - v0
     - Lovable
     - Create
     - Stitch
     - Claude / Gemini / ChatGPT 　（汎用系）
   - 完成した UI ラフは Html to design なども適宜つかって、Figma にいれておくと便利です
     - 実装時に Figma MCP を使うととっても速いです。

2. **開発フロー**

   - `vibecoding-plan.md`を Agent に作成させてください。
   - 計画書と仕様書に基づいてコードを実装します。計画書のステップごとに進めると良いです。
   - 実装後、コードレビューを行い、必要に応じて修正を加えます。

3. **Git 管理**

   - まめに変更を記録しましょう。Agent に「push しておいて」と依頼するだけでも OK です
   - コミットメッセージも Agent に依頼して作成してもらいましょう
   - 必要に応じてブランチを作成し、プルリクエストを通じて変更をマージしてください。

4. **最後に**
   - Vibe Coding が終わったら学びカリキュラムを生成してください。
   - 学びカリキュラムに沿って学習を進めてみましょう！

## ファイル構成

- `docs/details/api.md` : API 設計書
- `docs/details/database.md` : DB 設計書
- `docs/details/technology-selection.md` : 技術選定書
- `docs/prd/prd.md` : PRD 仕様書
- `docs/vibecoding-plan/vibecoding-plan.md` : VibeCoding 計画書
- `learning-path/learning-path.md` : 学びカリキュラム
- `src/` : ソースコード (実装部分)

## 注意事項

- 不明点があれば、運営やメンターに相談してください。

## API 設定

このアプリケーションは OpenAI API を使用してテキスト変換機能を実装しています。使用するには以下の手順に従ってください：

1. [OpenAI](https://platform.openai.com/)から API キーを取得します
2. プロジェクトのルートディレクトリに`.env.local`ファイルを作成します
3. 以下の内容を`.env.local`に追加します：

```
OPENAI_API_KEY=sk-your-api-key-here
```

4. `sk-your-api-key-here`の部分を実際の API キーに置き換えてください
5. アプリケーションを再起動して変更を適用します

※ `.env.local`ファイルは Git リポジトリにコミットしないでください
