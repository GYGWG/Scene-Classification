# 环境准备

---
## 目录
- [1. 手动配置PaddlePaddle, PaddleClas环境](#1)
  - [1.1 安装 PaddlePaddle](#1.1)
    - [1.1.1 使用Paddle官方镜像](#1.1.1)
    - [1.1.2 在现有环境中安装paddle](#1.1.2)
    - [1.1.3 安装验证](#1.1.3)
  - [1.2 克隆 PaddleClas](#1.2)
  - [1.3 安装 Python 依赖库](#1.3)
- [2. 快速创建PaddlePaddle, PaddleClas环境](#2)


我们提供了两种配置PaddlePaddle、PaddleClas环境的方法，第一种需要基于 docker 手动配置，您可以根据提供的命令更灵活的配置您的环境，详情请见[1. 手动配置PaddlePaddle, PaddleClas环境](#1)。第二种方式是我们将 PaddlePaddle、PaddleClas 相关的环境已配置到一个 docker 镜像中，您可以直接拉取使用，详情请见[2. 快速创建PaddlePaddle, PaddleClas环境](#2)。

<a name='1'></a>
## 1. 手动配置PaddlePaddle, PaddleClas环境

<a name='1.1'></a>
### 1.1 安装PaddlePaddle
目前，**PaddleClas** 要求 **PaddlePaddle** 版本 `>=2.3`。
建议使用Paddle官方提供的 Docker 镜像运行 PaddleClas，有关 Docker、nvidia-docker 的相关使用教程可以参考[链接](https://www.runoob.com/Docker/Docker-tutorial.html)。

<a name='1.1.1'></a>

#### 1.1.1 使用Paddle官方镜像

* 切换到工作目录下，例如工作目录为`/home/Projects`，则运行命令:

```shell
cd /home/Projects
```

* 创建 docker 容器

下述命令会创建一个名为 ppcls 的 Docker 容器，并将当前工作目录映射到容器内的 `/paddle` 目录。

```shell
# 对于 GPU 用户
sudo nvidia-docker run --name ppcls -v $PWD:/paddle --shm-size=8G --network=host -it registry.baidubce.com/paddlepaddle/paddle:2.3.0-gpu-cuda10.2-cudnn7 /bin/bash

# 对于 CPU 用户
sudo docker run --name ppcls -v $PWD:/paddle --shm-size=8G --network=host -it paddlepaddle/paddle:2.3.0-gpu-cuda10.2-cudnn7 /bin/bash
```

**注意**：
* 首次使用该镜像时，下述命令会自动下载该镜像文件，下载需要一定的时间，请耐心等待；
* 上述命令会创建一个名为 ppcls 的 Docker 容器，之后再次使用该容器时无需再次运行该命令；
* 参数 `--shm-size=8G` 将设置容器的共享内存为 8 G，如机器环境允许，建议将该参数设置较大，如 `64G`；
* 您也可以访问 [DockerHub](https://hub.Docker.com/r/paddlepaddle/paddle/tags/) ，手动选择需要的镜像；
* 退出/进入 docker 容器：
    * 在进入 Docker 容器后，可使用组合键 `Ctrl + P + Q` 退出当前容器，同时不关闭该容器；
    * 如需再次进入容器，可使用下述命令：

    ```shell
    sudo Docker exec -it ppcls /bin/bash
    ```
<a name='1.1.2'></a>
#### 1.1.2 在现有环境中安装paddle
您也可以用pip或conda直接安装paddle，详情请参考官方文档中的[快速安装](https://www.paddlepaddle.org.cn/install/quick?docurl=/documentation/docs/zh/install/docker/linux-docker.html)部分。

<a name='1.1.3'></a>
#### 1.1.3 安装验证
使用以下命令可以验证 PaddlePaddle 是否安装成功。
```python
import paddle
paddle.utils.run_check()
```
查看 PaddlePaddle 版本的命令如下：

```bash
python -c "import paddle; print(paddle.__version__)"
```

**注意**：
- 从源码编译的 PaddlePaddle 版本号为 `0.0.0`，请确保使用 PaddlePaddle 2.3 及之后的源码进行编译；
- PaddleClas 基于 PaddlePaddle 高性能的分布式训练能力，若您从源码编译，请确保打开编译选项 `WITH_DISTRIBUTE=ON`。具体编译选项参考 [编译选项表](https://www.paddlepaddle.org.cn/documentation/docs/zh/develop/install/Tables.html#bianyixuanxiangbiao)；
- 在 Docker 中运行时，为保证 Docker 容器有足够的共享内存用于 Paddle 的数据读取加速，在创建 Docker 容器时，请设置参数 `--shm-size=8g`，条件允许的话可以设置为更大的值。


<a name='1.2'></a>

### 1.2 克隆 PaddleClas

从 GitHub 下载：

```shell
git clone https://github.com/PaddlePaddle/PaddleClas.git
```

如果访问 GitHub 网速较慢，可以从 Gitee 下载，命令如下：

```shell
git clone https://gitee.com/paddlepaddle/PaddleClas.git
```
<a name='1.3'></a>

### 1.3 安装 PaddleClas 及其 Python 依赖库

* **[建议]** 直接安装 PaddleClas：

```shell
pip install paddleclas
```

* 如需使用 PaddleClas develop 分支体验最新功能，或是需要基于 PaddleClas 进行二次开发，请本地构建安装，命令如下：

```shell
python setup.py install
```

<a name='2'></a>
## 2. 快速创建PaddlePaddle, PaddleClas环境

我们也提供了包含最新 PaddleClas 代码的 docker 镜像，并预先安装好了所有的环境和库依赖，您只需要**拉取并运行docker镜像**，无需其他任何额外操作，即可开始享用 PaddleClas 的所有功能。

在[Docker Hub](https://hub.docker.com/repository/docker/paddlecloud/paddleclas)中获取这些镜像及相应的使用指南，包括CPU、GPU、ROCm 版本。

如果您对自动化制作docker镜像感兴趣，或有自定义需求，请访问[PaddlePaddle/PaddleCloud](https://github.com/PaddlePaddle/PaddleCloud/tree/main/tekton)做进一步了解。

**备注**：当前的镜像中的 PaddleClas 代码默认使用最新的 release/2.4 分支。

# 数据准备

<a name="2.1"></a>

## 数据集格式说明

PaddleClas 使用 `txt` 格式文件指定训练集和测试集，以场景分类为例，其中需要指定 `train_list.txt` 和 `val_list.txt` 当作训练集和验证集的数据标签，格式形如：

```
# 每一行采用"空格"分隔图像路径与标注
train/1.jpg 0
train/10.jpg 1
...
```

如果您想获取更多常用分类数据集的信息，可以参考文档可以参考 [PaddleClas 分类数据集格式说明](single_label_classification/dataset.md#1-数据集格式说明) 。

<a name="2.2"></a>

## 标注文件生成

如果您已经有实际场景中的数据，那么按照上节的格式进行标注即可。这里，我们提供了一个快速生成数据的脚本，您只需要将不同类别的数据分别放在文件夹中，运行脚本即可生成标注文件。

首先，假设您存放数据的路径为`./train`，`train/` 中包含了每个类别的数据，类别号从 0 开始，每个类别的文件夹中有具体的图像数据。

```shell
train
├── 0
│   ├── 0.jpg
│   ├── 1.jpg
│   └── ...
└── 1
    ├── 0.jpg
    ├── 1.jpg
    └── ...
└── ...
```

```shell
tree -r -i -f train | grep -E "jpg|JPG|jpeg|JPEG|png|PNG" | awk -F "/" '{print $0" "$2}' > train_list.txt
```

其中，如果涉及更多的图片名称尾缀，可以增加 `grep -E`后的内容， `$2` 中的 `2` 为类别号文件夹的层级。

**备注：** 以上为数据集获取和生成的方法介绍，本项目所使用的场景识别数据集如下。

进入 scene-classification/dataset/ 目录。

```
cd path/to/scene-classification/dataset
```

dataset文件夹下的SceneNet与SceneTest分别为场景识别任务的训练集与测试集，格式如下。

```shell
SceneNet
├── 0	#城市街巷
│   ├── 01.jpg
│   ├── 02.jpg
│   ├── ...
│   └── 90.jpg
├── 1	#丘陵
│   ├── 01.jpg
│   ├── 02.jpg
│   ├── ...
│   └── 90.jpg
├── 2	#山地
│   ├── 01.jpg
│   ├── 02.jpg
│   ├── ...
│   └── 90.jpg
└── 3	#戈壁
    ├── 01.jpg
    ├── 02.jpg
    ├── ...
    └── 90.jpg
```

```shell
SceneTest
├── 0	#城市街巷
│   ├── 91.jpg
│   ├── 92.jpg
│   ├── ...
│   └── 100.jpg
├── 1	#丘陵
│   ├── 91.jpg
│   ├── 92.jpg
│   ├── ...
│   └── 100.jpg
├── 2	#山地
│   ├── 91.jpg
│   ├── 92.jpg
│   ├── ...
│   └── 100.jpg
└── 3	#戈壁
    ├── 91.jpg
    ├── 92.jpg
    ├── ...
    └── 100.jpg
```

# 模型训练

## 骨干网络PP-LCNet

PULC 采用了轻量骨干网络 PP-LCNet，相比同精度竞品速度快 50%，您可以在[PP-LCNet介绍](../models/ImageNet1k/PP-LCNet.md)查阅该骨干网络的详细介绍。
直接使用 PP-LCNet 训练的命令为：

```shell
export CUDA_VISIBLE_DEVICES=0
python3 -m paddle.distributed.launch \
    --gpus="0" \
    tools/train.py \
        -c ./ppcls/configs/PULC/zw/PPLCNet_x1_0_search.yaml
```

# 模型导出

PaddlePaddle 支持导出 inference 模型用于部署推理场景，相比于训练调优场景，inference 模型会将网络权重与网络结构进行持久化存储，并且 PaddlePaddle 支持使用预测引擎加载 inference 模型进行预测推理。

## 分类模型导出

进入 scene-classification 目录下：

```shell
cd /path/to/scene-classification
```

场景识别使用的配置文件为 `ppcls/configs/PULC/zw/PPLCNet_x1_0_search.yaml`，将该模型转为 inference 模型只需运行如下命令：

```shell
python tools/export_model.py \
    -c ppcls/configs/PULC/zw/PPLCNet_x1_0_search.yaml \
    -o Global.pretrained_model=output/v4/PPLCNet_x1_0/best_model \
    -o Global.save_inference_dir=deploy/models/zw_SceneNet_v4
```

# 预测推理

## 场景识别推理

模型导出后，使用以下命令进行预测：

```shell
python3.7 deploy/python/predict_cls.py -c deploy/configs/inference_cls_zw.yaml
```
