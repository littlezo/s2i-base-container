KubeSphere base images
========================================

This repository contains Dockerfiles for images which serve as base images with all the
essential libraries and tools needed for KubeSphere language images, for example:

* [s2i-nodejs](https://github.com/littlezo/s2i-nodejs-container)
* [s2i-php](https://github.com/littlezo/s2i-php-container)

This container image also installs several development libraries,

Sharing those development packages in a common layer saves disk space and
improves pulling speed.

In order to offer good experience for web developers, the NPM package manager
is also installed in the image.

For containers where the development libraries and NPM package manager are not
necessary, users are advised to use the s2i-core variant of this container image.


Description
-----------

Normally, SCL requires manual operation to enable the collection you want to use.
This is burdensome and can be prone to error.
The KubeSphere S2I approach is to set Bash environment variables that
serve to automatically enable the desired collection:

* `BASH_ENV`: enables the collection for all non-interactive Bash sessions
* `ENV`: enables the collection for all invocations of `/bin/sh`
* `PROMPT_COMMAND`: enables the collection in interactive shell

Two examples:
* If you specify `BASH_ENV`, then all your `#!/bin/bash` scripts
do not need to call `scl enable`.
* If you specify `PROMPT_COMMAND`, then on execution of the
`docker exec ... /bin/bash` command, the collection will be automatically enabled.

*Note*:
Executables in Software Collections packages (e.g., `php`)
are not directly in a directory named in the `PATH` environment variable.
This means that you cannot do:

    $ docker exec <cid> ... php

but must instead do:

    $ docker exec <cid> ... /bin/bash -c php

The `/bin/bash -c`, along with the setting the appropriate environment variable,
ensures the correct `php` executable is found and invoked.


Usage
------------------------

*  **image**

This image is available on DockerHub. To download it run:

```console
docker pull littlezo/s2i-base-centos8
```

To build a Base image from scratch run:

```
$ git clone --recursive https://github.com/littlezo/s2i-base-container.git
$ cd s2i-base-container
$ make build VERSIONS=base
```

**Notice: By omitting the `VERSION` parameter, the build/test action will be performed
on all provided versions of s2i image.**


See also
--------
Dockerfile and other sources are available on https://github.com/littlezo/s2i-base-container.
In that repository you also can find another variants of S2I base Dockerfiles.
Dockerfile for CentOS7 is called Dockerfile, 
Dockerfile for CentOS8 is called Dockerfile, 
Dockerfile for RHEL7 is called Dockerfile.rhel7.
Dockerfile for RHEL8 is called Dockerfile.rhel8.
