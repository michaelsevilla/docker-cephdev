Ceph Builder Base
=================

Provides all the dependencies for building Ceph and putting all the binaries in
a Docker image.

# Quickstart

To compile Ceph and build a Docker daemon image:

1. Customize the Docker run command:
    ```bash
    wget https://raw.githubusercontent.com/ivotron/docker-cephdev/master/aliases.sh
    . aliases.sh
    ```

2. Compile Ceph and build the Docker image with the new binaries:

    ```bash
    mkdir ceph; cd ceph
    dmake \
      -e GIT_URL="https://github.com/ceph/ceph.git" \
      -e SHA1_OR_REF="remotes/origin/jewel" \
      -e RECONFIGURE="true" \
      -e BUILD_THREADS=`grep processor /proc/cpuinfo | wc -l`
      ceph-builder/mantle:jewel \
      build-cmake
    ```

This tells the builder to pull the source code from '`GIT_URL`', checkout
branch '`SHA1_OR_REF`', and reconfigure the source code. The `RECONFIGURE` flag
differs depending on the selection of `cmake` or `make` (e.g., for `make` this
will do `./autogen.sh` and `./configure`). The '`BUILD_THREADS`' environment
variable sets the number of cores to use during the compilation; in the example
above, we use all available cores.

By default, the Ceph source code is saved in the `ceph` directory created in
Step 2. 

# Modify Source Code and Rebuild

This framework is meant to accommodate Ceph developers -- so making changes and
re-building Ceph is easy. Assuming you are in the `ceph` directory created
above:

1. Change permissions since the Docker container runs as the `root` user:

    ```bash
    sudo chown -R ${USER}:${USER} .
    ```

2. Make a change; for example:

    ```diff
    diff --git a/src/common/version.cc b/src/common/version.cc
    index 0ca569e..33232e4 100644
    --- a/src/common/version.cc
    +++ b/src/common/version.cc
    @@ -35,7 +35,7 @@ const char *git_version_to_str(void)
     std::string const pretty_version_to_str(void)
     {
       std::ostringstream oss;
    -  oss << "ceph version " << CEPH_GIT_NICE_VER << " ("
    +  oss << "CUSTOMIZED ceph version " << CEPH_GIT_NICE_VER << " ("
           << STRINGIFY(CEPH_GIT_VER) << ")";
       return oss.str();
     }
    ```

3. Recompile Ceph and build a new image with a custome name:

    ```bash
    dmake -e ceph-builder/base:jewel build-cmake
    ```

4. Verify that the new image got built:

   ```bash
   $ docker run --entrypoint=ceph-fuse ceph-heads/remotes/origin/jewel --version
   CUSTOMIZED ceph version v10.2.1-39-g954af78 (954af787526a77b923fe85ed1282ba98277738e4)
   ```

# References

The [`scripts`](scripts) folder contains convenience bash scripts (inspired by
the [`autobuild-ceph`](https://github.com/ceph/autobuild-ceph) repo) to ceph in
multiple ways.

EOF
