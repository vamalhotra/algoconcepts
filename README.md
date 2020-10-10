# Site generation details

There are multiple ways to convert content written in markdown to html

1) Static site generators like [Jekyll](https://jekyllrb.com/tutorials/convert-site-to-jekyll/)

2) Standalone parser like [showdown](https://github.com/showdownjs/showdown) that is essentially a js file which can convert md to html. See sample usage [here](https://markdown-to-github-style-web.com/)

3) Github [REST API](https://docs.github.com/en/free-pro-team@latest/rest/reference/markdown) to convert markdown to html. 

If you have large no. of markdown files, you can use jekyll to convert all of them to html. 

Run `jekyll new` to initialize a new site. Edit _config.yml, follow naming convention in creating files inside _posts and you're good to go.  Run `jekyll serve` to compile all md files to html files and start local server or `jekyll build` to just compile the md files to html.  Refer to 'permalink' in _config.yml for url structure.

Three part tutorial:

[Part 1](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-jekyll-development-site-on-ubuntu-16-04)

[Part 2](https://www.digitalocean.com/community/tutorials/exploring-jekyll-s-default-content)

[Part 3](https://www.digitalocean.com/community/tutorials/how-to-control-urls-and-links-in-jekyll)

## Adding FrontMatter to markdown files or fix filenames of files

jekyll won't process the post unless it has front matter (pair of three dashes --- marking start and end of front matter.)

You can also add variables like title, date, category inside front matter. 

Both title and date are mandatory inside front matter. 

The date is used to display on the post while the date in filename is only useful for syntax by jekyll compiler i.e. jekyll will ignore files with future date until the future date arrives and it will ignore files without valid date.)

Refer to csutils\utils.cs for helper function to fix all md files.

## Website structure and regular tasks

1. Put new article with front-matter and proper naming convention into _posts directory

2. Run following functions in csutils\utils.cs	

   ```c#
   var index = GenerateIndex(dir);
   PrintIndex(index, dir);
   ```

3. Commit new *.search-index.md and new post.md files

4. Run jekyll serve to verify new post is available locally or do git commit and git push to view it on server

## How to generate index.html [Deprecated]

Update: We no longer need this. Kept here for future needs.

Currently, we do not have any navigational layout which can allow to navigate between multiple pages. We are currently using [apindex](https://github.com/libthinkpad/apindex) to generate index page for each directory. 

To run apindex, we need "apindex_share" directory which defines template and images and apindex.py which contains code to generate index.html. Both the directory and code file have been copied into this repo and modified slightly (share/apindex -> apindex_share and . directory exclusion as below)

Run `apindex.py .` to generate index.html for all directories except directories starting with `.` or `_` except `_site`. Refer `def getChildren(self):` in apindex.py for which directories to exclude

By default, it recursively writes index.html for each child directory also. You can control recursive behavior using `IndexWriter.writeIndex(sys.argv[1], recursive=False|True)`