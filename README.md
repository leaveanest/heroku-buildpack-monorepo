# heroku-buildpack-monorepo

モノレポから Heroku アプリをデプロイするための Buildpack。

指定したサブディレクトリをビルドルートに昇格させ、後続の Buildpack（PHP, Node.js 等）が単独アプリとして認識できるようにする。

## 使い方

### 1. Buildpack を追加（チェーンの先頭に）

```bash
heroku buildpacks:add -i 1 https://github.com/leaveanest/heroku-buildpack-monorepo --app your-app
```

### 2. `APP_BASE` にアプリのサブディレクトリを設定

```bash
heroku config:set APP_BASE=apps/api --app your-app
```

### 3. デプロイ

```bash
git push heroku main
```

## モノレポ構成例

```
my-monorepo/
  apps/
    api/           -> Heroku App A  (APP_BASE=apps/api)
    worker/        -> Heroku App B  (APP_BASE=apps/worker)
    admin/         -> Heroku App C  (APP_BASE=apps/admin)
```

## 仕組み

1. **detect** - `APP_BASE` が設定されているか確認
2. **compile** - サブディレクトリをステージング領域にコピーし、ビルドルートを置き換え
3. 後続の Buildpack が昇格されたディレクトリを単独リポジトリとして処理

## ライセンス

Apache License 2.0
