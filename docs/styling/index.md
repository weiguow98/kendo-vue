---
title: Overview
page_title: Styling & Themes - Overview - Kendo UI for Vue Docs & Demos
description: "Use themes to style the appearance of your project with Kendo UI for Vue."
slug: themesandstyles
position: 1
---

# Styling Overview

Kendo UI for Vue provides themes that you can used to style your application.

Currently, Kendo UI for Vue ships the following themes:

* [Default theme](slug:preview_kendothemedefault)&mdash;Available through the `@progress/kendo-theme-default` NPM module.
* [Bootstrap theme](slug:preview_kendothemebootstrap)&mdash;Available through the `@progress/kendo-theme-bootstrap` NPM module.
* [Material theme](slug:preview_kendothemematerial)&mdash;Available through the `@progress/kendo-theme-material` NPM module.
* [Fluent theme](slug:preview_kendothemefluent)&mdash;Available through the `@progress/kendo-theme-fluent` NPM module.

## Installation

1. To start using a theme, install its package through NPM. For example, to install the Default theme, run the following command:

    ```sh
    npm install --save @progress/kendo-theme-default
    ```

1. After the theme package is installed, reference it in your project.

## Including Themes in Your Project

Each theme package provides the two ways for including the theme in your project:

* [By using a precompiled CSS file](#toc-using-precompiled-css)&mdash;This approach does not require a build step and is therefore the faster of the two.
* [By compiling the theme from SCSS source files](#toc-customizing-themes)&mdash;This approach reduces the CSS output and allows you to customize the theme.

## Using Precompiled CSS

Each theme includes a precompiled `dist/all.css` CSS file that contains the styles for all Kendo UI for Vue components. To use `dist/all.css`, reference it in the `App.vue` file of the application.

```js
import '@progress/kendo-theme-default/dist/all.css';

```

While using the precompiled CSS file is faster than compiling the theme from the source code, the approach has the following drawbacks:

* It includes CSS for components that are not used in the application.
* It does not provide options for theme customization through SCSS variables (which is possible when you build the theme from the source code) because the theme is already compiled.

## Customizing Themes

Each Kendo UI for Vue theme package includes the source files of the theme which provides options for you to modify and rebuild the theme as part of your build process. For example, you can change the theme colors, remove the CSS of unused components, or use specific theme colors to style your application. The theme source files are located in the `scss` folder of the theme package.

For a list of variables that can be modified in a theme, refer to the article on each theme customization:
* [Customizing the Default theme](slug:variables_kendothemedefault)
* [Customizing the Bootstrap theme](slug:variables_kendothemebootstrap)
* [Customizing the Material theme](slug:variables_kendothemematerial)

To build a custom theme by using the theme variables, apply either of the following approaches:
* (Recommended) Use the build process of your application. This approach simplifies upgrades to new component and theme package versions.
* [Use the build process of the themes](#toc-using-the-build-process-of-the-themes)&mdash;This approach requires you to build the theme each time the theme packages are updated.

For a visual preview of the theme for all components and versatile color swatches, use the [ThemeBuilder application](#toc-using-themebuilder) which provides an interface for theme customization.

### Using the Build Process of the Application

* To use Node Sass (which uses [LibSass](https://sass-lang.com/libsass)), run the `npm install node-sass --save` command.
* To use [Dart Sass](https://sass-lang.com/dart-sass), run the `npm install node-sass@npm:sass --save` command.

```scss
// App.scss
@import "~@progress/kendo-theme-default/dist/all.scss";
```

With this setup, you can customize theme variables directly in your application. For example, you can change the default primary color from orange to pink with the following lines:

```scss
$primary: #ff69b4;

@import "~@progress/kendo-theme-default/dist/all.scss";
```

The `dist/all` file adds the styles for all components that are available in the theme. To trim down the size of the generated CSS, import only the source for the components that you use in your application. Each of them could be found in `scss/` folder.

```scss
// Import only the PanelBar and Grid styles using Node Sass
@import "~@progress/kendo-theme-default/scss/grid/_index.scss";

// or using Dart Sass
@import "~@progress/kendo-theme-default/scss/grid/";
```

### Using the Build Process of the Themes

While each Kendo UI for Vue theme has a dedicated NPM package (for example, @progress/kendo-theme-default), the source code for all themes is located in the [kendo-themes](https://github.com/telerik/kendo-themes) repository which contains a build task that compiles the theme sources to CSS. To customize a theme, modify the source code of the theme and use the build task to produce a CSS file for your application. This approach avoids the need for a setting up a build configuration when you compile SCSS, but may be harder to maintain as the process has to be repeated each time a theme is updated.

> To improve the development process, the previous independent GitHub repositories of each theme were merged in the single [kendo-themes](https://github.com/telerik/kendo-themes) repository and the individual repositories were archived.

#### Customizing Themes with Swatches

A swatch is a set of variables which customizes the appearance of the theme.

* Each swatch is placed in a separate file. A theme may contain multiple swatches.
* Swatches are useful for creating multiple, persistent theme variations.
* The `.css` output file can be shared across projects and requires no further processing.

To create a swatch:

1. Clone the [kendo-themes](https://github.com/telerik/kendo-themes) GitHub repository.
1. Install the [node-gyp](https://github.com/nodejs/node-gyp#installation).
1. Install the dependencies for all themes with `npm install && npx lerna bootstrap`.
1. Switch the working directory to `packages/<THEME_NAME>`.
1. Create a `SWATCH_NAME.scss` swatch file in the `scss/swatches` folder.
1. To build the swatches for the theme by running `npm run sass:swatches` or `npm run dart:swatches`.
1. Include the compiled CSS swatch file in your project. It could be found under `dist/SWATCH_NAME.css`.

For example, in the Material theme create `blue-pink-dark` swatch with the following lines:
```scss
// Variables.
$primary-palette-name: blue;
$secondary-palette-name: pink;
$theme-type: dark;

// Import the theme file for the components you use.
@import "../panelbar/_index.scss";
@import "../grid/_index.scss";

// Alternatively, include all components.
@import "../all.scss";
```

For the rest of the themes, the swatch should look like:
```scss
// Variables.
$primary: blue;
$secondary: pink;

// Import the theme file for the components you use.
@import "../grid/_index.scss";

// Alternatively, include all components.
@import "../all.scss";
```

#### Customizing the Source Code

To create a custom theme by modifying the themes source code:

1. Clone the [kendo-themes](https://github.com/telerik/kendo-themes) GitHub repository.
1. Install the dependencies for all themes with `npm install && npx lerna bootstrap`.
1. Customize the theme variables in the `packages/THEME_NAME/scss/_variables.scss` files.
1. Build the themes with the `npm run sass` or `npm run dart` command to create the customized version of the themes in the `packages/THEME_NAME/dist/all.css` file.
1. After the build completes, use the [compiled CSS](#toc-using-precompiled-css).

#### Creating Custom Components Bundle

You might want to omit the styles for some components in the CSS output. To include only the styles that you need:

1. Clone the [kendo-themes](https://github.com/telerik/kendo-themes) GitHub repository.
1. Install the dependencies for all themes with `npm install && npx lerna bootstrap`.
1. Switch the working directory to `packages/<THEME_NAME>`.
1. Create a `CUSTOM_THEME.scss` file in the `scss` folder. For example, create `custom.scss` file with the following lines:
    ```scss
    // Import the theme file for the components you use.
    @import "../grid/_index.scss";
    ```

1. To build the file, navigate to the theme folder and run `gulp sass --file "scss/CUSTOM_THEME.scss"`.
1. Include the compiled CSS file in your project. It could be found under `dist/CUSTOM_THEME.css`.

### Using ThemeBuilder

To take full control over the appearance of the Kendo UI for Vue components, you can create your own styles by using [ThemeBuilder](slug:themebuilder).

ThemeBuilder is a web application that enables you to create new themes and customize existing ones. Every change that you make is visualized almost instantly. Once you are done styling the Vue components, you can export a zip file with the styles for your theme and use them in your Vue app.

## Suggested Links

* [Web Font Icons in Kendo UI for Vue](slug:icons)
* [Getting Started with Kendo UI for Vue - JavaScript (Online Guide)](slug:getting_started_javascript_composition_api)
* [Getting Started with Kendo UI for Vue - TypeScript (Online Guide)](slug:getting_started_typescript_composition_api)
* [Getting Started with Kendo UI for Vue - JavaScript + Options API (Online Guide)](slug:getting_started_javascript_options_api)
* [Getting Started with Kendo UI for Vue - TypeScript + Options API (Online Guide)](slug:getting_started_typescript_options_api)
* [Getting Started with Kendo UI for Vue - Nuxt 3 (Online Guide)](slug:getting_started_nuxt_3)
* [Browse the Components](https://www.telerik.com/kendo-vue-ui/components)
