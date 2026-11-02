---
title: "レッスン 8 - 機能の PR の作成とマージ"
description: "フィルター機能、指示、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使用します。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

フィルター機能の実装、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストを1つのブランチに保存しました。これらをまとめてレビューし、pull request を作成します。星評価の pull request (PR) は自分でマージしましたが、今回は **Agent Merge** にプロセスの管理を任せます。

> [!NOTE]
> 通常は、機能、指示の更新、スキルの更新、QA エージェントをいくつかの別々の PR に分けます。ワークショップを円滑に進めるため、フィルター機能と品質に関するワークフロー全体を1つのセッションとブランチで進め、そのすべての作業をこの PR にまとめました。

このレッスンでは、次の内容を学習します。

- Agent Merge の概要と、マージのライフサイクルを自動化する仕組みを学ぶ。
- 機能の PR 全体と検証の証拠を確認する。
- レビュー後にのみ Agent Merge を承認し、PR のマージを確認する。

## シナリオ

フィルター機能のワークフロー全体を通じて、Copilot を使用して機能の計画、実装、検証を行いました。Tailspin Toys は、マージの承認を開発者の管理下に置きながら、残りの PR 作業を自動化したいと考えています。

## Agent Merge の概要

**Agent Merge** を使うと、GitHub Copilot app でプルリクエストをマージするまでの作業を自動化できます。有効にすると、アプリのセッションがプルリクエストを読み取り、失敗した継続的インテグレーション (CI) チェックの修正、レビューコメントへの対応、必要に応じたリベースなど、マージを妨げる問題に対処します。その後、GitHub で許可され次第マージします。バックグラウンドで動作し、アプリを再起動しても続行し、プルリクエストがマージされると自動的に停止します。

ここまでは、自分で **Merge pull request** を選択していました。Agent Merge に任せることもできますが、コードの編集やマージには引き続き明示的な承認が必要です。マージを許可する前に、許可する操作と作業内容をレビューしてください。

## Agent Merge で PR を管理する

コードの作成とレビューが完了したので、Agent Merge に PR プロセスを管理させましょう。

1. エージェントピッカーで **Default agent** を選択します。
2. **Create PR** の横にあるドロップダウンを選択します。
3. **Agent Merge** を選択します。ボタンが **Agent Merge** に変わります。
4. **Agent Merge** ボタンを選択してプロセスを開始します。ドロップダウンから選択するだけでは開始されません。

Agent Merge のプロセスが始まり、次の操作を行います。

- タイトルと説明を含むプルリクエストを作成します。
- Issue からセッションを開始した場合は、説明の本文で関連する Issue を参照します。
- ターゲットブランチへのリベースやマージ競合に対処します。
- CI を監視してすべてのチェックが成功することを確認します。
- 他の開発者や Copilot code review からのフィードバックを監視し、コメントを解決するために更新します。
- 必要に応じて、すべて成功した後に PR を自動的にマージします。

すべてが成功したら Agent Merge が PR もマージするように設定します。

5. マージを許可する前に、PR の差分と検証の証拠をレビューします。
6. **Agent Merge** の横にあるドロップダウンを選択します。
7. **Merge pull request** の横にチェックが付いていることを確認します。
8. エージェントに明示的にマージを依頼します。

    ```plaintext
    Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
    ```

9. 進捗を確認し、報告された阻害要因を解決します。続行する前に PR の状態が **Merged** になっていることを確認します。マージ権限を有効にしたことやエージェントが実行中であることは、マージ完了の確認にはなりません。

> [!IMPORTANT]
> Agent Merge は、リポジトリの保護や権限不足を回避しません。続行前にそれらの阻害要因を解消してください。

## まとめと次のステップ

コードの生成、テストと検証、pull request のプロセスなど、開発プロセスの複数の部分を自動化しました。具体的には、次の作業を行いました。

- Agent Merge の概要と、マージのライフサイクルを自動化する仕組みを学習した。
- 機能の PR 全体と検証の証拠を確認した。
- レビュー後にのみ Agent Merge を承認し、PR がマージされたことを確認した。

次は、[既存のキャンバスを使用してトリアージキャンバスを作成し][next-lesson]、エージェントと一緒に作業を確認、計画、視覚化するための、より豊かな方法を学びます。

## リソース

- [GitHub Copilot app での Issue と pull request の管理][managing-issues-prs]
- [GitHub Copilot app について][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app