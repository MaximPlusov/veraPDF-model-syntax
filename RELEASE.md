Release Process
===============
Introduction
------------
###Audience
 - veraPDF development staff looking to release a new version;
 - anyone interested in knowing how our release process works; and
 - anyone who wants to change the version number or re-package our software.

###Pre-requisites
If you're still reading you'll also need an understanding and some experience using [Git](https://git-scm.com) and [Maven](https://maven.apache.org/). A quick brush up on [Semantic Versioning](http://semver.org/) and a typical [Git branching workflow](http://nvie.com/posts/a-successful-git-branching-model/) and we're ready to go.

Releasing
---------
The veraPDF software projects use the `MINOR` version number to indicate the development status of a particular version. An even number signifies a release version while an odd numbers are assigned to development prototypes. We increment the `MINOR` version number twice at each release milestone. The first increment is from the current odd numbered development version number to the new even release version.

You should use the latest version of the `integration` branch of the project. In the following example we'll start from scratch and assume:
 - we're releasing `0.6.x` from development version `0.5.x`
 - you're using a bash shell; and
 - the `git remote` name for the repo used is `origin`.

###Getting to integration
Clone the repo:

    git clone git@github.com:veraPDF/veraPDF-model-syntax.git
    cd veraPDF-model-syntax

If you already have the git repository cloned then from the repo sub-tree:

    git fetch origin

We want to be sure we have the latest changes:

    git checkout -b integration origin/integration
    git pull origin integration

Finally check that we have no uncommitted local changes, i.e:

    git status

outputs:

    On branch integration
    Your branch is up-to-date with 'upstream/integration'.

    nothing to commit, working directory clean

###Bumping minor
We bump the `MINOR` version on integration from `5` to `6` using Maven. The Model Syntax project is [Tycho](https://eclipse.org/tycho/) based to produce an Eclipse plug-in. This means we need to use the maven Tycho plugin to set the version:

    cd org.verapdf.releng/
    mvn tycho-versions:set-version -DnewVersion=0.6.0

Now check this has worked so:

    git status

checks if we've changed anything, there should be some POM files and MANIFEST files that are altered, e.g:

    On branch integration
    Your branch is up-to-date with 'origin/integration'.

    Changes not staged for commit:
    (use "git add <file>..." to update what will be committed)
    (use "git checkout -- <file>..." to discard changes in working directory)

          modified:   pom.xml
          modified:   ../org.verapdf.sdk/feature.xml
          modified:   ../org.verapdf.sdk/pom.xml
          modified:   ../org.verapdf.tests/META-INF/MANIFEST.MF
          modified:   ../org.verapdf.tests/pom.xml
          modified:   ../org.verapdf.ui/META-INF/MANIFEST.MF
          modified:   ../org.verapdf.ui/pom.xml
          modified:   ../org.verapdf.updatesite/pom.xml
          modified:   ../org.verapdf/META-INF/MANIFEST.MF
          modified:   ../org.verapdf/pom.xml

          no changes added to commit (use "git add" and/or "git commit -a")

or issuing:

    cat pom.xml | grep version

will show the version numbers in the POM.
