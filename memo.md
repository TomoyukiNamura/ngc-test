# ngc-test

## install singularity
```
sudo apt-get update

sudo apt-get install -y \
    build-essential \
    libseccomp-dev \
    libglib2.0-dev \
    pkg-config \
    squashfs-tools \
    cryptsetup

sudo rm -r /usr/local/go

export VERSION=1.17.8 OS=linux ARCH=amd64  # change this as you need

wget -O /tmp/go${VERSION}.${OS}-${ARCH}.tar.gz https://dl.google.com/go/go${VERSION}.${OS}-${ARCH}.tar.gz && \
sudo tar -C /usr/local -xzf /tmp/go${VERSION}.${OS}-${ARCH}.tar.gz

echo 'export GOPATH=${HOME}/go' >> ~/.bashrc && \
echo 'export PATH=/usr/local/go/bin:${PATH}:${GOPATH}/bin' >> ~/.bashrc && \
source ~/.bashrc

curl -sfL https://install.goreleaser.com/github.com/golangci/golangci-lint.sh | sh -s -- -b $(go env GOPATH)/bin

mkdir -p ${GOPATH}/src/github.com/sylabs && \
cd ${GOPATH}/src/github.com/sylabs && \
git clone https://github.com/sylabs/singularity.git && \
cd singularity

git checkout v3.9.6

cd ${GOPATH}/src/github.com/sylabs/singularity && \
./mconfig && \
cd ./builddir && \
make && \
sudo make install

singularity version
```

# singularity usage
## pull

### tensorflow: 
singularity pull docker://nvcr.io/nvidia/tensorflow:22.02-tf2-py3

### pytorch: https://catalog.ngc.nvidia.com/orgs/nvidia/containers/pytorch
singularity pull docker://nvcr.io/nvidia/pytorch:22.02-py3

### TensorRT: https://catalog.ngc.nvidia.com/orgs/nvidia/containers/tensorrt
singularity pull docker://nvcr.io/nvidia/tensorrt:22.02-py3

## shell
singularity shell --nv tensorflow_22.02-tf2-py3.sif

# GPU libs version

## Cuda
```
$ nvcc -V
nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2021 NVIDIA Corporation
Built on Fri_Dec_17_18:16:03_PST_2021
Cuda compilation tools, release 11.6, V11.6.55
Build cuda_11.6.r11.6/compiler.30794723_0
```
## CuDNN()
```
$ cat /usr/include/cudnn_version.h  | grep CUDNN_MAJOR -A 2
#define CUDNN_MAJOR 8
#define CUDNN_MINOR 3
#define CUDNN_PATCHLEVEL 2
--
#define CUDNN_VERSION (CUDNN_MAJOR * 1000 + CUDNN_MINOR * 100 + CUDNN_PATCHLEVEL)

#endif /* CUDNN_VERSION_H */
```

```
$ dpkg -l | grep "cudnn"
ii  libcudnn8                         8.3.2.44-1+cuda11.5               amd64        cuDNN runtime libraries
ii  libcudnn8-dev                     8.3.2.44-1+cuda11.5               amd64        cuDNN development libraries and headers
```

## CUDA Toolkit
```
$ dpkg -l | grep "cuda-toolkit"
ii  cuda-toolkit-11-4-config-common   11.4.108-1                        all          Common config package for CUDA Toolkit 11.4.
ii  cuda-toolkit-11-6-config-common   11.6.55-1                         all          Common config package for CUDA Toolkit 11.6.
ii  cuda-toolkit-11-config-common     11.6.55-1                         all          Common config package for CUDA Toolkit 11.
ii  cuda-toolkit-config-common        11.6.55-1                         all          Common config package for CUDA Toolkit.
```

