# Release Script

This is intended to be a realease generating script to be used in other
projects.  Although, it's used to generate it's own releases too.

## git repository setup.

No specific requirements, however, you can use git config to avoid having to
repeatedly pass command-line options.

### release-script.project

```git config release-script.project name```

This will set the project name (--project on CLI).  You only need this if the
archive name should differ from the path to which the repository has been
checked out to.

For example, if I wanted to release release-script as create-release then I could do:

```git config release-script.project create-release```

And the create-release script would pick up the name from there.

### release-script.archive

You can also configure the default list of artifacts to create, for example, if
you want .tar.xz and .zip:

```git config release-script.archive "tar.xz zip"```

This avoids having to pass --archive multiple times on the command line.

### release-script.upstream

The upstream repository to which to push the tags prior to invoking the gh
command to create the release defaults to origin (or --repo via CLI).  Normally
create-release will invoke git push origin v${version}, but this allows you to
override the target repository.

## Usage

If the above has been setup, then in most cases with the target version checked
out (and with create-release in PATH), simply run:

```
create-release 1.0
```

Where 1.0 is the desired version number (a check is performed that it looks
like a typical version number, but it really can be anything.

The following actions will be performed on a git-archive extracted version of
the code:

1. If autogen.sh exists, it will be run.

### --archive

Normally create-release will only create a .tar.gz release archive, unless
other mechanisms have been set using git-config release-script.archive.  Using
this option (which can be specified multiple times) will use an alternative set
of archives.

### --project

Normally create-release will use release-script.project as the release name,
otherwise look at the folder name where the project has been checked out.  This
allows you to set an alternative name.  This only really affects the archive
folder and name.

### --repo

If the current working directory isn't the project you'd like to operate on,
specify the path to any folder in the repository using this option.

Release artifacts will always be created in the top folder of the repository.

### --skipgithub

Push the tag, create the artifacts, but don't actually generate the release on
github.  Useful for testing, or if you're not actually using github.

## Tips

### Excluding development files from release

You can add file path patterns to .gitattributes to have git archive exclude
those files.  For example:

```
.git* export-ignore
```
