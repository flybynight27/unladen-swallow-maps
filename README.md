# Unladen Swallow Maps - ReactJS maps without external dependencies

[![npm version](https://img.shields.io/npm/v/pigeon-maps.svg)](https://www.npmjs.com/package/pigeon-maps)

![Demonstration](https://github.com/mariusandra/pigeon-maps/blob/master/video.gif?raw=true)

Demo: https://mariusandra.github.io/pigeon-maps/ (using maps from Mapbox, Wikimedia and OSM)

## What is it?

Are you tired of waiting 3 seconds to parse 200kb of Google Maps JavaScript just to display a few tiles and a marker? 140kb of minified Leaflet too much?

Welcome to the club!

This project aims to provide a performance-first React-centric extendable map engine.

We're currently at:
- ~20KB minified
- ~5KB gzipped

Implemented:

- Show tiles
- Arbitrary overlays (markers, etc)
- Move the map by dragging
- Move the map by touch on mobile
- Zooming with the scroll wheel
- Zooming by touch
- Fractional zooming (e.g. to level 12.2)
- Zoom without flickering (keep old tiles until new ones load)
- Smooth animated zooming
- Slide when dragging and letting go
- Event handling (clicks, etc)
- Double click and double tap zooming

Missing:
- Double tap and then swipe touch zooming


## Install

One of:

```
# with yarn
yarn add pigeon-maps

# still on npm
npm install --save pigeon-maps
```


## Code

[Demo](https://mariusandra.github.io/pigeon-maps/)
[(source)](https://github.com/mariusandra/pigeon-maps/tree/master/demo/demo.js)

```js
import Map from 'pigeon-maps'
import Marker from 'pigeon-marker'
import Overlay from 'pigeon-overlay'

const map = (
  <Map center={[50.879, 4.6997]} zoom={12} width={600} height={400}>
    <Marker anchor={[50.874, 4.6947]} payload={1} onClick={({ event, anchor, payload }) => {}} />

    <Overlay anchor={[50.879, 4.6997]} offset={[120, 79]}>
      <img src='pigeon.jpg' width={240} height={158} alt='' />
    </Overlay>
  </Map>
)
```

## Inferno support

Pigeon Maps works very will with [Inferno](https://infernojs.org/). Just use these import paths:

```js
import Map from 'pigeon-maps/inferno'
import Marker from 'pigeon-marker/inferno'
import Overlay from 'pigeon-overlay/inferno'
```

[Here's the same demo running in Inferno](https://mariusandra.github.io/pigeon-maps/inferno/)

## Plugins

[pigeon-overlay](https://github.com/mariusandra/pigeon-overlay) ([demo](https://mariusandra.github.io/pigeon-overlay/)) - an anchored overlay

[pigeon-marker](https://github.com/mariusandra/pigeon-marker) ([demo](https://mariusandra.github.io/pigeon-marker/)) - a simple marker component

If you're interested in making a new plugin, check out the code of [pigeon-marker](https://github.com/mariusandra/pigeon-marker/blob/master/src/index.js) as a starting point. Feel free to clone the repo and rename every mention of `pigeon-marker` to `pigeon-myplugin`. You'll get a demo and linking system out of the box. More documentation about this coming soon. Contributions welcome.


## API

### Map

**defaultCenter** - Coordinates of the map center in the format `[lat, lng]`. Use if the component is uncontrolled.

**center** - Coordinates of the map center in the format `[lat, lng]`. Use if the component is controlled, e.g. you'll be listening to `onBoundsChanged` and passing a new `center` when the bounds change.

**zoom** - Current zoom level `12`

**width** - Width of the component in pixels. Must be set.

**height** - Height of the component in pixels. Must be set.

**provider** - Function that returns a [TMS URL](https://wiki.openstreetmap.org/wiki/TMS): `(x, y, z) => url`.

**animate** - Animations enabled, `true`.

**attribution** - What to show as an [attribution](https://www.openstreetmap.org/copyright). React node or `false` to hide.

**attributionPrefix** - Prefix before attribution. React node or `false` to hide.

**onClick** - When map is clicked `function ({ event, latLng, pixel })``

**onBoundsChanged** - When the bounds change, `function ({ center, zoom, bounds, initial })`. Use this for a controlled component, then set `center` and `zoom` when it's called. This callback also gets called on the initial mount (when the first bounds become known). In this case the prop `initial` will be set to `true`. It will be `false` on all subsequent calls.

### Overlays

`<Map />` takes random React components as its children. The children may have these special props:

**anchor** - At which coordinates `[lat, lng]` to anchor the overlay with the map.

**offset** - Offset in pixels relative to the anchor.

The children get passed these special props:

**left** - Pixels from the left of the map, calculated from `anchor` and `offset`

**top** - Pixels from the top of the map, calculated from `anchor` and `offset`

**mapState** - An object `{ center, zoom, bounds, width, height }` that gets updated at every animation frame.

**latLngToPixel** - A helper `function (latLng, center, zoom)` that returns the position in pixels `[x, y]` for any `[lat, lng]`. The last 2 arguments are optional.

**pixelToLatLng** - A helper `function (pixel, center, zoom)` that converts any pixel coordinates `[x, y]` to `[lat, lng]`. The last 2 arguments are optional.

Use these two functions to create beautiful widgets. See [a sample overlay](https://github.com/mariusandra/pigeon-overlay/blob/master/src/index.js).

Add the class `pigeon-drag-block` to disable dragging on the overlay. Add the class `pigeon-click-block` to disable map background clicks on the element.

Alternatively use the `<Overlay />` component. It accepts `anchor`, `offset` and `classNames` as its props and positions itself accordingly.

## Yeah, but why Unladen Swallow??

Because it's funnier than pigeon. That's all.

---

