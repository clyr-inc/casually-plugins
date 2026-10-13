# Casually for Codex and Claude Code

Casuallyの共有プロジェクト、キャンペーンLP・メール、素材生成、承認、実績確認をAIから操作するプラグインです。

## インストール

Node.js 20以上と、選んだアプリのプラグイン対応CLIを用意してください。

```sh
# Claude Code
claude plugin marketplace add https://github.com/clyr-inc/casually-plugins.git
claude plugin install casually@casually-plugins

# Codex
codex plugin marketplace add https://github.com/clyr-inc/casually-plugins.git
codex plugin add casually@casually-plugins
```

ビルド済みの接続プログラムを同梱しています。npm installやCasually本体のソースコードは不要です。

## 接続とstgでの利用

CasuallyのホームまたはAPIキー発行画面から「Codex・Claude Codeを接続」を開き、3ステップの接続ガイドに従ってください。

1. 管理者が「AI エージェント（MCP）」用キーを発行します。使う環境のキーを使用してください。
2. アプリのCasually setupスキルにAPIホストを伝えます。Claude Codeでは `/casually:setup` で開けます。
3. スキルが案内する `scripts/setup.cjs --host <APIホスト>` をターミナルで実行し、キーを非表示入力へ入れます。チャットにキーを貼る必要はありません。

stgの接続は `--host https://<stgのAPIホスト> --profile stg`、保存済み設定の確認は `--check --profile stg` を使用します。本番とstgのキーを別々に保存でき、次のようにアプリを起動して切り替えます。

```sh
CASUALLY_MCP_PROFILE=stg codex
CASUALLY_MCP_PROFILE=stg claude
```

名前付きプロファイルと、URL/キー環境変数・別の資格情報ファイルの併用は拒否します。ホストを変更する際は、その環境のキーを設定してください。

## できること

`setup`、`project`、`campaign`、`landing-page`、`creative`、`report` の6スキルを含みます。「CasuallyでキャンペーンのLPとメールを作って」「このプロジェクトの実績を確認して」と依頼できます。

生成はCasuallyのクレジットを使用します。権限・契約プランの制限、人間による承認、公開・一斉配信の確認はCasually側で適用されます。AIから自己承認はできません。

[詳しい接続手順](plugins/casually/README.md)。同梱ライブラリのライセンスは [THIRD_PARTY_NOTICES](plugins/casually/THIRD_PARTY_NOTICES.txt) に記載しています。
