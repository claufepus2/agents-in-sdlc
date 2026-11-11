---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩む組織は少なくありません。未解決のセキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残った機能リクエストなどがあります。GitHub Copilot の cloud agent は、コードの更新や機能の追加といったタスクを自律的に実行します。作業が完了すると、人間の開発者がレビューできるドラフト PR を作成します。これにより、面倒な作業を任せて開発を加速し、開発者はより大きな課題に集中できます。

Copilot cloud agent について、次のことを学びます。

- コード生成環境をカスタマイズする方法。
- 操作を安全に実行する方法。
- 範囲を明確に定めた issue の重要性。
- issue を Copilot に割り当てる方法。

## シナリオ

Tailspin Toys には解決したい技術的負債があります。最初のサイトを作成した委託先は、ドキュメントの整備を適切に行いませんでした。実際、ドキュメントはほとんどありません。まず、アプリケーションのエクスポート関数すべてに TSDoc のドキュメントコメントを追加します。

さらに、デザインチームはゲームの発見性を高めたいと考えています。各ゲームの詳細ページに、同じカテゴリに属する他のタイトル、つまり関連ゲームを表示し、利用者がゲームを見続けられるようにします。現時点では完成された実装は必要なく、UX の受け入れテストに使えるものを求めています。この作業は現在のブロッカーですが、優先度の高い他の issue があるため後回しになっています。

どちらも、優先度が下がりやすいタスクです。Copilot cloud agent に割り当てると、開発者が他の作業に取り組んでいる間に非同期で処理できます。その後、Copilot の作業をレビューして、期待どおりか確認できます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同様にバックグラウンドでタスクを実行できます。たとえば、[GitHub issue を Copilot に割り当て][assign-issue]ます。割り当てられると、Copilot は進行状況を追跡するドラフトプルリクエストを作成し、環境をセットアップして、タスクに取り組み始めます。進行中でも完了後でも、Copilot のセッションを詳しく確認できます。提案された解決方法のレビュー準備ができると、プルリクエストで通知されます。

## 適切な範囲の指示が重要

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見簡単そうな操作でも、詳しく見ると複雑な場合があります。[Copilot cloud agent に依頼するタスクの範囲を慎重に決めてください][cloud-agent-best-practices]。AI ペアプログラマーとして扱い、段階的に進めながら、学習し、試し、適応させます。生成 AI を加えても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示と Copilot への影響を確認しました。カスタム指示の演習をすでに行った場合は `.github/copilot-instructions.md` の効果を見ています。まだの場合は、この機会に確認しましょう。Copilot cloud agent に作業を割り当てる前に、このリポジトリに含まれる指示ファイルを読み取り専用で確認し、後でその影響を見つけられるようにします。

リポジトリの GitHub Web UI または起動中の codespace で、次のファイルを開きます。

- `.github/copilot-instructions.md` - **Code standards** セクション、特に TypeScript の規約と TSDoc ドキュメントコメントに関する要件を確認します。
- `.github/instructions/unit-tests.instructions.md` - `applyTo` frontmatter を確認します。この指示は `**/*.test.ts` ファイルに適用されます。

次のセクションで `Code lacks documentation` issue を cloud agent に割り当てます。プルリクエストに TSDoc コメント、TypeScript の規約、コメントヘッダーが含まれることを確認してください。これらの指示ファイルから反映されます。後の演習で PR をレビューする際に、あらためて確認します。

## Copilot cloud agent の開発環境をセットアップする

誰が作業する場合でも、コードの作成には通常、特定の環境とセットアップスクリプトが必要です。作業開始前に環境を整える必要があります。ソフトウェアエンジニアのように作業する Copilot にタスクを割り当てる場合も同様です。

Cloud agent は [GitHub Actions][github-actions] を使って作業環境を構築します。`.github/workflows/copilot-setup-steps.yml` に設定する[専用のセットアップワークフロー][setup-workflow]を作成すると、作業開始前に実行する処理をカスタマイズできます。必要な開発ツールや依存関係を利用できるようにします。このラボでは手順を円滑に進められるよう、あらかじめ設定されています。単一の Astro アプリのために、Node.js、プロジェクトの依存関係、エンドツーエンドテスト用のブラウザーバイナリ、移行とシードが済んだ SQLite データベースを Copilot が利用できます。

