# Shell scripts to handle git svn externals recursively


## Example structure

```
/projects
 * /projects/catmonitor { repo: https://mysvn.com/repo/catmonitor }
    * cat.c
    * cat.h
    * /projects/catmonitor/SDK/ { externals: cat-food        {repo: https://mysvn.com/repo/cat-food}
       |                                     tools/cat-log   {repo: https://mysvn.com/repo/cat-log}}
       * /projects/catmonitor/SDK/cat-food/ {repo: https://mysvn.com/repo/cat-food}
          * cat-food-h
          * cat-food.c
       * /projects/catmonitor/SDK/tools/
          * /projects/catmonitor/SDK/tools/cat-log/ {repo: https://mysvn.com/repo/cat-log}
             * cat-log.h
             * cat-log.c
```

## gs-clone `YOUR_REPO` `YOUR_CLONE_DIR`

Clone svn repo from scratch, and recursively clone externals.

```
$ cd /projects
$ gs-clone https://mysvn.com/repo/catmonitor catmonitor
```


### gs-clone

Recursively clone externals of **current svn** .

```
$ cd /projects/catmonitor
$ gs-clone
```


## gs-update

Call `git svn rebase` at current directory and  directories of externals of current svn recursively.

Addtionally call `git stash` before and `git stash pop` after in case of unstaged changes.

```
$ cd /projects/catmonitor
$ gs-update
```

## gs-ls

Format the git `svn show-externals`. Span all externals and parse into 4 tokens separated by space.

    FULL_DIR_WITH_EXTERNAL REPOSITORY_URL EXTERNAL_SUBDIR EXTERNAL_FULLDIR

```
$ cd /projects/catmonitor
$ gs-ls
/projects/catmonitor/SDK https://mysvn.com/repo/cat-food cat-food /projects/catmonitor/SDK/cat-food
/projects/catmonitor/SDK https://mysvn.com/repo/cat-log tools/cat-food /projects/catmonitor/SDK/tools/cat-food
```

### gs-ls 1, gs-ls 2, gs-ls 3, gs-ls 4

Get the nth field of gs-lsext. (Call `| cut -d ' ' -f n` internally)


```
$ cd /projects/catmonitor
$ gs-ls 1
/projects/catmonitor/SDK
/projects/catmonitor/SDK
$ gs-ls 2
https://mysvn.com/repo/cat-food
https://mysvn.com/repo/cat-log
$ gs-ls 3
cat-food
tools/cat-food
$ gs-ls 4
/projects/catmonitor/SDK/cat-food
/projects/catmonitor/SDK/tools/cat-food
```


## gs-splitext

Parse output of `git svn show-externals` from stdin. Usually used after pipe.

```
$ cd /projects/catmonitor
$ git svn show-externals | gs-splitext
/projects/catmonitor/SDK https://mysvn.com/repo/cat-food cat-food /projects/catmonitor/SDK/cat-food
/projects/catmonitor/SDK https://mysvn.com/repo/cat-log tools/cat-food /projects/catmonitor/SDK/tools/cat-food
```


## gs-url

Show svn repository url of the svn directory. (Parse the output of `git svn info`)

```
$ cd /projects
$ gs-url catmonitor
https://mysvn.com/repo/catmonitor
```



