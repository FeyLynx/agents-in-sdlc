---
title: "レッスン 5 - quality-checks スキルをカスタマイズして使う"
description: "既存の quality-checks スキルを確認し、結果のレポート形式をカスタマイズしてフィルター機能を検証します。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くこと以外にも、テスト、lint、継続的インテグレーション (CI) のほかの作業が必要です。これらのタスクには、**エージェントスキル**が適しています。スキルは、こうした操作を適切に実行する方法を Copilot が理解するために役立ちます。

このレッスンでは、次の内容を学びます。

- 既存の `quality-checks` スキルを確認します。
- 結果の報告形式をカスタマイズします。
- スキルを再読み込みして実行します。

## シナリオ

Tailspin Toys は、ユニットテスト、lint、型チェックに `quality-checks` スキルを使っています。チームは、結果を読みやすくするため、報告形式の改善を希望しています。

## 指示、スクリプト、リソース

エージェントスキルには、再利用可能なタスクの指示、実行可能なスクリプト、エージェントが必要に応じて読み込む補助リソースをまとめます。基本的には、スキル名のフォルダーと、その中の Markdown ファイル `SKILL.md` で構成されます。Markdown には、スキルの内容を定義する `name` と `description` のフロントマター、機能の概要、呼び出すタイミングの指針が含まれます。フォルダーには、スクリプトや補助リソース用のサブフォルダーを追加できます。

> [!NOTE]
> スキルに追加のフォルダーやファイルは必須ではありません。Tailspin Toys の `quality-checks` スキルは、プロジェクトにあるコマンドを使うため、`SKILL.md` だけで構成されています。

スキルは、チームで共有して再利用できるリポジトリアセットとしてプロジェクトの `.github/skills` フォルダーに配置することも、ユーザーのスキルフォルダー `~/.copilot/skills` に配置することもできます。

## スキルを確認する

Tailspin Toys のチームが作成した `quality-checks` スキルを確認します。ユニットテスト、lint、型チェックを実行するスキルです。

1. Codespace に戻ります。Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. 先頭の `name` と `description` を読みます。説明によって、Copilot がスキルを呼び出すタイミングを判断します。
3. 指示を読み、テストと lint の手順をどのように案内しているか確認します。
4. まだ **Results output formatting** セクションがないことを確認します。

## 変更前にスキルを実行する

スキルは Copilot CLI から直接呼び出すことも、自然な言葉で依頼することもできます。3 種類のチェックを実行するよう Copilot に依頼します。

1. Interactive モードでフィルター機能の会話に戻ります。
2. 次のプロンプトを送信します。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認します。

## レポートをカスタマイズする

実行した内容、チェックの成否、ツールの報告内容をわかりやすく示すレポートに改善します。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. 次のセクションをファイルの末尾に追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルは自動的に保存されます。

## 更新したスキルを実行する

変更を加えたので、スキルの動作を確認しましょう。Copilot CLI は会話を再起動せずに、編集されたスキルを再読み込みできます。

1. 次のコマンドを入力します。

   ```plaintext
   /skills reload
   ```

2. 前とまったく同じプロンプトを送信します。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認し、最初のレポートと比較します。

## まとめと次のステップ

既存のエージェントスキルをカスタマイズして使いました。このレッスンでは、次のことを行いました。

- ユニットテスト、lint、型チェックを実行する `quality-checks` スキルを確認しました。
- 結果の報告形式をカスタマイズしました。
- スキルを再読み込みして実行しました。

この変更は機能の PR に含めます。次は[Playwright MCP サーバー][next-lesson]を使って、Copilot がサイトを直接操作できるようにします。

## スキルの例

以下のコミュニティの例は参考資料であり、追加のタスクではありません。

- [Agent Skills 仕様][skill-spec]
- [コントリビューションワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図とエクスポートスクリプト: `drawio`][drawio-example]
- [ブラウザーテスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
