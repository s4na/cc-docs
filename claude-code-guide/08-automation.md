# 🤖 高度な自動化テクニック

> Claude Code を使った開発プロセスの自動化

## カスタムコマンドの作成

### 基本的なカスタムコマンド

```markdown
# .claude/commands/review.md
コードレビューを実行:
1. 変更されたファイルを特定
2. セキュリティチェック
3. パフォーマンス分析
4. コード品質評価
5. 改善提案を生成
```

### 複雑なワークフローコマンド

```markdown
# .claude/commands/deploy.md
デプロイメントプロセス:
1. テスト実行
2. ビルド
3. 環境変数チェック
4. Vercelデプロイ
5. Slackに通知
```

## フック（Hooks）による自動化

### 設定ファイルの構成

```toml
# .claude/settings.toml

# 編集後に自動フォーマット
[[hooks]]
event = "PostToolUse"
tool = "Edit"
command = "npx prettier --write {file}"

# ビルド前に型チェック
[[hooks]]
event = "PreToolUse"
tool = "Bash"
pattern = "npm run build"
command = "npm run typecheck"

# エラー時に通知
[[hooks]]
event = "Notification"
type = "error"
command = "osascript -e 'display notification \"Error occurred\" with title \"Claude Code\"'"

# セッション終了時にコミット
[[hooks]]
event = "Stop"
command = "git add -A && git commit -m 'WIP: Claude Code session'"
```

### 高度なフック設定

```toml
# テスト自動実行
[[hooks]]
event = "PostToolUse"
tool = "Edit"
pattern = "*.test.ts"
command = "npm test -- {file}"

# ドキュメント自動生成
[[hooks]]
event = "PostToolUse"
tool = "Write"
pattern = "src/**/*.ts"
command = "npx typedoc --out docs {file}"

# セキュリティチェック
[[hooks]]
event = "PreCommit"
command = "npm audit && snyk test"
```

## MCP（Model Context Protocol）活用

### データベース連携

```bash
# PostgreSQL MCP セットアップ
claude mcp add postgres \
  --env DATABASE_URL=postgresql://user:pass@localhost/db \
  -- npx @modelcontextprotocol/postgres-mcp

# 使用例
> データベースのusersテーブルからTypeScript型を生成してください
> 全テーブルのER図を生成してください
```

### Figma連携

```bash
# Figma MCP セットアップ
claude mcp add figma \
  --env FIGMA_TOKEN=your-token \
  -- npx @modelcontextprotocol/figma-mcp

# 使用例
> FigmaのデザインからReactコンポーネントを生成してください
> デザイントークンをCSS変数として出力してください
```

### カスタムMCPの作成

```javascript
// custom-mcp.js
import { Server } from '@modelcontextprotocol/sdk';

const server = new Server({
  name: 'custom-mcp',
  version: '1.0.0',
});

server.setRequestHandler('analyze', async (params) => {
  // カスタムロジック
  return { result: 'analysis complete' };
});

server.start();
```

## CI/CD統合

### GitHub Actions

```yaml
# .github/workflows/claude-review.yml
name: Claude Code Review
on:
  pull_request:
    types: [opened, synchronize]

jobs:
  review:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: anthropics/claude-code-action@v1
        with:
          api-key: ${{ secrets.CLAUDE_API_KEY }}
          command: |
            /review
            セキュリティ、パフォーマンス、ベストプラクティスの観点でレビュー
          
  test-generation:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: anthropics/claude-code-action@v1
        with:
          api-key: ${{ secrets.CLAUDE_API_KEY }}
          command: |
            変更されたファイルに対してテストを生成してください
```

### GitLab CI/CD

```yaml
# .gitlab-ci.yml
stages:
  - review
  - test
  - deploy

claude-review:
  stage: review
  script:
    - npm install -g @anthropic-ai/claude-code
    - |
      claude --no-interaction <<EOF
      マージリクエストの変更をレビューしてください
      EOF
  only:
    - merge_requests
```

## バッチ処理の自動化

### 定期実行スクリプト

