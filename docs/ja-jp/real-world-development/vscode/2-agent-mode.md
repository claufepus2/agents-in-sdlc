---
title: "演習 2 - Copilot エージェントモードで機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションへの単純な変更でも、通常は複数のファイルを更新し、テストなどの操作を実行する必要があります。開発者は、関連するファイルを探し、変更し、テストを実行して、デバッグし、見落としたファイルを特定して、さらに更新するという作業を行います。作業は次々に続きます。

そのような作業に役立つのが Copilot エージェントモードです。

Copilot エージェントモードは、IDE 内でより自律的に作業するように設計されています。開発者のように既存のプロジェクト構成を調べ、必要な更新を行い、テストなどのタスクを実行し、見つかった問題を自動的に修正します。エージェントモードを使って、サイトに新しい機能を追加しましょう。

> [!NOTE]
> 名前は似ていますが、エージェントモードと cloud agent は異なる用途向けに作られています。エージェントモードは IDE 内で動作し、すばやいフィードバックと対話を可能にします。Cloud agent はチームメンバーのように非同期で作業し、issue とプルリクエストを通じてやりとりする共同開発者として設計されています。

この演習では、次のことを学びます。

- Copilot エージェントモードでプロジェクトを調べ、関連ファイルを特定し、複数のファイルを調整して変更する方法。
- GitHub Copilot エージェントモードで UI とデータ層にまたがる新機能を実装する方法。
- コードベースにマージする前に、Copilot エージェントモードが生成した変更とテストをレビューする方法。

## シナリオ

ゲームの一覧が増えてきたため、カテゴリと販売元で絞り込めるようにします。前の演習では販売元ヘルパーを追加しました。次は Copilot エージェントモードを使って、残りのデータ層、UI、テストを実装します。

## Tailspin Toys の Web サイトを起動する

変更を加える前に、Tailspin Toys の Web サイトを確認して、現在の機能を把握しましょう。

この Web サイトは、開発者向けテーマのボードゲームのクラウドファンディングプラットフォームです。ゲームの一覧と詳細を表示できます。単一の Astro アプリで、ビルド時に静的 HTML としてページをレンダリングします。ページは frontmatter 内から Drizzle ORM を通じてローカル SQLite データベースを直接クエリします。別のバックエンド API やクライアントサイド UI フレームワークはありません。再利用可能なデータアクセスヘルパーは `src/lib/` に置き、インタラクティブな機能は標準 DOM API を使った小規模な Astro `<script>` で追加します。

### Web サイトを起動する

`npm run dev` タスクを使うと、単一の Astro 開発サーバーを起動できます。GitHub Codespace で次の手順を実行します。

1. codespace に戻ります。前の演習の続きで `main` にいるはずです。
2. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、Codespace 内に新しいターミナルウィンドウを開きます。
3. フィルタリング作業用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーが起動すると、次のような URL を含むバナーが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くよう促すダイアログが表示された場合は、**x** を選択して閉じます。

5. <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら、ターミナルに表示された Astro サーバーのアドレス `http://localhost:4321` を選択し、Web サイトを開きます。

> [!NOTE]
> codespace で Codespace ターミナルの localhost URL を選択すると、自動的に `https://<your-codespace-name>-4321.app.github.dev/` にリダイレクトされます。これは Web サーバーを実行している codespace へのプライベートトンネルです。

### Web サイトを確認する

Web サイトが起動したら、機能を確認します。主な機能は次のとおりです。

- **ホームページ**: タイトル、画像、説明を含むボードゲームの一覧を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、販売元、カテゴリなどの情報を表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に、codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

Web サイトの初期実装は動作しますが、新しい機能を追加したいと考えています。まずバックログを確認します。テンプレートからプロジェクトをセットアップすると、issue のバックログが自動的に作成されます。GitHub Copilot に内容を表示するよう依頼します。

