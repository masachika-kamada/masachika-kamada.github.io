---
title: 'Azure Functions 入門：サーバーレスで始める開発'
description: 'Azure Functions を使ったサーバーレスアプリケーション開発の基礎を解説します。'
pubDate: 'Sep 10 2024'
---

サーバーレスコンピューティングは、インフラ管理の手間を省き、コードの実行に集中できる魅力的な選択肢です。今回は Azure Functions を使ったサーバーレス開発について紹介します。

## Azure Functions とは

Azure Functions は、イベント駆動型のサーバーレスコンピューティングプラットフォームです。サーバーの管理を気にすることなく、必要なときに必要な分だけコードを実行できます。

### メリット

- **コスト効率**: 使った分だけ課金
- **自動スケーリング**: 負荷に応じて自動で拡張
- **多言語対応**: Python, JavaScript, C#, Java など
- **豊富なトリガー**: HTTP, Timer, Queue, Blob など

## はじめての Azure Function

### Python で HTTP トリガーを作成

```python
import azure.functions as func
import json

def main(req: func.HttpRequest) -> func.HttpResponse:
    name = req.params.get('name')
    
    if not name:
        try:
            req_body = req.get_json()
            name = req_body.get('name')
        except ValueError:
            pass
    
    if name:
        return func.HttpResponse(
            json.dumps({"message": f"Hello, {name}!"}),
            mimetype="application/json"
        )
    else:
        return func.HttpResponse(
            json.dumps({"message": "Hello, World!"}),
            mimetype="application/json"
        )
```

### 設定ファイル (function.json)

```json
{
  "scriptFile": "__init__.py",
  "bindings": [
    {
      "authLevel": "anonymous",
      "type": "httpTrigger",
      "direction": "in",
      "name": "req",
      "methods": ["get", "post"]
    },
    {
      "type": "http",
      "direction": "out",
      "name": "$return"
    }
  ]
}
```

## 実践的なユースケース

### 1. 定期実行タスク

Timer トリガーを使って、毎日決まった時刻にデータを集計したり、レポートを生成したりできます。

### 2. Webhook 受信

外部サービスからの通知を受け取り、処理を実行できます。

### 3. ファイル処理

Blob Storage にファイルがアップロードされたら自動的に処理を開始できます。

## ローカル開発環境

Azure Functions Core Tools を使えば、ローカルで開発・テストができます：

```bash
# インストール
npm install -g azure-functions-core-tools@4

# プロジェクト作成
func init MyFunctionApp --python

# 関数追加
func new --name HttpExample --template "HTTP trigger"

# ローカル実行
func start
```

## まとめ

Azure Functions は、小さなタスクから大規模なアプリケーションまで、幅広いユースケースに対応できます。まずは簡単な HTTP トリガーから始めて、徐々に機能を拡張していくのがおすすめです。
