Heroku buildpack: Wonderbread
=======================

This is a [Heroku buildpack](http://devcenter.heroku.com/articles/buildpacks).

Usage:

This buildpack is designed to be used as the part of a larger [multi-stage buildpack](https://github.com/ddollar/heroku-buildpack-multi) to run any script or bash command the user wants. The buildpack executes the enviornment variable WONDERBREAD_COMMAND (can be an explicit bash command or an instruction to run a bash script) in the main app directory with the BUILD_DIR, CACHE and ENVIR_DIR all passed in as arguments. If running the command evaluates to false or exits nonzero the build fails. Otherwise the command runs and moves onto the next stage in the buildpack.

Set the command using heroku

    # Executes any bash command
    heroku config:set VERIFIER_COMMAND='any_bash_command'

    # Run a script stored in the app dirctory
    heroku config:set VERIFIER_COMMAND='sh any_script.sh'


Example Uses
-------

Preventing unintentional deployment to production:

Set verifier command to check a file for a production key to authorize deployment

    heroku config:set VERIFIER_COMMAND='grep -q $secretproductionkey SOMEFILE'


Specify the buildpack when pushing to heroku

    $ heroku create --stack cedar --buildpack https://github.com/henryaspegren/heroku-buildpack-wonderbread.git

    $ git push heroku master
    ...
    -----> Fetching custom buildpack... done
    -----> Wonderbread app detected
    -----> Looking for WONDERBREAD_COMMAND in ENVIR_DIR
    -----> Setting command to grep -q $secretproductionkey SOMEFILE
    -----> Executing command specified
           Set command evaluated to false. Build failed # without production key the build fails

    !      Push Rejected, failed to compile Wonderbread app



Development
-------

To use this buildpack, fork it on Github.  Push up changes to your fork, then create a test app with `--buildpack <your-github-url>` and push to it. This buildpack is a platform for doing anything you want during a push to heroku

