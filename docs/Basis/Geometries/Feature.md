# Feature Modules

## Feature Class
```javascript
export default class Feature implements IFeature {
    id: number;
    geometry: Geometry;
    properties: Map<string, any>;
    /**
     * Construct a feature instance.
     * @param {Geometry} geom The geometry in the feature. It is required.
     * @param {Map<string, any>} properties The properties of this feature. Default is empty map.
     * @param id The id of this feature. Default id is the same as geometry's id.
     */
    constructor(geom: Geometry | IGeoJSON, properties?: Map<string, any> | any, id?: number);
    get type(): string;
    toJSON(): {
        id: number;
        type: string;
        geometry: IGeoJSON;
        properties: any;
    };
    envelope(): import("./Envelope").default;
    clone(fields?: 'none' | 'all' | string[]): Feature;
    static create(feature: IFeature): Feature;
    static create(json: IGeoJSON): Feature;
    static parseJSON(json: IGeoJSON): Feature;
}
```

## FeatureCollection Class
```javascript
export default class FeatureCollection {
    id: number;
    features: Feature[];
    type: string;
    constructor(features?: Array<IFeature>, id?: number);
    envelope(): Envelope;
    toJSON(): {
        id: number;
        type: string;
        features: {
            id: number;
            type: string;
            geometry: IGeoJSON;
            properties: any;
        }[];
    };
    /**
     *
     * @deprecated This method is deprecated. Please use parseJSON() instead.
     */
    static create(json: IGeoJSON): FeatureCollection;
    static parseJSON(json: IGeoJSON): FeatureCollection;
}
```
