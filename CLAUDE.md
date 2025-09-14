# CLAUDE.md

このファイルは、リポジトリ内のコードを扱う際に Claude Code (claude.ai/code) へのガイダンスを提供します。

## プロジェクト概要

これは Firebase を利用したリアルタイムチャットアプリケーションで、バニラ HTML、CSS、JavaScript を用いたシングルページアプリケーションとして構築されています。ユーザー認証、リアルタイムメッセージング、ファイルアップロード、自動位置情報付加、インタラクティブな地図表示機能を備えています。すべてのメッセージに自動的に位置情報が付加され、地図上でメッセージの詳細をポップアップ表示できます。

## アーキテクチャ

* **シングルページアプリケーション**: HTML構造とJavaScript、外部CSSファイルで構成されています
* **Firebase バックエンド**: Firebase Authentication、Firestore（メッセージ用）、Storage（ファイルアップロード用）を使用
* **リアルタイム更新**: Firestore の onSnapshot リスナーでメッセージをリアルタイム同期
* **自動位置情報取得**: 全メッセージ送信時に GPS 位置情報を自動取得・付加
* **インタラクティブ地図**: MapLibre GL JS を使用した高機能地図表示
* **地図スタイル切り替え**: 4種類の地図（OSM、地理院地図、航空写真、ベクタ地図）をドロップダウンで選択
* **主題図オーバーレイ**: 洪水浸水想定区域などの主題図を地図上にオーバーレイ表示
* **ファイル処理**: プレビュー機能付きの複数ファイルアップロードに対応

## 主なコンポーネント

* **認証**: Firebase Auth を使用したメール/パスワードによるログイン・サインアップ
* **メッセージング**: テキストとファイルをサポートするリアルタイムチャット（全メッセージに自動位置情報付加）
* **ファイルアップロード**: 複数ファイル選択およびドラッグ＆ドロップ対応、画像プレビュー機能
* **自動位置情報取得**: すべてのメッセージ送信時に GPS 位置情報を自動取得・表示
* **インタラクティブ地図**: メッセージをクリック可能ピンで表示、詳細ポップアップ機能
* **地図スタイル切り替え**: 4種類の地図スタイル（OSM、地理院地図、航空写真、ベクタ地図）をドロップダウンで選択
* **主題図オーバーレイ**: 洪水・内水浸水想定区域レイヤーをチェックボックスで表示/非表示切り替え
* **座標表示**: メッセージ内で緯度・経度を数値表示（小数点以下6桁精度）

## 設定

* **Firebase 設定**: `env.js` に Firebase プロジェクト設定を記述
* **環境変数**: `window.ENV` オブジェクトを介して公開
* **デバッグモード**: `ENV.DEBUG_MODE` フラグで制御
* **ファイル制限**: `env.js` 内で `MAX_FILE_SIZE`（10MB）および `MAX_FILES_COUNT`（5）を設定

## ファイル構成

* `index.html` - メインアプリケーションファイル（HTML構造とJavaScript）
* `styles.css` - 全CSSスタイルシート（外部ファイル分離）
* `env.js` - Firebase 設定および環境変数
* `package.json` - Firebase 依存関係（v12.2.1）

## 主要関数

* **認証**: `login()`, `signup()`, `logout()`, `showAuthContainer()`, `showChatContainer()`
* **メッセージング**: `sendMessage()`（自動位置情報付加）, `loadMessages()`, `displayMessage()`
* **ファイル処理**: `uploadFile()`（エラーハンドリング強化）, `updateFilePreview()`、ドラッグ＆ドロップ対応
* **位置情報**: `getCurrentLocation()`（タイムアウト短縮・バッテリー節約）, `requestLocationPermission()`
* **地図機能**: `initializeMap()`（非同期、動的中心位置）, `updateMapMarkers()`, `selectMapStyle()`, `createMarkerPopupContent()`
* **地図制御**: `showMap()`（非同期）, `hideMap()`, `showLocationOnMap()`（非同期）, `updateMapStyleButton()`
* **位置情報取得**: `getLastRecordedLocation()`, `getCurrentLocationForMap()`, `getMapInitialCenter()`
* **スタイル切り替え**: `toggleMapStyleDropdown()`, `selectMapStyle()`, `updateCurrentStyleDisplay()`
* **主題図機能**: `toggleOverlayPanel()`, `toggleFloodOverlay()`, `toggleInternalFloodOverlay()`, `showFloodLegend()`, `showInternalFloodLegend()`, `hideLegend()`, `hideInternalFloodLegend()`, `redrawActiveOverlays()`, `initializeDragging()`
* **UI 管理**: グローバル変数と DOM 操作による状態管理、レスポンシブ対応

