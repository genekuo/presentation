# Hugo

---

## Prerequisites
* Minimum version: v0.62.1

## Create
hugo new site <website name> --format yaml

---

## Generate maon pages and doc pages
hugo gen man
hugo gen doc

---

## Help
hugo <command> <subcommand>--help

---

## Version control
git init .
git add *
git commit -m "messages"

---

## Structure
* archetypes: content templates
* config.yaml: settings. metadata
* content: textual content
* data: structured content
* layouts and themes: template and design for individual pages
* static: additional content
* assets: images, javascript, css
* resources: caches
* public: default output dir
* docs: deployed by GitHub Pages

---

## Theme
* Hugo modules
* downloads theme source  code and places in themes dir
* git sub-modules to reference

---

## Hugo modules
hugo mod init <website name>
* creates go.mod
https://themes.gohugo.io/
* Add theme as dependency in go.mod
* Or
  hugo mod get <location>

---

## config.yaml
theme: <theme location>

---

## dev server
hugo server
localhost:1313
--disableFastRender
--disableLiveReload
go.sum

---

## Configuration
* baseUrl:
* languageCode:
* title:
* theme: author
* menu: (standard)
    main:
      - identifier: about
        name: About
        url: /about
        weight: 1
      - identifier: contact
        name: Contact
        url: /contact
        weight: 2
  params: (specific to the theme)
    color:
    copyright:
    footer:
      - title: About
        content:
      - title: Blog Posts
        recents: blog
        recentCount: 7
      - title: Contact Us
        contact: true

---

## Content Pages

---

## Netlify
* Sign up
* push source code
* New site from git -> app.netlify.com/start
* show advanced -> HOGO_VERSION 0.62.1
* build command: hugo --minify --baseURL $DEPLOY_PRIME_URL -> override url in config.yaml
