FROM ubuntu:22.04
# ubuntu 22.04 is called jammy

# add arch
RUN sed 's/^deb http/deb [arch=amd64] http/' -i '/etc/apt/sources.list'
RUN echo 'deb [arch=armhf] http://ports.ubuntu.com/ubuntu-ports jammy main universe multiverse restricted' >> /etc/apt/sources.list && \
    echo 'deb [arch=armhf] http://ports.ubuntu.com/ubuntu-ports jammy-updates main universe multiverse restricted' >> /etc/apt/sources.list && \
    echo 'deb [arch=armhf] http://ports.ubuntu.com/ubuntu-ports jammy-backports main universe multiverse restricted' >> /etc/apt/sources.list && \
    echo 'deb [arch=armhf] http://ports.ubuntu.com/ubuntu-ports jammy-security main universe multiverse restricted' >> /etc/apt/sources.list
RUN dpkg --add-architecture armhf
# add for actions
RUN apt update && apt install -y curl git && apt-get clean
# add main build deps
RUN apt install --no-install-recommends -y build-essential pkg-config m4 python3 python3-distutils llvm llvm-dev lld libclang-dev clang && apt-get clean
# add cross deps
RUN apt install --no-install-recommends -y gcc-arm-linux-gnueabihf g++-arm-linux-gnueabihf qemu-user qemu-user-static && apt-get clean
# add runtime deps
RUN apt install --no-install-recommends -y libc6:armhf libstdc++6:armhf && apt-get clean
# set runner to qemu
ENV CARGO_TARGET_ARMV7_UNKNOWN_LINUX_GNUEABIHF_RUNNER="/usr/bin/qemu-arm"
# set linker to cross linker
ENV CARGO_TARGET_ARMV7_UNKNOWN_LINUX_GNUEABIHF_LINKER="/usr/bin/arm-linux-gnueabihf-gcc"

# use clang compiler
RUN echo '#!/bin/bash \nclang -target armv7-unknown-linux-gnueabihf -isysroot=/usr/arm-linux-gnueabihf --sysroot=/ -Wno-unused-command-line-argument -fuse-ld=lld "$@"' > /bin/clang-arm && chmod +x /bin/clang-arm
RUN echo '#!/bin/bash \nclang++ -target armv7-unknown-linux-gnueabihf -isysroot=/usr/arm-linux-gnueabihf --sysroot=/ -Wno-unused-command-line-argument -fuse-ld=lld "$@"' > /bin/clang++-arm && chmod +x /bin/clang++-arm
ENV CC="clang-arm"
ENV CXX="clang++-arm"
ENV BINDGEN_EXTRA_CLANG_ARGS="-isysroot=/usr/arm-linux-gnueabihf --sysroot=/ -Wno-unused-command-line-argument -fuse-ld=lld"
ENV CARGO_TARGET_ARMV7_UNKNOWN_LINUX_GNUEABIHF_LINKER="clang-arm"