## 依存関係

* **Firebase SDK**: CDN 経由でインポート（v10.7.0、app/auth/firestore/storage）
* **maplibre-gl**: 地図機能と位置情報可視化に使用
* **Node.js**: パッケージ管理のみでビルド工程は不要

## 開発コマンド

ビルド工程は不要です。ブラウザで任意の HTML ファイルを直接開くだけでアプリケーションを実行できます。

## 使用している Firebase サービス

* **認証**: メール/パスワード認証
* **Firestore**: タイムスタンプ付きリアルタイムメッセージ保存（位置情報含む）
* **ストレージ**: ファイルのアップロードおよび取得（10MB上限、5ファイル上限）
* **セキュリティ**: Firestore と Storage のセキュリティルールは認証済みユーザーのみアクセス可能に設定すべきです

## 地図機能の詳細

* **地図ライブラリ**: MapLibre GL JS v3.6.2
* **地図スタイル**:
  - OSM: OpenStreetMap Japan（日本語対応）
  - 地理院地図: 国土地理院標準地図（ラスタータイル）
  - 航空写真: 国土地理院 seamlessphoto（高解像度航空写真）
  - ベクタ地図: 国土地理院公式ベクトルタイルスタイル
* **背景図切り替え**: ドロップダウンメニューから4種類の地図スタイルを選択可能
* **マーカー機能**:
  - 自分の投稿: 青色ピン、他人の投稿: 赤色ピン
  - クリックでポップアップ表示（ユーザー名、投稿時刻、メッセージ、ファイル、座標）
  - 画像ファイルはポップアップ内でプレビュー表示
* **位置情報**:
  - 全メッセージ送信時に自動取得（5秒タイムアウト、5分キャッシュ）
  - 位置情報許可は初回ログイン時にリクエスト
  - 取得失敗時もメッセージ送信は継続
* **動的地図初期位置**:
  - 優先順位: 最後の記録位置 → 現在位置 → デフォルト位置（東京駅）
  - Firestoreから位置情報付きメッセージを時系列逆順で検索
  - 各段階でのエラーハンドリングとフォールバック処理
* **主題図オーバーレイ機能**:
  - 洪水浸水想定区域: 国土地理院災害リスク情報（50%透過表示）
  - 内水（雨水出水）浸水想定区域: 国土地理院内水氾濫リスク情報（50%透過表示）
  - 主題図パネル: ヘッダの📊ボタンで表示/非表示切り替え
  - モードレス凡例ウィンドウ: ドラッグ可能な凡例表示、地図操作を妨げない
  - 背景図変更時の自動再描画: チェックされた主題図は背景図変更時に自動再描画

## セキュリティルール設定

### Firestore Rules
```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /messages/{messageId} {
      allow read, write: if request.auth != null;
    }
  }
}
```

### Storage Rules
```javascript
rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    match /uploads/{fileName} {
      allow read: if true;
      allow write: if request.auth != null &&
                   request.resource.size <= 10 * 1024 * 1024;
    }
  }
}
```

## 技術仕様詳細

### 地図スタイル設定
```javascript
const mapStyles = {
    osm: { // OpenStreetMap Japan
        name: 'OSM',
        icon: '🌐',
        style: 'https://tile.openstreetmap.jp/styles/maptiler-basic-ja/style.json'
    },
    gsi: { // 国土地理院標準地図（ラスタータイル）
        name: '地理院地図',
        icon: '🗺️',
        style: { /* ラスタータイル設定 */ }
    },
    satellite: { // 国土地理院航空写真
        name: '航空写真',
        icon: '🛰️',
        style: { /* 航空写真タイル設定 */ }
    },
    vector: { // 国土地理院公式ベクトルタイル
        name: 'ベクタ地図',
        icon: '🔷',
        style: 'https://gsi-cyberjapan.github.io/gsivectortile-mapbox-gl-js/std.json'
    }
}
```

