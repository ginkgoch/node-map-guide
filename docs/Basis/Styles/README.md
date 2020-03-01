# Styles

`Style` is the basis of data visualization. It describes how `Ginkgoch Map Library` to render geometries as map surface. Our API is designated as much as possible to close to the `HTML5 Canvas` API, so that JavaScript will understand what it is ued for. I think it is pretty straight forward. 

* `Style` section represents the base type of all styles and some of its simple children class: `FillStyle` for area based geometries (`Polygon`, `MultiPolygon`), `LineStyle` for line based geometries (`LineString`, `MultiLineString`) as well as `PointStyle` for point based geometries (`Point`, `MultiPoint`); also a `TextStyle` is capable for all kinds of geometries for label rendering.

* `Advanced` section includes some style extension for some advanced usage. Such as `ValueStyle` for rendering based on attribute value, `ClassBreakStyle` for breaking down attribute ranges, `General` style for unknown geometry type; it also includes some pretty useful utilities for setting style factors.