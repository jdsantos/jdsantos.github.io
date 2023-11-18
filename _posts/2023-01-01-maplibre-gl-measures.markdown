---
title: "maplibre-gl-measures"
layout: post
date: 2023-01-01 23:00
image: /assets/images/projects/1.png
headerImage: true
projects: true
hidden: true
tag:
- maplibre
- measurements
- opensource
star: false
category: project
author: jdsantos
description: A MapLibre GL JS plugin for taking length measures with lines and area measures with polygons
---

# maplibre-gl-measures

A MapLibre GL JS plugin for taking length measures with lines and area measures with polygons. Check it out here:

It's working with [MapLibre GL JS](http://maplibre.org) inspired by the great work done by [mapbox/mapbox-gl-draw](https://github.com/mapbox/mapbox-gl-draw)

## Code

Code is available on [this Github repo.](https://github.com/jdsantos/maplibre-gl-measures)


## Demo

You can rush to the [demo here.](https://jdsantos.github.io/demos/maplibre-gl-measures)


## Getting started

To use this plugin you need to run:

``` js
npm install --save maplibre-gl-measures
```

and then, in your code use it as follows:

``` js

// Import it into your code
import MeasuresControl from 'maplibre-gl-measures';

// your map logic here...

// add the plugin
map.addControl(new MeasuresControl({ /** see options below for further tunning */}), "top-left");

```
