# gitsvnutil
shell scripts to handle git svn externals recursively

# psudo structure

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

# Deploy

## gs-clone YOUR_REPO YOUR_CLONE_DIR 

Desc: Clone svn repo from scratch, and recursively clone externals

### Usage
```
$ cd /projects
$ gs-clone https://mysvn.com/repo/catmonitor catmonitor
```


## gs-clone

Desc: Recursively clone externals of current svn

### Usage
```
$ cd /projects/catmonitor
$ gs-clone
```

## gs-update

Desc: Call `git svn rebase` at current directory and  directories of externals of current svn recursively.
Note: Addtionally call git stash before and git stash pop after in case of unstaged changes.

### Usage
```
$ cd /projects/catmonitor
$ gs-update
```


--

# Tools

## gs-lsext

Desc: 

Format the git `svn show-externals`. Span all externals and parse into 4 tokens separated by space.

    FULL_DIR_WITH_EXTERNAL REPOSITORY_URL EXTERNAL_SUBDIR EXTERNAL_FULLDIR


### Usage
```
$ cd /projects/catmonitor
$ gs-lsext
/projects/catmonitor/SDK https://mysvn.com/repo/cat-food cat-food /projects/catmonitor/SDK/cat-food
/projects/catmonitor/SDK https://mysvn.com/repo/cat-log tools/cat-food /projects/catmonitor/SDK/tools/cat-food
```
