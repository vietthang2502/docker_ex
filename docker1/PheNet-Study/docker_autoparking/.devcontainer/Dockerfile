# ARG PYTORCH="1.11.0"
# ARG CUDA="11.3"
# ARG CUDA="10.1"
# ARG CUDNN="8"


ARG PYTORCH="1.6.0"
ARG CUDA="10.1"
ARG CUDNN="7"

# FROM pytorch/pytorch:${PYTORCH}-cuI got my setup running after installing pytorch 1.6.0 with CUDA 10.1:da${CUDA}-cudnn${CUDNN}-devel
FROM pytorch/pytorch:${PYTORCH}-cuda${CUDA}-cudnn${CUDNN}-devel

ENV TORCH_CUDA_ARCH_LIST="6.0 6.1 7.0+PTX"
ENV TORCH_NVCC_FLAGS="-Xfatbin -compress-all"
ENV CMAKE_PREFIX_PATH="$(dirname $(which conda))/../"

# To fix GPG key error when running apt-get update
RUN apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/3bf863cc.pub
RUN apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/machine-learning/repos/ubuntu1804/x86_64/7fa2af80.pub

RUN apt-get update && apt-get install -y git ninja-build libglib2.0-0 libsm6 libxrender-dev libxext6 libgl1-mesa-glx \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*


RUN pip install mmcv-full -f https://download.openmmlab.com/mmcv/dist/cu101/torch1.6.0/index.html
# RUN pip install mmcv-full -f https://download.openmmlab.com/mmcv/dist/cu${CUDA}/torch1.6.0/index.html

RUN pip install mmdet
RUN conda clean --all

# Install MMCV
ARG PYTORCH
ARG CUDA
ARG MMCV
RUN ["/bin/bash", "-c", "pip install --no-cache-dir mmcv-full -f https://download.openmmlab.com/mmcv/dist/cu${CUDA//./}/torch${PYTORCH}/index.html"]

# Install MMSegmentation
COPY requirements_segment .
COPY requirements.txt .
RUN pip install -r requirements.txt
#install MMRotation 
COPY requirements_rotate.txt .
RUN pip install -r  requirements_rotate.txt
# RUN pip install --no-cache-dir -e .
RUN pip install ipykernel


