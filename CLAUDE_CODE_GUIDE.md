# Claude Code 完全ガイド

## 目次

1. [Claude Codeとは](#claude-codeとは)
2. [インストールとセットアップ](#インストールとセットアップ)
3. [基本的な使い方](#基本的な使い方)
4. [主要機能](#主要機能)
5. [プロジェクト設定](#プロジェクト設定)
6. [高度な機能](#高度な機能)
7. [ベストプラクティス](#ベストプラクティス)
8. [トラブルシューティング](#トラブルシューティング)
9. [料金とコスト管理](#料金とコスト管理)
10. [実際の使用例](#実際の使用例)

## Claude Codeとは

Claude Codeは、Anthropic社が提供するターミナルベースのAIコーディングアシスタントです。自然言語による指示でコードの生成、編集、デバッグ、テスト、Git操作などを自動的に実行できます。最新のSonnetおよびOpus 4モデルを使用し、プロジェクト全体のコンテキストを理解しながら、複雑なタスクを自律的に実行します。

### 主な特徴

- **フルコードベース認識**: プロジェクト全体の構造とパターンを理解
- **直接ファイル操作**: ファイルの編集、コマンド実行、Git操作を直接実行
- **自律実行**: 複雑な複数ステップのワークフローを独立して実行
- **言語非依存**: JavaScript、Python、Go、Java、Rustなど多様な言語に対応

## インストールとセットアップ

### 必要要件

- **Node.js**: バージョン18以降
- **対応OS**: 
  - macOS 10.15以降
  - Ubuntu 20.04以降 / Debian 10以降
  - Windows 10以降（WSL経由）

### インストール方法

#### 1. NPMによるインストール（従来の方法）

```bash
npm install -g @anthropic-ai/claude-code
```

**注意**: `sudo npm install -g`は使用しないでください。権限の問題やセキュリティリスクが生じる可能性があります。

#### 2. ネイティブバイナリによるインストール（新しい方法）

Linux/macOS:
```bash
curl -fsSL https://claude.ai/install.sh | bash
```

Windows:
```powershell
irm https://claude.ai/install.ps1 | iex
```

### 認証

インストール後、プロジェクトディレクトリに移動してClaude Codeを起動します。認証オプション：

1. **Anthropic Console**（デフォルト）: console.anthropic.comでアクティブな課金が必要
2. **Claude App**: ProまたはMaxプランのサブスクリプション

### 初期設定

```bash
# プロジェクトディレクトリに移動
cd your-awesome-project

# Claude Codeを起動
claude

# インストールの確認
claude doctor
```

## 基本的な使い方

### ターミナル設定

Shift+Enterがデフォルトでは動作しないため、以下のコマンドで修正：

```bash
/terminal-setup
```

### 基本コマンド

#### セッション管理
- `/clear` - 会話履歴をクリアして新規開始
- `/compact` - 会話を要約して重要なコンテキストを保持
- `/model` - 使用するClaudeモデルを切り替え
- `/help` - 利用可能なスラッシュコマンドのリスト表示

#### プロジェクト設定
- `/init` - プロジェクト全体をスキャンして包括的な概要を作成
- `/ide` - IDEと連携してファイルやリンターの警告を共有
- `/install-github-app` - GitHub統合による自動PR レビュー

### ファイル編集

自然言語で編集内容を指定するだけで、Claude Codeが自動的にファイルを編集：

```
「client.pyファイルのコードをリファクタリングしてください」
「calculateTotal関数にコメントを追加してください」
```

### Git操作

```
「新しいブランチ'feature-xyz'を作成して変更をコミットしてください」
「適切なコミットメッセージを生成してPRを作成してください」
```

## 主要機能

### 1. ファイル編集とコード生成

Claude Codeは以下のツールを提供：

- **Edit**: 既存ファイルの修正
- **MultiEdit**: 複数ファイルの一括編集
- **Write**: ファイルの作成または上書き
- **Read**: ファイルの内容を読み取り

### 2. パーミッション管理

デフォルトでは、システムを変更する可能性のあるアクションに対して許可を求めます：

- `/permissions`コマンドで許可リストを管理
- `claude --dangerously-skip-permissions`で許可プロンプトをスキップ（注意して使用）

### 3. カスタムコマンド

`.claude/commands`フォルダにMarkdownファイルを作成してカスタムスラッシュコマンドを定義：

```markdown
# .claude/commands/bug-fix.md
バグを修正するワークフロー：
1. GitHubイシューを作成
2. フィーチャーブランチを作成
3. 修正を実装してテスト
4. PRを作成
```

### 4. フック（Hooks）

Claude Codeのライフサイクルの特定ポイントで自動実行されるシェルコマンド：

- **PreToolUse**: ツール使用前に実行
- **PostToolUse**: ツール使用後に実行
- **Notification**: 通知送信時に実行
- **Stop**: Claude応答終了時に実行

設定は`.claude/settings.toml`に記述：

```toml
[[hooks]]
event = "PostToolUse"
tool = "Edit"
command = "npm run lint"
```

## プロジェクト設定

### CLAUDE.mdファイル

プロジェクトのコンテキストと規約を記録する特別なファイル：

#### 3種類のメモリファイル

1. **プロジェクトメモリ（./CLAUDE.md）**: チーム共有の規約
2. **ユーザーメモリ（~/.claude/CLAUDE.md）**: 個人の設定
3. **ローカルプロジェクトメモリ（./CLAUDE.local.md）**: プロジェクト固有の個人設定

#### CLAUDE.mdの例

```markdown
# プロジェクト概要
このプロジェクトはEコマースプラットフォームです。

# Bashコマンド
- npm run build: プロジェクトのビルド
- npm run typecheck: 型チェックの実行

# コードスタイル
- ES modules（import/export）構文を使用
- 可能な限りdestructuringを使用

# ワークフロー
- コード変更後は必ず型チェックを実行
- パフォーマンスのため個別テストを優先
```

### インポート機能

CLAUDE.mdファイルは`@path/to/import`構文で追加ファイルをインポート可能：

```markdown
プロジェクト概要は@READMEを参照
利用可能なnpmコマンドは@package.jsonを参照

# 追加指示
- git workflow @docs/git-instructions.md
```

## 高度な機能

### 1. MCP（Model Context Protocol）

外部ツールやデータソースと接続するための標準化プロトコル：

#### セットアップ

```bash
claude mcp add clickup --env CLICKUP_API_KEY=YOUR_KEY --env CLICKUP_TEAM_ID=YOUR_ID -- npx -y @hauptsache.net/clickup-mcp
```

#### 設定ファイル（.mcp.json）

```json
{
  "mcpServers": {
    "api-server": {
      "type": "sse",
      "url": "${API_BASE_URL:-https://api.example.com}/mcp",
      "headers": {
        "Authorization": "Bearer ${API_KEY}"
      }
    }
  }
}
```

### 2. IDE統合

VS Code、Cursor、Windsurf、JetBrainsと統合：

- **クイック起動**: Cmd+Esc（Mac）またはCtrl+Esc（Windows/Linux）
- **差分表示**: IDEの差分ビューアで変更を確認
- **コンテキスト共有**: 開いているファイル、診断、選択をClaude Codeと共有

### 3. CI/CD統合

#### GitHub Actions統合

```yaml
- name: Claude Code Review
  uses: anthropics/claude-code-action@v1
  with:
    api-key: ${{ secrets.CLAUDE_API_KEY }}
```

#### ヘッドレスモード

```bash
# 非対話型実行
claude -p "コードをレビューしてください"

# ストリーミングJSON出力
claude -p "テストを実行" --output-format stream-json
```

### 4. 並列開発

Gitワークツリーを使用して複数のClaude Codeセッションを並列実行：

```bash
# 異なるブランチで複数のClaude Codeを実行
git worktree add ../feature-a feature-a
git worktree add ../feature-b feature-b
```

## ベストプラクティス

### コンテキスト管理

1. **頻繁に/clearを使用**: タスク間でコンテキストをリセット
2. **/compactを戦略的に使用**: 自然な区切りで会話を要約
3. **タスクを小さく分割**: コンテキストウィンドウの最後の5分の1に入る前に完了

### パフォーマンス最適化

1. **モデル選択戦略**:
   - 重要な低頻度タスク: Opus 4
   - ルーチンタスク: Sonnet 4またはHaiku 3.5

2. **プロンプトキャッシング**: 入力トークンコストを最大90%削減

3. **バッチ処理**: 出力トークンで50%の節約

### コード規約の遵守

1. **既存パターンを理解**: 編集前にファイルのコード規約を確認
2. **ライブラリを確認**: package.jsonやcargo.tomlで使用ライブラリを確認
3. **セキュリティベストプラクティス**: 秘密情報をコミットしない

### タスク管理

TodoWriteツールを使用してタスクを追跡：

```
タスクリストを作成：
1. バグを調査
2. 修正を実装
3. テストを作成
4. コードレビュー
```

## トラブルシューティング

### よくある問題と解決策

#### インストールエラー

```bash
# NPMパーミッション修正
sudo chown -R $(whoami) ~/.npm

# ディレクトリ所有権
sudo chown -R $(whoami) .
```

#### 接続問題

1. インターネット接続を確認
2. Anthropicのステータスページを確認
3. ブラウザキャッシュをクリア

#### パフォーマンス問題

- `--verbose`フラグでデバッグ情報を表示
- MCPデバッグには`--mcp-debug`フラグを使用

#### Windows固有の問題

WSLが必須：
```bash
# WSL内でNode.jsをインストール
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt install -y nodejs
```

## 料金とコスト管理

### サブスクリプションプラン

- **Claude Pro**: $20/月（固定料金）
- **Claude Max**: 実質無制限の使用

### APIプライシング

トークンベースの従量課金：
- 入力トークン: 安価
- 出力トークン: 約8倍高価

### コスト最適化戦略

1. **ハイブリッドモデルアプローチ**: タスクに応じてモデルを選択
2. **コンテキスト管理**: /clearコマンドで不要なトークン使用を削減
3. **プロンプトテンプレート**: `.claude/commands/`に再利用可能なテンプレートを作成
4. **使用状況監視**: `/cost`コマンドでトークン使用統計を確認

## 実際の使用例

### 1. 大規模コードマイグレーション

ReactからVueへの移行など、数千ファイルの変換を自動化。

### 2. ドキュメント生成と品質向上

既存コードベースのリファクタリング、ドキュメント化、デバッグ。

### 3. マーケティング自動化

CSVファイルから数百の広告バリエーションを生成。

### 4. エンタープライズアーキテクチャ

Java/AWSの企業アーキテクチャ管理とコードレビュー。

### 5. テスト駆動開発

包括的なユニットテストの生成と、エッジケースの特定。

### 実装例

#### WordPress iOS アプリ開発
```
# フィーチャーブランチ作成
+3,672 −4,967 変更（約90%がプロンプトで生成）
# Objective-CからSwiftUIへの書き換え
# 新しいデザインシステムの実装
```

#### E2Eテスト自動化
```
# テスト駆動開発ループ
1. Claude Codeがテストを作成
2. テストの実行と失敗の確認
3. 実装の修正
4. テストの成功確認
```

## セキュリティベストプラクティス

### APIキー管理

1. **環境変数を使用**: ハードコーディングを避ける
2. **定期的にローテーション**: 漏洩の疑いがある場合は即座に更新
3. **最小権限の原則**: 必要最小限の権限のみ付与
4. **使用ログの監視**: 不正なアクセスを検知

### セキュアな設定

```bash
# 環境変数でAPIキーを設定
export CLAUDE_API_KEY="your-key-here"

# リバースプロキシ設定でクライアント側から隠蔽
```

### 自動セキュリティレビュー

```bash
# セキュリティレビューコマンド
/security-review

# GitHub Actions統合で自動化
```

## Claude Code vs 競合ツール

### Cursor
- 価格: $20/月
- IDE統合型
- コード品質はやや高い

### Windsurf
- 価格: $15/月
- よりクリーンなUI
- 自動コンテキスト分析

### Claude Code
- ターミナルベース
- より細かい制御が可能
- Unix哲学に基づく構成可能性

## まとめ

Claude Codeは、単なるコード生成ツールを超えた、包括的な開発パートナーです。適切な設定とベストプラクティスの活用により、開発効率を大幅に向上させることができます。

### 成功のポイント

1. **コンテキスト管理を徹底**: /clearを頻繁に使用
2. **プロジェクト設定を活用**: CLAUDE.mdファイルで規約を共有
3. **自動化を推進**: フックとカスタムコマンドでワークフローを最適化
4. **コスト意識を持つ**: 適切なモデル選択とトークン管理

### 今後の展望

Claude Codeは急速に進化しており、新機能が継続的に追加されています。最新情報は以下で確認：

- 公式ドキュメント: https://docs.anthropic.com/en/docs/claude-code
- GitHubリポジトリ: https://github.com/anthropics/claude-code
- コミュニティリソース: https://github.com/hesreallyhim/awesome-claude-code

---

*このガイドは2025年8月時点の情報に基づいています。最新情報は公式ドキュメントをご確認ください。*