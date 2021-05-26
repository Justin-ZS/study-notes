# Browser and Render
Dive into browser architecture and rendering flow.

## Resources
1. [Chromium architecture overview](http://szeged.github.io/sprocket/architecture_overview.html)
1. [How Blink works](https://docs.google.com/document/d/1aitSOucL0VHZa9Z2vbRJSyAIsAz24kX8LFByQ5xQnUg/edit#heading=h.v5plba74lfde)
1. Life of a pixel([Video](https://www.youtube.com/watch?v=K2QHdgAKP-s)|[PPT](https://docs.google.com/presentation/d/1boPxbgNrTU0ddsc144rcXayGA_WF53k96imRH8Mp34Y/edit#slide=id.p))

## Chromium Architecture
### Chromium is a multi-process browser
Chromium has one browser process and N sandboxed renderer processes
1. Browser process
    * main process, startup process, broker process
    * Main thread
        * render browser UI
    * I/O thread
        * communicate with other process
    * N Worker threads
        * run tasks, like DNS lookup
        * there exists a worker pool to add/remove threads dynamically
1. Sandboxed renderer process
    * Usually one process per tab 
        * because of the performance, no 1:1 mapping between renderer processes, iframes and tabs
    * Main thread (runs Blink)
        * render web content
        * JavaScript, CSS, DOM
    * I/O thread
        * communicate with browser process
        * from the view of renderer process, the browser process is a set of services
    * N worker threads
        * [Web Workers](https://html.spec.whatwg.org/multipage/workers.html#workers)
        * [Service Workers](https://w3c.github.io/ServiceWorker/)
        * [Workerlets](https://www.w3.org/TR/worklets-1/)
    * Internal threads
        * web audio
        * database
        * GC
### Concepts
1. A Page corresponds to a concept of a tab
    * Renderer process : Page = 1 : N
1. A Frame corresponds to a concept of a frame (the main frame or an iframe)
    * Page : Frame = 1 : M
1. A DOMWindow corresponds to a `window` object in JavaScript
    * Frame : DOMWindow = 1 : 1
1. A Document corresponds to a `window.document` object in JavaScript
    * DOMWindow : Document = 1 : 1
1. An ExecutionContext is a concept that abstracts a Document (the main thread) and a WorkerGlobalScope (a worker thread)
1. e.g., `iframe.contentWindow.location.href = "https://example.com";`
    * original Page
    * original Frame
    * new DOMWindow
    * new Document
    * new ExecutionContext

## Render Flow
From an HTML file to pixels in the screen
### Pipeline
1. HTML
    ```html
    <div>
        <p> hello </p>
        <p> world </p>
    </div>
    ```
1. Parse HTML: `HTML -> DOM`
    * [HTMLDocumentParser](https://source.chromium.org/chromium/chromium/src/+/main:third_party/blink/renderer/core/html/parser/html_document_parser.h;l=83?q=HTMLDocumentParser&sq=&ss=chromium)
    * [HTMLTreeBuilder](https://source.chromium.org/chromium/chromium/src/+/main:third_party/blink/renderer/core/html/parser/html_tree_builder.h;l=49?q=HTMLTreeBuilder&ss=chromium)
1. DOM (document object model)
    ```
    - Document
      - HTML
        - BODY
          - DIV
            - P
              - Text
            - P
              - Text
    ```
    * contains [shadow DOM](https://developer.mozilla.org/en-US/docs/Web/Web_Components/Using_shadow_DOM)
1. Style
    ```css
    /* rules */
      p { /* selector */
        color: red; /* declaration: property + value */
      }
    ```
1. Parse Style: `Style -> StyleSheetContents`
    * [CSSParser](https://source.chromium.org/chromium/chromium/src/+/main:third_party/blink/renderer/core/css/parser/css_parser.h;l=32;drc=0b4c01de4879deb0e3b288e3732641d1d9e343c9;bpv=1;bpt=1?q=CSSParser&ss=chromium%2Fchromium%2Fsrc)
1. StyleSheetContents
    ```ts
    type CSSProperty = Color | ...;
    type CSSValue = CSSColorValue | ...;

    type CSSPropertyValue = {
      Property: CSSProperty;
      Value: CSSValue;
    }
    type StyleRule = {
      CSSSelectorList: CSSSelector[];
      CSSPropertyValueSet: CSSPropertyValue[];
    };
    type StyleSheetContents = StyleRule[];
    ```
1. [Style Calculation](https://chromium.googlesource.com/chromium/src/+/HEAD/third_party/blink/renderer/core/css/style-calculation.md): `StyleSheetContents + DOM -> ComputedStyle`
    * **Style recalc**: for every node in the DOM tree, compute the value of every style property
    * [Document::UpdateStyle](https://source.chromium.org/chromium/chromium/src/+/main:third_party/blink/renderer/core/dom/document.cc;l=2109;drc=0b4c01de4879deb0e3b288e3732641d1d9e343c9;bpv=1;bpt=1)
    * [StyleResolver::ResolveStyle](https://source.chromium.org/chromium/chromium/src/+/main:third_party/blink/renderer/core/css/resolver/style_resolver.cc;l=740?q=symbol:%5Eblink::StyleResolver::ResolveStyle$&sq=&ss=chromium)
    * JS: `getComputedStyle(element)["propertyName"]`
1. DOM with ComputedStyle (CSSOM?)
    ```
    - Document          + ComputedStyle
      - HTML            + ComputedStyle
        - BODY          + ComputedStyle
          - DIV         + ComputedStyle
            - P         + ComputedStyle
              - Text    + ComputedStyle
            - P         + ComputedStyle
              - Text    + ComputedStyle
    ```
1. [Layout](https://chromium.googlesource.com/chromium/src/+/refs/heads/main/third_party/blink/renderer/core/layout/README.md): `DOMTree -> LayoutTree -> FragmentTree`
    * Computes the exact position and size of each object
      * Block box and inline box
      * Normal flow. floats and absolute
      * Text font, size, family
      * Overflow and scrollable
    * [StyleEngine::RebuildLayoutTree](https://source.chromium.org/chromium/chromium/src/+/main:third_party/blink/renderer/core/css/style_engine.cc;l=2064;drc=0b4c01de4879deb0e3b288e3732641d1d9e343c9?q=StyleEngine::RebuildLayoutTree&sq=&ss=chromium%2Fchromium%2Fsrc)
    * [LocalFrameView::UpdateLayout](https://source.chromium.org/chromium/chromium/src/+/main:third_party/blink/renderer/core/frame/local_frame_view.cc;l=847;drc=0b4c01de4879deb0e3b288e3732641d1d9e343c9?q=LocalFrameView::UpdateLayout&ss=chromium%2Fchromium%2Fsrc)
1. Layout Tree: ready to layout
    ```
    - Document       --> - LayoutView      
      - HTML         -->   - LayoutBlockFlow        
        - BODY       -->     - LayoutBlockFlow      
          - DIV      -->       - LayoutBlockFlow     
            - P      -->         - LayoutBlockFlow     
              - Text -->           - LayoutText
            - P      -->         - LayoutBlockFlow     
              - Text -->           - LayoutText
    ```
    * DOM nodes are not 1:1 with layout objects.
        * usually, one DOM node gets one LayoutObject
        * a node -> no LayoutObject: `display: none;`
        * a node -> more than one LayoutObject: inline box with text before and after a block box([demo](https://jsbin.com/lidupogiyi/edit?html,output))
        * no node -> a LayoutObject: [Anonymous block boxes](https://www.w3.org/TR/CSS22/visuren.html#anonymous-block-level) and [Anonymous inline boxes](https://www.w3.org/TR/CSS22/visuren.html#anonymous)
1. FragmentTree: layout result
    * Walks the layout tree, computing the visual geometry of each LayoutObject
    ```
    Box (block-flow) at 0,0 100x12
      Box (block-flow children-inline) at 0,0 100x54
        LineBox at 24.9,0 0x18
          Box (floating block-flow children-inline) at 0,0 24.9x34
            LineBox at 8,8 8.9x18
              Text 'F' at 8,8 8.9x17
          Text '\n' at 24.9,0 0x17
        LineBox at 24.9,18 67.1x18
          Text 'The ' at 24.9,18 28.9x17
          Text 'quick' at 53.8,18 38.25x17
        LineBox at 0,36 69.5x18
          Text 'brown' at 0,36 44.2x17
          Text ' fox' at 44.2,36 25.3x17
      Box (block-flow children-inline) at 80,-6 20x18
        LineBox at 0,0 39.125x18
          Text 'jumps' at 0,0 39.125x17
    ```
1. Paint: `FragmentTree -> PaintOperation[]`
    * Paint records paint operations into a list of display items.
    * Paint elements in **stacking order** (controlled by `z-index`)
    * Paint runs in multiple phases, each paint phase is a separate traversal of a **stacking context**.
    * LocalFrameView::PaintTree
1. Raster(*Compositor thread*): `PaintOperation[] -> ColorBitmap`
    * Raster can be accelerated by the GPU process
1. Draw(*Compositor thread*)
    * Display content with GPU process

### Changes
1. Each pipeline stage tracks granular asynchronous invalidations.
    * Style -> `Node::SetNeedsStyleRecalc()`
    * Layout -> `LayoutObject::SetNeedsLayout()`
    * Paint -> `PaintInvalidator::InvalidatePaint()`
    * Raster -> `RasterInvalidator::Generate()`
1. Layers
    * New **stacking context** will promote a layout object to a layer
    * Main thread will decompose the page into layers before paint
    * Compositor thread will raster layers independently and composite them into one
    * The compositor can handle input events like scroll, clip, scale, ...
1. Compositing Assignments
    * Build layers before paint in the main thread
    * [PaintLayerCompositor::UpdateAssignmentsIfNeeded](https://source.chromium.org/chromium/chromium/src/+/main:third_party/blink/renderer/core/paint/compositing/paint_layer_compositor.cc;l=301;drc=0b4c01de4879deb0e3b288e3732641d1d9e343c9?q=PaintLayerCompositor::UpdateAssignmentsIfNeeded&sq=&ss=chromium%2Fchromium%2Fsrc)
1. Pre-paint
    * Build the property trees before paint in the main thread
    * [PrePaintTreeWalk::Walk](https://source.chromium.org/chromium/chromium/src/+/main:third_party/blink/renderer/core/paint/pre_paint_tree_walk.cc;l=234;drc=0b4c01de4879deb0e3b288e3732641d1d9e343c9?q=PrePaintTreeWalk::Walk&ss=chromium%2Fchromium%2Fsrc)
    * [PaintPropertyTreeBuilder](https://source.chromium.org/chromium/chromium/src/+/main:third_party/blink/renderer/core/paint/paint_property_tree_builder.h;l=275;drc=0b4c01de4879deb0e3b288e3732641d1d9e343c9?q=PaintPropertyTreeBuilder&ss=chromium%2Fchromium%2Fsrc)
    * The property trees contain various properties for a layer
        * overflow
        * clip
        * transform
        * ...
    * The compositor can apply them to a layer

## [Rendering Optimization](https://developers.google.com/web/fundamentals/performance/rendering?hl=en)
1. Recap the render pipeline: `Style -> Layout -> Paint -> Composite`
    * Main thread: `Style`, `Layout`, `Paint`
    * Compositor thread: `Composite`
1. Whole pipeline: some properties will reflow the page
    * Geometry: `width`, `height`, `top`, `left`, `padding`, `margin`, ...
    * Position: `float`, `display`, `position`, `flex`, ...
    * BFC Changing: `overflow`
1. Skip `Layout`: some properties are "paint only"
    * Visual: `background`, `color`
    * Stacking Context Changing: `z-index`, `opacity`
1. Only `Composite`: best performance!
    * Layer properties: `transform`, `opacity`
    * Promote elements with `will-change` or `translateZ`
    * Avoid layer explosions
1. Resources
    * [CSS Triggers](https://csstriggers.com/): A CSS property will trigger which render flow