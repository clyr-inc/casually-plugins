# Casually for Codex and Claude Code

Casuallyのプロジェクト、キャンペーンLP・メール、素材生成、承認、実績確認をAIから操作するプラグインです。

## インストール

Claude Codeのデスクトップ画面では、次の順に操作します。

1. 入力欄の「＋」からプラグインのメニューを開き、**Manage Plugins** を選びます。
2. **Marketplaces** タブで `https://github.com/clyr-inc/casually-plugins.git` を入力し、**Add** を押します。
3. **Plugins** タブに戻り、**Casually** の **Install** を押します。
4. 新しい会話を開きます。

Codexではプラグイン画面からマーケットプレイスを追加し、Casuallyをインストールします。CLIで導入する場合は次のコマンドも使えます。

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
3. アプリに戻ると接続完了になります。認証情報は各クライアントが保存・更新します。MCP用APIキーは発行できません。以前のMCPキーも接続には使えません。自社システム向けのREST APIキーは引き続き使えます。

本番の接続先は `https://api.casually.marketing/api/v2/mcp` です。Casuallyのホームから `/dashboard/plugins` の接続ガイドを開けます。許可した接続もそこで解除できます。

**v0.3.2以降はAPIとWebのMCP OAuth対応が必要です。** 未対応の環境では接続できません。古いAPIキー方式への自動切り替えは行いません。

## stg・ローカルのホスト指定

Casuallyの `/dashboard/plugins` は、現在の接続先に合わせて本番・stg・ローカルの説明、接続先、接続名を自動表示します。通常の手順でホストを入力する必要はありません。本番はインストールしたプラグインをそのまま使います。stg・ローカルは、検証担当者がガイド内の入力済みコマンドを初回に一度実行します。別のホストを使う場合だけ「別の接続先を指定する（開発者向け）」を開きます。次はstgの登録例です。

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

事業（商材・ブランド）ごとにデータを分けている組織では、AIが作業の前に事業を確かめ、決まっていなければ事業名で尋ねます。作業フォルダーで「このフォルダーを Casually の事業「〇〇」に紐付けて」と頼むと、AIが `.casually/workspace.json` を作り、以後そのフォルダーでの作業はその事業で行います（v0.3.3から。Casuallyの「表示中の事業 → AIエージェントでこの事業を使う」に頼み方と手動の設定があります）。

生成にはCasuallyのクレジットを使います。操作はログイン中のメンバー権限と契約プランに従い、公開・一斉配信にはCasuallyに保存された人間の承認が必要です。プラグインからの自己承認はできません。

仕様: [Claude MCP認証](https://code.claude.com/docs/en/mcp#authenticate-with-remote-mcp-servers)、[Codex MCP](https://developers.openai.com/codex/mcp)、[MCP認証仕様](https://modelcontextprotocol.io/specification/2025-11-25/basic/authorization)。

## 開発元での検証

```sh
cd casually/plugins
npm ci
npm run build
npm test
npm run typecheck
npm run validate:clients
npm run security:check
```

`casually/output/assistant-plugins` が独立した配布用フォルダです。ビルドは配布対象だけをコピーします。OAuthのAPI・Web実装と運用手順は、本体の `docs/MCP_OAUTH_SETUP.md` に記載します。
