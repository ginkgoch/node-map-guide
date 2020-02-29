# Geometry Shared Modules

## ICoordinate Interface
```javascript
export default interface ICoordinate {
    x: number;
    y: number;
}
```

## IEnvelope Interface
```javascript
export default interface IEnvelope {
    minx: number;
    miny: number;
    maxx: number;
    maxy: number;
}
```

## IFeature Interface
```javascript
export default interface IFeature {
    id: number;
    geometry: Geometry;
    properties: Map<string, any>;
    type?: string;
}
```

## IGeoJSON Interface
```javascript
export default interface IGeoJSON {
    type: string;
    id?: number;
    coordinates?: any;
    geometries?: IGeoJSON[];
    features?: IGeoJSON[];
    geometry?: IGeoJSON;
    properties?: any;
}
```

## Envelope Class
```javascript
export default class Envelope implements IEnvelope {
    minx: number;
    miny: number;
    maxx: number;
    maxy: number;
    constructor(minx: number, miny: number, maxx: number, maxy: number);
    get width(): number;
    get height(): number;
    centroid(): ICoordinate;
    expand(envelope: IEnvelope): void;
    area(): number;
    perimeter(): number;
    static from(coordinates: any): Envelope;
    static from(coordinates: ICoordinate[]): Envelope;
    static union(env1: IEnvelope, env2: IEnvelope): Envelope;
    static intersection(env1: IEnvelope | undefined, env2: IEnvelope | undefined): Envelope | undefined;
    static unionAll(envelopes: IEnvelope[]): Envelope;
    static disjoined(envelope1: IEnvelope | undefined, envelope2: IEnvelope | undefined): boolean;
    static contains(envelope1: IEnvelope | undefined, envelope2: IEnvelope | undefined): boolean;
    static intersects(envelope1: IEnvelope | undefined, envelope2: IEnvelope | undefined): boolean;
    static equals(envelope1: IEnvelope | undefined, envelope2: IEnvelope | undefined, tolerance?: number): boolean;
    static init(): Envelope;
    static overlaps(envelope1: IEnvelope | undefined, envelope2: IEnvelope | undefined): boolean;
}
```

## BufferCaps Enum
```javascript
export declare enum BufferCaps {
    default = 1,
    round = 1,
    flat = 2,
    square = 3
}
```

## SpatialOps Class
```javascript
export declare class SpatialOps {
    static buffer(geom: Geometry, distance: number, quadrantSegments?: number, endCapStyle?: BufferCaps): Geometry;
    static convexHull(geom: Geometry): Geometry;
    static diff(geom1: Geometry, geom2: Geometry): Geometry;
    static intersection(geom1: Geometry, geom2: Geometry): Geometry;
    static union(geom1: Geometry, geom2: Geometry): Geometry;
    static normalize(geom: Geometry): Geometry;
}
```
