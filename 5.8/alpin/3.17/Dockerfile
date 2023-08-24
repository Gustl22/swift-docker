FROM python:3.11.4-alpine3.17
LABEL maintainer="Swift Infrastructure <swift-infrastructure@forums.swift.org>"
LABEL description="Docker Container for the Swift programming language"

#RUN export DEBIAN_FRONTEND=noninteractive DEBCONF_NONINTERACTIVE_SEEN=true && apt-get -q update && \
#    apt-get -q install -y \
#    binutils \
#    git \
#    unzip \
#    gnupg2 \
#    libc6-dev \
#    libcurl4-openssl-dev \
#    libedit2 \
#    libgcc-11-dev \
#    libpython3-dev \
#    libsqlite3-0 \
#    libstdc++-11-dev \
#    libxml2-dev \
#    libz3-dev \
#    pkg-config \
#    python3-lldb-13 \
#    tzdata \
#    zlib1g-dev \
#    && rm -r /var/lib/apt/lists/*

# Everything up to here should cache nicely between Swift versions, assuming dev dependencies change little

ARG GLIBC_VERSION=2.34-r0
ARG SWIFT_PLATFORM=alpine3.17
ARG SWIFT_BRANCH=swift-5.8.1-release
ARG SWIFT_VERSION=swift-5.8.1-RELEASE

ENV SWIFT_SIGNING_KEY=$SWIFT_SIGNING_KEY \
    SWIFT_PLATFORM=$SWIFT_PLATFORM \
    SWIFT_BRANCH=$SWIFT_BRANCH \
    SWIFT_VERSION=$SWIFT_VERSION \
    SWIFT_WEBROOT=$SWIFT_WEBROOT

RUN wget -q -O /etc/apk/keys/sgerrand.rsa.pub https://alpine-pkgs.sgerrand.com/sgerrand.rsa.pub &&\
    wget https://github.com/sgerrand/alpine-pkg-glibc/releases/download/$GLIBC_VERSION/glibc-$GLIBC_VERSION.apk &&\
    apk add --no-cache --force-overwrite glibc-$GLIBC_VERSION.apk

RUN apk add --no-cache libuuid git bash
RUN apk add --no-cache clang cmake sccache ninja cmark

RUN git clone https://github.com/apple/swift.git swift
RUN cd swift && utils/update-checkout --clone

RUN wget https://github.com/sgerrand/alpine-pkg-glibc/releases/download/$GLIBC_VERSION/glibc-bin-$GLIBC_VERSION.apk &&\
    apk --no-cache --force-overwrite add glibc-bin-$GLIBC_VERSION.apk

RUN export PATH="/usr/glibc-compat/bin:${PATH}"
RUN apk add binutils musl-dev gcc

# install on linux:
# LDFLAGS="-Xlinker -z -Xlinker nostart-stop-gc" CC=/usr/bin/clang CXX=/usr/bin/clang++ ./swift/utils/build-script --release-debuginfo --sccache --llvm-max-parallel-lto-link-jobs 1 --llvm-cmake-options='-DLLVM_ENABLE_LLD=' --extra-cmake-options="-DSWIFT_USE_LINKER= -DLLVM_USE_LINKER=/home/ksmiley/.bin/ld.lld" --reconfigure --bootstrapping off --skip-early-swift-driver --skip-early-swiftsyntax
# LDFLAGS="-Xlinker -z -Xlinker nostart-stop-gc" CC=/usr/bin/clang CXX=/usr/bin/clang++ ./swift/utils/build-script --release-debuginfo --sccache --llvm-max-parallel-lto-link-jobs 1 --reconfigure --bootstrapping off --skip-early-swift-driver --skip-early-swiftsyntax

# Print Installed Swift Version
#RUN swift --version
ENTRYPOINT ["/bin/bash"]
