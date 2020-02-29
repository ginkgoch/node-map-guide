# Style Modules

## FillPatternRepeat Type
```javascript
export declare type FillPatternRepeat = 'repeat' | 'repeat-x' | 'repeat-y' | 'no-repeat';
```

## FillPattern Interface
```javascript
export interface FillPattern {
    image: Image;
    repeat?: FillPatternRepeat;
}
```

## FillStyle Class
Extends `Style`. This class represents a style for an area based geometries (e.g. polygon, multi-polygon) only.

```javascript
export declare class FillStyle extends Style {
    /**
     * The fill style with color or pattern.
     * e.g. '#cccccc' | 'gray' | { image: new Image('pattern.png'), repeat: 'repeat' }
     * */
    fillStyle: string | FillPattern;
    /** The stroke width in pixel. */
    lineWidth: number;
    /** The stroke color string. e.g. "#000000" or "black" */
    strokeStyle: string;
    /** The line dash array. e.g. [4, 4] */
    lineDash?: Array<number>;
    /**
     * Constructs a fill style instance.
     * @param {string | FillPattern} fillStyle The fill color string.
     * @param {string} strokeStyle The stroke color string.
     * @param {number} lineWidth The stroke width in pixel.
     * @param {string} name The name of this style.
     */
    constructor(fillStyle?: string | FillPattern, strokeStyle?: string, lineWidth?: number, name?: string);
}
```

## LineStyle Class
Extends `Style`. This class represents a line style.

```javascript
export declare class LineStyle extends Style {
    /** The stroke color string. e.g. "#000000" or "black" */
    strokeStyle: string;
    /** The stroke width in pixel. */
    lineWidth: number;
    /** The line dash array. e.g. [4, 4] */
    lineDash?: Array<number>;
    /** Constructs a line style instance. */
    constructor(strokeStyle?: string, lineWidth?: number, name?: string);
}
```

## PointStyle Class
Extends `Style`. This class represents a simple point style to render point based geometries (point, multi-point).

```javascript
export declare class PointStyle extends Style {
    /**
     * The fill style with color or pattern.
     * e.g. '#cccccc' | 'gray' | { image: new Image('pattern.png'), repeat: 'repeat' }
     * */
    fillStyle: string;
    /** The stroke color string. e.g. "#000000" or "black". */
    strokeStyle: string;
    /** The stroke width in pixel. */
    lineWidth: number;
    /** The radius in pixel for drawing circle or rect. */
    radius: number;
    /** The point symbol type. */
    symbol: PointSymbolType;
    /** The line dash array. e.g. [4, 4] */
    lineDash?: Array<number>;
    /**
     * Constructs a point style instance.
     * @param {string} fillStyle The fill color string. e.g. "#ff0000" or "red".
     * @param {string} strokeStyle The stroke color string. e.g. "#000000" or "black".
     * @param {number} lineWidth The stroke width in pixel.
     * @param {number} radius The radius in pixel for drawing circle or rect.
     * @param {PointSymbolType} symbol The point symbol type.
     * @param {string} name The name of this style.
     */
    constructor(fillStyle?: string, strokeStyle?: string, lineWidth?: number, radius?: number, symbol?: PointSymbolType, name?: string);
}
```

## Style Class
This class represents a base class of all styles.

