# Map Modules

## MapEngine Class
This class represents a complete structure of a map instance. It wraps necessary information that are used for a map rendering. A complete demo to create a map and draw as an image, then save to disk.

```javascript
export declare class MapEngine {
    /**
     * The name of map.
     */
    name: string;
    /**
     * This map's spatial reference system. It applies as the target SRS to each feature source.
     */
    srs: Srs;
    /**
     * The width of this map
     */
    width: number;
    /**
     * The height of this map.
     */
    height: number;
    /**
     * The background color. Default to undefined which means transparent background.
     */
    background?: string;
    /**
     * The maximum visible scale. Defaults to positive infinity.
     */
    maximumScale: number;
    /**
     * The minimum visible scale. Default to 0.
     */
    minimumScale: number;
    /**
     * The layer groups that maintains the layers inside for rendering.
     */
    groups: Array<LayerGroup>;
    /**
     * The scales list for defining the zoom levels for rendering.
     */
    scales: Array<number>;
    /**
     * Indicates the tile origin for calculating the tile system. Default is calculating from the upper left corner.
     *
     * Some tiling system is calculating from lower right corner, this is the property to set.
     */
    origin: TileOrigin;
    renderContextOptions: RenderContextOptions;
    /**
     * Constructs a map engine instance.
     * @param {number} width The width of map. Optional with default value 256 px.
     * @param {number} height The height of map. Optional with default value 256 px.
     * @param {Srs} srs The spatial reference system of this map. Optional with default value `EPSG:3857`.
     * @param {Array<number>} scales The scale list that represents the zoom levels for map rendering.
     */
    constructor(width?: number, height?: number, srs?: string, scales?: Array<number>);
    /**
     * Converts this map instance into a JSON format data.
     * @returns {any} A JSON format data that is converted from this map engine.
     */
    toJSON(): any;
    /**
     * Parses the map instance from a specified JSON format data.
     * The JSON data must match the map engine schema, otherwise, it throws exception.
     * @param {any} json The JSON format data that matches the map engine schema.
     * @returns {MapEngine} The map engine instance that is parsed from the JSON format data.
     */
    static parseJSON(json: any): MapEngine;
    static fromOptions(mapOptions: MapOptions): MapEngine;
    /**
     * Gets the envelope of this map. It unions all the layers inside and returns a minimum envelope that includes all the visible layers.
     * @returns {Envelope} The envelope of this map.
     */
    envelope(): Promise<Envelope>;
    /**
     * This is a shortcut function for simply pushing a layer into a group. If the group doesn't exist, it automatically creates a new group to reserve the layer.
     *
     * @param {FeatureLayer} layer The layer to push into this map.
     * @param {string} groupName The group name to reserve the layer. If there is no group matches the specific group name, a new group will be pushed into the map. Optional with default value `Default`.
     */
    pushLayer(layer: FeatureLayer, groupName?: string): void;
    /**
     * This is a shortcut function for simply pushing layers into a group. If the group doesn't exist, it automatically creates a new group to reserve the layer.
     *
     * @param {Array<FeatureLayer>} layers The layers to push into this map.
     * @param {string} groupName The group name to reserve the layer. If there is no group matches the specific group name, a new group will be pushed into the map. Optional with default value `Default`.
     */
    pushLayers(layers: Array<FeatureLayer>, groupName?: string): void;
    /**
     * Pushes multiple groups into map.
     * @param {...Array<LayerGroup>} groups The groups to push into this map.
     */
    pushGroups(...groups: Array<LayerGroup>): void;
    /**
     * A shortcut function to look for a group by name.
     * @param {string} name The group name.
     * @returns {LayerGroup|undefined} The layer group that matches the name. If not found, returns undefined.
     */
    group(name: string): LayerGroup | undefined;
    /**
     * A shortcut function to look for a layer by name through all groups. If the group name is defined, it only looks for the layer from the group.
     * @param {string} name The layer name to look for.
     * @param {string} groupName The group name where the layer is looking for.
     * @returns {FeatureLayer|undefined} The layer that matches the name. If not found, returns undefined.
     */
    layer(name: string, groupName?: string): FeatureLayer | undefined;
    /**
     * A shortcut function to look for a layer by id through all groups.
     * @param {string} id The identity of a layer.
     * @returns {FeatureLayer|undefined} The layer that matches the name. If not found, returns undefined.
     */
    layerByID(id: string): FeatureLayer | undefined;
    /**
     * Query features through all feature layers with spatial relationship - intersection.
     * @param geom Geometry to find intersection.
     * @param geomSrs Geometry srs to find intersection.
     * @param zoomLevel Zoom level number. Starts from 0.
     * @param pointTolerance Tolerance for point geometry.
     * @returns {Array<{layerID:string, features: Feature[]}>} The intersected features that are categorized by layers.
     */
    intersection(geom: Geometry, geomSrs: string, zoomLevel: number, pointTolerance?: number, includeInvisibleLayers?: boolean, layersToQuery?: string[]): Promise<Array<{
        layer: string;
        features: Feature[];
    }>>;
    /**
     * @deprecated This method is deprecated. Please call image(envelope?: IEnvelope) instead.
     * @ignore
     */
    draw(envelope?: IEnvelope): Promise<Image>;
    /**
     * Gets an image of this map instance.
     * @param {IEnvelope} envelope The envelope that the viewport will be rendered. Optional with the minimal envelope of this map.
     * @returns {Image} The image that is rendered with this map instance.
     */
    image(envelope?: IEnvelope): Promise<Image>;
    /**
     * This is a shortcut function to render this map instance with XYZ tiling system.
     *
     * NOTE: XYZ rendering requires some other settings, it is automatically reflected to the properties on this map instance. e.g.
     *
     * tile width -> map.width
     * tile height -> map.height
     * tile zoom levels -> map.scales
     * tile origin -> map.origin
     * ...
     *
     * @param {number} x The column number. Start from 0.
     * @param {number} y The row number. Start from 0.
     * @param {number} z The zoom level number. Start from 0.
     */
    xyz(x?: number, y?: number, z?: number): Promise<Image>;
}
```
