ARG IMAGE_NAME
FROM ${IMAGE_NAME}:10.1-devel-ubuntu20.04

ENV NV_CUDNN_PACKAGE_VERSION 7.6.5.32-1
ENV NV_CUDNN_VERSION 7.6.5.32

ENV NV_CUDNN_PACKAGE_NAME libcudnn7
ENV NV_CUDNN_PACKAGE ${NV_CUDNN_PACKAGE_NAME}=${NV_CUDNN_PACKAGE_VERSION}+cuda10.1
ENV NV_CUDNN_PACKAGE_DEV ${NV_CUDNN_PACKAGE_NAME}-dev=${NV_CUDNN_PACKAGE_VERSION}+cuda10.1
ENV NV_CUDNN_DL_HASHCMD sha256sum
ENV NV_CUDNN_DL_SUM 07d73672d03836126050e5b78b1a5199fabaa5a540b924903acba00cbfe81848
ENV NV_CUDNN_DL_BASENAME libcudnn7_7.6.5.32-1+cuda10.1_amd64.deb
ENV NV_CUDNN_DL_URL https://developer.download.nvidia.com/compute/machine-learning/repos/ubuntu1804/x86_64/libcudnn7_7.6.5.32-1+cuda10.1_amd64.deb

RUN apt-get update && apt-get install -y --no-install-recommends wget

RUN wget -q ${NV_CUDNN_DL_URL} \
    && echo "${NV_CUDNN_DL_SUM}  ${NV_CUDNN_DL_BASENAME}" | ${NV_CUDNN_DL_HASHCMD} -c - \
    && dpkg -i ${NV_CUDNN_DL_BASENAME} \
    && rm -f ${NV_CUDNN_DL_BASENAME}

ENV NV_CUDNN_DEV_DL_HASHCMD sha256sum
ENV NV_CUDNN_DEV_DL_SUM 3f83b8d599a2ccfd9bdc6c22363d1abdccd02cfb0bffd7350a93e7b961e26ecb
ENV NV_CUDNN_DEV_DL_BASENAME libcudnn7-dev_7.6.5.32-1+cuda10.1_amd64.deb
ENV NV_CUDNN_DEV_DL_URL https://developer.download.nvidia.com/compute/machine-learning/repos/ubuntu1804/x86_64/libcudnn7-dev_7.6.5.32-1+cuda10.1_amd64.deb

RUN wget -q ${NV_CUDNN_DEV_DL_URL} \
    && echo "${NV_CUDNN_DEV_DL_SUM}  ${NV_CUDNN_DEV_DL_BASENAME}" | ${NV_CUDNN_DEV_DL_HASHCMD} -c - \
    && dpkg -i ${NV_CUDNN_DEV_DL_BASENAME} \
    && rm -f ${NV_CUDNN_DEV_DL_BASENAME} \
    && apt-get purge --autoremove -y wget

ENV CUDNN_VERSION ${NV_CUDNN_VERSION}

LABEL com.nvidia.cudnn.version="${CUDNN_VERSION}"

RUN apt-get update && apt-get install -y --no-install-recommends \
    ${NV_CUDNN_PACKAGE} \
    ${NV_CUDNN_PACKAGE_DEV} \
    && apt-mark hold ${NV_CUDNN_PACKAGE_NAME} && \
    rm -rf /var/lib/apt/lists/*