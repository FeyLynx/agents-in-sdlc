---
title: "レッスン 2 - 星評価を追加する: 小さな改善"
description: "GitHub Copilot app で最初のエージェントセッションを開始し、ゲームカードに小さな変更を加えて、最初のプルリクエストをマージします。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

ワークスペースを確認し、クイックチャットを試しました。次は**エージェントセッション**を開始し、プロジェクトに最初の変更を加えます。小さな変更から始めましょう。ゲームのデータには星評価が含まれていますが、ホームページのゲームカードにはまだ表示されていません。エージェントに表示してもらい、変更をレビューして、最初のプルリクエストとしてマージします。

このレッスンでは、次の内容を学びます。

- エージェントセッションを開始し、セッションの構成を確認します。
- エージェントにプロジェクトの小さく対象を絞った変更を依頼します。
- ワークスペースの差分ビューで変更をレビューします。
- アプリをローカルで実行し、ブラウザーで変更を確認します。
- 最初のプルリクエストを作成してマージします。

## シナリオ

Tailspin Toys の各ゲームには星評価があり、ゲーム詳細ページにはすでに表示されています。しかし、ホームページのゲームカードにはタイトル、カテゴリー、publisher、説明しか表示されていません。準備運動として、各カードに既存の評価を表示するようエージェントに依頼します。小規模で自己完結した、最初のセッションに最適な変更です。

## セッションの構成

**セッション**とは、分離された独自のワークスペースで実行されるエージェントとの会話です。各セッションに**専用の git worktree とブランチ**が割り当てられます。そのため、一方で機能を追加し、もう一方でバグを修正するなど、変更を競合させずに複数のセッションを同時に実行できます。セッションはリポジトリごとにサイドバーにグループ化され、選択すると切り替えられます。

セッション内には、エージェントとの**会話**、ファイルを調査・編集するときのエージェントの**ツールアクティビティ**、差分付きの**変更済みファイル**一覧が表示されます。

## セッションを開始して変更を依頼する

プロジェクトを調べて、機能を実装する新しいセッションを開始しましょう。[アプリのセットアップ][prior-lesson]で、GitHub リポジトリからプロジェクトを追加しました。そのリポジトリ用のセッションを作成して、変更を依頼します。

1. GitHub Copilot app に戻ります。閉じている場合は開きます。
2. サイドバー上部の **+** を選択してセッションを開始します。
3. リポジトリとして `tailspin-toys` を選択します。
4. プロンプトボックスの下で **new working tree** と **Interactive** モードを選びます。次のプロンプトを送信して変更を依頼します。

    ```plaintext
    Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
    ```

5. <kbd>Enter</kbd> を押して、プロンプトを Copilot に送信します。

Copilot app は最初にプロジェクトの分離されたコピーである新しい worktree を作成します。次にプロジェクトを調べ、新機能の追加に必要な更新対象ファイルを見つけ、コードを作成します。これで Copilot app を使って新機能を追加できました。

## 差分をレビューする

AI が生成した変更は、どれほど小さくてもマージ前にレビューします。Copilot app で変更を確認しましょう。

1. アプリの右上隅にある **Toggle review panel** を選択します。開いたパネルで **Changes** を選び、変更されたファイルと差分を確認します。

    ![Create PR ボタンの右側に Toggle review panel ボタンがある GitHub Copilot app の上部ツールバー](../../../_images/app-2-review-panel.png)

2. ゲームの詳細を表示する中心的なファイル `GameCard.astro` にコードが追加されていることを確認します。評価があれば表示し、`starRating` が `null` の場合は "No rating yet" に置き換える、小さなブロックが追加されているはずです。

   ```astro
   {game.starRating !== null ? (
       <span class="text-xs font-medium px-2.5 py-0.5 rounded bg-amber-900/60 text-amber-300" data-testid="game-rating">
           ★ {game.starRating} / 5
       </span>
   ) : (
       <span class="text-xs font-medium text-slate-500" data-testid="game-rating-empty">
           No rating yet
       </span>
   )}
   ```

> [!NOTE]
> すべての生成 AI ツールと同様に、Copilot は確率的に動作するため、実際のコードは上記と異なる場合があります。おおむね同じ内容になるはずです。

## 変更を確認する

ブラウザーを開く前に、エージェントが自動チェックを実行した結果を確認します。数値の `starRating` と、`null` の場合の代替表示がテストされていることを確認します。前提条件が不足していたり、チェックがスキップされていたりする場合は成功ではありません。インストールの要求は内容を確認してから承認してください。

コードを読むだけで動くと判断せず、Copilot に Web サイトを起動してもらい、更新した UI を確認しましょう。ブラウザーキャンバスで Web サイトを開くよう依頼します。

> [!TIP]
> キャンバスは、Copilot app 内で使えるインタラクティブなウィジェットです。後のレッスンではカスタムキャンバスを調べ、自分でも作成しますが、ここでは組み込みのブラウザーキャンバスを使います。

1. 次のプロンプトを送信し、アプリを起動してブラウザーキャンバスでページを開くよう Copilot に依頼します。

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

2. しばらくするとアプリが起動し、Copilot app 内にブラウザーウィンドウが開きます。
3. 評価済みのゲームカードに 5 点満点の値が表示されることを確認します。
4. 確認が終わったら、次のプロンプトでこのセッションが起動した開発サーバーを停止します。

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## 最初のプルリクエストを作成してマージする

機能を作成できました。次はプルリクエスト (PR) を作成し、新しいコードを既存のコードベースにマージします。

1. 右上隅にある **Create PR** を選択します。
2. 求められた場合は **Sign in with your browser** を選択し、画面の指示に従って認証します。
3. PR の作成前に対応する Issue が必要だと Copilot が説明した場合は、その手順に従うよう確認します。
4. Copilot が PR を作成します。
5. チャットのすぐ上にある **PR** バブルを選択し、レビューペインでプルリクエストを開きます。ここで PR をレビューできます。
6. 問題がなければ **Ready to merge** を選択します。
7. 新しいダイアログで **Merge pull request** を選択し、プルリクエストをマージします。

## まとめと次のステップ

GitHub Copilot app を使って、最初の変更をリリースできました。具体的には、次のことを行いました。

- エージェントセッションを開始し、セッションの構成を学びました。
- ゲームカードに小さく対象を絞った変更を加えるよう、エージェントに依頼しました。
- ワークスペースの差分ビューで変更をレビューしました。
- アプリをローカルで実行し、ブラウザーで星評価を確認しました。
- PR 1 を作成し、チェックをレビューして明示的にマージしました。

次は[フィルター機能の Issue から開始し、Plan モードと Autopilot モードを使って][next-lesson]、より大きな機能を作成します。

## リソース

- [GitHub Copilot app でのエージェントセッションの操作][agent-sessions]
- [GitHub Copilot app について][about-copilot-app]
- [GitHub Copilot app で Issue とプルリクエストを管理する][managing-issues-prs]

[prior-lesson]: ../1-install-copilot-app/#github-copilot-app-をインストールして構成する
[next-lesson]: ../3-agent-modes/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
