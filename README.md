# Philippe Grosjean's Web Site

Currently, work in progress... migrating from servr/Jekyll to blogdown/Hugo!

- Created a **hugo** branch and moved there.

- Eliminated everything from the site, except CNAME, LICENSE, phgrsojean.github.io.Rproj and README.md

- Installed blogdown and hugo (version 0.31.1) using `blogdown::install_hugo()`.

- Created the Ugo blogdown site with academic theme using `blogdown::new_site(theme = "gcushen/hugo-academic")`, but it does not work because the directory is not empty. Hence, I installed in another directory, selecting new project, then new website using Hugo, and indicate `gcushen/hugo-academic` as theme in another directory. Then, I copy all files in the initial dir, and make sure build tools are correct.

- Customized config.toml.

- Added pages...

- Choose banner images:
    * http://www.publicdomainpictures.net/view-image.php?image=21486&picture=beach-background, CC0 Public Domain
    * http://www.publicdomainpictures.net/view-image.php?image=21644&picture=beach-wallpaper, CC0 Public Domain
    * http://www.publicdomainpictures.net/view-image.php?image=42719&picture=cell, CC0 Public Domain
    * http://www.publicdomainpictures.net/view-image.php?image=137855&picture=sunset-beach, CC0 Public Domain
    * http://www.publicdomainpictures.net/view-image.php?image=210025&picture=solitary-sunset, CC0 Public Domain
    * Other images from pixabay.com, CC0 Public Domain
