# SB-Media
A dead-simple way to do module-based media queries and styles.

## How it works
The two files `no-mq.scss` and `mq.scss` generate two separate stylesheets, one for browsers that do not support Media Queries and one for browsers that do. These files are then included in the `index.html` file using conditional comments to specify which file gets served to which browsers.

**index.html**
```html
<!--[if (lt IE 9) & (!IEMobile)]>
  <link href="c/no-mq.css" rel="stylesheet">
<![endif]-->

<!--[if (gte IE 9) | (IEMobile)]><!-->
  <link href="c/mq.css" rel="stylesheet">
<!--<![endif]-->
```

## The mixin
**_sb-media.scss**
```scss
$default-feature: 'min-width: ';
$no-mq-support: false !default;

@mixin sbmedia($query) {
  @if type-of( $query ) == 'number' {
    $query: $default-feature + $query;
  }
  @if $no-mq-support{
    @content;
  } @else {
    @media ( $query ) {
      @content;
    }
  }
}
```
The mixin is pretty straightforward. It defaults to `min-width` media queries and does not support no-mq browsers. To query for something other than min-width, simply wrap the entire query in quotes. To support no-mq browsers, simply add `$no-mq-support: true;` in your `no-mq.scss` file and import the rest of your project like normal.

## An example
Here is the mixin in action. As you can see, we wrapped the max-width query in quotes.

**_styles.scss**
```scss
body{
  background-color:red;

  @include sbmedia('max-width: 40em'){
    background-color: blue;
  }
}
```

Pretty simple, the generated css is below.

**no-mq.css**
```css
body {
  background-color: red;
  background-color: blue;
}
```
**mq.css**
```css
body {
  background-color: red;
}
@media (max-width: 40em) {
  body {
    background-color: blue;
  }
}
```
