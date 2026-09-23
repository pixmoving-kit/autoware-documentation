<a id="rosbag2-anonymizer"></a>

# Rosbag2 匿名化工具

<a id="overview"></a>

## 概述

Autoware 提供了一个工具（[autoware_rosbag2_anonymizer](https://github.com/autowarefoundation/autoware_rosbag2_anonymizer)），用于对 ROS 2 bag 文件进行匿名化处理。
如果希望向 Autoware 社区分享数据，同时保护数据隐私，该工具会很有用。

它可以模糊处理 bag 文件中的任意目标（人脸、车牌等），并生成新的 bag 文件，
其中包含处理后的图像。

<a id="installation"></a>

## 安装

<a id="clone-the-repository"></a>

### 克隆仓库

```bash
git clone https://github.com/autowarefoundation/autoware_rosbag2_anonymizer.git
cd autoware_rosbag2_anonymizer
```

<a id="download-the-pretrained-models"></a>

### 下载预训练模型

```bash
wget https://dl.fbaipublicfiles.com/segment_anything/sam_vit_h_4b8939.pth

wget https://huggingface.co/ShilongLiu/GroundingDINO/resolve/main/GroundingDINO_SwinB.cfg.py
wget https://huggingface.co/ShilongLiu/GroundingDINO/resolve/main/groundingdino_swinb_cogcoor.pth

wget https://github.com/autowarefoundation/autoware_rosbag2_anonymizer/releases/download/v0.0.0/yolov8x_anonymizer.pt
wget https://github.com/autowarefoundation/autoware_rosbag2_anonymizer/releases/download/v0.0.0/yolo_config.yaml
```

<a id="install-ros-2-mcap-dependencies-if-you-will-use-mcap-files"></a>

### 如需使用 mcap 文件，安装 ROS 2 mcap 依赖

!!! warning

    请确保系统已安装 ROS 2。

```bash
sudo apt install ros-humble-rosbag2-storage-mcap
```

<a id="install-autoware_rosbag2_anonymizer-tool"></a>

### 安装 `autoware_rosbag2_anonymizer` 工具

安装工具前，应先更新 pip 包管理器。

```bash
python3 -m pip install pip -U
```

然后可以使用以下命令安装工具。

```bash
python3 -m pip install .
```

<a id="configuration"></a>

## 配置

在 `validation.json` 中定义提示词，工具会用这些提示词检测目标。可以在 prompts 键下
以字典形式添加提示词。每个字典应包含两个键：

- `prompt`：用于检测目标的提示词。该提示词对应的目标将在匿名化过程中被模糊处理。
- `should_inside`：目标应位于其中的提示词列表。如果目标不在这些提示词对应的区域内，
  工具就不会对其进行模糊处理。

```json
{
  "prompts": [
    {
      "prompt": "license plate",
      "should_inside": ["car", "bus", "..."]
    },
    {
      "prompt": "human face",
      "should_inside": ["person", "human body", "..."]
    }
  ]
}
```

应根据使用方式，设置 config 文件夹下的配置文件。以下
说明将指导你设置各配置文件。

- `config/anonymize_with_unified_model.yaml`

```yaml
rosbag:
  input_bags_folder: "/path/to/input_bag_folder" # Path to the input folder which contains ROS 2 bag files
  output_bags_folder: "/path/to/output_folder" # Path to the output ROS 2 bag folder
  output_save_compressed_image: True # Save images as compressed images (True or False)
  output_storage_id: "sqlite3" # Storage id for the output bag file (`sqlite3` or `mcap`)

grounding_dino:
  box_threshold: 0.1 # Threshold for the bounding box (float)
  text_threshold: 0.1 # Threshold for the text (float)
  nms_threshold: 0.1 # Threshold for the non-maximum suppression (float)

open_clip:
  score_threshold: 0.7 # Validity threshold for the OpenCLIP model (float

yolo:
  confidence: 0.15 # Confidence threshold for the YOLOv8 model (float)

bbox_validation:
  iou_threshold: 0.9 # Threshold for the intersection over union (float), if the intersection over union is greater than this threshold, the object will be selected as inside the validation prompt

blur:
  kernel_size: 31 # Kernel size for the Gaussian blur (int)
  sigma_x: 11 # Sigma x for the Gaussian blur (int)
```

- `config/yolo_create_dataset.yaml`

```yaml
rosbag:
  input_bags_folder: "/path/to/input_bag_folder" # Path to the input ROS 2 bag files folder

dataset:
  output_dataset_folder: "/path/to/output/dataset" # Path to the output dataset folder
  output_dataset_subsample_coefficient: 25 # Subsample coefficient for the dataset (int)

grounding_dino:
  box_threshold: 0.1 # Threshold for the bounding box (float)
  text_threshold: 0.1 # Threshold for the text (float)
  nms_threshold: 0.1 # Threshold for the non-maximum suppression (float)

open_clip:
  score_threshold: 0.7 # Validity threshold for the OpenCLIP model (float

bbox_validation:
  iou_threshold: 0.9 # Threshold for the intersection over union (float), if the intersection over union is greater than this threshold, the object will be selected as inside the validation prompt
```

- `config/yolo_train.yaml`

```yaml
dataset:
  input_dataset_yaml: "path/to/data.yaml" # Path to the config file of the dataset, which is created in the previous step

yolo:
  epochs: 100 # Number of epochs for the YOLOv8 model (int)
  model: "yolov8x.pt" # Select the base model for YOLOv8 ('yolov8x.pt' 'yolov8l.pt', 'yolov8m.pt', 'yolov8n.pt')
```

- `config/yolo_anonymize.yaml`

```yaml
rosbag:
  input_bag_path: "/path/to/input_bag/bag.mcap" # Path to the input ROS 2 bag file with 'mcap' or 'sqlite3' extension
  output_bag_path: "/path/to/output_bag_file" # Path to the output ROS 2 bag folder
  output_save_compressed_image: True # Save images as compressed images (True or False)
  output_storage_id: "sqlite3" # Storage id for the output bag file (`sqlite3` or `mcap`)

yolo:
  model: "path/to/yolo/model" # Path to the trained YOLOv8 model file (`.pt` extension) (you can download the pre-trained model from releases)
  config_path: "path/to/input/data.yaml" # Path to the config file of the dataset, which is created in the previous step
  confidence: 0.15 # Confidence threshold for the YOLOv8 model (float)

blur:
  kernel_size: 31 # Kernel size for the Gaussian blur (int)
  sigma_x: 11 # Sigma x for the Gaussian blur (int)
```

<a id="usage"></a>

## 使用方法

此工具提供两种方式，对 ROS 2 bag 文件中的图像进行匿名化。

!!! warning

    如果 ROS 2 bag 文件包含来自 Autoware 或其他功能包的自定义消息类型，应在运行工具前 source
    相应的工作区。

    可以使用以下命令 source Autoware 工作区。
    ```bash
    source /path/to/your/workspace/install/setup.bash
    ```

<a id="option-1-anonymize-with-unified-model"></a>

### 方式 1：使用统一模型匿名化

提供一个 rosbag 后，工具会使用统一模型对其中的图像进行匿名化。该模型结合了
GroundingDINO、OpenCLIP、YOLOv8 和 SegmentAnything。如果不想使用预训练的 YOLOv8 模型，可以
按方式 2 的说明训练自己的 YOLOv8 模型。

应在 config/anonymize_with_unified_model.yaml 文件中设置配置。

```bash
python3 main.py config/anonymize_with_unified_model.yaml --anonymize_with_unified_model
```

<a id="option-2-anonymize-using-the-yolov8-model-trained-on-a-dataset-created-with-the-unified-model"></a>

### 方式 2：使用基于统一模型所建数据集训练的 YOLOv8 模型匿名化

<a id="step-1-create-a-dataset"></a>

#### 第 1 步：创建数据集

使用统一模型创建初始数据集。可以提供多个 ROS 2 bag 文件来构建数据集。运行
以下命令后，工具将创建 YOLO 格式的数据集。

应在 config/yolo_create_dataset.yaml 文件中设置配置。

```bash
python3 main.py config/yolo_create_dataset.yaml --yolo_create_dataset
```

<a id="step-2-manually-label-the-missing-labels"></a>

#### 第 2 步：手动补充缺失标注

第 1 步创建的数据集存在部分缺失标注，需要手动补齐。
可以使用以下工具补充标注：

- [label-studio](https://github.com/HumanSignal/label-studio)
- [Roboflow](https://roboflow.com/)（可以使用免费版本）

<a id="step-3-split-the-dataset"></a>

#### 第 3 步：划分数据集

将数据集划分为训练集和验证集，并提供第 1 步创建的
数据集文件夹路径。

```bash
autoware-rosbag2-anonymizer-split-dataset /path/to/dataset/folder
```

<a id="step-4-train-the-yolov8-model"></a>

#### 第 4 步：训练 YOLOv8 模型

使用第 1 步创建的数据集训练 YOLOv8 模型。

应在 config/yolo_train.yaml 文件中设置配置。

```bash
python3 main.py config/yolo_train.yaml --yolo_train
```

<a id="step-5-anonymize-images-in-ros-2-bag-files"></a>

#### 第 5 步：对 ROS 2 bag 文件中的图像匿名化

使用训练后的 YOLOv8 模型对 ROS 2 bag 文件中的图像进行匿名化。如果只想使用
YOLOv8 模型，请执行以下命令。但为获得更好的结果，建议使用统一模型。你可以
按方式 1 操作，在统一模型中使用自己训练的 YOLOv8 模型。

应在 config/yolo_anonymize.yaml 文件中设置配置。

```bash
python3 main.py config/yolo_anonymize.yaml --yolo_anonymize
```

<a id="troubleshooting"></a>

## 故障排查

- **错误 1**：`torch.OutOfMemoryError: CUDA out of memory`

```bash
torch.OutOfMemoryError: CUDA out of memory. Tried to allocate 1024.00 MiB. GPU 0 has a total capacity of 10.87 GiB of which 1010.88 MiB is free. Including non-PyTorch memory, this process has 8.66 GiB memory in use. Of the allocated memory 8.21 GiB is allocated by PyTorch, and 266.44 MiB is reserved by PyTorch but unallocated. If reserved but unallocated memory is large try setting PYTORCH_CUDA_ALLOC_CONF=expandable_segments:True to avoid fragmentation.  See documentation for Memory Management  (https://pytorch.org/docs/stable/notes/cuda.html#environment-variables)
```

GPU 显存不足以运行模型时会发生此错误。可以添加以下环境变量
来避免此错误。

```bash
export PYTORCH_CUDA_ALLOC_CONF=expandable_segments:True
```

<a id="share-your-anonymized-data"></a>

## 分享匿名化数据

数据匿名化后，可以与 Autoware 社区分享。如果希望向 Autoware 社区分享
数据，应创建 issue 和拉取请求，提交到
[Autoware 文档仓库](https://github.com/autowarefoundation/autoware-documentation)。

<a id="citation"></a>

## 引用

```bibtex
@article{liu2023grounding,
  title={Grounding dino: Marrying dino with grounded pre-training for open-set object detection},
  author={Liu, Shilong and Zeng, Zhaoyang and Ren, Tianhe and Li, Feng and Zhang, Hao and Yang, Jie and Li, Chunyuan and Yang, Jianwei and Su, Hang and Zhu, Jun and others},
  journal={arXiv preprint arXiv:2303.05499},
  year={2023}
}
```

```bibtex
@article{kirillov2023segany,
  title={Segment Anything},
  author={Kirillov, Alexander and Mintun, Eric and Ravi, Nikhila and Mao, Hanzi and Rolland, Chloe and Gustafson, Laura and Xiao, Tete and Whitehead, Spencer and Berg, Alexander C. and Lo, Wan-Yen and Doll{\'a}r, Piotr and Girshick, Ross},
  journal={arXiv:2304.02643},
  year={2023}
}
```

```bibtex
@software{ilharco_gabriel_2021_5143773,
  author       = {Ilharco, Gabriel and
                  Wortsman, Mitchell and
                  Wightman, Ross and
                  Gordon, Cade and
                  Carlini, Nicholas and
                  Taori, Rohan and
                  Dave, Achal and
                  Shankar, Vaishaal and
                  Namkoong, Hongseok and
                  Miller, John and
                  Hajishirzi, Hannaneh and
                  Farhadi, Ali and
                  Schmidt, Ludwig},
  title        = {OpenCLIP},
  month        = jul,
  year         = 2021,
  note         = {If you use this software, please cite it as below.},
  publisher    = {Zenodo},
  version      = {0.1},
  doi          = {10.5281/zenodo.5143773},
  url          = {https://doi.org/10.5281/zenodo.5143773}
}
```
