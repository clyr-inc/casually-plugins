# Casually plugin for Codex and Claude Code

Casuallyの共有プロジェクト、キャンペーンLP・メール、素材生成、承認、実績をCodexとClaude Codeから操作するプラグインです。両方のクライアントで同じMCPと6つのスキルを使用します。

| スキル | 用途 |
| --- | --- |
| `setup` | アカウント接続と読み取りによる接続確認 |
| `project` | プロジェクト、サービスLP、ブランド、フォームの準備 |
| `campaign` | LP・メール作成、プレビュー、承認依頼、承認済み施策の実行 |
| `landing-page` | 保存済みLPのプレビューと修正 |
| `creative` | 見積もり付きの画像・動画生成と下書きLPへの配置 |
| `report` | 実際の計測値による実績確認とレポート保存 |

## 導入

Node.js 20以上と、対応するプラグイン機能を備えたCodexまたはClaude Codeが必要です。配布用フォルダには接続プログラムと依存ライブラリを同梱しており、利用者側でnpm installする必要はありません。Claude Desktopの拡張形式はこのパッケージの対象外です。

通常の導入では、配布用リポジトリを登録してインストールします。

```sh
# Claude Code
claude plugin marketplace add https://github.com/clyr-inc/casually-plugins.git
claude plugin install casually@casually-plugins

# Codex
codex plugin marketplace add https://github.com/clyr-inc/casually-plugins.git
codex plugin add casually@casually-plugins
```

配布元はプラグイン専用です。Casuallyのアプリケーションのソースや資格情報は含めません。stg・本番とも同じプラグインを使い、接続先はセットアップのHost指定で切り替えます。Webのホーム、APIキー発行後、`/dashboard/plugins` から同じ接続ガイドを開けます。

開発元のリポジトリでは、パッケージをビルドできます。

```sh
cd casually/plugins
npm ci
npm run build
npm test
npm run validate:clients
```

`casually/output/assistant-plugins` が独立した配布用フォルダです。このフォルダを任意の場所に置き、そのフォルダをマーケットプレイスとして登録します。以下はリポジトリのルートから実行する例です。

ビルドは配布対象のファイルだけをコピーし、接続キー・設定済みのアカウント情報・node_modulesを含めません。同梱ライブラリのライセンスは `THIRD_PARTY_NOTICES.txt` に記載します。接続・セットアップのテスト、型検査、両クライアントの検証は開発時に実行します。ネイティブ検証に必要なCLIがない場合は、そのクライアントの検証をスキップしたと表示します。

Codexではマーケットプレイスを追加し、プラグイン画面のCasuallyからインストールします。`plugin add`に対応するCLIでは次のコマンドでも導入できます。

```sh
codex plugin marketplace add ./casually/output/assistant-plugins
codex plugin add casually@casually-plugins
```

Claude Codeでは次のコマンドを使います。

```sh
claude plugin marketplace add ./casually/output/assistant-plugins
claude plugin install casually@casually-plugins
```

リポジトリ内で開発する場合、ルートの `.agents/plugins/marketplace.json` と `.claude-plugin/marketplace.json` から `casually/plugins/casually` を参照できます。プラグインを有効にする設定は利用者側で選びます。

## Casuallyへの接続

1. CasuallyのAPIキー画面で、管理者が「AI エージェント（MCP）」用キーを発行します。キーの接続先URLを使います。
2. キャンペーン業務には `orchestration`、プロジェクト操作と素材生成には `data` と `action` も付けます。キーとは別に、現在のメンバーの権限・契約プランで操作が制限されます。
3. 次のコマンドを利用者自身のターミナルで実行し、非表示の入力欄にキーを入力します。実際の接続先に置き換えてください。

```sh
node casually/output/assistant-plugins/plugins/casually/scripts/setup.cjs --url http://localhost:3001/api/v2/mcp
```

接続先は公開HTTPS、またはlocalhostのHTTPに対応します。セットアップはツール一覧だけを取得し、確認に成功した場合に `~/.config/casually/mcp.json` を保存します。キーはチャットやコマンド引数へ貼り付けません。macOS/Linuxではディレクトリ700・ファイル600を使用します。Windowsでは専用ユーザーのプロファイル内に保管し、ユーザー本人に限定したACLを設定してください。

