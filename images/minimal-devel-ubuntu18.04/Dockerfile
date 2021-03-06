# Copyright (c) 2019-2020 Intel Corporation.
# SPDX-License-Identifier: BSD-3-Clause

# requires os-tools image
ARG base_image="intel/oneapi:os-tools-ubuntu18.04"
FROM "$base_image"

ARG DEBIAN_FRONTEND=noninteractive
ARG APT_KEY_DONT_WARN_ON_DANGEROUS_USAGE=1

# add apt repo public key
ARG url=https://apt.repos.intel.com/intel-gpg-keys/GPG-PUB-KEY-INTEL-SW-PRODUCTS-2023.PUB
ADD $url /
RUN file=$(basename "$url") && \
    apt-key add "$file" && \
    rm "$file"

# configure the repository
ARG apt_repo=https://apt.repos.intel.com/oneapi
RUN echo "deb $apt_repo all main" > /etc/apt/sources.list.d/oneAPI.list

RUN apt-get update -y

# install Intel(R) oneAPI dpcpp compiler package only
RUN apt-get install -y --no-install-recommends -o=Dpkg::Use-Pty=0 \
intel-oneapi-dpcpp-compiler
--

COPY fpgavars.sh /opt/intel/inteloneapi/compiler/latest/linux/lib/oclfpga

# install Intel GPU drivers
RUN echo 'deb [trusted=yes arch=amd64] https://repositories.intel.com/graphics/ubuntu bionic-devel main' \
> /etc/apt/sources.list.d/intel-graphics.list

RUN apt-get update -y && \
apt-get install -y --no-install-recommends -o=Dpkg::Use-Pty=0 \
intel-opencl \
intel-level-zero-gpu \
level-zero

# entrypoint
COPY entrypoint.sh /
RUN chmod a+x entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]
CMD ["/bin/bash"]
