# Building Common API C in Jenkins

## Prerequisites

* A recent version of [Jenkins](https://jenkins-ci.org/) CI/CD installed together with the necessary plugins
  - Tested with https://github.com/gmacario/easy-jenkins
* An Internet browser able to access the Jenkins dashboard at `${JENKINS_URL}`
  - Example: http://192.168.99.100:9080/

## Step-by-step instructions

### Create project `common-api-c`

Browse `${DOCKER_URL}`, then click **New Item**

* Name: `common-api-c`
* Type: **Freestyle project**

then click **OK**. Inside the project configuration page, add the following information:

* Discard Old Builds: Yes
  - Strategy: Log Rotation
    - Days to keep build: (none)
    - Max # of builds to keep: 5
* Source Code Management: Git
  - Repositories
    - Repository URL: `git://git.projects.genivi.org/common-api/c-poc.git`
    - Credentials: - none -
  - Branches to build
    - Branch Specifier (blank for 'any'): `*/master`
  - Repository browser: (Auto)
* Build Environment
  - Build inside a Docker container: Yes
    - Docker image to use: Pull docker image from repository (TODO: Build from Dockerfile)
      - Image id/tag: `ubuntu:16.04`
* Build
  - Execute shell
    - Command
```
# DEBUG
id
ls -la
```
  - Execute shell
    - Command
```
# TODO: Use Dockerfile when merged upstream
apt-get update && apt-get -y dist-upgrade
apt-get -y install autoconf build-essential file git libtool make sudo
apt-get -y install libsystemd-dev pkg-config
# id build 2>/dev/null || useradd -m build
# echo "build ALL=(ALL) NOPASSWD: ALL" | tee -a /etc/sudoers
# su - build
```
  - Execute shell
    - Command
```
# Actual build steps
autoreconf -i
./configure
make
sudo make install
```

then click **Save**.

### Build project `common-api-c`

Browse `${DOCKER_URL}/jobs/common-api-c`, then click **Build Now**

Result: SUCCESS

Excerpt from build console:

```
Started by user anonymous
[EnvInject] - Loading node environment variables.
Building on master in workspace /var/jenkins_home/jobs/common-api-c/workspace
 > git rev-parse --is-inside-work-tree # timeout=10
Fetching changes from the remote Git repository
 > git config remote.origin.url git://git.projects.genivi.org/common-api/c-poc.git # timeout=10
Fetching upstream changes from git://git.projects.genivi.org/common-api/c-poc.git
 > git --version # timeout=10
 > git -c core.askpass=true fetch --tags --progress git://git.projects.genivi.org/common-api/c-poc.git +refs/heads/*:refs/remotes/origin/*
 > git rev-parse refs/remotes/origin/master^{commit} # timeout=10
 > git rev-parse refs/remotes/origin/origin/master^{commit} # timeout=10
Checking out Revision d6ec42ce45c33f40560d1f24b9143e9b1e6816e9 (refs/remotes/origin/master)
 > git config core.sparsecheckout # timeout=10
 > git checkout -f d6ec42ce45c33f40560d1f24b9143e9b1e6816e9
First time build. Skipping changelog.
Docker container ea564b83f235f3032c1d48912e3bde7416971dabc73caf0c23f12e3759373708 started to host the build
$ docker exec --tty ea564b83f235f3032c1d48912e3bde7416971dabc73caf0c23f12e3759373708 env
[workspace] $ docker exec --tty --user 0:0 ea564b83f235f3032c1d48912e3bde7416971dabc73caf0c23f12e3759373708 env affinity:container==e1d192d7db587826ac8fe8aa16904668acfa610aca385d3941c67924ea61154a 
...
+ autoreconf -i
libtoolize: Consider adding 'AC_CONFIG_MACRO_DIRS([m4])' to configure.ac,
libtoolize: and rerunning libtoolize and aclocal.
libtoolize: Consider adding '-I m4' to ACLOCAL_AMFLAGS in Makefile.am.
+ ./configure
checking for a BSD-compatible install... /usr/bin/install -c
...
config.status: executing libtool commands
+ make
make  all-am
make[1]: Entering directory '/var/jenkins_home/jobs/common-api-c/workspace'
make[1]: Leaving directory '/var/jenkins_home/jobs/common-api-c/workspace'
+ sudo make install
make[1]: Entering directory '/var/jenkins_home/jobs/common-api-c/workspace'
 /bin/mkdir -p '/usr/local/lib'
 /bin/bash ./libtool   --mode=install /usr/bin/install -c   libcapic.la '/usr/local/lib'
libtool: install: /usr/bin/install -c .libs/libcapic.so.0.0.0 /usr/local/lib/libcapic.so.0.0.0
libtool: install: (cd /usr/local/lib && { ln -s -f libcapic.so.0.0.0 libcapic.so.0 || { rm -f libcapic.so.0 && ln -s libcapic.so.0.0.0 libcapic.so.0; }; })
libtool: install: (cd /usr/local/lib && { ln -s -f libcapic.so.0.0.0 libcapic.so || { rm -f libcapic.so && ln -s libcapic.so.0.0.0 libcapic.so; }; })
libtool: install: /usr/bin/install -c .libs/libcapic.lai /usr/local/lib/libcapic.la
libtool: finish: PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/sbin" ldconfig -n /usr/local/lib
----------------------------------------------------------------------
Libraries have been installed in:
   /usr/local/lib

If you ever happen to want to link against installed libraries
in a given directory, LIBDIR, you must either use libtool, and
specify the full pathname of the library, or use the '-LLIBDIR'
flag during linking and do at least one of the following:
   - add LIBDIR to the 'LD_LIBRARY_PATH' environment variable
     during execution
   - add LIBDIR to the 'LD_RUN_PATH' environment variable
     during linking
   - use the '-Wl,-rpath -Wl,LIBDIR' linker flag
   - have your system administrator add LIBDIR to '/etc/ld.so.conf'

See any operating system documentation about shared libraries for
more information, such as the ld(1) and ld.so(8) manual pages.
----------------------------------------------------------------------
 /bin/mkdir -p '/usr/local/lib/pkgconfig'
 /usr/bin/install -c -m 644 capic.pc '/usr/local/lib/pkgconfig'
 /bin/mkdir -p '/usr/local/include/capic'
 /usr/bin/install -c -m 644 src/capic/backend.h src/capic/log.h src/capic/dbus-private.h '/usr/local/include/capic'
make[1]: Leaving directory '/var/jenkins_home/jobs/common-api-c/workspace'
Stopping Docker container after build completion
Notifying upstream projects of job completion
Finished: SUCCESS
```

<!-- EOF -->
