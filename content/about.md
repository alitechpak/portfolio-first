---
title: About
slug: abooooot
---


- [HUGO The World's Fastest Framework for Building Static Websites](#hugo-the-worlds-fastest-framework-for-building-static-websites)
  - [Features](#features)
  - [Install on linux](#install-on-linux)
    - [Via apt-get](#via-apt-get)
    - [Via Homebrew](#via-homebrew)
  - [Create Site](#create-site)
  - [Add Layout pages](#add-layout-pages)
    - [`baseof.html` template](#baseofhtml-template)
    - [`list.html` template](#listhtml-template)
      - [`list.html` for general](#listhtml-for-general)
      - [Add `list.html` for specific folder](#add-listhtml-for-specific-folder)
    - [Add `single.html`](#add-singlehtml)
      - [Add `single.html` for general](#add-singlehtml-for-general)
      - [Add `single.html` for specific folder](#add-singlehtml-for-specific-folder)
  - [Add Content](#add-content)
    - [Add `_index.md` for general](#add-_indexmd-for-general)
      - [Add `_index.md` for specific folder](#add-_indexmd-for-specific-folder)
    - [Add `about.md` sample content page](#add-aboutmd-sample-content-page)
    - [Add `about.md` sample content page](#add-aboutmd-sample-content-page-1)
    - [Update `list.html`](#update-listhtml)
  - [Add `css` stylesheet](#add-css-stylesheet)
    - [Add `css` in `assets` directory](#add-css-in-assets-directory)
    - [Add `scss`](#add-scss)
      - [Add `scss` Components](#add-scss-components)
    - [Add `css` Components (still confusing)](#add-css-components-still-confusing)
  - [Add Components](#add-components)
      - [Add Partials](#add-partials)
      - [Add partials (hard coded)](#add-partials-hard-coded)
      - [Show data in partials from frontmatter](#show-data-in-partials-from-frontmatter)
      - [Include `values` of `_index.md` `keys` in partials](#include-values-of-_indexmd-keys-in-partials)
      - [Loop over `_index.md`'s `keys` and `values` data manually](#loop-over-_indexmds-keys-and-values-data-manually)
      - [Loop over all `_index.md` dynamically](#loop-over-all-_indexmd-dynamically)
      - [Create data just like API](#create-data-just-like-api)
        - [Update `<site>/layouts/partials/hero.html`](#update-sitelayoutspartialsherohtml)
        - [Update `<site>/layouts/partials/grid.html`](#update-sitelayoutspartialsgridhtml)
  - [Deployment](#deployment)
    - [GitHub Actions for Hugo](#github-actions-for-hugo)
    - [Create workflow](#create-workflow)

#   [HUGO](https://gohugo.io/) The World's Fastest Framework for Building Static Websites

##  Features
-   Multilingual
-   Standard binary
-   Drafts
-   Pagination
-   Redirects (Aliases)
-   Asset pipline
-   Live reload
-   Taxonomies
-   Content Types
-   Shortcodes

##  Install on linux

### Via apt-get
-   `sudo apt-get install hugo`: This method is not recommended because the Hugo in linux package managers  for Debian and Ubuntu is usually a few versions behind.
-   `sudo hugo version`: Check version
-   `sudo apt-get remove hugo`: Uninstall

### Via Homebrew
-   `brew install hugo` Install recommended (because of lates version)
-   `hugo version`: Check version

##  Create Site
-   `hugo new site <siteName>`: It will create a new site `mysite`

        Congratulations! Your new Hugo site is created in /media/ali/data/projects/hugo/hugoSecond/mysite.

        Just a few more steps and you're ready to go:

        1. Download a theme into the same-named folder.
        Choose a theme from https://themes.gohugo.io/ or
        create your own with the "hugo new theme <THEMENAME>" command.
        2. Perhaps you want to add some content. You can add single files
        with "hugo new <SECTIONNAME>/<FILENAME>.<FORMAT>".
        3. Start the built-in live server via "hugo server".

        Visit https://gohugo.io/ for quickstart guide and full documentation.

-   Rename `config.toml` to `config.yml` and change syntax to

    ```yml
    baseURL: "http://example.org/"
    languageCode: "en-us"
    title: "My New Hugo Site"
    ```

-   `hugo server -D`: here `-D` flag allows us to see any drafts that we have 
-   site at `http://localhost:1313/` will have no content yet

##  Add Layout pages

### `baseof.html` template

`<site>/layouts/_default/baseof.html`
```html
<!DOCTYPE html>
<html lang="en">
{{ partial "head" . }} <!-- {{/* <site>/layouts/partials/head.html */}}-->
<body>
    <!-- Nav bar -->
    <nav>
        <a href="/">Home</a>
        <div>
            {{ range where .Site.RegularPages "Type" "page" }}
                <a href="{{ .Permalink }}">{{ .Title }}</a>
            {{ end }}
        </div>
    </nav>

    <!-- {{/* Create block content just like django */}} -->
    {{ block "content" . }}

    {{ end }}
</body>
</html>
```

### `list.html` template

####    `list.html` for general

```html
{{ define "content" }}
    <h1>{{ .Title }}</h1>

    {{ range .Params.Components }}
        {{ range $component, $data := . }}
            {{ partial (printf "%s.html" $component) (dict $component $data) }}
        {{ end }}
    {{ end }}
    <br>
    Check out our blog:
    <ul>
        {{ range where .Site.Pages "Type" "blog_posts" }}
            <li><a href="{{ .Permalink }}">{{ .Title }}</a></li>
        {{ end }}
    </ul>
{{ end }}
```

####    Add `list.html` for specific folder

-   Add file `list.html` in `<site>/layouts/list.html`

    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>{{ .Site.Title }}</title>
    </head>
    <body>
        body
    </body>
    </html>
    ```
-   Insert variable `{{ .Site.Title }}`
    -   `.Site` is for `config.yml` file
    -   `.Title` is for information inside `config.yml` file

### Add `single.html`

####    Add `single.html` for general

####    Add `single.html` for specific folder

##  Add Content

### Add `_index.md` for general

####    Add `_index.md` for specific folder


`<site>/content/_index.md`
```md
---
title: Homepageee
---
```
-   Update `list.html`'s body

    ```html
    <h1>{{ .Page.Title }}</h1>
    ```
    even you can remove `.Page`
    ```html
    <h1>{{ .Title }}</h1>
    ```
    Result will be same

### Add `about.md` sample content page

-   Add `about.md` and `contact.md` in _content_ i.e. `content/about.md`
-   Update titles

    ```md
    ---
    title: "About"
    ---
    ```

### Add `about.md` sample content page

### Update `list.html`

```html
{{ range .Pages }}
    {{ .Title }}
{{ end}}
```

```html
{{ range where .Site.Pages "Type" "blog_posts" }}
    <a href={{ .Permalink }}>{{ .Title }}</a>
{{ end }}
```
**DESCRIPTION**:<br>
Loop over pages in `content` directory _Type_ (in) the `blog_posts` directory

-   `slug: path`: changes the url of a file
-   `url: path`: changes the lengthy url of a file

##  Add `css` stylesheet

-   Create dir and file `static/css/main.css`
-   Update `main.css`
-   Create dir and file `layout/partials.head.html`
-   Cut head from `baseof.html` and paste into `head.html`
-   Add to `head.html`

    ```html
    <link rel="stylesheet" src="css/main.css">

### Add `css` in `assets` directory
-   Create dir `<site>/assets`
-   Move `css/main.css` inside  `<site>/assets/css/main.css`
-   update `head.html`

    ```html
    {{ $style := resources.Get "css/main.css" }}
    <link rel="stylesheet" href="{{ $style.Permalink }}>
-   Restart the server

### Add `scss`
-   Rename `assets/css/main.css` to `assets/scss/main.scss`
-   Update `head.html`

    ```html
    {{ $style := resources.Get "scss/main.scss" }}
    {{ $style = $style | toCSS | minify | fingerprint }}
    <link href="{{ $style.Permalink }}" rel="stylesheet">
    ```

    **`fingerprint`** function: calculates the sha256 hash of the file and includes it in the filename. This is an easy way to implement cache busting for the stylesheet because the filename will change whenever the content changes and as a result possible browser caches will then be invalidated automatically.

    **`minify`** function: only put the stylesheet in condensed format. Condensed version have less whitespaces so uses less bandwidth

    **`toCSS`** function: Converts the `.scss` file into `.css` file

####    Add `scss` Components
-   Create a dir inside `scss` i.e. `<site>/scss/components`
-   You can create as many files as you can i.e. `file1.scss`, `file2.scss`...
-   Import component files in `main.scss`

    ```scss
    @import "components/file1";
    @import "components/file2";
    ```
You are done!


### Add `css` Components (still confusing)
-   Create a dir `<site>/assets/css/components`
-   Create component files inside `components` folder i.e. `test1.css`, `test2.css`
-   Update `head.html`

```html
{{ $style := resources.Get "css/main.css" }}
{{ $components := readDir "assets/css/components" }}
{{ range $components }}
    {{ $component := resources.Get (printf "css/components/%s .Name") }}
    {{ $style = slice $component $style | resources.Concat .Name }}
{{ end }}
{{ $style = $style | minify | fingerprint }}
<link href="{{ $style.Permalink }}" rel="stylesheet">
```

`{{ $style = slice $style | resources.Concat "all.css" | minify | fingerprint }}`

`all.css` will be shown as a name of stylesheet

## Add Components

`<site>/content/_index.md`
```yml
---
custome_name:
    - hero: I'm a hero
    - grid: I'm a grid
    - blurg: I'm a blurb
---
```

Print on `list.html` page
`layouts/_default/list.html`
```html
{{ range .Params.Custom_component_name }}
    {{ . }}
{{ end }}
```
-   Capitalize the first letter
-   Check on HOME page

####    Add Partials
-   Create files inside `partials`
    -   `hero.html`
    -   `grid.html`
    -   `blurb.html`
    -   any number of partials
-   Update `<site>/layouts/partials/hero.html`

    ```html
    <h3>This is a hero</h3>
    ```
-   Similarly update `grid.html` and `blurb.html`
-   Include in `layouts/_default/list.html`

####    Add partials (hard coded)
```html
{{ range .Params.Custom_name }}
    {{ if .hero }}
        {{ partial "hero.html" }}
    {{ end }}
{{ end }}
```
-   If `.hero` parameter is available in `_index.md` file then include `hero.html`
-   Use here **`partial`** not `partials`

####    Show data in partials from frontmatter
-   Update `layouts/_default/list.html`

```html
{{ range .Params.Custom_name }}
    {{ if .hero }}
        {{ partial "hero.html" (dict "hero" .hero) }}
    {{ end }}
{{ end }}
```
-   (`dict` `key` `.value`)
-   `key` is the name of parameter in `_index.md`
-   `.value` is the data of parameter in `_index.md`

####    Include `values` of `_index.md` `keys` in partials
Update `layouts/partials/hero.html`
```html
<h3>This is a hero</h3>
<h3>{{ .hero }}</h3>
```

####    Loop over `_index.md`'s `keys` and `values` data manually

```html
{{ range .Params.Custom_name }}
    {{ range $key, $value := . }}
        {{ $key }}
    {{ end }}

    {{ if .hero }}
        {{ partial "hero.html" (dict "hero" .hero) }}
    {{ end }}
{{ end }}
```

####    Loop over all `_index.md` dynamically
```html
{{ range .Params.Custom_name }}
    {{ range $component, $data := . }} {{/* $component, $data are custom names */}}
        {{ partial (printf "%s.html" $component) (dict $component $data) }}
    {{ end }}
{{ end }}
```

####    Create data just like API

```yml
title: Homepage
components:
    - hero:
        title: I'm a hero
        color: green
    - grid:
        title: I'm a grid
        items:
            - title: Item 1
              desc: I'm the first item
            - title: Item 2
              desc: 2nd!!!
    - blurb: I'm a blurb
    # - hero: hero!! (This should not be used It will return an error because of duplicate key)
    - hero: # This duplicate is in same manner as above so its OK
        title: Second hero
        color: lightblue 
    - text block: Here's some cool text
```

#####   Update `<site>/layouts/partials/hero.html`

```html
<h3 style="background-color: {{ .hero.color }};">{{ .hero.title }}</h3>
```

#####   Update `<site>/layouts/partials/grid.html`

```html
<h3>{{ .grid.title }}</h3>

{{ range .grid.items }}
    {{ .title }} <br>
    {{ .desc }} <br>
{{ end }}
```

##  Deployment



### [GitHub Actions for Hugo](https://github.com/marketplace/actions/hugo-setup)

### Create workflow
-   Create dirs `<site>/.github/workflows/gh-pages.yml`