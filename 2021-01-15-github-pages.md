# GitHub pages and static websites

In this session we'll talk about different template engines and GitHub pages. 
 
GitHub pages is a nice and free ($0) option for hosting static, [JAMstack](https://jamstack.org/) style, websites. 
It uses markdown based jekyll (https://jekyllrb.com/) for templating, but can host any static content, 
e.g. based on pkgdown (https://pkgdown.r-lib.org/), blogdown (https://bookdown.org/yihui/blogdown/), bookdown (https://bookdown.org/).
 
After an overview, we will concentrate on bookdown as the lowest barrier option for creating manuals for various projects.

Also [pagedown](https://github.com/rstudio/pagedown) and [pagedreport](https://pagedreport.rfortherestofus.com/).

## GitHub pages

> Websites for you and your projects

https://pages.github.com/

### User or organization pages

Add a username.github.io or orgname.github.io repo which will deploy html and md files from the main/master branch.

Example 1: simplest case

- website: https://abbiodiversity.github.io/
- source: https://github.com/ABbiodiversity/abbiodiversity.github.io
- check html
- check settings: https, custom domains
- deployment is trivial, just display the html

Example 2: full statis website

- website: https://borealbirds.github.io/
- built static source: https://github.com/borealbirds/borealbirds.github.io/
- source: https://github.com/borealbirds/borealbirds.github.io/tree/dev
- deployment via gh actions: https://github.com/borealbirds/borealbirds.github.io/blob/dev/.github/workflows/build.yml

### Project pages

Example 1: R package documentation using pkgdown

- source: https://github.com/psolymos/bSims
- `pkgdown::build_site()` will deploy into the `/docs` folder
- and we get this: https://peter.solymos.org/bSims/
- can be set up to auto deploy but I was lazy and it is not really needed that often

Example 2: personal websites and blogs using blogdown

- lots of examples: https://awesome-blogdown.com/
- supports Hugo and Jakyll out of the box (but there are more generators out there: https://jamstack.org/generators/)
- useful if your blog is based on Rmd (lots of stats/ML/ggplot with code + output)

Example 3: hand crafter Jekyll website

- Jekyll docs: https://jekyllrb.com/
- https://borealbirds.github.io/ring-of-fire/
- [`_config.yml`](https://github.com/borealbirds/ring-of-fire/blob/main/docs/_config.yml)
- [markdown files with yaml header](https://raw.githubusercontent.com/borealbirds/ring-of-fire/main/docs/index.md)
- [layout](https://github.com/borealbirds/ring-of-fire/blob/main/docs/_layouts/default.html#L117)
- [species pages](https://raw.githubusercontent.com/borealbirds/ring-of-fire/main/docs/species/AMRO/index.md)
- templating was done using the [wisker](https://github.com/borealbirds/GNM/blob/master/regions/ring-of-fire/rof.R#L231) package

### Documentation with bookdown

- https://bookdown.org/
- output: https://peter.solymos.org/qpad-book/ html + pdf + ebook
- source: https://github.com/psolymos/qpad-book

Our goal is to understand this: https://github.com/ABbiodiversity/species-manual

- [`index.Rmd`](https://github.com/ABbiodiversity/species-manual/blob/master/index.Rmd)
- [Chapters](https://github.com/ABbiodiversity/species-manual/blob/master/01-intro.Rmd) with all the bells and whistles of Rmd

Besides the html version that uses Hugo behind the scenes, we can also get pdf/epub/mobi formats, all based on [pandoc](https://pandoc.org/).

## Next up

CI/CD, GitHub actions, where we are getting dangerously close to talking about Docker...
