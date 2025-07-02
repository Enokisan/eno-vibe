# 🍀 運まかせ決定マシン - ENO VIBE

あなたの代わりに運が何でも決めてくれる、楽しいWebアプリケーションです！

## 🎯 機能

- **今日の運勢** 🔮 - 一日の運勢を占います
- **晩ご飯決め** 🍽️ - 今日の夕食メニューをランダムに決定
- **Yes/No判定** ⚖️ - 迷った時の二択判定
- **ラッキーカラー** 🌈 - 今日のラッキーカラーを選択
- **今日やること** ⚡ - おすすめの活動を提案
- **カスタム選択** 🎲 - 自分で選択肢を入力して運に任せる

## 🚀 技術スタック

- **フロントエンド**: Vue.js 3 + Vite
- **デプロイ**: Vercel
- **CI/CD**: GitHub Actions
- **言語**: JavaScript (ES6+)

## 📦 セットアップ

### 前提条件
- Node.js (v18以上)
- npm

### ローカル開発

1. リポジトリをクローン
```bash
git clone <リポジトリURL>
cd eno-vibe
```

2. 依存関係をインストール
```bash
npm install
```

3. 開発サーバーを起動
```bash
npm run dev
```

4. ブラウザで http://localhost:5173 を開く

### ビルド

本番用ビルドを作成:
```bash
npm run build
```

ビルドをプレビュー:
```bash
npm run preview
```

## 🎨 カスタマイズ

### 新しい選択肢を追加

`src/components/LuckyDecision.vue` の `data()` 内で配列を編集できます:

```javascript
// 食べ物の選択肢を追加
foods: [
  '寿司', 'ラーメン', '新しい料理' // ← ここに追加
]

// 新しいカテゴリを追加
categories: [
  {
    id: 'new-category',
    name: '新カテゴリ',
    icon: '🎭',
    description: '新しい選択肢です'
  }
]
```

### スタイルのカスタマイズ

背景やカラーテーマは `index.html` と `LuckyDecision.vue` の `<style>` セクションで変更できます。

## 📱 レスポンシブ対応

モバイル、タブレット、デスクトップに対応済みです。

## 🤝 コントリビューション

1. Fork the project
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📄 ライセンス

このプロジェクトはMITライセンスの下で公開されています。

---

運に任せて、楽しい一日を過ごしてください！ 🍀✨
