# Site generation details

There are multiple ways to convert content written in markdown to html

1) Static site generators like [Jekyll](https://jekyllrb.com/tutorials/convert-site-to-jekyll/)

2) Standalone parser like [showdown](https://github.com/showdownjs/showdown) that is essentially a js file which can convert md to html. See sample usage [here](https://markdown-to-github-style-web.com/)

3) Github [REST API](https://docs.github.com/en/free-pro-team@latest/rest/reference/markdown) to convert markdown to html. 

If you have large no. of markdown files, you can use jekyll to convert all of them to html. 

Run `jekyll new` to initialize a new site. Edit _config.yml, follow naming convention and you're good to go.  Run `jekyll serve` to compile all md files to html files and start local server or `jekyll build` to just compile the md files to html.  Refer to 'permalink' in config.yml on site structure.

Three part tutorial:

[Part 1](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-jekyll-development-site-on-ubuntu-16-04)

[Part 2](https://www.digitalocean.com/community/tutorials/exploring-jekyll-s-default-content)

[Part 3](https://www.digitalocean.com/community/tutorials/how-to-control-urls-and-links-in-jekyll)

## How to generate index.html [Deprecated]

Currently, we do not have any navigational layout which can allow to navigate between multiple pages. We are currently using [apindex](https://github.com/libthinkpad/apindex) to generate index page for each directory. 

To run apindex, we need "apindex_share" directory which defines template and images and apindex.py which contains code to generate index.html. Both the directory and code file have been copied into this repo and modified slightly (share/apindex -> apindex_share and . directory exclusion as below)

Run `apindex.py .` to generate index.html for all directories except directories starting with `.` or `_` except `_site`. Refer `def getChildren(self):` in apindex.py for which directories to exclude

By default, it recursively writes index.html for each child directory also. You can control recursive behavior using `IndexWriter.writeIndex(sys.argv[1], recursive=False|True)`