# voxel-land

[
![NPM version](https://img.shields.io/npm/v/voxel-land.svg)
](https://www.npmjs.com/package/voxel-land)

[voxel.js](https://voxel.github.io/)向けの多機能な地形生成プラグインです。丘、草、土、石、木、鉱石を含む風景を生成します。

地形の表面はシンプレックスノイズ（[voxel-perlin-terrain](https://github.com/maxogden/voxel-perlin-terrain)と同様の手法）を使用して生成され、木は[voxel-forest](https://github.com/deathcap/voxel-forest)によって提供されます。


![手続き的に生成された木々のある草原の風景](http://i.imgur.com/ZzVFUAj.png "Screenshot overview")


![草、土、石の層がわかる断面図](http://i.imgur.com/D918dUX.png "Screenshot both")


![石炭と鉄鉱石の鉱脈が見える採掘跡](http://i.imgur.com/XB8k8XP.png "Screenshot mined")


## 機能

-   **多層地形:** 草、土、石の層を持つワールドを生成します。
-   **手続き的な丘の生成:** シンプレックスノイズを使用し、高さや粗さを設定可能な、変化に富んだ起伏のある風景を作成します。
-   **木の生成:** 密度を制御しつつ、ワールドにオークの木を配置します。
-   **鉱脈:** 石の層の内部に石炭や鉄鉱石の鉱脈（クラスター）を生成します。
-   **Web Worker対応:** チャンク生成を別スレッドにオフロードすることでラグを防ぎ、ゲームの応答性を保ちます。
-   **自動ブロック登録:** 必要なすべてのブロック（草、土、石、原木、葉、鉱石など）を`voxel-registry`に登録します。

## インストール

```bash
npm install voxel-land
```

## 使い方

このモジュールは[voxel-plugins](https://github.com/deathcap/voxel-plugins)互換のプラグインです。先に`voxel-registry`を読み込む必要があります。

ゲームの起動コードで、エンジンのデフォルトのチャンク生成を無効にしてから、本プラグインを読み込みます。

```javascript
// 1. voxel-engine-stackglを使用する
const createEngine = require('voxel-engine-stackgl');

// 2. デフォルトのチャンク生成を無効にする
const game = createEngine({
  // ... その他のオプション
  generateChunks: false
});

// 3. voxel-landをプラグインとして読み込む
const land = game.plugins.get('voxel-land') || game.plugins.load('voxel-land', {
  // 以下の設定オプションを参照
  seed: 'a random seed',
  crustLower: 0,
  crustUpper: 2,
  hillScale: 20,
  treesMaxDensity: 5
});
```

プラグインは自動的に`missingChunk`イベントをリッスンし、地形を生成します。

### API

-   `land.enable()`: 地形を生成するために`missingChunk`イベントのリッスンを開始します。読み込み時にデフォルトで呼び出されます。
-   `land.disable()`: `missingChunk`イベントのリッスンを停止します。

## 設定オプション

プラグインの読み込み時にオプションオブジェクトを渡すことができます。

| オプション | 型 | デフォルト | 説明 |
| --- | --- | --- | --- |
| `seed` | `string` | `'foo'` | ワールド生成用のランダムシード。 |
| `crustLower` | `number` | `0` | 地形表面の最小基準高さ。 |
| `crustUpper` | `number` | `2` | 地形表面の最大基準高さ。 |
| `hillScale` | `number` | `20` | 局所的な丘のスケール。値が小さいほど丘が頻繁に生成されます。 |
| `roughnessScale` | `number` | `200` | 大規模な地形の起伏のスケール。 |
| `populateTrees` | `boolean` | `true` | 地形に木を生成するかどうか。 |
| `treesScale` | `number` | `200` | 木の密度のノイズスケール。森の配置に影響を与えます。 |
| `treesMaxDensity` | `number` | `5` | 1つのチャンク内に生成できる木の最大数。 |
| `registerBlocks` | `boolean` | `true` | `true`の場合、草、土、石、鉱石、原木などのブロックを登録します。 |
| `registerItems` | `boolean` | `true` | `true`の場合、石炭などのアイテムを登録します。 |
| `registerRecipes` | `boolean` | `true` | `true`の場合、`voxel-recipes`のシソーラスに木材と葉を登録します。 |

## ライセンス

MIT
