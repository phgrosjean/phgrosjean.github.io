# Philippe Grosjean's Web Site

Currently, work in progress... migrating from servr/Jekyll to blogdown/Hugo!

- Created a **hugo** branch and moved there.

- Eliminated everything from the site, except CNAME, LICENSE, phgrsojean.github.io.Rproj and README.md

- Installed blogdown and hugo (version 0.31.1) using `blogdown::install_hugo()`.

- Created the Ugo bolgdown site with academic theme using `blogdown::new_site(theme = "gcushen/hugo-academic")`, but it does not work because the directory is not empty. Hence, I installed in another directory, selecting new project, then new website using Hugo, and indicate `gcushen/hugo-academic` as theme in another directory. Then, I copy all files in the initial dir, and make sure build tools are correct.
