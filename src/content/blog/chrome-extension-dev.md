---
title: 'Chrome 拡張機能を作ってみよう'
description: 'はじめての Chrome 拡張機能開発。ChatGPT Ctrl+Enter Sender の開発経験から学んだことを共有します。'
pubDate: 'Oct 20 2024'
---

Chrome 拡張機能は、ブラウザの機能を拡張する強力なツールです。今回は、100人以上のユーザーに使っていただいている「ChatGPT Ctrl+Enter Sender」の開発経験をもとに、Chrome 拡張機能の作り方を紹介します。

## Chrome 拡張機能とは

Chrome 拡張機能は、Google Chrome ブラウザに追加機能を提供する小さなプログラムです。HTML、CSS、JavaScript という Web 技術で開発できるため、Web 開発者にとって取り組みやすい領域です。

## 基本構成

Chrome 拡張機能は主に以下のファイルで構成されます：

```
my-extension/
├── manifest.json    # 拡張機能の設定ファイル
├── background.js    # バックグラウンドで動作するスクリプト
├── content.js       # Web ページに注入されるスクリプト
├── popup.html       # ポップアップ UI
└── icons/           # アイコン画像
```

### manifest.json

拡張機能の心臓部とも言えるファイルです：

```json
{
  "manifest_version": 3,
  "name": "My Extension",
  "version": "1.0.0",
  "description": "説明文をここに",
  "permissions": ["activeTab"],
  "content_scripts": [
    {
      "matches": ["https://*.example.com/*"],
      "js": ["content.js"]
    }
  ],
  "action": {
    "default_popup": "popup.html",
    "default_icon": "icons/icon48.png"
  }
}
```

## ChatGPT Ctrl+Enter Sender の仕組み

この拡張機能は、ChatGPT のページで `Ctrl+Enter` キーを押すとメッセージを送信できるようにするものです。

### Content Script

```javascript
document.addEventListener('keydown', (e) => {
  if (e.ctrlKey && e.key === 'Enter') {
    const sendButton = document.querySelector('[data-testid="send-button"]');
    if (sendButton) {
      sendButton.click();
    }
  }
});
```

シンプルですが、これだけで多くのユーザーの生産性を向上させることができました。

## 開発のコツ

### 1. 小さく始める

最初から完璧を目指さず、まずは動くものを作りましょう。

### 2. ユーザーの声を聞く

Chrome Web Store のレビューやフィードバックは貴重な情報源です。

### 3. Manifest V3 に対応する

古い Manifest V2 は非推奨になっているため、新規開発では V3 を使用しましょう。

## まとめ

Chrome 拡張機能開発は、アイデア次第で多くの人の役に立つツールを作ることができます。まずは身近な不便を解消する小さな拡張機能から始めてみてはいかがでしょうか。

リポジトリ: [ChatGPT-Ctrl-Enter-Sender](https://github.com/masachika-kamada/ChatGPT-Ctrl-Enter-Sender)
