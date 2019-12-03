FROM openshift/jenkins-slave-base-centos7

# Labels consumed by Red Hat build service
LABEL com.redhat.component="rh-dotnet30-jenkins-slave-docker" \
      name="dotnet/dotnet-30-jenkins-slave-centos7" \
      version="3.0" \
      architecture="x86_64" \
      release="1" \
      io.k8s.display-name="Jenkins Slave .NET Core 3.0" \
      io.k8s.description="The jenkins slave dotnet image has the dotnet tools on top of the jenkins slave base image." \
      io.openshift.tags="openshift,jenkins,slave,dotnet,dotnet30"

# Don't download/extract docs for nuget packages
# Enable nodejs and dotnet scl
ENV DOTNET_CORE_VERSION=3.0 \
    BASH_ENV=/usr/local/bin/scl_enable \
    ENV=/usr/local/bin/scl_enable \
    PROMPT_COMMAND=". /usr/local/bin/scl_enable" \
    ENABLED_COLLECTIONS="rh-nodejs10 rh-dotnet30" \
    NUGET_XMLDOC_MODE=skip

COPY contrib/bin/scl_enable /usr/local/bin/scl_enable

# Install
RUN yum install -y centos-release-dotnet centos-release-scl-rh && \
    INSTALL_PKGS="rh-dotnet30 rh-nodejs10-npm" && \
    yum install -y --setopt=tsflags=nodocs $INSTALL_PKGS && \
    rpm -V $INSTALL_PKGS && \
    yum clean all -y && \
# yum cache files may still exist (and quite large in size)
    rm -rf /var/cache/yum/*

# Trigger first time actions.
RUN scl enable rh-dotnet30 'dotnet help'

RUN chown -R 1001:0 $HOME && \
    chmod -R g+rw $HOME

USER 1001