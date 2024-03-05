ARG DOCKER_BASE_IMAGE=debian:8
# extract kernel and toolkit dev
FROM ${DOCKER_BASE_IMAGE} AS extract

ARG KERNEL_SRC_FILENAME
ADD downloads/${KERNEL_SRC_FILENAME} /

# tool chain image
FROM ${DOCKER_BASE_IMAGE}

ARG DEBIAN_FRONTEND=noninteractive
ARG TARGET_NAME
ARG TARGET_PLATFORM
ARG TARGET_VERSION
ARG DSM_VERSION
ARG COMPILE_WITH
ARG TARGET_REVISION
ARG REDPILL_LKM_MAKE_TARGET

RUN cat /etc/os-release | grep -q 'jessie' && echo 'APT::Get::AllowUnauthenticated "true";' | tee /etc/apt/apt.conf.d/99disable-gpg-auth || true

RUN if [ "6.2.4" = "$TARGET_VERSION" ]; then \
      sed -i 's#deb.debian.org#archive.debian.org/debian-archive#g' /etc/apt/sources.list && \
      sed -i 's#security.debian.org#archive.debian.org/debian-archive#g' /etc/apt/sources.list && \
      sed -i 's#jessie-updates#jessie#g' /etc/apt/sources.list; \
    fi

RUN apt-get update && \
    apt-get install --yes ca-certificates build-essential git libssl-dev curl cpio bspatch vim gettext bc bison flex dosfstools kmod && \
    curl --progress-bar --output /usr/bin/jq --location https://github.com/stedolan/jq/releases/download/jq-1.6/jq-linux64 && chmod +x /usr/bin/jq && \
    rm -rf /var/lib/apt/lists/* /tmp/*

ARG REDPILL_LKM_REPO=https://github.com/RedPill-TTG/redpill-lkm.git
ARG REDPILL_LKM_BRANCH=master
ARG REDPILL_LKM_SRC=/opt/redpill-lkm

ARG REDPILL_LOAD_REPO=https://github.com/RedPill-TTG/redpill-load.git
ARG REDPILL_LOAD_BRANCH=master
ARG REDPILL_LOAD_SRC=/opt/redpill-load

RUN git clone ${REDPILL_LKM_REPO}  -b ${REDPILL_LKM_BRANCH}  ${REDPILL_LKM_SRC} && \
    git clone ${REDPILL_LOAD_REPO} -b ${REDPILL_LOAD_BRANCH} ${REDPILL_LOAD_SRC}

LABEL redpill-tool-chain=${TARGET_PLATFORM}-${TARGET_VERSION}-${TARGET_REVISION}

ENV ARCH=x86_64 \
    LINUX_SRC=/opt/${COMPILE_WITH}-${TARGET_PLATFORM}-${TARGET_VERSION}-${TARGET_REVISION} \
    REDPILL_LKM_SRC=${REDPILL_LKM_SRC} \
    REDPILL_LOAD_SRC=${REDPILL_LOAD_SRC} \
    TARGET_NAME=${TARGET_NAME} \
    TARGET_PLATFORM=${TARGET_PLATFORM} \
    TARGET_VERSION=${TARGET_VERSION} \
    TARGET_REVISION=${TARGET_REVISION} \
    REDPILL_LKM_MAKE_TARGET=${REDPILL_LKM_MAKE_TARGET}

ARG EXTRACTED_KSRC
COPY --from=extract ${EXTRACTED_KSRC} ${LINUX_SRC}

RUN if [ "6.2.4" = "$TARGET_VERSION" ];then \
      cd ${REDPILL_LOAD_SRC}/config/DS918+/6.2.4-25556/ && pwd && \
      sed -i 's#ee021d9cd012a434578309845e3af999a43f81a7887ceec7319e6005ac5930d4#24336ef3aa4ab2c484602c6fe9b3b05edf68b7915df8db07b96a7bf21eff5fda#g' config.json && \
      sed -i 's#3f0f954c9bb6ef0fa195e244f6219ecb3c7a7b297ff6d584a119437eec612e88#e48eacb14f775119a1aba1c36bca33b784d6bb7e8fe0a2d2352070aa5a517bea#g' config.json && \
      sed -i 's#bdb6a1b0f48a5ed713b9095db2b5976971cf714425982a1b5782f70749231324#66f8ccbc99e430b205c998a076ce7cbcfa2fbe4dd024cf4355a510b8d75783a0#g' config.json \
    fi

RUN if [ "apollolake" = "$TARGET_PLATFORM" ] || [ "broadwellnk" = "$TARGET_PLATFORM"  ] || [ "geminilake" = "$TARGET_PLATFORM" ] || [ "v1000" = "$TARGET_PLATFORM" ] || [ "denverton" = "$TARGET_PLATFORM" ]; then mkdir -p ${LINUX_SRC} && echo '+' > ${LINUX_SRC}/.scmversion; fi && \
    if [ "$COMPILE_WITH" = "kernel" ]; then \
        cp ${LINUX_SRC}/synoconfigs/${TARGET_PLATFORM} ${LINUX_SRC}/.config && \
        make -C ${LINUX_SRC} oldconfig && \
        make -C ${LINUX_SRC} modules_prepare ;\
    fi

WORKDIR "/opt"

COPY Makefile /opt/

COPY entrypoint.sh /entrypoint.sh
COPY helper.sh ./
RUN chmod +x /entrypoint.sh  && chmod +x ./helper.sh

ENTRYPOINT [ "/entrypoint.sh" ]
