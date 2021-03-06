Leaflet Panel Layers
====================

[![npm version](https://badge.fury.io/js/leaflet-panel-layers.svg)](http://badge.fury.io/js/leaflet-panel-layers)

Leaflet Control Layers extended with support groups and icons

Copyright [Stefano Cudini](http://labs.easyblog.it/stefano-cudini/)

Tested in Leaflet 0.7.x, 1.4.x

**Demo:**

[DEMO with some of the new features]() No demo currently available on a public site. Comming soon.

[labs.easyblog.it/maps/leaflet-panel-layers](http://labs.easyblog.it/maps/leaflet-panel-layers/)

**Source code:**  

[Github](https://github.com/stefanocudini/leaflet-panel-layers)  

**Use Cases:**

[Websites-that-use-Leaflet-Panel-Layers](https://github.com/stefanocudini/leaflet-panel-layers/wiki/Websites-that-use-Leaflet-Panel-Layers)

![Image](https://raw.githubusercontent.com/stefanocudini/leaflet-panel-layers/master/examples/images/screenshot/leaflet-panel-layers-layout.jpg)

# Options
| Option	        | Default   | Description                       |
| ------------------| ----------| ----------------------------------------- |
| compact	        | false     | panel height minor of map height |
| compactOffset     | 0         | the distance from the bottom of the map that the panal will not expand into | 
| collapsed         | false     | panel collapsed at startup |
| autoZIndex 	    | true      | set zindex layer by order definition |
| collapsibleGroups | false     | groups of layers is collapsible by button |
| collapsiblePanel  | true      | panel can be of collapsed by button |
| groupCheckboxes   | true      | check box to select and deselect all items in a group|
| buildItem	        | null      | function that return row item html node(or html string) |
| title	            | ''        | title of panel |
| className	        | ''        | additional class name for panel |
| position	        | 'topright'| position of control |
| tooltip           | ''        | Setes the string that will be showen in a tooltip for the layer when you hover over the name|

# Events
| Event			         | Data			          | Description                               |
| ---------------------- | ---------------------- | ----------------------------------------- |
| 'panel:selected'       | {layerDef}             | fired after moved and show markerLocation |
| 'panel:unselected'	 | {}	                  | fired after control was expanded          |

# Methods
| Method		         | Arguments		     | Description                                              |
| ---------------------- | --------------------- | -------------------------------------------------------- |
| addBaseLayer()         | layerDef,group,collapsed | add new layer item definition to panel as baselayers     |
| addOverlay()           | 'Text message' 	     | add new layer item definition to panel as overlay        |
| removeLayer()		     | 'Text searched'	     | remove layer item from panel                             |
| configToControlLayers()| 'Text searched'	     | convert config from Control.PanelLayers to Control.Layers|
| expandGroup()          | group                 | expand a group in a panel |
| collapseGroup()        | group                 | collapse a group in a panel | 
# Usage

**Panel Item Definition formats**
```javascript
	{
		name: "Bar",
		icon: iconByName('bar'),
		layer: L.geoJson(Bar, {pointToLayer: featureToMarker })
	}
```
```javascript
	{
		layer: {
			type: "geoJson",
			args: [ river ]
		},
	}
```
```javascript
	{
		group: "Title Group",
		collapsed: true,
		layers: [
		...other items...
		]
	}
```

**Multiple active layers with icons and tooltips**
```javascript
var baseLayers = [
	{
		active: true,
		name: "OpenStreetMap",
		layer: L.tileLayer('http://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png')
	}
];
var overLayers = [
	{
		name: "Drinking Water",
		icon: '<i class="icon icon-water"></i>',
		tooltip: "This Shows where you can find drinking water.",
		layer: L.geoJson(WaterGeoJSON)
	},
	{
		active: true,
		name: "Parking",
		tooltip: "Avalibale parking",
		icon: '<i class="icon icon-parking"></i>',
		layer: L.geoJson(ParkingGeoJSON)
	}
];
map.addControl( new L.Control.PanelLayers(baseLayers, overLayers) );
```

**Build panel layers from pure JSON Config**
```javascript
var panelJsonConfig = {
    "baselayers": [
        {
            "active": true,
            "name": "Open Cycle Map",
            "layer": {
                "type": "tileLayer",
                "args": [
                    "http://{s}.tile.opencyclemap.org/cycle/{z}/{x}/{y}.png"
                ]
            }
        },
        {
            "name": "Landscape",
            "layer": {
                "type": "tileLayer",
                "args": [
                    "http://{s}.tile3.opencyclemap.org/landscape/{z}/{x}/{y}.png"
                ]
            }
        },        
        {
            "name": "Transports",
            "layer": {
                "type": "tileLayer",
                "args": [
                    "http://{s}.tile2.opencyclemap.org/transport/{z}/{x}/{y}.png"
                ]
            }
        }
    ],
    "overlayers": [
        {
            "name": "Terrain",
            "layer": {
            "type": "tileLayer",
            "args": [
                "http://toolserver.org/~cmarqu/hill/{z}/{x}/{y}.png", {
                "opacity": 0.5
                }
            ]
            }
        }
    ]
};
L.control.panelLayers(panelJsonConfig.baseLayers, panelJsonConfig.overLayers).addTo(map);
```

**Grouping of layers**
```javascript
L.control.panelLayers(
	[
		{
			name: "Open Street Map",
			layer: osmLayer
		},
		{
			group: "Walking layers",
			layers: [
				{
					name: "Open Cycle Map",
					layer: L.tileLayer('http://{s}.tile.opencyclemap.org/cycle/{z}/{x}/{y}.png')
				},
				{
					name: "Hiking",
					layer: L.tileLayer("http://toolserver.org/tiles/hikebike/{z}/{x}/{y}.png")
				}
			]
		},
		{
			group: "Road layers",
			layers: [
				{
					name: "Transports",
					layer: L.tileLayer("http://{s}.tile2.opencyclemap.org/transport/{z}/{x}/{y}.png")
				}
			]
		}
	],
	{collapsibleGroups: true}
).addTo(map);
```

**Collapse some layers' groups**
```javascript
L.control.panelLayers([
	{
		name: "Open Street Map",
		layer: osmLayer
	},
	{
		group: "Walking layers",
		layers: [
			{
				name: "Open Cycle Map",
				layer: L.tileLayer('http://{s}.tile.opencyclemap.org/cycle/{z}/{x}/{y}.png')
			},
			{
				name: "Hiking",
				layer: L.tileLayer("http://toolserver.org/tiles/hikebike/{z}/{x}/{y}.png")
			}			
		]
	},
	{
		group: "Road layers",
		collapsed: true,
		layers: [
			{
				name: "Transports",
				layer: L.tileLayer("http://{s}.tile2.opencyclemap.org/transport/{z}/{x}/{y}.png")
			}
		]
	}
]).addTo(map);
```

**Add layers dynamically at runtime**
```javascript
var panel = L.control.panelLayers();

$.getJSON('some/url/path.geojson', function(data){
	panel.addOverlay({
		name: "Drinking Water",
		icon: '<i class="icon icon-water"></i>',
		layer: L.geoJson(data)
	});
});
```
the interface is:
```javascript
panel.addOverlay(layer, name, group, colapsed);
``` 
If you want the group you are adding to to be colapsed after adding the layer then colapsed must be 'true'
the deafult is for it to expand the group.

**Using bootstrap glyphicons**

To use bootstrap glyphicons make sure that you include `leaflet-panal-layers-glyphicons.css` **AFTER** bootstrap.
You can then define an icon like with `'<i class="glyphicon glyphicon-glass"></i>'`

# Build

This plugin support [Grunt](http://gruntjs.com/) for building process.
Therefore the deployment require [NPM](https://npmjs.org/) installed in your system.
After you've made sure to have npm working, run this in command line:
```bash
npm install
grunt
```
