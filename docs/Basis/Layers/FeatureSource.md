# FeatureSource Modules

## CSVFieldOptions Interface
```javascript
export interface CSVFieldOptions {
    fields?: string[];
    hasFieldsRow?: boolean;
    geomField?: string | {
        x: string;
        y: string;
    };
}
```

## CSVFeatureSource Class
Extends `MemoryFeatureSource`. 

```javascript
export declare class CSVFeatureSource extends MemoryFeatureSource {
    filePath?: string | undefined;
    fieldOptions: CSVFieldOptions;
    delimiter: string;
    /**
     * Constructs a `CSVFeatureSource` instance.
     * @param {string} filePath The CSV file path.
     * @param {CSVFieldOptions} fieldOptions The CSV field options. e.g. { geoField: {x:'longitude', y:'latitude'}, hasFieldsRow: true }
     * @param {string} delimiter The delimiter.
     * @param {string} name The feature source name.
     */
    constructor(filePath?: string | undefined, fieldOptions?: CSVFieldOptions, delimiter?: string, name?: string);
    /**
     * @override
     */
    get editable(): boolean;
    static parseJSON(json: any): CSVFeatureSource;
    static create(filePath: string, delimiter: string, fieldOptions: CSVFieldOptions, features: Array<Feature>, encoding?: string): void;
}
```

## FieldFilters Type
This is a new type that could be replaced with 'all', 'none' or a string array.

```javascript
export declare type FieldFilters = 'all' | 'none' | string[];
```

## DynamicField Interface
```javascript
export interface DynamicField {
    name: string;
    fieldsDependOn: string[];
    mapper: (f: IFeature) => any;
}
```

## SpatialQueryRelationship Type
```javascript
export declare type SpatialQueryRelationship = 'intersection' | 'within' | 'disjoint' | 'overlap' | 'touch';
```

## FeatureSource Class
Extends `Opener`. This class represents a base class of all feature source. It provides the portal for developers to CRUD features from the the database. A feature source stands for a feature based database. For instance, ShapeFile is a popular feature based database. @see {@link ShapefileFeatureSource} for reading Shapefile data format.