```bash
#!/bin/bash
# daily-maintenance.sh

# Claude Code で日次メンテナンス
claude --no-interaction <<EOF
以下のタスクを実行してください：
1. 依存関係の更新チェック
2. セキュリティ脆弱性のスキャン
3. 未使用コードの検出
4. パフォーマンスボトルネックの分析
5. レポートを生成してSlackに送信
EOF
```

### Cron設定

```bash
# crontab -e
0 9 * * * /path/to/daily-maintenance.sh
0 */4 * * * /path/to/test-runner.sh
0 0 * * 0 /path/to/weekly-report.sh
```

## IDE統合の自動化

### VS Code Tasks

```json
// .vscode/tasks.json
{
  "version": "2.0.0",
  "tasks": [
    {
      "label": "Claude Review",
      "type": "shell",
      "command": "claude",
      "args": [
        "--no-interaction",
        "-c",
        "現在開いているファイルをレビューしてください"
      ],
      "problemMatcher": []
    },
    {
      "label": "Claude Test Generate",
      "type": "shell",
      "command": "claude",
      "args": [
        "--no-interaction",
        "-c",
        "${file}のテストを生成してください"
      ]
    }
  ]
}
```

### Cursor AI 連携

```javascript
// .cursor/settings.json
{
  "claude-code": {
    "autoReview": true,
    "autoTest": true,
    "model": "opus-4",
    "hooks": {
      "onSave": "claude review ${file}",
      "onCommit": "claude test-check"
    }
  }
}
```

## 監視とアラート

### エラー監視

```bash
# error-monitor.sh
#!/bin/bash

# エラーログを監視
tail -f /var/log/app.log | while read line; do
  if echo "$line" | grep -q "ERROR"; then
    claude --no-interaction <<EOF
    以下のエラーを分析して解決策を提示してください：
    $line
    EOF
  fi
done
```

### パフォーマンス監視

```javascript
// performance-monitor.js
const { exec } = require('child_process');

setInterval(() => {
  exec('npm run lighthouse', (error, stdout) => {
    const score = parseFloat(stdout.match(/Score: (\d+)/)[1]);
    if (score < 90) {
      exec(`claude -c "Lighthouseスコアが${score}に低下しました。改善策を提案してください"`);
    }
  });
}, 3600000); // 1時間ごと
```

## テンプレート生成

### プロジェクトテンプレート

```bash
# create-project.sh
#!/bin/bash

PROJECT_NAME=$1
PROJECT_TYPE=$2

claude --no-interaction <<EOF
${PROJECT_TYPE}プロジェクト「${PROJECT_NAME}」を作成してください：
1. ディレクトリ構造を作成
2. 必要な設定ファイルを生成
3. 基本的なコンポーネントを作成
4. テスト環境をセットアップ
5. CI/CDパイプラインを設定
6. README.mdを生成
EOF
```

## 自動化のベストプラクティス

### DO's ✅

1. **冪等性を保つ** - 何度実行しても同じ結果
2. **エラーハンドリング** - 失敗時の処理を定義
3. **ログ出力** - 実行結果を記録
4. **テスト可能** - 自動化スクリプトもテスト
5. **ドキュメント化** - 使い方を明記

### DON'Ts ❌

1. **破壊的操作の自動化** - 削除などは手動確認
2. **無限ループ** - 終了条件を必ず設定
3. **ハードコーディング** - 設定は外部化
4. **エラー無視** - 全てのエラーを処理
5. **過度な自動化** - 人間の判断が必要な部分は残す

## 効果測定

| 自動化項目 | 導入前 | 導入後 | 削減時間 |
|----------|--------|--------|---------|
| コードレビュー | 2時間/PR | 15分/PR | 87.5% |
| テスト作成 | 1時間/機能 | 10分/機能 | 83.3% |
| デプロイ | 30分 | 5分 | 83.3% |
| ドキュメント生成 | 2時間 | 自動 | 100% |

## 次のステップ

- [チーム開発](./09-team-development.md)
- [実例集](./10-case-studies.md)
- [クイックスタート](./01-quick-start.md)