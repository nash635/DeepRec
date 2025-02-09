# DeepRec源代码编译&安装

## 开发环境准备

**CPU Base Docker Image**

```
registry.cn-shanghai.aliyuncs.com/pai-dlc-share/deeprec-developer:deeprec-base-cpu-py36-ubuntu18.04
```

**GPU(cuda11.0) Base Docker Image**

```
registry.cn-shanghai.aliyuncs.com/pai-dlc-share/deeprec-developer:deeprec-base-gpu-py36-cu110-ubuntu18.04
```

**CPU Dev Docker (with bazel cache)**

```
registry.cn-shanghai.aliyuncs.com/pai-dlc-share/deeprec-developer:deeprec-dev-cpu-py36-ubuntu18.04
```

**GPU(cuda11.0) Dev Docker (with bazel cache)**

```
registry.cn-shanghai.aliyuncs.com/pai-dlc-share/deeprec-developer:deeprec-dev-gpu-py36-cu110-ubuntu18.04
```

## 代码编译

```bash
./configure
```

**GPU/CPU版本编译**

```bash
bazel build -c opt --config=opt //tensorflow/tools/pip_package:build_pip_package
```

**GPU/CPU版本编译+ABI=0**

```bash
bazel build --cxxopt="-D_GLIBCXX_USE_CXX11_ABI=0" --host_cxxopt="-D_GLIBCXX_USE_CXX11_ABI=0" -c opt --config=opt //tensorflow/tools/pip_package:build_pip_package
```

**编译开启OneDNN + Eigen Threadpool工作线程池版本（CPU）**

```bash
bazel build  -c opt --config=opt  --config=mkl_threadpool --define build_with_mkl_dnn_v1_only=true //tensorflow/tools/pip_package:build_pip_package
```

**编译开启OneDNN + Eigen Threadpool工作线程池版本+ABI=0的版本 （CPU）**

```bash
bazel build --cxxopt="-D_GLIBCXX_USE_CXX11_ABI=0" --host_cxxopt="-D_GLIBCXX_USE_CXX11_ABI=0" -c opt --config=opt --config=mkl_threadpool --define build_with_mkl_dnn_v1_only=true //tensorflow/tools/pip_package:build_pip_package
```

## 生成Whl包

```bash
./bazel-bin/tensorflow/tools/pip_package/build_pip_package /tmp/tensorflow_pkg
```

## 安装Whl包

```bash
pip3 install /tmp/tensorflow_pkg/tensorflow-1.15.5+${version}-cp36-cp36m-linux_x86_64.whl
```

## Nightly 镜像

**GPU CUDA11.0镜像**

```
registry.cn-shanghai.aliyuncs.com/pai-dlc-share/deeprec-training:deeprec-nightly-gpu-py36-cu110-ubuntu18.04
```

**CPU镜像**

```
registry.cn-shanghai.aliyuncs.com/pai-dlc-share/deeprec-training:deeprec-nightly-cpu-py36-ubuntu18.04
```

## Estimator编译&打包

由于DeepRec新增了分布式grpc++、star_server等protocol，在使用DeepRec配合原生Estimator会存在像grpc++, star_server功能使用时无法通过Estimator检查的问题，因为我们提供了针对DeepRec版本的Estimator，对应代码库：[https://github.com/AlibabaPAI/estimator](https://github.com/AlibabaPAI/estimator)

**代码编译**

```bash
bazel build //tensorflow_estimator/tools/pip_package:build_pip_package
```

**生成Wheel包**

```bash
bazel-bin/tensorflow_estimator/tools/pip_package/build_pip_package /tmp/estimator_pip
```

**安装**

安装DeepRec会默认安装原生的tensorflow-estimator的版本，请重装新编译的tensorflow-estimator即可。
