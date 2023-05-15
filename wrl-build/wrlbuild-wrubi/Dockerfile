FROM windriver/wrlx-image:full-x86-64-lts22-10.22.33.5

RUN \
    echo "multilib_policy=best" >> /etc/dnf/dnf.conf && \
    echo "deltarpm=0" >> /etc/dnf/dnf.conf && \
    dnf -y --noplugins install \
    packagegroup-core-buildessential \
    ldconfig \
    curl \
    diffstat \
    ldd \
    libcrypt-dev \
    libc6-utils \
    libgcc-s-dev \
    libgomp \
    libgomp-dev \
    glibc-binary-localedata-en-us \
    glibc-localedata-en-us \
    git \
    perl \
    perl-misc \
    perl-module-* \
    python3 \
    python3-pip \
    python3-pexpect \
    python3-pyyaml \
    rsync \
    tini \
    util-linux-getopt \
    xz && \
    dnf -y --noplugins clean all && \
    rm -rf /etc/ld.so.cache && ldconfig && \
    rm -rf /var/cache/dnf && \
    rm -rf /usr/share/{man,doc,info,gnome/help} && \
    rm -rf /usr/share/texlive/texmf-dist/{fonts,doc,tex} && \
    cd /usr/libexec/git-core && \
    find . -samefile git -name 'git-*' -exec ln -sf git {} \; && \
    cd / && git config --global --add sendemail.suppresscc all && \
    curl -L -o /usr/bin/mc https://dl.min.io/client/mc/release/linux-amd64/mc && \
    chmod +x /usr/bin/mc && \
    rm -f /lib/depmod.d/exclude.conf && \
    useradd --home-dir /home/wrlbuild --uid 1000 --gid 100 --shell /bin/bash wrlbuild

ENTRYPOINT ["/usr/bin/docker-init"]

USER wrlbuild

CMD ["/bin/bash"]
