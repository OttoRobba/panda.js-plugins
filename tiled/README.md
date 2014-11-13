## Tiled plugin for Panda.js

http://www.mapeditor.org/

### Install

Copy `tiled.js` into `src/plugins/` folder.

### Example

    game.module(
        'game.main'
    )
    .require(
        'plugins.tiled'
    )
    .body(function() {

    game.addAsset('desert.json');
    game.addAsset('tmw_desert_spacing.png');

    game.createScene('Main', {
        init: function() {
            var tilemap = new game.TileMap('desert.json');
            tilemap.addTo(this.stage);
        }
    });

    });

### Options

config.js
    
    tileMap: {
        cacheLayersAsBitmap: true, // Cache every layer as bitmap
        cacheAsBitmap: true // Cache whole tilemap as bitmap
    }
    