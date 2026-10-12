# YouTube Queue (minimal)

YouTube URL を Ctrl+V でキューへ追加し、YouTube 公式埋め込みプレイヤーで順番に再生する最小版です。

## 使い方

1. `index.html` を GitHub リポジトリのルートへ置く
2. GitHub Pages を有効化
3. 公開URLを開く
4. YouTube 動画URLをコピーしてページ上で `Ctrl+V`
5. oEmbed でタイトルを取得しキューへ追加
6. 「再生」または項目をダブルクリック
7. 動画終了時は次へ自動で進む

## 主な仕様

- APIキー不要
- YouTube Data API 不使用
- タイトル取得: YouTube oEmbed
- 再生: YouTube IFrame Player API
- キュー保存: localStorage
- Ctrl+V追加
- 右クリック → クリップボードから追加
- 複数URLを改行区切りで一括追加
- 上下移動 / 削除 / 全消去
- 再生終了で自動次送り

## 注意

`file://` で直接開くと YouTube 埋め込みプレイヤーがエラー153になることがあります。GitHub Pages など HTTPS 上での使用を前提にしています。
