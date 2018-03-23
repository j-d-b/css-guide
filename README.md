# CSS Methodology and Architecture
*A summarized guide for organizing CSS, from class names to file structure*

*Note, this guide is currently still a draft*

## Introduction
This is a style guide promoting consistent, maintainable styling in web development.

I incorporated and summarized ideas that most inspired me from [BEM](http://getbem.com/naming/), [Harry Roberts](https://csswizardry.com/), and [other sources](#references).

There is nothing to download; the `scss/` directory contains some examples files with empty classes [file structure example](#file-structure) below.

Preliminary notes:
* Only style with classes, *not ids*
* Limit (or totally avoid) use of [CSS combinators](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Selectors#Combinators)
* Aim for maximum abstraction and reusability

## Class names
We'll be using BEM-style class names (*see: http://getbem.com/naming/*), but changing the double underscore `__` to a more relaxed single dash `-`.

Component are divided into three parts, **blocks**, **elements**, and **modifiers**. This creates a connection between classes which greatly enhances clarity. No longer must one wonder if two classes are related, or if one depends on another; it's all in the name.

`.block-element--modifier`

### Blocks
* Blocks are a top-level class which hold meaning on their own
* Blocks begin a context in which child **elements** can reside

Examples: `menu`, `modal`

### Elements
* Elements are a subcomponent of a block that have no meaning outside of the block

Examples: `menu-item`, `modal-header`

### Modifiers
* An suffix to a block or element signifying a modification to the base class, such as a different color or size
* Can be thought of as a `flag`

Examples: `menu--large`, `menu-header--blue`

### Class naming: A full example
Here's an example navbar, showing BEM class names (*modified from [csswizardry](https://csswizardry.com/2015/03/more-transparent-ui-code-with-namespaces/)*):
```
/* The top-level 'block' of a component, in this example, a navbar */
.navbar {}

/* An 'element' that is part of the larger block */
.navbar-title {}

/* Another 'element' that is part of the navbar */
.navbar-item {}

/* A 'modifier' of a 'block'; a version of the navbar that is blue */
.navbar--blue {}

/* A 'modifier' of an 'element' */
.navbar-item--new {}
```

Putting it together:
```
<div class="navbar navbar--blue">
  <div class="navbar-title"></div>
  <div class="navbar-item"></div>
  <div class="navbar-item navbar-item--new"></div>
</div>
```

## Adding namespaces
In addition to naming, we'll be adding namespace prefixes to further clarify the role of a specific class.

Using namespaces enhances readability in HTML and facilitates organization into files. All classes will have a namespace prefix, except distinct components. A class without a namespace is presumed to be a distinct component.

We'll use six distinct namespaces, and they're all pretty straightforward:
* `l-` for global layout utilities
* `u-` for specific purpose utilities
* `t-` for typography related styling
* `is-` and `has-` to reflect temporary states
* `js-` for javascript hooks
* `a-` for abstract objects
* no prefix for distinct components

### Layout
* Prefix layout classes with `l-`
* Layout classes are the organizational building blocks of the page
* Layout classes do not logically have descendent elements or modifiers

Examples: `l-grid`, `l-row`

### Utilities
* Prefix utility classes with `u-`
* Utilities have a specific role, such as centering text or adding padding
* They are not tied to any individual piece of UI and are often atomic
* Utility classes do not logically have descendent elements or modifiers

Examples: `u-zindex-2`, `u-display-none`, `u-text-center`

### Typography
* Prefix **typography** related classes with `t-`
* These include font sizes, families, weights, and styles
* Do not change padding or margins in typography classes
* Typography classes are similar to utilities in that they have are often atomic and have a single, specific effect, while not tied to a context

Examples: `t-lg`, `t-roboto`, `t-italic`

### State
* Prefix state classes with `is-` or `has-`
* State classes do not logically have descendent elements or modifiers

Examples: `is-hidden`, `has-dropdown`

### Javascript hooks
* Prefix classes that are used as javascript hooks with  with `js-`
* Do not style these; they should not appear in CSS files

### Abstracts
* Prefix abstracts with `a-`
* Abstracts are general and highly reusable
* Abstracts may be used in any number of unrelated contexts
* Abstracts should not change any structure outside itself (e.g. `margin`, `position`, `display`)
* Abstracts should not contain components, only other abstracts and sub-elements
* Abstracts are a subset of components that aren't tied to a specific piece of UI 

Examples: `a-button`, `a-media`, `a-list`, `a-card`, `a-menu`

### Components
* Components are distinct, specifically-purposed part of the UI
* Components need no prefix
* Components start a context
* Components can contain other components and objects
* Components are basically any of your custom elements!

Examples: `chatbox`, `sidebar`, `staff-list`

Now let's reflect these additions in our navbar example, and add a couple more elements:
```
<div class="l-row u-width-50">
  <div class="navbar navbar--blue t-bold js-main-nav">
    <div class="navbar-title"></div>
    <div class="navbar-item"></div>
    <div class="navbar-item navbar-item--new"></div>
    <div class="navbar-item">
      <div class="a-button a-button--navbar"></div>
    </div>
  </div>
</div>
```

## File structure
We'll separate our scss partials (or css files) into five directories, which import into a master `style.scss` file to be compiled for distribution:

`base/` contains resets/normalize, and HTML element modifying code

`layout/` contains a partial for high-level, global layout classes

`utilities/` contains a partial for types of non-layout utilities, including typography

`abstracts/` contains a partial for each abstract, with relevant state classes, sub-elements, and modifiers

`components/` contains a partial for all components, with relevant state classes, sub-elements, and modifiers

Here's an example directory structure:
```
scss/
├─ abstracts/
│   ├─ _button.scss
│   └─ _media.scss
├─ base/
│   ├─ _normalize.scss
│   ├─ _root.scss
├─ components/
│   ├─ _sidebar.scss
│   └─ _footer.scss
├─ layout/
│   ├─ _grid.scss
│   └─ _container.scss
├─ utilities/
│   ├─ _spacing.scss
│   └─ _typography.scss
└─ style.scss
```

Here `_root.scss` is the only location in which HTML elements themselves are styled. You don't have to use a specific `_root.scss` file, but all styled HTML elements should be in `base/` only.

This is useful for a few resets, or common actions like removing margins from headings or something like
```
html {
  font-size: 62.5%
}
```

`style.scss` is composed entirely of imports in the following order:

`base/`, `layout/`, `utilities/`, `abstracts/`, `components/`

## References
http://nicolasgallagher.com/about-html-semantics-front-end-architecture/

https://www.lullabot.com/articles/bem-atomic-design-a-css-architecture-worth-loving

https://csswizardry.com/2015/03/more-transparent-ui-code-with-namespaces/

http://www.jamesturneronline.net/blog/bemit-naming-convention.html

http://getbem.com/
