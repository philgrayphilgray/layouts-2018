# UX/UI Designs 2018

A personal collection of projects, demos and online course work revisiting UX/UI with the goal of exploring the latest techniques and testing strategies to ensure that designs are:

* Accessible
* Maintainable
* Inclusive
* Beautiful

Web layout is generally concered with how components align and flow in the context of the page and viewport. But components themselves are often containers of other components, aware only of their own parent containers. Because of this fractal nature, it's never a good idea to design layouts in isolation of other UI concerns. A wireframe or mockup first approach is attractive, addressing the needs of the app from a purely aesthetic point of view. However, this leads to box thinking as well as a lot of repeated effort, miscommunication, and mismanagement of expectations between stakeholders.

Design the application, not a screenshot of the application. On the other end of the spectrum, designing a full-scale component or pattern library from the bottom-up without a clear direction for the app as a whole is a good way to do a lot of extra work without getting much closer to the stated goal of creating an app.

Layout is a good starting place for design; it gives us neat slots to fill in. But it needs to be rooted in a few good principles. From the outset, a good design should address the functional needs of both the end-user and the codebase user. It should envision a multifaceted interface that's a joy to work with and a joy to use. Disorganized and inflexible systems are only a joy to discard.

Consideration for a wide range of users and devices, for naming and categorical conventions, codebase maintainability, and aesthetic beauty should all be undertaken with the first round of laying out the application. Ideally, this undertaking produces a working prototype, a real, interactive static application (perhaps with basic routing) like a shell or foundation for further development, not a bait-and-switch for signoff. First round deliverables should be readable, flexible, modular, well-tested, well-documented, and ready to hand off and plug in to other systems for enhanced functionality.

Starting with a pre-written component library like Bootstrap may address some of these concerns. These tools are great for projects where branding will never be a priority. However, when choosing this route, there's a good chance that at some point during the application's lifespan, more time will have been spent customizing the library than if it had been built from scratch. More likely, though, leaning on this tool will lead to bypassing the design process entirely, failing to ask some of the most important existential questions about app at a crucial stage of inception, putting these off to a later date when the inclination is to either scrap it or rebuild from scratch.

## Notes

### Accessibility

