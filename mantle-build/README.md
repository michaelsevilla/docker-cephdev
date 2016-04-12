Provides all the dependencies for building Mantle. Assuming the source 
tree for ceph is in the host machine at `/path/to/mantlesrc`, the 
following will build Mantle:

```bash
docker run --rm \
  --name mantle-build \
  -v /tmp/ceph:/ceph/ \
  -e BUILD_THREADS=24 \
  -e GIT_URL=https://github.com/michaelsevilla/ceph.git \
  -e SHA1_OR_REF=remotes/origin/cls-lua-mantle-master \
  michaelsevilla/mantle-build
```

Then you can build the package:
 
```bash
docker run --rm \
  --name mantle-pkg \
  -v /tmp/ceph:/ceph \
  -e BUILD_THREADS=24 \
  -e PKG_ARGS="-b -nc -us -uc" \
  michaelsevilla/mantle-build
```

# References:

This is identical to [cephdev-build][cephdev-build], except that we explicitly
add the proper Lua libraries and linker flags for the Ceph configure.

[cephdev-build]: https://github.com/ivotron/docker-cephdev/tree/master/build

