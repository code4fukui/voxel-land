# voxel-land

> 日本語のREADMEはこちらです: [README.ja.md](README.ja.md)

[
![NPM version](https://img.shields.io/npm/v/voxel-land.svg)
](https://www.npmjs.com/package/voxel-land)

A feature-rich terrain generator for [voxel.js](https://voxel.github.io/) that creates landscapes with hills, grass, dirt, stone, trees, and ores.

The terrain surface is generated with Simplex noise (similar to [voxel-perlin-terrain](https://github.com/maxogden/voxel-perlin-terrain)), and trees are provided by [voxel-forest](https://github.com/deathcap/voxel-forest).


![A grassy landscape with procedurally generated trees](http://i.imgur.com/ZzVFUAj.png "Screenshot overview")


![Cross-section revealing layers of grass, dirt, and stone](http://i.imgur.com/D918dUX.png "Screenshot both")


![Mined area showing clusters of coal and iron ore](http://i.imgur.com/XB8k8XP.png "Screenshot mined")


## Features

-   **Multi-Layer Terrain:** Generates a world with layers of grass, dirt, and stone.
-   **Procedural Hills:** Uses Simplex noise to create a varied, rolling landscape with configurable height and roughness.
-   **Tree Generation:** Populates the world with oak trees, with controllable density.
-   **Ore Veins:** Generates clusters of coal and iron ore within the stone layer.
-   **Web Worker Support:** Offloads chunk generation to a separate thread to prevent lag and keep the game responsive.
-   **Automatic Block Registration:** Registers all necessary blocks (grass, dirt, stone, logs, leaves, ores, etc.) with `voxel-registry`.

## Installation

```bash
npm install voxel-land
```

## Usage

This module is a [voxel-plugins](https://github.com/deathcap/voxel-plugins) compatible plugin. It requires `voxel-registry` to be loaded first.

In your game's startup code, disable the engine's default chunk generation and then load the plugin:

```javascript
// 1. Use voxel-engine-stackgl
const createEngine = require('voxel-engine-stackgl');

// 2. Disable default chunk generation
const game = createEngine({
  // ... other options
  generateChunks: false
});

// 3. Load voxel-land as a plugin
const land = game.plugins.get('voxel-land') || game.plugins.load('voxel-land', {
  // see configuration options below
  seed: 'a random seed',
  crustLower: 0,
  crustUpper: 2,
  hillScale: 20,
  treesMaxDensity: 5
});
```

The plugin will automatically listen for `missingChunk` events and generate terrain.

### API

-   `land.enable()`: Starts listening for `missingChunk` events to generate terrain. Called by default on load.
-   `land.disable()`: Stops listening for `missingChunk` events.

## Configuration Options

You can pass an options object when loading the plugin.

| Option              | Type      | Default     | Description                                                              |
| ------------------- | --------- | ----------- | ------------------------------------------------------------------------ |
| `seed`              | `string`  | `'foo'`     | The random seed for the world generator.                                 |
| `crustLower`        | `number`  | `0`         | The minimum base height of the terrain surface.                          |
| `crustUpper`        | `number`  | `2`         | The maximum base height of the terrain surface.                          |
| `hillScale`         | `number`  | `20`        | The scale of local hills. Smaller values create more frequent hills.     |
| `roughnessScale`    | `number`  | `200`       | The scale of large-scale terrain ruggedness.                             |
| `populateTrees`     | `boolean` | `true`      | Whether to generate trees on the terrain.                                |
| `treesScale`        | `number`  | `200`       | The noise scale for tree density, affecting forest placement.            |
| `treesMaxDensity`   | `number`  | `5`         | The maximum number of trees that can spawn in a single chunk.            |
| `registerBlocks`    | `boolean` | `true`      | If `true`, registers blocks like grass, dirt, stone, ores, and logs.     |
| `registerItems`     | `boolean` | `true`      | If `true`, registers items like coal.                                    |
| `registerRecipes`   | `boolean` | `true`      | If `true`, registers wood and leaves with `voxel-recipes` thesaurus.     |

## License

MIT