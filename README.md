The Vanagon Project
===
 * What is vanagon?
 * Runtime requirements
 * Configuration and Usage
 * Overview
 * License
 * Maintainers

What is vanagon?
---
Vanagon is a tool to build a single package out of a project, which can itself
contain one or more components. This tooling is being used to develop the
puppet-agent package, which contains components such as openssl, ruby, and
augeas among others. For a simple example, please see the examples directory.

Vanagon builds up a Makefile and packaging files (specfile for RPM,
control/rules/etc for DEB) and copies them to a remote host, where make can be
invoked to build all of the components and make a package of the contents.

Runtime Requirements
---
Vanagon is self-contained. A recent version of ruby should be all that is
required. Beyond that, ssh, rsync and git are also required on the host, and
ssh-server and rsync is required on the target (package installation for the
target can be customized in the platform config for the target).

Configuration and Usage
---
Vanagon won't be much use without a project to build. Beyond that, you must
define any platforms you want to build for. Vanagon ships with some simple
binaries to use, but the one you probably care about is named 'build'.

### `build` usage

The build command has positional arguments and position independent flags.

#### Arguments (position dependent)

##### project name
The name of the project to build, and a file named \<project\_name\>.rb must be
present in configs/projects in the working directory.

##### platform name
The name of the platform to build against, and a file named
\<platform\_name\>.rb must be present in configs/platforms in the working
directory.

##### target host [optional]
Target host is an optional argument to override the host selection. Instead of using
a vm collected from the pooler, the build will attempt to ssh to target as the
root user.

#### Flags (can be anywhere in the command)

##### --preserve
Indicates that the host used for building the project should be left intact
after the build instead of destroyed. The host is usually destroyed after a
successful build, or left after a failed build.

##### -v, --verbose (not yet implemented)
Increase verbosity of output.

#### Environment variables

##### VANAGON\_SSH\_KEY
A full path on disk for a private ssh key to be used in ssh and rsync
communications. This will be used instead of whatever defaults are configured
in .ssh/config.

#### Example usage
`build --preserve puppet-agent el-6-i386` will build the puppet-agent project
on the el-6-i386 platform and leave the host intact afterward.

License
---
See [LICENSE](LICENSE) file.

Overview
---
Vanagon is broken down into three core ideas: the project, the component and
the platform. The project contains one or more components and is built for a
platform. As a quick example, if I had a ruby app and wanted to package it, the
project would probably contain a component for ruby and a component for my app.
If I wanted to build it for debian wheezy, I would define a platform called
wheezy and build my project against it.

For more detailed examples of the DSLs available, please see the
[examples](examples) directory and the YARD documentation for vanagon.

Maintainers
---
The Release Engineering team at Puppet Labs
