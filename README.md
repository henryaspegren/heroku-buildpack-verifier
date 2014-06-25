Heroku buildpack: Verifier
=======================

This is a [Heroku buildpack](http://devcenter.heroku.com/articles/buildpacks).

Usage:

This buildpack is designed to be used as part of a [multi-stage buildpack](https://github.com/ddollar/heroku-buildpack-multi) to prevent unintentional deployment to production. It checks to see if the SHORTLOG file contains a specific key set by the environment variable VERIFIER_KEY. If the SHORTLOG lacks the key or a key is not set the buildpack rejects the push.

To use the git commit messages as the SHORTLOG add following to your build script
    #!/usr/bin/env bash
    ...
    echo 'Adding SHORTLOG'
    git log --oneline -n 2 > SHORTLOG #saves the last two commit messages to the SHORTLOG
    git add SHORTLOG


-----

Example usage:


    $ ls
    SHORTLOG

    $ heroku create --stack cedar --buildpack https://github.com/henryaspegren/heroku-buildpack-verifier.git

    $ git push heroku master
    ...
    -----> Heroku receiving push
    -----> Fetching custom buildpack
    -----> VerifierBuildPack app detected
    -----> Looking for a VERIFIER_KEY in ENVIR_DIR
    -----> Set key to $key
    -----> Searching SHORTLOG for key to authorize deployment
           Key match not found in most recent two git commits. Build failed

    !      Push Rejected, failed to compile VerifierBuildPack app

The buildpack will detect if your app

Hacking
-------

To use this buildpack, fork it on Github.  Push up changes to your fork, then create a test app with `--buildpack <your-github-url>` and push to it.

