# Style Advanced Modules

## AutoStyleOptions Interface
```javascript
export interface AutoStyleOptions {
    fromColor?: string;
    toColor?: string;
    strokeColor?: string;
    strokeWidth?: number;
    radius?: number;
    symbol?: PointSymbolType;
}
```

## ClassBreakStyle Class
Extends `Style`. This class represents a style to render different sub-styles based on the break down values.

```javascript
export declare class ClassBreakStyle extends Style {
    /**
     * The field name where to fetch the values to break down.
     */
    field: string;
    /**
     * classBreaks The break down sub-styles and its corresponding value.
     */
    classBreaks: Array<ClassBreakItem>;
    /**
     * Constructs a class break style instance.
     * @param {string} field The field name where to fetch the values to break down.
     * @param {Array<ClassBreakItem>} classBreaks The break down sub-styles and its corresponding value.
     * @param {string} name The name of this style.
     */
    constructor(field?: string, classBreaks?: Array<ClassBreakItem>, name?: string);
    /**
     * Collects the required field names that will be used for rendering.
     */
    fields(): string[];
    /**
     * This is a shortcut function to automatically generate class breaks based on the values,
     * and assign a gradient colors to each item.
     * @deprecated Use `autoByRange` function instead.
     * @param {'fill'|'linear'|'point'} styleType The style type of the sub-styles.
     * @param {string} field The field name where the value is fetched.
     * @param {number} maximum The maximum value to break down.
     * @param {number} minimum The minimum value to break down.
     * @param {number} count The break down items count to generate.
     * @param {string} fromColor The fill color begins from.
     * @param {string} toColor The fill color ends with.
     * @param {string} strokeColor The stroke color.
     * @param {number} strokeWidth The stroke width in pixel.
     * @param {number} radius The radius for points symbols.
     * @param {PointSymbolType} symbol The point symbol type.
     * @returns {ClassBreakStyle} A class break style with sub styles filled with the specified conditions.
     */
    static auto(styleType: 'fill' | 'linear' | 'point', field: string, maximum: number, minimum: number, count: number, fromColor?: string, toColor?: string, strokeColor?: string, strokeWidth?: number, radius?: number, symbol?: PointSymbolType): ClassBreakStyle;
    /**
     * This is a shortcut function to automatically generate class breaks based on the values,
     * and assign a gradient colors to each item.
     * @param {'fill'|'linear'|'point'} styleType The style type of the sub-styles.
     * @param {string} field The field name where the value is fetched.
     * @param {number} maximum The maximum value to break down.
     * @param {number} minimum The minimum value to break down.
     * @param {number} count The break down items count to generate.
     * @param {AutoStyleOptions} autoStyleOptions The auto style options.
     * @returns {ClassBreakStyle} A class break style with sub styles filled with the specified conditions.
     */
    static autoByRange(styleType: 'fill' | 'linear' | 'point', field: string, maximum: number, minimum: number, count: number, autoStyleOptions?: AutoStyleOptions): ClassBreakStyle;
    static autoByAggregator(styleType: 'fill' | 'linear' | 'point', field: string, aggregator: PropertyAggregator, breakCount: number, breakBy?: 'value' | 'position', autoStyleOptions?: AutoStyleOptions): ClassBreakStyle;
    static autoByValues(styleType: 'fill' | 'linear' | 'point', field: string, breakDownValues: Array<Range>, autoStyleOptions?: AutoStyleOptions): ClassBreakStyle;
}
```

## ClassBreakItem Interface
Extends `Range`. This interface represents a structure of a class break item.

```javascript
export interface ClassBreakItem extends Range {
    style: Style;
}
```

## Range Interface
```javascript
export interface Range {
    minimum: number;
    maximum: number;
}
```

## GeneralStyle Class
Extends `Style`. This class represents a general style regardless the geometry type to draw.

