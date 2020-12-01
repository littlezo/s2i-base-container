# This image is the core image for all s2i configurable container images.
FROM centos:7

ENV SUMMARY="CentOS Core Image which allows using of source-to-image."	\
    DESCRIPTION="The s2i-core image provides any images layered on top of it with all the tools needed to use source-to-image functionality while keeping the image size as small as possible."

ARG BUILD_DATE

LABEL summary="$SUMMARY" \
    description="$DESCRIPTION" \
    io.k8s.description="$DESCRIPTION" \
    io.k8s.display-name="s2i core image" \
    io.kubesphere.s2i.scripts-url=image:///usr/libexec/s2i \
    io.openshift.s2i.scripts-url=image:///usr/libexec/s2i \
    io.s2i.scripts-url=image:///usr/libexec/s2i \
    com.label-schema.maintainer-url="https://github.com/littlezo" \
    com.cloudify.component="s2i-core-container" \
    name="littleof/s2i-core-centos7" \
    version="1.0" \
    maintainer="@小小只^v^ <littlezov@qq.com>"\
    io.label-schema.license="GPL v2.0" \
    io.label-schema.build-date="${BUILD_DATE}"

ENV \
    # DEPRECATED: Use above LABEL instead, because this will be removed in future versions.
    STI_SCRIPTS_URL=image:///usr/libexec/s2i \
    # Path to be used in other layers to place s2i scripts into
    STI_SCRIPTS_PATH=/usr/libexec/s2i \
    APP_ROOT=/app-root \
    # The $HOME is not set by default, but some applications needs this variable
    HOME=/app-root/src \
    PATH=/app-root/src/bin:/app-root/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin \
    PLATFORM=el7


# When bash is started non-interactively, to run a shell script, for example it
# looks for this variable and source the content of this file. This will enable
# the SCL for all scripts without need to do 'scl enable'.
ENV BASH_ENV=${APP_ROOT}/etc/scl_enable \
    ENV=${APP_ROOT}/etc/scl_enable \
    PROMPT_COMMAND=". ${APP_ROOT}/etc/scl_enable"

# This is the list of basic dependencies that all language container image can
# consume.
# Also setup the 'kubesphere' user that is used for the build execution and for the
# application runtime execution.
# TODO: Use better UID and GID values
RUN rpmkeys --import file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7 && \
    INSTALL_PKGS="bsdtar \
    findutils \
    gettext \
    groff-base \
    rsync \
    scl-utils \
    tar \
    unzip \
    xz \
    yum-utils" && \
    mkdir -p ${HOME}/.pki/nssdb && \
    chown -R 1001:0 ${HOME}/.pki && \
    yum install -y --setopt=tsflags=nodocs $INSTALL_PKGS && \
    rpm -V $INSTALL_PKGS && \
    yum -y clean all --enablerepo='*'

# Copy extra files to the image.
COPY ./root/ /

# Directory with the sources is set as the working directory so all STI scripts
# can execute relative to this path.
WORKDIR ${HOME}

ENTRYPOINT ["container-entrypoint"]
CMD ["base-usage"]

# Reset permissions of modified directories and add default user
RUN rpm-file-permissions && \
    useradd -u 1001 -r -g 0 -d ${HOME} -s /sbin/nologin \
    -c "Default Application User" default && \
    chown -R 1001:0 ${APP_ROOT}