Heroku buildpack: Verifier
=======================

This is a [Heroku buildpack](http://devcenter.heroku.com/articles/buildpacks).

Usage:

This buildpack is designed to be used as the firt part of a [multi-stage buildpack](https://github.com/ddollar/heroku-buildpack-multi) to prevent unintentional deployment to production by checking a user-specified file for a set key. The buildpack checks to see if the environment variable VERIFIER_COMMAND (can be a file or something else) contains a specific key set by the environment variable VERIFIER_KEY. If the VERIFIER_COMMAND lacks the key or either the key or command is not set by the environment variables the buildpack rejects the push. If the key is found the buildpack terminates successfuly and moves on to the next buildpack (specified by the multi-stage buildpack).

To use the git commit messages as the VERIFIER_COMMAND add following to your build script

    #!/usr/bin/env bash
    ...
    echo 'Adding GITLOG'
    git log --oneline -n 2 > GITLOG #saves the last two commit messages to the GITLOG
    git add GITLOG

And set environment variables in heroku

    heroku config:set VERIFIER_COMMAND='GITLOG'
    heroku config:set VERIFIER_KEY='$secret_production_key'


-----

Example usage:


    $ heroku create --stack cedar --buildpack https://github.com/henryaspegren/heroku-buildpack-verifier.git

    $ git push heroku master
    ...
    -----> Fetching custom buildpack... done
    -----> VerifierBuildPack app detected
    -----> Looking for a VERIFIER_KEY and VERIFIER_COMMAND in ENVIR_DIR
    -----> Setting key to $VERIFIER_KEY
    -----> Setting command to $VERIFIER_COMMAND
    -----> Searching VERIFIER_COMMAND for VERIFIER_KEZY to authorize deployment
           Verifier command lacks the required key to push to production. Build failed

    !      Push Rejected, failed to compile VerifierBuildPack app



Hacking
-------

To use this buildpack, fork it on Github.  Push up changes to your fork, then create a test app with `--buildpack <your-github-url>` and push to it.

