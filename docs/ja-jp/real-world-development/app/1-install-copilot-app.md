---
title: "レッスン 1 - GitHub Copilot app のインストール"
description: "GitHub Copilot app をインストールし、テンプレートから作成したリポジトリを接続して、ワークスペースを確認し、クイックチャットを試します。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

[**GitHub Copilot app**][about-copilot-app] は、エージェント主導の開発に使用するデスクトップアプリケーションです。GitHub Copilot CLI を基盤とし、GitHub とネイティブに統合されているため、リポジトリ、ブランチ、CI パイプラインをすぐに利用できます。すべての作業を自分で行うのではなく、複数のエージェントをそれぞれ分離されたワークスペースで並列に指示し、繰り返し発生するタスクを自動化するワークフロー向けに設計されています。Node.js のインストールとプロジェクトのコピーが完了したので、次はアプリをインストールして、そのリポジトリを接続します。

このレッスンでは、次の内容を学習します。

- GitHub Copilot app をインストールしてサインインする。
- GitHub リポジトリからプロジェクトをアプリに追加する。
- テンプレートによって用意されたバックログを含め、ワークスペースを確認する。
- クイックチャットを試して、アプリ自体について学ぶ。

## シナリオ

チームは、増え続けるバックログに対応するために AI エージェントを導入しています。Copilot app では、Issue の選択、エージェントの実行、変更のレビュー、pull request のマージを一か所から指示できます。このレッスンでは、アプリをインストールして接続し、プロジェクトについての会話を始められるようにします。

> [!NOTE]
> 対象となる Copilot プラン (Copilot Free、Copilot Student、または Pro、Pro+、Business、Enterprise の有料プラン) が必要です。Copilot Business または Copilot Enterprise を使用している場合、アプリを動作させるには管理者が **GitHub Copilot app** ポリシーを有効にする必要があります。

## GitHub Copilot app をインストールして構成する

GitHub Copilot app を使用するには、まずアプリをインストールします。Windows、macOS、Linux 向けのバージョンが用意されています。アプリをインストールして認証し、Tailspin Toys リポジトリを追加します。

1. ブラウザーで [GitHub Copilot app のランディングページ][download-app]を開きます。
2. 使用しているプラットフォーム向けのアプリをダウンロードし、ランディングページの手順に従ってインストールします。
3. インストールが完了したら、アプリを開きます。
4. **Sign in to GitHub** を選択し、画面の指示に従って認証します。データ所在地を利用する GitHub Enterprise Cloud の場合は、**Use GitHub Enterprise** を選択し、求められたら `*.ghe.com` ホスト名を入力します。GitHub Enterprise Server はサポートされていません。
5. オンボーディングでリポジトリの選択を求められたら、テンプレートから作成した Tailspin Toys リポジトリを選択します。リポジトリ選択をスキップし、オンボーディング完了後にプロジェクトを追加することもできます。
6. テーマの選択を求められたらテーマを選び、オンボーディングを完了します。
7. サイドバーの **Sessions** でリポジトリを探します。すでに一覧にある場合は、再度追加する必要はありません。
8. リポジトリがまだ接続されていない場合は、**Sessions** の横にある **+** を選択して新しいセッションを作成し、**Add GitHub repository** を選びます。前提条件のレッスンで記録した所有者とリポジトリ名を使用します。
9. サイドバーにリポジトリが表示されていることを確認します。ここで使うレイアウトでは、`tailspin-toys` などのリポジトリフォルダーが **Sessions** の下にまとまり、その下に個々のセッションが表示されます。

> [!NOTE]
> オンボーディングでリポジトリを選択しなかった場合は、**Sessions** の横にある **+** から後で追加できます。

## ワークスペースを確認する

プロジェクトを接続したら、各領域を確認します。アプリのサイドバーは、主に次の領域で構成されています。