1. Chat ビューのエージェントドロップダウンから **Agent** を選択します。**Agent** はファイルを横断して変更を計画・実装し、ターミナルコマンドやツールを実行できます。

   ![Chat ビューでエージェントを選択するドロップダウンのスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

2. 使用可能なモデル一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者が、特定のモデルを推奨するものではありません。ワークショップを作成した際に Claude Sonnet 4.5 を使用したため、手順にも記載しています。提案の一貫性を高め、学習しやすくすることを意図しています。ただし、LLM は確率的に動作するため、ワークショップに記載された内容と異なる提案が返されることがあります。これは通常の動作です。

> [!NOTE]
> LLM の確率的な動作により、Copilot が異なる MCP コマンドを使うことがありますが、タスク自体は完了できるはずです。

3. 次のプロンプトを Copilot に送信して、バックログの issue を確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての issue を一覧表示するコマンドを実行します。
5. 生成された issue の一覧を確認します。

Copilot は利用者にとって有益だと判断した順に、項目の優先順位も付けます。

## 指示ファイルを確認する

エージェントにコードを生成させる前に、Copilot にコンテキストを与える指示ファイルを確認します。Web サイトに機能を追加する方法を説明する [ユーザーインターフェイス (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md) の指示ファイルを使います。

1. codespace で `.github/instructions/ui.instructions.md` に移動します。
2. 機能を追加する際の全体的なガイダンスを確認します。内容には次のものが含まれます。
   - アーキテクチャの概要。
   - コンポーネントの設計、テスト容易性、アクセシビリティに関する原則。
   - ファイル種別に応じた指示へのリンク。対象には次のものがあります。
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 指示ファイルから、他の指示ファイルやプロジェクトのファイルを参照できます。パスは指示ファイルの場所からの相対パスです。指示を再利用し、大きな内容を管理しやすい単位に分け、Copilot が参照できる例やテンプレートを示すことができます。

## フィルタリング機能を実装する

フィルタリングを完成させるには、少なくとも次の3つを更新します。

- データ層 (`src/lib/`) の残りのフィルタリングロジックを追加または調整します。
- フィルタリングの動作を確認するテストを追加または更新します。
- ゲーム一覧ページにフィルタリング UI を追加します。

さらに、コードベースにマージする前にテストを実行して成功させる必要があります。Copilot エージェントモードでこれらの作業を実行できます。機能を追加しましょう。

1. Copilot との現在の会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択し、指示ファイルを追加します。

   ![UI 指示ファイルを選択する例を示すスクリーンショット](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェントドロップダウンで、引き続き **Agent** が選択されていることを確認します。

   ![Chat ビューでエージェントを選択するドロップダウンのスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

4. モデルも **Claude Sonnet 4.5** のままであることを確認します。
5. 次のプロンプトを使い、バックログの関連 issue に基づいて機能を実装するよう Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、機能に関連するファイルを特定する様子を確認します。データ層のヘルパー、UI、テストを見つけてファイルを変更し、テストを実行します。

   ![Copilot がプロジェクトファイルを調査している様子を示すスクリーンショット](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトを調べ、ファイルを変更し、テストを実行します。タスクやコードベースの複雑さによっては、数分かかる場合があります。作業中、コードエディターに **Keep** と **Undo** ボタンが表示されることがあります。完了するとすべての変更に対して **Keep** または **Undo** が表示されるので、作業中に選択する必要はありません。

7. Copilot に求められたら **Continue** を選択してテストを実行します。

   ![Copilot Chat パネルでテストの実行を確認するダイアログのスクリーンショット](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に待ち時間が発生したり、テストが失敗したりする場合があります。問題ありません。Copilot はコード生成とテストを反復し、エラーが見つからなくなるまで作業します。

   ![Copilot エージェントモードの Chat セッションが完了した状態を示すスクリーンショット](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを開いているブラウザーに戻り、新機能を試します。
11. 動作を確認してコードをレビューしたら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

この演習では、GitHub Copilot エージェントモードを使って Tailspin Toys の Web サイトに機能を追加する方法を確認しました。次のことを学びました。

- GitHub Copilot エージェントモードで UI とデータ層にまたがる新機能を実装する方法。
- Copilot エージェントモードでプロジェクトを調べ、関連ファイルを特定し、複数のファイルを調整して変更する方法。
- コードベースにマージする前に、Copilot エージェントモードが生成した変更とテストをレビューする方法。

次は [Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成します。

### ボーナス演習 - ページネーションを実装する

ゲームの一覧が増えると、ページネーションが必要になります。この演習で学んだスキルを使い、Copilot にページネーションの実装を依頼します。次の点を検討してください。

- 既存の指示ファイルを使うなど、既存のベストプラクティスに従います。
- ページサイズを利用者が選択できるようにするか、固定にするかなど、ページネーションの方式を検討します。
- 希望どおりの実装に必要なガイダンスを、プロンプトに含めます。
- Copilot にコンテキストを伝えて変更を依頼し、何度か反復する必要があるかもしれません。Copilot を使う際の通常の進め方です。

## リソース

- [Copilot の ask、edit、agent モード: 機能と使い分け][choose-mode]
- [VS Code のエージェントモード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
