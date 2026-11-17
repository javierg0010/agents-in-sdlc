---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前の2つの演習では、Copilot cloud agent にユーザー体験の改善や機能追加を目的とする3つのタスクを依頼しました。cloud agent は非同期かつ自律的に動作しますが、タスクを監視することも重要です。

cloud agent に割り当てたタスクを管理する方法はいくつかあります。GitHub.com の[エージェントページ][agents-page]もその1つです。この Mission control では、オープンなプルリクエスト (PR) があるエージェントのタスクをすべて確認できます。実行内容を調べたり、進行中のセッションに追加の指示を与えたりできます。

このレッスンでは、次の内容を学習します。

- エージェントページで cloud agent のタスクを監視する。
- 実行中のセッションに追加機能を依頼し、作業の方向を調整する。

## シナリオ

ハイコントラストモードの作成をエージェントに依頼した後、チームはライトモードも追加するとよいと考えました。サイトのスタイルを更新し、切り替え機能を追加する作業をすでに進めているため、ライトモードも含めるのが自然です。エージェントに指示を追加し、ハイコントラストに加えてライトモードも実装してもらいます。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの現在の状態を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)に移動します。
2. メインペインと左ペインのタスクリストを確認します。次のようなタスクが表示されます。
   - コードのドキュメントを更新する。
   - ゲームを変更する API を生成する。
   - Web サイトにハイコントラストモードを追加する。
3. 実行中のタスクを選択し、Copilot が実行した作業を確認します。たとえば、次の作業が含まれます。
   - リポジトリからコードをチェックアウトする。
   - Copilot が作業する環境を作成する。
   - MCP サーバーをセットアップする。
   - 割り当てられたタスクを完了するためのさまざまな処理を行う。

> [!NOTE]
> 表示される具体的な手順は、Copilot の進行状況や作業方法によって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインも確認します。ここでは、PR と変更されたファイルを確認し、作業を監視できます。

## cloud agent の作業の方向を調整する

実行中のタスクを確認できました。次は、エージェントがハイコントラストモードに取り組んでいる間に、ライトモードの切り替えも追加するよう依頼します。

1. ハイコントラストモードを追加するセッションを選択します。エージェントが使うタイトルや作業の状態によって、表示は異なる場合があります。

    ![Mission control のアクセシビリティセッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業が始まるまで数分間セッションを確認します。以下のようなメッセージが表示されたら、作業が始まっています。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![agents ページの cloud agent タスクで、作業中のセッションへの追加指示ダイアログが強調表示された画面](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業に反映したことを確認します。

## Copilot の作業を待つ

これまでと同様に、Copilot が更新されたタスクに取り組みます。メッセージを送信した時点で実行中だった作業が終わると、新しい依頼が作業の流れに反映されます。

この作業にも数分かかります。ここで一度立ち止まり、これまでに学んだことを振り返りましょう。

## まとめと次のステップ

Copilot agents ページは、GitHub Copilot cloud agent のタスクを監視して方向付ける中心的な場所です。進行中と完了済みのタスクを追跡し、実行された作業をレビューし、実行中のタスクに追加の指示を与えて範囲を調整できます。

このレッスンでは、次の内容を学習しました。

- Mission control とエージェントページを使って cloud agent のタスクを監視した。
- 実行中のセッションに追加機能を依頼した。

Copilot がアクセシビリティ機能を実装している間に、[Copilot が作成したプルリクエストを反復して改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