```javascript
export declare class GeneralStyle extends Style {
    /**
     * The fill style with color or pattern.
     * e.g. '#cccccc' | 'gray' | { image: new Image('pattern.png'), repeat: 'repeat' }
     * */
    fillStyle: string | FillPattern;
    /** The stroke width in pixel. */
    lineWidth: number;
    /** The stroke color string. e.g. "#000000" or "black". */
    strokeStyle: string;
    /** The symbol type for point geometry. */
    symbol: PointSymbolType;
    /** The radius width in pixel for point geometry. */
    radius: number;
    /** The line dash array. e.g. [4, 4] */
    lineDash?: Array<number>;
    /** Constructs a general style instance that regardless the geometry type to draw. */
    constructor(fillStyle?: string | FillPattern, strokeStyle?: string, lineWidth?: number, radius?: number, symbol?: PointSymbolType, name?: string);
}
```

## IconStyle Class
Extends `Style`. This class represents an icon style to draw icons on map.

```javascript
export declare class IconStyle extends Style {
    /** The image source to draw. */
    icon: Image;
    /** The horizontal offset to draw. */
    offsetX: number;
    /** The vertical offset to draw. */
    offsetY: number;
    /**
     * Constructs an icon style instance.
     * @param {Image} icon The image source to draw.
     * @param {number} offsetX The horizontal offset to draw.
     * @param {number} offsetY The vertical offset to draw.
     * @param {string} name The name of this style.
     */
    constructor(icon?: Image, offsetX?: number, offsetY?: number, name?: string);
}
```

## StyleUtils Class
This class represents an internal utilities to generate random colors and related resources for styles.

```javascript
export declare class StyleUtils {
    /**
     * Gets a color or random color if not specified.
     * @category styles
     */
    static colorOrRandom(color?: string, option?: RandomColorOption): string;
    /** Gets a color or random color in dark color family if not specified. */
    static colorOrRandomDark(color?: string): string;
    /** Gets a color or random color in light color family if not specified. */
    static colorOrRandomLight(color?: string): string;
    /**
     * Gets a color arrays between two colors.
     * @param {color} count The color count to generate.
     * @param {string} fromColor The color begins from. Optional with default value - a random color.
     * @param {string} toColor The color ends with. Optional with default value - a random color.
     * @returns {Array<string>} A color list between the two colors.
     */
    static colorsBetween(count: number, fromColor?: string, toColor?: string): string[];
}
```

## ValueStyle Class
Extends `Style`. This class represents a value style which allows to set various sub-styles based on a field value.

```javascript
export declare class ValueStyle extends Style {
    /** A value item list with the definition of value and its corresponding style. */
    items: ValueItem[];
    /** The field name where the value is fetched from. */
    field: string;
    /** Constructs a value style instance. */
    constructor(field?: string, items?: ValueItem[], name?: string);
    /**
     * Collects the required field names that will be used for rendering.
     * @returns {string[]} The required field names that will be used for rendering.
     */
    fields(): string[];
    /**
     * @deprecated Use `auto` function instead.
     * This is a shortcut function to automatically generate value items based on the distinct values,
     * and assign a gradient colors to each item.
     * @param {'fill'|'linear'|'point'} styleType The style type of the sub-styles.
     * @param {string} field The field name where the value is fetched.
     * @param {any[]} values The distinct value array.
     * @param {string} fromColor The fill color begins from.
     * @param {string} toColor The fill color ends with.
     * @param {string} strokeColor The stroke color.
     * @param {number} strokeWidth The stroke width in pixel.
     * @param {number} radius The radius for points symbols.
     * @param {PointSymbolType} symbol The point symbol type.
     * @returns {ValueStyle} A value style with sub styles filled with the specified conditions.
     */
    static auto(styleType: 'fill' | 'linear' | 'point', field: string, values: any[], fromColor?: string, toColor?: string, strokeColor?: string, strokeWidth?: number, radius?: number, symbol?: PointSymbolType): ValueStyle;
    static autoByValues(styleType: 'fill' | 'linear' | 'point', field: string, values: any[], autoStyleOptions?: AutoStyleOptions): ValueStyle;
}
```
