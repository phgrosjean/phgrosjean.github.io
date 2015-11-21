---
layout: post
title:  "Introduction to SciViews"
categories: [sciviews, rstats]
tags: [sciviews, svMisc, svUnit, tcltk2]
---

My **new bio** should go here...


----

_This website was made using [**servr**](https://github.com/yihui/servr). For more info, see https://github.com/yihui/knitr-jekyll. Use `servr::jekyll()` in the R Studio console to start updating the site._


## R code chunks

Now we write some R code chunks in this post. For example,


{% highlight r %}
options(digits = 3)
cat("hello world!")
{% endhighlight %}



{% highlight text %}
## hello world!
{% endhighlight %}



{% highlight r %}
set.seed(123)
(x = rnorm(40) + 20)
{% endhighlight %}



{% highlight text %}
##  [1] 19.4 19.8 21.6 20.1 20.1 21.7 20.5 18.7 19.3 19.6 21.2 20.4 20.4
## [14] 20.1 19.4 21.8 20.5 18.0 20.7 19.5 18.9 19.8 19.0 19.3 19.4 18.3
## [27] 20.8 20.2 18.9 21.3 20.4 19.7 20.9 20.9 20.8 20.7 20.6 19.9 19.7
## [40] 19.6
{% endhighlight %}



{% highlight r %}
# generate a table
knitr::kable(head(mtcars))
{% endhighlight %}



|                  |  mpg| cyl| disp|  hp| drat|   wt| qsec| vs| am| gear| carb|
|:-----------------|----:|---:|----:|---:|----:|----:|----:|--:|--:|----:|----:|
|Mazda RX4         | 21.0|   6|  160| 110| 3.90| 2.62| 16.5|  0|  1|    4|    4|
|Mazda RX4 Wag     | 21.0|   6|  160| 110| 3.90| 2.88| 17.0|  0|  1|    4|    4|
|Datsun 710        | 22.8|   4|  108|  93| 3.85| 2.32| 18.6|  1|  1|    4|    1|
|Hornet 4 Drive    | 21.4|   6|  258| 110| 3.08| 3.21| 19.4|  1|  0|    3|    1|
|Hornet Sportabout | 18.7|   8|  360| 175| 3.15| 3.44| 17.0|  0|  0|    3|    2|
|Valiant           | 18.1|   6|  225| 105| 2.76| 3.46| 20.2|  1|  0|    3|    1|



{% highlight r %}
(function() {
  if (TRUE) 1 + 1  # a boring comment
})()
{% endhighlight %}



{% highlight text %}
## [1] 2
{% endhighlight %}



{% highlight r %}
names(formals(servr::jekyll))  # arguments of the jekyll() function
{% endhighlight %}



{% highlight text %}
## [1] "dir"     "input"   "output"  "script"  "serve"   "command"
## [7] "..."
{% endhighlight %}

Just to test inline R expressions[^2] in **knitr**, we know the first element in `x` (created in the code chunk above) is 19.44. You can certainly draw some graphs as well:

[^2]: The syntax in R Markdown for inline expressions is `` `r code` ``, where `code` is the R expression that you want to evaluate, e.g. `x[1]`.


{% highlight r %}
par(mar = c(4, 4, .1, .1))
plot(cars, pch = 19, col = 'green')  # a scatterplot
{% endhighlight %}

![A scatterplot of the cars data](http://tinyurl.com/sciviews/jekyll/2015-11-17-Introduction-to-SciViews/cars-1.png) 

## The build script

Zero-configuration is required for `servr::jekyll()` to work on your Jekyll website. However, there is always demand for more control over some options, which can be defined in a custom build script. Here are the arguments of `servr::jekyll()`:


{% highlight r %}
jekyll(dir = ".", input = c(".", "_source", "_posts"), output = c(".", 
    "_posts", "_posts"), script = c("Makefile", "build.R"), serve = TRUE, 
    command = "jekyll build", ...)
{% endhighlight %}

By default, `jekyll()` looks for `.Rmd` files under the root directory, the `_source` directory, and the `_posts` directory of the Jekyll website. For example, if you put your R Markdown posts under `_source`, they will be compiled to the `_posts` directory[^3].

[^3]: The reason that we may need to write R Markdown posts in `_source` instead of `_posts` is that Jekyll has [a subtle bug](https://github.com/jekyll/jekyll/pull/3147) (fixed in v2.5.3): its variable `site.posts` will count `.Rmd` files under `_posts` as well. The consequence is, if you list all the posts of your website, the post `_posts/yyyy-mm-dd-foo.md` will show up twice due to the existence of `_posts/yyyy-mm-dd-foo.Rmd`, therefore I would recommend you to put your R Markdown posts in a separate directory, such as `_source`.

The `script` argument specifies a Makefile or an R script to be used to compile your R Markdown files. If it is a Makefile, `jekyll()` will run `make -q` to see if the site needs to be recompiled, then `make` if it does. If the script is an R script, say, named `build.R`, it is called via command line of the form

    Rscript build.R arg1 arg2

See `?servr::jekyll` for more details. You can define all your **knitr** options and any other options in this R script. See the script [build.R](https://github.com/yihui/knitr-jekyll/blob/gh-pages/build.R) in the knitr-jekyll repository for an example: it will automatically set up the output renderers for **knitr**, e.g., when the Jekyll Markdown engine is `kramdown`, this script will call `knitr::render_jekyll()` so that the code chunk output will be put inside the Liquid tag `{% raw %}{% highlight lang %} {% endhighlight %}{% endraw %}`; it also sets up some **knitr** chunk and package options so that figures can be displayed correctly. For those who do not wish to store images in GIT (because normally they are binary files), you may check out how I host my images in Dropbox for this repository (see the code below `Sys.getenv('USER') == 'yihui'`).

## On the Markdown renderers

Jekyll supports a number of Markdown renderers, such as kramdown, redcarpet, rdiscount, and so on. At the moment, it is a little annoying that kramdown supports LaTeX math expressions via `$$ math $$`[^4], but does not support syntax highlighting of code blocks using the three backticks syntax (you must write the awkward Liquid tags); on the other hand, redcarpet does not support LaTeX math but does support three backticks. In my opinion, all the different flavors and implementations of Markdown is the biggest problem of Markdown, since there is not an unambiguous spec for Markdown. [CommonMark](http://commonmark.org) looks like a promising project to set up a common spec for Markdown, and [Pandoc](http://johnmacfarlane.net/pandoc/) is a great implementation that has brought almost all the features that you may ever need in Markdown. You may find some Pandoc plugins for Jekyll by searching online. However, GitHub Pages does not support arbitrary Jekyll plugins, so you cannot just use a Pandoc plugin there, but that does not mean you cannot use Pandoc locally, nor does it mean you cannot push locally compiled HTML pages to GitHub Pages[^5].

[^4]: Unfortunately, kramdown does not support math expressions in single dollars, e.g. `$ \alpha $`.

[^5]: If you choose to generate your Jekyll website locally, and push the HTML files to GitHub, you will need the file [`.nojekyll`](https://help.github.com/articles/using-jekyll-with-pages) in the root directory of your website.

I'd love you to [fork](https://github.com/yihui/knitr-jekyll) this repository, make some (hopefully minor) changes, and [let me know](https://github.com/yihui/knitr-jekyll/issues) your success of using Pandoc with Jekyll. Happy hacking, and good luck!
