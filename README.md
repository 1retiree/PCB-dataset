# DsPCBSD+

[![数据集 DOI](https://img.shields.io/badge/数据集_DOI-10.6084%2Fm9.figshare.24970329-blue)](https://doi.org/10.6084/m9.figshare.24970329)
[![论文 DOI](https://img.shields.io/badge/论文_DOI-10.1038%2Fs41597--024--03656--8-green)](https://doi.org/10.1038/s41597-024-03656-8)
[![许可证：CC BY 4.0](https://img.shields.io/badge/许可证-CC_BY_4.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/deed.zh-hans)

**DsPCBSD+** 是一个面向深度学习目标检测的真实工业印制电路板（Printed Circuit Board，PCB）表面缺陷数据集。

数据集图像采集自实际生产环境中蚀刻后的 PCB 内层板和外层板，使用工业自动光学检测（Automated Optical Inspection，AOI）设备获取。数据集共包含 **10,259 张图像**、**20,276 个经过人工标注的缺陷目标**和 **9 种 PCB 表面缺陷类别**。

数据集同时提供 **YOLO** 和 **COCO** 两种标注格式。

## 数据集特点

- 数据来自真实 PCB 工业生产环境
- 包含 10,259 张 JPG 图像
- 图像分辨率为 226 × 226 像素
- 包含 20,276 个缺陷边界框
- 包含 9 种 PCB 表面缺陷
- 提供 YOLO 和 COCO 两种标注格式
- 提供官方训练集和验证集划分
- 所有图像及标注均经过 PCB 行业专家审核
- 使用 CC BY 4.0 许可证公开发布

## 数据集统计

| 项目 | 数量或说明 |
|---|---:|
| 图像总数 | 10,259 |
| 缺陷边界框总数 | 20,276 |
| 缺陷类别数 | 9 |
| 训练集图像 | 8,208 |
| 验证集图像 | 2,051 |
| 训练集缺陷标注 | 16,184 |
| 验证集缺陷标注 | 4,092 |
| 图像格式 | JPG |
| 图像分辨率 | 226 × 226 |
| 标注格式 | YOLO、COCO |

## 缺陷类别

| 缩写 | 英文名称 | 中文名称 | 实例数量 |
|---|---|---|---:|
| SH | Short | 短路 | 915 |
| SP | Spur | 毛刺 | 4,584 |
| SC | Spurious copper | 残余铜 | 1,593 |
| OP | Open | 开路 | 1,770 |
| MB | Mouse bite | 鼠咬 | 2,529 |
| HB | Hole breakout | 破孔 | 2,883 |
| CS | Conductor scratch | 导体划伤 | 2,490 |
| CFO | Conductor foreign object | 导体异物 | 1,832 |
| BMFO | Base material foreign object | 基材异物 | 1,680 |
| **合计** |  |  | **20,276** |

一张图像中可能同时包含多个缺陷，也可能包含不同类别的缺陷。

## 缺陷尺寸分布

缺陷尺寸按照 COCO 数据集的定义进行划分：

| 缺陷尺寸 | 定义 | 实例数量 |
|---|---|---:|
| 小目标 | 面积小于 32² 像素 | 13,575 |
| 中等目标 | 面积大于等于 32² 且小于 96² 像素 | 5,797 |
| 大目标 | 面积大于等于 96² 像素 | 904 |
| **合计** |  | **20,276** |

DsPCBSD+ 的正式版本已保存在本仓库中，可直接获取数据集压缩包。

压缩包大小：约 122.59 MB

## 数据集目录结构

下载并解压后，数据集目录结构大致如下：

```text
data/
├── Data_YOLO/
│   ├── images/
│   │   ├── train/
│   │   └── val/
│   └── labels/
│       ├── train/
│       └── val/
├── Data_COCO/
│   ├── train2017/
│   ├── val2017/
│   └── annotations/
│       ├── instances_train2017.json
│       └── instances_val2017.json
```

## 标注格式

### YOLO 格式

每张图像对应一个同名的 `.txt` 标注文件。

标注文件中的每一行代表一个缺陷目标：

```text
<class_id> <x_center> <y_center> <width> <height>
```

其中：

- `class_id`：缺陷类别编号
- `x_center`：边界框中心点横坐标
- `y_center`：边界框中心点纵坐标
- `width`：边界框宽度
- `height`：边界框高度

坐标和边界框尺寸均按照图像宽度和高度进行了归一化。

请以下载数据中的类别配置和标注文件为准，不要仅根据本文档中类别表格的排列顺序推断类别编号。

### COCO 格式

COCO 标注文件主要包含以下字段：

- `images`：图像 ID、文件名、宽度和高度
- `annotations`：标注 ID、图像 ID、类别 ID、边界框和目标面积
- `categories`：缺陷类别 ID 和类别名称

COCO 边界框使用以下格式：

```text
[x_min, y_min, width, height]
```


## 数据采集与处理

原始缺陷图像采集自 PCB 制造车间中的工业 AOI 设备，主要包括蚀刻后的 PCB 内层板和外层板。

数据集构建流程包括：

1. 从 AOI 管理系统中收集候选缺陷图像
2. 删除无缺陷图像
3. 过滤重复或高度相似的缺陷图像
4. 删除缺陷边界不完整的图像
5. 删除无法仅通过二维图像可靠判断的缺陷
6. 对缺陷进行分类并标注边界框
7. 由 PCB 行业专家审核图像和标注
8. 按照 8:2 的比例划分训练集和验证集

原始候选数据包含 32,259 张图像。经过筛选、分类和标注后，最终形成包含 10,259 张图像的 DsPCBSD+ 数据集。

## 标注质量控制

所有图像和标注均经过人工检查。

共有五名具有 PCB 制造经验的专家参与标注审核。对于以下容易产生分歧的情况，由专家组共同讨论并确定最终标注：

- 不同缺陷类别外观相似
- 一个缺陷横跨多个 PCB 元素
- 同一区域存在多个相互重叠的缺陷
- 缺陷类别或边界框位置存在歧义

审核过程中综合考虑了缺陷对 PCB 性能的影响、缺陷所在位置、缺陷所占比例以及缺陷的可见程度。

## 基准实验结果

论文使用 Co-DETR 和 YOLOv6-L6 在官方训练集和验证集上进行了实验。

| 模型 | AP50 | AP75 | AP50:95 |
|---|---:|---:|---:|
| Co-DETR | 0.848 | 0.490 | 0.492 |
| YOLOv6-L6 | 0.851 | 0.525 | 0.514 |

以上结果为论文报告的参考结果。由于软件版本、硬件环境、训练参数和随机种子不同，复现实验结果可能存在一定差异。

论文实验环境包括：

- Ubuntu 20.04 64 位
- Intel Xeon Gold 6242R
- NVIDIA GeForce RTX 3090
- Co-DETR
- YOLOv6-L6

## 数据集局限性

使用 DsPCBSD+ 时需要注意以下限制：

- 图像仅包含二维视觉信息，无法识别需要深度信息的凸起或凹陷缺陷。
- 数据主要来自蚀刻后的 PCB 内层板和外层板。
- 数据集不包含阻焊工序之后产生的缺陷。
- 数据集中的图像为完整 PCB 上裁剪得到的局部区域。
- 数据集不能直接提供缺陷在完整 PCB 上的位置。
- 部分缺陷类别存在较大的类内差异。
- 小目标缺陷的视觉特征有限，检测难度较高。
- 在实际工业应用中，需要进一步将局部缺陷检测结果映射到完整 PCB。

## 相关论文

**论文名称：**

A dataset for deep learning based detection of printed circuit board surface defect

**作者：**

Shengping Lv、Bin Ouyang、Zhihua Deng、Tairan Liang、Shixin Jiang、Kaibin Zhang、Jianyu Chen、Zhuohui Li

**期刊：**

Scientific Data，第 11 卷，文章编号 811，2024 年

**论文地址：**

https://doi.org/10.1038/s41597-024-03656-8

**数据集地址：**

https://doi.org/10.6084/m9.figshare.24970329

## 引用方式

如果 DsPCBSD+ 对您的研究有所帮助，请同时引用数据集及其配套论文。

### 论文引用

```bibtex
@article{lv2024dspcbsd,
  author  = {Lv, Shengping and Ouyang, Bin and Deng, Zhihua and
             Liang, Tairan and Jiang, Shixin and Zhang, Kaibin and
             Chen, Jianyu and Li, Zhuohui},
  title   = {A dataset for deep learning based detection of printed
             circuit board surface defect},
  journal = {Scientific Data},
  volume  = {11},
  pages   = {811},
  year    = {2024},
  doi     = {10.1038/s41597-024-03656-8}
}
```

### 数据集引用

```bibtex
@misc{lv2024dspcbsd_dataset,
  author    = {Lv, Shengping and Ouyang, Bin and Deng, Zhihua and
               Liang, Tairan and Jiang, Shixin and Zhang, Kaibin and
               Chen, Jianyu and Li, Zhuohui},
  title     = {{DsPCBSD+}},
  year      = {2024},
  publisher = {Figshare},
  doi       = {10.6084/m9.figshare.24970329},
  url       = {https://doi.org/10.6084/m9.figshare.24970329}
}
```

## 许可证

DsPCBSD+ 使用
[知识共享署名 4.0 国际许可证（CC BY 4.0）](https://creativecommons.org/licenses/by/4.0/deed.zh-hans)
公开发布。

您可以：

- 复制和重新分发本数据集
- 修改、转换或基于本数据集开展研究
- 将本数据集用于学术或商业用途

但必须满足以下条件：

- 正确注明原作者和数据来源
- 提供 CC BY 4.0 许可证链接
- 保留数据集 DOI
- 明确说明是否对原始数据进行了修改
- 不得增加限制其他用户行使许可证权利的附加条款

建议署名格式：

```text
DsPCBSD+, Shengping Lv et al. (2024),
https://doi.org/10.6084/m9.figshare.24970329,
licensed under CC BY 4.0.
```

本仓库所引用的第三方软件仍然适用其各自的许可证。

## 致谢

本研究获得以下项目支持：

- 国家自然科学基金，项目编号：52275487
- 广东省自然科学基金，项目编号：2021A1515012395

感谢广州兴森快捷电路科技有限公司提供 PCB 表面缺陷图像，并在缺陷分类和数据标注过程中提供专业指导。

同时感谢所有参与数据筛选、分类、标注和审核的工作人员。

## 联系方式

关于原始数据集或论文的问题，请联系论文通讯作者：

**Shengping Lv（吕盛坪）**  
华南农业大学工程学院  
邮箱：lvshengping@scau.edu.cn
