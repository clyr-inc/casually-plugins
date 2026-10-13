# Casually for Codex and Claude Code

Casuallyのプロジェクト、キャンペーンLP・メール、素材生成、承認、実績確認をAIから操作するプラグインです。

## インストール

選んだアプリのプラグイン対応CLIで実行し、アプリを再起動します。

```sh
# Claude Code
claude plugin marketplace add https://github.com/clyr-inc/casually-plugins.git
claude plugin install casually@casually-plugins

# Codex
codex plugin marketplace add https://github.com/clyr-inc/casually-plugins.git
codex plugin add casually@casually-plugins
```

プラグインは共有スキルとHTTP MCPの接続設定を含みます。Casually本体のソース、資格情報、実行スクリプトは含みません。

## Webで認証

1. Claude Codeの `/mcp` からCasuallyの「Authenticate」を選択します。CodexではMCP設定のCasuallyからログインします。
2. ブラウザでCasuallyにログインし、アカウント・接続先・戻り先・許可する操作を確認して「接続を許可」を押します。
3. アプリに戻ると接続完了になります。認証情報は各クライアントが保存・更新します。APIキーの発行や貼り付けは不要です。

本番の接続先は `https://api.casually.marketing/api/v2/mcp` です。Casuallyのホームから `/dashboard/plugins` の接続ガイドを開けます。許可した接続もそこで解除できます。

**v0.3.0はAPIとWebのMCP OAuth対応が必要です。** 未対応の環境では接続できません。古いAPIキー方式への自動切り替えは行いません。

## stg・ローカルのホスト指定

Casuallyの接続ガイドでAPIホストと接続名を指定すると、その環境用のコマンドをコピーできます。次はstgの例です。実際のAPIホストに置き換えてください。

```sh
claude mcp add --transport http --scope user casually-stg https://STAGING_API_HOST/api/v2/mcp
codex mcp add casually-stg --url https://STAGING_API_HOST/api/v2/mcp
```

`casually-stg` をMCP画面から認証します。手動登録したCodexの接続は `codex mcp login casually-stg` でもログインできます。stg作業ではプラグイン内の本番用CasuallyをMCP設定で無効にしてください。接続名とホストを確認してから操作します。認証は選んだ環境に限定されます。

HTTPSを使用してください。ローカル開発では `http://localhost:3001/api/v2/mcp` などのHTTPも使えます。ホストを切り替えるときは、別名のMCP接続を登録し、その環境のWebで認証します。

Claude Desktop／claude.aiのカスタムコネクタにも、同じHTTPSのMCP URLを登録してWebで認証できます。上記のplugin installコマンドはClaude CodeとCodex用です。

## 使い方

`setup`、`project`、`campaign`、`landing-page`、`creative`、`report` の6スキルを含みます。Claude Codeでは `/casually:setup` や `/casually:campaign`、Codexではスキル一覧または自然文から利用できます。

- 「CasuallyでキャンペーンのLPとメールを作って」
- 「保存済みLPの見出しを修正して」
- 「このプロジェクトの実績を確認して」

生成にはCasuallyのクレジットを使います。操作はログイン中のメンバー権限と契約プランに従い、公開・一斉配信にはCasuallyに保存された人間の承認が必要です。プラグインからの自己承認はできません。

仕様: [Claude MCP認証](https://code.claude.com/docs/en/mcp#authenticate-with-remote-mcp-servers)、[Codex MCP](https://developers.openai.com/codex/mcp)、[MCP認証仕様](https://modelcontextprotocol.io/specification/2025-11-25/basic/authorization)。
