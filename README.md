# docker-okubo-practice

## MySQL サーバ コンテナ

公式 `mysql:8.4` イメージを用いた学習／検証用の MySQL サーバ環境です。永続化用に名前付きボリュームを使用します。

### 環境変数とユーザ

| 目的                  | 変数                  | 値     |
| --------------------- | --------------------- | ------ |
| root パスワード       | `MYSQL_ROOT_PASSWORD` | okubo  |
| 初期データベース      | `MYSQL_DATABASE`      | sample |
| 一般ユーザ            | `MYSQL_USER`          | admin  |
| 一般ユーザ パスワード | `MYSQL_PASSWORD`      | okubo  |

### 起動

```powershell
docker compose up -d
```

初回起動でデータディレクトリが初期化され、ユーザと DB が作成されます。

### 動作確認 (mysql クライアント)

ローカルに MySQL クライアントがある場合:

```powershell
mysql -h 127.0.0.1 -P 3306 -u admin -pokubo sample
```

root で接続する場合:

```powershell
mysql -h 127.0.0.1 -P 3306 -u root -pokubo
```

Docker 内の mysql クライアントを使う (クライアントがホストに無い場合):

```powershell
docker exec -it mysql-server mysql -u admin -pokubo sample
```

### 終了 / クリーンアップ

コンテナ停止 (データは volume に残る):

```powershell
docker compose down
```

データも含め完全削除する場合:

```powershell
docker compose down -v
```

### ファイル構成

```
docker-compose.yml
```

### 注意事項

- パスワードは学習用の簡易設定です。実運用では強固な値へ変更してください。
- `--default-authentication-plugin=mysql_native_password` を指定し、古いクライアント互換性を確保しています。不要なら削除可能です。

---
必要に応じて phpMyAdmin 追加やレプリケーション構成など拡張してください。