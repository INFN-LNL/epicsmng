ARG OS_VERSION=10

FROM almalinux:${OS_VERSION}-minimal AS install-epicsmng

ARG USERNAME=epics
ARG USER_UID=1000
ARG USER_GID=${USER_UID}

ENV HOME=/home/${USERNAME} \
    PATH=${PATH}:/home/${USERNAME}/.local/bin \
    USERNAME=${USERNAME}

# Create a user with the given name, UID and GID 
# Add the user to the sudoers, to allow to execute commands 
# requiring sudoers permissions (such as apt install).
RUN microdnf -y install shadow-utils sudo \
    && groupadd --gid ${USER_GID} ${USERNAME} \
    && useradd --uid ${USER_UID} --gid ${USER_GID} -m ${USERNAME} \
    && microdnf clean all \
    && rm -rf /var/cache/dnf \
    && echo "${USERNAME} ALL=(root) NOPASSWD:ALL" > /etc/sudoers.d/${USERNAME} \
    && chmod 0440 /etc/sudoers.d/${USERNAME}

# Copy epicsmng code from repository
COPY ./utils /epicsmng/utils
COPY ./epicsmng ./install.sh /epicsmng/

# Install, clean source files and set the github mirror of the base to be used (faster)
# Install required packets to build epics base and asyn
RUN sh /epicsmng/install.sh \
    && rm -rf /epicsmng \
    && install -D <(echo "base_url=\"https://github.com/epics-base/epics-base\"") ${HOME}/.config/epicsmng/settings/user.settings \
    && microdnf -y install git make perl readline-devel rpcgen libtirpc-devel

WORKDIR ${HOME}

EXPOSE 5064

USER epics
