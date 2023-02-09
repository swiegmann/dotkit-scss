# API



## Functions



### dk-convert-unit($value, $unit)

Convert a value to another unit

```SCSS
dk-convert-unit(1rem, em)
// 1em
```



### dk-explode($str, $delimiter)

Split a string into several parts using a delimiter

```SCSS
dk-explode("The quick brown fox", $delimiter: " ")
// ("The", "quick", "brown", "fox")
```



### dk-implode($keys, $delimiter)

Join list elements to form a single string

```SCSS
dk-implode(("The", "quick", "brown", "fox"), $glue: " ")
// "The quick brown fox"
```



### dk-parse-float($value)

Remove a unit from a value

```SCSS
dk-parse-float(1em)
// 1
```



### dk-rem($value [, $unit: rem/em])

Convert a variable with the unit pixel/rem/em (or unitless) to its equivalent in rem/em

```SCSS
dk-rem(16)
// 1rem

dk-rem(16px)
// 1rem

dk-rem(16px, em)
// 1em

dk-rem(1em)
// 1rem
```



### dk-var($m [, $value: null])

(Deep-) Get/Set a Variable

```SCSS
// get:
$foo: dk-var(node); // Get the value of "node"
$foo: dk-var(node1/node2); // Get the value of "node1/node2"

// set:
$foo: dk-var(node1, 1rem); // Set the value of "node" to 1rem
$foo: dk-var(node1/node2, 1rem); // Set the value of "node1/node2" to 1rem
$foo: dk-var(node1/node2, (node3: 1rem, node4: 2rem)) // Set the value of node2 to a map of sub-nodes
```




## Mixins



### dk-custom-properties($map, $map-properties)

Apply mapped custom-properties ("CSS Variables") based on name/breakpoint-map to the current selector

```SCSS
// Example #1
html { // any selector
  @include dk-custom-properties(
    ( // name/breakpoint-map
      MOBILE: (XS: 0em, MD: 60em)
    ),
    ( // property-map
      "padding": (XS: 1rem, MD: 2rem)
    )
  );
}

/*
Generated output:
html {
  --padding: 1rem; }
    @media (min-width: 60em) {
      html {
        --padding: 2rem; } }
*/



// Example #2
html { // any selector
  @include dk-custom-properties(
    ( // name/breakpoint-map
      MOBILE: (XS: 0em, MD: 60em),
      DESKTOP: (XS: 0em, MD: 60em)
    ),
    ( // property-map
      "padding": (
        MOBILE: (XS: 1rem, MD: 2rem),
        DESKTOP: (XS: 2rem, MD: 4rem)
      )
    )
  );
}

/*
Generated output:
  html {
    --padding: 1rem; }
    @media (min-width: 60em) {
      html {
        --padding: 2rem; } }
      html.desktop {
        --padding: 2rem; }
    @media (min-width: 60em) {
      html.desktop {
        --padding: 4rem; } }
  */
```



### dk-device($name [, $from] [, $until] [, $and] [, $media-type] [, $breakpoints] [, $responsive] [, $static-breakpoint]) { @content; }

Apply `@content` to a device using the `@mixin mq()`

```SCSS
// Prerequisites: name/breakpoint-map set-up with: "$dk: dk-var(device, ...)"

// Example #1
dk-device(DESKTOP) {
  font-size: 1rem;
}

// Example #2
dk-device(DESKTOP, MD) {
  font-size: 1rem;
}

// Example #3
dk-device(DESKTOP, XS, $until: MD) { // with one @mixin mq()-Argument
  font-size: 1rem;
}
```




### dk-theme($name [, $from] [, $until] [, $and] [, $media-type] [, $breakpoints] [, $responsive] [, $static-breakpoint]) { @content; }

Apply `@content` to a theme using the `@mixin mq()`

```SCSS
// Prerequisites: name/breakpoint-map set-up with: "$dk: dk-var(theme, ...)"

// Example #1
dk-theme(DEFAULT) {
  background-color: #000;
}

// Example #2
dk-theme(DARKMODE, MD) {
  background-color: #000;
}

// Example #3
dk-theme(DARKMODE, XS, $until: MD) { // with one @mixin mq()-Argument
  background-color: #000;
}
```

