# FeatureSource Shared Modules

## Field Class
This class maintains a field definition of a feature source. It is a general field definition for almost all feature sources. A field is formed with name, data type, length of a field to store corresponding value, and an extra hash map (key - value pairs) to store other infos that are not covered by pre-defined properties.

```javascript
export declare class Field {
    /**
     * The name of the field.
     */
    name: string;
    /**
     * The data type of the field.
     */
    type: string;
    /**
     * The maximum data length of the field.
     */
    length: number;
    /**
     * The extra info of the field.
     */
    extra: Map<string, any>;
    /**
     * Constructs a field instance.
     * @param {string} name Field name.
     * @param {string} type Field data type.
     * @param {number} length Field data length.
     * @param {any|Map} extra Field extra info. Optional, it can be either an object or Map.
     */
    constructor(name?: string, type?: string, length?: number, extra?: any);
    /**
     * Converts current instance into JSON format.
     */
    toJSON(): {
        name: string;
        type: string;
        length: number;
        extra: any;
    };
    /**
     * Parse JSON format data to a concrete instance.
     * @static
     * @param {any} json JSON format data.
     * @returns {Field} A field instance that is converted from the JSON data.
     */
    static parseJSON(json: any): Field;
}
```

## Srs Class
This class represents the SRS info (spatial reference system).

```javascript
export declare class Srs {
    /**
     * Constructs a SRS info instance.
     * @param {string} projection The projection string; can be either proj4, EPSG name or WKT.
     */
    constructor(projection?: string);
    /**
     * Gets the unit of SRS.
     */
    get unit(): Unit;
    /**
     * A shortcut property to get the projection string; can be either proj4, EPSG name or WKT.
     */
    get projection(): string | undefined;
    /**
     * Sets the projection string; can be either proj4, EPSG name or WKT.
     */
    set projection(projection: string | undefined);
    /**
     * Converts this SRS instance into a JSON format data.
     * @returns {any} A JSON format data of this SRS.
     */
    toJSON(): {
        projection: string | undefined;
        unit: Unit;
    };
    /**
     * Parses the JSON format data into a SRS instance. If the data doesn't match the SRS schema, it throws exception.
     * @param json
     */
    static parseJSON(json: any): Srs;
}
```

## PropertyAggregator Class
This class represents a utility for aggregating property data from a feature source.

```javascript
export declare class PropertyAggregator {
    /**
     * The properties for aggregating.
     */
    properties: Array<Map<string, any>>;
    /**
     * Constructs an aggregator instance.
     * @param {Array<Map<string, any>>} properties The properties for aggregating.
     */
    constructor(properties: Array<Map<string, any>>);
    /**
     * Selects values by a specific field name.
     * @param {string} field The field name to filter from the source property data.
     * @returns {any[]} An array of field values of the specified field name.
     */
    select(field: string, sort?: boolean, sortFn?: (a: any, b: any) => number): any[];
    /**
     * Select values by a specific field name and get rid of the duplicated values.
     * @param {string} field The field name to filter from the source property data.
     * @param {boolean} sort Whether the return values need to be sorted.
     * @returns {any[]} An array of distinct field values of the specified field name.
     */
    distinct(field: string, sort?: boolean): any[];
    breakDownValues(field: string, breakCount: number, breakBy?: 'value' | 'position'): Array<{
        minimum: number;
        maximum: number;
    }>;
    /**
     * Gets some general aggregated result from a specific field name.
     * @param {string} field The field name to calculate the general aggregated result.
     * @returns {AggregationResult} The general aggregation result.
     */
    general(field: string): AggregationResult;
}
```

## FeatureSourceFactory Class
This class is a shortcut to build FeatureSource instance.

```javascript
export declare class FeatureSourceFactory {
    /**
     * Parse supported feature source json data into a corresponding FeatureSource instance.
     * @param {any} json The JSON format data of a feature source.
     */
    static parseJSON(json: any): MemoryFeatureSource | ShapefileFeatureSource | undefined;
}
```