- **New** - ここから Copilot との新しいチャットセッションを開始できます。
- **Pull requests** - アプリの GitHub ネイティブ統合を通じて表示される pull request です。アプリを離れずに、pull request の参照や絞り込み、CI ステータスの確認、pull request のレビューを行えます。
- **Issues** - 同じように表示される Issue です。Issue の参照や絞り込み、Issue からのセッション開始を行えます。
- **Automations** - スケジュールまたはオンデマンドで実行する、保存済みのエージェントタスクです。やることリストの管理、定期的なプロジェクトの保守、その他の手間のかかる作業を任せるのに役立ちます。振り返りでは次のステップとしてリンクを紹介し、追加の演習にはしません。
- **Customize** - MCP server、プラグイン、スキルなどのコンポーネントによって、Copilot app に機能を追加します。Playwright MCP の設定に使用します。
- **Chats** - 独自のブランチやワークスペースを必要としない、質問やブレインストーミング向けの簡易的な会話です。最初のチャットを始めると表示されます。表示されない場合は、**Sessions** の横にある **+** を選択して最初のチャットを開始します。
- **Sessions** - リポジトリフォルダーとエージェントセッションをまとめます。`tailspin-toys` などのリポジトリ名を選択するとリポジトリビューが開き、その下のセッションを選択すると会話に戻ります。各セッションは分離された独自のワークスペースで動作するため、変更を競合させずに複数のセッションを同時に実行できます。星評価を追加するレッスンで最初のセッションを開始します。

ワークショップを進めながら、ワークスペースを確認していきます。

> [!TIP]
> 迷ったときは Copilot に質問しましょう。操作方法がわからない場合や、何かが可能かどうか知りたい場合は、Copilot に尋ねると案内してくれます。

### 用意されたバックログを確認する

バックログのないプロジェクトはほとんどなく、Tailspin Toys も例外ではありません。テンプレートからリポジトリを作成したときに用意された、現在のバックログを確認しましょう。

1. サイドバーで **Issues** を選択します。
2. Issue 番号を決めつけず、次のタイトルで検索します。

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. Issue を選択して詳細を読みます。各 Issue はエージェントセッションの開始点にもなります。小規模な最初の変更を完了した後、フィルター機能の Issue から作業を開始します。

> [!NOTE]
> **Issues** と **Pull requests** の項目一覧は自動的に絞り込まれ、Copilot app に追加したリポジトリの項目だけが表示されます。ほかのリポジトリの作業項目を表示するには、そのリポジトリをアプリに追加してください。

## クイックチャットを試す

アプリに慣れるには、アプリ自体について質問するのが効果的です。その用途には **quick chat** が適しています。Quick chats ではブランチや worktree を作成せずに質問やブレインストーミングができるため、セッションを必要としない、その場限りの簡単な質問に最適です。

1. サイドバーで **Chats** の横にある **+** を選択し、新しいチャットを開きます。
2. アプリのセッションがどのように動作するかを尋ねます。

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. 会話ビューで回答を読みます。各セッションが分離された独自の git worktree で実行されるため、変更が競合することなく複数のエージェントを並列実行できることがわかります。会話はいつでも継続でき、新しいチャットも開始できます。

## まとめと次のステップ

GitHub Copilot app をインストールし、プロジェクトを接続して、ワークスペースを確認しました。学習した内容は次のとおりです。

- アプリをインストールして GitHub にサインインする。
- GitHub リポジトリからプロジェクトを追加する。
- ワークスペースを確認し、**Issues** で用意されたバックログを見つける。
- クイックチャットを使って、その場限りの簡単な質問をする。

次は、最初のエージェントセッションを開始し、ゲームカードに星評価を表示する最初の変更をプロジェクトに加えます。[レッスン 2「星評価の追加で小さな成果を得る」][next-lesson]に進んでください。

## リソース

- [GitHub Copilot app について][about-copilot-app]
- [GitHub Copilot app の概要][getting-started]
- [GitHub Copilot app でのエージェントセッションの操作][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app