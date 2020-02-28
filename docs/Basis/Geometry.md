# Geometry

`Geometry` is the most important object in GIS world. It defines a spatial location and an associated geometric shape. In many spatial analysis workflows, you may not work easily without it. Geometry objects can be created from scratch using `GeometryFactory`, `Point`, `LineString`, `Polygon`, `MultiPoint`, `MultiLineString`, `MultiPolygon` and `GeometryCollection`.

## Geometry Class
The base class of all geometries.

```javascript
export default abstract class Geometry {
    /** The id of geometry. */
    id: number;
    /** Gets the geometry type. */
    get type(): GeometryType;
    /** Gets a flatten coordinates array. */
    abstract coordinatesFlat(): Array<ICoordinate>;
    /** Gets the coordinates array. */
    abstract coordinates(): any;
    /** Gets the centroid of this geometry. */
    centroid(): ICoordinate;
    /** Gets the envelope of this geometry. */
    envelope(): Envelope;
    /** Gets the area of this geometry. */
    area(): number;
    /** Gets the perimeter of this geometry. */
    perimeter(): number;
    /** Gets the interior point of this geometry. */
    interiorPoint(): ICoordinate;
    /** Converts this geometry to GeoJSON format. */
    toJSON(): IGeoJSON;
    /** Clones this geometry as a new one. */
    clone(convert?: (coordinate: ICoordinate) => ICoordinate): Geometry;
    /** Converts this geometry to WKT format. */
    toWKT(): string;
    /** Converts this geometry to WKB format. */
    toWKB(bigEndian?: boolean): Buffer;
    /** Loops all coordinates in this geometry. */
    abstract forEachCoordinates(callback: (coordinate: ICoordinate) => void): void;
    /** Move coordinates by the specified offset. */
    move(offsetX: number, offsetY: number): void;
    /** Rotates this geometry by the specified angle and origin point. */
    rotate(angle: number, origin?: ICoordinate): void;
    /** Detects whether this geometry contains with the specified geometry. */
    contains(geom: Geometry): boolean;
    /** Detects whether this geometry covers with the specified geometry. */
    covers(geom: Geometry): boolean;
    /** Detects whether this geometry crosses with the specified geometry. */
    crosses(geom: Geometry): boolean;
    /** Detects whether this geometry is disjoint with the specified geometry. */
    disjoint(geom: Geometry): boolean;
    /** Calculates distance between this and the specified geometry. */
    distance(geom: Geometry): number;
    /** Detects whether this geometry intersects with the specified geometry. */
    intersects(geom: Geometry): boolean;
    /** Detects whether this geometry overlaps on the specified geometry. */
    overlaps(geom: Geometry): boolean;
    /** Detects whether this geometry is within the specified geometry. */
    within(geom: Geometry): boolean;
    /** Detects whether this geometry touches on the specified geometry. */
    touches(geom: Geometry): boolean;
}
```

### Point Class
Extends `Geometry`.
```javascript
export default class Point extends Geometry {
    x: number;
    y: number;
    constructor(x?: number, y?: number);
    static fromNumbers(...coordinates: number[]): Point;
}
```

### LineString Class
Extends `Geometry`.
```javascript
export default class LineString extends Geometry {
    _coordinates: Array<ICoordinate>;
    constructor(coordinates?: Array<ICoordinate>);
    static fromNumbers(...coordinates: number[]): LineString;
    static fromPoints(...points: ICoordinate[]): LineString;
}
```

### Polygon Class
Extends `Geometry`.
```javascript
export default class Polygon extends Geometry {
    externalRing: LinearRing;
    internalRings: Array<LinearRing>;
    constructor(externalRing?: LinearRing, ...internalRings: LinearRing[]);
    static fromNumbers(...coordinates: number[][]): Polygon;
}
```

### GeometryCollectionBase Class
Extends `Geometry`.
```javascript
export default abstract class GeometryCollectionBase<T extends Geometry> extends Geometry {
    constructor(geometries?: Array<T>);
    get children(): T[];
}
```

### MultiPoint Class Class
Extends `Geometry`.
```javascript
export default class MultiPoint extends GeometryCollectionBase<Point> {
    constructor(points?: Point[]);
}
```

### MultiLineString Class
Extends `GeometryCollectionBase<Point>`.
```javascript
export default class MultiLineString extends GeometryCollectionBase<LineString> {
    constructor(lines?: LineString[]);
}
```

### MultiPolygon Class
Extends `GeometryCollectionBase<Polygon>`.
```javascript
export default class MultiPolygon extends GeometryCollectionBase<Polygon> {
    constructor(polygon?: Polygon[]);
}
```

### GeometryCollection Class
Extends `GeometryCollectionBase<Geometry>`.
```javascript
export default class GeometryCollection extends GeometryCollectionBase<Geometry> {
    constructor(geometries?: Geometry[]);
    get geometries(): Geometry[];
}
```
### GeometryFactory Class
All Geometries could be constructed by `GeometryFactory`. It also provides some shortcut of building some special polygons.

```javascript
export default class GeometryFactory {
    static buildCircle(center: ICoordinate, radius: number, segments?: number): Polygon;
    static buildEllipse(center: ICoordinate, radiusX: number, radiusY: number, segments?: number): Polygon;
    static buildStar(center: ICoordinate, vertexCount: number, radiusLong: number, radiusShort?: number): Polygon;
    static buildSquare(center: ICoordinate, sideLength: number): Polygon;
    static buildRectangle(center: ICoordinate, width: number, height: number): Polygon;
    static envelopeAsPolygon(envelope: IEnvelope): Polygon;
    static envelopeAsLinearRing(envelope: IEnvelope): LinearRing;
    static create(wkt: string): Geometry;
    static create(wkb: Buffer): Geometry;
    static create(geoJson: IGeoJSON): Geometry;
}
```


