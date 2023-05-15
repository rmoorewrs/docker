FROM golang:1.18
RUN \
    apt-get update && \
    apt-get install -y libgpgme-dev libassuan-dev libbtrfs-dev libdevmapper-dev pkg-config && \
    git clone https://github.com/opencontainers/umoci.git /go/src/umoci && \
    cd /go/src/umoci && make umoci.static && \
    git clone https://github.com/containers/skopeo /go/src/skopeo && \
    cd /go/src/skopeo && \
    CGO_ENABLE=0 GO111MODULE=on GOOS=linux GOARCH=amd64 go build -mod=vendor '-buildmode=pie' -ldflags '-extldflags -static' -gcflags '' -tags 'exclude_graphdriver_devicemapper exclude_graphdriver_btrfs containers_image_openpgp' -o ./bin/skopeo-linux-amd64 ./cmd/skopeo

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
    #skopeo \
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

COPY --from=0 /go/src/umoci/umoci.static /usr/bin/umoci

# Skopeo binary can be downloaded directly, but v1.10.0 has CVE-2022-39237, so not use it for now
#RUN wget --progress dot:giga https://github.com/lework/skopeo-binary/releases/download/v1.10.0/skopeo-linux-amd64 -O /usr/bin/skopeo && \
#    chmod 755 /usr/bin/skopeo && \
#    /usr/bin/skopeo --version
COPY --from=0 /go/src/skopeo/bin/skopeo-linux-amd64 /usr/bin/skopeo

ENTRYPOINT ["/usr/bin/docker-init"]

USER wrlbuild

CMD ["/bin/bash"]