### オーバーレイ層設定
```javascript
// 洪水浸水想定区域オーバーレイ
const floodOverlay = {
    source: 'https://disaportaldata.gsi.go.jp/raster/01_flood_l2_shinsuishin_data/{z}/{x}/{y}.png',
    opacity: 0.5, // 50%透過
    legend: 'https://disaportal.gsi.go.jp/hazardmapportal/hazardmap/copyright/img/shinsui_legend3.png'
};

// 内水（雨水出水）浸水想定区域オーバーレイ
const internalFloodOverlay = {
    source: 'https://disaportaldata.gsi.go.jp/raster/02_naisui_data/{z}/{x}/{y}.png',
    opacity: 0.5, // 50%透過
    legend: 'https://disaportal.gsi.go.jp/hazardmapportal/hazardmap/copyright/img/naisui_legend.png'
};
```

### UI制御機能
* **背景図切り替え**: ドロップダウンメニューで4種類から選択、選択状態をハイライト表示
* **主題図パネル**: デフォルト非表示、📊ボタンクリックで表示/非表示切り替え
* **複数オーバーレイ対応**: 洪水・内水浸水想定区域の同時表示可能
* **オーバーレイ自動再描画**: 背景図変更時にチェック済み主題図を自動で再描画
* **モードレス凡例ウィンドウ**: ドラッグ可能な凡例表示、地図操作と並行利用可能

### 動的地図初期位置機能
```javascript
// 地図初期位置決定のロジック
async function getMapInitialCenter() {
    // 1. 最後に記録された位置を検索
    const lastLocation = await getLastRecordedLocation();
    if (lastLocation) return lastLocation;

    // 2. 現在位置を取得
    const currentLocation = await getCurrentLocationForMap();
    return currentLocation; // デフォルト位置へのフォールバック含む
}
```
* **優先順位**: 最後の記録位置 → 現在位置 → デフォルト位置（東京駅）
* **非同期処理**: 地図初期化前に位置情報を確定
* **エラーハンドリング**: 各段階で失敗した場合の適切なフォールバック

### モードレス凡例ウィンドウ機能
```javascript
// ドラッグ機能の実装例
function initializeDragging(modal) {
    const header = modal.querySelector('.legend-header');
    // マウス・タッチイベントでドラッグ操作を実現
    // 画面境界制限付きの自由移動
}
```
* **モードレス表示**: 背景オーバーレイなし、地図操作を妨げない
* **ドラッグ機能**: ヘッダー部分をドラッグしてウィンドウを自由移動
* **複数ウィンドウ**: 洪水・内水凡例を同時表示可能
* **自動配置**: 初期表示時に重ならないよう自動位置調整
* **タッチ対応**: マウス・タッチの両方でドラッグ操作可能

### データソース
* **Firebase**: リアルタイムチャット、認証、ファイルストレージ
* **国土地理院**: 地図タイル、航空写真、ベクタータイル、災害リスク情報
* **OpenStreetMap**: 日本語対応地図タイル

## 最新の改良履歴

### コードの分離・整理 (2025年9月)

#### CSS外部ファイル分離
* **目的**: コードの可読性向上と保守性向上
* **変更内容**:
  - 約800行のCSSコードを `styles.css` に分離
  - `index.html` にCSSリンクタグ追加
  - ファイルサイズの大幅削減とロード効率改善
* **効果**: スタイルとロジックの明確な分離、デバッグの容易化

#### ベクトルタイル設定の最適化
* **変更前**: 独自定義のsources・layers設定（約40行）
```javascript
style: {
    version: 8,
    sources: { 'gsi-vector': { ... } },
    layers: [ ... ] // 複数のレイヤー定義
}
```
* **変更後**: GSI公式スタイルJSON使用（3行）
```javascript
style: 'https://gsi-cyberjapan.github.io/gsivectortile-mapbox-gl-js/std.json'
```
* **効果**:
  - コードの大幅簡略化
  - 国土地理院公式スタイルによる安定した表示
  - 保守性の向上
