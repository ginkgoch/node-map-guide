# Layer Modules

## Layer Class
Extends `Opener`. 

```javascript
export declare abstract class Layer extends Opener {
    id: string;
    name: string;
    minimumScale: number;
    maximumScale: number;
    visible: boolean;
    margin: number;
    constructor(name?: string);
    abstract envelope(): Promise<Envelope>;
    /**
     * Enlarges the specified envelope based on the `margin` property.
     * @param {IEnvelope} envelope The envelope to enlarge.
     * @param {Render} render The render.
     * @returns {Envelope} The enlarged envelope.
     */
    applyMargin(envelope: IEnvelope, render: Render): Envelope;
    /**
     * Gets a thumbnail image of this layer.
     * @param {number} width The width in pixel of the thumbnail image.
     * @param {number} height The height in pixel of the thumbnail image.
     */
    thumbnail(width?: number, height?: number): Promise<Image>;
    /**
     * Converts this layer into a JSON format data.
     * @returns A JSON format data of this layer.
     */
    toJSON(): any;
    /**
     * Draws this layer with styles and feature source in a restricted envelope.
     * @param {Render} render The renderer that holds the image source and necessary spatial infos.
     */
    draw(render: Render): Promise<void>;
}
```

## FeatureLayer Class
Extends `Layer`. FeatureLayer responses for rendering FeatureSource with styles. 

```javascript
export declare class FeatureLayer extends Layer {
    source: FeatureSource;
    styles: Array<Style>;
    /**
     * Constructs a FeatureLayer instance.
     * @param {FeatureSource} source The feature source where the features are fetched.
     * @param {string} name The name of this layer. Optional with default value `layer-${uuid()}`.
     */
    constructor(source: FeatureSource, name?: string);
    /**
     * Pushes multiple styles into this layer.
     * @param {Array<Style>} styles The styles to render features.
     */
    pushStyles(styles: Array<Style>): void;
    /**
     * Gets the envelope of this layer.
     * @returns {IEnvelope} The envelope of this layer.
     */
    envelope(): Promise<Envelope>;
        type: JSONKnownTypes;
        id: string;
        name: string;
        source: any;
        styles: any[];
        minimumScale: number;
        maximumScale: number;
        visible: boolean;
    }
```

## LayerFactory Class
```javascript
export declare class LayerFactory {
    /**
     * Create {FeatureLayer} by specific url format.
     * @param sourceURL The url to create a {FeatureLayer}.
     * @description
     * {FeatureLayer} is a uniform maintainer for {FeatureSource}. It describes how the features in {FeatureSource} are rendered.
     * This method is a shortcut of creating a feature layer with one line of code.
     * URL is formed with protocol, path and parameters.
     * - Protocol is defined as the abbr. of the feature source. e.g. shp = ShapefileFeatureSource, mem = MemoryFeatureSource.
     * - Path is used for the concrete path or name.
     * - Parameters are used for some optional configurations.
     *
     * 1. Create a FeatureLayer with a ShapefileFeatureSource.
     * ```typescript
     * let layer = FeatureLayerFactory.create(new URL('shp://./cntry02.shp'));
     * ```
     * 2. Create a FeatureLayer with a ShapefileFeatureSource with rs+ flag.
     * ```typescript
     * let layer = FeatureLayerFactory.create(new URL('shp://./cntry02.shp?flag=rs+'));
     * ```
     * 3. Create a FeatureLayer with a MemoryFeatureSource with name = 'dynamic' and fields = ['name', 'recid'].
     * ```typescript
     * let layer = FeatureLayerFactory.create(new URL('mem://dynamic?fields=name|c,recid|n,landlocked|b'));
     * ```
     * @returns {FeatureLayer} The feature layer with specified feature source.
     */
    static create(sourceURL: URL): FeatureLayer;
}
```

## LayerGroup Class
This class represents a collection of a layers. It is used for organize the layer better in the app implementation. For instance, we usually have a set of layers that are stable and not change very often; while another set of layers are stored in memory and change very often, just like dynamic editing or highlighting features. Then we could maintain the two sets of layers in two LayerGroup; then it is easier to manage their rendering or caching.

```javascript
export declare class LayerGroup {
    /**
     * ID of this group. Default value is `group-${uuid()}`.
     */
    id: string;
    /**
     * Name of this group. Default value is `Unknown`.
     */
    name: string;
    /**
     * Indicates whether this group is visible or not.
     */
    visible: boolean;
    /**
     * The FeatureLayer array.
     */
    layers: Array<FeatureLayer>;
    /**
     * Constructs an instance of LayerGroup.
     * @param {Array<FeatureLayer>} layers The layers that are about to add into this group. Pass `undefined` to initiate an empty array.
     * @param {string} name The name of this group.
     */
    constructor(layers?: Array<FeatureLayer>, name?: string);
    /**
     * Gets the envelope of this group.
     * This function unions the envelope of each layers as a minimum envelope to contains all layers.
     * @returns {Envelope} A minimum envelope to contains all the layers inside.
     */
    envelope(): Promise<Envelope>;
