**PCB4ZSAD** 是一个面向印制电路板（Printed Circuit Board，PCB）零样本异常检测（Zero-Shot Anomaly Detection，ZSAD）的工业视觉数据集。该数据集用于同时评估：

- 图像级异常识别：判断输入 PCB 图像是否存在异常；
- 像素级异常定位：定位异常区域并输出像素级异常图。

项目主页：

https://github.com/1retiree/PCB-dataset/tree/PCB4ZSAD

本仓库提供 PCB4ZSAD 的数据说明、目录结构、像素级异常标注和预定义数据划分，用于验证异常检测模型对真实 PCB 表面缺陷的跨类别泛化能力。

## 数据集简介

| 项目 | 说明 |
|---|---:|
| 图像总数 | 6,802 |
| 正常图像 | 4,751 |
| 异常图像 | 2,051 |
| 缺陷类别 | 9 类 |
| 标注层级 | 图像级标签、像素级异常掩膜 |
| 数据划分 | 提供预定义划分 |
| 主要任务 | 零样本异常检测、异常定位 |
| 应用场景 | PCB 自动光学检测与工业质量控制 |

正常图像约占全部图像的 69.8%，异常图像约占 30.2%。

## 数据集特点

- 包含正常 PCB 图像和真实缺陷图像；
- 覆盖 9 种具有代表性的 PCB 表面缺陷；
- 为异常图像提供像素级异常区域标注；
- 同时支持图像级检测和像素级定位评估；
- 包含从小面积缺陷到大面积缺陷的多尺度异常；
- 提供固定的数据划分，便于不同方法进行公平比较；
- 面向零样本场景，适合评估模型在未见 PCB 缺陷上的泛化能力。

## 缺陷类别

| 缩写 | 英文名称 | 中文名称 | 异常样本数 |
|---|---|---|---:|
| BMFO | Base Material Foreign Object | 基材异物 | 226 |
| CFO | Conductor Foreign Object | 导体异物 | 199 |
| CS | Conductor Scratch | 导体划伤 | 211 |
| HB | Hole Breakout | 破孔／孔环破损 | 255 |
| MB | Mouse Bite | 鼠咬 | 295 |
| OP | Open | 开路 | 250 |
| SH | Short | 短路 | 150 |
| SC | Spurious Copper | 残余铜／多余铜 | 123 |
| SP | Spur | 毛刺 | 342 |
| **合计** |  |  | **2,051** |

## 缺陷尺寸分布

缺陷尺寸根据像素级异常掩膜包含的异常像素数量进行划分。设异常区域面积为 \(A\)：

- 小缺陷：\(A < 500\) 像素；
- 中等缺陷：\(500 \le A < 2000\) 像素；
- 大缺陷：\(A \ge 2000\) 像素。

| 缺陷类别 | 小缺陷 | 中等缺陷 | 大缺陷 | 合计 |
|---|---:|---:|---:|---:|
| BMFO | 167 | 32 | 27 | 226 |
| CFO | 73 | 71 | 55 | 199 |
| CS | 69 | 91 | 51 | 211 |
| HB | 2 | 136 | 117 | 255 |
| MB | 281 | 13 | 1 | 295 |
| OP | 178 | 62 | 10 | 250 |
| SH | 126 | 21 | 3 | 150 |
| SC | 65 | 49 | 9 | 123 |
| SP | 316 | 24 | 2 | 342 |
| **合计** | **1,277** | **499** | **275** | **2,051** |

在全部异常样本中，小缺陷约占 62.3%，中等缺陷约占 24.3%，大缺陷约占 13.4%。因此，PCB4ZSAD 尤其适合评估模型对小面积、低对比度和稀疏 PCB 缺陷的识别与定位能力。

## 标注信息

PCB4ZSAD 包含以下两类标注：

### 图像级标签

每张图像被标记为正常或异常，用于计算图像级异常检测指标。

### 像素级标注

每张异常图像提供对应的像素级异常掩膜，用于表示真实缺陷区域并评估异常定位性能。

使用数据时，请以发布包中的说明文件、划分文件和掩膜文件为准，并确保：

1. 图像与对应掩膜能够通过文件名或索引一一匹配；
2. 正常图像不应带有异常区域；
3. 评估时保持官方预定义划分不变；
4. 不使用目标测试数据进行类别特定训练或参数优化。

## 数据文件