保存済みキーの更新には `--replace`、接続確認だけなら `--check` を指定します。既存ファイルは自動で上書きしません。

```sh
node casually/output/assistant-plugins/plugins/casually/scripts/setup.cjs --check
```

その後、クライアントのMCP接続を再読み込みします。共通の専用ファイルを使用するので、同じPCのCodexとClaude Codeは同じ接続を使用できます。別アカウントを使う場合は、クライアント起動時の `CASUALLY_MCP_CREDENTIALS_FILE` に別のファイルの絶対パスを設定します。設定にキー自体は記載しません。

### stg・本番のホスト切り替え

インストールしたアプリでCasuallyのsetupスキルを開き、使用するAPIホストとプロファイルを指定します。スキルが案内するインストール先の `scripts/setup.cjs` を実行します。開発用パッケージでは以下の形です。例のstgホストは実際のAPIホストに置き換えてください。

```sh
node casually/output/assistant-plugins/plugins/casually/scripts/setup.cjs --host https://stg-api.example.com --profile stg
node casually/output/assistant-plugins/plugins/casually/scripts/setup.cjs --check --profile stg

CASUALLY_MCP_PROFILE=stg codex
CASUALLY_MCP_PROFILE=stg claude
```

`--host` はAPIのホスト名またはoriginを受け取り、`/api/v2/mcp` を補います。`--url` との併用はできません。ホストの変更時には、その環境で発行したキーを非表示入力で設定します。保存済みの本番キーを別ホストへ転送する切り替えは行いません。

`default` は従来の `~/.config/casually/mcp.json`、`stg` は `~/.config/casually/profiles/stg.json` に保存します。名前付きプロファイルは英小文字から始め、数字とハイフンを含められます。クライアント起動後に変更した場合は、アプリとMCP接続を再起動してください。

環境変数による接続も使用できます。`CASUALLY_MCP_URL` と `CASUALLY_MCP_API_KEY` の両方をクライアント起動時に渡してください。このモードはファイルより優先し、片方だけの場合は接続を拒否します。通常は専用ファイルでの接続を推奨します。

名前付きプロファイルと、明示的な資格情報ファイル・URL/キー環境変数は併用できません。stgを選んだのに既存の本番用環境変数を使う、といった曖昧な指定では接続を止めます。`--check --profile stg` は環境変数のキーではなく、そのプロファイルの保存内容を確認します。

## 使い方

Claude Codeでは `/casually:campaign`、`/casually:project`、`/casually:report` などで呼び出せます。Codexではスキル一覧からCasuallyの対応スキルを選ぶか、自然文でCasuallyへの操作を依頼します。

- 「CasuallyでこのプロジェクトのキャンペーンLPとメールを作成して」
- 「保存済みLPの見出しとCTAを修正して」
- 「Casuallyの今月の施策実績を確認して」

ツール一覧・スキーマ・権限はサーバーから取得します。生成はCasuallyクレジットを使用し、提供元のAPIキーは不要です。LP公開と一斉配信はCasuallyに保存された人間の承認後に実行します。修正後は再承認が必要です。プラグインからの自己承認はできません。

## 接続できない場合

- `--check` で、接続先・MCP用キー・スコープ・バックエンド稼働を確認します。
- ファイルの権限と所有者を確認します。シンボリックリンクや公開HTTPの接続先は拒否します。
- キーを変更したらMCP接続を再読み込みします。生成などの長いツールにはクライアント側でも600秒のタイムアウトを設定します。
- タイムアウト後は保存済みの業務・生成IDから状態を確認します。プラグインは書き込みを自動で再送しません。

公開ディレクトリへの申請、Claude Desktop用拡張、OAuthログインの追加はこのパッケージには含めません。Gitの配布カタログから各クライアントの標準コマンドで導入し、既存のAPIキー認証で接続します。

仕様の参照: [Codex plugin packaging](https://developers.openai.com/plugins/build/plugins)、[Claude Code plugin manifest](https://code.claude.com/docs/en/plugins-reference)、[Claude Code marketplaces](https://code.claude.com/docs/en/plugin-marketplaces)。
