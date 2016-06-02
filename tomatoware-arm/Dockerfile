FROM thewtex/cross-compiler-base
MAINTAINER Matt McCormick "matt.mccormick@kitware.com"

# Enable 32 bits binaries
RUN dpkg --add-architecture i386 && \
    apt-get update && \
    apt-get install -y libstdc++6:i386 libgcc1:i386 zlib1g:i386

RUN apt-get update && \
  apt-get -y install --no-install-recommends \
  autoconf \
  automake \
  bash \
  bc \
  binutils \
  bison \
  build-essential \
  bzip2 \
  cvs \
  cpio \
  diffutils \
  doxygen \
  dpkg-dev \
  fakeroot \
  file \
  flex \
  g++ \
  gawk \
  gcc \
  gcc-multilib \
  gettext \
  git-core \
  gperf \
  groff-base \
  intltool \
  libbz2-dev \
  libc6-dev \
  libcurl4-openssl-dev \
  libgc-dev \
  libglib2.0-dev \
  libslang2 \
  libtool \
  make \
  patch \
  perl \
  pkg-config \
  python \
  python-all \
  python-dev \
  python2.7-dev \
  lib32z1 \
  lib32z-dev \
  libc6 \
  libexpat1-dev \
  libffi-dev \
  libgdbm-dev \
  libncurses-dev \
  libreadline6-dev \
  libssl-dev \
  libsqlite3-dev \
  libxml-parser-perl \
  locales \
  m4 \
  sed \
  shtool \
  sqlite \
  subversion \
  sudo \
  tar \
  texinfo \
  tk-dev \
  zlib1g \
  zlib1g-dev \
  unzip \
  libxml2-dev

RUN dpkg-reconfigure locales && \
    locale-gen C.UTF-8 && \
    /usr/sbin/update-locale LANG=C.UTF-8

ENV LC_ALL C.UTF-8

# The cross-compiling emulator
RUN apt-get update && apt-get install -y \
  qemu-user \
  qemu-user-static

# Allow dynamically linked executables to run with qemu-arm
ENV QEMU_LD_PREFIX ${CROSS_ROOT}/libc
ENV QEMU_SET_ENV "LD_LIBRARY_PATH=${CROSS_ROOT}/lib:${CROSS_ROOT}/libc/lib/${CROSS_TRIPLE}/"

ENV DESTARCH=arm \
    PREFIX=/opt/tomatoware \
    FLOAT=soft
ENV PATH=/opt/tomatoware/${DESTARCH}-${FLOAT}/usr/bin/:${PATH}
ENV EXTRACFLAGS="-O2 -pipe -march=armv7-a -mtune=cortex-a9"

WORKDIR /usr/src
RUN git clone https://github.com/lancethepants/cross-python.git && \
  cd cross-python && \
  git checkout 521c41b805dd1a92e556274c2cdf335dd41b4dca
RUN cd cross-python && \
  ./scripts/toolchain.sh

ENV CROSS_TRIPLE arm-linux
ENV CROSS_ROOT /usr/${CROSS_TRIPLE}
ENV AS=/usr/bin/${CROSS_TRIPLE}-as \
    AR=/usr/bin/${CROSS_TRIPLE}-ar \
    CC=/usr/bin/${CROSS_TRIPLE}-gcc \
    CPP=/usr/bin/${CROSS_TRIPLE}-cpp \
    CXX=/usr/bin/${CROSS_TRIPLE}-g++ \
    LD=/usr/bin/${CROSS_TRIPLE}-ld

COPY Toolchain.cmake ${CROSS_ROOT}/
ENV CMAKE_TOOLCHAIN_FILE ${CROSS_ROOT}/Toolchain.cmake
