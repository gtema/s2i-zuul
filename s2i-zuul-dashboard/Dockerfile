# s2i-zuul-zookeeper
FROM registry.fedoraproject.org/f28/s2i-base

LABEL maintainer="Artem Goncharov <artem.goncharov@gmail.com>"

ENV BUILDER_VERSION 0.1

ENV PATH=$HOME/.local/bin/:$PATH \
    LC_ALL=en_US.UTF-8 \
    LANG=en_US.UTF-8

LABEL io.k8s.description="Platform for building Zuul Dashboard" \
      io.k8s.display-name="Zuul Dashboard builder F28" \
      io.openshift.expose-services="8080:tcp" \
      io.openshift.tags="builder,zuul,dashboard,0.1.0"

RUN curl --silent --location https://dl.yarnpkg.com/rpm/yarn.repo | tee /etc/yum.repos.d/yarn.repo

RUN dnf install -y npm tree yarn lighttpd -v && dnf clean all -y --enablerepo='*'

# TODO (optional): Copy the builder files into /opt/app-root
# COPY ./<builder_folder>/ /opt/app-root/

# TODO: Copy the S2I scripts to $STI_SCRIPTS_PATH, since s2i-base image
# sets io.openshift.s2i.scripts-url label that way, or update that label
COPY ./s2i/bin/ $STI_SCRIPTS_PATH

# Copy extra files to the image.
COPY ./etc/ ${APP_ROOT}/etc

# - In order to drop the root user, we have to make some directories world
#   writable as OpenShift default security model is to run the container
#   under random UID.
RUN mkdir -m 0755 ${APP_ROOT}/static_html && \
    chown -R 1001:0 ${APP_ROOT} && \
    fix-permissions ${APP_ROOT} -P

# This default user is created in the s2i-base image
USER 1001

# TODO: Set the default port for applications built using this image
EXPOSE 8080

# Set the default CMD to print the usage of the language image.
CMD $STI_SCRIPTS_PATH/usage
