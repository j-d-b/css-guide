# Using My CSS Methodology and Architecture with [Bootstrap](https://github.com/twbs/bootstrap)

## Introduction
Here I propose modifications to the style guide if aiming for maximum leverage of [Bootstrap](https://github.com/twbs/bootstrap) classes for styling and layout.

**Goal:** Use Bootstrap classes as much as possible

## Major changes:
* Abstract and distinct components combined into just "components", and prefixed with a `c-`, to distinguish themselves from Bootstrap classes
* Since we'll be relying on the Bootstrap grid, the `layouts/` directory has been removed and any custom layout classes (which are now often unneeded) added to `utilities/`

## Class names
We'll still be using the `.block-element--modifier` convention to name classes.

Bootstrap follows a similar convention, but uses only a single dash for modifiers as well.

## Namespaces
All classes will have a namespace prefix. Thus, classes without a prefix are assumed to be Bootstrap classes.

* `l-` for global layout utilities
* `u-` for specific purpose utilities
* `t-` for typography related styling
* `is-` and `has-` to reflect temporary states
* `js-` for javascript hooks
* `c-` for all components

### Components
* Prefix all components with `c-`
* Components can contain other components and objects
* Components start a context for sub-elements

Examples: `c-media`, `c-footer`, basically any of your custom elements!

Now let's summarize in an example. Classes without a prefix are from Bootstrap.
```
<div class="row">
  <div class="col-md-8">
    <div class="card border-dark t-bold js-main-nav">
      <div class="card-header"></div>
      <div class="card-body u-center t-sm"></div>
      <div class="card-footer">
        <div class="button button-primary js-close"></div>
      </div>
    </div>
  </div>
</div>
```

## File structure
We'll now separate our scss partials (or css files) into three directories, which import into a master `style.scss` file to be compiled for distribution:

`base/` contains resets/normalize, and HTML element modifying code

`utilities/` contains a partial for types of utilities, including typography and custom layouts

`components/` contains a partial for each custom component, with relevant state classes, sub-elements, and modifiers

Here's an example directory structure:
```
scss/
├─ base/
│   ├─ _normalize.scss
│   ├─ _root.scss
├─ components/
│   ├─ _menu.scss
│   ├─ _sidebar.scss
│   └─ _footer.scss
├─ utilities/
│   ├─ _spacing.scss
│   └─ _typography.scss
└─ style.scss
```

`style.scss` is composed entirely of imports in the following order:

`base/`, `utilities/`, `components/`
