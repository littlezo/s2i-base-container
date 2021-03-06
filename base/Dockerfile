# This image is the base image for all KubeSphere v3 language container images.
FROM littleof/s2i-core-centos7

ENV SUMMARY="CentOS Base Image which allows using of source-to-image."	\
    DESCRIPTION="The s2i-base image provides any images layered on top of it with all the tools needed to use source-to-image functionality while keeping the image size as small as possible."

ARG BUILD_DATE

LABEL summary="$SUMMARY" \
    description="$DESCRIPTION" \
    io.k8s.description="$DESCRIPTION" \
    io.k8s.display-name="s2i base image" \
    io.kubesphere.s2i.scripts-url=image:///usr/libexec/s2i \
    io.openshift.s2i.scripts-url=image:///usr/libexec/s2i \
    io.s2i.scripts-url=image:///usr/libexec/s2i \
    com.label-schema.maintainer-url="https://github.com/littlezo" \
    com.cloudify.component="s2i-base-container" \
    name="littleof/s2i-base-centos7" \
    version="1.0" \
    maintainer="@小小只^v^ <littlezov@qq.com>"\
    io.label-schema.license="GPL v2.0" \
    io.label-schema.build-date="${BUILD_DATE}"

# This is the list of basic dependencies that all language container image can
# consume.

RUN yum install -y centos-release-scl && \
    yum update -y --setopt=tsflags=nodocs  && \
    INSTALL_PKGS="autoconf \
    automake \
    bzip2  \
    gcc-c++ \
    gd-devel \
    gdb \
    git \
    libcurl-devel \
    libpq-devel  \
    libxml2-devel  \
    libxslt-devel \
    lsof \
    make \
    openssl-devel \
    patch \
    procps-ng  \
    redhat-rpm-config \
    unzip \
    wget \
    which \
    openssl \
    supervisor \
    mysql-devel \
    zlib-devel" && \
    yum install -y --setopt=tsflags=nodocs $INSTALL_PKGS --nogpgcheck && \
    rm -rf /var/cache/yum/* && \
    yum repolist && \
    yum -y clean all --enablerepo='*'
RUN rpm-file-permissions