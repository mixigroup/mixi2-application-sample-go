# mixi2 Application Sample for Go

[mixi2 Application SDK for Go](https://github.com/mixigroup/mixi2-application-sdk-go) を使用したサンプルアプリケーションです。

## 必要条件

- Go 1.24 以上

## セットアップ

```bash
cp .env.example .env
# .env を編集して環境変数を設定
source .env
go mod tidy
```

## 環境変数

| 変数名 | 必須 | 説明 |
|--------|------|------|
| `CLIENT_ID` | ○ | OAuth2 クライアントID |
| `CLIENT_SECRET` | ○ | OAuth2 クライアントシークレット |
| `TOKEN_URL` | ○ | トークンエンドポイント URL |
| `API_ADDRESS` | ○ | API サーバーアドレス |
| `STREAM_ADDRESS` | △ | Stream サーバーアドレス ※ gRPC ストリーミングのみ必須 |
| `SIGNATURE_PUBLIC_KEY` | △ | イベント署名検証用の公開鍵（Base64）※ webhook のみ必須 |
| `PORT` | | Webhook サーバーポート（デフォルト: `8080`） |

## 実行方法

### gRPC ストリーミング

gRPC ストリーミングでイベントを受信します。

```bash
source .env
go run cmd/stream/main.go
```

### HTTP Webhook

HTTP エンドポイントでイベントを受信します。App Runner や Cloud Run での実行を想定しています。

```bash
source .env
go run cmd/webhook/main.go
```

エンドポイント:
- `POST /events`: イベント受信
- `GET /healthz`: ヘルスチェック

### Vercel（サーバーレス）

[Vercel](https://vercel.com) にデプロイしてサーバーレス関数としてイベントを受信します。

```bash
# Vercel CLI のインストール
npm i -g vercel

# Vercel にログイン・プロジェクトをリンク
vercel link

# 環境変数を設定
vercel env add CLIENT_ID
vercel env add CLIENT_SECRET
vercel env add TOKEN_URL
vercel env add API_ADDRESS
vercel env add SIGNATURE_PUBLIC_KEY

# Vercel へデプロイ
vercel deploy --prod
```

## セキュリティ

- `CLIENT_SECRET` は環境変数やシークレット管理システムから読み込んでください。ソースコードにハードコードしないでください。
- イベント署名は Ed25519 で検証されます。
- タイムスタンプ検証によりリプレイ攻撃を防止します（5分間のウィンドウ）。

## 関連ドキュメント

- **セキュリティポリシー**: [SECURITY.md](SECURITY.md)
- **コントリビューション方針**: [CONTRIBUTING.md](CONTRIBUTING.md)

本リポジトリは mixi2 チームが管理しています。外部からの Pull Request は受け付けていません。
バグ報告やフィードバックは Issue でご報告ください。詳しくは [CONTRIBUTING.md](CONTRIBUTING.md) を参照してください。

## ライセンス

[LICENSE](LICENSE) を参照してください。
