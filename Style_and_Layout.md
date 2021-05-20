# Style and Layout
Relearn CSS and layout systematically

## Resources
1. [CSS Home](https://www.w3.org/Style/CSS/)
1. [CSS Spec 2.2](https://www.w3.org/TR/CSS22/Overview.html#minitoc)
1. [CSS MDN](https://developer.mozilla.org/en-US/docs/Web/CSS)

## Style
### What is CSS?
1. Browser will render HTML with default style to make it readable basically
    * Headings will look larger than regular text
    * Links are colored and underlined to distinguish them from the text
1. With more specific CSS, the rendered document can be great-looking
    * Appearance: text color and heading size
    * Layout: normal flow, absolute, flexbox
    * Advanced: animation, gradient image
1. A rule-based language
    ```css
    [selector] {
        property: value; /* a declaration */
    }
    ```

### Apply CSS to a document
#### External stylesheet
Reference an external CSS stylesheet from an HTML \<`link`> element
```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>My CSS experiment</title>
    <!-- Inside a subdirectory called styles inside the current directory -->
    <link rel="stylesheet" href="styles/style.css">
    <!-- Inside a subdirectory called general, which is in a subdirectory called styles, inside the current directory -->
    <link rel="stylesheet" href="styles/general/style.css">
    <!-- Go up one directory level, then inside a subdirectory called styles -->
    <link rel="stylesheet" href="../styles/style.css">
  </head>
  <body>
    <h1>Hello World!</h1>
    <p>This is my first CSS example</p>
  </body>
</html>
```
#### Internal stylesheet
Place CSS inside a \<`style`> element contained inside the HTML \<`head`>.
```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>My CSS experiment</title>
    <style>
      h1 {
        color: blue;
        background-color: yellow;
        border: 1px solid black;
      }
    </style>
  </head>
  <body>
    <h1>Hello World!</h1>
    <p>This is my first CSS example</p>
  </body>
</html>
```
#### Inline styles
Inline styles are CSS declarations that affect a single HTML element, contained within a `style` attribute.
```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>My CSS experiment</title>
  </head>
  <body>
    <h1 style="color: blue;background-color: yellow;border: 1px solid black;">Hello World!</h1>
    <p style="color:red;">This is my first CSS example</p>
  </body>
</html>
```

#### Attention
1. If document includes multiple \<`style`> and \<`link`> elements , they will be applied to the DOM in the order they are included in the document
1. Exclude `!important`, inline styles have the highest specificity.

### CSS Rules
#### CSS Selector
`(selector, element) -> boolean`  
Describe the elements that will have the specified style applied to them.

  | Selector | Example | Specificity(A,B,C) |
  | -- | -- | -- |
  | Universal | `*` | 0 |
  | Negation Pseudo-Class | `:not()` | 0 for itself, inner selector counts |
  | Type | `h` | C |
  | Pseudo-Element | `p::first-line` | C |
  | Attribute | `li[class^="box-"]` | B |
  | Class | `.box` | B |
  | Pseudo-Class | `p:first-child` | B |
  | ID | `#unique` | A |

  | Combinator | Example | Specificity(A,B,C) |
  | -- | -- | -- |
  | Descendant | `article p` | (0,0,2)|
  | Child | `#uniq > p` | (1,0,1) |
  | Adjacent sibling | `h1 + *[rel=up]` | (0,1,1) |
  | General sibling | `.level ~ p:first-child` | (0,2,1) |

  The numbers `(A,B,C)` (in a number system with a large base) gives the specificity

#### CSS Declarations
1. A CSS declaration consists of three parts:
    * **Property**: Human-readable identifiers that indicate the stylistic features
    * **Value**: Each property is assigned a value. This value indicates how to style the property.
    * **Important**: an important flag which is initially unset.
1. `!important`
    * This is used to make a particular declaration the most specific thing
    * **Never use it unless you absolutely have to**
    * Override `!important`
        * Another `!important` flag with higher specificity.
        * Another `!important` flag with the same specificity in the later source order
1. Functions: a type of CSS value
    * [calc()](https://developer.mozilla.org/en-US/docs/Web/CSS/calc()): perform calculations when specifying CSS property values
1. at-rules: 
    * [@media](https://developer.mozilla.org/en-US/docs/Web/CSS/Media_Queries/Using_media_queries): a conditional group rule whose condition is a media query
#### Conflicting Rules
  * **Specificity**: more specific selectors override the conflicting styles.  
    `(important, inline, id, attribute, element)`  
  * **Cascade**: having same specificity, the later styles replace conflicting styles that appear earlier

## Layout
A document is laid out by transforming the elements into a set of boxes, whose size, position, and stacking level comes from the values of their CSS properties.
### The CSS Box Model
1. Each box has a `content area` and optional surrounding `padding`, `border`, and `margin` areas
    * **Content box**
        * contains text, descendant boxes, replaced element content(image)
        * depend on the element content and/or its *containing block* size
        * sized using `width` and `height`(block box)
    * **Padding box**: sized using `padding`
    * **Border box**: sized using `border`
    * **Margin box**
        * sized using `margin`
        * negative value is valid
    * **Scrollbar**: inserted between the inner border edge and the outer padding edge
    ```
      |-------------------------------------------------|
      |                  margin-top                     |
      |    |---------------------------------------|    |
      |    |             border-top                |    |
      |    |    |--------------------------|--|    |    |
      |    |    |       padding-top        |##|    |    |    BL = border-left
      |    |    |    |----------------|    |##|    |    |    BR = border-right
      |    |    |    |                |    |  |    |    |    ML = margin-left
      | ML | BL | PL |  content box   | PR |SW| BR | MR |    MR = margin-right
      |    |    |    |                |    |  |    |    |    PL = padding-left
      |    |    |    |----------------|    |  |    |    |    PR = padding-right
      |    |    |      padding-bottom      |  |    |    |    SC = scroll corner
      |    |    |--------------------------|--|    |    |    SW = scrollbar width
      |    |    |     scrollbar height ####|SC|    |    |
      |    |    |-----------------------------|    |    |
      |    |           border-bottom               |    |
      |    |---------------------------------------|    |
      |                margin-bottom                    |
      |-------------------------------------------------|
    ```
1. [The containing block](https://www.w3.org/TR/css-display-3/#containing-block)
    * A containing block is a rectangle, not a box.
    * The size and position of an element are often impacted by its *containing block*
      * `with: <percentage>;` relative to the *width* of the containing block.
      * `height: <percentage>;` relative to the *height* of the containing block.
      * `padding: <percentage>;` relative to the *width* of the containing block.
      * `margin: <percentage>;` relative to the *width* of the containing block.
      * offset properties of [absolutely positioned elements](https://developer.mozilla.org/en-US/docs/Web/CSS/position#types_of_positioning): computed from the containing block.
    * [Identifying the containing block](https://developer.mozilla.org/en-US/docs/Web/CSS/Containing_block#identifying_the_containing_block)
1. Relevant CSS properties
    * `box-sizing: border-box;`: The `width` and `height` properties include the content, padding, and border
    * `background: red;`: cover the content, padding, and border areas of a box
    * `display: inline-block;`: allows to set `width` and `height` properties compare to `display: inline;`

### CSS Flow Layout ([Normal Flow](https://www.w3.org/TR/CSS22/visuren.html#normal-flow))
The default way to lay out all elements(boxes) in documents.
1. There exists two types of boxes
    * **Block Boxes**: `display: block;`
      * The box will break onto new line
      * The box will extend in the inline direction to fill the space available in its container
      * new line + fill space = as wide as its container
      * The `width` and `height` properties are respected
      * Examples: \<`div`>, \<`p`>
    * **Inline Boxes**: `display: inline;`
      * The box will **not** break onto new line
      * The `width` and `height` properties will not apply, only takes up as much width as necessary
      * The rectangular area that contains the boxes that form a **line box**
          * A line box is always tall enough for all of the boxes it contains. ([line-height](https://developer.mozilla.org/en-US/docs/Web/CSS/line-height) specify the *minimal* height)
          * `line-height: normal;` roughly means 1.2 * `font-size`
          * The height if a line box is calculated as [follows](https://www.w3.org/TR/CSS22/visudet.html#line-height)
          * When the height of a box is less than the height of the line box containing it, the vertical alignment of that box is determined by the [vertical-align](https://developer.mozilla.org/en-US/docs/Web/CSS/vertical-align)
          * When the total width of the inline boxes on a line is less than the width of the line box containing them, their horizontal distribution within the line box is determined by the [text-align](https://developer.mozilla.org/en-US/docs/Web/CSS/text-align)
      * Examples: \<`a`>, \<`span`>
1. A **block container box** either contains only block boxes or contains only inline boxes
    * `<div>Some text<p>More text</p></div>` generate an anonymous block box around "Some text"
    * `<p>Some <em>emphasized</em> text</p>` generate anonymous inline boxes around "some" and "text"
    * when an inline box contains block boxes, split the inline box into anonymous block boxes.([demo](https://jsbin.com/lidupogiyi/edit?html,output)) 
1. Any boxes in normal flow will be part of a *formatting context*, includes
    * Block formatting context(BFC) for block boxes
    * Inline formatting context(IFC) for inline boxes
1. Block boxes are laid out in a *[BFC](https://www.w3.org/TR/CSS22/visuren.html#block-formatting)*(a new and separate block flow layout), which is created by
    * The root element of the document (\<`html`>)
    * Floats (elements where `float` isn't none)
    * [Absolutely positioned elements](https://developer.mozilla.org/en-US/docs/Web/CSS/position#types_of_positioning)
    * Inline-blocks (elements with `display: inline-block;`).
    * Block elements where `overflow` has a value other than `visible` and `clip`.
    * [...](https://developer.mozilla.org/en-US/docs/Web/Guide/CSS/Block_formatting_context)
1. BFCs cannot interfere with other BFCs
    * Vertical margins between adjacent block boxes collapse in the same BFC
    * A floated box only affect other elements in the same BFC
    * Contains floated boxes (Floated box is taken out of the containing box, but still within the BFC)
1. Inline boxes are laid out in an *[IFC](https://www.w3.org/TR/CSS22/visuren.html#inline-formatting)*, which is created by
    * A block container box that contains no block boxes
    * In blink, it was created by adding [anonymous block boxes](https://chromium.googlesource.com/chromium/src/+/refs/heads/main/third_party/blink/renderer/core/layout/ng/inline/README.md)
1. Relative positioning(`position: relative;`)
    * Boxes will be laid out according to the normal flow, and then shifted *relative to itself*(normal position)
    * Reserve its normal position, so the size and position of other elements remain unchanged
    * The vertical offset from its normal position is specified by [top](https://developer.mozilla.org/en-US/docs/Web/CSS/top) and [bottom](https://developer.mozilla.org/en-US/docs/Web/CSS/bottom)
    * The horizontal offset from its normal position is specified by [left](https://developer.mozilla.org/en-US/docs/Web/CSS/left) and [right](https://developer.mozilla.org/en-US/docs/Web/CSS/right)

### Float and Absolute
1. Float(`float: left;`)
    * A floated box is shifted to the left or right until
        * its outer edge touches the containing block edge
        * or the outer edge of another float (can be prohibited by [clear](https://developer.mozilla.org/en-US/docs/Web/CSS/clear))
    * A floated box is not in the normal flow
        * other boxes in flow will be laid out normally as if the floated box did not exist
        * a block box with only floated boxes inside has zero height.([demo](https://jsbin.com/pufucegamo/edit?html,output))
    * A floated box will create a new BFC for inner boxes
    * A floated box only affect other boxes in the same BFC
    * A BFC box will increase its height to contains participated floated boxes.([demo](https://jsbin.com/tirenedice/edit?html,output))
1. Absolute(`position: absolute;` | `position: fixed;`)
    * An absolutely positioned box is explicitly offset with respect to its containing block, which is
      * the *padding edge* of the nearest positioned ancestor (`position` value is anything but `static`)
      * or the initial containing block (\<`html`>)
      * [viewport](https://developer.mozilla.org/en-US/docs/Glossary/Viewport) for fixed positioned box
    * An absolutely positioned box is not in the normal flow
    * An absolutely positioned box create a new BFC for inner boxes

### Flexbox
1. [A Complete Guide to Flexbox](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)

### Layered Presentation
Each box also lie along a "z-axis"
  * each box belongs to one **stacking context**
  * each box in a given stacking context has an integer **stack level** and stacked accordingly
1. A stacking context is formed as following
    * Root element of the document (\<`html`>)
    * Positioned box and z-index value other than `auto`
    * Element with a [opacity](https://developer.mozilla.org/en-US/docs/Web/CSS/opacity) value less than 1
    * [...](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Positioning/Understanding_z_index/The_stacking_context#the_stacking_context)
1. The **stack level** of the box is specified by [z-index](https://developer.mozilla.org/en-US/docs/Web/CSS/z-index) value
    * only applies to positioned boxes(`position` other than `static`)
    * positive values: boxes with a larger z-index cover those with a smaller one
    * default `auto` value: the stack level is `0`
    * negative values: stacked behind the boxes with `auto` value
1. Layers in each stacking context
    * **Root** (current stacking context)
        * the background and borders of the element forming the stacking context.
    * **Negative Stack Level** (most negative first)
        * the child stacking contexts with negative stack levels
    * **Blocks in flow**
        * the in-flow, non-inline-level, non-positioned descendants.
    * **Floats**
        * the non-positioned floats (`float: left`)
    * **Inlines in flow**
        * the in-flow, inline-level, non-positioned descendants
        * inline tables
        * inline blocks
    * **0 Stack Level**
        * the child stacking contexts with stack level 0 (`opacity: 0.1;`)
        * the positioned descendants with stack level 0 (`position: relative;`)
    * **Positive Stack Level** (least positive first) (`position: relative; z-index: 10;`)
        * the child stacking contexts with positive stack levels