ARG OS_VERSION=10

FROM almalinux:${OS_VERSION}-minimal AS install-epicsmng

ARG HOME=/root
ENV HOME=${HOME}
ENV PATH=${PATH}:${HOME}/.local/bin

# Copy epicsmng code from repository
COPY ./utils /epicsmng/utils
COPY ./epicsmng ./install.sh /epicsmng/

# Install, clean source files and set the github mirror of the base to be used (faster)
# Install required packets to build epics base and asyn
RUN sh /epicsmng/install.sh \
    && rm -rf /epicsmng \
    && install -D <(echo "base_url=\"https://github.com/epics-base/epics-base\"") \
        ${HOME}/.config/epicsmng/settings/user.settings \
    && microdnf --enablerepo=crb -y install \
        git \
        make \
        gcc \
        g++ \
        perl-FindBin \
        perl-File-Copy \
        perl-ExtUtils-Install \
        readline-devel \
        rpcgen \
        libtirpc-devel \
    && microdnf clean all \
    && rm -rf /var/cache/dnf

EXPOSE 5064/tcp
EXPOSE 5064/udp
EXPOSE 5065/udp
