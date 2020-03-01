# Geometries

`Geometry` is the most important object in GIS world. It defines a spatial location and an associated geometric shape. In many spatial analysis workflows, you may not work easily without it. Geometry objects can be created from scratch using `GeometryFactory`, `Point`, `LineString`, `Polygon`, `MultiPoint`, `MultiLineString`, `MultiPolygon` and `GeometryCollection`.

All geometries are formed by an interface calls `ICoordinate` and maintain their own property envelope whose type is `IEnvelope`. Refer to the `Shared` sub section for more shared modules.

`Feature` is one level above `Geometries`. Besides the geometry, it also describes attribute data of a geometry.




