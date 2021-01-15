# GitHub pages and static websites

In this session we'll talk about different template engines and GitHub pages. 
 
GitHub pages is a nice and free ($0) option for hosting static, [JAMstack](https://jamstack.org/) style, websites. 
It uses markdown based jekyll (https://jekyllrb.com/) for templating, but can host any static content, 
e.g. based on pkgdown (https://pkgdown.r-lib.org/), blogdown (https://bookdown.org/yihui/blogdown/), bookdown (https://bookdown.org/).
 
After an overview, we will concentrate on bookdown as the lowest barrier option for creating manuals for various projects.

__!!! Reschedule for Thursday morning so that Erin can make it? !!!__

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