```javascript
export declare abstract class FeatureSource extends Opener {
    /**
     * The name of feature source.
     */
    name: string;
    /**
     * The projection of feature source.
     */
    projection: Projection;
    /**
     * The feature geometry type.
     */
    type: JSONKnownTypes;
    /**
     * Enable or disable the spatial index if it is being set.
     * It works when this property is set to `true` and the index property is set properly.
     *
     * The default value is true.
     */
    indexEnabled: boolean;
    /**
     * The spatial index instance that is used to speed up the query performance.
     * It is optional property.
     */
    index?: BaseIndex;
    decorateFeature?: (f: Feature) => Feature;
    dynamicFields: Array<DynamicField>;
    /**
     * This is the constructor of feature source.
     *
     * It sets the default name as `Unknown`
     * and initialize an undefined from and to projection instance.
     */
    constructor();
    /**
     * Gets features by condition; if the condition is not set, all features will be fetched.
     * @param {IEnvelope} envelope The condition to filter out the features within the specified envelope. Optional with default value undefined.
     * @param {FieldFilters} fields The fields will come with the returned feature array. Optional with default value undefined.
     * @returns {Promise<Feature[]>} The feature array that matches the specified condition.
     */
    features(envelope?: IEnvelope, fields?: FieldFilters): Promise<Feature[]>;
    /**
     * Query features spatially with a specific relationship.
     * @param {SpatialQueryRelationship} queryRelationship The relationship to query. NOTE: 'disjoint' requires to fetch all features in a data source. If the source has too many features, this operation will be pretty slow. Consider to set viewportEnvelope to reduce the querying area.
     * @param {Geometry} geom The geometry to compare relationship among the features in this feature source.
     * @param {string | Srs | undefined} geomSrs The geometry SRS. Default is undefined which means using the geometry directly.
     * @param {FieldFilters | undefined} fields The fields included in the returning features.
     * @param {IEnvelope | undefined} viewportEnvelope The restriction area of querying features.
     */
    query(queryRelationship: SpatialQueryRelationship, geom: Geometry, geomSrs?: string | Srs, fields?: FieldFilters, viewportEnvelope?: IEnvelope): Promise<Array<Feature>>;
    /**
     * Gets a feature instance by its id. If it doesn't exist, returns undefined.
     * @param {number} id The id of the feature to find.
     * @param {FieldFilters} fields The field filters that indicate the fields will be fetched with the returned feature instance.
     * @returns {Promise<Feature|undefined>} The feature that id equals to the specified id.
     */
    feature(id: number, fields?: FieldFilters): Promise<Feature | undefined>;
    /**
     * Gets the features count.
     * @returns The feature count.
     */
    count(): Promise<number>;
    /**
     * Gets all fields info in this feature source.
     * @returns {Promise<Field[]>} An array of fields info in this feature source.
     */
    fields(): Promise<Field[]>;
    /**
     * This is an aggregator utility based on the properties.
     * It provides to find min, max, average, distinct etc. values based on the feature source properties.
     *
     * Use case:
     *
     * @param {FieldFilters} fields The fields that will be included for aggregation.
     * @returns {PropertyAggregator} A PropertyAggregator instance.
     */
    propertyAggregator(fields?: FieldFilters): Promise<PropertyAggregator>;
    /**
     * This methods fetches properties from all features in this feature source.
     * @param fields The fields filter.
     * @returns {Promise<Array<Map<string, any>>>} An array of properties from all features in this feature source.
     */
    properties(fields?: FieldFilters): Promise<Array<Map<string, any>>>;
    /**
     * Gets the envelope (bounding box) of this feature source.
     * @returns {Promise<Envelope>} The envelope (bounding box) of this feature source.
     */
    envelope(): Promise<Envelope>;
    /**
     * A shortcut of getting source SRS (spatial reference system) from the projection property.
     * @returns {string|undefined} Either the concrete SRS or undefined if it is not set nor detected.
     */
    get srs(): string | undefined;
    /**
     * A shortcut of setting source SRS (spatial reference system) from the projection property.
     */
    set srs(srs: string | undefined);
    /**
     * Gets whether this feature source is editable (creating, updating and deleting).
     * @returns {boolean} Whether this feature source is editable.
     */
    get editable(): boolean;
    /**
     * Pushes a feature into this feature source.
     * @param {IFeature} feature The feature to push into this feature source.
     * If this feature source's source and target SRS are defined, this feature must be the same SRS as the target SRS of this feature source.
     */
    push(feature: IFeature): Promise<void>;
    /**
     * Updates an existing feature in this feature source.
     * @param {IFeature} feature The feature to update in this feature source.
     * If this feature source's source and target SRS are defined, this feature must be the same SRS as the target SRS of this feature source.
     */
    update(feature: IFeature): Promise<void>;
    /**
     * Update properties only for a feature for performance consideration. If it is not implemented, please call `update` method instead.
     * @param {IFeature} feature The feature to update in this feature source.
     */
    updateProperties(feature: IFeature): Promise<void>;
    /**
     * Removes a feature by a specified feature id.
     * @param {number} id The feature id to remove.
     */
    remove(id: number): Promise<void>;
    /**
     * Pushes a new field into this feature source.
     * @param {Field} field A new field to push into this feature source.
     */
    pushField(field: Field): Promise<void>;
    /**
     * Updates an existing field info by field name.
     * @param {string} sourceFieldName The source field name to update.
     * @param {Field} newField A new field to replace to the old field.
     */
    updateField(sourceFieldName: string, newField: Field): Promise<void>;
    /**
     * Removes a field by field name.
     * @param {string} fieldName A field name to remove.
     */
    removeField(fieldName: string): Promise<void>;
    /**
     * Flush the field changes into this feature source storage.
     */
    flushFields(): Promise<void>;
    /**
     * Converts this feature source into JSON data.
     */
    toJSON(): any;
}
```