PCB4ZSAD 按缺陷类别组织数据。根目录中的 `meta.json` 保存数据集元信息，9 个缺陷类别目录采用相同结构。

```text
PCB4ZSAD/
├── meta.json
├── BMFO/
│   ├── train/
│   │   └── img/
│   │       ├── 111.png
│   │       └── ...
│   └── test/
│       ├── img/
│       │   ├── 000.png
│       │   └── ...
│       ├── anomaly_mask/
│       │   ├── 000.png
│       │   └── ...
│       └── good/
│           ├── 000.png
│           └── ...
├── CFO/
│   └── ...
├── CS/
│   └── ...
├── HB/
│   └── ...
├── MB/
│   └── ...
├── OP/
│   └── ...
├── SH/
│   └── ...
├── SC/
│   └── ...
└── SP/
    └── ...
```

各目录含义如下：

| 路径 | 内容 |
|---|---|
| `meta.json` | 数据集全局元信息 |
| `<类别>/train/img/` | 该类别对应的训练图像 |
| `<类别>/test/good/` | 正常测试图像 |
| `<类别>/test/img/` | 异常测试图像 |
| `<类别>/test/anomaly_mask/` | 异常测试图像对应的像素级掩膜 |

`<类别>` 为以下 9 个目录之一：

```text
BMFO  CFO  CS  HB  MB  OP  SH  SC  SP
```

异常图像与异常掩膜通过文件名对应。例如：

```text
BMFO/test/img/000.png
BMFO/test/anomaly_mask/000.png
```

如果 `anomaly_mask` 中还包含子目录，应以文件主名和 `meta.json` 中记录的对应关系为准。

为了避免大规模图像文件使 Git 仓库过于臃肿，建议通过本仓库的 [Releases](../../releases) 发布完整数据压缩包，并在源码仓库中保留 README、划分文件、元数据和评估代码。

## 快速读取

以下 Python 代码可用于读取 `meta.json`，并统计各类别中的训练图像、正常测试图像、异常测试图像和异常掩膜。

```python
import json
from pathlib import Path

DATASET_ROOT = Path("PCB4ZSAD")
CLASSES = ["BMFO", "CFO", "CS", "HB", "MB", "OP", "SH", "SC", "SP"]
IMAGE_SUFFIXES = {".png", ".jpg", ".jpeg", ".bmp", ".tif", ".tiff"}


def list_images(directory: Path):
    if not directory.exists():
        return []
    return sorted(
        path
        for path in directory.rglob("*")
        if path.is_file() and path.suffix.lower() in IMAGE_SUFFIXES
    )


meta_path = DATASET_ROOT / "meta.json"
with meta_path.open("r", encoding="utf-8") as file:
    metadata = json.load(file)

print("Metadata loaded from:", meta_path)

for class_name in CLASSES:
    class_root = DATASET_ROOT / class_name

    train_images = list_images(class_root / "train" / "img")
    normal_test_images = list_images(class_root / "test" / "good")
    anomaly_test_images = list_images(class_root / "test" / "img")
    anomaly_masks = list_images(class_root / "test" / "anomaly_mask")

    print(
        f"{class_name:>4} | "
        f"train={len(train_images):4d} | "
        f"test_good={len(normal_test_images):4d} | "
        f"test_anomaly={len(anomaly_test_images):4d} | "
        f"masks={len(anomaly_masks):4d}"
    )
```

### 检查异常图像与掩膜是否匹配

下面的代码按照不含扩展名的文件主名检查异常图像和掩膜。该写法也兼容 `anomaly_mask` 下存在子目录的情况。

```python
from pathlib import Path

DATASET_ROOT = Path("PCB4ZSAD")
CLASSES = ["BMFO", "CFO", "CS", "HB", "MB", "OP", "SH", "SC", "SP"]
IMAGE_SUFFIXES = {".png", ".jpg", ".jpeg", ".bmp", ".tif", ".tiff"}


def index_by_stem(directory: Path):
    return {
        path.stem: path
        for path in directory.rglob("*")
        if path.is_file() and path.suffix.lower() in IMAGE_SUFFIXES
    }


for class_name in CLASSES:
    test_root = DATASET_ROOT / class_name / "test"
    anomaly_images = index_by_stem(test_root / "img")
    anomaly_masks = index_by_stem(test_root / "anomaly_mask")

    missing_masks = sorted(set(anomaly_images) - set(anomaly_masks))
    extra_masks = sorted(set(anomaly_masks) - set(anomaly_images))

    print(
        f"{class_name}: "
        f"anomaly_images={len(anomaly_images)}, "
        f"masks={len(anomaly_masks)}, "
        f"missing_masks={len(missing_masks)}, "
        f"extra_masks={len(extra_masks)}"
    )

    if missing_masks:
        print("  Missing mask examples:", missing_masks[:5])
    if extra_masks:
        print("  Extra mask examples:", extra_masks[:5])
```

