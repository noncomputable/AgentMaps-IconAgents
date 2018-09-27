# IconAgents Documentation

The sidebar of this page contains a list of classes and functions used by Agentmap-IconAgents. 
Click them to see their methods and properties, their purposes, and their types.

Here I'll explain some features of Agentmap-IconAgents that the auto-generated docs probably aren't sufficiently helpful for.

For iconagents, custom icons are [Leaflet Icons](https://leafletjs.com/reference-1.3.4.html#icon)
and the iconagents themselves are [Leaflet Markers](https://leafletjs.com/reference-1.3.4.html#marker), 
instead of the usual [Leaflet CircleMarkers](https://leafletjs.com/reference-1.3.4.html#circlemarker). 
To accomodate icons and markers, the parameters for the `IconAgent` constructor are slightly different from the `Agent` constructor.

The `IconAgent` constructor accepts 4 arguments: 
* A LatLng specifying where the iconagent should be placed
* A [Leaflet Icon](https://leafletjs.com/reference-1.3.4.html#icon)
* An Agentmap instance to add the iconagent to

Agentmap-IconAgents also includes a lowercase `iconagent` factory function which returns a new `IconAgent` instance given the constructor's arguments.

Finally, Agentmap-IconAgents adds a new `Agentmap.iconagentify` method to the Agentmap, which is is effectively the same as [Agentmap.agentify](https://noncomputable.github.io/AgentMaps/docs/Agentmap.html#agentify) except it produces spritegents instead of agents.
Accordingly, `Agentmap.iconagentify` accepts a `IconAgentFeatureMaker`, which is like an [AgentFeatureMaker](https://noncomputable.github.io/AgentMaps/docs/global.html#agentFeatureMaker), 
except instead of a `layer_options` property, it must have an `icon` property whose value is a Leaflet Icon.

Here's an example of how you'd add a new iconagent into an existing agentmap:
```javascript
let my_icon = L.Icon({
	"iconUrl": "images/icon.jpg",
	"iconSize": [50, 50],
	"iconAnchor": [25, 25]
)},
my_iconagent = L.A.iconagent(
	[40.3425, -82.9139], 
	"icon": my_icon,
	agentmap
);

agentmap.agents.addLayer(my_iconagent);
```

And here's an example of a proper `IconAgentFeatureMaker` and an associated use of `Agentmap.iconagentify`:
```javascript
function epidemicIconAgentMaker = function(id) {
	let feature = { 
		"type": "Feature",
		"properties": {
			"place": {
				"type": "unit",
				"id": random_unit_id
			},
			"icon": my_icon,
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

agentmap.iconagentify(50, epidemicIconAgentMaker);
```

While normal agents are based on `L.CircleMarker` which has a `setStyle` method you can pass options to at any time, if you'd like to modify the style of the `L.Marker` representing an IconAgent, you must call one of its [specific styling methods](https://leafletjs.com/reference-1.3.0.html#marker-method). 
If you'd like to change an IconAgent's icon, you can create a new icon and use its `setIcon` method like this:
```javascript
let new_icon = L.icon({	
	"iconUrl": "images/pic.jpg",
	"iconSize": [20, 18],
	"iconAnchor": [10, 9]
)};

my_iconagent.setIcon(new_icon);
```