```javascript
export declare abstract class Style {
    /**
     * The id of this style. Default value is `style-${uuid()}`.
     */
    id: string;
    /**
     * The geometry type that is proper for this style. e.g. Polygon is proper for FillStyle, not proper for polyline.
     */
    type: string;
    /**
     * The name of this style.
     */
    name: string;
    /**
     * The maximum visible scale for drawing this style.
     */
    maximumScale: number;
    /**
     * The minimum visible scale for drawing this style.
     */
    minimumScale: number;
    /**
     * The main visible switcher of this style.
     */
    visible: boolean;
    /**
     * Represents the default implementation of base style class. The following items are set as default values.
     * 1. id = `style-${uuid()}`
     * 2. maximumScale = positive_infinity
     * 3. minimumScale = 0
     * @param {string} name The name of this style. Optional with default name `unknown`.
     */
    constructor(name?: string);
    /**
     * Collects the required field names that will be used for rendering.
     * @returns {string[]} The required field names that will be used for rendering.
     */
    fields(): string[];
    /**
     * Draws a feature with this style.
     * @param {IFeature} feature The feature to draw.
     * @param {Render} render The renderer to draw.
     */
    draw(feature: IFeature, render: Render): void;
    /**
     * Draws multiple features with this style.
     * @param {IFeature[]} features The features to draw.
     * @param {Render} render The renderer to draw.
     */
    drawAll(features: IFeature[], render: Render): void;
    /**
     * Converts this style to a JSON format data.
     * @returns {any} The JSON format data.
     */
    toJSON(): any;
    /**
     * @deprecated Use htmlStyle() instead.
     * @ignore
     */
    props(): any;
    /**
     * Collects all the necessary raw HTML style that will be used.
     */
    htmlStyle(): any;
}
```

## TextStyle Class
Extends `Style`. This class represents text style for drawing text based on feature properties on various geometries.

```javascript
export declare class TextStyle extends Style {
    /**
     * The text content to draw. It can be either a concrete string with or without a placeholder to replace on the fly.
     *
     * Renders "FOO" as text content on all features.
     * ```typescript
     * textStyle.content = "FOO";
     * ```
     *
     * Renders a value based on a field name "NAME".
     * ```typescript
     * textStyle.content = "[NAME]";
     * ```
     *
     * Renders values based on multiple fields such as "NAME" and "Hobby", and join them with some static string.
     * ```typescript
     * textStyle.content = "Hi [NAME], I like [Hobby]";
     * ```
     */
    content: string | undefined;
    /** The font string including either font family or size or both. e.g. "ARIAL 12px". */
    font: string;
    /** The font color string. e.g. "#000000" or "black". */
    fillStyle: string;
    /** The text alignment. */
    textAlign: "start" | "end" | "left" | "right" | "center";
    /** The outline color string on text. If it is not set, no outline will be drawn. */
    strokeStyle: string | undefined;
    /** The outline stroke width in pixel on text. If it is zero, no outline will be drawn. */
    lineWidth: number;
    /** Use `centroid` or `interior` as text location. Default is `centroid`. */
    location: 'centroid' | 'interior';
    /**
     * Constructs a text style instance.
     * @param {string|undefined} content The text content to draw. It can be either a concrete string with or without a placeholder to replace on the fly.
     *
     * Renders "FOO" as text content on all features.
     * ```typescript
     * textStyle.content = "FOO";
     * ```
     *
     * Renders a value based on a field name "NAME".
     * ```typescript
     * textStyle.content = "[NAME]";
     * ```
     *
     * Renders values based on multiple fields such as "NAME" and "Hobby", and join them with some static string.
     * ```typescript
     * textStyle.content = "Hi [NAME], I like [Hobby]";
     * ```
     *
     * @param {string} fillStyle The font color string. e.g. "#000000" or "black".
     * @param {string} font The font string including either font family or size or both. e.g. "ARIAL 12px".
     * @param {string} name The name of this style.
     */
    constructor(content?: string, fillStyle?: string, font?: string, name?: string);
    /**
     * Collects the required field names that will be used for rendering.
     * @returns {string[]} The required field names that will be used for rendering.
     */
    fields(): string[];
    /**
     * Normalize multiple font settings into a HTML font expression.
     * @param {string} fontFamily The font family. e.g. "arial".
     * @param {number} fontSize The font size. Default size is 12.
     * @param {FontWeight} fontWeight The font weight.
     * @param {FontStyle} fontStyle The font style.
     * @returns {string} A normalized font style string. e.g. "italic small-caps bold 12px arial".
     */
    static normalizeFont(fontFamily?: string, fontSize?: number, fontWeight?: FontWeight, fontStyle?: FontStyle): string;
}
```
