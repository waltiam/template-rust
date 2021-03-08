FROM rust:1

# [Option] Install zsh
ARG INSTALL_ZSH="true"
# [Option] Upgrade OS packages to their latest versions
ARG UPGRADE_PACKAGES="false"

# Install needed packages and setup non-root user. Use a separate RUN statement to add your own dependencies.
ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID
COPY library-scripts/* /tmp/library-scripts/
RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
    # Remove imagemagick due to https://security-tracker.debian.org/tracker/CVE-2019-10131
    && apt-get purge -y imagemagick imagemagick-6-common \
    # Install common packages, non-root user, updated lldb, dependencies
    && bash /tmp/library-scripts/common-debian.sh "${INSTALL_ZSH}" "${USERNAME}" "${USER_UID}" "${USER_GID}" "${UPGRADE_PACKAGES}" \
    && bash /tmp/library-scripts/rust-debian.sh "${CARGO_HOME}" "${RUSTUP_HOME}" "${USERNAME}" "true" "true" 
    
# Install my tooling
ARG INSTALL_NEOVIM="true"
COPY library-scripts/init.vim \
    library-scripts/install-neovim.sh \
    /tmp/library-scripts/
# RUN  if [ "$INSTALL_NEOVIM" = "true"]; then bash /tmp/library-scripts/install-neovim.sh; fi 
RUN   bash /tmp/library-scripts/install-neovim.sh s

# [Optional] Uncomment this section to install additional OS packages.
# RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
#     && apt-get -y install --no-install-recommends <your-package-list-here>
