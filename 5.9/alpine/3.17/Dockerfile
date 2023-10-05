FROM python:3.11.4-alpine3.17
LABEL maintainer="Swift Infrastructure <swift-infrastructure@forums.swift.org>"
LABEL description="Docker Container for the Swift programming language"

RUN apk add --no-cache \
    bash \
    binutils \
    build-base \
    clang clang-dev \
    cmake \
    cmark \
    gcc \
    g++ \
    git \
    libgcc \
    libstdc++ \
    libuuid \
    musl-dev \
    ninja \
    sccache

# Everything up to here should cache nicely between Swift versions, assuming dev dependencies change little

#ARG GLIBC_VERSION=2.34-r0
ARG SWIFT_VERSION=swift-5.9-DEVELOPMENT-SNAPSHOT-2023-10-02-a

#ENV GLIBC_VERSION=$GLIBC_VERSION
ENV SWIFT_VERSION=$SWIFT_VERSION

#RUN wget -q -O /etc/apk/keys/sgerrand.rsa.pub https://alpine-pkgs.sgerrand.com/sgerrand.rsa.pub &&\
#    wget https://github.com/sgerrand/alpine-pkg-glibc/releases/download/$GLIBC_VERSION/glibc-$GLIBC_VERSION.apk &&\
#    apk add --no-cache --force-overwrite glibc-$GLIBC_VERSION.apk && rm glibc-$GLIBC_VERSION.apk
#RUN wget https://github.com/sgerrand/alpine-pkg-glibc/releases/download/$GLIBC_VERSION/glibc-bin-$GLIBC_VERSION.apk &&\
#    apk --no-cache --force-overwrite add glibc-bin-$GLIBC_VERSION.apk && rm glibc-bin-$GLIBC_VERSION.apk

RUN git clone https://github.com/Gustl22/swift.git swift -b "maxd/alpine-build-runtime"
RUN cd swift && utils/update-checkout --clone
#RUN cd swift && utils/update-checkout --tag $SWIFT_VERSION

#RUN export PATH="/usr/glibc-compat/bin:${PATH}"
#RUN apk --no-cache add libgcc

# install on linux:
#RUN LDFLAGS="-Xlinker -z -Xlinker nostart-stop-gc" CC=/usr/bin/clang CXX=/usr/bin/clang++ ./swift/utils/build-script --release-debuginfo --sccache --llvm-max-parallel-lto-link-jobs 1 --llvm-cmake-options='-DLLVM_ENABLE_LLD=' --extra-cmake-options="-DSWIFT_USE_LINKER= -DLLVM_USE_LINKER=/home/ksmiley/.bin/ld.lld" --reconfigure --bootstrapping off --skip-early-swift-driver --skip-early-swiftsyntax
RUN ./swift/utils/build-script --preset=buildbot_incremental_linux_base
#RUN LDFLAGS="-Xlinker -z -Xlinker nostart-stop-gc" CC=/usr/bin/clang CXX=/usr/bin/clang++ ./swift/utils/build-script --release-debuginfo --sccache --llvm-max-parallel-lto-link-jobs 1 --reconfigure --bootstrapping off --skip-early-swift-driver --skip-early-swiftsyntax

# SOME_PATH/swift-project/build/Ninja-RelWithDebInfoAssert/swift-macosx-arm64

# Print Installed Swift Version
#RUN swift --version
