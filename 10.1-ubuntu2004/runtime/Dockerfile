ARG IMAGE_NAME
FROM ${IMAGE_NAME}:10.1-base-ubuntu20.04

ENV NV_CUDA_LIB_VERSION 10.1.243-1
ENV NV_NVTX_VERSION 10.1.243-1
ENV NV_LIBNPP_VERSION 10.1.243-1
ENV NV_LIBCUSPARSE_VERSION 10.1.243-1


ENV NV_LIBCUBLAS_PACKAGE_NAME libcublas10

ENV NV_LIBCUBLAS_VERSION 10.2.1.243-1
ENV NV_LIBCUBLAS_PACKAGE ${NV_LIBCUBLAS_PACKAGE_NAME}=${NV_LIBCUBLAS_VERSION}

ENV NV_LIBNCCL_PACKAGE_NAME "libnccl2"
ENV NV_LIBNCCL_PACKAGE_VERSION 2.8.3-1
ENV NCCL_VERSION 2.8.3
ENV NV_LIBNCCL_PACKAGE ${NV_LIBNCCL_PACKAGE_NAME}=${NV_LIBNCCL_PACKAGE_VERSION}+cuda10.1
ENV NV_LIBNCCL_PACKAGE_SHA256SUM 2e2218653517288004b25cafbf511f523c42a3fa7af21e7edf32f145a4deda94
ENV NV_LIBNCCL_PACKAGE_SOURCE https://developer.download.nvidia.com/compute/machine-learning/repos/ubuntu1804/x86_64/libnccl2_2.8.3-1+cuda10.1_amd64.deb
ENV NV_LIBNCCL_PACKAGE_SOURCE_NAME libnccl2_2.8.3-1+cuda10.1_amd64.deb

RUN apt-get update && apt-get install -y --no-install-recommends wget

RUN wget -q ${NV_LIBNCCL_PACKAGE_SOURCE} \
    && echo "$NV_LIBNCCL_PACKAGE_SHA256SUM  ${NV_LIBNCCL_PACKAGE_SOURCE_NAME}" | sha256sum -c --strict - \
    && dpkg -i ${NV_LIBNCCL_PACKAGE_SOURCE_NAME} \
    && rm -f ${NV_LIBNCCL_PACKAGE_SOURCE_NAME} \
    && apt-get purge --autoremove -y wget \
    && rm -rf /var/lib/apt/lists/*

RUN apt-get update && apt-get install -y --no-install-recommends \
    cuda-libraries-10-1=${NV_CUDA_LIB_VERSION} \
    cuda-npp-10-1=${NV_LIBNPP_VERSION} \
    cuda-nvtx-10-1=${NV_NVTX_VERSION} \
    cuda-cusparse-10-1=${NV_LIBCUSPARSE_VERSION} \
    ${NV_LIBCUBLAS_PACKAGE} \
    ${NV_LIBNCCL_PACKAGE} \
    && rm -rf /var/lib/apt/lists/*

# Keep apt from auto upgrading the cublas and nccl packages. See https://gitlab.com/nvidia/container-images/cuda/-/issues/88
RUN apt-mark hold ${NV_LIBNCCL_PACKAGE_NAME} ${NV_LIBCUBLAS_PACKAGE_NAME}

# Add entrypoint items
COPY entrypoint.d/ /opt/nvidia/entrypoint.d/
COPY nvidia_entrypoint.sh /opt/nvidia/
ENV NVIDIA_PRODUCT_NAME="CUDA"
ENV NVIDIA_CUDA_END_OF_LIFE=1
ENTRYPOINT ["/opt/nvidia/nvidia_entrypoint.sh"]