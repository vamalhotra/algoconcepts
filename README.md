# Site generation details

There are multiple ways to convert content written in markdown to html

1) Static site generators like [Jekyll](https://jekyllrb.com/tutorials/convert-site-to-jekyll/)

2) Standalone parser like [showdown](https://github.com/showdownjs/showdown) that is essentially a js file which can convert md to html. See sample usage [here](https://markdown-to-github-style-web.com/)

3) Github [REST API](https://docs.github.com/en/free-pro-team@latest/rest/reference/markdown) to convert markdown to html. 

If you have large no. of markdown files, you can use jekyll to convert all of them to html. You need to create _config.yml, _layouts folder and md files. Run `jekyll serve` to compile all md files to html files and start local server or `jekyll build` to just compile the md files to html.  

## How to generate index.html

Currently, we do not have any navigational layout which can allow to navigate between multiple pages. We are currently using [apindex](https://github.com/libthinkpad/apindex) to generate index page for each directory. 

To run apindex, we need "apindex_share" directory which defines template and images and apindex.py which contains code to generate index.html. 

Run `apindex.py .` to generate index.html for all directories except directories starting with `.` or `_` except `_site`. Refer `def getChildren(self):` in apindex.py for which directories to exclude

By default, it recursively writes index.html for each child directory also. You can control recursive behavior using `IndexWriter.writeIndex(sys.argv[1], recursive=False|True)`