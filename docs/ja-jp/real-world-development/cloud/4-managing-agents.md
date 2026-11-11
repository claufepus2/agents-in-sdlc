---
title: "演習 4 - エージェントを監視して管理する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前の2つの演習では、Copilot cloud agent にユーザーエクスペリエンスの改善や機能追加に関する3つのタスクを割り当てました。Cloud agent は非同期かつ自律的に作業するように設計されていますが、タスクを監視することも重要です。

cloud agent に割り当てたタスクを管理する方法はいくつかあります。[GitHub.com のエージェントページ][agents-page]もそのひとつです。mission control から、未完了のプルリクエスト (PR) があるすべてのエージェントタスクを確認できます。実行内容を調べたり、進行中のセッションを導いて作業を調整したりすることもできます。

このレッスンでは、次のことを行います。

- エージェントページを開き、cloud agent のタスクを監視します。
- 進行中のセッションに追加の機能を依頼します。

## シナリオ

エージェントに高コントラストモードを作成するよう割り当てた後、チームはライトモードも追加するよい機会だと考えました。サイトのスタイル更新と切り替え機能を作業中なので、これも一緒に実装するのが自然です。エージェントに高コントラストモードとライトモードの両方を追加するよう案内します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの現在の状態を確認しましょう。

1. [Copilot agents ページ](https://github.com/copilot/agents)に移動します。
2. メインペインと左ペインの両方に表示されるタスク一覧を確認します。次のような、Copilot に割り当てたタスクが表示されます。
    - コードのドキュメントを更新する。
    - ゲーム変更用の API を生成する。
    - Web サイトに高コントラストモードを追加する。
3. 実行中のタスクを選択し、Copilot が実行したタスクを確認します。次のような作業が含まれる場合があります。
    - リポジトリをチェックアウトする。
    - Copilot が作業する環境を作成する。
    - MCP サーバーを設定する。
    - 割り当てられたタスクを完了するための作業を行う。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や実装方法によって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインも確認します。PR と変更されたファイルを確認して、作業を監視できます。

## Cloud agent に作業を追加で依頼する

実行中のタスクを確認できたので、高コントラストモードの作業中にライトモードの切り替え機能も追加するよう Copilot に依頼します。

1. 高コントラストモードの追加に関するセッションを選択します。正確なタイトルは Copilot が付けた名前や作業状況によって異なります。

    ![mission control に表示されるアクセシビリティセッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業が始まったことを示すまで数分間セッションを確認します。次のようなメッセージが表示されたら、作業を開始しています。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![エージェントページのクラウドエージェントタスクで、作業中のセッションを導くダイアログが強調表示されたスクリーンショット](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業に含めることを確認します。

## Copilot の作業を待つ

Copilot が更新されたタスクに取り組みます。メッセージを送信した時点で作業していた手順が終わると、新しい要望が作業の流れに反映されます。

今回も数分かかります。ここで一度休憩して、ここまでに学んだことを振り返りましょう。

## まとめと次のステップ

このレッスンでは、GitHub Copilot cloud agent のタスクを監視して方向付ける中心的な場所である Copilot agents ページを確認しました。mission control では、進行中および完了したタスクの追跡、実行中の作業のレビュー、進行中のタスクの方向修正ができます。

次のことを学びました。

- mission control とエージェントページを使って、cloud agent のタスクを監視しました。
- 進行中のセッションに追加の機能を依頼しました。

Copilot がアクセシビリティ機能を実装している間に、[Copilot が作成したプルリクエストを反復して改善][next-lesson]しましょう。

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
