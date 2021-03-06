# GraphViz editor

A GraphViz editor, which is live on [graphviz.herokuapp.com](http://graphviz.herokuapp.com) uses the following buildpacks:
    
    https://github.com/heroku/heroku-buildpack-php
    https://github.com/mrquincle/heroku-buildpack-graphviz

This is not to worry about, they will be automatically picked up if you set the `heroku-buildpack-multi` as `BUILDPACK_URL` as indicated below. Note that the packages above are slightly different from the original ones, just check the source code of the [compile](https://github.com/mrquincle/heroku-buildpack-graphviz/blob/master/bin/compile) script to get rid of the magic you might think is going on. An important change was `--disable-perl` for the `configure` script, or else the installation breaks.

On the moment the `CACHE_DIR` is emptied with the top-line in the `.buildpacks` file:

    https://github.com/mrquincle/heroku-buildpack-clearcache

If everything runs fine, it is safe to remove this line. It will allow you to cache the build results over different deployments on heroku.

To install:

    heroku apps:create invent_name
    heroku config:set BUILDPACK_URL=https://github.com/ddollar/heroku-buildpack-multi.git
    git add -u .
    git push heroku master

Perhaps there are some problems with the composer.lock file. In that case download the composer and run it.

    curl -sS https://getcomposer.org/installer | php
    sudo mv composer.phar /usr/local/bin/composer
    composer update
    git add composer.lock

To redeploy to heroku, this trick might be handy:

    git push -f heroku master^:master
    git push heroku master

It pushes the previous version and then the current version to heroku.

The environmental variables in the buildpack seems to be overwritten by the php one, or actually what happens, I don't care... They don't get exported... So, this means you have to do this yourself.

    heroku config:set PATH=/app/.heroku/php/bin:/app/.heroku/php/sbin:/app/.heroku/graphviz/bin:/usr/local/bin:/usr/bin:/bin
    heroku config:set LD_LIBRARY_PATH=/app/.heroku/graphviz/lib

Use `heroku run bash` to check if `echo $PATH` has other variables and adjust accordingly.

This information should be good enough to get you up and running!

## Copyrights

Contributions: 
* Main code comes mainly from Potherca. 
* Bug fixing by Anne

Summarized:

* Author: Potherca
* Author: Anne van Rossum
* License: unknown, ask Potherca
* Date: 2012

