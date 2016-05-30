Ceph Builder Mantle
===================

Provides all the dependencies for building Mantle and putting all the binaries
in a Docker image. This is identical to [builder-base](../builder-base), except
that we explicitly add the proper Lua libraries and linker flags for the Ceph
configuration.

# Quickstart

To compile Mantle and build a Docker daemon image:

1. Customize the Docker run command:

    ```bash
    wget https://raw.githubusercontent.com/ivotron/docker-cephdev/master/aliases.sh
    . aliases.sh
    ```

2. Compile Ceph and build the Docker image with the new binaries:

    ```bash
    mkdir ceph; cd ceph
    dmake \
      -e GIT_URL="https://github.com/michaelsevilla/ceph.git" \
      -e SHA1_OR_REF="remotes/origin/cls-lua-mantle-jewel" \
      -e RECONFIGURE="true" \
      -e BUILD_THREADS=`grep proc /proc/cpuinfo | wc -l` \
      michaelsevilla/builder-mantle:jewel \
      build-make
    ```

3. Make some changes to the source code, then recompile and build a new image:

    ```bash
    dmake ceph-builder/mantle:jewel build-make
    ```

For a description of these commands and for directions on making changes to the
source code and recompiling, please see the [Ceph Base Builder
README](../builder-base/README.md).

EOF
