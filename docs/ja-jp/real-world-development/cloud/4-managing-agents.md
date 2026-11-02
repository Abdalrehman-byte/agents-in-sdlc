---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

直前の演習では、ユーザーエクスペリエンスの改善と機能追加を目的に、Copilot cloud agent に 3 つの別々のタスクを依頼しました。cloud agent は非同期かつ自律的に動作しますが、タスクを監視することも重要です。

cloud agent に割り当てたタスクを管理する方法はいくつかあります。[GitHub.com のエージェントページ][agents-page]もその 1 つです。このミッションコントロールでは、開いているプルリクエスト (PR) があるすべてのエージェントタスクを確認できます。実行された操作を調べたり、進行中のセッションに指示を出したりできます。

このレッスンでは、次の内容を学びます。

- エージェントページを開き、cloud agent のタスクを監視する。
- 実行中のセッションに指示を出して、機能を追加するよう依頼する。

## シナリオ

高コントラストモードを作成するようエージェントに依頼した後、チームはライトモードも追加するよい機会だと気づきました。サイトのスタイル更新と切り替え機能をすでに実装しているため、一緒に追加するのが合理的です。高コントラストモードに加えてライトモードも実装するよう、エージェントの作業に指示を出します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの現在の状態を確認しましょう。

1. [Copilot agents ページ](https://github.com/copilot/agents)に移動します。
2. メインペインと左側のペインに表示されたタスク一覧を確認します。次のようなタスクが表示されます。
   - コードベースのドキュメントを更新する。
   - ゲーム変更用の API を生成する。
   - Web サイトに高コントラストモードを追加する。
3. 実行中のタスクを選択し、Copilot が実行した作業を確認します。次のような操作が含まれることがあります。
   - リポジトリからコードをチェックアウトする。
   - Copilot が作業する環境を作成する。
   - MCP サーバーを設定する。
   - 割り当てられたタスクを完了するための各種作業を行う。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や採用した方法によって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインも確認します。PR と変更されたファイルが表示され、さらに状況を監視できます。

## cloud agent に指示を出す

実行中のタスクを確認したので、高コントラストモードの作業中にライトモードの切り替えも追加するよう Copilot に依頼します。

1. 高コントラストモードを追加するセッションを選択します。Copilot が付けたタイトルや作業状況によって、正確なタイトルは異なります。

    ![ミッションコントロールのアクセシビリティセッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了して作業が始まるまで、数分間セッションを確認します。下に示すようなメッセージが表示されたら作業開始です。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Copilot が作業中のエージェントタスクを指示するダイアログが強調表示された、agents ページのスクリーンショット](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot が指示を確認し、作業に取り込むことを確認します。

## Copilot に作業を任せる

Copilot は更新されたタスクに取り組みます。メッセージを送信したときに実行中の手順が終わると、新しい依頼が作業に取り込まれます。

前と同じく、完了まで数分かかります。これまでに学んだことを振り返りながら待ちましょう。

## まとめと次のステップ

GitHub Copilot cloud agent のタスクを監視してガイドする中心的な場所である Copilot agents ページを確認しました。このミッションコントロールでは、実行中および完了したタスクを追跡し、進行中の作業をレビューし、範囲の変更や追加のガイダンスの提供を依頼できます。

このレッスンでは、次の内容を学びました。

- ミッションコントロールとエージェントページを使って cloud agent のタスクを監視しました。
- 実行中のセッションに新機能の追加を依頼しました。

Copilot がアクセシビリティ機能を作成している間に、[Copilot が作成したプルリクエストを改善します][next-lesson]。

## 参考資料

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