## GeoJSONFeatureSource Class
Extends `MemoryFeatureSource`. 

```javascript
export declare class GeoJSONFeatureSource extends MemoryFeatureSource {
    constructor(geoJSON?: string | any, name?: string);
    get geoJSON(): any | string | undefined;
    set geoJSON(geoJSON: any | string | undefined);
    /**
     * @override
     */
    get editable(): boolean;
    static parseJSON(json: any): GeoJSONFeatureSource;
}
```

## MemoryFeatureSource Class
Extends `FeatureSource`. This class represents a feature source that maintains features in memory. This feature source is used to store temporary features. For instance, highlight, editing or adding feature buffer are all alow to store in this memory. Remind to clear it to release the memory usage. Watch out the memory usage (if too many features are about to push into this source), please consider other feature source with persistent storage.

```javascript
export declare class MemoryFeatureSource extends FeatureSource {
    /**
     * Constructs an MemoryFeatureSource instance.
     * @param {IFeature[]} features The features to be pushed into this feature source. It is optional.
     * @param fields
     * @param name
     */
    constructor(features?: IFeature[], fields?: Field[], name?: string);
    get internalFeatures(): Feature[];
    /**
     * Parses the specified JSON format data as `MemoryFeatureSource`.
     * Note: If the data doesn't match the schema, it throws exception.
     * @param {any} json The JSON format data that matches the `MemoryFeatureSource` schema.
     * @returns {MemoryFeatureSource} A memory feature source instance.
     */
    static parseJSON(json: any): MemoryFeatureSource;
    /**
     * Gets whether this feature source is editable (creating, updating and deleting).
     * @returns {boolean} This is feature source is always editable.
     */
    get editable(): boolean;
    /**
     * Indicates whether this feature source is necessary to manually open before doing CRUD operation.
     * @returns False means this feature source is allow to CRUD operation.
     */
    get _openRequired(): boolean;
}
```

## ShapefileFeatureSource Class
Extends `FeatureSource`. This class represents a feature source that CRUD records from shapefile.

```javascript
export declare class ShapefileFeatureSource extends FeatureSource {
    /**
     * The file system flags to open the shapefile.
     * @see {@link https://nodejs.org/api/fs.html#fs_file_system_flags} for options.
     */
    flag: string;
    /**
     * Constructs a shapefile feature source instance.
     * @param {string} filePath The shape file path name.
     * @param {string} flag The file system flags to open the shapefile.
     * @param {string} name The name of this feature source.
     */
    constructor(filePath?: string, flag?: string, name?: string);
    /**
     * filePath Gets the path file name of the shapefile.
     */
    get filePath(): string;
    /**
     * Sets the path file name of the shapefile.
     */
    set filePath(filePath: string);
    /**
     * Gets the shapefile type.
     */
    get shapeType(): ShapefileType;
    /**
     * Parses the JSON data into a shapefile source instance.
     * @param {any} json The JSON data to convert.
     * @returns {ShapefileFeatureSource} The shapefile source instance that is converted from the JSON data.
     */
    static parseJSON(json: any): ShapefileFeatureSource;
    /**
     * Gets whether this feature source is editable (creating, updating and deleting).
     * @returns {boolean} This is feature source is always editable.
     */
    get editable(): boolean;
    /**
     * Builds index for this shapefile source.
     * @param {boolean} overwrite True means overwrite if the target index file exists;
     * otherwise, skip building index if exists. Default value is false.
     */
    buildIndex(overwrite?: boolean): void;
    static createEmpty(filePath: string, fileType: ShapefileType, fields: Field[]): ShapefileFeatureSource;
    copySchemaAs(targetFilePath: string): Promise<ShapefileFeatureSource>;
}
```
