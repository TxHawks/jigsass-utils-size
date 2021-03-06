# JigSass Utils Size
[![NPM version][npm-image]][npm-url]  [![Dependency Status][daviddm-image]][daviddm-url]   

  > A collection of dynamically generated size-related utility classes.

Class names follow the [Emmet](http://docs.emmet.io/cheat-sheet/) abbreviation
syntax, with colons (':') replaced by two dashes (`--`) to follow BEM naming
conventions.

## Available classes

  - `.u-w--<amount>`: width
  - `.u-w--auto`: width: auto
  - `.u-miw--<amount>`: min-width
  - `.u-miw--none`: min-width: none
  - `.u-maw--<amount>`: max-width
  - `.u-maw--none`: max-width: none

  - `.u-h--<amount>`: height
  - `.u-h--auto`: height: auto
  - `.u-mih--<amount>`: min-height
  - `.u-mih--none`: min-height: none
  - `.u-mah--<amount>`: max-height
  - `.u-mah--none`: max-height: none

Where `<amount>` can be either a unitless number representing a number of
typographic rhythm units as defined in
[$jigsass-sizes](https://txhawks.github.io/jigsass-tools-typography/#variable-jigsass-sizes),
a percentage, or a length specified in pixels, ems or rems, auto or none.


## Installation

Using npm:

```sh
npm i -S jigsass-utils-size
```

## Usage
Import JigSass Utils Size into your main scss file near its very end, together with all
other utilities (utilities should always be the last to be imported).

```scss
@import 'path/to/jigsass-utils-size/scss/index';
```

Like all other JigSass Utils, JigSass Size does not automatically generate any CSS
when imported. You would need to explicitly indicate that each individual size
class should actually be generated in each component or object it is used in
(clarification: This will include style declarations inside `.foo` and .`bar`):

```scss
// _c.foo.scss
.foo {
  @include jigsass-util(u-w, $modifier: 360px); // <-- width: 360px

  ...
}
```

```scss
// _c.bar.scss
.bar {
  @include jigsass-util(u-miw, $modifier: 600px);  // <-- min-width: 600px
  @include jigsass-util(
    u-miw, $modifier: 1200px,
    $from: large
  ); // <-- max-width: 1200px from large bp and on.

  ...
}
```

Doing so helps us a great deal with portability, as no matter where we import component or object
partials, the correct utility classes will be generated. Think of it as a poor man's dependency
management.

Developer communication is also assisted by including "dependencies" wherever they are required,
as anyone going through a partial, can easily understand how it should be marked up with just a
glance.

As far as bloat goes, just don't worry about it - the actual styles will only be generated once,
at the location in the cascade where the Jigsass Size partial was imported into the main file.


JigSass Size classes are responsive-enabled, using [JigSass MQ](https://txhawks.github.io/jigsass-tools-mq/)
and the breakpoints defined in the [$jigsass-breakpoints](https://txhawks.github.io/jigsass-tools-mq/#variable-jigsass-breakpoints) variable.

Based on the breakpoint arguments passed to `jigsass-util` when including a Size class,
responsive modifiers are generated according to the following logic:

```scss
.u-w--<modifier>[-[-from-<breakpoint-name>][-until-<breakpoint-name>][-misc-<breakpoint-name>]]
```

So, assuming the `medium`, `large` and `landscape` breakpoints are defined in `$jigsass-breakpoints`
as `600px`, `1024px` and `(orientation: landscape)` respectively,

```scss
@include jigsass-util(u-h, $modifier: 50);
```
will generate the `.u-h--50` class, which is not limited to any media-query.

```scss
@include jigsass-util(u-h, $modifier: 10, $until: medium);
```

will generate the `.u-h--10--until-medium` class, which will be in effect at
`(max-width: 37.49em)` and will override styles in the default class until that point.

```scss
@include jigsass-util(u-mih, $modifier: 300px, $from: large, $misc: landscape);
```

will generate the `.u-mih--200px--from-large-when-landscape` class, which will go into
effect at `(min-width: 64em) and (orientation: landscape)` and will override styles in the default
class under these  conditions.

## Documentation

The full documentation was generated using mdcss, and is available at 
[https://txhawks.github.io/jigsass-utils-size/](https://txhawks.github.io/jigsass-utils-size/)

## Contributing

It is a best practice for JigSass modules to *not* automatically generate css on `@import`, but 
rather have the user explicitly enable the generation of specific styles from the module.

Contributions in the form of pull-requests, issues, bug reports, etc. are welcome.
Please feel free to fork, hack or modify JigSass Size in any way you see fit.

#### Writing documentation

Good documentation is crucial for usability, scalability and maintainability. When 
contributing, please do make sure that both its Sass functionality (functions, mixins, 
variables and placeholder selectors), as well as the CSS it generates (selectors, 
concepts, usage exmples, etc.) are well documented.

Jigsass Size uses Jonathan Neal's [mdcss](//github.com/jonathantneal/mdcss).

When styles and documentation comments are not automatically generated by your module on `@import`,
please use the `sgSrc/sg.scss` file to enable their generation.

In addition, any file in `sgSrc/assets` will be available for use in the style guide.



## File structure
```bash
┬ ./
│
├─┬ scss/ 
│ └─ index.scss # The module's importable file.
│
├─┬ sgSrc/      # Style guide sources
│ │
│ ├── sg.scc    # It is a best practice for JigSass 
│ │             # modules to not automatically generate 
│ │             # css and documentation on `@import.` 
│ │             # Please use this file to enable css
│ │             # and documentation comments) generation.
│ │
│ └── assets/   # Files in `sgSrc/assets` will be 
│               # available for use in the style guide
│
└─┬ docs/      # Documention
  │
  └── styleguide/ # Generated documentation 
                  # of the module's CSS
```




**License:** MIT



[npm-image]: https://badge.fury.io/js/jigsass-utils-size.svg
[npm-url]: https://npmjs.org/package/jigsass-utils-size

[daviddm-image]: https://david-dm.org/TxHawks/jigsass-utils-size.svg?theme=shields.io
[daviddm-url]: https://david-dm.org/TxHawks/jigsass-utils-size
