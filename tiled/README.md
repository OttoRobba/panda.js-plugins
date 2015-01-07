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
    
### Using Object Layers

With this plugin you can position and spawn entities directly within Tiled. With the map editor open:

1. Create an object layer.
2. Draw and position the object where you want it
3. Assign a name to it
4. Save the map into a json

Now back to your favorite text editor, create a class with the name you assigned (case sensitive). Suppose we had made an object named Troll in Tiled:

    game.createClass('Troll', {
        init: function(x, y, container, width, height, properties) {
    		this.sprite = new game.Sprite('troll.png');
    		this.sprite.position.set(x, y);
    		this.sprite.addTo(container);
    		//We add the object so that its update function is automatically called by the scene
    		game.scene.addObject(this); 
    	}
    });

We will use the X and Y to position the entity - this are the coordinates of the object you drew in Tiled.

The container argument is used so that the plugin can properly add the entity to the stage in the correct Z order. If you want to manually call this entity, just pass this.stage as the third argument, like this:

    var troll = new game.Troll(100, 100, this.stage);
    
The width and height arguments are useful if you are creating something like bounding boxes or entites which can have varying scales.

The properties argument is an object, containing all the properties that you add yourself on the Tiled object. This means you can easily change things like the health of an enemy or the message in a billboard. Very versatile.

### Expanded example

    game.module(
        'game.main'
    )
    .require(
        'plugins.tiled'
    )
    .body(function() {
    
    game.addAsset('desert.json');
    game.addAsset('troll.png');
    game.addAsset('tmw_desert_spacing.png');
    
    game.createClass('Troll', {
        init: function(x, y, container, width, height, properties) {
    		this.sprite = new game.Sprite('troll.png');
    		this.sprite.position.set(x, y);
    		this.health = properties.health;
    		this.sprite.addTo(container);
    		game.scene.addObject(this);
    	},
    	update: function() {
    		this.sprite.position.x += 1;
    	},
    });
    
    game.createScene('Main', {
        init: function() {
            var tilemap = new game.TileMap('desert.json');
            tilemap.addTo(this.stage);
            var troll = new game.Troll(10, 10, this.stage, null, null, {health: 200});
        }
    });
    
    });
