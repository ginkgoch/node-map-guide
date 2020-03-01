# Layers

In the previous sections, `Geometry` and `Style` are introduced. They are both low level APIs, it is terrible to develop mapping software with only those two kinds of modules. Imagine, where are the geometries from, or how we deliver the geometries? Yes, from some pre-built files or services.

* `FeatureSource` is the placeholder to store the GIS data. Supported data sources (e.g. `ShapeFile`, `CSV`, `GeoJSON` etc.) are represented in this page. With the power `FeatureSource`, developers could query features based on a spatial relation to a specific geometry or feature, loop features inside and get any possible feature information. Besides, it also acts as the data provider of map rendering.

    `FeatureSource` is the base class of all feature sources. You can easily extend it to implement your own.

* `Layer` is the minimal unit for map rendering. It wraps the data provider (`FeatureSource`) as well as a list of user defined `Style`s. Each layer defines where the geometries from and how to render geometries as a map surface. Multiple layers overlapping together forms the final map surface like your Google, OSM or Bing maps.