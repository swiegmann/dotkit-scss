# dotkit-scss-2

[![License](https://img.shields.io/badge/licence-MIT-brightgreen.svg?style=flat)](https://mit-license.org/)

- Version: 2.02
- Date: September 22, 2021
- Github Repository: https://github.com/swiegmann/dotkit-scss/
- [MIT License](LICENSE)



## Overview

dotKit-scss-2 is a lightweight `SASS/SCSS`-System (not even a framework due to its size).
It comes with a bare minimum of mixins/functions.

Key features:

- Setup and address "***devices***" (marked by a class in the HTML-Element). Each device can have own breakpoints
- Setup and address "***themes***" (marked by a class in the Body-Element). Each theme can have own breakpoints
- A comfortable way to organize, generate, set & retrieve ***custom properties*** (aka "CSS Variables") and other variables for any device/theme/selector
- It ***integrates*** in existing code/other frameworks (dotKit uses its own namespace)





## How to use it

Import it from your own files with: `@import "[path-to-your-dotkit-scss-2-folder]/_dotkit.scss";`



## Getting Started

- [API](API.md)
- [Example SCSS File](/example/_example.scss)




## Examples:

**Set & get a variable:**

```SCSS
// set
$dk: dk-var(color, (
  background: #fff
));
// $dk: dk-var(color/background, #fff); // same as above in node-style

// get
div {
  background-color: dk-var(color/background);
}
```

*Setting variables can be done in "node-style" (single) or maps (multiple). Because of a reserved wordlist in SASS, it may be neccessary to quote variable names/nodes.*



**Devices-Setup**:

```SCSS
$dk: dk-var(device, ( // "device" is a reserved node for all devices
  "default": ( // name #1
    XS: dk-rem(0px, em), // breakpoint: from 0px - 960px (= next entry)
    MD: dk-rem(960px, em) // breakpoint: from 960px onward
  ),
  desktop: ( // name #2
    XS: dk-rem(0px, em),
    MD: dk-rem(960px, em)
  )
));
```

*From now on you can address devices and their breakpoints in custom-properties/variables/selectors.*



**Address a device:**

```SCSS
div {
  @include dk-device(desktop) {
    ...
  }
}

div {
  @include dk-device(desktop, MD) { // only from breakpoint MD (onward)
    ...
  }
}
```

*All arguments of `SASS` [mq()](https://github.com/sass-mq/sass-mq) are accepted after the 1st argument (device-name).
Although using `@include dk-device()` might be handy, there is probably no need for it when using custom-properties (see next example).*



**Add custom-properties to devices/breakpoints & apply them all to the HTML-Element:**

```SCSS
// set
$dk: dk-var(custom-properties-device, (
  "font": (
    "size": (
      "default": (
        XS: clamp(1rem, calc(0.75rem + 1vw), 100rem),
        MD: clamp(1rem, calc(0.25rem + 1vw), 100rem)
      )
    )
  ),

  "padding": (
    h: (XS: dk-rem(20), MD: dk-rem(60)),
    v: (XS: dk-rem(10), MD: dk-rem(30))
  )
));

// apply/generate
html {
  @include dk-custom-properties(dk-var(device), dk-var(custom-properties-device));
}

/*
Generated output:
html {
  --font-size: clamp(1rem, calc(0.75rem + 1vw), 100rem);
  --padding-h: 1.25rem;
  --padding-v: 0.625rem; }
  @media (min-width: 60em) {
    html {
      --font-size: clamp(1rem, calc(0.25rem + 1vw), 100rem); } }
  @media (min-width: 60em) {
    html {
      --padding-h: 3.75rem; } }
  @media (min-width: 60em) {
    html {
      --padding-v: 1.875rem; } }
*/
```



**Theme-Setup:**

Themes are basically another layer of Devices.
The difference is that a device-class will be applied to the HTML-Element, whereas a theme-class gets applied to the body-element.
Therefore you can overwrite any device-definition through a theme.
Best practice for themes would be the use of color-definitions.

```scss
$dk: dk-var(theme, ( // "theme" is a reserved node for all themes
  "default": ( // name #1
    XS: dk-rem(0px, em), // breakpoint: from 0px - 960px (= next entry)
    MD: dk-rem(960px, em) // breakpoint: from 960px onward
  ),
  darkmode: ( // name #2
    XS: dk-rem(0px, em),
    MD: dk-rem(960px, em)
  )
));
```

*From now on you can address themes and their breakpoints in custom-properties/variables/selectors.*



**Address a theme:**

```scss
div {
  @include dk-theme(darkmode) {
    ...
  }
}

div {
  @include dk-theme(darkmode, MD) { // only from breakpoint MD (onward)
    ...
  }
}
```

*All arguments of `SASS` [mq()](https://github.com/sass-mq/sass-mq) are accepted after the 1st argument (theme-name).
Although using `@include dk-theme()` might be handy, there is probably no need for it when using custom-properties (see next example).*



**Add custom-properties to themes/breakpoints & apply them all to the body-Element:**

```SCSS
// set
$dk: dk-var(custom-properties-theme, (
  color: (
    background: (
      default: #fff,
      darkmode: #000
    )
  )
));

// apply/generate
body {
  @include dk-custom-properties(dk-var(theme), dk-var(custom-properties-theme));
}

/*
Generated output:
body {
  --color-background: #fff;
  body.darkmode {
    --color-background: #000; }
*/
```



### Contributing

I am by far no SASS-Guru and don't have the skills to write the shortest/most logical possible code.
Some code here is the result of trying, testing, fixing and beeing happy to finally make it work.
I wrote this, because I wanted to write less SASS code, have more flexibility and still have things sorted.

*I am very thankful for corrections, help and additions of any type, even documentation.* :v:
