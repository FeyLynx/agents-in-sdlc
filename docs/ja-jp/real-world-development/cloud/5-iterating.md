---
title: "演習 5: GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、GitHub Copilot と一緒にユーザー体験の改善や機能追加を行うタスクに取り組みました。コードにドキュメントを追加し、デザインチームが改善できる関連ゲーム機能を実装し、高コントラストモードとライトモードを含むアクセシビリティ機能を実装するよう Copilot に依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを伝え、作業を改善してからチームのレビューを有効にします。

### シナリオ

生成 AI を利用しても、ソフトウェア設計と DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って作業します。チームメンバーがレビューできるようにする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能について GitHub Copilot が提案した変更を確認します。

## GitHub Copilot cloud agent とセキュリティ

Copilot cloud agent は監視されていない状態で非同期にタスクを実行するため、安全性を保つためにいくつかのセキュリティ制約が設定されています。

- Copilot はリポジトリへの読み取りアクセスのみを持ち、書き込みできるのは作業に使用するブランチ**のみ**です。
- Cloud agent は GitHub Actions 内で動作し、作業用の独立した一時環境を作成します。
- GitHub Actions ワークフローを実行するには、人間による承認が必要です。
- [外部リソースへのアクセスは既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。MCP サーバーも対象です。

## 生成されたドキュメントをレビューする

GitHub Copilot cloud agent が最初に作成したプルリクエスト (PR)、つまりコードへのドキュメント追加をレビューします。GitHub.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR を確認するときに、ファイアウォールによって GitHub Copilot がブロックされているという警告が表示されることがあります。Copilot は MCP サーバーへの外部呼び出しを含む外部リソースへのアクセスが既定で制限されているため、これは**想定どおり**です。必要であれば、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して、プルリクエストの一覧を開きます。
3. **Add missing documentation** などのタイトルのプルリクエストを開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストのタイトルに **[WIP]** が含まれます。作業が完了するまで待ちます。数分かかることがあるため、休憩したり、これまでに学んだことを振り返ったりしてください。

4. 準備ができたら **Files changed** タブを選択し、変更をレビューします。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新たに追加された TSDoc のドキュメントコメントやその他のドキュメントを確認します。具体的な変更内容は異なります。

    変更を確認するときは、TSDoc のドキュメントコメント、TypeScript 規約、コメントヘッダーに注目してください。これらは演習 2 の冒頭で確認したカスタム指示ファイルから反映されています。

6. 更新内容をレビューして問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す表示を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. ワークフローがプルリクエストのチェックに追加されます。問題がなければ、単一 Astro アプリのプロジェクトチェックが成功します。完了まで数分かかることがあります。

## GitHub Copilot に変更を依頼する

プルリクエストで Copilot とやり取りすることもできます。チームのメンバーに依頼するように、プルリクエストのコメントやコードのインラインコメントで Copilot にメンションします。Copilot はコメントを確認し、対応するための新しいセッションを開始します。結果は確率的に変わるため、具体的な依頼文は指定できません。たとえば、次のような変更を依頼できます。

- 各コードファイルの先頭に、ファイルの役割を簡潔に説明するコメントヘッダーを追加する。
- TypeScript と Astro のファイルに TSDoc のドキュメントコメントを追加する。
- Astro アプリの構造を説明する README を作成する。

1. 上記の例またはコードベースで確認したい別のドキュメント改善を使い、**@copilot** をメンションして変更を依頼するコメントを追加します。
2. **View Session** を選択し、Copilot が作業する様子を確認します。変更のために新しいセッションが開始されます。
3. **Back to pull request** を選択するとプルリクエストに戻れます。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が終わると、プルリクエストに新しいコミットが表示されます。
5. **Files changed** タブを選択して変更をレビューします。

満足するまで改善を繰り返せます。問題がなければ、ドラフト PR をレビュー可能な状態に変更して `main` にマージできます。

![PR をレビュー可能な状態に変更](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する Issue に対し、Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルの PR を選択します。
4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 更新を確認して問題がなければ **Conversation** タブに戻り、下にスクロールします。
6. ワークフローの承認待ちを示す表示を確認します。
7. ワークフローが承認待ちの場合は **Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. ワークフローがプルリクエストのチェックに追加されます。問題がなければ、単一 Astro アプリのプロジェクトチェックが成功します。完了まで数分かかることがあります。
9. **省略可能:** Codespace でこのブランチに切り替えて、関連ゲーム機能を手動テストできます。Codespace に移動し、ターミナルを開いて、Copilot が作成したブランチ名 (例: **copilot/fix-8**) に置き換えて次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を作成しました。前の PR と同様に、Copilot cloud agent に追加の変更を依頼できます。たとえば、表示する関連ゲームの数を調整したり、コメントヘッダーや TSDoc のドキュメントコメントを追加したりできます (この Issue はカスタム指示を更新する**前**に割り当てたことを思い出してください)。**Conversation** タブでコメントを追加すると、Copilot が新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ用カスタムエージェントで実装した機能をレビューします。この PR には、演習 3 で依頼した高コントラストモードと、演習 4 の mission control で追加を依頼したライトモードの両方が含まれます。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルの PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストのタイトルに **[WIP]** が含まれます。作業が完了するまで待ちます。数分かかることがあります。

4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 実装を確認するときは、特に次の点を確認します。
   - モード切り替え用の UI コンポーネント
   - ユーザー設定を保存する local storage の使い方
   - 高コントラストモードとライトモードの CSS またはスタイルの変更
   - アクセシビリティ属性 (ARIA ラベル、キーボード操作など)
   - モード切り替えを管理する JavaScript/TypeScript コード

6. 更新内容をレビューして問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す表示を確認します。
8. ワークフローが承認待ちの場合は **Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. ワークフローがプルリクエストのチェックに追加されます。問題がなければ、単一 Astro アプリのプロジェクトチェックが成功します。完了まで数分かかることがあります。
10. **省略可能:** Codespace でこのブランチに切り替えて、アクセシビリティ機能を手動テストできます。Codespace に移動し、ターミナルを開いて、Copilot が作成したブランチ名に置き換えて次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    その後、アプリケーションを起動し、ブラウザーで高コントラストモードとライトモードの切り替えをテストします。ページを再読み込みしても設定が保持されることを確認します。

アクセシビリティ用カスタムエージェントが、アクセシビリティのベストプラクティスに沿った機能実装を Copilot に促したことを確認してください。アクセシビリティ上の問題や改善点があれば、前の PR と同じようにコメントで **@copilot** をメンションし、変更を依頼できます。

## 省略可能な演習 — 作業をさらに委任する

Cloud agent に実際のバックログ項目を割り当てれば、その間にほかの作業を進められます。練習として、リポジトリにいくつか Issue を登録し、Copilot に割り当ててください。たとえば次のような作業があります。

- ゲーム詳細ページに支援者向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセスヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent のハーネスを完了しました。これらのレッスンを通じて、次のことを行いました。

- **このリポジトリのカスタム指示ファイルを確認**し、その指示が cloud agent の出力にどう影響するかを確認しました。
- **GitHub Issue を Copilot cloud agent に割り当て**、環境のセットアップ、計画、非同期の実行を確認しました。
- **アクセシビリティ用のカスタムエージェントを作成して使用**し、高コントラストとライトモードの切り替え機能を追加しました。
- **Copilot agents ページを mission control として使い**、アクセシビリティ作業中のセッションを監視して方向付けました。
- **cloud agent のプルリクエストをレビューして改善**し、`@copilot` に変更を依頼し、ワークフローを承認しました。

## レビューと次のステップ

Cloud agent のハーネスを完了しました。さらに学びたい場合は、ほかのハーネスで今回の内容を深められます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE 内で Copilot Chat のエージェントモードと MCP の統合を確認します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan と Autopilot モード、エージェントスキル、QA 用カスタムエージェント、Playwright MCP、Agent Merge、省略可能な `/delegate` による cloud agent への連携を使い、ターミナルから変更をレビューして 2 件完了します。

ご自身のリポジトリでは、次のようなことを試してください。

- リファクタリングやテストカバレッジの Issue を cloud agent に割り当てる。
- 別の分野向けのカスタムエージェントを作成する。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agents について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローのベストプラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを構成する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
