simply-deploy
=============

A simple Phing-based deployment script. Written in a few hours, I wanted the 
script to adhere to a few key principles:

 - Atomic - no partial uploads
 - Git-based - deploy the latest tagged release, downloaded from a repo
 - Tidy - cleans up after itself (both on local and remote)


Nutshell
--------

After calling `phing`, the following things happen:

 - The repo is downloaded into a temporary directory using `git archive`
 - An "install" script is created in the temporary directory
 - A temporary directory is created on the remote host
 - The install script and archive are pushed up to the remote host
 - The install script is executed remotely
 - The install script unpacks the archive and rsyncs the temp directory's contents into the live project folder
 - The "current" symlink is deleted and repointed at the latest release
 - The install script is deleted as are all temporary directories using a safe `rmdir` call

Usage
-----

The script must be called from within its directory. By that, I mean you can't
currently call it like:

`phing -f my/project/directory/build.xml`

You must be in `my/project/directory` and call `phing`

Todo
----

As this script currently deploys the entire repo, there's no scope for pushing
up local config files. I wouldn't commit sensitive information (passwords or
keys) to the repo, so those files must be sourced locally, rather than from
the repo. This is a very simple thing to fix, I just need a few moments and some real use cases to test it out on.
