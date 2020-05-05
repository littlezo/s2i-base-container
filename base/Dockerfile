# This image is the base image for all KubeSphere v3 language container images.
FROM littlezo/s2i-core-centos7

ENV SUMMARY="Base image with essential libraries and tools used as a base for \
builder images like perl, python, ruby, etc." \
    DESCRIPTION="The s2i-base image, being built upon s2i-core, provides any \
images layered on top of it with all the tools needed to use source-to-image \
functionality. Additionally, s2i-base also contains various libraries needed for \
it to serve as a base for other builder images, like s2i-python or s2i-ruby."

LABEL summary="$SUMMARY" \
      description="$DESCRIPTION" \
      io.k8s.description="$DESCRIPTION" \
      io.k8s.display-name="s2i base" \
      com.cloudify.component="s2i-base-container" \
      name="littlezo/s2i-base-centos7" \
      version="1" \
      maintainer="@小小只 <little@qq.com>"

# This is the list of basic dependencies that all language container image can
# consume.
RUN yum install -y centos-release-scl && \
  yum update -y --setopt=tsflags=nodocs  && \
  INSTALL_PKGS="autoconf automake bzip2 gcc-c++ gd-devel gdb git libcurl-devel libxml2-devel libxslt-devel lsof make openssl-devel patch unzip wget which openssl supervisor zlib-devel"  && \
  yum install -y --setopt=tsflags=nodocs $INSTALL_PKGS --nogpgcheck  && \
  rm -rf /var/cache/yum/* && \
  yum repolist && \
  yum -y clean all --enablerepo='*'
RUN rpm-file-permissions