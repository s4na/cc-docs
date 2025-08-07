# 🛠 開発ワークフロー最適化

> Claude Code を組み込んだ最適な開発フローの構築

## テスト駆動開発フロー

```bash
# 1. テストファースト
> UserService.test.tsを作成してください
> ユーザー登録、ログイン、パスワードリセットのテストを含めてください

# 2. Red（失敗確認）
!npm test
> テストが失敗することを確認しました

# 3. Green（実装）
> テストが通るようにUserService.tsを実装してください

# 4. Refactor（改善）
> UserServiceのコードを最適化してください
> パフォーマンステストも追加してください
```

## Git Workflow との統合

### Feature Branch 戦略

```bash
# 1. ブランチ作成
git checkout -b feature/user-auth

# 2. Claude Code でタスク実行
claude
> ユーザー認証機能を実装してください

# 3. 段階的コミット
git add -p  # 部分的にステージング
git commit -m "feat: Add JWT authentication"

# 4. プッシュとPR作成
git push -u origin feature/user-auth
> GitHub ActionsでCIが通ることを確認してください
```

### Worktree による並列開発

```bash
# メイン作業とバグ修正を並行
git worktree add ../hotfix hotfix/critical-bug
git worktree add ../feature feature/new-feature

# それぞれで独立したセッション
cd ../hotfix && claude
cd ../feature && claude  # 別ターミナル
```

## CI/CD パイプラインとの連携

### GitHub Actions 統合

```yaml
# .github/workflows/claude-assist.yml
name: Claude Code Assist
on:
  pull_request:
    types: [opened, synchronize]

jobs:
  review:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Claude Review
        run: |
          claude --no-interaction <<EOF
          このPRの変更をレビューしてください
          セキュリティとパフォーマンスの観点で問題がないか確認してください
          EOF
```

### Pre-commit Hook 設定

```bash
# .git/hooks/pre-commit
#!/bin/bash
claude --no-interaction <<EOF
コミットされるファイルをチェック:
- ESLintエラーがないか
- 型エラーがないか
- テストが通るか
EOF
```

## コードレビューワークフロー

```bash
# レビュアーとして
gh pr checkout 123
claude
> このPRの変更をレビューしてください
> 以下の観点でチェック:
> - セキュリティの問題
> - パフォーマンスの懸念
> - コードの可読性
> - テストカバレッジ

# 開発者として
> レビューコメントに基づいて修正してください
> 修正内容をPRコメントにまとめてください
```

## デバッグワークフロー

### 効率的なデバッグプロセス

```bash
# 1. エラーの特定
> このエラーログを分析してください：
> [エラーログ]

# 2. 原因の調査
> スタックトレースから原因を特定してください
> 関連するファイルを調査してください

# 3. 修正案の提示
> 複数の修正案を提示してください
> それぞれのメリット・デメリットも説明してください

# 4. 実装とテスト
> 最適な修正案を実装してください
> 再発防止のテストも追加してください
```

## リリースワークフロー

```bash
# 1. リリース準備
> バージョンを更新してください
> CHANGELOGを生成してください
> 破壊的変更がないか確認してください

# 2. テスト実行
!npm run test:all
!npm run e2e

# 3. ビルドと検証
!npm run build
> ビルドサイズを前回と比較してください

# 4. タグとリリース
git tag -a v1.2.0 -m "Release version 1.2.0"
git push origin v1.2.0
```

## 日次ワークフローの最適化

### 朝のルーティン

```bash
# 1. 状況確認
git pull
claude
> 昨日からの変更を要約してください
> 今日のタスクをTodoWriteで管理してください

# 2. 優先順位設定
> 優先度の高い順にタスクを並べ替えてください
```

### タスク実行フロー

```bash
# 1. タスク選択
> TodoWriteから次のタスクを選択

# 2. プラン作成
Shift+Tab×2  # プランモード
> このタスクの実装計画を立ててください

# 3. 実装
> 計画に従って実装してください

# 4. 検証
!npm test
!npm run lint

# 5. 完了処理
> タスクを完了にマークしてください
git add . && git commit
```

### 終業時のルーティン

```bash
# 1. 進捗確認
> 今日の成果を要約してください

# 2. ドキュメント更新
> 変更内容をドキュメントに反映してください

# 3. 明日の準備
> 明日のタスクをTodoWriteに追加してください
> CLAUDE.mdを更新してください
```

## 緊急対応フロー

```bash
# 本番障害発生時
# 1. 状況把握
> エラーログから原因を特定してください

# 2. 応急処置
> 最速で問題を解決する方法を提示してください

# 3. 実装
> 修正を実装してください

# 4. デプロイ
!npm run deploy:hotfix

# 5. 事後対応
> 根本原因の分析レポートを作成してください
> 再発防止策を提案してください
```

## 生産性メトリクス

| メトリクス | 測定方法 | 目標値 |
|----------|---------|--------|
| タスク完了率 | TodoWrite統計 | 80%以上 |
| バグ修正時間 | セッション時間 | 30分以内 |
| コードレビュー時間 | PR作成からマージ | 2時間以内 |
| テストカバレッジ | npm run coverage | 80%以上 |
| ビルド時間 | CI/CD | 5分以内 |

## 次のステップ

- [CLAUDE.md活用術](./06-claude-md.md)
- [トラブルシューティング](./07-troubleshooting.md)
- [自動化テクニック](./08-automation.md)

## 参考文献

### 開発ワークフロー
- [Modern Development Workflows](https://martinfowler.com/articles/developer-effectiveness.html) - Martin Fowler
- [Test-Driven Development](https://www.amazon.com/Test-Driven-Development-Kent-Beck/dp/0321146530) - Kent Beck著
- [Continuous Integration](https://www.atlassian.com/continuous-delivery/continuous-integration) - Atlassian CIガイド

### Gitワークフロー
- [Git Flow](https://nvie.com/posts/a-successful-git-branching-model/) - Vincent Driessen
- [GitHub Flow](https://docs.github.com/en/get-started/quickstart/github-flow) - GitHub公式フロー
- [GitLab Flow](https://docs.gitlab.com/ee/topics/gitlab_flow.html) - GitLabフロー

### CI/CD統合
- [GitHub Actions Documentation](https://docs.github.com/en/actions) - GitHub Actions公式
- [GitLab CI/CD](https://docs.gitlab.com/ee/ci/) - GitLab CI/CDドキュメント
- [Jenkins Pipeline](https://www.jenkins.io/doc/book/pipeline/) - Jenkinsパイプライン

### デバッグテクニック
- [Debugging Techniques](https://www.amazon.com/Debugging-Indispensable-Software-Hardware-Problems/dp/0814474578) - David J. Agans著
- [The Art of Debugging](https://www.oreilly.com/library/view/the-art-of/9781593271749/) - O'Reilly

### 生産性メトリクス
- [DORA Metrics](https://dora.dev/) - DevOps Research and Assessment
- [SPACE Framework](https://queue.acm.org/detail.cfm?id=3454124) - GitHub/Microsoft研究