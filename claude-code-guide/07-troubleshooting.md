# 🔧 トラブルシューティング完全ガイド

> よくあるエラーと即座の対処法

## 頻出エラーと即座の対処法

### 1. API Error (Connection error)

```bash
# 原因: ネットワーク問題またはAPI制限
# 対処法:
1. インターネット接続を確認
2. VPNを一時的に無効化
3. claude doctor を実行
4. 5分待ってから再試行
```

### 2. Context Limit Exceeded (400エラー)

```bash
# 原因: 会話が長すぎる
# 対処法:
/compact  # 会話を要約
# または
/clear   # 完全リセット

# 予防策:
- タスクごとに/clearを実行
- 大きなファイルは分割して処理
```

### 3. Rate Limit Reached

```bash
# 原因: 使用制限到達
# 対処法:
1. /cost で現在の使用量確認
2. 別のモデルに切り替え: /model haiku
3. 翌日まで待つ
4. 緊急時は別アカウント使用
```

### 4. Permission Denied

```bash
# 原因: ファイル権限の問題
# 対処法:
sudo chown -R $(whoami) .  # プロジェクトディレクトリの所有権変更
# または
chmod -R 755 .  # 権限を修正
```

### 5. Module Not Found

```bash
# 原因: 依存関係の問題
# 対処法:
> node_modulesを削除してnpm installを実行してください
> package-lock.jsonも削除してください
```

## プラットフォーム別の問題

### macOS 特有の問題

```bash
# 仮想化ツール使用時のエラー
# Parallels/VMware使用時
export NODE_TLS_REJECT_UNAUTHORIZED=0  # 一時的な回避策

# ターミナル設定
/terminal-setup  # キーバインディング修正

# Rosetta 2 関連（M1/M2 Mac）
softwareupdate --install-rosetta
```

### Windows (WSL) 特有の問題

```bash
# WSL2 セットアップ
wsl --set-default-version 2
wsl --install Ubuntu-22.04

# Node.js インストール
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt-get install -y nodejs

# パス問題の解決
export PATH=$PATH:/mnt/c/Program\ Files/nodejs
```

### Linux 特有の問題

```bash
# 権限エラーの解決
sudo npm install -g @anthropic-ai/claude-code --unsafe-perm

# libsecret エラー
sudo apt-get install libsecret-1-dev
```

## バージョン問題の対処

### 安定版へのダウングレード

```bash
# 現在のバージョン確認
claude --version

# アンインストール
npm uninstall -g @anthropic-ai/claude-code

# 安定版をインストール
npm install -g @anthropic-ai/claude-code@1.0.59

# バージョン固定
npm config set save-exact true
```

### Node.js バージョン管理

```bash
# nvm を使用
nvm install 20
nvm use 20
nvm alias default 20

# バージョン確認
node --version  # v20.x.x
npm --version   # 10.x.x
```

## エラー別の詳細対処法

### TypeError: Cannot read property

```bash
# 原因: 未定義のプロパティアクセス
# 対処:
> このエラーを分析してください：[エラーログ]
> nullチェックを追加してください
> オプショナルチェイニングを使用してください
```

### SyntaxError: Unexpected token

```bash
# 原因: 構文エラーまたはJSONパースエラー
# 対処:
> このファイルの構文エラーを修正してください
> JSONの形式を検証してください
```

### ENOSPC: System limit for number of file watchers

```bash
# Linux/WSL での対処
echo fs.inotify.max_user_watches=524288 | sudo tee -a /etc/sysctl.conf
sudo sysctl -p
```

## セッション関連の問題

### セッションが復元できない

```bash
# キャッシュクリア
rm -rf ~/.claude/cache
rm -rf ~/.claude/sessions

# 新規セッション開始
claude --fresh
```

### コマンドが認識されない

```bash
# PATHの確認
echo $PATH

# 手動でPATHに追加
export PATH="$PATH:$(npm config get prefix)/bin"

# .bashrc/.zshrc に追加
echo 'export PATH="$PATH:$(npm config get prefix)/bin"' >> ~/.zshrc
source ~/.zshrc
```

## パフォーマンス問題

### 処理が遅い

```bash
# 1. モデルを軽量版に切り替え
/model haiku

# 2. コンテキストをクリア
/clear

# 3. ファイルを分割
> 大きなファイルを100行ずつ処理してください
```

### メモリ不足