* Refer to the [WebAIM checklist](https://webaim.org/standards/wcag/checklist)
* Provide keyboard shortcuts for UI
* Only certain elements are tabbable:

```
'a[href], area[href], input:not([disabled]), select:not([disabled]), textarea:not([disabled]), button:not([disabled]), iframe, object, embed, [tabindex="0"], [contenteditable], audio[controls], video[controls]'
```

* You can use the `tabindex` attribute to make a non-tabbable element tabbable, override default order, or to skip a tabbable element
* Style `:focus` state for visible focus
* Add a skip link at the top of the document to allow users to skip to the main content
* Focus control with `document.activeElement`; you can capture this value when a user enters a modal and then call `.focus()` on it when they close
* Tabtrapping: get a list of all tabable elements inside a modal, listen for the shift and tab keys, and `preventDefault()` and manually set the next tabbed element with `.focus()` when a user reaches the last item going forwards or the first element going backwards.
* Screen Readers: (use `⌘ + F5` in OSX, try ZoomText in OSX, JAWS in Windows, ChromeVox in the Browser)
* Alt text: empty gets skipped, UPPERCASE gets read letter-by-letter
* Use an alt attribute with an empty value to skip images not for screen readers
* Screen readers don't read `display:none`, `visibility:hidden`, `[hidden]`
* For screen readers but not other users, absolute position outside of the viewport or use `background-clip`
* Labels are important for screenreaders; it's fine to visually hide them
* Alternatively, you can set a value to `aria-labelledby` on the form elements themselves
* Use `role` attribute
* Avoid using captchas if possible
* Semantics: always define the language in the html `lang` attribute
* Semantics: use real `button` and `input[type=checkbox]` elements
* Semantics: use appropriate headings; use CSS to override size and style defaults
* Use `aria-describedby` to provide a lengthier description of the UI feature. You can set it to the id of another element with the helper text.
* Use aria properties in CSS attribute selectors, e.g. [aria-expanded="false"]
* Use the `aria-live` attribute on a region of the site that demands attention and is going to be updating.
* Use the `aria-relevant` attribute for specific updates that should be highlighted to the user

#### Tools

* Visual Impairment: NoCoffee Visual Simulator extension
* Analyze color palettes: [Create a Color Palette Accessibility Evaluator
  ](https://accessibility.oit.ncsu.edu/tools/color-contrast/index.php?colors=ffffff,f5f5f5,d5d5d5,e5e5e5,333333,666666,e5f7ff,00628c,00354C,00628,C,008FCC,006e9dmain), [Accessible color palette builder](https://toolness.github.io/accessible-color-matrix/), [Color Safe](http://colorsafe.co/), [Material Color Tool](https://material.io/color/)
* Do not use color as the sole way to communicate or distinguish any content or interface
* Use the accessibility audit in Chrome's built-in [Lighthouse](https://developers.google.com/web/tools/lighthouse/) feature in the console. It can be run from the [command line or as a node module](https://github.com/GoogleChrome/lighthouse). Try to incorporate this tool into the design build process and continuous integration workflow. Also, comes with some other useful audits.
* [Tenon.io](https://tenon.io/) API
* [tota11y](http://khan.github.io/tota11y/), an accessibility visualization toolkit
* [a11y](https://github.com/addyosmani/a11y), command line tool for accessibility audits powered by the Chrome Accessibility Tools
* Testing tools for React: [react-a11y](https://github.com/reactjs/react-a11y); can use it in testing suites:

```
beforeEach(() => a11y(React))
afterEach(() => a11y.restoreAll())
```

* Also, read the [Accessibility section of the react docs](https://reactjs.org/docs/accessibility.html)
* The VueJS community is currently working on a [guide](https://github.com/vuejs/vuejs.org/pull/1002)

Source: [Website Accessibility on FrontendMasters](https://frontendmasters.com/courses/web-accessibility/)

See also: [WebAIM](https://webaim.org/)

### CSS Maintainability

#### SMACSS

SMACSS is a simple approach to structuring modular CSS. It establishes a convention for ordering CSS classes by specificity, dividing them into 5 categories:

1.  **Base Rules**: Element selector rules and resets that apply as always-ok defaults.
2.  **Layout Rules**: Rules specific to the alignment and flow of 'major' components, such as header, main, and footer, distinct from that of 'minor' components.
3.  **Module Rules**: Rules for 'minor' components or the components that are nested in the 'major' components, such as navigation bars, carousels, dialogs, widgets, etc.
4.  **State Rules**: Something that augments and overrides all other styles. A modifier class, usually dynamically applied during user interaction. Example: `--active`, `--open`, `--expanded`, `--closed`.
5.  **Theme Rules**: Colors, (images, gradients, etc) and typography styles that give a site its aesthetic look and feel. Theme styles should be included very last, like a final aesthetic layer or filter.

Source: [SMACSS site](https://smacss.com/)

#### BEM

* A block is a standalone entity, meaningful on its own.
* An element is any part of a block with no standalone meaning, semantically tied to its block.
* A modifier is a flag on a block element to change appearance or behavior.
* Examples of the naming convention: `block--modifier`, `block__element`, `block__element--modifier`
* Hyphenate mutliple-word names
* Any DOM node within a block can be an element; there is no need for nesting
* When adding a modifier class, preserve the original class
* Example:

```html
<div class="menu menu--expanded">
  <ul class="menu__list">
    <li class="menu__item menu__item--active">Edit</li>
    <li class="menu__item">Help</li>
  </ul>
</div>
```

Source: [GetBEM.com](http://getbem.com/introduction/)

* There are a number of variations on BEM. One that I've found helpful, especially in component design, is ABEM (Atomic Block Element Modifier).
* The convention is `[a/m/o]-blockName__elementName -modifierName`
* We often want to create small components without much standalone meaning to reuse is various other components
* Consider, for instance, a button. If we're using or building a Vue or React component library, we might want to create an entire standalone component for a button. We then might use it in both the navigation menu, in a form, and also in a call-to-action. The button will have different semantic meaning in each use-case, but we can reuse the same component. It may have its own wrapper classes, state, and logic. But in each use case, we'll have to consider it an element of another block, and therefore, by BEM's conventions, it should be flattened. But thats not what we want.
* Atomic design addresses this limitation. We can think of components as organisms, made up of molecules, that are made up of atoms. At every level, we can have components. An atomic button can be part of a form field molecule, which is part of a form organism.
* By giving each block name an atomic prefix, we can create a hierarchy of blocks that build bigger blocks.
* Use camelCase for multiple-word names
* Separate the modifier flag from the block name; use only one hyphen, and try to use the modifier only at the block level

* Example:

```html
<div class="o-headerMenu">
<div class="m-searchBar">
<input class="searchBar__input" />
<button class="searchBar__submit">Search</button>
</div>
<div class="m-helpMenu -expanded">
  <ul class="helpMenu__list">
    <li class="helpMenu__item -active">Edit</li>
    <li class="helpMenu__item">Help</li>
  </ul>
</div>
</div>
```

Source: [ABEM. A more useful adaptation of BEM.](https://css-tricks.com/abem-useful-adaptation-bem/)

#### Component-scoped CSS

[React Styled Components](https://www.styled-components.com/)

[react-css-modules](https://github.com/gajus/react-css-modules)

[VUE Scoped CSS](https://vue-loader.vuejs.org/en/features/scoped-css.html)

#### Style Guides

[Website Style Guide Resources](http://styleguides.io/)

### Inclusive

* [Inclusive Web Design Checklist
  ](https://github.com/Heydon/inclusive-design-checklist)

#### Responsive

#### Cross-browser support

#### Performance

[20 Easy Win Performance Tips](https://syntax.fm/show/038/20-easy-win-performance-tips)

* Load JS before closing body tag
* Preload content with Link rel="preload" and rel="prefetch"
* Use picturefill and srcset for responsive images
* Use caching and LocalStorage
* Enable GZIP
* Host media on a CDN
* Compress images
* Use a tool for inlining SVG icons
* Load critical CSS in link tag in the head
* configure `no-unused-imports` in ESlint, purify css, and implement tree shaking
* Implement code splitting
* async and defer script tags
* Use native font stack when possible; use `font-display` property

https://alligator.io/vuejs/using-svg-icons/
https://medium.com/@kossnocorp/solving-the-unsolvable-svg-icons-with-react-preact-webpack-4185032f2601

[Web Performance on Front End Masters](https://frontendmasters.com/courses/web-performance)

* [slides](https://speakerdeck.com/stevekinney/web-performance)

##### Benchmarks

* Jakob Nielsen
* 0.1s feels instantaneous
* "1.0s is about the limit for the user's flow of thought to stay uninterrupted"
* 1s slow down results in 11% fewer pv, 7% less conversion
* 2s delay = >100% bounce rate
* 53% users will leave a mobile website that takes more than 3s to load
* To feel faster, you need to be 20% faster than competitors

* (Google) RAIL: Response, Animation, Idle, Load
* Do less stuff takes less time.
* If you can do it later, do it later

##### Javascript

* Parsing can be slow, as slow as 1mb/s on mobile
* Eager, Lazy parsing
* Optimize-js
* https://nolanlawson.github.io/test-optimize-js/
* After parsing, code is turned into an abstract syntax tree (ast)
* From AST, the interpreter makes bytecode
* Where possible, code is sent through the optimizing compiler
* Optimizing compiler (TurboFan in Chrome V8) does three things that can help: speculative optimization, hidden classes for dynamic lookups, function inlining
* `node --trace-opt <file-name>`: trace optimizations
* `node --trace-opt <file-name> | grep add`
* `trace-deopt`:

#### Internationalization

### Beautiful

#### Content Strategy

#### Vector Graphic Design

#### Data Visualization

#### Layout

#### Typography

* [Professional
  Web Typography](https://prowebtype.com/)
* [Everything I know about Responsive Web Typography](https://zellwk.com/blog/responsive-typography/)
* [The Elements
  of Typographic Style
  Applied to the Web](http://webtypography.net/)
* [Responsive Web Typography v2 on Front End Masters](https://frontendmasters.com/courses/responsive-typography-v2)
* [Flexible typography with CSS locks](https://blog.typekit.com/2016/08/17/flexible-typography-with-css-locks/)


##### Notes from Responsive Web Typography v2 on Front End Masters
* Plan for FOUT with fallback fonts
* Small screens demand subtle scale; do not just scale with screen size
* There's no harm in creating additional breakpoints (tweak points) just for a single element or for type; when we design for a CMS, we need to think more about scalability
* Feel free to embrace serifs; sans-serifs were originally popular in screen design because of low-screen resolution; but that's no longer a factor
* Look at vintage posters and typographers from different regions and eras for inspiration
* Try pairing fonts that were created by the same type designer
* Try to use the actual brand typeface for each project
* Performance matters; load only what you need
* Typeface vs. Font. The typeface is a whole family (like Trade Gothic Next LT Pro); the font is a specific instance of that typeface (like Trade Gothic Next LT Pro Bold)
* Progressive Enhancement: CSS, fonts, and JS are all enhancements. First load it with CSS, and then use a JS loader (like [FONT FACE OBSERVER](https://fontfaceobserver.com/) for async and session storage fix. Linking to CSS or JS is a blocking request. 
* FOUT is a feature, not a bug.
* `font-display: swap;` is a newer property to handle.
* As a convention, use the classes `.wf-active` and `.wf-inactive` on the `html` element.
* Take some time to adjust `font-size`, `letter-spacing`, and `line-height` for the fallback font to avoid jumpiness; you only need to worry about what's above the fold.
* We can be more subtle with headings on small screens because there are fewer distractions.
* Use OpenType kerning and ligatures, turn on with CSS; don't leave orphans behind (use something like Widowtamer.js on the client side)
* Large blocks of centered text are difficult to read; a smooth left or right edge is faster to read; do not center more than 2 lines of text; if you just want to visually center text, using padding-left
* Use a paragraph max-width of `38em`; em is relative to the font-size.
* Don't use pixels; every device is doing something slightly different to report back what the reference pixel value is; some devices go with true resolution when you don't expect it to. Set breakpoints with ems. Ems will alway relate back to 16px. Whatever px value you were going to use, divide by 16.
* Use rems for setting a consistent max-width for headings and body copy.
* Don't forget to use real content.
* Use WOFF2
* Rather than have separate files for a typeface, it's all delivered in one file. It's only using what has deliberately been put there by the type designer. There are a small number of registered axes (like `wght`, `slnt`, `ital`, `opsz`).
* Variant fonts are completely extensible. Some common custom axes are `GRAD`, `YTAS`, `YTDE`. Custom axes need to be in all caps.
* Don't block page render
* Use feature-detection (like adding `.no-js` class to body, or tools like Modernizr)
* `@font-face` support is better than js support; tune for loading process
* When lines are short, you don't need extra line-height
* Fonts are not loaded with `@font-face`; it's okay to list many options here. They get loaded when referenced in CSS with `font-family` declaration.
* The syntax for a variable font `@font-face` declaration is no different
* Set a min and max for font-sizes on the root:

```css

// see Tim Brown's original formula: https://blog.typekit.com/2016/08/17/flexible-typography-with-css-locks/

:root {
  /* vars */
}

// min

h1 {
  font-size: calc( var(--h1-font-size-min) * 1em );
}

// everything in between

@media screen and (min-width: 24.15em) {
  h1 {
    font-size: calc(( var(--h1-font-size-min) * 1em ) + ( var(--h1-font-size-max) - var(--h1-font-size-min)) * ((100vw - (var(--bp-small) * 1em)) / ( var(--bp-xlarge) - var(--bp-small) )));
  }
}


// max

@media screen and (min-width: 75em) {
  h1 {
    font-size: calc( var(--h1-font-size-max) * 1em )
  }
}
```

* Use `font-variation-settings: normal` with `@supports`
* Use optical sizing when available
* Use OpenType features (ligatures & swashes, fractions, kerning). Example: `font-feature-settings: "liga" 1, "kern" 1;`
* See all the OpenType features for a font with [https://wakamaifondue.com/](https://wakamaifondue.com/)
* Other finishing touches (initial caps, better blockquotes, multi-column layouts)
* Create your own typography starter kit
* Use pseudo-selectors like `first-line` and `first-letter` to style initial line and first letter, as in `p:first-of-type:first-line`
* Discover and play with variable fonts: [https://v-fonts.com/](https://v-fonts.com/)




#### Animation

#### Color
