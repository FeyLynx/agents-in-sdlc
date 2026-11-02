---
title: "レッスン 4 - カスタム指示で Copilot を導く"
description: "リポジトリの指示を確認し、ドキュメント標準を追加して、フィルター機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを実行したい場合は、Copilot がその指針にアクセスできるようにします。[指示ファイル][instruction-files]には、求めるコードの内容だけでなく、その構成方法も記載します。フィルター機能を実装したので、Copilot が使った指示を確認し、ドキュメント標準を追加してコードに適用します。

このレッスンでは、次の内容を学びます。

- リポジトリの指示とパスを限定した指示ファイルがエージェントに適用される方法を確認します。
- コーディング標準が守られるように指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発手法に関するガイドラインと要件があります。たとえば、次のようなものです。

- コメントでは、コードを言い換えるのではなく、意図や自明でない判断を説明します。
- `db/` と `src/lib/` にあるエクスポートされた関数には、目的、パラメーター、戻り値を説明する TSDoc/JSDoc を付けます。`db` 引数を注入できる場合は、その引数についても記載します。
- 再利用可能な Astro コンポーネントには `Props` の契約を記載し、関連コードが変わったらコメントも最新の状態にします。
- 既存のフォーマットと lint の指針を維持します。

指示ファイルを使い、Copilot がこれらの開発方針に沿ってタスクを実行できるようにします。

## 指示ファイル

カスタム指示では、コーディングスタイルや要件に関するコンテキストと希望を Copilot に伝えます。コーディング規約、ライブラリ、コードに含めたいコメントの種類などを指定でき、より適切な提案やコードスニペットを得るのに役立ちます。リポジトリ全体、または特定の種類のファイルやタスクに対する指示を作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に送信される単一の指示ファイルです。ほとんどのリクエストに関連するプロジェクトレベルの情報を記載します。
- `.github/instructions/*.instructions.md` ファイルには、特定の言語、ファイル形式、タスクに関する指針を記載します。

> [!NOTE]
> ほかの指示形式への対応状況は、環境によって異なります。特定の形式に依存する前に、[カスタム指示の対応状況][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

スタータープロジェクトには、すでに指示ファイルがあります。変更を加える前に現在の内容を確認し、影響を比較します。

1. Codespace に戻ります。
2. Codespaces エディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡潔な説明とコーディング指針を確認します。この指示は、このリポジトリで Copilot を使うすべての操作に適用されます。
4. `.github/instructions` フォルダーを開いて確認します。Astro ファイル、Drizzle データレイヤー、テストなどに関する指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。この glob で、指示の適用先となるファイルを指定します。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの指示ファイルやプロジェクト内のファイルへのリンクを確認します。大きな指示を再利用しやすい小さなファイルに分け、コード生成の参考となる例に Copilot を誘導できます。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting guidance** セクションにはプロジェクトの標準が記載されていますが、コード内のドキュメントはまだ必須ではありません。次の手順で TSDoc のドキュメントコメントとファイルヘッダーコメントの規則を追加します。

## チームの方針に合うように指示ファイルを更新する

指示ファイルはよい出発点ですが、追加の指示が必要です。`copilot-instructions.md` に TSDoc コメントのルールを追加し、新しく生成される TypeScript に適用します。

1. `.github/copilot-instructions.md` で、35 行目付近にある **Code formatting guidance** セクションを探します。
2. 最後の箇条書きとして、次の行を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

変更は自動的に保存されます。

## 更新した指示を使う

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。指示を保存したら、フィルター機能の会話を終了して再開します。これにより、機能のコンテキストを保ったまま新しい指示を読み込めます。

1. フィルター機能の会話を続けている場合は、`/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. リポジトリのルートから `game-filters-cli` ブランチで保存済みの会話を再開します。

   ```bash
   copilot --resume
   ```

   求められた場合はフィルター機能の会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. 次のプロンプトを Copilot に送信し、更新した指示を適用します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しい TSDoc コメントが追加され、内容がコードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が指示ファイルを読み込む方法を確認し、新しい標準を機能に適用しました。具体的には、次のことを行いました。

- リポジトリの指示とパスを限定した指示ファイルがエージェントに適用される方法を確認しました。
- 指示ファイルに新しい標準を追加しました。
- 指示がコードに与える影響を確認しました。

次は再利用可能な `quality-checks` スキルを[カスタマイズして実行し][next-lesson]、lint とテストが一貫して実行されるようにします。

## リソース

- [Copilot CLI にカスタム指示を追加する][instruction-files]
- [カスタム指示の対応状況][custom-instructions-support]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