```yaml
name: "Copilot Setup Steps"

# リポジトリの Actions タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 必要な権限だけを設定します。Copilot は操作用の独自トークンを受け取ります
    permissions:
      # セットアップで依存関係のインストールなどのためにリポジトリをクローンする場合は、`contents: read` 権限が必要です。セットアップでクローンしない場合は、手順の完了後に Copilot が自動的にクローンします
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体が Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ビルドとテスト用にローカル SQLite データベースを移行してシードします
      - name: Set up the database
        run: npm run db:setup
```

このファイルは一般的な GitHub ワークフローと同じように見えますが、いくつか重要な点があります。

- `copilot-setup-steps` という単一のジョブがあります。このジョブは、Copilot がプルリクエストの作業を開始する前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーが設定されています。リポジトリの **Actions** タブからワークフローを手動実行できます。Copilot が実行するのを待たずに、ワークフローが成功するか確認できます。

## ドキュメントを追加する

ドキュメントの重要性は広く理解されていますが、情報が古い、またはドキュメントが不足しているプロジェクトは少なくありません。このような技術的負債は放置されがちで、生産性を下げ、コードベースの保守や新しい開発者の受け入れを難しくします。Copilot はドキュメントの作成に役立つため、この issue は Copilot cloud agent に依頼するのに適しています。Copilot がバックグラウンドで必要なドキュメントを生成します。後の演習で結果をレビューします。

1. 新しいブラウザータブで github.com のリポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択して issue を作成します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択して割り当てダイアログを開きます。

  ![Copilot を issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

  ![Copilot の割り当て詳細画面](../../../_images/ex4-assign-copilot-details.png)

10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のようなものです。一覧に表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分後、Copilot が TODO リストを作成していることを確認します。

> [!NOTE]
> Copilot が PR に TODO リストを表示するまで、数分かかる場合があります。前述のワークフローを使って環境を作成し、プロジェクトを分析して、問題に取り組む方法を判断しています。

13. リストと実行予定のタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が issue の作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選択します。

  ![Copilot セッションの表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新後の状態を示す表示を確認するには、ウィンドウを更新する必要がある場合があります。

16. ライブセッションをスクロールし、Copilot が問題を解決する様子を確認します。コードを調べて状態を理解し、方針を検討して計画を決め、コードを作成していきます。

完了まで数分かかる場合があります。Copilot cloud agent の主な利点は、非同期にタスクを進め、開発者が別の作業に集中できることです。この機能を使い、別のタスクも Copilot cloud agent に割り当ててから、アプリケーションの機能を追加する作業に取り組みます。

## ゲーム詳細ページに関連ゲームを追加する

GitHub Copilot cloud agent の大きな利点のひとつは、開発者がある作業に集中している間に、別の作業を並行して任せられることです。デザインチームのために関連ゲームを追加する作業は時間がかからないかもしれませんが、その時間を別の作業に使えます。Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択して、空のテンプレートを使います。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択して割り当てダイアログを開きます。

  ![Copilot を issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、Copilot が issue に取り組んでいることを示す 👀 が、issue の最初のコメントに表示されます。

![issue に取り組んでいることを示す目の絵文字を Copilot が使う例](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は2つ目の依頼にも取り組んでいます。Copilot cloud agent はソフトウェアエンジニアと同じように作業するため、常に監視する必要はありません。完了後にレビューできます。次は、カスタムエージェントの作成と使用を確認します。

## まとめと次のステップ

このレッスンでは [GitHub Copilot cloud agent][copilot-agents] を確認しました。Cloud agent に issue を割り当てると、非同期に作業を実行できます。技術的負債への対処、新機能の作成、別のフレームワークからのコード移行などに活用できます。

次のことを学びました。

- コード生成環境のカスタマイズ。
- 操作を安全に実行すること。
- 範囲を明確に定めた issue の重要性。
- issue を Copilot に割り当てる方法。

Cloud agent がバックグラウンドで作業している間に、カスタムエージェントの作成と使用に取り組みます。[Copilot cloud agent は MCP サーバーも利用でき][cloud-agent-mcp]、前のモジュールで学んだカスタム指示にも対応しています。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローに関するベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
