# How to edit the Talk To The Duck blog

This site is built with [Hugo](https://gohugo.io/).
Hugo renders the pages in a server side fashion.


To see how to use the Hugo theme see the [Bootstrap theme reference](https://hbs-cn.razonyang.com/en/)

If you don't want the draft pages to be rendered:
```shell
hugo server
```

If instead you want the draft pages to be rendered:
```shell
hugo server -D
```

## How to create a post
```shell
hugo new posts/first-post.md
```
Don't forget to commit.

## How to delete a post
Search for the content and delete it.
Be careful with cross-links in the website.
Don't forget to commit.

# How to use Git modules
See this [article](https://www.vogella.com/tutorials/GitSubmodules/article.html) by Vogella. 


Starting from the version 0.93 of Hugo, this site cannot be built anymore, probably because of a bug introduced in 0.93

If you wish to execute Hugo with Docker, run the script below:
```shell
docker run --rm -it \
    -v $(pwd):/src \
    -p 1313:1313 \
    klakegg/hugo:0.92.2 \
    server
```

If you wish to see the draft articles then you can use the command below:

```shell
docker run --rm -it \
    -v $(pwd):/src \
    -p 1313:1313 \
    klakegg/hugo:0.92.2 \
    server -D
```

```shell
docker run --rm -it \
    -v $(pwd):/src \
    -p 1313:1313 \
    klakegg/hugo:0.111.3-ext-ubuntu-onbuild \
    server -D --disableFastRender --ignoreCache --noHTTPCache
```

How to publish an article in posts:

hugo new posts/what-is-code-formatting-and-why-does-it-matter.md
hugo new posts/Qu-est-ce-que-la-mise-en-page-du-code-et-pourquoi-est-ce-important.md

To localize the permalink, add the slug to the front matter with the translated permalink.


DA QUI

https://gohugo.io/content-management/page-bundles/
