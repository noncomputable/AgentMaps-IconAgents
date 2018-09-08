# Spritegents Documentation

The sidebar of this page contains a list of classes and functions used by Agentmap-Spritegents. 
Click them to see their methods and properties, their purposes, and their types.

Here I'll explain some features of Agentmap-Spritegents that the auto-generated docs probably aren't sufficiently helpful for.

For spritegents, custom images are [Leaflet Icons](https://leafletjs.com/reference-1.3.4.html#icon)
and the agents (spritegents) themselves are [Leaflet Markers](https://leafletjs.com/reference-1.3.4.html#marker), 
instead of the usual [Leaflet CircleMarkers](https://leafletjs.com/reference-1.3.4.html#circlemarker). 
To accomodate icons and markers, the parameters for the `Spritegent` constructor are slightly different from the `Agent` constructor.

The `Spritegent` constructor accepts 4 arguments: 
* A LatLng specifying where the spritegent should be placed
* An object specifying the [Leaflet options](https://leafletjs.com/reference-1.3.4.html#icon) for the image (e.g. the file name, size, anchor, etc.)
* An object specifying the [Leaflet options](https://leafletjs.com/reference-1.3.4.html#marker) for the	marker, excluding its icon (e.g. the opacity)
* An Agentmap instance to add the Spritegent into

Agentmap-Spritegents also includes a lowercase `spritegent` factory function which returns a new `Spritegent` instance given the constructor's arguments.

Finally, Agentmap-Spritegents adds a new `Agentmap.spritegentify` method to the Agentmap, which is is effectively the same as [Agentmap.agentify](https://noncomputable.github.io/AgentMaps/docs/Agentmap.html#agentify) except it produces spritegents instead of agents.
Accordingly, `Agentmap.spritegentify` accepts a `SpritegentFeatureMaker`, which is like an [AgentFeatureMaker](https://noncomputable.github.io/AgentMaps/docs/global.html#agentFeatureMaker), 
except instead of a `layer_options` property, it must have an `icon_options` and `marker_options` property specifying the relevant icon options and marker options (excluding the Marker's `icon` option).

Here's an example of how you'd add a new spritegent into an existing agentmap:
```javascript
let my_spritegent = L.A.spritegent(
	[40.3425, -82.9139], 
	{
		"iconUrl": "images/icon.jpg",
		"iconSize": [50, 50],
		"iconAnchor": [25, 25]
	},
	{
		"opacity": .9
	},
	agentmap
);

agentmap.agents.addLayer(my_spritegent);
```

And here's an example of a proper `SpritegentFeatureMaker` and an associated use of `Agentmap.spritegentify`:
```javascript
function epidemicSpritegentMaker = function(id) {
	let feature = { 
		"type": "Feature",
		"properties": {
			"place": {
				"type": "unit",
				"id": random_unit_id
			},
			"icon_options": {
				"iconUrl": "images/icon.jpg",
				"iconSize": [50, 50],
				"iconAnchor": [25, 25]
			},
			"marker_options": {
				"opacity": .9
			},
			"infected": Math.random() > .15 ? false : true,
			"ticks_until_recovery": Math.random() * 2000,
		},
		"geometry": {
			"type": "Point",
			"coordinates": center_coords
		},
	};

	return feature;
}

agentmap.spritegentify(50, epidemicSpritegentMaker);
```

While normal agents are based on `L.CircleMarker` which has a `setStyle` method you can pass options to at any time, if you'd like to change the style of the `L.Marker` representing the spritegent, you must call one of its [specific styling methods](https://leafletjs.com/reference-1.3.0.html#marker-method). 
If you'd like to change a spritegent's icon, you can create a new icon and use its `setIcon` method like this:
```javascript
let new_icon = L.icon({	
	"iconUrl": "images/pic.jpg",
	"iconSize": [20, 18],
	"iconAnchor": [10, 9]
)};

spritegent.setIcon(new_icon);
```

If you have an existing `L.Icon` whose options you'd like to use to create a new spritegent, either via the `Spritegent` constructor or `Agentmap.spritegentify`, 
you can access its options via its `L.Icon.options` property.
