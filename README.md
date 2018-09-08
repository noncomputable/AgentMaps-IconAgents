# SpriteMaps

SpriteMaps is an extension for [AgentMaps](https://github.com/noncomputable/AgentMaps) that displays the agents as custom images instead of circles.

You can get a bundle [here]() or install it via npm (`npm install spritemaps`). You'll need to include Agentmaps and [Leaflet](https://leafletjs.com/download.html) first.

Images are represented with Leaflet [Icons](https://leafletjs.com/reference-1.3.4.html#icon) and the agents (spritegents) are Leaflet [Markers](https://leafletjs.com/reference-1.3.4.html#marker) instead of CircleMarkers. To accomodate icons and markers, the `Spritegent` constructor is slightly different from the `Agent` constructor.

The `Spritegent` constructor accepts 4 arguments: 
* A LatLng specifying where the agent should be placed
* An object specifying the [Leaflet options](https://leafletjs.com/reference-1.3.4.html#icon) for the image (the file name, size, etc.)
* An object specifying the [Leaflet options](https://lea
* An Agentmap instance to embed the Spritegent into

It also includes a lowercase `spritegent` factory function which returns a new `Spritegent` instance given the constructor arguments.

Finally, SpriteMaps adds a new `Agentmap.spritegentify` method to an Agentmap, which is is effectively the same as [Agentmap.agentify](https://noncomputable.github.io/AgentMaps/docs/Agentmap.html#agentify) except it produces spritegents instead of agents. Accordingly, `Agentmap.spritegentify` accepts a `SpritegentFeatureMaker`, which is like an [AgentFeatureMaker](https://noncomputable.github.io/AgentMaps/docs/global.html#agentFeatureMaker), except its `layer_options` property must specify options for a Leaflet Marker (instead of a CircleMarker, as in a normal agent), and it must have an additional `image_options` property, specifying options for a Leaflet Icon.

Here's an example of how you'd create a new spritegent.
```javascript
let s
```

And here's an example of a proper `SpritegentFeatureMaker` and an associated use of `Agentmap.spritegentify`:
```javascript
let s
```