## 适用任务

PCB4ZSAD 可用于以下研究任务：

- 零样本异常检测；
- PCB 表面缺陷检测；
- 像素级异常定位；
- 小缺陷与低对比度缺陷检测；
- 工业视觉模型跨类别泛化评估；
- 视觉语言模型异常检测；
- 异常热图和缺陷分割算法评估。

## 推荐的零样本评估协议

PCB4ZSAD 作为未见目标数据集进行评估。推荐遵循以下协议：

1. 使用不包含 PCB4ZSAD 目标样本的辅助数据集训练模型；
2. 训练阶段不得使用 PCB4ZSAD 图像、标签或掩膜优化模型；
3. 模型训练完成后，直接在 PCB4ZSAD 的预定义测试划分上推理；
4. 同时报告图像级异常识别和像素级异常定位结果；
5. 若调整阈值或超参数，应明确说明所使用的数据，不得利用测试标签进行调参；
6. 保持原始数据划分，以保证不同方法之间的结果可比性。

在本项目提供的参考实验中，评估 PCB4ZSAD 时使用 MVTec AD 作为辅助训练数据集。

需要注意，目录名称中的 `train` 表示数据集提供的预定义训练部分，但在严格的零样本目标域评估中，不应使用 PCB4ZSAD 的目标图像、标签或掩膜对模型进行类别特定训练。若将 `train/img` 用于其他监督、半监督或无监督实验，应明确标注所采用的实验协议。

## 评估指标

### 图像级指标

- **AUROC**：接收者操作特征曲线下面积；
- **AP**：平均精度。

### 像素级指标

- **AUPRO**：区域重叠率曲线下面积，本项目在假阳性率 \(FPR < 0.3\) 的范围内计算；
- **F1-max**：在 200 个阈值上计算得到的最大像素级 F1 分数。

对于包含多个缺陷类别的结果，建议先分别计算每个类别的指标，再报告所有类别的平均值。


## 使用注意事项

- PCB4ZSAD 面向零样本异常检测，不应将目标测试样本混入辅助训练数据；
- 建议同时报告图像级和像素级结果，避免仅使用单一指标描述模型性能；
- 小缺陷占异常样本的大多数，应关注模型在小目标上的漏检问题；
- 不同缺陷类别的空间尺度差异明显，使用固定阈值时应说明阈值选择方式；
- 对图像尺寸、归一化、插值和掩膜缩放所做的任何修改都应在实验中明确报告；
- 若重新划分数据，应将结果标记为自定义划分，避免与官方划分结果直接比较；
- 对数据进行增强、裁剪或重新标注时，应保留修改记录并注明与原始版本的差异。

## 局限性

- 数据集聚焦 PCB 工业视觉场景，不能代表所有工业产品的异常分布；
- 9 种缺陷的样本数量和尺寸分布并不完全均衡；
- 小面积缺陷占比较高，对模型的局部特征提取能力要求较高；
- 像素级标注主要描述二维可见异常，无法反映深度、内部结构或电气性能；
- 基准结果依赖具体的零样本训练协议，采用不同辅助训练数据可能产生不同结果；
- 使用时应以 GitHub 仓库中的正式数据发布说明、`meta.json` 和最新版本记录为准。

## 引用

如果 PCB4ZSAD 对您的研究有所帮助，请引用或注明本 GitHub 数据集仓库：

https://github.com/1retiree/PCB-dataset/tree/PCB4ZSAD

```bibtex
@misc{pcb4zsad,
  author       = {1retiree},
  title        = {PCB4ZSAD: A PCB Dataset for Zero-Shot Anomaly Detection},
  howpublished = {GitHub repository},
  url          = {https://github.com/1retiree/PCB-dataset/tree/PCB4ZSAD}
}
```

## 联系方式

如对数据集、标注或评估协议有疑问，请通过 GitHub Issues 反馈：

https://github.com/1retiree/PCB-dataset/issues