```bash
# Node.js のメモリ制限を増やす
export NODE_OPTIONS="--max-old-space-size=8192"

# または起動時に指定
NODE_OPTIONS="--max-old-space-size=8192" claude
```

## ネットワーク関連

### プロキシ設定

```bash
# HTTP/HTTPS プロキシ
export HTTP_PROXY=http://proxy.example.com:8080
export HTTPS_PROXY=http://proxy.example.com:8080

# 認証が必要な場合
export HTTP_PROXY=http://username:password@proxy.example.com:8080
```

### SSL証明書エラー

```bash
# 企業ネットワークでの対処
export NODE_TLS_REJECT_UNAUTHORIZED=0  # 開発環境のみ

# 証明書を追加
export NODE_EXTRA_CA_CERTS=/path/to/certificate.pem
```

## デバッグモード

```bash
# 詳細ログを有効化
export CLAUDE_DEBUG=1
claude

# ログファイルに出力
claude 2>&1 | tee claude-debug.log
```

## よくある質問（FAQ）

### Q: Claude Code が起動しない
```bash
# A: 以下を順に試す
1. npm cache clean --force
2. npm uninstall -g @anthropic-ai/claude-code
3. npm install -g @anthropic-ai/claude-code@latest
4. claude doctor
```

### Q: ファイルの変更が反映されない
```bash
# A: ファイルシステムの監視をリセット
> ファイルを再読み込みしてください
/refresh
```

### Q: コマンドが実行できない
```bash
# A: シェル設定を確認
echo $SHELL
which claude
# パスが表示されない場合は再インストール
```

## エラー報告の方法

```bash
# 1. 詳細情報を収集
claude doctor > claude-doctor.txt
claude --version >> claude-doctor.txt
node --version >> claude-doctor.txt
npm --version >> claude-doctor.txt

# 2. GitHubでIssue作成
# https://github.com/anthropics/claude-code/issues

# 3. 含めるべき情報
- エラーメッセージ全文
- 実行したコマンド
- OS とバージョン
- Node.js/npm バージョン
- claude doctor の出力
```

## 緊急時の対処法

### 完全リセット手順

```bash
# 1. 全データをバックアップ
cp -r ~/.claude ~/.claude.backup

# 2. クリーンアンインストール
npm uninstall -g @anthropic-ai/claude-code
rm -rf ~/.claude
rm -rf ~/.npm/_cacache

# 3. 再インストール
npm install -g @anthropic-ai/claude-code@latest

# 4. 設定を復元
claude login
```

## 次のステップ

- [自動化テクニック](./08-automation.md)
- [チーム開発](./09-team-development.md)
- [実例集](./10-case-studies.md)

## 参考文献

### 公式サポート
- [Claude Code Troubleshooting](https://docs.anthropic.com/en/docs/claude-code/troubleshooting) - 公式トラブルシューティング
- [GitHub Issues](https://github.com/anthropics/claude-code/issues) - 公式Issueトラッカー
- [Claude Support](https://support.anthropic.com/) - Anthropicサポート

### プラットフォーム別ガイド
- [macOS Setup Guide](https://docs.anthropic.com/en/docs/claude-code/installation#macos) - macOSセットアップ
- [Windows/WSL Guide](https://docs.anthropic.com/en/docs/claude-code/installation#windows) - Windowsセットアップ
- [Linux Installation](https://docs.anthropic.com/en/docs/claude-code/installation#linux) - Linuxインストール

### Node.js関連
- [Node.js Documentation](https://nodejs.org/docs/) - Node.js公式ドキュメント
- [npm Documentation](https://docs.npmjs.com/) - npm公式ドキュメント
- [nvm Documentation](https://github.com/nvm-sh/nvm) - Nodeバージョン管理

### ネットワーク設定
- [Corporate Proxy Setup](https://docs.anthropic.com/en/docs/claude-code/corporate-proxy) - 企業プロキシ設定
- [SSL/TLS Configuration](https://nodejs.org/api/tls.html) - Node.js TLS設定

### コミュニティリソース
- [Stack Overflow Claude Tag](https://stackoverflow.com/questions/tagged/claude-ai) - Stack Overflow Q&A
- [Reddit r/ClaudeAI](https://reddit.com/r/ClaudeAI) - Redditコミュニティ
- [Discord Community](https://discord.gg/claude) - Discordコミュニティ