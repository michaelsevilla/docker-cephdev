# Ceph Development in Docker

Generate Docker images with customized versions of Ceph. The general 
procedure is the following:

 1. Build the Ceph code base using any builder image.
 2. Pack a new `ceph-daemon` image.
 3. Deploy the image using the official 
    [`ceph-ansible`](https://github.com/ceph/ceph-ansible) playbooks.

# Quickstart

Every builder image is in a `builder-*` folder. By convention, a 
builder image generates an install directory on the host that is later 
used to generate an ansible-deployable image (see below). For examples 
see the README for the builder that you want to use:

 * [vanilla Ceph](builder-base)
 * [Mantle](builder-mantle)
 * [zlog](builder-zlog)

For information about our rationale or motivation, see the
[wiki](https://github.com/ivotron/docker-cephdev/wiki/).

EOF
