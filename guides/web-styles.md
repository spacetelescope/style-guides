# Web Style Guide

The following is a style guide for the STScI-hosted web applications to have the 'look and feel' of the STScI branding.

## Typography

![Typography](https://github.com/spacetelescope/style-guides/blob/master/guides/images/web-style-typography.png)

```css
h1, h2, h3, h4, h5, h6 {
    font-family: 'Oswald', sans-serif;
}

body {
    font-family: 'Overpass', sans-serif;
}
```


## Colors

![Colors](https://github.com/spacetelescope/style-guides/blob/master/guides/images/web-style-colors.png)


## Iconography

![st-logo](https://github.com/spacetelescope/style-guides/blob/master/guides/images/stsci-logo.png)


## Buttons

![Buttons](https://github.com/spacetelescope/style-guides/blob/master/guides/images/web-style-buttons.png)

```css
.btn-primary, .btn-primary.disabled, .btn-outline-primary:hover,
.btn-outline-primary.active {
    background-color: #c85108 !important;
    border-color: #c85108 !important;
    color: white !important;
    border-radius: 0px;
}

.btn-primary:hover, .btn-primary.active, .btn-outline-primary,
.show > .btn-primary.dropdown-toggle {
    background-color: white !important;
    border-color: #c85108 !important ;
    color: #c85108 !important;
    border-radius: 0px;
}

.btn:focus, .btn.focus, .btn:active:focus, .btn.active:focus, .btn:active,
.btn.active, .show > .btn.dropdown-toggle:focus {
    box-shadow: none !important;
}
```

## Padding
