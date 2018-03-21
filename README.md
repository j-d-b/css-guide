# CSS Methodology and Architecture
*A summarized guide for organizing CSS, from class names to file structure*

## Introduction
This is a style guide promoting consistent, maintainable styling in web development.

I incorporated and summarized ideas that most inspired me from [BEM](http://getbem.com/naming/), [Harry Roberts](https://csswizardry.com/), and [other sources](#references).

There is nothing to download; the `sass/` directory contains empty directories and files from the [file structure example](#file-structure) below.

Preliminary notes:
* Only style with classes, *not ids*
* Limit (or totally avoid) use of [CSS combinators](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Selectors#Combinators)
* Aim for maximum abstraction and reusability

## Class names
We'll be using standard BEM class names (*see: http://getbem.com/naming/*)

BEM naming separates styling classes into three parts, **blocks**, **elements**, and **modifiers**. This creates a connection between classes which greatly enhances clarity. No longer must one wonder if two classes are related, or if one depends on another; it's all in the name.

`.block__element--modifier`

### Blocks
* Blocks are a top-level class which hold meaning on their own
* Blocks begin a context in which child **elements** can reside

Examples: `menu`, `modal`

### Elements
* Elements are part of a block that have no meaning outside of the block

Examples: `menu__item`, `modal__header`

### Modifiers
* An additional class signifying a modification to a block or element, such as a different color or size

Examples: `menu--large`, `modal__header--blue`

### BEM naming: A full example
Here's an example navbar, showing BEM class names (*modified from [csswizardry](https://csswizardry.com/2015/03/more-transparent-ui-code-with-namespaces/)*):
```
/* The top-level 'block' of a component, in this example, a navbar */
.navbar {}

/* An 'element' that is part of the larger block */
.navbar__title {}

/* Another 'element' that is part of the navbar */
.navbar__item {}

/* A 'modifier' of a 'block'; a version of the navbar that is blue */
.navbar--blue {}

/* A 'modifier' of an 'element' */
.navbar__item--new {}

/* Another 'block'
.button {}

/* And modifier */
.button--navbar {}
```

Putting it together:
```
<div class="navbar navbar--blue">
  <div class="navbar__title"></div>
  <div class="navbar__item"></div>
  <div class="navbar__item navbar__item--new"></div>
  <div class="navbar__item">
    <div class="button button--navbar"></div>
  </div>
</div>
```

## Adding namespaces
In addition to BEM class naming, we'll be adding namespace prefixes to further clarify the role of a specific class.

Using namespaces enhances readability in HTML and facilitates organization into files.

We'll use seven distinct namespaces, and they're all pretty straightforward:
* `js-` for javascript hooks
* `t-` for typography related styling
* `u-` for specific purpose utilities
* `l-` for global layout classes
* `is-` and `has-` to reflect temporary states
* `o-` for abstract objects
* `c-` for distinct components

### Javascript hooks
* Prefix classes that are used as javascript hooks with  with `js-`
* Do not style these; they should not appear in CSS files

### Utilities
* Prefix utility classes with `u-`
* Utilities have a specific single role, such as centering text
* They are not tied to any individual piece of UI and are often atomic
* Utility classes do not logically have descendent elements or modifiers

Examples: `u-clearfix`, `u-display-none`, `u-text-center`

### Typography
* Prefix **typography** related classes with `t-`
* These include font sizes, families, weights, and styles
* Do not change padding or margins in typography classes
* Typography classes are similar to utilities in that they have are often atomic and have a single, specific effect, while not tied to a context

Examples: `t-lg`, `t-roboto`, `t-italic`

### Layout
* Prefix layout classes with `l-`
* Layout classes do not logically have descendent elements or modifiers
* Layout classes are global only, such as grids and wraps
* Some CSS guides find additional clarify in block-level `l-` prefixes, such as `l-navbar--small`. I find it cleaner to keep all `l-` prefixed layouts global. `navbar--small` works well enough for me

Examples: `l-wrap`, `l-row`, `l-zindex-2`

### State
* Prefix state classes with `is-` or `has-`
* State classes do not logically have descendent elements or modifiers

Examples: `is-hidden`, `has-dropdown`

### Objects
* Prefix **objects** with `-o`
* Objects are the smallest, abstract building blocks of a website
* Objects may be used in any number of unrelated contexts
* Objects should not change any structure outside itself (e.g. `margin`, `position`, `display`)
* Objects cannot contain other objects or components

Examples: `o-button`, `o-media`

### Components
* Prefix **components** with `-c`
* Components are distinct pieces of UI
* Components start a context
* Components can contain other components and objects

Examples: Basically any of your custom elements!

Now let's reflect these additions in our navbar example:

```
<div class="l-row">
  <div class="l-column"
    <div class="c-navbar c-navbar--blue t-bold js-main-nav">
      <div class="c-navbar__title"></div>
      <div class="c-navbar__item"></div>
      <div class="c-navbar__item c-navbar__item--new"></div>
      <div class="c-navbar__item">
        <div class="o-button o-button--navbar"></div>
      </div>
    </div>
  </div>
</div>
```

## File structure
We'll separate our scss partials (or css files) into three directories, which import into a master `style.scss` file to be compiled a minified for distribution:

`base/` contains global resets/normalize, utilities, layouts, and typography

`objects/` contains a partial for each object, with relevant state classes, sub-elements, and modifiers

`components/` contains a partial for each component, with relevant state classes, sub-elements, and modifiers

Feel free to create directories for utilities, layout, and typography as well and further splitting scss files if that makes sense in your project.

Here's an example directory structure for ya:

```
scss/
├─ base/
│   ├─ _normalize.scss
│   ├─ _root.scss
│   ├─ _utilities.scss
│   ├─ _layout.scss
│   └─ _typography.scss
├─ objects/
│   ├─ _button.scss
│   └─ _media.scss
├─ components/
│   ├─ _navbar.scss
│   └─ _modal.scss
└─ style.scss
```

`_root.scss` is the only location in which HTML elements themselves are styled. This is useful for a few resets, or common actions like removing margins from headings or
```
html {
  font-size: 62.5%
}
```
You don't have to use a specific `_root.scss` file, but all styled HTML elements should be in `base/` only.

`style.scss` is composed entirely of imports; no classes declared here.

Check out the source for import order example.

## Additional notes
* I'm considering combining `layout` and `utility` namespaces, depending on the project
* `margin` classes could technically be `l-` prefixed, but padding `u-` prefixed. This is annoying, so I prefix both with `u-`

## References
http://nicolasgallagher.com/about-html-semantics-front-end-architecture/

https://www.lullabot.com/articles/bem-atomic-design-a-css-architecture-worth-loving

https://csswizardry.com/2015/03/more-transparent-ui-code-with-namespaces/

http://www.jamesturneronline.net/blog/bemit-naming-convention.html

http://getbem.com/

https://smacss.com/
